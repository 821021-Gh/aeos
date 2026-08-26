---
doc-id: AEOS-ARCH-013
doc-name: Enterprise AI Agent Architecture
doc-type: Architecture
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-26
updated: 2026-08-26
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

本文件建立 AEOS 的 Enterprise AI Agent Architecture 候選基線，定義 Agent Control Plane 與 Agent Runtime Plane 的責任、Authority、Interface、Dependency 與生命週期邊界，並確立 Runtime Neutral 原則。

本架構不指定任何特定 Agent framework、模型供應商、工具平台、memory product 或部署技術。任何具體 runtime 或 provider 均視為可替換實作，必須透過受治理的契約與 Adapter Boundary 接入。Control Plane 保有企業層級的 policy、approval、authorization、routing、audit 與 revocation authority；Runtime 僅能在有效 Execution Contract 與授權範圍內執行工作。

本文件目前為 Draft，不構成 AEOS-ARCH-001 Approved Architecture Register 的正式條目；核准後始得透過後續 amendment 納入正式基線。

## 1. Purpose

本文件之目的為：

- 定義 Enterprise AI Agent 的共同架構語言與責任模型。
- 建立 Agent Control Plane 與 Agent Runtime Plane 的正式邏輯邊界。
- 將 Runtime Neutral、Provider Neutral 與 Adapter Boundary 轉化為可治理規則。
- 定義跨 Runtime 穩定的 Agent Execution Contract。
- 定義 Agent identity、policy、approval、tool、model、memory/data、credential、observability 與 failure containment 的責任歸屬。
- 使 Agent implementation 可演進、替換或並存，而不改變 Enterprise governance semantics。

## 2. Scope

### 2.1 In Scope

- Enterprise Agent logical architecture。
- Control Plane / Runtime Plane boundary。
- Execution Contract 與 Provider Adapter Boundary。
- Agent identity 與 lifecycle control。
- Task admission、orchestration policy 與 runtime routing。
- Policy evaluation、approval state 與 authorization evidence。
- Tool、model、memory/data 與 credential access boundary。
- Audit、telemetry、observability 與 execution evidence。
- Cancellation、revocation、quarantine、failure isolation 與 recovery boundary。
- Runtime portability、provider substitution 與 compatibility requirements。
- 與既有 Platform、Layer、Capability、Repository、Dependency、Workspace Architecture 的映射。

### 2.2 Out of Scope

- 任何具名 Agent framework、runtime、orchestration product 或 vendor selection。
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
| AEOS-ADR-003 | Control Plane / Runtime separation 與 Runtime Neutral 候選決策 |

在 AEOS-ADR-003 與本文件獲 Approved 前，本文件僅為 review candidate。

## 4. Core Concepts

### 4.1 Agent

Agent 是在受治理的 identity、policy、permission 與 execution context 下，接收 intent / task、進行 reasoning / planning、使用允許之 model、memory、data 與 tool，並產生可驗證 execution result 的邏輯執行主體。

Agent 不等同於：

- 單一 model；
- 單一 prompt；
- 單一 runtime process；
- 單一 framework；
- 單一 API client；
- 單一 automation workflow；
- Platform 本身。

### 4.2 Agent Control Plane

Agent Control Plane 是負責 Enterprise-level governance、coordination、admission、authorization、routing、policy、approval 與 evidence requirements 的邏輯控制平面。

Control Plane 是 architecture responsibility boundary，不代表特定產品、service 或 deployment unit。

### 4.3 Agent Runtime Plane

Agent Runtime Plane 是實際承載 Agent execution loop 的受控執行平面。Runtime 可包含 reasoning、planning、model invocation、tool invocation、state handling 與 result assembly，但其行為必須受 Execution Contract 與授權範圍限制。

### 4.4 Provider

Provider 是提供 runtime、model、tool、memory/data 或其他 execution capability 的實作來源。Provider 本身不因技術重要性取得 Enterprise governance authority。

### 4.5 Adapter Boundary

Adapter Boundary 是 Enterprise contract 與 provider-specific implementation 之間的轉譯邊界。Adapter 可處理協定、schema、capability mapping 與 provider extension，但不得改變 Enterprise policy semantics 或提升權限。

## 5. Logical Architecture

Enterprise AI Agent Architecture 定義下列邏輯責任平面：

| Plane / Boundary | Core Responsibility | Authority Level |
|---|---|---|
| Governance Authority | Enterprise policy、architecture、risk/approval rules 的來源 | 上位治理權威 |
| Agent Control Plane | Admission、identity、policy context、approval、authorization、routing、revocation、evidence requirements | Enterprise control authority |
| Execution Contract Boundary | 將 control intent 與 runtime execution 解耦的版本化契約 | Contract authority |
| Agent Runtime Plane | 在授權範圍內執行 task / reasoning / tool-model-memory calls | Execution authority only |
| Provider Adapter Boundary | 連接 runtime/model/tool/memory providers，隔離 provider-specific semantics | Translation authority only |
| External Providers / Systems | 提供實際模型、工具、資料、記憶或運算能力 | No Enterprise architecture authority by default |
| Audit / Observability Boundary | 接收並關聯 execution evidence、telemetry、status | Evidence plane |

主要方向：

`Governance → Control Plane → Execution Contract → Runtime → Adapter → Provider`

Audit / Observability 橫跨 Control Plane、Runtime 與 Provider Adapter，但不取得執行決策權。

## 6. Control Plane Responsibilities

Agent Control Plane MUST：

1. **Agent Identity**：確認 Agent identity、role、owner reference、lifecycle status 與允許執行範圍。
2. **Task Admission**：決定 task 是否可進入 execution lifecycle。
3. **Policy Context**：取得並固定本次 execution 適用的 policy / governance context。
4. **Approval State**：取得或驗證必要的 human/system approval evidence。
5. **Authorization Scope**：建立 tool、model、memory/data、credential 與 environment scope。
6. **Runtime Routing**：依 capability、risk、cost、availability、data boundary 等政策選擇合規 runtime 類型或候選 provider。
7. **Execution Contract**：建立版本化且可驗證的 execution request。
8. **Budget and Limits**：設定時間、token/cost、tool invocation、retry、concurrency 或其他受治理限制。
9. **Revocation / Cancellation**：能撤銷尚未完成之 execution authority。
10. **Evidence Requirements**：定義 audit、telemetry、result provenance 與 completion evidence 最低要求。
11. **Failure Policy**：定義 retry、fallback、quarantine、escalation、human handoff 或 fail-closed / fail-open 條件。

Control Plane MUST NOT：

- 假設任一特定 provider 永久存在。
- 將 provider-specific config 當作 Enterprise policy source。
- 以 runtime 技術能力取代 approval / authorization 決策。

## 7. Runtime Plane Responsibilities

Agent Runtime MUST：

- 驗證 Execution Contract 版本與完整性。
- 僅執行被授權的 Agent / task。
- 僅使用被允許的 model、tool、memory/data 與 credential scope。
- 遵守 budget、timeout、cancellation、sandbox / isolation 與 evidence requirements。
- 將 provider-specific 行為限制在 Runtime / Adapter implementation boundary。
- 回報 execution status、result、error、tool/model calls 與必要 audit evidence。
- 接受 Control Plane 的 cancellation、revocation 或 quarantine 指令。

Agent Runtime MUST NOT：

- 自行將 Proposed / Pending approval 提升為 Approved。
- 自行擴張 permissions 或 credential scope。
- 在未授權時切換至更高權限 provider 或 tool。
- 以 local default 覆寫 Enterprise policy。
- 將 runtime-local memory 或 state 自動升格為 Enterprise source of truth。
- 將 runtime-specific identifier 當作跨 Enterprise 唯一 identity，除非經 contract mapping。
- 因 provider failure 而繞過 control policy。

## 8. Execution Contract

### 8.1 Minimum Contract

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
| Runtime Constraints | timeout、budget、retry、concurrency、isolation、network / environment constraints |
| Evidence | required logs、events、result provenance、audit correlation |
| Completion | success / failure semantics、result schema、handoff / escalation requirements |

### 8.2 Contract Rules

- Contract MUST 具版本識別。
- Runtime MUST 能拒絕不支援或不完整的 contract。
- Contract schema evolution MUST 保持 backward/forward compatibility policy 或明確 migration rule。
- Provider-specific extension MAY 存在，但 MUST 位於 namespace / extension boundary，且不得改寫 mandatory governance fields。
- Contract MUST 支援 idempotency / duplicate detection 所需 identity，若 execution 類型需要。
- Contract MUST 支援 cancellation / revocation correlation。

## 9. Runtime Neutrality

### 9.1 Principle

Enterprise architecture 定義 **what must be governed and guaranteed**，而不是指定 **which product must execute it**。

因此：

- Runtime implementation MUST be replaceable。
- Runtime migration MUST NOT require redefining Enterprise policy semantics。
- Provider selection MUST be policy / capability driven，而非 architecture hard-code。
- 同一 Control Plane MAY 支援多個 runtime implementation。
- 同一 runtime MAY 在不同 Workspace / Product context 下使用不同 policy contract，但不得自行建立 enterprise policy。

### 9.2 Portability Requirements

Runtime 替換時至少下列語意 SHOULD 可維持：

- Agent identity mapping。
- Task / execution identity。
- Approval semantics。
- Tool permission semantics。
- Model/data handling constraints。
- Memory/data scope。
- Cancellation / revocation semantics。
- Audit correlation。
- Completion / error semantics。

若 provider 無法表達必要語意，該 provider MUST 被視為 capability mismatch，而不是降低 Enterprise governance requirement。

## 10. Provider Boundaries

### 10.1 Runtime Provider

提供 execution loop 或 agent orchestration implementation。不得擁有 Enterprise policy authority。

### 10.2 Model Provider

提供 inference / reasoning / embedding / multimodal 等模型能力。Model identity 與 provider identity MUST 與 Agent identity 分離。

### 10.3 Tool Provider

提供外部 action / API / system capability。Tool invocation MUST 受到 operation-level scope 限制；「可連線」不等於「可執行所有操作」。

### 10.4 Memory / Data Provider

提供 transient state、working memory、long-term memory、retrieval、knowledge 或 business data。Data authority、retention 與 write permission MUST 明確區分。

### 10.5 Credential Provider

提供 credential reference、delegation 或 temporary access capability。Runtime SHOULD 取得最小必要 capability，而非直接持有長期高權限 secret。

## 11. Policy and Approval Boundary

### 11.1 Policy

- Enterprise policy source MUST 位於 Runtime 之外的上位治理來源。
- Runtime MAY 執行 local enforcement，但 MUST 以 Control Plane 提供之 policy context / policy reference 為依據。
- Runtime local safety guard MAY 加嚴限制，但 MUST NOT 放寬 Enterprise policy。

### 11.2 Approval

- Approval 是 governance evidence，不是 UI click 或 runtime flag 本身。
- Approval evidence MUST 可追溯至 approver / authority、scope、time、対象 execution 或 operation class。
- Runtime MUST NOT reuse 已過期、scope 不符或已 revocation 的 approval。
- 對需要每次核准的 operation，不得以先前 task approval 推定永久授權。

## 12. Tool Access Boundary

Tool access MUST 以 capability / operation scope 控制，至少區分：

- Read / observe。
- Create / write。
- Update / mutate。
- Delete / destructive action。
- Execute / command。
- Deploy / activate。
- Credential / permission administration。

高風險操作 MAY 要求 per-operation approval、stronger isolation 或不同 runtime class。

Runtime MUST NOT 將一個 broad provider token 自動等同所有 tool operations 均獲授權。

## 13. Model Boundary

Model selection MUST 與 Agent identity、policy 與 runtime implementation 分離。

Control Plane MAY 依下列因素限制或路由 model capability：

- task capability requirement；
- data sensitivity；
- jurisdiction / residency；
- latency；
- cost / token budget；
- context size；
- modality；
- safety / compliance class；
- availability / fallback policy。

Runtime MAY 在已授權 model scope 內做 local selection，但 MUST NOT 超出 scope。

## 14. Memory and Data Boundary

Agent state MUST 區分：

| State Type | Meaning | Authority |
|---|---|---|
| Ephemeral Runtime State | 單次 execution 的暫存狀態 | Runtime local |
| Working Memory | 任務期間可重用之受控狀態 | Contract governed |
| Long-term Agent Memory | 跨 execution 的 Agent memory | Governed store / policy |
| Enterprise Knowledge | 正式知識、文件、catalog、repository facts | External authoritative source |
| Business/System Data | CRM、ERP、operational data 等 | Domain authority |

Runtime cache、conversation context 或 local vector state MUST NOT 自動成為 Enterprise source of truth。

Write-back 到長期記憶或 authoritative data source MUST 具有明確 write permission、provenance 與 audit evidence。

## 15. Observability and Audit

每次 execution SHOULD 具備跨 Control Plane 與 Runtime 可關聯的 correlation identity。

最低 evidence SHOULD 包含：

- execution admitted / rejected；
- policy context / version reference；
- approval / authorization state；
- runtime / adapter class identity；
- model/tool/memory capability calls；
- denied operations；
- budget / timeout / retry events；
- cancellation / revocation；
- final status / result reference；
- failure / escalation evidence。

Observability system MUST NOT 因記錄資料而無限制取得原始 secret、敏感 prompt、private business data 或 provider credential。

## 16. Failure Isolation and Recovery

Agent architecture MUST 將 provider failure 與 governance decision 分離。

- Runtime failure MAY 觸發 retry 或 fallback，但不得降低 policy / approval requirement。
- Provider unavailable MAY 切換替代 provider，但替代者 MUST 滿足相同或更嚴格 contract constraints。
- Policy service / approval evidence 無法驗證時，受管制 operation SHOULD fail closed，除非上位政策明確允許 fail-open。
- Repeated anomalous behavior MAY 觸發 quarantine。
- Control Plane MUST 能停止新 execution admission 或撤銷既有 execution authority。

## 17. Security Boundary

- Least privilege 為預設。
- Credential scope MUST 與 task / tool scope 對齊。
- Runtime isolation level MUST 可由 contract 指定。
- Tool / provider input MUST 視為不可信邊界，除非另有明確 trust policy。
- Prompt / tool output / retrieved content MUST NOT 直接取得 policy authority。
- Runtime MUST 防止 external content 透過 instruction injection 取得超出 contract 的操作權限。
- Secrets SHOULD 以 reference / delegated access 方式提供，避免寫入長期 memory、logs 或 model context。

## 18. Lifecycle Model

### 18.1 Agent Definition Lifecycle

Candidate → Active → Deprecated → Retired

Agent identity、owner、policy class、allowed capability 與 lifecycle change SHOULD 可被治理與追溯。

### 18.2 Runtime Implementation Lifecycle

Candidate → Qualified → Active → Deprecated → Retired

Runtime qualification SHOULD 驗證：

- contract compatibility；
- policy enforcement behavior；
- approval semantics；
- cancellation / revocation；
- evidence completeness；
- isolation requirements；
- provider adapter behavior；
- failure handling。

更換 Runtime 不等同變更 Agent identity。

## 19. Alignment with Existing AEOS Layers

| AEOS Layer | Agent Architecture Mapping |
|---|---|
| L1 Governance | policy、approval、risk、review、standards |
| L2 Enterprise Architecture | 本文件、Agent logical architecture、cross-platform boundary |
| L3 Platform | 未來經核准的具名 Agent-related Platform boundary；本文件不直接新增 |
| L4 Capability | orchestration、policy enforcement、execution、tool access、memory、observability 等 capability 定義 |
| L5 Repository | 承載 control-plane / runtime / adapter / governance implementation 或 architecture assets 的 repositories |
| L6 Implementation | 具體 runtime、framework、provider、deployment、SDK、configuration |

L6 implementation MUST NOT 反向覆寫 L1～L5 authority。

## 20. Platform and Capability Relationship

本文件不宣告任何具名 Platform。

Agent Control Plane 是 logical architecture plane；只有在符合 AEOS-ARCH-005 Platform identity、mission、boundary、owner、capability 與 lifecycle 條件，並完成正式 Architecture Review 後，才可將某一具體 Enterprise Agent Platform 登錄至 Platform Catalog。

同樣地，本文件可界定候選 capability domains，但具名 Capability Catalog 條目須依 AEOS-ARCH-007 與 Catalog governance 另行核准。

## 21. Dependency Rules

- Control Plane → Runtime：`Defines / Authorizes / Constrains`。
- Runtime → Control Plane：`Reports / Requests / References`；不得 `Overrides`。
- Runtime → Provider Adapter：`Consumes`。
- Provider Adapter → Provider：`Integrates With`。
- Audit / Observability ← Control Plane / Runtime / Adapter：`Receives Evidence`。
- Runtime / Provider MUST NOT 建立對上位 governance 的 reverse-control dependency。
- Cross-runtime fallback MUST 經 Control Plane policy 或 contract 明確允許。

## 22. Conformance Requirements

一個 Agent implementation 欲宣稱符合本架構，至少 MUST 證明：

1. Control authority 與 runtime execution authority 可清楚區分。
2. Runtime 接收並遵守版本化 Execution Contract 或等效受治理介面。
3. Runtime 無法自行提升 approval / authorization。
4. Tool、model、memory/data、credential scope 可受控。
5. Cancellation / revocation 可傳遞至 execution。
6. Audit correlation 可跨主要執行階段維持。
7. Provider-specific semantics 被限制在 implementation / adapter boundary。
8. 替換 runtime 不要求改寫 Enterprise policy semantics。
9. Runtime failure 不會自動造成 governance bypass。
10. 具體產品名稱不是符合性的必要條件。

## 23. Review Questions

Architecture Review MUST 至少確認：

- Control Plane 是否被誤定義為某一產品？
- Runtime 是否仍持有不應有的 governance authority？
- Execution Contract 是否足以支援 approval、authorization、revocation 與 audit？
- Provider extension 是否可能覆寫 mandatory fields？
- Memory / data authority 是否清楚區分？
- Runtime substitution 是否仍需重寫 governance semantics？
- 本文件是否意外新增未經核准的具名 Platform / Capability？

## 24. Approval and Baseline Integration

本文件目前為 **Draft 0.1.0**。

核准前：

- 不得登錄為 AEOS-ARCH-001 Approved Architecture Register 正式條目。
- 不得據此宣稱任何具名產品、runtime 或 provider 已獲 AEOS 架構核准。
- 可作為 EWO-AEOS-0044 Review Package 的候選架構載體。

核准後：

- 應透過獨立 baseline amendment 更新 AEOS-ARCH-001 Architecture Register。
- 必要時同步 AEOS-ARCH-004／005／006／007／009／010 的 cross-reference，但不得重複定義本文件內容。

## 25. References

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

## 26. Revision History

| Version | Date | Change | Author |
|---|---|---|---|
| 0.1.0 | 2026-08-26 | 建立 Enterprise AI Agent Architecture Draft：Control Plane / Runtime separation、Runtime Neutral、Execution Contract、Provider / Tool / Model / Memory / Approval / Observability / Failure boundaries | ChatGPT |
