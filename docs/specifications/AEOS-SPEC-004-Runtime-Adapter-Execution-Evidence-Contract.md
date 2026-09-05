---
doc-id: AEOS-SPEC-004
doc-name: Runtime Adapter Execution Evidence Contract
doc-type: Specification
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-069
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
---

# AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract

## Executive Summary

本規格定義 Agent Runtime adapter 在完成 AEOS execution request 後必須回傳的 provider-neutral execution evidence contract。

Runtime adapter execution evidence 的目的，是讓 Agent Control Plane、Gate runner、Audit / Observability boundary 與後續 verification 能判斷 runtime 做了什麼、用了哪些受授權能力、結果狀態為何、是否有錯誤、成本與 latency 為何，以及是否符合 evidence minimization。它不是 source validation、memory confirmation、learning case publication、human approval、production authorization 或 quality gate pass。

本文件不選定任何 runtime、harness、provider、model、tool platform、workflow engine 或 product repository；不實作 adapter；不定義 CRM domain capability；不授權 Production action。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-004 |
| 文件名稱 | Runtime Adapter Execution Evidence Contract |
| 型別 | Specification |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #69、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-003 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003 |

## 1. Purpose

本規格目的為：

- 定義 Runtime adapter execution request / response boundary。
- 定義 provider-neutral execution evidence minimum fields。
- 定義 cost、token、latency、tool reference 等 optional evidence fields。
- 定義 forbidden data rules。
- 定義 provider / model / runtime metadata extension envelope。
- 防止 runtime success 被 Control Plane 或 Gate runner 誤解為 validation pass、authority decision 或 approval result。

## 2. Scope

### 2.1 In Scope

- Runtime adapter execution response boundary。
- Execution evidence minimum fields。
- Runtime、adapter、capability、provider/model、execution identity。
- Start / end time、duration、outcome、error class。
- Optional cost、token、latency、tool reference。
- Forbidden data rules。
- Provider/model/runtime metadata extension envelope。
- Runtime evidence interpretation rules。

### 2.2 Out of Scope

- Runtime adapter implementation。
- API endpoint、SDK、database schema 或 telemetry pipeline implementation。
- Source authority、quality gate result、memory promotion、learning case publication、human approval 或 production authorization。
- CRM domain capability。
- Any named runtime、provider、model、workflow engine、tool platform、vector database or product selection。

## 3. Governing Authority

| Authority | Role |
|---|---|
| AEOS-ADR-005 | AEOS owns runtime-neutral Agent Collaboration governance |
| AEOS-ADR-003 | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 | Execution Contract, runtime boundary and evidence boundary |
| AEOS-SPEC-002 | Collaboration trace envelope and quality gate semantics |
| AEOS-SPEC-003 | Runtime adapter execution request boundary and Gate runner interpretation |

Rules：

- Runtime executes authorized request only。
- Runtime adapter evidence reports execution facts, not governance decisions。
- Runtime failure MUST NOT be interpreted as validation pass。
- Runtime success MUST NOT be interpreted as source validation success, memory confirmation, case publication, human approval or production authorization。

## 4. Runtime Adapter Boundary

Runtime adapter is a provider-neutral boundary between AEOS Control Plane execution request and concrete runtime / harness / provider implementation。

Runtime adapter MAY translate request fields into provider-specific calls, but MUST preserve AEOS Execution Contract semantics and MUST NOT expand authority。

Runtime adapter response MUST be treated as execution evidence。It may feed Gate runner and verification, but it is not a gate decision by itself。

## 5. Execution Evidence Minimum Fields

Every runtime adapter response MUST include the following minimum fields or equivalent structured evidence：

| Field | Requirement |
|---|---|
| `evidence_id` | Stable evidence record identity |
| `execution_id` | Execution identity from the authorized request |
| `execution_request_id` | Runtime adapter request identity |
| `trace_id` | Collaboration Trace Envelope identity |
| `runtime_id` | Runtime or harness implementation identity / class |
| `adapter_id` | Adapter identity |
| `adapter_version` | Adapter contract / implementation version |
| `capability_id` | Capability or capability class executed |
| `provider_id` | Provider identity or provider class when available |
| `model_id` | Model identity or model capability class when applicable |
| `started_at` | Execution start timestamp |
| `ended_at` | Execution end timestamp |
| `outcome` | completed / failed / cancelled / timed_out / rejected / unsupported |
| `error_class` | Stable error class when outcome is not completed |
| `evidence_summary` | Minimal execution summary suitable for audit |

If a field is not applicable, the response MUST state not-applicable rather than omit governance-relevant meaning。

## 6. Optional Evidence Fields

When available and policy permits, runtime adapter response SHOULD include：

| Field | Meaning |
|---|---|
| `duration_ms` | Runtime execution duration |
| `latency_ms` | Observed provider / model / tool latency |
| `token_input` | Input token count or estimate |
| `token_output` | Output token count or estimate |
| `token_total` | Total token count or estimate |
| `cost_estimate` | Provider/runtime cost estimate |
| `cost_actual` | Actual cost when known |
| `tool_references` | Tool invocation references, not raw secret or full payload |
| `artifact_refs` | Output artifact references |
| `retry_count` | Runtime-level retry count |
| `cancellation_seen` | Whether cancellation / revocation was received |
| `provider_request_ref` | Provider-specific request reference if safe to retain |

Optional fields MUST follow forbidden data rules in §8。

## 7. Outcome and Error Semantics

Runtime adapter outcome describes execution status only。

| Outcome | Meaning |
|---|---|
| completed | Runtime completed requested execution within authorized scope |
| failed | Runtime attempted execution but failed |
| cancelled | Execution stopped due to cancellation / revocation |
| timed_out | Execution exceeded time constraint |
| rejected | Runtime rejected request due to policy, authorization, validation or local guardrail |
| unsupported | Runtime cannot support contract version, capability, tool, schema or required evidence |

Error class SHOULD be stable and provider-neutral, including：

| Error Class | Meaning |
|---|---|
| `contract_invalid` | Request contract missing or invalid |
| `contract_unsupported` | Contract version or mandatory semantic unsupported |
| `capability_unsupported` | Requested capability unavailable |
| `tool_unauthorized` | Tool scope not authorized or not expressible |
| `model_unavailable` | Model capability unavailable |
| `provider_error` | Provider failed or returned unavailable |
| `timeout` | Execution exceeded time limit |
| `cancelled` | Cancellation / revocation applied |
| `forbidden_data_detected` | Response would violate forbidden data rules |
| `evidence_incomplete` | Required evidence cannot be produced |

Runtime `completed` outcome MUST NOT be treated as quality gate `PASS` without Gate runner evaluation。

## 8. Forbidden Data Rules

Runtime adapter evidence MUST NOT include：

- secrets、API keys、tokens、passwords or raw credential material;
- complete prompt or hidden/system instruction content unless policy explicitly allows retention;
- chain-of-thought or private reasoning trace;
- unapproved raw customer content;
- unnecessary sensitive business data;
- raw tool payloads containing protected data when a safe reference is sufficient;
- provider-specific debug dumps that bypass evidence minimization;
- data outside the authorized Execution Contract scope。

Evidence SHOULD use references, summaries, redaction markers, hashes or artifact IDs when full content is not required for audit。

If required evidence cannot be produced without forbidden data, runtime adapter MUST return `rejected` or `failed` with `forbidden_data_detected` / `evidence_incomplete` rather than leaking the data。

## 9. Metadata Extension Envelope

Provider、model、runtime or tool-specific metadata MAY be included only inside an extension envelope。

Extension envelope SHOULD include：

| Field | Meaning |
|---|---|
| `extension_namespace` | Provider/runtime/tool namespace |
| `extension_version` | Extension schema version |
| `metadata_class` | runtime / provider / model / tool / diagnostic |
| `metadata` | Provider-specific metadata after minimization |
| `redaction_applied` | Whether sensitive fields were removed |

Extension metadata MUST NOT override stable core fields。If extension metadata conflicts with core evidence, core evidence and Control Plane policy prevail, and the conflict SHOULD be escalated。

## 10. Evidence Interpretation Rules

Control Plane and Gate runner MUST interpret runtime evidence under these rules：

- Runtime success is execution success only。
- Runtime failure is not validation pass。
- Runtime response MUST NOT be treated as source authority。
- Runtime response MUST NOT confirm memory promotion。
- Runtime response MUST NOT publish learning case or authoritative knowledge。
- Runtime response MUST NOT satisfy human approval unless it references valid approval evidence from an authorized approval authority。
- Runtime response MUST NOT authorize production deployment, destructive action or customer data mutation。
- Missing mandatory evidence SHOULD cause Gate runner to return `NEEDS_WORK`, `HUMAN_APPROVAL`, `REJECTED` or `BLOCKED` according to AEOS-SPEC-002 / AEOS-SPEC-003 policy。

## 11. Trace Alignment

Runtime adapter evidence MUST be linkable to AEOS-SPEC-002 Collaboration Trace Envelope and AEOS-SPEC-003 Runtime Execution Request trace。

Minimum trace linkage：

| Trace Field | Requirement |
|---|---|
| `trace_id` | MUST match Collaboration Trace Envelope |
| `execution_request_id` | MUST match Runtime Execution Request |
| `execution_id` | MUST preserve Execution Contract identity |
| `capability_id` | MUST map to authorized capability |
| `tool_references` | MUST connect tool execution to run trace when applicable |
| `artifact_refs` | SHOULD identify output artifacts without leaking forbidden data |

## 12. Conformance Checklist

Runtime adapter evidence contract adoption SHOULD demonstrate：

1. Evidence includes runtime id、adapter version、capability id、provider/model id、execution id、start/end time、outcome and error class。
2. Evidence includes cost、token、latency and tool reference when applicable and permitted。
3. Evidence excludes secrets、complete prompt、chain-of-thought and unapproved raw customer content。
4. Runtime does not return authority decision、memory confirmation、learning case publication or human approval result。
5. Runtime failure cannot be interpreted as validation pass。
6. Provider-specific metadata stays inside extension envelope。
7. Extension metadata cannot override stable core evidence fields。
8. Evidence links to Collaboration Trace Envelope and Runtime Execution Request trace。

## 13. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #69 | GitHub Issue | Runtime Adapter Execution Evidence Contract 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS owns Agent Collaboration governance |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / Runtime boundary |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Execution Contract and Runtime responsibility |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec | Specification | Trace envelope and gate semantics |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension | Specification | Runtime execution request boundary and Gate runner semantics |

## 14. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-09-05 | 建立 Runtime Adapter Execution Evidence Contract Draft，定義 provider-neutral runtime evidence minimum fields、optional cost/token/latency/tool references、forbidden data rules、extension envelope、evidence interpretation rules 與 trace alignment | Codex |
