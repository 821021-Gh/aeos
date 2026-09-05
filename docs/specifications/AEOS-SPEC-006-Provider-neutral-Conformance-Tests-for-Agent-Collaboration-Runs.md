---
doc-id: AEOS-SPEC-006
doc-name: Provider-neutral Conformance Tests for Agent Collaboration Runs
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-06
updated: 2026-09-06
related:
  - AEOS-ISSUE-071
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
  - AEOS-SPEC-004
  - AEOS-SPEC-005
---

# AEOS-SPEC-006 — Provider-neutral Conformance Tests for Agent Collaboration Runs

## Executive Summary

本規格定義 AEOS Agent Collaboration Runs 的 provider-neutral conformance test requirements。其目的不是建立可執行測試程式，而是定義任何 runtime / provider adapter 若宣稱符合 AEOS Agent Collaboration governance，至少必須通過哪些 fixture、invariant 與 evidence minimization 檢查。

同一 AEOS collaboration plan 在不同 runtime adapter 下，MUST preserve gate semantics、validation semantics、role/profile authority、memory promotion boundary、approval boundary、evidence minimization、extension envelope isolation 與 fail-closed behavior。

Runtime、provider、model、harness、tool 或 workflow engine 均為 replaceable edge。更換 edge MUST NOT 改變 AEOS role、truth、memory promotion、approval 或 quality gate semantics。

本文件不要求所有 runtime 功能等價、不測真實 Production action、不使用真實 customer data 或 secrets、不實作測試程式、不推進 YCRM downstream adoption。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-006 |
| 文件名稱 | Provider-neutral Conformance Tests for Agent Collaboration Runs |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-06 |
| 最後更新 | 2026-09-06 |
| 依據文件 | AEOS Issue #71、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004、AEOS-SPEC-005 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004、AEOS-SPEC-005 |

## 1. Purpose

本規格目的為：

- 定義 Agent Collaboration Run conformance fixture。
- 定義 runtime success / failure classification invariants。
- 定義 gate result invariants。
- 定義 validation semantics invariants。
- 定義 evidence minimization and forbidden-data conformance checks。
- 定義 provider-specific metadata extension envelope checks。
- 定義 unknown schema / version / capability fail-closed checks。

## 2. Scope

### 2.1 In Scope

- Provider-neutral conformance fixture requirements。
- Runtime adapter behavior classification。
- Gate result invariants across runtime adapters。
- Validation semantics and Source of Truth verification boundary。
- Evidence minimization and forbidden data tests。
- Provider-specific metadata extension envelope checks。
- Unknown schema、version、capability fail-closed cases。
- Replaceable edge assertion for runtime/provider adapters。

### 2.2 Out of Scope

- Executable test code、test framework selection、CI configuration or automation implementation。
- Requiring all runtimes to have equivalent capability。
- Testing real Production action。
- Using real customer data、secrets、credential material or protected operational payload。
- CRM domain capability implementation。
- Any named runtime、provider、model、harness、workflow engine、tool platform or vector database selection。
- YCRM downstream adoption。

## 3. Governing Authority

| Authority | Role |
|---|---|
| AEOS-ADR-005 | AEOS owns runtime-neutral Agent Collaboration governance |
| AEOS-ADR-003 | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 | Runtime/harness/provider neutral execution boundary |
| AEOS-SPEC-002 | Collaboration model, gate result contract and trace envelope |
| AEOS-SPEC-003 | Control Plane orchestration, Gate runner and runtime request boundary |
| AEOS-SPEC-004 | Runtime execution evidence and forbidden data rules |
| AEOS-SPEC-005 | Capability/tool registry, schema versioning and fail-closed rules |

Rules：

- Runtime / provider adapter is a replaceable edge。
- Runtime success is not validation pass。
- Model text output cannot directly change quality gate status。
- Registry discovery does not grant authorization。
- Forbidden data MUST NOT be emitted in evidence or trace。

## 4. Conformance Fixture

Each conformance run SHOULD use a synthetic Agent Collaboration Run fixture that contains no real customer data, no secrets and no Production action。

Minimum fixture components：

| Component | Requirement |
|---|---|
| Collaboration Plan | Stable plan with parent task, sub-tasks, role archetypes and capability classes |
| Execution Contract | Authorized request scope, runtime constraints and evidence requirements |
| Source of Truth Stub | Synthetic approved facts for verification |
| Tool Registry Fixture | Synthetic capability/tool discovery results and schema versions |
| Runtime Adapter Profiles | At least two provider-neutral adapter profiles or simulated profiles |
| Expected Gate Results | Expected PASS / NEEDS_WORK / HUMAN_APPROVAL / REJECTED / BLOCKED states |
| Forbidden Data Sample | Synthetic markers representing secrets, raw customer content and chain-of-thought |
| Extension Envelope Sample | Provider-specific metadata sample that must not override stable core fields |

The fixture MUST be deterministic enough to compare semantics across runtime adapters。

## 5. Runtime Success / Failure Classification

Runtime outcome classification MUST follow AEOS-SPEC-004。

Conformance checks SHOULD include：

| Case | Expected Result |
|---|---|
| Runtime completed authorized request | Execution evidence outcome may be `completed`; gate still requires Gate runner evaluation |
| Runtime failed request | Gate runner MUST NOT interpret failure as validation pass |
| Runtime timed out | Gate runner returns `NEEDS_WORK`, `BLOCKED` or escalation according to policy |
| Runtime rejected unsupported contract | Control Plane treats as unsupported / blocked, not as successful execution |
| Runtime returns incomplete evidence | Gate runner returns `NEEDS_WORK`, `HUMAN_APPROVAL`, `REJECTED` or `BLOCKED` according to policy |

Runtime failure MUST NOT be automatically downgraded into successful validation。

## 6. Gate Result Invariants

The same collaboration plan SHOULD produce consistent gate decision semantics across runtime adapters when given equivalent authorized inputs and evidence。

Conformance MUST verify：

- `PASS` requires gate evidence, not merely model text。
- `NEEDS_WORK` returns to bounded role/capability with retry constraints。
- `HUMAN_APPROVAL` cannot be overwritten by runtime output、model confidence or provider metadata。
- `REJECTED` stops affected path and records rejection evidence。
- `BLOCKED` records missing dependency, authority, schema, capability, approval or Source of Truth。
- Gate runner remains the authority for gate state under AEOS-SPEC-003。

Model text output MUST NOT directly change quality gate status。

## 7. Validation Semantics Invariants

Validation semantics MUST remain independent from runtime success。

Conformance MUST verify：

- Source of Truth verification uses approved source references。
- Missing Source of Truth produces `BLOCKED`, `HUMAN_APPROVAL` or scoped uncertainty outcome。
- Runtime output is evidence input, not truth authority。
- Memory promotion requires policy-compliant provenance and approval。
- Human approval requires approval evidence from authorized authority。
- Learning case or authoritative knowledge publication cannot be produced by runtime evidence alone。

## 8. Evidence Minimization and Forbidden Data Tests

Evidence minimization tests MUST verify that runtime evidence and trace output exclude forbidden data under AEOS-SPEC-004。

Forbidden data checks SHOULD include：

| Forbidden Data | Expected Behavior |
|---|---|
| Secret marker | Redacted, omitted, referenced safely, or rejected |
| Raw credential marker | Not emitted |
| Complete prompt marker | Not emitted unless policy explicitly permits |
| Chain-of-thought marker | Not emitted |
| Raw customer content marker | Not emitted unless explicitly approved and minimized |
| Protected tool payload marker | Replaced by safe reference when possible |

If evidence cannot be produced without forbidden data, adapter MUST return a failed or rejected evidence state rather than leak data。

## 9. Extension Envelope Checks

Provider-specific metadata MUST remain inside extension envelope under AEOS-SPEC-004 and AEOS-SPEC-005。

Conformance MUST verify extension metadata cannot override：

- role archetype;
- Agent Profile;
- policy allow / deny;
- approval state;
- capability ID;
- tool operation class;
- gate result;
- evidence minimization;
- forbidden data rules。

Conflicting extension metadata SHOULD cause escalation or fail-closed behavior。

## 10. Unknown Schema / Version / Capability Fail-Closed Cases

Conformance tests MUST include fail-closed cases for：

| Unknown / Unsupported Case | Expected Behavior |
|---|---|
| Unknown capability | Deny dispatch or return `BLOCKED` / `HUMAN_APPROVAL` |
| Unknown tool | Deny tool use |
| Unknown input schema | Deny tool use until schema is known |
| Unknown output schema | Deny acceptance of tool result |
| Unsupported schema version | Deny or require migration |
| Unsupported contract version | Runtime rejects or Control Plane blocks |
| Missing evidence support | Deny, reduce scope or return `NEEDS_WORK` / `BLOCKED` |

Unknown schema / version / capability MUST fail closed。

## 11. Replaceable Edge Invariant

Runtime / provider adapter MUST remain a replaceable edge。

Conformance MUST verify that replacing runtime/provider adapter does not change：

- role archetype assignment semantics;
- Agent Profile authority;
- policy allow / deny meaning;
- Source of Truth validation boundary;
- memory promotion boundary;
- human approval boundary;
- quality gate state semantics;
- evidence minimization and forbidden data rules;
- extension envelope isolation;
- trace identity and correlation semantics。

Capability mismatch MAY produce `BLOCKED` or `HUMAN_APPROVAL`; it MUST NOT change AEOS governance semantics。

## 12. Conformance Result Record

Each conformance run SHOULD produce a provider-neutral conformance result record。

Minimum fields：

| Field | Meaning |
|---|---|
| `conformance_run_id` | Stable conformance run identity |
| `fixture_id` | Fixture identity |
| `collaboration_plan_id` | Collaboration plan identity |
| `runtime_adapter_profile_id` | Runtime / provider adapter profile under test |
| `contract_version` | Execution Contract or equivalent version |
| `spec_refs` | AEOS-SPEC-002/003/004/005/006 references |
| `case_results` | Pass/fail/blocked results by case |
| `gate_result_comparison` | Expected vs actual gate semantics |
| `forbidden_data_result` | Evidence minimization result |
| `extension_envelope_result` | Extension isolation result |
| `fail_closed_result` | Unknown schema/version/capability result |
| `final_status` | conformant / non-conformant / blocked / not-applicable |

## 13. Conformance Checklist

An implementation or adapter claiming conformance SHOULD demonstrate：

1. Same collaboration plan produces consistent gate decision semantics across runtime adapters。
2. Runtime failure is not automatically downgraded into successful validation。
3. Model text output cannot directly change quality gate status。
4. Evidence minimization and forbidden-data checks pass。
5. Runtime/provider adapter remains a replaceable edge。
6. Unknown schema, version and capability fail closed。
7. Provider-specific metadata remains inside extension envelope。
8. Extension metadata cannot override stable core governance fields。
9. Source of Truth validation boundary remains outside runtime evidence。
10. No real Production action, customer data or secrets are required for conformance。

## 14. Status and Approval

本規格目前為 **Approved 1.0.0**。

PR #81 已合併至 main，merge commit 為 `d11da71ccae27e68bed6de7875c2bc9671be2f47`。此合併作為 Repository Owner final approval evidence，正式核准本規格為 Provider-neutral Conformance Tests for Agent Collaboration Runs。

本次核准確認以下治理邊界：

- 同一 AEOS collaboration plan 在不同 runtime adapter 下必須維持一致 gate semantics 與 validation semantics。
- Runtime failure 不可自動降級為 successful validation。
- Model text output 不可直接改變 quality gate status。
- Evidence minimization and forbidden-data checks are required。
- Runtime/provider adapter remains replaceable edge。
- Unknown schema、version、capability fail closed。
- 本規格不授權 executable test implementation、production actions、real customer data/secrets 或 YCRM downstream adoption。

## 15. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #71 | GitHub Issue | Provider-neutral Conformance Tests for Agent Collaboration Runs 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS owns Agent Collaboration governance |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / Runtime boundary |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Runtime / harness / provider neutral execution boundary |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec | Specification | Gate result contract and trace envelope |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension | Specification | Gate runner and runtime execution request boundary |
| AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract | Specification | Runtime evidence and forbidden data rules |
| AEOS-SPEC-005 — Capability / Tool Registry Adapter Contract | Specification | Capability discovery, schema versioning and fail-closed rules |

## 16. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-06 | 依 PR #81 merge evidence（`d11da71ccae27e68bed6de7875c2bc9671be2f47`）升級為 Approved Specification；正式核准 Provider-neutral Conformance Tests for Agent Collaboration Runs，作為 AEOS Agent Collaboration runtime/provider replaceable edge conformance boundary | Codex |
| 0.1.0 | 2026-09-06 | 建立 Provider-neutral Conformance Tests for Agent Collaboration Runs Draft，定義 fixture requirements、runtime success/failure classification、gate result invariants、validation semantics、evidence minimization、extension envelope checks、fail-closed cases 與 replaceable edge invariant | Codex |
