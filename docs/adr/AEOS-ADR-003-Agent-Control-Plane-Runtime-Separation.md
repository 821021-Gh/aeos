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

## 1. Context

AEOS 已正式定義 Platform、Layer、Capability、Repository、Dependency、Workspace 與 Architecture Principles，但尚未有一項明確決策界定 Enterprise AI Agent 的治理與執行責任。

若 Agent orchestration、policy、approval、tool access、model selection、memory access 與 execution runtime 被實作在同一產品邊界內，Enterprise Architecture 將容易產生下列風險：

- Runtime、Agent Harness 或 framework 被誤認為 Enterprise Architecture 本身。
- 更換或重新組合 Agent Harness / runtime 時必須同步改變治理語意與權限模型。
- Runtime 或 Harness 可在缺乏上位授權時自行擴張工具、模型、資料或憑證權限。
- 特定產品成為不可替換的架構前提。
- Audit、approval 與 execution evidence 無法跨 runtime / harness chain 維持一致。

EWO-AEOS-0044 因此需要一項正式 Architecture Decision，建立 Control Plane、Agent Harness / Orchestration 與 Runtime 之間的穩定責任邊界。

## 2. Decision

### D-01 — Logical Separation

AEOS SHALL 將 **Agent Control Plane** 與 **Agent Execution Plane** 定義為邏輯上分離的責任平面。Agent Execution Plane MAY 由一個或多個 Agent Harness、orchestration implementation、runtime 或 provider adapter 組成。

此分離是 Enterprise Architecture boundary，不要求各責任必須部署於不同 process、host、cluster 或產品；實作可共置或組合，但其 authority、contract 與責任 MUST 可被清楚區分。

### D-02 — Control Plane Authority

Agent Control Plane SHALL 擁有或協調下列 Enterprise-level responsibilities：

- Agent identity 與 lifecycle policy。
- Task / intent admission 與 orchestration policy。
- Policy evaluation 與 policy context delivery。
- Approval state 與 human / system authorization evidence。
- Tool permission、model permission、memory/data access scope。
- Execution contract 建立與版本治理。
- Harness / runtime selection、routing 與 composition policy。
- Audit correlation、observability requirements 與 execution evidence requirements。
- Cancellation、revocation、quarantine 與 failure containment policy。

Control Plane MAY 委派局部執行控制與協調責任，但 SHALL NOT 將最終 Enterprise governance authority 隱性轉移給 Agent Harness、Runtime 或 Provider。

### D-03 — Agent Harness / Orchestration Responsibility

Agent Harness / Orchestration implementation MAY 在有效 Execution Contract 與授權範圍內承擔：

- task decomposition；
- sub-agent assignment；
- workflow / execution coordination；
- planning / reasoning loop coordination；
- model、tool、memory capability routing；
- retry、scheduling、local fallback 與 context coordination；
- 對下游 harness / runtime 的 delegated execution control。

Agent Harness / Orchestration implementation MUST NOT：

- 因承擔 orchestration 或 supervisor 角色而取得 Enterprise governance authority；
- 自行建立、提升或擴張 approval / authorization；
- 將 delegated execution control 再解讀為 unrestricted authority；
- 覆寫 Control Plane policy 或 mandatory Execution Contract semantics；
- 以產品角色、技術能力或拓撲位置推定其具有上位治理權限。

**Delegation Rule**：Execution control MAY be delegated；Enterprise governance authority SHALL NOT be implicitly delegated。

### D-04 — Runtime Responsibility

Agent Runtime SHALL 僅在有效 Execution Contract 與授權範圍內執行 Agent workload。

Runtime MUST NOT：

- 自行建立或提升 Enterprise approval。
- 覆寫 Control Plane policy。
- 擴張未授權的 tool、model、memory、data 或 credential scope。
- 將 runtime-local configuration 提升為 Enterprise Architecture authority。
- 以技術可行性推定治理允許性。

### D-05 — Runtime and Harness Neutrality

Enterprise AI Agent Architecture SHALL 保持 **Runtime Neutral** 與 **Harness Neutral**。

任何 Agent Harness、Agent runtime、framework、model provider、tool provider、memory provider 或 orchestration implementation 均 SHALL 被視為可替換或可組合實作，除非另有經核准的 Architecture / ADR 明確賦予更高階身分。

AEOS Architecture MUST NOT 以特定產品名稱作為 mandatory component、mandatory dependency 或唯一合法實作，也 MUST NOT 預先將任一具名產品固定為 supervisor、runtime、model provider 或其他唯一角色。

### D-06 — Composable Runtime / Harness Chain

一個受治理的 Agent execution MAY 由多個 Harness / Runtime implementation 串接或分層組成，例如：

`Control Plane → Harness A → Harness / Runtime B → Provider`

但整條 execution chain MUST：

- 由 Control Plane policy / Execution Contract 明確允許；
- 維持一致的 execution、task 與 audit correlation identity；
- 維持 approval、authorization、tool/model/memory/data/credential scope 不被放寬；
- 讓每一層 delegated authority 可被追溯；
- 支援 cancellation / revocation 向下游傳遞；
- 不得因新增中介 Harness / Runtime 而形成 governance bypass。

若中介實作無法保留 mandatory governance semantics，該組合 SHALL 被視為不相容，而不是降低 Enterprise governance requirement。

### D-07 — Stable Execution Contract

Control Plane 與 Agent Execution Plane 之間 SHALL 透過受版本治理的 Execution Contract 互動。契約至少包含：

- execution / task identity；
- agent identity / role reference；
- requested intent / task；
- policy context reference；
- approval state / authorization evidence reference；
- permitted tool scopes；
- permitted model scopes；
- permitted memory / data scopes；
- delegated execution authority / downstream constraints（如適用）；
- runtime constraints（budget、timeout、cancellation、sandbox / isolation requirements）；
- audit correlation identity；
- execution result / status / evidence requirements。

Runtime / Harness-specific parameters MAY 存在於 adapter extension，但 MUST NOT 改變 Enterprise contract 的治理語意。

## 3. Consequences

### Positive

- Harness / Runtime 可替換、並存或組合，而不重寫 Enterprise governance semantics。
- 可支援單一 runtime、supervisor + runtime、多 harness chain 等不同 implementation topology。
- Agent policy、approval 與 audit 可跨實作保持一致。
- 降低供應商或 framework lock-in。
- 下位 Implementation 無法正當化對上位 Architecture 的反向控制。
- 可建立一致的 security、observability 與 execution evidence boundary。

### Trade-offs

- 需要明確 adapter / interface design。
- Control Plane 與 Agent Execution Plane contract 需要版本治理。
- Composite Harness / Runtime chain 需要 propagation、revocation 與 evidence correlation 規則。
- 可能增加 implementation integration complexity。
- 某些單體 Agent framework 必須透過 logical separation 才能符合 Enterprise Architecture。

## 4. Alternatives Considered

### A. Product-Centric Agent Platform

將特定 Agent framework 直接定義為企業 Agent Architecture 核心。

**Rejected**：會將實作選擇提升為架構事實，違反 Capability Driven、Layer separation 與 Runtime / Harness Neutral 方向。

### B. Runtime-Owned Governance

由各 Runtime / Harness 自行處理 policy、approval、tool permission 與 audit。

**Rejected**：導致治理語意分裂、runtime / harness replacement 成本高，且形成下位層反向控制上位責任的風險。

### C. Fixed Supervisor / Runtime Product Roles

預先指定某一具名產品永遠擔任 supervisor，而另一具名產品永遠擔任 runtime。

**Rejected**：產品能力與最佳組合會演進；固定角色會把目前 implementation selection 誤升格為 Enterprise Architecture constraint。

### D. No Formal Separation

僅以 deployment architecture 描述 Agent components，不建立 Control Plane / Agent Execution authority boundary。

**Rejected**：無法形成可驗證的 Enterprise responsibility model。

## 5. Architecture Alignment

- AEOS-ARCH-004：維持 Platform Oriented、Capability Driven、Dependency Explicit。
- AEOS-ARCH-005：不將產品、Harness 或單一 runtime 自動等同 Platform。
- AEOS-ARCH-006：Control Plane governance semantics 不得被 L6 Implementation 反向控制。
- AEOS-ARCH-007：Agent capabilities 以企業能力描述，不以 Harness / runtime 實作替代。
- AEOS-ARCH-009：Harness、Runtime、tool、model、memory provider 依賴須為顯式、受治理依賴。
- AEOS-ARCH-012：後續 Agent implementation MUST 遵循既有 Architecture Principles。

## 6. Status and Approval

本 ADR 目前為 **Approved 1.0.0**。

EWO-AEOS-0044 Review Package 已完成合併，本文作為 Agent Control Plane / Runtime separation、Harness / Runtime Neutral 與 composable execution chain 的 Approved Architecture Decision。

核准後：

- 可作為 AEOS Agent Architecture 後續設計與 implementation governance 的決策權威。
- AEOS-ARCH-013 依本文建立 Enterprise AI Agent Architecture 的 Approved Architecture 定義。
- AEOS-ARCH-001 Approved Architecture Register 應登錄 AEOS-ARCH-013。

## 7. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0044 Post-Merge Closure Verification：將 Agent Control Plane / Runtime Separation Decision 升級為 Approved Architecture Decision，支援 AEOS-ARCH-013 baseline 登錄 | Codex |
| 0.2.0 | 2026-08-26 | 補入 Agent Harness / Orchestration Boundary、Delegated Execution Control、Composable Harness / Runtime Chain 與 Harness Neutral；避免將任何具名產品預先固定為 supervisor/runtime 角色 | ChatGPT |
| 0.1.0 | 2026-08-26 | 建立 Agent Control Plane / Runtime logical separation、Runtime Neutral 與 Execution Contract 候選決策 | ChatGPT |
