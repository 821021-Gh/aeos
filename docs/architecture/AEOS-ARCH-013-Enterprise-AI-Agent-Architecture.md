---
doc-id: AEOS-ARCH-013
doc-name: Enterprise AI Agent Architecture
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-26
updated: 2026-08-27
related:
  - EWO-AEOS-0044
  - AEOS-ADR-003
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-010
  - AEOS-ARCH-012
---

# AEOS-ARCH-013 — Enterprise AI Agent Architecture

## 執行摘要

本文件建立 AEOS 的 Enterprise AI Agent Architecture 候選基線，定義 Agent Control Plane、Agent Harness / Orchestration Boundary、Agent Runtime、Execution Contract 與 Provider Boundary 的責任、Authority、Interface、Dependency 與生命週期邊界，並確立 Runtime Neutral 與 Harness Neutral 原則。

本架構不指定任何特定 Agent 框架、Harness、執行環境、模型供應商、工具平台、記憶體產品或部署技術。具體實作可單獨使用、彼此替換或組合成受治理的執行 chain，但不得因位於監督器、編排或執行環境位置而取得 Enterprise 治理權限。

Control Plane 保有企業層級的政策、核准、授權、准入、組成政策、路由、稽核與撤銷權限；Harness / Runtime 僅能在有效 Execution Contract、已委派執行權限與授權範圍內執行、協調或轉派工作。

本文件目前為 Approved 1.0.0，已納入 AEOS-ARCH-001 Approved Architecture Register，作為 Enterprise AI Agent Architecture 的正式定義載體。

## 1. 目的

本文件之目的為：

- 定義 Enterprise AI Agent 的共同架構語言與責任模型。
- 建立 Agent Control Plane 與 Agent Execution Plane 的正式邏輯邊界。
- 定義 Agent Harness / Orchestration Boundary，避免把任何單一產品角色固定為 Enterprise Architecture。
- 將 Runtime Neutral、Harness Neutral、Provider Neutral 與 Adapter Boundary 轉化為可治理規則。
- 定義跨 Harness / Runtime 穩定的 Agent Execution Contract。
- 支援單一 Runtime、Supervisor + Runtime、多 Harness / Runtime Chain 等不同實作 topology。
- 定義 Agent 身分、政策、核准、工具、模型、記憶體/資料、憑證、可觀測性與失敗 containment 的責任歸屬。
- 使 Agent 實作可演進、替換、並存或組合，而不改變 Enterprise 治理語意。

## 2.範圍

### 2.1 在範圍內

- 企業 Agent 邏輯架構。
- Control Plane / Agent Execution Plane 邊界。
- Agent Harness / 編排邊界。
- Execution Contract 與 Provider Adapter Boundary。
- Delegated 執行 control 與權限 propagation。
- 可組合的Harness／執行環境鏈。
- Agent 身分與生命週期 control。
- Task 准入、編排政策與執行環境 / Harness 路由。
- Policy evaluation、核准狀態與授權證據。
- Tool、模型、記憶體/資料與憑證存取邊界。
- Audit、遙測資料、可觀測性與執行證據。
- Cancellation、撤銷、quarantine、失敗隔離與 recovery 邊界。
- Runtime / Harness portability、供應商 substitution 與 compatibility 要求。
- 與既有 Platform、Layer、Capability、Repository、Dependency、Workspace Architecture 的對應。

### 2.2 超出範圍

- 任何具名 Agent 框架、Harness、執行環境、編排產品或供應商選擇。
- 預先指定某一具名產品必須擔任監督器、執行環境、模型 router 或其他唯一角色。
- 任何具名 LLM / 模型供應商選擇。
- 任何具名向量資料庫、記憶體產品、工具平台或 secret manager 選擇。
- Production 部署拓撲、host sizing、network layout 或基礎設施設計。
- 原始碼、SDK、API 實作細節。
- 個別 Product Agent 的提示詞、角色設定、business 工作流程或領域邏輯。
- 未經核准的具名 Platform 或 Capability Catalog 條目。

## 3. 架構權威

本文件受下列既有 AEOS 架構約束：

|權威|角色 |
|---|---|
| AEOS-ARCH-001 | Architecture Baseline 與最高正式架構入口 |
| AEOS-ARCH-004 | Enterprise Architecture 域關係 |
| AEOS-ARCH-005 | Platform 邊界、身分與 product-neutral 平台規則 |
| AEOS-ARCH-006 | Layer 責任與 anti-bypass / anti-reverse-control 規則 |
| AEOS-ARCH-007 | Capability-first definition；不得以實作取代能力 |
| AEOS-ARCH-009 | Dependency 方向、類型與治理 |
| AEOS-ARCH-010 | Workspace 組成與 cross-repository 整合 |
| AEOS-ARCH-012 | Architecture Principles |
| AEOS-ADR-003 | Control Plane / Agent Execution 分離、Harness / Runtime Neutral 與 composable 執行候選決策 |

AEOS-ADR-003 與本文件已於 EWO-AEOS-0044 closure 中升級為 Approved 1.0.0；本文件為 Enterprise AI Agent Architecture 的正式架構權威。

## 4. 核心概念

### 4.1 代理

Agent 是在受治理的身分、政策、permission 與執行上下文下，接收意圖 / 任務、進行推理 / planning、使用允許之模型、記憶體、資料與工具，並產生可驗證執行結果的邏輯執行主體。

Agent 不等同於單一模型、提示詞、執行環境流程、Harness、框架、API client、automation 工作流程或 Platform。

### 4.2 Agent Control Plane

Agent Control Plane 是負責 Enterprise-level 治理、coordination 政策、准入、授權、路由、組成政策、核准與證據要求的邏輯控制平面。

Control Plane 是架構責任邊界，不代表特定產品、服務或部署 unit。

### 4.3 Agent Execution Plane

Agent Execution Plane 是承載受控 Agent workload 的邏輯執行平面，可由一個或多個 Agent Harness、編排實作、執行環境、轉接器與供應商組成。

Execution Plane 具有執行權限，不具有 Enterprise 治理權限。

### 4.4 Agent Harness / 編排邊界

Agent Harness / Orchestration Boundary 是負責執行 coordination 的可替換實作邊界。它 MAY 提供：

- 任務分解；
- 子代理分配；
- 工作流程協調；
- 規劃/推理循環協調；
- 模型、工具、記憶體路由；
- 重試、調度、本機回退；
- 上下文協調；
- 對下游 Harness / Runtime 的已委派執行 control。

Harness 可以同時包含執行環境能力，也可以只負責編排；AEOS 不預先固定產品角色。

### 4.5 Agent Runtime

Agent Runtime 是實際承載 Agent 執行 loop 或 workload 執行的受控實作。Runtime 可包含推理、planning、模型 invocation、工具 invocation、狀態 handling 與結果 assembly，但必須受 Execution Contract 與授權範圍限制。

### 4.6 供應商

Provider 是提供執行環境、模型、工具、記憶體/資料、憑證或其他執行能力的實作來源。Provider 本身不因技術重要性取得 Enterprise 治理權限。

### 4.7 轉接器邊界

Adapter Boundary 是 Enterprise 契約與供應商特定實作之間的轉譯邊界。Adapter 可處理協定、結構描述、能力對應與供應商擴充，但不得改變 Enterprise 政策語意或提升權限。

## 5. 邏輯架構

Enterprise AI Agent Architecture 定義下列邏輯責任平面：

|平面/邊界|核心責任|權限等級|
|---|---|---|
| Governance Authority | Enterprise 政策、架構、risk/核准規則的來源 | 上位治理權威 |
| Agent Control Plane |准入、身分、政策背景、核准、授權、組合、路由、撤銷、證據要求 |企業控制權|
| Execution Contract Boundary | 將 control 意圖與執行實作解耦的版本化契約 | Contract 權限 |
| Agent Harness / 編排邊界 |任務分解、協調、分代理/下游執行委託 |僅限委託執行權 |
| Agent Runtime | 在授權範圍內執行任務 / 推理 / tool-model-memory calls | Execution 權限 only |
| Provider Adapter Boundary | 連接執行環境/模型/工具/記憶體 providers，隔離供應商特定語意 | Translation 權限 only |
| External Providers / Systems | 提供實際模型、工具、資料、記憶或運算能力 | No Enterprise 架構權限 by 預設 |
| Audit / Observability Boundary | 接收並關聯執行證據、遙測資料、狀態 | Evidence plane |

最簡單 topology：

`Governance → Control Plane → Execution Contract → Runtime → Adapter → Provider`

允許的 composite topology：

`Governance → Control Plane → Execution Contract → Harness A → Harness / Runtime B → Adapter → Provider`

或：

`Governance → Control Plane → Execution Contract → Harness A → Runtime B / Runtime C → Providers`

這些 topology 是實作 options，不是具名產品角色定義。

Audit / Observability 橫跨 Control Plane、Harness、Runtime 與 Provider Adapter，但不取得執行或治理決策權。

## 6. Control Plane 職責

Agent Control Plane MUST：

1. **Agent Identity**：確認 Agent 身分、角色、擁有者參考、生命週期狀態與允許執行範圍。
2. **Task Admission**：決定任務是否可進入執行生命週期。
3. **Policy Context**：取得並固定本次執行適用的政策 / 治理上下文。
4. **Approval State**：取得或驗證必要的 human/system 核准證據。
5. **Authorization Scope**：建立工具、模型、記憶體/資料、憑證與 environment 範圍。
6. **Composition Policy**：決定允許使用單一 Runtime、Harness + Runtime 或多層 Harness / Runtime Chain 的條件。
7. **Runtime / Harness Routing**：依能力、risk、cost、availability、資料邊界等政策選擇合規實作。
8. **Execution Contract**：建立版本化且可驗證的執行要求。
9. **Budget and Limits**：設定時間、token/cost、工具 invocation、retry、concurrency 或其他受治理限制。
10. **Revocation / Cancellation**：能撤銷尚未完成之執行權限，且要求向下游 propagation。
11. **Evidence Requirements**：定義稽核、遙測資料、結果來源與 completion 證據最低要求。
12. **Failure Policy**：定義 retry、fallback、quarantine、升級處理、human 交接或 fail-closed / fail-open 條件。

Control Plane MUST NOT：

- 假設任一特定供應商、Harness 或執行環境永久存在。
- 將供應商特定 / harness-specific config 當作 Enterprise 政策來源。
- 以實作技術能力取代核准 / 授權決策。
- 因某實作被稱為監督器 / orchestrator 而自動賦予 Enterprise 治理權限。

## 7. 委託執行控制

### 7.1 委託原則

**執行控制權 MAY 被委託；企業治理權限 MUST NOT 被隱式委託。 **

Control Plane MAY 將局部執行協調責任委派給 Harness / Runtime，包括任務分解、sub-agent assignment、工作流程 coordination、本機模型/工具路由、retry 與 scheduling。

### 7.2 委託要求

每一次已委派執行權限 MUST：

- 可追溯至上游 Execution Contract 或其受治理子契約；
- 不得超過上游核准 / 授權範圍；
- 明確保留預算、timeout、工具/模型/記憶體/資料/憑證 constraints；
- 保留執行、任務與稽核 correlation 身分；
- 支援 cancellation / 撤銷向下游傳遞；
- 不得被中介 Harness / Runtime 擴權。

### 7.3 Harness / Orchestration MUST NOT

- 自行將 Proposed / Pending 核准提升為 Approved。
- 自行擴張 permissions 或憑證範圍。
- 將已委派執行 control 重新解釋為 unrestricted 權限。
- 以本機預設覆寫 Enterprise 政策。
- 將自身監督器 / 編排角色升格為 Enterprise 治理權限。
- 將下游技術可行性推定為治理允許性。

## 8. 執行環境職責

Agent Runtime MUST：

- 驗證 Execution Contract / 已委派契約版本與完整性。
- 僅執行被授權的 Agent / 任務。
- 僅使用被允許的模型、工具、記憶體/資料與憑證範圍。
- 遵守預算、timeout、cancellation、sandbox / 隔離與證據要求。
- 將供應商特定行為限制在 Runtime / Adapter 實作邊界。
- 回報執行狀態、結果、error、工具/模型 calls 與必要稽核證據。
- 接受 Control Plane 或經授權上游 Harness 的 cancellation、撤銷或 quarantine 指令。

Agent Runtime MUST NOT：

- 自行將 Proposed / Pending 核准提升為 Approved。
- 自行擴張 permissions 或憑證範圍。
- 在未授權時切換至更高權限供應商或工具。
- 以本機預設覆寫 Enterprise 政策。
- 將 runtime-local 記憶體或狀態自動升格為 Enterprise 來源 of truth。
- 因供應商失敗而繞過 control 政策。

## 9. Execution Contract

### 9.1 最低合約

每次可治理執行 MUST 具有可追溯的 Execution Contract，至少包含：

|現場組|最低限度的意義|
|---|---|
|合約|合約版本、創建時間、發行方/控制面參考 |
|執行身分 |執行 ID、任務 ID、關聯 ID |
|代理身分|代理 ID / 角色參考、所有者 / 負責人參考 |
|意向 |任務/目標/要求的結果|
| Policy | 政策上下文參考、risk class / control class（如適用） |
|核准|核准要求、核准狀態、核准證據參考 |
|工具範圍|允許/拒絕的工具能力、操作範圍 |
|型號範圍|允許的模型能力/類別、資料處理限制 |
|記憶體/資料範圍|可讀/可寫入來源、保留/敏感度約束 |
| Credential Scope | 憑證參考 / 能力範圍；不得傳遞不必要 secret material |
|代表團|允許下游委託、最大範圍、鏈/父身分 |
|執行環境約束|逾時、預算、重試、並發、隔離、網路/環境限制 |
|證據|所需日誌、事件、結果來源、稽核關聯 |
|完成 |成功/失敗語意、結果模式、切換/升級需求 |

### 9.2 合約規則

- Contract MUST 具版本識別。
- Harness / Runtime MUST 能拒絕不支援或不完整的契約。
- Contract 結構描述 evolution MUST 保持 backward/forward compatibility 政策或明確 migration rule。
- Provider / Harness-specific 擴充 MAY 存在，但 MUST 位於 namespace / 擴充邊界，且不得改寫 mandatory 治理 fields。
- Contract MUST 支援 idempotency / duplicate detection 所需身分，若執行類型需要。
- Contract MUST 支援 cancellation / 撤銷 correlation。
- 下游已委派契約 MUST NOT 放寬上游 mandatory 治理 constraints。

## 10. 執行環境與Harness中立性

### 10.1 原則

Enterprise Architecture 定義 **what must be governed and guaranteed**，而不是指定 **which 產品 must execute or orchestrate it**。

因此：

- Harness／執行環境實作 MUST 可替換。
- 在政策允許的情況下，Harness／執行環境 MAY 共存或組合。
- 遷移/重組 MUST NOT 需要重新定義企業策略語意。
- Provider / Harness 選擇 MUST be 政策 / 能力 driven，而非架構 hard-code。
- 同一 Control Plane MAY 支援多個 Harness / Runtime 實作。
- AEOS MUST NOT 預先指定具名產品必須扮演監督器、執行環境、模型 router 或其他唯一角色。

### 10.2 可移植性要求

Harness / Runtime 替換或重新組合時至少下列語意 SHOULD 可維持：

- 代理身份對應。
- 任務/執行/父子身分。
- 核准語意。
- 授權/委託權限語意學。
- 工具權限語意。
- 模型/資料處理約束。
- 記憶體/資料範圍。
- 取消/撤銷傳播。
- 審查相關性。
- 完成/錯誤語意。

若實作無法表達必要語意，該實作 MUST 被視為能力 mismatch，而不是降低 Enterprise 治理 requirement。

## 11. 可組合Harness／執行環境鏈

一個執行 MAY 經過多個 Harness / Runtime 層級，但整條 chain MUST 維持治理不變式。

### 11.1 強制不變量

- **No Authority Expansion**：下游權限不得大於上游授權。
- **No Approval Inflation**：下游不得自行建立更高核准狀態。
- **Scope Monotonicity**：工具/模型/記憶體/資料/憑證範圍只能相同或收斂，不得自行放寬。
- **Identity Continuity**：執行 / 任務 / correlation / parent-child 身分必須可追溯。
- **Revocation Propagation**：上游 cancellation / 撤銷必須可向下游傳遞。
- **Evidence Continuity**：主要決策 / 執行證據必須可跨 chain 關聯。
- **Failure Containment**：任一中介失敗不得造成治理 bypass。

### 11.2 鏈範例

以下皆為合法的抽象形態，前提是符合本文件規則：

- `Control Plane → Runtime → Provider`
- `Control Plane → Harness → Runtime → Provider`
- `Control Plane → Harness A → Harness B → Runtime → Provider`
- `Control Plane → Harness → Runtime A / Runtime B → Providers`

上述名稱均為 logical roles，不代表任何具名產品。

## 12. 供應商邊界

### 12.1 Harness／執行環境提供程序

提供編排、執行 loop、代理工作流程或監督器實作。不得擁有 Enterprise 政策權限。

### 12.2 模型供應商

提供 inference / 推理 / embedding / multimodal 等模型能力。Model 身分與供應商身分 MUST 與 Agent 身分分離。

### 12.3 工具供應商

提供外部 action / API / system 能力。Tool invocation MUST 受到 operation-level 範圍限制；「可連線」不等於「可執行所有操作」。

### 12.4 記憶體/資料供應商

提供 transient 狀態、working 記憶體、long-term 記憶體、retrieval、knowledge 或 business 資料。Data 權限、保留與 write permission MUST 明確區分。

### 12.5 憑證供應商

提供憑證參考、委派或 temporary 存取能力。Execution 實作 SHOULD 取得最小必要能力，而非直接持有長期高權限 secret。

## 13. 政策與核准邊界

### 13.1 政策

- Enterprise 政策來源 MUST 位於 Agent Execution Plane 之外的上位治理來源。
- Harness / Runtime MAY 執行本機 enforcement，但 MUST 以上位政策上下文 / 政策參考為依據。
- Local safety guard MAY 加嚴限制，但 MUST NOT 放寬 Enterprise 政策。

### 13.2 核准

- Approval 是治理證據，不是 UI click 或執行環境 flag 本身。
- Approval 證據 MUST 可追溯至 approver / 權限、範圍、time、対象執行或操作 class。
- Harness / Runtime MUST NOT reuse 已過期、範圍不符或已 revoked 的核准。
- 對需要每次核准的操作，不得以先前任務核准推定永久授權。

## 14. 工具存取邊界

Tool 存取 MUST 以能力 / 操作範圍控制，至少區分：Read / observe、Create / write、Update / mutate、Delete / destructive action、Execute / 命令、Deploy / activate、Credential / permission administration。

高風險操作 MAY 要求 per-operation 核准、stronger 隔離或不同執行 class。

任何 Harness / Runtime MUST NOT 將 broad 供應商 token 自動等同所有工具 operations 均獲授權。

## 15. 模型邊界

Model 選擇 MUST 與 Agent 身分、政策與 Harness / Runtime 實作分離。

Control Plane MAY 依任務能力、資料 sensitivity、jurisdiction / residency、延遲、cost / token 預算、上下文 size、modality、safety / compliance class、availability / fallback 政策限制或路由模型能力。

Harness / Runtime MAY 在已授權模型範圍內做本機選擇，但 MUST NOT 超出範圍。

## 16. 記憶體與資料邊界

Agent 狀態 MUST 區分：

|狀態類型 |意義|權威|
|---|---|---|
| Ephemeral Runtime State | 單次執行的暫存狀態 | Runtime 本機 |
| Working Memory | 任務期間可重用之受控狀態 | Contract governed |
| Long-term Agent Memory | 跨執行的 Agent 記憶體 | Governed store / 政策 |
| Enterprise Knowledge | 正式知識、文件、目錄、儲存庫 facts | External authoritative 來源 |
| Business/System Data | CRM、ERP、operational 資料等 | Domain 權限 |

Runtime 快取、Harness 上下文、conversation 上下文或本機 vector 狀態 MUST NOT 自動成為 Enterprise 來源 of truth。

Write-back 到長期記憶或 authoritative 資料來源 MUST 具有明確 write permission、來源與稽核證據。

## 17. 可觀察性與稽核

每次執行 SHOULD 具備跨 Control Plane、Harness 與 Runtime 可關聯的 correlation 身分。

最低證據 SHOULD 包含：執行 admitted / 已拒絕、政策上下文 / version 參考、核准 / 授權狀態、Harness / Runtime / 轉接器身分、委派 chain、模型/工具/記憶體能力 calls、denied operations、預算 / timeout / retry events、cancellation / 撤銷、最終狀態 / 結果參考、失敗 / 升級處理證據。

Observability system MUST NOT 因記錄資料而無限制取得原始 secret、敏感提示詞、private business 資料或供應商憑證。

## 18. 故障隔離與恢復

Agent Architecture MUST 將實作失敗與治理決策分離。

- Harness / Runtime 失敗 MAY 觸發 retry 或 fallback，但不得降低政策 / 核准 requirement。
- Provider unavailable MAY 切換替代供應商，但替代者 MUST 滿足相同或更嚴格契約 constraints。
- Chain 中介層失敗 MUST NOT 自動跳過該層的 mandatory 治理 constraints。
- Policy 服務 / 核准證據無法驗證時，受管制操作 SHOULD fail 已關閉，除非上位政策明確允許 fail-open。
- Repeated anomalous 行為 MAY 觸發 quarantine。
- Control Plane MUST 能停止新執行准入或撤銷既有執行權限。

## 19. 安全邊界

- Least privilege 為預設。
- Credential 範圍 MUST 與任務 / 工具範圍對齊。
- Isolation 層級 MUST 可由契約指定。
- Tool / 供應商 / 下游 Harness input MUST 視為不可信邊界，除非另有明確 trust 政策。
- Prompt / 工具輸出 / retrieved content MUST NOT 直接取得政策權限。
- Harness / Runtime MUST 防止 external content 透過 instruction injection 取得超出契約的操作權限。
- Secrets SHOULD 以參考 / 已委派存取方式提供，避免寫入長期記憶體、logs 或模型上下文。

## 20.生命週期模型

### 20.1 代理定義生命週期

候選項目 → Active → Deprecated → 退休

### 20.2 工具/執行環境實作生命週期

候選項目 → 合格 → Active → Deprecated → 退休

Qualification SHOULD 驗證：契約 compatibility、政策 enforcement 行為、核准語意、委派語意、cancellation / 撤銷 propagation、證據 completeness、隔離要求、供應商轉接器行為、失敗 handling。

更換或重新組合 Harness / Runtime 不等同變更 Agent 身分。

## 21.與現有 AEOS 層對齊

| AEOS 層 |代理架構對應 |
|---|---|
| L1 治理 |政策、審查、風險、審查、標準 |
| L2 Enterprise Architecture | 本文件、Agent logical 架構、cross-platform 邊界 |
| L3 Platform | 未來經核准的具名 Agent-related Platform 邊界；本文件不直接新增 |
| L4 Capability | 編排、政策 enforcement、執行、工具存取、記憶體、可觀測性等能力定義 |
| L5 Repository | 承載 control-plane / Harness / 執行環境 / 轉接器 / 治理實作或架構 assets 的儲存庫 |
| L6 Implementation | 具體 Harness、執行環境、框架、供應商、部署、SDK、設定 |

L6 實作 MUST NOT 反向覆寫 L1～L5 權限。

## 22. 平台與能力關係

本文件不宣告任何具名 Platform。

Agent Control Plane 與 Agent Harness / Orchestration Boundary 都是 logical 架構 boundaries；只有在符合 AEOS-ARCH-005 Platform 身分、mission、邊界、擁有者、能力與生命週期條件，並完成正式 Architecture Review 後，才可將某一具體 Enterprise Agent Platform 登錄至 Platform Catalog。

同樣地，本文件可界定候選能力 domains，但具名 Capability Catalog 條目須依 AEOS-ARCH-007 與 Catalog 治理另行核准。

## 23. 依賴規則

- 治理 → Control Plane：`Defines / Constrains`。
- Control Plane → 裝備/執行環境：`Defines / Authorizes / Constrains / Delegates Execution`。
- Harness → 下游 Harness / Runtime：`Delegates Execution / Coordinates`，不得 `Delegates Governance`。
- Harness / Runtime → Control Plane：`Reports / Requests / References`；不得 `Overrides`。
- Harness／執行環境 → 供應商轉接器：`Consumes`。
- 供應商轉接器→供應商：`Integrates With`。
- 稽核/可觀察性←Control Plane/Harness／執行環境/轉接器：`Receives Evidence`。
- Harness / Runtime / Provider MUST NOT 建立對上位治理的 reverse-control 相依性。
- Cross-runtime / cross-harness fallback MUST 經 Control Plane 政策或契約明確允許。

## 24. 一致性要求

一個 Agent 實作欲宣稱符合本架構，至少 MUST 證明：

1. Enterprise control 權限與執行權限可清楚區分。
2. Harness / Runtime 接收並遵守版本化 Execution Contract 或等效受治理介面。
3. Harness / Runtime 無法自行提升核准 / 授權。
4. Delegated 執行權限不得超過上游範圍。
5. Tool、模型、記憶體/資料、憑證範圍可受控且沿 chain 不擴張。
6. Cancellation / 撤銷可傳遞至下游執行。
7. Audit correlation 可跨主要 Harness / Runtime 階段維持。
8. Provider-specific 語意被限制在實作 / 轉接器邊界。
9. 替換或重新組合 Harness / Runtime 不要求改寫 Enterprise 政策語意。
10. Execution chain 失敗不會自動造成治理 bypass。
11. 具體產品名稱不是符合性的必要條件。
12. Architecture 不預先固定任何具名實作的監督器 / 執行環境 / 供應商角色。

## 25.複習問題

Architecture Review MUST 至少確認：

- Control Plane 是否被誤定義為某一產品？
- 是否誤把具名 Harness / Runtime 固定成監督器或執行環境角色？
- Harness / Runtime 是否持有不應有的治理權限？
- Delegation 是否可能造成權限 expansion？
- Composite chain 是否維持核准、授權、範圍、撤銷與證據延續性？
- Execution Contract 是否足以支援核准、授權、委派、撤銷與稽核？
- Provider / Harness 擴充是否可能覆寫 mandatory fields？
- Memory / 資料權限是否清楚區分？
- Runtime / Harness substitution 是否仍需重寫治理語意？
- 本文件是否意外新增未經核准的具名 Platform / Capability？

## 26. 核准和基準集成

本文件目前為 **Approved 1.0.0**。

核准後：

- 登錄為 AEOS-ARCH-001 Approved Architecture Register 正式條目。
- 作為 Enterprise AI Agent Architecture 的正式定義載體。
- 不得據此宣稱任何具名產品、Harness、執行環境或供應商已獲 AEOS 架構核准；本文件核准的是 logical 架構、權限邊界與符合性要求。
- 必要時可於後續 EWO 同步 AEOS-ARCH-004／005／006／007／009／010 的 cross-reference，但不得重複定義本文件內容。

## 27. 參考文獻

- AEOS-ARCH-001 — Architecture Baseline
- AEOS-ARCH-004 — AI Enterprise Architecture Overview
- AEOS-ARCH-005 — Platform Architecture
- AEOS-ARCH-006 — Layer Architecture
- AEOS-ARCH-007 — Capability Architecture
- AEOS-ARCH-009 — Dependency Architecture
- AEOS-ARCH-010 — Workspace Architecture
- AEOS-ARCH-012 — Architecture Principles
- AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision
- EWO-AEOS-0044 — Enterprise AI Agent Architecture 基礎

## 28. 修訂歷史

|版本 |日期 |改變 |作者 |
|---|---|---|---|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0044 Post-Merge Closure Verification：升級為 Approved Architecture，納入 AEOS-ARCH-001 Approved Architecture Register，作為 Enterprise AI Agent Architecture 正式定義載體 | Codex |
| 0.2.0 | 2026-08-26 | 補入 Agent Harness / Orchestration Boundary、Delegated Execution Control、Composable Harness / Runtime Chain、Harness Neutral 與 chain 符合性；不預先固定任何具名實作角色 | ChatGPT |
| 0.1.0 | 2026-08-26 | 建立 Enterprise AI Agent Architecture Draft：Control Plane / Runtime 分離、Runtime Neutral、Execution Contract、Provider / Tool / Model / Memory / Approval / Observability / Failure boundaries | ChatGPT |
