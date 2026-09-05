---
doc-id: AEOS-SPEC-005
doc-name: Capability / Tool Registry Adapter Contract
doc-type: Specification
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-09-06
updated: 2026-09-06
related:
  - AEOS-ISSUE-070
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
  - AEOS-SPEC-004
---

# AEOS-SPEC-005 — Capability / Tool Registry Adapter Contract

## Executive Summary

本規格定義 runtime / provider / tool edge 如何向 AEOS 宣告可用 capability 與 tool，同時確保 policy allow / deny 仍由 AEOS Agent Control Plane 決定。

Capability discovery 只代表 availability，不代表 authorization。Tool schema、provider-specific metadata、runtime capability details 與 discovery result 不得污染 AEOS stable core semantics，也不得讓 runtime 決定 role、profile、gate、approval 或 policy。

本文件不建立 tool marketplace、不授權任何 high-risk action、不選定 runtime/provider/model/tool、不實作 adapter、不碰 Production、不推進 YCRM downstream adoption。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-005 |
| 文件名稱 | Capability / Tool Registry Adapter Contract |
| 型別 | Specification |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-06 |
| 最後更新 | 2026-09-06 |
| 依據文件 | AEOS Issue #70、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-003、AEOS-SPEC-004 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004 |

## 1. Purpose

本規格目的為：

- 定義 Capability discovery result。
- 定義 Tool input / output schema versioning。
- 定義 Policy allow / deny 與 availability 的分離。
- 定義 unknown capability / unknown schema / unsupported version fail-closed。
- 定義 tool execution trace reference。
- 定義 provider-specific metadata extension envelope。

## 2. Scope

### 2.1 In Scope

- Capability discovery result。
- Tool registry adapter response boundary。
- Tool input / output schema versioning。
- Capability availability and policy allow / deny separation。
- Unknown capability fail-closed。
- Unknown schema / unsupported version fail-closed。
- Tool execution trace reference。
- Provider / runtime / tool metadata extension envelope。

### 2.2 Out of Scope

- Tool marketplace、catalog commerce、pricing or package distribution。
- Runtime adapter implementation、tool execution implementation、SDK、API endpoint 或 database schema。
- Any high-risk action authorization。
- Runtime role/profile/gate decision authority。
- Production deployment、customer data mutation、credential provisioning or operational activation。
- CRM domain capability implementation。
- Any named runtime、provider、model、workflow engine、tool platform or vector database selection。

## 3. Governing Authority

| Authority | Role |
|---|---|
| AEOS-ADR-005 | AEOS owns runtime-neutral Agent Collaboration governance |
| AEOS-ADR-003 | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 | Execution Contract, provider adapter boundary and tool access boundary |
| AEOS-SPEC-002 | Skill / Tool scope, gate semantics and trace envelope |
| AEOS-SPEC-003 | Capability dispatch and runtime execution request boundary |
| AEOS-SPEC-004 | Runtime execution evidence and extension envelope rules |

Rules：

- Capability discovery reports availability only。
- Policy allow / deny is decided by AEOS Agent Control Plane。
- Tool execution must link to Agent Collaboration Run trace。
- Provider-specific metadata stays inside extension envelope and cannot override stable core fields。

## 4. Registry Adapter Boundary

Capability / Tool Registry Adapter is a provider-neutral discovery boundary between AEOS Control Plane and runtime / provider / tool edge。

The adapter MAY translate provider-specific discovery into AEOS stable fields, but MUST NOT：

- grant policy permission;
- assign role archetype or Agent Profile;
- decide quality gate result;
- satisfy human approval;
- authorize production, destructive, credential or customer data mutation action;
- expand capability, tool, data, memory, model or credential scope。

## 5. Capability Discovery Result

Every discovery result SHOULD include：

| Field | Meaning |
|---|---|
| `discovery_id` | Stable discovery record identity |
| `registry_adapter_id` | Adapter identity |
| `registry_adapter_version` | Adapter version |
| `runtime_id` | Runtime / harness identity or class |
| `provider_id` | Provider identity or class |
| `capability_id` | Capability identity or class |
| `capability_name` | Human-readable capability name |
| `capability_status` | available / unavailable / deprecated / unsupported / unknown |
| `tool_ids` | Tools exposed for this capability, if any |
| `schema_refs` | Input/output schema references |
| `evidence_requirements_supported` | Evidence fields runtime/tool can return |
| `risk_hints` | Non-authoritative risk hints, if provided |
| `data_handling_hints` | Non-authoritative data handling hints, if provided |
| `extension_ref` | Provider-specific extension envelope reference |
| `discovered_at` | Discovery timestamp |

`capability_status = available` MUST NOT be interpreted as policy allowed。

## 6. Tool Schema Versioning

Tool input and output schema MUST be versioned and provider-neutral at the stable core boundary。

Each tool schema reference SHOULD include：

| Field | Meaning |
|---|---|
| `tool_id` | Stable tool identity or mapped tool class |
| `tool_operation_class` | observe / read、create / write、update / mutate、delete / destructive、execute、deploy / activate、credential / permission administration |
| `input_schema_id` | Input schema identity |
| `input_schema_version` | Input schema version |
| `output_schema_id` | Output schema identity |
| `output_schema_version` | Output schema version |
| `required_evidence_fields` | Evidence required after execution |
| `forbidden_data_profile` | Forbidden data constraints |
| `extension_schema_ref` | Extension schema reference when provider-specific |

Schema evolution MUST preserve compatibility rules or require explicit migration。Unknown schema or unsupported version MUST fail closed。

## 7. Policy Allow / Deny Separation

The registry adapter reports what exists。The Agent Control Plane decides what is allowed。

Control Plane allow / deny decision SHOULD evaluate：

- task and sub-task scope;
- Agent Profile and role archetype;
- capability class;
- tool operation class;
- data sensitivity;
- approval state;
- policy context;
- runtime constraints;
- evidence requirements;
- separation-of-duties constraints;
- forbidden data profile。

Discovery result MAY inform capability dispatch, but MUST NOT bypass Control Plane policy evaluation。

## 8. Fail-Closed Rules

The following conditions MUST fail closed unless an explicit AEOS policy grants a safer fallback：

| Condition | Required Behavior |
|---|---|
| Unknown capability | Deny dispatch or return `BLOCKED` / `HUMAN_APPROVAL` |
| Unknown tool | Deny tool use |
| Unknown schema | Deny tool use until schema is known |
| Unsupported schema version | Deny or require migration |
| Missing evidence capability | Deny or require reduced scope |
| Provider metadata conflict | Prefer stable core fields and escalate |
| Ambiguous operation class | Treat as higher-risk or deny |
| Missing data handling hints | Use stricter policy classification |

Fail-closed result MUST be traceable to the Agent Collaboration Run。

## 9. Tool Execution Trace Reference

Every authorized tool execution SHOULD be linked to the Agent Collaboration Run trace。

Minimum trace reference fields：

| Field | Meaning |
|---|---|
| `trace_id` | Collaboration Trace Envelope identity |
| `execution_request_id` | Runtime execution request identity |
| `tool_execution_ref` | Stable tool execution reference |
| `tool_id` | Tool identity or mapped tool class |
| `capability_id` | Capability identity or class |
| `policy_decision_ref` | Control Plane allow / deny decision reference |
| `schema_ref` | Tool input / output schema reference |
| `evidence_ref` | Runtime execution evidence reference |
| `extension_ref` | Provider-specific extension reference when applicable |

Tool execution trace MUST NOT contain secrets、raw credentials、unnecessary full prompt、chain-of-thought、unapproved raw customer content or unnecessary sensitive payloads。

## 10. Metadata Extension Envelope

Provider-specific, runtime-specific or tool-specific metadata MAY be included only inside an extension envelope。

Extension envelope SHOULD include：

| Field | Meaning |
|---|---|
| `extension_namespace` | Provider/runtime/tool namespace |
| `extension_version` | Extension schema version |
| `extension_kind` | capability / tool / schema / provider / diagnostic |
| `metadata` | Minimized provider-specific metadata |
| `redaction_applied` | Whether forbidden or sensitive fields were removed |
| `core_mapping_ref` | Reference to stable core field mapping |

Extension metadata MUST NOT override：

- `capability_id`;
- `tool_operation_class`;
- policy allow / deny;
- approval state;
- role or profile assignment;
- gate result;
- evidence minimization;
- forbidden data rules。

## 11. Conformance Checklist

Capability / Tool Registry Adapter adoption SHOULD demonstrate：

1. Capability discovery represents availability only, not policy allowed。
2. Policy allow / deny is decided by AEOS Agent Control Plane。
3. Tool input / output schema is provider-neutral and versioned。
4. Unknown capability fails closed。
5. Unknown schema and unsupported schema version fail closed。
6. Tool execution links to Agent Collaboration Run trace。
7. Provider-specific metadata stays inside extension envelope。
8. Extension metadata cannot override stable core fields。
9. Runtime does not decide role, profile or gate result through registry metadata。
10. No high-risk action is authorized by discovery result alone。

## 12. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #70 | GitHub Issue | Capability / Tool Registry Adapter Contract 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS owns Agent Collaboration governance |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / Runtime boundary |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Provider adapter and tool access boundary |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec | Specification | Skill / Tool scope and trace envelope |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension | Specification | Capability dispatch and runtime execution request boundary |
| AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract | Specification | Runtime evidence and extension envelope boundary |

## 13. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-09-06 | 建立 Capability / Tool Registry Adapter Contract Draft，定義 capability discovery result、tool schema versioning、policy allow/deny separation、fail-closed rules、tool execution trace reference 與 metadata extension envelope | Codex |
