---
doc-id: AEOS-SPEC-002
doc-name: Agent Collaboration Model Architecture Spec
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-066
  - AEOS-ISSUE-067
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-001
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-013
  - AEOS-SPEC-001
---

# AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec

## Executive Summary

本規格定義 AEOS 的 runtime-neutral Agent Collaboration Model。它建立 Role Archetype、Agent Profile、Capability、Skill / Tool、Policy、Task Decomposition、Separation of Duties、Independent Verification / Reality Checker、Quality Gates、Evidence / Confidence / Provenance、Information Lifecycle、Human Approval / Escalation 與 Runtime Neutral execution boundary 的共同語意。

本規格承接 AEOS-ADR-005 的 ownership decision：Agent Collaboration governance belongs to AEOS。Agent Control Plane 執行 AEOS policy；Agent Runtime 僅執行已授權 request 並回傳 execution evidence；Product repositories 僅做 adoption mapping，不建立平行 multi-agent governance。

本文件不指定任何具名 runtime、harness、model provider、tool provider、workflow engine、vector database、CRM product、deployment topology、SDK、API implementation 或 production operation。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-002 |
| 文件名稱 | Agent Collaboration Model Architecture Spec |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #66、AEOS Issue #67、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013 |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-007、AEOS-ARCH-009、AEOS-SPEC-001 |

## 1. Purpose

本規格目的為：

- 建立 AEOS Agent Collaboration 的共同概念模型。
- 定義 multi-agent collaboration 的治理責任與 execution boundary。
- 支援 task decomposition、role assignment、independent verification、quality gates 與 escalation。
- 確保 collaboration governance 不依賴任一 runtime、harness、provider、workflow engine 或 product repository。
- 提供 downstream product repositories 的 adoption mapping 基準。

## 2. Scope

### 2.1 In Scope

- Role Archetype。
- Agent Profile。
- Capability。
- Skill / Tool。
- Policy。
- Task Decomposition。
- Separation of Duties。
- Independent Verification / Reality Checker。
- Quality Gates。
- Evidence / Confidence / Provenance。
- Information Lifecycle。
- Human Approval / Escalation。
- Runtime Neutral execution boundary。
- Product adoption mapping constraints。

### 2.2 Out of Scope

- Source code、SDK、API、schema implementation 或 runtime adapter implementation。
- Production deployment、activation、operation、monitoring setup 或 infrastructure design。
- 任何具名 runtime、harness、provider、model、tool platform、vector database、workflow engine 或 CRM repository selection。
- Product-specific prompt、persona、domain workflow、sales process、customer service script 或 UI behavior。
- YCRM downstream adoption execution；此類 adoption MUST remain blocked by AEOS until this model is approved or otherwise declared adoptable。

## 3. Governing Authority

| Authority | Role |
|---|---|
| AEOS-ADR-005（Approved 1.0.0） | Agent Collaboration ownership decision |
| AEOS-ADR-003 | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 | Enterprise AI Agent Architecture, Execution Contract and evidence boundary |
| AEOS-ARCH-007 | Capability-first architecture rules |
| AEOS-ARCH-009 | Dependency direction and explicit dependency governance |
| AEOS-SPEC-001 | Local-first routing profile and escalation evidence reference |

Rules：

- AEOS owns Agent Collaboration governance（AEOS-ADR-005 Approved 1.0.0）。
- Agent Control Plane executes AEOS policy。
- Agent Runtime executes authorized requests only。
- Product repositories map adoption; they do not redefine governance。

## 4. Concept Model

### 4.1 Role Archetype

Role Archetype 是 AEOS 定義的抽象 collaboration role，用於描述 agent 在 collaboration 中的責任型態，而非具名 persona、prompt、model 或 runtime。

Role Archetype SHOULD define：

| Field | Meaning |
|---|---|
| Role Archetype ID | Stable role identifier |
| Mission | Role purpose in collaboration |
| Allowed Responsibilities | Responsibilities this role may hold |
| Prohibited Responsibilities | Responsibilities this role must not hold |
| Required Capabilities | Capability classes needed to perform the role |
| Independence Requirements | Separation or conflict rules when applicable |
| Evidence Responsibilities | Evidence the role must produce |

Minimum archetypes MAY include planner、executor、reviewer、reality checker、approver assistant、coordinator and observer, but product adoption MUST map these as AEOS archetypes instead of redefining governance。

### 4.2 Agent Profile

Agent Profile binds one logical agent identity to allowed role archetypes, capability scopes, policy constraints and evidence obligations。

Agent Profile MUST NOT be treated as a runtime process, model identity, prompt file or provider account。

Agent Profile SHOULD include：

| Field | Meaning |
|---|---|
| Agent Profile ID | Stable logical identity |
| Owner / Accountable Reference | Responsible owner or governance reference |
| Allowed Role Archetypes | Roles this profile may assume |
| Capability Scope | Approved capability classes |
| Tool / Skill Scope | Allowed tool or skill classes by operation |
| Memory / Data Scope | Read/write/retain constraints |
| Approval Requirements | Human or system approval dependencies |
| Evidence Requirements | Logs, citations, tests, review notes or other evidence |
| Lifecycle State | Candidate / Active / Deprecated / Retired |

### 4.3 Capability

Capability describes what an agent can responsibly perform under policy, independently from how a runtime implements it。

Capability MUST be defined at capability level before being mapped to skill, tool, model, provider or product workflow。

Capability classification SHOULD distinguish at least：

| Capability Class | Meaning |
|---|---|
| Reasoning | Analyze, infer, compare, decide under policy |
| Planning | Decompose work, sequence tasks, identify dependencies |
| Execution | Perform authorized action through tools or workflow |
| Verification | Validate output, inspect evidence, challenge assumptions |
| Retrieval | Access approved knowledge or data sources |
| Memory Management | Read/write governed memory with provenance |
| Communication | Prepare user-facing or system-facing messages |
| Escalation | Trigger human approval or blocked state |

### 4.4 Skill / Tool

Skill / Tool is an executable or callable capability surface available to an agent under Execution Contract constraints。

Tool availability MUST NOT imply authorization。

Skill / Tool mapping MUST define：

- operation class: observe / read、create / write、update / mutate、delete / destructive、execute、deploy / activate、credential / permission administration;
- allowed role archetypes and agent profiles;
- required approval or quality gate;
- data sensitivity and retention constraints;
- evidence returned after invocation;
- provider / adapter boundary when product-specific or provider-specific。

### 4.5 Policy

Policy defines collaboration rules evaluated or enforced by the Agent Control Plane。

Policy SHOULD cover：

- admission criteria;
- role assignment constraints;
- separation of duties;
- capability and tool scope;
- model / runtime / provider class constraints;
- data sensitivity and information lifecycle;
- approval and escalation;
- quality gates;
- evidence and provenance;
- revocation, cancellation and blocked state。

Runtime-local configuration MAY add stricter safeguards, but MUST NOT relax AEOS policy。

## 5. Collaboration Lifecycle

An AEOS-governed collaboration SHOULD follow this lifecycle：

1. Intake：Control Plane receives task / intent and context。
2. Admission：Control Plane evaluates policy, risk, data sensitivity, authority and readiness。
3. Decomposition：A planner or coordinator decomposes work under authorized scope。
4. Assignment：Control Plane or authorized harness assigns role archetypes and agent profiles。
5. Execution：Runtime / Harness executes authorized sub-requests only。
6. Verification：Independent verifier / reality checker validates outputs and evidence。
7. Quality Gate：Control Plane or authorized review boundary evaluates completion criteria。
8. Escalation / Approval：Human approval is requested when policy requires or confidence is insufficient。
9. Completion：Result, evidence, provenance and confidence are returned。
10. Information Lifecycle Handling：Artifacts, memory writes, retained context and disposal follow policy。

## 6. Task Decomposition

Task Decomposition transforms a task into bounded sub-tasks with role, capability, dependency, quality and evidence requirements。

Every governed sub-task SHOULD define：

| Field | Meaning |
|---|---|
| Sub-task ID | Stable identity for audit correlation |
| Parent Task ID | Link to parent task |
| Goal | Intended outcome |
| Assigned Role Archetype | Required collaboration role |
| Agent Profile | Authorized profile or selection constraints |
| Required Capability | Capability class |
| Authorized Skill / Tool Scope | Allowed operations |
| Input Boundary | Data and context allowed |
| Output Boundary | Expected result shape |
| Quality Gate | Completion / acceptance criteria |
| Evidence Requirement | Required proof, citation, test, review or artifact |
| Escalation Trigger | Conditions requiring human or higher-tier review |

Task Decomposition MUST NOT expand authority beyond the parent Execution Contract。

## 7. Separation of Duties

Separation of Duties prevents one agent, role or runtime from holding incompatible responsibilities in the same governance decision。

AEOS collaboration policy SHOULD define incompatible combinations, including：

| Combination | Default Rule |
|---|---|
| Planner and final approver | SHOULD be separated for high-risk work |
| Executor and independent verifier | MUST be separated when independent verification is required |
| Evidence producer and sole evidence judge | SHOULD be separated |
| Tool mutator and destructive-operation approver | MUST be separated for high-risk or irreversible action |
| Product adoption author and AEOS governance approver | MUST be separated |

If separation cannot be achieved, the task MUST escalate or record an approved exception with bounded scope and evidence。

## 8. Independent Verification / Reality Checker

Independent Verification validates whether outputs are true, complete, authorized and supported by evidence。

Reality Checker is a role archetype specializing in challenge, contradiction detection, source validation, assumption testing and risk surfacing。

Verification SHOULD check：

- facts are traceable to approved sources or explicitly marked uncertain;
- claimed work was actually performed;
- evidence supports the conclusion;
- no policy, approval or scope boundary was bypassed;
- outputs meet quality gates;
- downstream adoption claims are blocked or limited when AEOS authority is not yet approved。

Reality Checker MUST NOT be the same logical decision authority as the executor when independence is required。

## 9. Quality Gates

Quality Gates define conditions that must pass before a collaboration result can be accepted, promoted, merged, used for downstream adoption or escalated as complete。

Quality Gate types MAY include：

| Gate Type | Minimum Meaning |
|---|---|
| Policy Gate | Policy, approval and authorization are satisfied |
| Evidence Gate | Required evidence is present and traceable |
| Verification Gate | Independent verification has passed or exception approved |
| Scope Gate | Output stays within authorized task and product boundary |
| Confidence Gate | Confidence meets threshold or escalates |
| Provenance Gate | Sources, runtime, tool and memory lineage are recorded |
| Human Approval Gate | Required human approval is attached and scoped |

Quality Gate failure MUST produce failure evidence and an escalation or blocked state。

### 9.1 Gate Result Contract

每一個 Quality Gate MUST return one of the following stable result states：

| State | Meaning | Required Next Action |
|---|---|---|
| PASS | Gate requirement satisfied with sufficient evidence | Continue to next gate or completion |
| NEEDS_WORK | Output is incomplete, invalid or insufficient but repairable within authorized scope | Return to assigned role / sub-task with bounded correction request |
| HUMAN_APPROVAL | Gate cannot pass without human decision or authority | Escalate to human approval with decision scope and evidence |
| REJECTED | Output violates policy, scope, evidence or quality requirement and should not be repaired under current request | Stop affected path and record rejection evidence |
| BLOCKED | Required input, authority, source of truth, dependency, approval or capability is unavailable | Pause affected path and record blocker, owner and unblock condition |

Gate result MUST include gate ID, evaluator role, evidence reference, confidence, reason and next action。

### 9.2 Retry / Return Loop

`NEEDS_WORK` MAY return to a planner、executor、researcher、generator or other authorized role only when the parent Execution Contract still allows correction。

Retry / return loop MUST preserve：

- parent task and sub-task correlation identity;
- original authorization scope;
- quality gate failure reason;
- maximum retry or budget constraint;
- required evidence for re-evaluation;
- escalation trigger when repeated correction fails。

Runtime / Harness MAY execute the retry request, but MUST NOT decide that a failed gate has passed unless explicitly authorized by Control Plane policy or assigned verification authority。

## 10. Evidence / Confidence / Provenance

Every governed collaboration MUST produce enough evidence to allow review of what was decided, executed, verified and escalated。

Minimum evidence SHOULD include：

- task, sub-task and execution correlation IDs;
- role archetype and agent profile used;
- policy context and approval state;
- decomposition decision and assignment rationale;
- tool / model / memory / data usage summary;
- source references and provenance;
- confidence level and reason;
- verification result;
- quality gate status;
- escalation, failure, blocked or human approval evidence。

Confidence MUST be supported by evidence or explicitly marked unknown。Confidence MUST NOT be used to override approval or policy requirements。

Provenance SHOULD distinguish：

| Provenance Type | Meaning |
|---|---|
| Source Provenance | Where facts or content came from |
| Execution Provenance | Which runtime / harness / provider class executed |
| Decision Provenance | Which policy, role or approval caused a decision |
| Memory Provenance | What was read, written, retained or discarded |
| Product Mapping Provenance | Which product-specific mapping was applied |

### 10.1 Collaboration Trace Envelope

Every governed collaboration SHOULD produce a Collaboration Trace Envelope or equivalent audit record。

Minimum Trace Envelope fields SHOULD include：

| Field | Meaning |
|---|---|
| `trace_id` | Stable trace identity for the collaboration |
| `parent_task_id` | Parent task or work order identity |
| `sub_task_id` | Sub-task identity when applicable |
| `correlation_id` | Cross-runtime / cross-harness audit correlation identity |
| `role_archetype_id` | AEOS Role Archetype used |
| `agent_profile_id` | Logical Agent Profile identity or selection reference |
| `capability_class` | Capability class exercised |
| `policy_context_ref` | AEOS policy / governance context reference |
| `execution_contract_ref` | Execution Contract or delegated contract reference |
| `source_of_truth_refs` | Approved source references used for verification |
| `tool_model_memory_summary` | Tool, model, memory and data usage summary |
| `gate_results` | Quality Gate states using §9.1 |
| `confidence` | Confidence value and rationale |
| `provenance_refs` | Source, execution, decision, memory and product mapping provenance |
| `approval_evidence_ref` | Human/system approval evidence when required |
| `final_status` | completed / needs_work / escalated / rejected / blocked / cancelled |

Trace Envelope MUST NOT include unnecessary secrets, raw credential material or unapproved sensitive data。

### 10.2 Source of Truth Verification

Independent Verification MUST check claims against approved Source of Truth when such source exists。

If no Source of Truth exists, the verification result MUST state the missing authority and return `BLOCKED`, `HUMAN_APPROVAL` or an explicitly scoped uncertainty outcome according to policy。

## 11. Information Lifecycle

Collaboration information MUST be governed from intake through disposal or promotion。

Information lifecycle states SHOULD include：

| State | Meaning |
|---|---|
| Intake Context | User, system or repository context received for the task |
| Working Context | Temporary context used during execution |
| Evidence Artifact | Retained proof needed for audit or review |
| Candidate Memory | Proposed memory or knowledge update pending validation |
| Approved Memory / Knowledge | Promoted memory or knowledge with authority and provenance |
| Discarded / Redacted Context | Context removed according to policy |

Runtime-local context, chat history, retrieved content or vector state MUST NOT automatically become Enterprise Knowledge。Promotion requires policy-compliant provenance, authority and approval。

## 12. Human Approval / Escalation

Human Approval is a governance decision with scope, authority and evidence; it is not merely a UI interaction or runtime flag。

Escalation MUST occur when：

- policy requires human approval;
- risk, data sensitivity, authority or approval state is unknown;
- confidence is below threshold;
- quality gate fails;
- separation of duties cannot be satisfied;
- verification contradicts executor output;
- requested action touches destructive, production, credential, protected or irreversible boundary;
- product adoption depends on unapproved AEOS governance artifact。

Escalation evidence SHOULD include reason code, blocked condition, requested decision, authorized scope if approved, and follow-up execution constraints。

## 13. Runtime Neutral Execution Boundary

Agent Collaboration MUST be executed through AEOS-ARCH-013 compatible Execution Contract semantics。

Control Plane responsibilities：

- evaluate admission, role assignment, decomposition policy, quality gates and approval requirements;
- create or authorize collaboration request / sub-request;
- define evidence, confidence, provenance and lifecycle requirements;
- revoke or cancel execution authority when needed。

Runtime / Harness responsibilities：

- execute only authorized request or delegated sub-request;
- preserve parent constraints;
- enforce or reject unsupported contract semantics;
- avoid expanding role, capability, tool, model, memory, data or credential scope;
- return execution evidence, status, errors, confidence and provenance signals。

Runtime / Harness MUST NOT：

- establish enterprise collaboration governance;
- override AEOS policy;
- approve its own high-risk output;
- decide truth, memory promotion, approval or final gate result unless explicitly assigned that authority by AEOS policy and the Execution Contract;
- convert product workflow into AEOS authority;
- treat provider capability as permission。

## 14. Product Adoption Mapping

Product repositories MAY create adoption mapping from AEOS Agent Collaboration Model to product workflows。

Adoption mapping SHOULD include：

| Mapping Area | Requirement |
|---|---|
| Product role | Map to AEOS Role Archetype |
| Product agent config | Map to AEOS Agent Profile constraints |
| Product workflow step | Map to Task Decomposition sub-task |
| Product tool / integration | Map to Skill / Tool scope and provider boundary |
| Product approval | Map to AEOS approval and escalation semantics |
| Product evidence | Map to AEOS evidence / confidence / provenance requirement |
| Product memory / data | Map to Information Lifecycle and authority boundary |

Adoption mapping MUST identify AEOS dependencies and blocked-by status when AEOS artifacts remain Draft or unapproved。

## 15. Conformance Checklist

An adoption or implementation claiming alignment with this spec SHOULD demonstrate：

1. AEOS remains the owner of collaboration governance。
2. Role Archetype and Agent Profile are separated from runtime, model and provider identities。
3. Capability is defined before skill/tool/provider mapping。
4. Task Decomposition preserves parent authorization scope。
5. Separation of Duties rules are defined and enforced or escalated。
6. Independent Verification / Reality Checker is independent where required。
7. Quality Gates produce pass/fail/blocked evidence。
8. Evidence, confidence and provenance are captured for review。
9. Information lifecycle prevents runtime-local memory from becoming authority automatically。
10. Human approval has scoped decision evidence。
11. Runtime / Harness executes authorized requests only。
12. Runtime / Harness does not decide truth, memory promotion, approval or final gate result unless explicitly authorized。
13. Gate status and retry / return loop use the stable contract in §9.1 and §9.2。
14. Collaboration Trace Envelope or equivalent audit record exists。
15. Product repositories provide adoption mapping, not parallel governance。

## 16. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #66 | GitHub Issue | Agent Collaboration Model Architecture Spec 工作來源 |
| AEOS Issue #67 | GitHub Issue | Agent Collaboration ownership decision 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | Ownership and authority decision |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Runtime-neutral Control Plane / Runtime separation |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Execution Contract, Control Plane and Runtime boundary |
| AEOS-ARCH-007 — Capability Architecture | Architecture | Capability-first architecture authority |
| AEOS-ARCH-009 — Dependency Architecture | Architecture | Explicit dependency and anti-reverse-control authority |
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile | Specification | Routing, escalation and evidence reference |

## 17. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 #66 post-#67 promotion：在 AEOS-ADR-005 Approved 1.0.0 ownership decision 下，升級為 Approved Specification；補明 Gate Result Contract、Retry / Return Loop、Collaboration Trace Envelope、Source of Truth Verification 與 Runtime 不得決定 truth / memory promotion / approval / final gate result 的 boundary | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Collaboration Model Architecture Spec draft，涵蓋 role archetype、agent profile、capability、skill/tool、policy、task decomposition、separation of duties、independent verification、quality gates、evidence/confidence/provenance、information lifecycle、human approval/escalation 與 runtime-neutral execution boundary | Codex |
