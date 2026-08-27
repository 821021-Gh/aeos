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

## Executive Summary

本文件建立 AEOS 的 Enterprise AI Agent Architecture 候選基線，定義 Agent Control Plane、Agent Harness / Orchestration Boundary、Agent Runtime、Execution Contract 與 Provider Boundary 的責任、Authority、Interface、Dependency 與生命週期邊界，並確立 Runtime Neutral 與 Harness Neutral 原則。

本架構不指定任何特定 Agent framework、Harness、runtime、模型供應商、工具平台、memory product 或部署技術。具體實作可單獨使用、彼此替換或組合成受治理的 execution chain，但不得因位於 supervisor、orchestration 或 runtime 位置而取得 Enterprise governance authority。

Control Plane 保有企業層級的 policy、approval、authorization、admission、composition policy、routing、audit 與 revocation authority；Harness / Runtime 僅能在有效 Execution Contract、delegated execution authority 與授權範圍內執行、協調或轉派工作。

本文件目前為 Approved 1.0.0，已納入 AEOS-ARCH-001 Approved Architecture Register，作為 Enterprise AI Agent Architecture 的正式定義載體。

## 1. Purpose

本文件之目的為：

- 定義 Enterprise AI Agent 的共同架構語言與責任模型。
- 建立 Agent Control Plane 與 Agent Execution Plane 的正式邏輯邊界。
- 定義 Agent Harness / Orchestration Boundary，避免把任何單一產品角色固定為 Enterprise Architecture。
- 將 Runtime Neutral、Harness Neutral、Provider Neutral 與 Adapter Boundary 轉化為可治理規則。
- 定義跨 Harness / Runtime 穩定的 Agent Execution Contract。
- 支援單一 Runtime、Supervisor + Runtime、多 Harness / Runtime Chain 等不同 implementation topology。
- 定義 Agent identity、policy、approval、tool、model、memory/data、credential、observability 與 failure containment 的責任歸屬。
- 使 Agent implementation 可演進、替換、並存或組合，而不改變 Enterprise governance semantics。

## 2. Scope

### 2.1 In Scope

- Enterprise Agent logical architecture。
- Control Plane / Agent Execution Plane boundary。
- Agent Harness / Orchestration Boundary。
- Execution Contract 與 Provider Adapter Boundary。
- Delegated execution control 與 authority propagation。
- Composable Harness / Runtime Chain。
- Agent identity 與 lifecycle control。
- Task admission、orchestration policy 與 runtime / harness routing。
- Policy evaluation、approval state 與 authorization evidence。
- Tool、model、memory/data 與 credential access boundary。
- Audit、telemetry、observability 與 execution evidence。
- Cancellation、revocation、quarantine、failure isolation 與 recovery boundary。
- Runtime / Harness portability、provider substitution 與 compatibility requirements。
- 與既有 Platform、Layer、Capability、Repository、Dependency、Workspace Architecture 的映射。

### 2.2 Out of Scope

- 任何具名 Agent framework、Harness、runtime、orchestration product 或 vendor selection。
- 預先指定某一具名產品必須擔任 supervisor、runtime、model router 或其他唯一角色。
- 任何具名 LLM / model provider selection。
- 任何具名 vector database、memory product、tool platform 或 secret manager selection。
- Production deployment topology、host sizing、network layout 或 infrastructure design。
- Source code、SDK、API implementation details。
- 個別 Product Agent 的 prompt、persona、business workflow 或 domain logic。
- 未經核准的具名 Platform 或 Capability Catalog 條目。

## 3. Architecture Authority

本文件受下列既有 AEOS 架構約束：

| Authority | Role |
|---|---|
| AEOS-ARCH-001 | Architecture Baseline 與最高正式架構入口 |
| AEOS-ARCH-004 | Enterprise Architecture domain relationship |
| AEOS-ARCH-005 | Platform boundary、identity 與 product-neutral platform rules |
| AEOS-ARCH-006 | Layer responsibility 與 anti-bypass / anti-reverse-control rules |
| AEOS-ARCH-007 | Capability-first definition；不得以 implementation 取代 capability |
| AEOS-ARCH-009 | Dependency direction、type 與 governance |
| AEOS-ARCH-010 | Workspace composition 與 cross-repository integration |
| AEOS-ARCH-012 | Architecture Principles |
| AEOS-ADR-003 | Control Plane / Agent Execution separation、Harness / Runtime Neutral 與 composable execution 候選決策 |

AEOS-ADR-003 與本文件已於 EWO-AEOS-0044 closure 中升級為 Approved 1.0.0；本文件為 Enterprise AI Agent Architecture 的正式架構權威。

## 4. Core Concepts

### 4.1 Agent

Agent 是在受治理的 identity、policy、permission 與 execution context 下，接收 intent / task、進行 reasoning / planning、使用允許之 model、memory、data 與 tool，並產生可驗證 execution result 的邏輯執行主體。

Agent 不等同於單一 model、prompt、runtime process、Harness、framework、API client、automation workflow 或 Platform。

### 4.2 Agent Control Plane

Agent Control Plane 是負責 Enterprise-level governance、coordination policy、admission、authorization、routing、composition policy、approval 與 evidence requirements 的邏輯控制平面。

Control Plane 是 architecture responsibility boundary，不代表特定產品、service 或 deployment unit。

### 4.3 Agent Execution Plane

Agent Execution Plane 是承載受控 Agent workload 的邏輯執行平面，可由一個或多個 Agent Harness、orchestration implementation、runtime、adapter 與 provider 組成。

Execution Plane 具有 execution authority，不具有 Enterprise governance authority。

### 4.4 Agent Harness / Orchestration Boundary

Agent Harness / Orchestration Boundary 是負責 execution coordination 的可替換實作邊界。它 MAY 提供：

- task decomposition；
- sub-agent assignment；
- workflow coordination；
- planning / reasoning loop coordination；
- model、tool、memory routing；
- retry、scheduling、local fallback；
- context coordination；
- 對下游 Harness / Runtime 的 delegated execution control。

Harness 可以同時包含 runtime 能力，也可以只負責 orchestration；AEOS 不預先固定產品角色。

### 4.5 Agent Runtime

Agent Runtime 是實際承載 Agent execution loop 或 workload execution 的受控實作。Runtime 可包含 reasoning、planning、model invocation、tool invocation、state handling 與 result assembly，但必須受 Execution Contract 與授權範圍限制。

### 4.6 Provider

Provider 是提供 runtime、model、tool、memory/data、credential 或其他 execution capability 的實作來源。Provider 本身不因技術重要性取得 Enterprise governance authority。

### 4.7 Adapter Boundary

Adapter Boundary 是 Enterprise contract 與 provider-specific implementation 之間的轉譯邊界。Adapter 可處理協定、schema、capability mapping 與 provider extension，但不得改變 Enterprise policy semantics 或提升權限。

## 5. Logical Architecture

Enterprise AI Agent Architecture 定義下列邏輯責任平面：

| Plane / Boundary | Core Responsibility | Authority Level |
|---|---|---|
| Governance Authority | Enterprise policy、architecture、risk/approval rules 的來源 | 上位治理權威 |
| Agent Control Plane | Admission、identity、policy context、approval、authorization、composition、routing、revocation、evidence requirements | Enterprise control authority |
| Execution Contract Boundary | 將 control intent 與 execution implementation 解耦的版本化契約 | Contract authority |
| Agent Harness / Orchestration Boundary | task decomposition、coordination、sub-agent / downstream execution delegation | Delegated execution authority only |
| Agent Runtime | 在授權範圍內執行 task / reasoning / tool-model-memory calls | Execution authority only |
| Provider Adapter Boundary | 連接 runtime/model/tool/memory providers，隔離 provider-specific semantics | Translation authority only |
| External Providers / Systems | 提供實際模型、工具、資料、記憶或運算能力 | No Enterprise architecture authority by default |
| Audit / Observability Boundary | 接收並關聯 execution evidence、telemetry、status | Evidence plane |

最簡單 topology：

`Governance → Control Plane → Execution Contract → Runtime → Adapter → Provider`

允許的 composite topology：

`Governance → Control Plane → Execution Contract → Harness A → Harness / Runtime B → Adapter → Provider`

或：

`Governance → Control Plane → Execution Contract → Harness A → Runtime B / Runtime C → Providers`

這些 topology 是 implementation options，不是具名產品角色定義。

Audit / Observability 橫跨 Control Plane、Harness、Runtime 與 Provider Adapter，但不取得執行或治理決策權。

## 6. Control Plane Responsibilities

Agent Control Plane MUST：

1. **Agent Identity**：確認 Agent identity、role、owner reference、lifecycle status 與允許執行範圍。
2. **Task Admission**：決定 task 是否可進入 execution lifecycle。
3. **Policy Context**：取得並固定本次 execution 適用的 policy / governance context。
4. **Approval State**：取得或驗證必要的 human/system approval evidence。
5. **Authorization Scope**：建立 tool、model、memory/data、credential 與 environment scope。
6. **Composition Policy**：決定允許使用單一 Runtime、Harness + Runtime 或多層 Harness / Runtime Chain 的條件。
7. **Runtime / Harness Routing**：依 capability、risk、cost、availability、data boundary 等政策選擇合規 implementation。
8. **Execution Contract**：建立版本化且可驗證的 execution request。
9. **Budget and Limits**：設定時間、token/cost、tool invocation、retry、concurrency 或其他受治理限制。
10. **Revocation / Cancellation**：能撤銷尚未完成之 execution authority，且要求向下游 propagation。
11. **Evidence Requirements**：定義 audit、telemetry、result provenance 與 completion evidence 最低要求。
12. **Failure Policy**：定義 retry、fallback、quarantine、escalation、human handoff 或 fail-closed / fail-open 條件。

Control Plane MUST NOT：

- 假設任一特定 provider、Harness 或 runtime 永久存在。
- 將 provider-specific / harness-specific config 當作 Enterprise policy source。
- 以 implementation 技術能力取代 approval / authorization 決策。
- 因某實作被稱為 supervisor / orchestrator 而自動賦予 Enterprise governance authority。

## 7. Delegated Execution Control

### 7.1 Delegation Principle

**Execution control MAY be delegated；Enterprise governance authority MUST NOT be implicitly delegated。**

Control Plane MAY 將局部執行協調責任委派給 Harness / Runtime，包括 task decomposition、sub-agent assignment、workflow coordination、local model/tool routing、retry 與 scheduling。

### 7.2 Delegation Requirements

每一次 delegated execution authority MUST：

- 可追溯至上游 Execution Contract 或其受治理子契約；
- 不得超過上游 approval / authorization scope；
- 明確保留 budget、timeout、tool/model/memory/data/credential constraints；
- 保留 execution、task 與 audit correlation identity；
- 支援 cancellation / revocation 向下游傳遞；
- 不得被中介 Harness / Runtime 擴權。

### 7.3 Harness / Orchestration MUST NOT

- 自行將 Proposed / Pending approval 提升為 Approved。
- 自行擴張 permissions 或 credential scope。
- 將 delegated execution control 重新解釋為 unrestricted authority。
- 以 local default 覆寫 Enterprise policy。
- 將自身 supervisor / orchestration 角色升格為 Enterprise governance authority。
- 將下游技術可行性推定為治理允許性。

## 8. Runtime Responsibilities

Agent Runtime MUST：

- 驗證 Execution Contract / delegated contract 版本與完整性。
- 僅執行被授權的 Agent / task。
- 僅使用被允許的 model、tool、memory/data 與 credential scope。
- 遵守 budget、timeout、cancellation、sandbox / isolation 與 evidence requirements。
- 將 provider-specific 行為限制在 Runtime / Adapter implementation boundary。
- 回報 execution status、result、error、tool/model calls 與必要 audit evidence。
- 接受 Control Plane 或經授權上游 Harness 的 cancellation、revocation 或 quarantine 指令。

Agent Runtime MUST NOT：

- 自行將 Proposed / Pending approval 提升為 Approved。
- 自行擴張 permissions 或 credential scope。
- 在未授權時切換至更高權限 provider 或 tool。
- 以 local default 覆寫 Enterprise policy。
- 將 runtime-local memory 或 state 自動升格為 Enterprise source of truth。
- 因 provider failure 而繞過 control policy。

## 9. Execution Contract

### 9.1 Minimum Contract

每次可治理 execution MUST 具有可追溯的 Execution Contract，至少包含：

| Field Group | Minimum Meaning |
|---|---|
| Contract | contract version、created time、issuer/control-plane reference |
| Execution Identity | execution ID、task ID、correlation ID |
| Agent Identity | agent ID / role reference、owner / accountable reference |
| Intent | task / goal / requested outcome |
| Policy | policy context reference、risk class / control class（如適用） |
| Approval | approval requirement、approval state、approval evidence reference |
| Tool Scope | allowed / denied tool capabilities、operation scope |
| Model Scope | allowed model capability/class、data handling constraints |
| Memory/Data Scope | readable/writable sources、retention / sensitivity constraints |
| Credential Scope | credential reference / capability scope；不得傳遞不必要 secret material |
| Delegation | allowed downstream delegation、maximum scope、chain / parent identity |
| Runtime Constraints | timeout、budget、retry、concurrency、isolation、network / environment constraints |
| Evidence | required logs、events、result provenance、audit correlation |
| Completion | success / failure semantics、result schema、handoff / escalation requirements |

### 9.2 Contract Rules

- Contract MUST 具版本識別。
- Harness / Runtime MUST 能拒絕不支援或不完整的 contract。
- Contract schema evolution MUST 保持 backward/forward compatibility policy 或明確 migration rule。
- Provider / Harness-specific extension MAY 存在，但 MUST 位於 namespace / extension boundary，且不得改寫 mandatory governance fields。
- Contract MUST 支援 idempotency / duplicate detection 所需 identity，若 execution 類型需要。
- Contract MUST 支援 cancellation / revocation correlation。
- 下游 delegated contract MUST NOT 放寬上游 mandatory governance constraints。

## 10. Runtime and Harness Neutrality

### 10.1 Principle

Enterprise Architecture 定義 **what must be governed and guaranteed**，而不是指定 **which product must execute or orchestrate it**。

因此：

- Harness / Runtime implementation MUST be replaceable。
- Harness / Runtime MAY coexist or compose when policy allows。
- Migration / recomposition MUST NOT require redefining Enterprise policy semantics。
- Provider / Harness selection MUST be policy / capability driven，而非 architecture hard-code。
- 同一 Control Plane MAY 支援多個 Harness / Runtime implementation。
- AEOS MUST NOT 預先指定具名產品必須扮演 supervisor、runtime、model router 或其他唯一角色。

### 10.2 Portability Requirements

Harness / Runtime 替換或重新組合時至少下列語意 SHOULD 可維持：

- Agent identity mapping。
- Task / execution / parent-child identity。
- Approval semantics。
- Authorization / delegated authority semantics。
- Tool permission semantics。
- Model/data handling constraints。
- Memory/data scope。
- Cancellation / revocation propagation。
- Audit correlation。
- Completion / error semantics。

若 implementation 無法表達必要語意，該 implementation MUST 被視為 capability mismatch，而不是降低 Enterprise governance requirement。

## 11. Composable Harness / Runtime Chain

一個 execution MAY 經過多個 Harness / Runtime 層級，但整條 chain MUST 維持治理不變式。

### 11.1 Mandatory Invariants

- **No Authority Expansion**：下游 authority 不得大於上游授權。
- **No Approval Inflation**：下游不得自行建立更高 approval state。
- **Scope Monotonicity**：tool/model/memory/data/credential scope 只能相同或收斂，不得自行放寬。
- **Identity Continuity**：execution / task / correlation / parent-child identity 必須可追溯。
- **Revocation Propagation**：上游 cancellation / revocation 必須可向下游傳遞。
- **Evidence Continuity**：主要 decision / execution evidence 必須可跨 chain 關聯。
- **Failure Containment**：任一中介 failure 不得造成 governance bypass。

### 11.2 Chain Examples

以下皆為合法的抽象形態，前提是符合本文件規則：

- `Control Plane → Runtime → Provider`
- `Control Plane → Harness → Runtime → Provider`
- `Control Plane → Harness A → Harness B → Runtime → Provider`
- `Control Plane → Harness → Runtime A / Runtime B → Providers`

上述名稱均為 logical roles，不代表任何具名產品。

## 12. Provider Boundaries

### 12.1 Harness / Runtime Provider

提供 orchestration、execution loop、agent workflow 或 supervisor implementation。不得擁有 Enterprise policy authority。

### 12.2 Model Provider

提供 inference / reasoning / embedding / multimodal 等模型能力。Model identity 與 provider identity MUST 與 Agent identity 分離。

### 12.3 Tool Provider

提供外部 action / API / system capability。Tool invocation MUST 受到 operation-level scope 限制；「可連線」不等於「可執行所有操作」。

### 12.4 Memory / Data Provider

提供 transient state、working memory、long-term memory、retrieval、knowledge 或 business data。Data authority、retention 與 write permission MUST 明確區分。

### 12.5 Credential Provider

提供 credential reference、delegation 或 temporary access capability。Execution implementation SHOULD 取得最小必要 capability，而非直接持有長期高權限 secret。

## 13. Policy and Approval Boundary

### 13.1 Policy

- Enterprise policy source MUST 位於 Agent Execution Plane 之外的上位治理來源。
- Harness / Runtime MAY 執行 local enforcement，但 MUST 以上位 policy context / policy reference 為依據。
- Local safety guard MAY 加嚴限制，但 MUST NOT 放寬 Enterprise policy。

### 13.2 Approval

- Approval 是 governance evidence，不是 UI click 或 runtime flag 本身。
- Approval evidence MUST 可追溯至 approver / authority、scope、time、対象 execution 或 operation class。
- Harness / Runtime MUST NOT reuse 已過期、scope 不符或已 revoked 的 approval。
- 對需要每次核准的 operation，不得以先前 task approval 推定永久授權。

## 14. Tool Access Boundary

Tool access MUST 以 capability / operation scope 控制，至少區分：Read / observe、Create / write、Update / mutate、Delete / destructive action、Execute / command、Deploy / activate、Credential / permission administration。

高風險操作 MAY 要求 per-operation approval、stronger isolation 或不同 execution class。

任何 Harness / Runtime MUST NOT 將 broad provider token 自動等同所有 tool operations 均獲授權。

## 15. Model Boundary

Model selection MUST 與 Agent identity、policy 與 Harness / Runtime implementation 分離。

Control Plane MAY 依 task capability、data sensitivity、jurisdiction / residency、latency、cost / token budget、context size、modality、safety / compliance class、availability / fallback policy 限制或路由 model capability。

Harness / Runtime MAY 在已授權 model scope 內做 local selection，但 MUST NOT 超出 scope。

## 16. Memory and Data Boundary

Agent state MUST 區分：

| State Type | Meaning | Authority |
|---|---|---|
| Ephemeral Runtime State | 單次 execution 的暫存狀態 | Runtime local |
| Working Memory | 任務期間可重用之受控狀態 | Contract governed |
| Long-term Agent Memory | 跨 execution 的 Agent memory | Governed store / policy |
| Enterprise Knowledge | 正式知識、文件、catalog、repository facts | External authoritative source |
| Business/System Data | CRM、ERP、operational data 等 | Domain authority |

Runtime cache、Harness context、conversation context 或 local vector state MUST NOT 自動成為 Enterprise source of truth。

Write-back 到長期記憶或 authoritative data source MUST 具有明確 write permission、provenance 與 audit evidence。

## 17. Observability and Audit

每次 execution SHOULD 具備跨 Control Plane、Harness 與 Runtime 可關聯的 correlation identity。

最低 evidence SHOULD 包含：execution admitted / rejected、policy context / version reference、approval / authorization state、Harness / Runtime / adapter identity、delegation chain、model/tool/memory capability calls、denied operations、budget / timeout / retry events、cancellation / revocation、final status / result reference、failure / escalation evidence。

Observability system MUST NOT 因記錄資料而無限制取得原始 secret、敏感 prompt、private business data 或 provider credential。

## 18. Failure Isolation and Recovery

Agent Architecture MUST 將 implementation failure 與 governance decision 分離。

- Harness / Runtime failure MAY 觸發 retry 或 fallback，但不得降低 policy / approval requirement。
- Provider unavailable MAY 切換替代 provider，但替代者 MUST 滿足相同或更嚴格 contract constraints。
- Chain 中介層 failure MUST NOT 自動跳過該層的 mandatory governance constraints。
- Policy service / approval evidence 無法驗證時，受管制 operation SHOULD fail closed，除非上位政策明確允許 fail-open。
- Repeated anomalous behavior MAY 觸發 quarantine。
- Control Plane MUST 能停止新 execution admission 或撤銷既有 execution authority。

## 19. Security Boundary

- Least privilege 為預設。
- Credential scope MUST 與 task / tool scope 對齊。
- Isolation level MUST 可由 contract 指定。
- Tool / provider / downstream Harness input MUST 視為不可信邊界，除非另有明確 trust policy。
- Prompt / tool output / retrieved content MUST NOT 直接取得 policy authority。
- Harness / Runtime MUST 防止 external content 透過 instruction injection 取得超出 contract 的操作權限。
- Secrets SHOULD 以 reference / delegated access 方式提供，避免寫入長期 memory、logs 或 model context。

## 20. Lifecycle Model

### 20.1 Agent Definition Lifecycle

Candidate → Active → Deprecated → Retired

### 20.2 Harness / Runtime Implementation Lifecycle

Candidate → Qualified → Active → Deprecated → Retired

Qualification SHOULD 驗證：contract compatibility、policy enforcement behavior、approval semantics、delegation semantics、cancellation / revocation propagation、evidence completeness、isolation requirements、provider adapter behavior、failure handling。

更換或重新組合 Harness / Runtime 不等同變更 Agent identity。

## 21. Alignment with Existing AEOS Layers

| AEOS Layer | Agent Architecture Mapping |
|---|---|
| L1 Governance | policy、approval、risk、review、standards |
| L2 Enterprise Architecture | 本文件、Agent logical architecture、cross-platform boundary |
| L3 Platform | 未來經核准的具名 Agent-related Platform boundary；本文件不直接新增 |
| L4 Capability | orchestration、policy enforcement、execution、tool access、memory、observability 等 capability 定義 |
| L5 Repository | 承載 control-plane / harness / runtime / adapter / governance implementation 或 architecture assets 的 repositories |
| L6 Implementation | 具體 Harness、runtime、framework、provider、deployment、SDK、configuration |

L6 implementation MUST NOT 反向覆寫 L1～L5 authority。

## 22. Platform and Capability Relationship

本文件不宣告任何具名 Platform。

Agent Control Plane 與 Agent Harness / Orchestration Boundary 都是 logical architecture boundaries；只有在符合 AEOS-ARCH-005 Platform identity、mission、boundary、owner、capability 與 lifecycle 條件，並完成正式 Architecture Review 後，才可將某一具體 Enterprise Agent Platform 登錄至 Platform Catalog。

同樣地，本文件可界定候選 capability domains，但具名 Capability Catalog 條目須依 AEOS-ARCH-007 與 Catalog governance 另行核准。

## 23. Dependency Rules

- Governance → Control Plane：`Defines / Constrains`。
- Control Plane → Harness / Runtime：`Defines / Authorizes / Constrains / Delegates Execution`。
- Harness → downstream Harness / Runtime：`Delegates Execution / Coordinates`，不得 `Delegates Governance`。
- Harness / Runtime → Control Plane：`Reports / Requests / References`；不得 `Overrides`。
- Harness / Runtime → Provider Adapter：`Consumes`。
- Provider Adapter → Provider：`Integrates With`。
- Audit / Observability ← Control Plane / Harness / Runtime / Adapter：`Receives Evidence`。
- Harness / Runtime / Provider MUST NOT 建立對上位 governance 的 reverse-control dependency。
- Cross-runtime / cross-harness fallback MUST 經 Control Plane policy 或 contract 明確允許。

## 24. Conformance Requirements

一個 Agent implementation 欲宣稱符合本架構，至少 MUST 證明：

1. Enterprise control authority 與 execution authority 可清楚區分。
2. Harness / Runtime 接收並遵守版本化 Execution Contract 或等效受治理介面。
3. Harness / Runtime 無法自行提升 approval / authorization。
4. Delegated execution authority 不得超過上游 scope。
5. Tool、model、memory/data、credential scope 可受控且沿 chain 不擴張。
6. Cancellation / revocation 可傳遞至 downstream execution。
7. Audit correlation 可跨主要 Harness / Runtime 階段維持。
8. Provider-specific semantics 被限制在 implementation / adapter boundary。
9. 替換或重新組合 Harness / Runtime 不要求改寫 Enterprise policy semantics。
10. Execution chain failure 不會自動造成 governance bypass。
11. 具體產品名稱不是符合性的必要條件。
12. Architecture 不預先固定任何具名 implementation 的 supervisor / runtime / provider 角色。

## 25. Review Questions

Architecture Review MUST 至少確認：

- Control Plane 是否被誤定義為某一產品？
- 是否誤把具名 Harness / Runtime 固定成 supervisor 或 runtime 角色？
- Harness / Runtime 是否持有不應有的 governance authority？
- Delegation 是否可能造成 authority expansion？
- Composite chain 是否維持 approval、authorization、scope、revocation 與 evidence continuity？
- Execution Contract 是否足以支援 approval、authorization、delegation、revocation 與 audit？
- Provider / Harness extension 是否可能覆寫 mandatory fields？
- Memory / data authority 是否清楚區分？
- Runtime / Harness substitution 是否仍需重寫 governance semantics？
- 本文件是否意外新增未經核准的具名 Platform / Capability？

## 26. Approval and Baseline Integration

本文件目前為 **Approved 1.0.0**。

核准後：

- 登錄為 AEOS-ARCH-001 Approved Architecture Register 正式條目。
- 作為 Enterprise AI Agent Architecture 的正式定義載體。
- 不得據此宣稱任何具名產品、Harness、runtime 或 provider 已獲 AEOS 架構核准；本文件核准的是 logical architecture、authority boundary 與 conformance requirements。
- 必要時可於後續 EWO 同步 AEOS-ARCH-004／005／006／007／009／010 的 cross-reference，但不得重複定義本文件內容。

## 27. References

- AEOS-ARCH-001 — Architecture Baseline
- AEOS-ARCH-004 — AI Enterprise Architecture Overview
- AEOS-ARCH-005 — Platform Architecture
- AEOS-ARCH-006 — Layer Architecture
- AEOS-ARCH-007 — Capability Architecture
- AEOS-ARCH-009 — Dependency Architecture
- AEOS-ARCH-010 — Workspace Architecture
- AEOS-ARCH-012 — Architecture Principles
- AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision
- EWO-AEOS-0044 — Enterprise AI Agent Architecture Foundation

## 28. Revision History

| Version | Date | Change | Author |
|---|---|---|---|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0044 Post-Merge Closure Verification：升級為 Approved Architecture，納入 AEOS-ARCH-001 Approved Architecture Register，作為 Enterprise AI Agent Architecture 正式定義載體 | Codex |
| 0.2.0 | 2026-08-26 | 補入 Agent Harness / Orchestration Boundary、Delegated Execution Control、Composable Harness / Runtime Chain、Harness Neutral 與 chain conformance；不預先固定任何具名 implementation 角色 | ChatGPT |
| 0.1.0 | 2026-08-26 | 建立 Enterprise AI Agent Architecture Draft：Control Plane / Runtime separation、Runtime Neutral、Execution Contract、Provider / Tool / Model / Memory / Approval / Observability / Failure boundaries | ChatGPT |
