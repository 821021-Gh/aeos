---
doc-id: AEOS-SPEC-003
doc-name: Agent Control Plane Orchestration Extension
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-068
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-001
  - AEOS-SPEC-002
---

# AEOS-SPEC-003 — Agent Control Plane Orchestration Extension

## Executive Summary

本規格定義 AEOS Agent Control Plane 的 Agent Collaboration orchestration extension。它將 AEOS-SPEC-002 的 Agent Collaboration Model 操作化為 Control Plane 可治理的 admission、task decomposition、profile resolution、capability dispatch、quality gate execution、`NEEDS_WORK` return loop、human approval escalation、runtime adapter execution request 與 collaboration trace emission。

本規格不實作 Control Plane、不定義 runtime adapter 程式、不選擇任何 harness、runtime、model、provider、workflow engine、vector database 或 product repository。它只定義 Control Plane 在執行 AEOS policy 時必須持有或協調的 orchestration semantics。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-003 |
| 文件名稱 | Agent Control Plane Orchestration Extension |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #68、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-002 |
| 關聯文件 | AEOS-SPEC-001、AEOS-SPEC-002 |

## 1. Purpose

本規格目的為：

- 定義 Agent Control Plane 如何依 AEOS Agent Collaboration Model 執行 orchestration。
- 定義 task decomposition、role selection、profile resolution 與 capability dispatch 的治理邊界。
- 定義 Gate runner 與共通 gate decision schema。
- 定義 `NEEDS_WORK` return loop 與 `HUMAN_APPROVAL` escalation semantics。
- 定義 Runtime adapter execution request 的最小邊界。
- 定義 Agent Collaboration Run、Role Assignment 與 Gate Decision trace emission。

## 2. Scope

### 2.1 In Scope

- Orchestrator responsibilities。
- Agent Profile resolution。
- Capability dispatch。
- Role selection。
- Gate runner。
- Gate decision schema。
- `NEEDS_WORK` return loop。
- Human approval escalation。
- Runtime adapter execution request boundary。
- Agent Collaboration Run trace。
- Role Assignment trace。
- Gate Decision trace。

### 2.2 Out of Scope

- Source code、SDK、API endpoint 或 database schema implementation。
- Runtime adapter implementation。
- CRM Memory、Learning Case、Knowledge 或任何 product domain capability implementation。
- Production deployment、customer data mutation、credential provisioning 或 runtime operations。
- 任一具名 runtime、harness、provider、model、workflow engine、tool platform 或 vector database selection。
- 將 Control Plane 變成 product fact source。

## 3. Governing Authority

| Authority | Role |
|---|---|
| AEOS-ADR-005 | Agent Collaboration ownership decision |
| AEOS-ADR-003 | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 | Enterprise AI Agent Architecture, Execution Contract and evidence boundary |
| AEOS-SPEC-001 | Local-first routing profile, escalation and routing telemetry |
| AEOS-SPEC-002 | Agent Collaboration Model, gate result contract and trace envelope |

Rules：

- Agent Control Plane executes AEOS policy。
- Agent Runtime executes authorized request only。
- Runtime success is not source validation success, memory promotion, approval or gate pass。
- Product repositories do adoption mapping only。

## 4. Orchestrator Responsibilities

Agent Control Plane Orchestrator is a logical Control Plane responsibility。It MAY be implemented by one or more services or components, but its authority remains Control Plane authority under AEOS-ARCH-013。

The Orchestrator MUST：

1. receive task / intent and context;
2. evaluate admission policy;
3. classify risk, data sensitivity, complexity and approval requirements;
4. decompose admitted task into governed sub-tasks;
5. select required role archetypes;
6. resolve eligible Agent Profiles;
7. dispatch authorized capabilities;
8. create runtime adapter execution requests;
9. run or coordinate quality gates;
10. handle `NEEDS_WORK` return loops;
11. escalate `HUMAN_APPROVAL` and blocked states;
12. emit Agent Collaboration Run, Role Assignment and Gate Decision traces。

The Orchestrator MUST NOT：

- become a product fact source;
- replace Source of Truth verification;
- override AEOS-ADR-005 ownership;
- let runtime output directly decide approval, truth, memory promotion or final gate result;
- use downstream product adoption pressure to bypass AEOS policy。

## 5. Orchestration Lifecycle

An AEOS-governed orchestration SHOULD follow this lifecycle：

1. **Intake**：receive task, context and requester identity。
2. **Admission**：evaluate AEOS policy, risk, data sensitivity, approval and authority。
3. **Planning**：create bounded task decomposition。
4. **Role Selection**：select role archetypes required by each sub-task。
5. **Profile Resolution**：resolve eligible Agent Profiles。
6. **Capability Dispatch**：select capability class and allowed skill/tool scope。
7. **Execution Request**：create runtime adapter request under Execution Contract constraints。
8. **Evidence Collection**：collect runtime execution evidence and source/provenance references。
9. **Gate Evaluation**：run policy, evidence, verification, confidence and approval gates。
10. **Return / Escalation**：return `NEEDS_WORK`, escalate `HUMAN_APPROVAL`, reject or block as required。
11. **Completion**：emit final trace and result evidence。

## 6. Task Decomposition and Role Selection

Each decomposed sub-task MUST preserve parent authorization scope and MUST NOT introduce broader tool、model、memory/data、credential、runtime or product authority。

Each sub-task SHOULD define：

| Field | Meaning |
|---|---|
| `sub_task_id` | Stable sub-task identity |
| `parent_task_id` | Parent task or collaboration run identity |
| `goal` | Intended outcome |
| `role_archetype_id` | Required AEOS role archetype |
| `capability_class` | Required capability class |
| `input_scope` | Allowed context and data boundary |
| `output_scope` | Expected result boundary |
| `tool_scope` | Allowed skill/tool operation scope |
| `quality_gates` | Required gate IDs and pass criteria |
| `evidence_requirements` | Required evidence and provenance |
| `escalation_triggers` | Conditions for human approval or blocked state |

Role selection MUST be based on task need, policy, risk, data scope, required capability and separation-of-duties constraints。Role selection MUST NOT be based on product persona naming or runtime availability alone。

## 7. Agent Profile Resolution

Profile resolution maps required role archetype and capability class to eligible Agent Profiles。

Profile resolution inputs SHOULD include：

| Input | Requirement |
|---|---|
| Task / sub-task scope | MUST fit profile allowed responsibilities |
| Role archetype | MUST be allowed by profile |
| Capability class | MUST be within profile capability scope |
| Tool / skill scope | MUST be authorized for the operation class |
| Data scope | MUST satisfy data sensitivity and retention constraints |
| Policy context | MUST satisfy AEOS policy and approval rules |
| Runtime constraints | MUST fit allowed runtime class, isolation, budget and evidence requirements |
| Separation of duties | MUST avoid incompatible role/profile combinations |

If no eligible profile exists, Orchestrator MUST return `BLOCKED` or `HUMAN_APPROVAL` according to policy。It MUST NOT silently fall back to a broader profile。

## 8. Capability Dispatch

Capability dispatch selects an authorized capability class and allowed skill/tool operation for a sub-task。

Dispatch MUST verify：

- capability is required by the sub-task;
- selected Agent Profile may perform the capability;
- requested skill/tool operation is within authorized operation scope;
- data, model, memory and credential boundaries are satisfied;
- runtime class can express the required evidence and cancellation semantics;
- unknown capability, unknown schema or unsupported version fails closed。

Capability availability from runtime or tool discovery MUST NOT imply policy allowance。

## 9. Gate Runner

Gate runner is the Control Plane responsibility that evaluates or coordinates Quality Gates defined by AEOS-SPEC-002。

Gate runner MUST support at least：

| Gate Type | Requirement |
|---|---|
| Policy Gate | Verify policy, approval and authorization |
| Evidence Gate | Verify required evidence exists |
| Verification Gate | Confirm independent verification result |
| Scope Gate | Confirm output remains within task and product boundary |
| Confidence Gate | Check confidence threshold or escalate |
| Provenance Gate | Confirm source, execution, decision and memory provenance |
| Human Approval Gate | Confirm scoped human decision when required |

Gate runner MAY consume runtime evidence, but MUST NOT treat runtime success as gate pass by itself。

## 10. Gate Decision Schema

Each gate decision MUST emit a provider-neutral Gate Decision trace with at least：

| Field | Meaning |
|---|---|
| `gate_decision_id` | Stable gate decision identity |
| `trace_id` | Collaboration trace identity |
| `gate_id` | Gate identity |
| `gate_type` | Policy / Evidence / Verification / Scope / Confidence / Provenance / Human Approval |
| `state` | PASS / NEEDS_WORK / HUMAN_APPROVAL / REJECTED / BLOCKED |
| `evaluator_role` | Role archetype or human authority performing evaluation |
| `evaluated_output_ref` | Output or artifact evaluated |
| `evidence_refs` | Evidence used for the decision |
| `source_of_truth_refs` | Source of Truth references where applicable |
| `confidence` | Confidence value and rationale |
| `reason` | Human-readable decision reason |
| `next_action` | continue / return / escalate / stop / pause |
| `created_at` | Decision timestamp |

`HUMAN_APPROVAL` MUST NOT be rewritten to `PASS` by model text, runtime metadata or local harness configuration。Only an authorized human/system approval authority may satisfy a Human Approval Gate。

## 11. NEEDS_WORK Return Loop

When a gate returns `NEEDS_WORK`, Orchestrator MAY return work to an assigned role/capability only within the original or narrowed authorization scope。

Return request MUST include：

- failed gate ID and decision ID;
- corrective instruction;
- target role archetype and eligible profile constraints;
- permitted capability and tool scope;
- evidence required for re-evaluation;
- retry / budget limit;
- escalation trigger。

Repeated `NEEDS_WORK` MUST escalate when policy-defined retry, confidence, budget or quality threshold is exceeded。

## 12. Human Approval Escalation

`HUMAN_APPROVAL` is a governance state。It MUST NOT be resolved by runtime output、model confidence、provider success or tool completion alone。

Escalation request SHOULD include：

| Field | Meaning |
|---|---|
| `approval_request_id` | Stable approval request identity |
| `trace_id` | Collaboration trace identity |
| `reason_code` | Approval or escalation reason |
| `requested_decision` | What decision is needed |
| `scope` | Authorized scope if approved |
| `evidence_refs` | Evidence available to approver |
| `risk_classification` | Risk basis |
| `data_sensitivity` | Data boundary |
| `expiry_or_revalidation` | Time or condition requiring revalidation |

Approval evidence MUST be attached before affected execution continues。

## 13. Runtime Adapter Execution Request Boundary

Runtime adapter request is a Control Plane-issued or Control Plane-authorized execution request sent to a runtime/harness/adapter。

Minimum request fields SHOULD include：

| Field | Meaning |
|---|---|
| `execution_request_id` | Stable request identity |
| `trace_id` | Collaboration trace identity |
| `sub_task_id` | Target sub-task |
| `agent_profile_ref` | Authorized Agent Profile reference |
| `role_archetype_id` | Assigned role archetype |
| `capability_class` | Authorized capability |
| `tool_scope` | Allowed operation scope |
| `model_scope` | Allowed model capability class |
| `memory_data_scope` | Read/write/retain boundary |
| `credential_scope` | Credential or delegated capability boundary |
| `runtime_constraints` | Timeout, budget, retry, isolation and cancellation |
| `evidence_requirements` | Required runtime evidence |

Runtime adapter response is not defined in full by this spec；provider-neutral execution evidence is expected to be governed by the follow-up runtime evidence contract issue。

## 14. Trace Emission

The Orchestrator MUST emit or coordinate trace records aligned with AEOS-SPEC-002 Collaboration Trace Envelope。

Minimum trace categories：

| Trace | Purpose |
|---|---|
| Agent Collaboration Run | Records run identity, requester, policy context, final status and completion evidence |
| Role Assignment | Records selected role archetype, profile resolution and separation-of-duties basis |
| Capability Dispatch | Records selected capability, tool scope and policy allow/deny decision |
| Runtime Execution Request | Records authorized request boundary sent to runtime / adapter |
| Gate Decision | Records gate result and next action |
| Approval Escalation | Records human approval requirement and outcome reference |

Trace MUST minimize sensitive data and MUST NOT include secrets, raw credentials, unnecessary full prompts, chain-of-thought or unapproved raw customer content。

## 15. Conformance Checklist

A Control Plane orchestration implementation claiming alignment with this spec SHOULD demonstrate：

1. Orchestrator selects role/profile/capability based on task, risk, data scope and policy。
2. Profile resolution never silently broadens authority。
3. Capability dispatch separates availability from policy allow/deny。
4. Gate runner uses provider-neutral gate decision schema。
5. `NEEDS_WORK` returns to a bounded role/capability with retry and escalation constraints。
6. `HUMAN_APPROVAL` cannot be overwritten by model or runtime output。
7. Runtime adapter request is bounded by Execution Contract and evidence requirements。
8. Routing, gate and approval traces align with AEOS-SPEC-002 trace envelope。
9. Runtime success is not interpreted as validation pass。
10. Product repository mapping does not become Control Plane source of truth。

## 16. Status and Approval

本規格目前為 **Approved 1.0.0**。

PR #75 已合併至 `main`，merge commit 為 `515f9fb663f2f1d27e66e68165ef34c03e9a3e42`。此合併作為 Repository Owner final approval evidence，正式核准本規格為 Agent Control Plane Orchestration Extension 的 Specification。

核准後：

- Agent Control Plane 可依本規格執行 Agent Collaboration orchestration semantics。
- #69 / #70 / #71 可在本規格邊界下分別處理 runtime evidence contract、capability / tool registry adapter contract 與 provider-neutral conformance tests。
- 本規格不授權 runtime implementation、Production deployment、customer data mutation 或 YCRM downstream adoption。

## 17. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #68 | GitHub Issue | Agent Control Plane Orchestration Extension 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS owns runtime-neutral Agent Collaboration governance |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / Runtime separation |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Agent Control Plane, Execution Contract and Runtime boundary |
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile | Specification | Routing and escalation reference |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec | Specification | Collaboration model, gate result contract and trace envelope |

## 18. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 PR #75 merge evidence（515f9fb663f2f1d27e66e68165ef34c03e9a3e42）升級為 Approved Specification；正式核准 Agent Control Plane Orchestration Extension 規格，作為 #69 / #70 / #71 後續 runtime adapter 與 conformance 工作的 Control Plane 邊界依據 | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Control Plane Orchestration Extension Draft，定義 task decomposition、profile resolution、capability dispatch、gate runner、gate decision schema、NEEDS_WORK return loop、human approval escalation、runtime adapter request boundary 與 collaboration trace emission | Codex |
