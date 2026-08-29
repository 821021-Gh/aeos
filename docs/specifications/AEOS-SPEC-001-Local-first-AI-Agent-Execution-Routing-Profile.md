---
doc-id: AEOS-SPEC-001
doc-name: Local-first AI Agent Execution Routing Profile
doc-type: Specification
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-29
updated: 2026-08-29
related:
  - EWO-AEOS-0046
  - AEOS-RPT-004
  - AEOS-ARCH-013
  - AEOS-ADR-003
  - AEOS-ARCH-014
  - AEOS-STD-007
  - YEOS-ENG-STD-008
---

# AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile

> This Draft Specification operationalizes the local-first routing gap identified in AEOS-RPT-004. It defines governance semantics for routing AI Agent work across deterministic, local, cloud, frontier and human tiers. It does not select a model, runtime, harness, provider, tool, memory product or deployment topology.

## Executive Summary

AEOS-ARCH-013 and AEOS-ADR-003 already establish the Agent Control Plane, Agent Execution Plane, Execution Contract, Provider Adapter Boundary and runtime / harness / provider neutrality. This specification adds a local-first routing profile that the Control Plane MAY use when admitting and routing AI Agent work.

The routing order is:

`Deterministic Rule -> Local AI -> Low-cost Cloud AI -> Frontier Cloud AI -> Human Approval`

Local-first routing is a cost, latency and containment strategy. It is not an approval shortcut. A lower execution tier MUST NOT reduce YEOS ENG-STD-008 command classification, risk classification, human review, human approval, repository owner authorization, branch protection, review gate or audit requirements.

## 1. Purpose

This specification exists to:

- define Tier 0 through Tier 4 local-first AI Agent execution routing semantics;
- define minimum routing inputs, entry criteria, exit criteria and escalation reasons;
- define telemetry required to prove local/cloud/frontier/human usage ratios, token usage, cost, retries, success and escalation behavior;
- preserve AEOS runtime, harness, model and provider neutrality;
- preserve YEOS ENG-STD-008 approval and repository protection boundaries;
- provide a reviewable AEOS source for YEOS and production repository adoption reports.

## 2. Scope

### 2.1 In Scope

- Control Plane routing semantics for AI Agent execution.
- Execution tier entry, exit and escalation criteria.
- Routing inputs: Risk, Complexity, Confidence, Cost, Latency and Data Sensitivity.
- Execution Contract fields required to carry routing and audit evidence.
- Telemetry fields for tier usage, token/cost accounting, retry, success and escalation analysis.
- Compatibility requirements for YEOS governance adoption.

### 2.2 Out of Scope

- Named local LLM, cloud LLM, frontier LLM, agent framework, runtime, harness, vector store, tool provider or memory provider selection.
- Production deployment topology, host sizing, infrastructure, credential provisioning or KMS policy.
- Modification of YEOS ENG-STD-008 command approval policy.
- Product-specific implementation in yeelight-ai-crm or any other production repository.
- Autonomous production activation, protected branch mutation, destructive operation or credential access.

## 3. Architecture Authority

This specification is subordinate to:

| Authority | Role |
|-----------|------|
| AEOS-ARCH-013 | Defines Enterprise AI Agent Architecture, Control Plane and Execution Plane boundaries |
| AEOS-ADR-003 | Defines Control Plane / Runtime separation and neutrality decision |
| AEOS-ARCH-014 | Defines productizable platform and reference implementation boundaries |
| AEOS-STD-007 | Defines AI Engineering context, token budget and model-routing principles |
| YEOS ENG-STD-008 | Defines command classification, risk classification, approval and repository protection boundaries |

If this specification conflicts with an Approved Architecture, ADR, Standard or YEOS approval rule, the approved governance source prevails until this specification is revised and approved.

## 4. Routing Model

The Agent Control Plane SHOULD evaluate the minimum viable execution tier first and escalate only when evidence shows that the current tier is insufficient, disallowed or approval-bound.

| Tier | Name | Primary Role | Typical Entry Criteria | Exit / Escalation Criteria | Mandatory Boundary |
|------|------|--------------|------------------------|----------------------------|--------------------|
| Tier 0 | Deterministic Rule | Exact validation, schema checks, static policy lookup, formatting, classification, CI-style checks | Rule exists; input is structured enough; no model reasoning is required | Rule conflict, unknown input, policy ambiguity, failed validation or incomplete evidence | Deterministic automation cannot approve M/H/C risk or protected operations |
| Tier 1 | Local AI | Low-risk reasoning, summarization, extraction, classification, draft generation and verifiable preprocessing | Data sensitivity permits local processing; expected output can be validated; task is low/medium complexity; no cloud capability is required | Low confidence, context/capability limit, repeated failure, unavailable local runtime, higher risk impact or unsupported modality | Local AI cannot self-approve, bypass review, expand credentials or weaken repository protection |
| Tier 2 | Low-cost Cloud AI | Cost-aware cloud reasoning when local execution is insufficient | Cloud processing is allowed by data sensitivity; cost budget is available; task does not require frontier capability; governance scope is intact | Confidence remains insufficient, cost cap reached, policy denial, high complexity, high ambiguity or approval-bound action | Cloud tier cannot process disallowed sensitive data or lower approval requirements |
| Tier 3 | Frontier Cloud AI | Complex architecture reasoning, high-context review, difficult debugging and cross-repository decision support | Stronger reasoning/context is justified; budget and data policy allow it; previous tiers are insufficient or inefficient | Human decision required, protected operation, production activation, destructive action, credential access, unresolved uncertainty | Frontier AI is decision support, not final approval authority |
| Tier 4 | Human Approval | Final authorization, review and risk acceptance | Required by command/risk classification, production impact, credential/sensitive data, irreversible operation or policy uncertainty | Human approves, rejects, asks for changes or delegates bounded execution | Human decision is the final approval source |

## 5. Routing Inputs

Every routing decision SHOULD record the following inputs:

| Input | Description | Required Treatment |
|-------|-------------|--------------------|
| Risk | Expected impact if the action is wrong or unauthorized | MUST map to the active repository risk policy; unknown risk fails closed |
| Complexity | Reasoning depth, ambiguity, cross-repository scope, context size and failure mode count | SHOULD select the lowest tier that can produce verifiable output |
| Confidence | Evidence that the selected tier can complete correctly | MUST include validation basis or reason for escalation |
| Cost | Estimated local compute, cloud tokens, frontier usage, retries and budget impact | MUST be measured before claiming cost reduction |
| Latency | User-facing responsiveness, queue delay and timeout constraints | MAY justify escalation when lower tiers are too slow or unavailable |
| Data Sensitivity | Secret, credential, customer, production, proprietary or regulated data classification | MUST gate whether cloud tiers are allowed at all |

Additional inputs MAY include artifact type, repository protection state, approval state, runtime availability, validation availability and prior retry history.

## 6. Decision Rules

The Control Plane SHOULD apply these routing rules:

1. Prefer Tier 0 when a deterministic rule can produce sufficient evidence.
2. Prefer Tier 1 when local AI can complete a low-risk, verifiable task within data, cost and latency constraints.
3. Escalate to Tier 2 only when local execution is insufficient and cloud processing is permitted.
4. Escalate to Tier 3 only when stronger reasoning or context is justified and budget/data policy allows it.
5. Route to Tier 4 whenever command classification, risk classification, production impact, repository protection, credential access, destructive operation, sensitive data or unresolved uncertainty requires human authority.
6. Fail closed when command classification, risk classification, approval state or data sensitivity is unknown.
7. Never allow tier selection to broaden tool, model, memory, data or credential scope beyond the Execution Contract.

## 7. Non-bypass Requirements

Local-first execution MUST preserve governance.

A Tier 0, Tier 1, Tier 2 or Tier 3 execution MUST NOT:

- bypass YEOS ENG-STD-008 command classification, risk classification, human review or human approval;
- bypass repository protection, branch protection, required status checks or review gates;
- self-approve a pull request, production activation, protected operation, credential access or destructive action;
- treat a model recommendation as approval evidence;
- expand tool, data, memory, credential or repository scope beyond the Execution Contract;
- use runtime-local configuration as Enterprise Architecture authority;
- convert a provider-specific adapter into a mandatory AEOS architecture dependency.

## 8. Execution Contract Additions

A local-first routing Execution Contract SHOULD include the following fields in addition to AEOS-ADR-003 required contract fields:

| Field | Purpose |
|-------|---------|
| routing_profile_id | Identifies this routing profile and version |
| selected_tier | Current execution tier |
| max_allowed_tier | Highest tier allowed without additional approval |
| candidate_tiers | Tiers considered before selection |
| routing_inputs | Structured Risk, Complexity, Confidence, Cost, Latency and Data Sensitivity inputs |
| routing_reason | Human-readable reason for selecting the tier |
| escalation_policy_ref | Reference to escalation rules or policy source |
| approval_state_ref | Reference to existing human/system approval evidence |
| data_sensitivity_decision | Why the selected tier may handle the data |
| budget_ref | Cost/token/latency budget reference |
| validation_plan | How the output will be verified |
| audit_trace_id | Correlation identity across Control Plane, Harness, Runtime and Adapter evidence |

Provider-specific or runtime-specific fields MAY appear as adapter extensions, but MUST NOT change governance semantics.

## 9. Telemetry Schema

Implementations adopting this profile SHOULD record one routing event per execution attempt or meaningful escalation step.

| Category | Fields |
|----------|--------|
| Identity | routing_event_id, timestamp, repository, work_item_id, execution_id, agent_id, actor |
| Request | requested_action, artifact_type, repository_scope, protected_operation_flag |
| Inputs | risk_classification, complexity_score, confidence_score, cost_budget_ref, latency_target_ms, data_sensitivity_classification |
| Routing | routing_profile_id, selected_tier, previous_tier, next_tier, candidate_tiers, routing_reason, escalation_reason |
| Governance | command_classification, approval_required, approval_state_ref, authorization_scope, policy_refs |
| Cost | token_input_estimate, token_output_estimate, token_input_actual, token_output_actual, estimated_cost_usd, actual_cost_usd |
| Quality | validation_status, validation_ref, retry_count, failure_reason, confidence_basis |
| Runtime Neutrality | runtime_adapter_id, provider_family, capability_class, model_capability_class |
| Outcome | success, final_status, produced_artifacts, audit_trace_id, review_ref |

Dashboard-level metrics SHOULD include local completion ratio, cloud escalation ratio, frontier usage ratio, human approval ratio, token/cost by tier, retry rate, validation failure rate, escalation reason distribution and approval-blocked rate.

## 10. Escalation Reason Taxonomy

Implementations SHOULD use a neutral escalation reason taxonomy:

| Reason | Meaning |
|--------|---------|
| deterministic_rule_missing | No applicable deterministic rule exists |
| deterministic_conflict | Deterministic rules disagree or are ambiguous |
| local_capability_limit | Local tier lacks required capability, context or modality |
| local_confidence_low | Local output confidence or validation evidence is insufficient |
| validation_failed | Output failed required validation |
| retry_exhausted | Retry policy has been exhausted |
| cloud_disallowed | Data or policy does not permit cloud processing |
| cost_limit_reached | Budget or cost threshold has been reached |
| latency_limit_reached | Lower tier cannot meet latency requirement |
| high_complexity | Task requires stronger reasoning or cross-repository context |
| approval_required | Human review, approval or owner authorization is required |
| protected_operation | Branch, repository or production protection applies |
| sensitive_data | Sensitive data requires stricter handling or human decision |
| policy_uncertain | Applicable policy is missing, conflicting or unclear |

## 11. YEOS Compatibility

YEOS adoption MUST treat this AEOS specification as a routing profile, not as a new permission model.

Required compatibility rules:

- YEOS ENG-STD-008 remains authoritative for command classification, risk classification and approval policy.
- Local-first routing cannot downgrade Human Review, Human Approval, Repository Owner authorization or repository protection.
- Routing tier and risk classification are separate dimensions.
- Unclassified or uncertain commands must fail closed and follow YEOS unclassified-command treatment.
- Human approval evidence must be referenced as a governance artifact, not inferred from AI output.

## 12. Repository Adoption Requirements

A production repository SHOULD adopt this profile only after it has:

1. a repository-specific adoption report or specification;
2. a declared routing profile version;
3. command and risk classification mapping;
4. data sensitivity rules for local/cloud/frontier execution;
5. telemetry storage and review plan;
6. validation gates for Tier 0 and Tier 1 outputs;
7. explicit protected-operation and human-approval boundaries;
8. a rollback or stop condition for failed validation and low confidence.

Initial adoption SHOULD be telemetry-only or limited to low-risk, verifiable Tier 0/Tier 1 work. It MUST NOT begin with production deployment, credential access, protected branch mutation, destructive operation or irreversible business decision automation.

## 13. Validation

This Draft specification is ready for AEOS review when the following are true:

| Acceptance Criteria | Status |
|---------------------|--------|
| Tier 0-4 entry, exit, escalation, governance and audit requirements are defined | Met |
| Routing inputs include Risk, Complexity, Confidence, Cost, Latency and Data Sensitivity | Met |
| Telemetry covers local/cloud/frontier/human ratios, tokens, cost, retries, success and escalation reason | Met |
| YEOS ENG-STD-008 approval and repository protection are preserved | Met |
| Runtime, model, harness and provider neutrality are preserved | Met |
| No product-specific implementation is introduced | Met |

Repository validation SHOULD verify metadata, naming, cross-reference integrity and Markdown structure before merge.

## 14. References

| Document | Type | Usage |
|----------|------|-------|
| AEOS-RPT-004 | Report | Gap analysis that identified this specification as the next AEOS artifact |
| AEOS-ARCH-013 | Architecture | Agent Control Plane and Execution Plane architecture authority |
| AEOS-ADR-003 | ADR | Control Plane / Runtime separation and neutrality decision |
| AEOS-ARCH-014 | Architecture | Productization and reference implementation boundary |
| AEOS-STD-007 | Standard | AI Engineering context, token budget and model-routing principles |
| YEOS ENG-STD-008 | Standard | Command, risk, approval and repository protection authority |
| YEOS AIG-M7-T003 | Report | YEOS adoption mapping for local-first routing |

## 15. Revision History

| Version | Date | Summary | Author |
|---------|------|---------|--------|
| 0.1.0 | 2026-08-29 | Initial Draft local-first AI Agent execution routing profile | Codex |
