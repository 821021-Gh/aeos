---
doc-id: AEOS-ADR-003
doc-name: Agent Control Plane and Runtime Separation Decision
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-26
updated: 2026-08-27
related:
  - EWO-AEOS-0044
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-012
  - AEOS-ARCH-013
---

# AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision

## 1. 背景

AEOS 已正式定義 Platform、Layer、Capability、Repository、Dependency、Workspace 與 Architecture Principles，但尚未有一項明確決策界定 Enterprise AI Agent 的治理與執行責任。

若 Agent 編排、政策、核准、工具存取、模型選擇、記憶體存取與執行執行環境被實作在同一產品邊界內，Enterprise Architecture 將容易產生下列風險：

- Runtime、Agent Harness 或框架被誤認為 Enterprise Architecture 本身。
- 更換或重新組合 Agent Harness / 執行環境時必須同步改變治理語意與權限模型。
- Runtime 或 Harness 可在缺乏上位授權時自行擴張工具、模型、資料或憑證權限。
- 特定產品成為不可替換的架構前提。
- Audit、核准與執行證據無法跨執行環境 / Harness chain 維持一致。

EWO-AEOS-0044 因此需要一項正式 Architecture Decision，建立 Control Plane、Agent Harness / Orchestration 與 Runtime 之間的穩定責任邊界。

## 2. 決定

### D-01 — 邏輯分離

AEOS SHALL 將 **Agent Control Plane** 與 **Agent Execution Plane** 定義為邏輯上分離的責任平面。Agent Execution Plane MAY 由一個或多個 Agent Harness、編排實作、執行環境或供應商轉接器組成。

此分離是 Enterprise Architecture 邊界，不要求各責任必須部署於不同流程、host、cluster 或產品；實作可共置或組合，但其權限、契約與責任 MUST 可被清楚區分。

### D-02 — Control Plane 權威

Agent Control Plane SHALL 擁有或協調下列 Enterprise-level responsibilities：

- Agent 身分與生命週期政策。
- Task / 意圖准入與編排政策。
- Policy evaluation 與政策上下文 delivery。
- Approval 狀態與 human / system 授權證據。
- 工具權限、模型權限、記憶體/資料存取範圍。
- Execution 契約建立與版本治理。
- Harness / 執行環境選擇、路由與組成政策。
- Audit correlation、可觀測性要求與執行證據要求。
- Cancellation、撤銷、quarantine 與失敗 containment 政策。

Control Plane MAY 委派局部執行控制與協調責任，但 SHALL NOT 將最終 Enterprise 治理權限隱性轉移給 Agent Harness、Runtime 或 Provider。

### D-03 — Agent Harness / 協調職責

Agent Harness / Orchestration 實作 MAY 在有效 Execution Contract 與授權範圍內承擔：

- 任務分解；
- 子代理分配；
- 工作流程/執行協調；
- 規劃/推理循環協調；
- 模型、工具、記憶體能力路由；
- retry、scheduling、本機 fallback 與上下文 coordination；
- 對下游 Harness / 執行環境的已委派執行 control。

Agent Harness / 編排實作 MUST NOT：

- 因承擔編排或監督器角色而取得 Enterprise 治理權限；
- 自行建立、提升或擴張核准 / 授權；
- 將已委派執行 control 再解讀為 unrestricted 權限；
- 覆寫 Control Plane 政策或 mandatory Execution Contract 語意；
- 以產品角色、技術能力或拓撲位置推定其具有上位治理權限。

**委派規則**：執行控制權 MAY 委派；企業治理權限 SHALL NOT 被隱含委派。

### D-04 — 執行環境責任

Agent Runtime SHALL 僅在有效 Execution Contract 與授權範圍內執行 Agent workload。

執行環境 MUST NOT：

- 自行建立或提升 Enterprise 核准。
- 覆寫 Control Plane 政策。
- 擴張未授權的工具、模型、記憶體、資料或憑證範圍。
- 將 runtime-local 設定提升為 Enterprise Architecture 權限。
- 以技術可行性推定治理允許性。

### D-05 — 執行環境與Harness中立

Enterprise AI Agent Architecture SHALL 保持 **Runtime Neutral** 與 **Harness Neutral**。

任何 Agent Harness、Agent 執行環境、框架、模型供應商、工具供應商、記憶體供應商或編排實作均 SHALL 被視為可替換或可組合實作，除非另有經核准的 Architecture / ADR 明確賦予更高階身分。

AEOS Architecture MUST NOT 以特定產品名稱作為 mandatory component、mandatory 相依性或唯一合法實作，也 MUST NOT 預先將任一具名產品固定為監督器、執行環境、模型供應商或其他唯一角色。

### D-06 — 可組合執行環境／Harness鏈

一個受治理的 Agent 執行 MAY 由多個 Harness / Runtime 實作串接或分層組成，例如：

`Control Plane → Harness A → Harness / Runtime B → Provider`

但整條執行 chain MUST：

- 由 Control Plane 政策 / Execution Contract 明確允許；
- 維持一致的執行、任務與稽核 correlation 身分；
- 維持核准、授權、工具/模型/記憶體/資料/憑證範圍不被放寬；
- 讓每一層已委派權限可被追溯；
- 支援 cancellation / 撤銷向下游傳遞；
- 不得因新增中介 Harness / Runtime 而形成治理 bypass。

若中介實作無法保留 mandatory 治理語意，該組合 SHALL 被視為不相容，而不是降低 Enterprise 治理 requirement。

### D-07 — 穩定 Execution Contract

Control Plane 與 Agent Execution Plane 之間 SHALL 透過受版本治理的 Execution Contract 互動。契約至少包含：

- 執行/任務識別；
- 代理身份/角色參考；
- 請求的意圖/任務；
- 政策背景參考；
- 核准狀態/授權證據參考；
- 允許的工具範圍；
- 允許的型號範圍；
- 允許的記憶體/資料範圍；
- 已委派執行權限 / 下游 constraints（如適用）；
- 執行環境限制（預算、逾時、取消、沙箱/隔離要求）；
- 審查關聯身分；
- 執行結果/狀態/證據要求。

Runtime / Harness-specific parameters MAY 存在於轉接器擴充，但 MUST NOT 改變 Enterprise 契約的治理語意。

## 3. 後果

### 積極

- Harness / Runtime 可替換、並存或組合，而不重寫 Enterprise 治理語意。
- 可支援單一執行環境、監督器 + 執行環境、多 Harness chain 等不同實作 topology。
- Agent 政策、核准與稽核可跨實作保持一致。
- 降低供應商或框架 lock-in。
- 下位 Implementation 無法正當化對上位 Architecture 的反向控制。
- 可建立一致的 security、可觀測性與執行證據邊界。

### 權衡

- 需要明確轉接器 / interface design。
- Control Plane 與 Agent Execution Plane 契約需要版本治理。
- Composite Harness / Runtime chain 需要 propagation、撤銷與證據 correlation 規則。
- 可能增加實作整合 complexity。
- 某些單體 Agent 框架必須透過 logical 分離才能符合 Enterprise Architecture。

## 4. 考慮的替代方案

### A. 以產品為中心的代理平台

將特定 Agent 框架直接定義為企業 Agent Architecture 核心。

**Rejected**：會將實作選擇提升為架構事實，違反 Capability Driven、Layer 分離與 Runtime / Harness Neutral 方向。

### B. 執行環境擁有的治理

由各 Runtime / Harness 自行處理政策、核准、工具 permission 與稽核。

**Rejected**：導致治理語意分裂、執行環境 / Harness replacement 成本高，且形成下位層反向控制上位責任的風險。

### C. 固定主管/執行環境產品角色

預先指定某一具名產品永遠擔任監督器，而另一具名產品永遠擔任執行環境。

**Rejected**：產品能力與最佳組合會演進；固定角色會把目前實作選擇誤升格為 Enterprise Architecture constraint。

### D. 未正式分離

僅以部署架構描述 Agent components，不建立 Control Plane / Agent Execution 權限邊界。

**Rejected**：無法形成可驗證的 Enterprise 責任模型。

## 5. 架構對齊

- AEOS-ARCH-004：維持 Platform Oriented、Capability Driven、Dependency Explicit。
- AEOS-ARCH-005：不將產品、Harness 或單一執行環境自動等同 Platform。
- AEOS-ARCH-006：Control Plane 治理語意不得被 L6 Implementation 反向控制。
- AEOS-ARCH-007：Agent capabilities 以企業能力描述，不以 Harness / 執行環境實作替代。
- AEOS-ARCH-009：Harness、Runtime、工具、模型、記憶體供應商依賴須為顯式、受治理依賴。
- AEOS-ARCH-012：後續 Agent 實作 MUST 遵循既有 Architecture Principles。

## 6. 狀態與核准

本 ADR 目前為 **Approved 1.0.0**。

EWO-AEOS-0044 Review Package 已完成合併，本文作為 Agent Control Plane / Runtime 分離、Harness / Runtime Neutral 與 composable 執行 chain 的 Approved Architecture Decision。

核准後：

- 可作為 AEOS Agent Architecture 後續設計與實作治理的決策權威。
- AEOS-ARCH-013 依本文建立 Enterprise AI Agent Architecture 的 Approved Architecture 定義。
- AEOS-ARCH-001 Approved Architecture Register 應登錄 AEOS-ARCH-013。

## 7. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0044 Post-Merge Closure Verification：將 Agent Control Plane / Runtime Separation Decision 升級為 Approved Architecture Decision，支援 AEOS-ARCH-013 基準登錄 | Codex |
| 0.2.0 | 2026-08-26 | 補入 Agent Harness / Orchestration Boundary、Delegated Execution Control、Composable Harness / Runtime Chain 與 Harness Neutral；避免將任何具名產品預先固定為監督器/執行環境角色 | ChatGPT |
| 0.1.0 | 2026-08-26 | 建立 Agent Control Plane / Runtime logical 分離、Runtime Neutral 與 Execution Contract 候選決策 | ChatGPT |
