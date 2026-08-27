---
doc-id: AEOS-SPEC-001
doc-name: Local-first AI Agent Execution Routing Profile
doc-type: Specification
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-27
related:
  - EWO-AEOS-0046
  - AEOS-RPT-004
  - AEOS-ARCH-001
  - AEOS-ARCH-013
  - AEOS-ARCH-014
  - AEOS-ADR-003
  - AEOS-STD-005
  - AEOS-STD-007
  - YEOS-ENG-STD-008
---

# AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile

> EWO-AEOS-0046：建立 AEOS Local-first AI Agent Execution Routing Profile。本文件為 Draft Specification；定義 routing profile、tier semantics、entry / exit criteria、escalation reason、telemetry 與 validation requirements。本文件不指定任何具名 local LLM、cloud model、runtime、harness、framework 或 provider。

## Executive Summary

本規格將 AEOS-ARCH-013 已核准的 Agent Control Plane routing、budget、failure policy、approval、authorization 與 audit responsibilities 操作化為 Local-first execution profile。

Local-first execution 的目標是優先使用 deterministic rule 與 local AI 完成低風險、可驗證、低敏感度的工作，只有在 local confidence、context、capability 或 governance 條件不足時，才升級至 cloud model、frontier model 或 human approval。

Local-first 是 routing policy，不是 runtime architecture；任何具名 local model、cloud model、agent harness 或 provider 均只能出現在 Adapter / Provider / Reference Implementation / PoC 邊界，不得成為 AEOS core dependency。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-001 |
| 文件名稱 | Local-first AI Agent Execution Routing Profile |
| 型別 | Specification |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-27 |
| 最後更新 | 2026-08-27 |
| 依據文件 | EWO-AEOS-0046、AEOS-RPT-004、AEOS-ARCH-013、AEOS-ADR-003、AEOS-ARCH-014、AEOS-STD-007、YEOS ENG-STD-008 |
| 關聯文件 | AEOS-ARCH-001、AEOS-STD-005、AEOS-RPT-004 |

## 1. Purpose

本規格目的為：

- 定義 Local-first AI Agent Execution 的 tier model。
- 定義每一 tier 的 entry criteria、exit criteria、escalation criteria、governance guardrails 與 audit evidence。
- 定義 Control Plane routing inputs 與 routing decision output。
- 定義 cost / token / latency / success / escalation telemetry 的最低欄位。
- 明確保留 YEOS ENG-STD-008 的 command classification、risk classification、approval policy 與 repository protection requirements。
- 明確保留 AEOS runtime / harness / provider neutrality。

## 2. Scope

### 2.1 In Scope

- Agent Control Plane routing profile。
- Deterministic, local AI, cloud AI, frontier AI and human approval tiers。
- Execution Contract extension fields needed for local-first routing。
- Cost governance and observability telemetry minimum fields。
- Escalation reason taxonomy。
- Validation checklist for adoption by implementation repositories。

### 2.2 Out of Scope

- Any named runtime, harness, model, provider, vector database, workflow engine or tool selection。
- Production deployment topology, host sizing, network topology, KMS or credential provisioning。
- Modification of YEOS ENG-STD-008 command / risk / approval semantics。
- Product-specific CRM workflow, prompt, persona, channel behavior or domain model。
- Pricing, SKU, sales or commercial packaging decisions。

## 3. Governing Principles

| ID | Principle | Requirement |
|----|-----------|-------------|
| LFR-001 | Local-first, not local-only | Control Plane SHOULD attempt the lowest adequate tier first, but MAY escalate when evidence shows lower tiers are insufficient. |
| LFR-002 | Governance preserved | Routing MUST NOT lower approval, repository protection, authorization, data sensitivity or audit requirements. |
| LFR-003 | Runtime neutral | Tier definitions MUST NOT name or require a specific local LLM, cloud model, harness, framework or provider. |
| LFR-004 | Evidence-driven routing | Tier selection, retry and escalation MUST produce auditable evidence. |
| LFR-005 | Cost-aware execution | Routing SHOULD minimize cloud token cost and human intervention when risk and quality constraints allow. |
| LFR-006 | Fail closed on ambiguity | Unknown risk, unknown data sensitivity or missing approval MUST escalate to a safer tier or human approval. |

## 4. Execution Tiers

| Tier | Name | Authority Meaning | Typical Use |
|------|------|-------------------|-------------|
| Tier 0 | Deterministic Rule | Non-model execution under deterministic rules and policy checks | schema validation, static checks, formatting, classification, CI, rule-based routing |
| Tier 1 | Local AI | Local model / local reasoning provider under bounded Execution Contract | summarization, extraction, draft generation, preprocessing, retrieval-assisted low-risk reasoning |
| Tier 2 | Low-cost Cloud AI | Cloud model class selected for cost-effective reasoning when local is insufficient | larger context, stronger reasoning, low/medium-risk task support where data policy allows cloud |
| Tier 3 | Frontier Cloud AI | Highest capability model class for complex reasoning and high-context decision support | architecture reasoning, complex debugging, cross-repository analysis, difficult review |
| Tier 4 | Human Approval | Human decision / approval boundary | production activation, destructive operations, credentials, sensitive data, irreversible or high-risk decisions |

Tier names define routing semantics and evidence requirements. They do not define products.

## 5. Routing Inputs

Control Plane MUST evaluate the following inputs before selecting an execution tier:

| Input | Values / Shape | Requirement |
|-------|----------------|-------------|
| Risk | Repository-approved risk taxonomy, or mapped L/M/H/C when YEOS applies | MUST preserve repository approval baseline. |
| Complexity | low / medium / high / critical, or repository-defined equivalent | SHOULD consider ambiguity, required reasoning depth, cross-repository scope and context size. |
| Confidence | deterministic / high / medium / low / unknown | MUST be supported by validation evidence or explicit uncertainty. |
| Cost | local compute class, token estimate, token actual, cost estimate / actual | SHOULD prefer lower cost when governance and quality are equivalent. |
| Latency | expected / actual duration and queue profile | SHOULD satisfy user-facing and workflow constraints. |
| Data Sensitivity | public / internal / confidential / restricted / unknown, or repository-defined equivalent | MUST gate whether cloud tiers are allowed. |
| Approval State | not required / pending / approved / rejected / unknown | MUST satisfy repository policy before execution. |
| Authorization Scope | tools, model class, data scope, credential scope, environment scope | MUST be bounded by Execution Contract. |

## 6. Tier Entry and Exit Criteria

### 6.1 Tier 0 — Deterministic Rule

Entry criteria:

- Task can be completed by explicit rule, schema, static analysis, policy lookup or deterministic automation.
- No LLM reasoning is required.
- Input data sensitivity allows local deterministic processing.

Exit / escalation criteria:

- Rule conflict or missing rule.
- Unknown input type.
- Policy ambiguity.
- Deterministic validation fails and requires reasoning.

Required evidence:

- rule identifier / version;
- input class;
- output / decision;
- validation result;
- escalation reason when applicable.

### 6.2 Tier 1 — Local AI

Entry criteria:

- Task is low risk or bounded medium-risk preparation work.
- Work is verifiable by deterministic checks, review, tests or constrained output schema.
- Required data can remain within approved local boundary.
- Local model capability class is sufficient for expected complexity.

Exit / escalation criteria:

- confidence below threshold;
- repeated invalid output;
- context exceeds local profile;
- required modality or reasoning exceeds local capability;
- data policy requires a different handling path;
- command approval requirement exceeds local autonomous authority.

Required evidence:

- local provider class, not product name as architecture requirement;
- model capability class;
- prompt / context budget estimate;
- output schema validation result;
- confidence and reason;
- retry count;
- escalation reason when applicable.

### 6.3 Tier 2 — Low-cost Cloud AI

Entry criteria:

- Tier 0 / Tier 1 evidence indicates insufficient capability, context or confidence.
- Data sensitivity policy permits cloud processing.
- Task remains within allowed risk and approval scope.
- Cost profile is acceptable relative to expected value.

Exit / escalation criteria:

- cloud confidence remains insufficient;
- task requires frontier reasoning or cross-repository architecture judgment;
- policy, approval or data sensitivity blocks cloud processing;
- cost budget would be exceeded.

Required evidence:

- provider tier class;
- token estimate and actual usage when available;
- cost estimate and actual cost when available;
- data sensitivity decision;
- approval state;
- escalation reason when applicable.

### 6.4 Tier 3 — Frontier Cloud AI

Entry criteria:

- Complex architecture, debugging, cross-repository reasoning or high-context review requires highest model capability.
- Data sensitivity and approval policy permit frontier cloud use.
- Lower tiers are insufficient or not cost-effective after failed attempts.

Exit / escalation criteria:

- requested action requires human approval;
- operation is production, destructive, credential/KMS-related, sensitive data related or irreversible;
- confidence remains insufficient for autonomous execution;
- governance requires human accountability.

Required evidence:

- frontier tier justification;
- lower-tier attempts or explicit bypass reason;
- token and cost telemetry;
- result evidence;
- human handoff reason when applicable.

### 6.5 Tier 4 — Human Approval

Entry criteria:

- Repository policy requires Human Review, Human Approval, or Repository Owner authorization.
- Operation involves production deploy / activation, destructive action, credential / KMS, sensitive data, irreversible change or high-risk business decision.
- Risk, data sensitivity or approval state is unknown.

Exit criteria:

- Human approves with bounded authorization scope;
- Human rejects;
- Human requests changes;
- Human delegates a lower-risk bounded execution under a new or amended Execution Contract.

Required evidence:

- approver identity or review reference;
- decision timestamp;
- approval policy reference;
- authorized scope;
- decision result;
- follow-up execution contract when applicable.

## 7. Escalation Reason Taxonomy

| Code | Reason | Description |
|------|--------|-------------|
| ESC-RULE-UNKNOWN | Missing deterministic rule | Tier 0 cannot classify or complete the task. |
| ESC-CONFIDENCE-LOW | Confidence below threshold | Output confidence is too low for the requested action. |
| ESC-CONTEXT-LIMIT | Context limit exceeded | Lower tier cannot handle required context. |
| ESC-CAPABILITY-GAP | Capability gap | Required reasoning, modality or tool capability is unavailable. |
| ESC-VALIDATION-FAIL | Validation failed | Output failed schema, tests, policy checks or review checks. |
| ESC-DATA-SENSITIVITY | Data sensitivity gate | Data policy blocks the current tier. |
| ESC-COST-BUDGET | Cost budget gate | Current or next tier would exceed budget. |
| ESC-APPROVAL-REQUIRED | Approval required | Repository policy requires human review / approval. |
| ESC-PROTECTED-OP | Protected operation | Operation touches protected repository or production boundary. |
| ESC-UNKNOWN-RISK | Unknown risk | Risk cannot be classified safely. |

## 8. Execution Contract Extension

A local-first Execution Contract SHOULD include the following additional fields:

| Field | Required | Purpose |
|-------|----------|---------|
| `routing_profile_id` | Yes | Identifies this routing profile version. |
| `initial_tier` | Yes | Tier selected for first attempt. |
| `current_tier` | Yes | Tier used for current execution attempt. |
| `max_allowed_tier` | Yes | Highest tier allowed by policy and approval. |
| `risk_classification` | Yes | Repository-approved risk classification or mapping. |
| `complexity_classification` | Yes | Complexity used for routing. |
| `data_sensitivity_classification` | Yes | Data boundary decision. |
| `confidence_threshold` | Yes | Minimum confidence required for autonomous completion. |
| `cost_budget` | Yes | Budget for token / provider / runtime consumption. |
| `latency_budget` | No | Time or queue target. |
| `approval_state_ref` | Yes | Approval evidence or policy reference. |
| `escalation_policy_ref` | Yes | Policy governing retry and escalation. |

## 9. Telemetry Minimum Fields

Every execution attempt SHOULD emit an audit / observability event with at least:

| Field | Purpose |
|-------|---------|
| `execution_id` | Correlates all attempts for one execution. |
| `task_id` | Correlates task or work order. |
| `repository` | Source repository or implementation repository. |
| `routing_profile_id` | Routing profile version. |
| `tier` | Tier used for this attempt. |
| `provider_class` | deterministic / local / low-cost-cloud / frontier-cloud / human. |
| `model_capability_class` | Capability class without requiring product-specific model identity. |
| `risk_classification` | Risk basis. |
| `complexity_classification` | Complexity basis. |
| `data_sensitivity_classification` | Data boundary basis. |
| `approval_state` | Approval state at execution time. |
| `token_estimate` | Expected token usage where applicable. |
| `token_actual` | Actual token usage where available. |
| `cost_estimate` | Expected cost where applicable. |
| `cost_actual` | Actual cost where available. |
| `latency_actual` | Observed execution latency. |
| `retry_count` | Attempt count. |
| `validation_status` | passed / failed / skipped / not-applicable. |
| `confidence` | deterministic / high / medium / low / unknown. |
| `escalation_reason` | One of §7 when escalated. |
| `final_status` | completed / escalated / rejected / failed / cancelled. |

## 10. Cost Governance Metrics

Repositories adopting this profile SHOULD aggregate at least:

| Metric | Description |
|--------|-------------|
| Local completion ratio | Percentage of executions completed at Tier 0 or Tier 1. |
| Cloud escalation ratio | Percentage escalated to Tier 2 or Tier 3. |
| Frontier usage ratio | Percentage requiring Tier 3. |
| Human approval ratio | Percentage requiring Tier 4. |
| Token consumption by tier | Estimated and actual token usage grouped by tier. |
| Cost by tier | Estimated and actual provider cost grouped by tier. |
| Retry rate | Attempts per completed execution. |
| Escalation reason distribution | Frequency of §7 reason codes. |
| Validation failure rate | Failed validation grouped by tier and task type. |
| Approval-blocked rate | Executions blocked by missing or rejected approval. |

## 11. YEOS Approval Mapping

When a repository adopts YEOS ENG-STD-008 or an equivalent policy, routing MUST preserve the following minimum behavior:

| YEOS Risk | Default Routing Constraint |
|-----------|----------------------------|
| L | Tier 0 or Tier 1 MAY execute autonomously when otherwise permitted and auditable. |
| M | Human Review is required before execution that mutates local state or prepares repository-impacting changes. |
| H | Human Approval is required before repository-level state change. |
| C | Human Approval plus Repository Owner authorization is required; unclassified command is treated as C until classified. |

This mapping does not replace YEOS ENG-STD-008. If there is conflict, the repository-approved policy prevails and MAY impose stricter controls.

## 12. Runtime Neutrality Requirements

Implementations MUST:

- expose local AI, cloud AI, tool, memory and workflow engines through provider / adapter boundaries;
- avoid hard-coding any named runtime, harness, local model, cloud model or framework into AEOS core;
- describe provider-specific configuration as implementation or reference implementation detail;
- preserve the Execution Contract semantics across provider substitution;
- keep model identity separate from agent identity, policy authority and approval authority.

## 13. Adoption Validation Checklist

An implementation repository adopting this profile SHOULD demonstrate:

| Check | Requirement |
|-------|-------------|
| Tier coverage | Tier 0-Tier 4 handling is documented or intentionally not supported with reason. |
| Non-bypass | Approval and repository protection rules cannot be lowered by local execution. |
| Evidence | Every execution attempt emits minimum telemetry or equivalent audit record. |
| Escalation | Escalation reasons use a stable taxonomy. |
| Cost metrics | Local/cloud/frontier/human ratios and token/cost usage can be reported. |
| Adapter boundary | Provider-specific code is isolated from architecture / policy core. |
| Data sensitivity | Cloud routing is blocked or approved based on data classification. |
| Human handoff | Tier 4 produces decision evidence and bounded follow-up authorization. |

## 14. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS-ARCH-001 — Architecture Baseline | Architecture | Architecture entry and register authority |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Control Plane, Execution Plane, Runtime Neutrality and Execution Contract authority |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Approved decision for Control Plane / Runtime separation |
| AEOS-ARCH-014 — Productizable Platform Architecture | Architecture | Reference implementation and Platform Core boundary |
| AEOS-STD-005 — Review Standard | Standard | Review workflow and decision requirements |
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard | Standard | Context, token budget and model routing baseline |
| AEOS-RPT-004 — Local-first AI Agent Execution Architecture Gap Analysis | Report | Baseline, gap analysis and implementation plan |
| YEOS ENG-STD-008 — AI Agent Command Approval Standard | External Standard | Command classification, risk classification and approval policy |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-27 | Initial Draft local-first routing profile specification for EWO-AEOS-0046 | Codex |
