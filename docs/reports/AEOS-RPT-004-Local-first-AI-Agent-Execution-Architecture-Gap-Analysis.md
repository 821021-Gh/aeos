---
doc-id: AEOS-RPT-004
doc-name: Local-first AI Agent Execution Architecture Gap Analysis
doc-type: Report
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-27
related:
  - EWO-AEOS-0046
  - AEOS-ARCH-001
  - AEOS-ARCH-013
  - AEOS-ARCH-014
  - AEOS-ADR-003
  - AEOS-STD-005
  - AEOS-STD-007
  - AEOS-SPEC-001
  - YEOS-ENG-STD-008
  - YEELIGHT-AI-CRM-AGENTS
---

# AEOS-RPT-004 — Local-first AI Agent Execution Architecture Gap Analysis

> EWO-AEOS-0046：本報告依 2026-08-27 使用者工作要求，檢查 AEOS、YEOS 與 yeelight-ai-crm `main` 分支之現況，評估 Local-first AI Agent Execution Architecture 的既有覆蓋與差距。本文件為 Draft Report；不取代 AEOS-ARCH-013、AEOS-ADR-003、YEOS ENG-STD-008 或任何已核准 Approval Policy。

## Executive Summary

AEOS `main` 已具備 Enterprise AI Agent Architecture 的核心治理骨架：Agent Control Plane 與 Agent Execution Plane 已分離，Runtime / Harness / Provider Neutrality 已明確成立，Execution Contract、routing、approval、authorization、audit、budget 與 failure policy 均有架構責任歸屬。

本次差距不在於缺少 agent architecture，而在於尚未把「Local-first execution」操作化為可審核的 routing tier、entry / exit criteria、cost telemetry、escalation reason 與 cross-repository adoption plan。建議第一階段不重寫既有 YEOS command approval 與 risk policy，而是在 AEOS 中補上 local-first routing 與 cost governance 的候選規格，並由 CRM 作為 reference implementation PoC 的候選場景。

最小下一步為：依 EWO-AEOS-0046 建立 `AEOS-SPEC-001`，將 Tier 0 到 Tier 4 路由模型、cost telemetry 與 escalation reason 操作化為 Draft Specification，並保持所有具名 runtime、local model、cloud model、harness 與 tool 僅位於 Adapter / Provider / Reference Implementation 邊界。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-RPT-004 |
| 文件名稱 | Local-first AI Agent Execution Architecture Gap Analysis |
| 型別 | Report |
| 用途分類 | Architecture Gap Analysis / Candidate Assessment |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-27 |
| 最後更新 | 2026-08-27 |
| 依據文件 | EWO-AEOS-0046、AEOS-ARCH-013、AEOS-ADR-003、AEOS-ARCH-014、AEOS-STD-007、YEOS ENG-STD-008、yeelight-ai-crm AGENTS.md / .ai/PROJECT.md |
| 關聯文件 | AEOS-ARCH-001、AEOS-STD-005、AEOS-STD-007、AEOS-SPEC-001、YEOS ENG-STD-008、yeelight-ai-crm AGENTS.md |

## 1. Purpose

本報告目的為：

- 建立 AEOS / YEOS / yeelight-ai-crm 三個 repository 的 Local-first AI Agent Execution Architecture baseline。
- 評估既有架構對 Agent Control Plane、Execution Plane、Runtime Neutrality、routing、approval、audit、cost governance 與 observability 的覆蓋程度。
- 識別尚未正式定義的缺口。
- 提出不繞過 YEOS command approval、不綁定特定 LLM / runtime / harness 的 target architecture tiers。
- 建議後續 repo-first 實作順序、文件位置、PR order 與驗證方式。

## 2. Scope

### 2.1 In Scope

- AEOS `main` 上已核准 Agent Architecture 與 Productizable Platform Architecture。
- YEOS `main` 上已釋出的 AI Agent Command Approval Standard。
- yeelight-ai-crm `main` 上的 agent 操作守則、project context、CI / repository baseline 與 implementation boundary。
- Local-first execution tiers：Deterministic Rule、Local AI、Low-cost Cloud AI、Frontier Cloud AI、Human Approval。
- Routing、escalation、cost telemetry、approval、audit 與 observability 的 gap analysis。

### 2.2 Out of Scope

- 選定或背書任何具名 local LLM、cloud model、agent framework、harness、runtime、vector database 或 tool provider。
- 修改 YEOS 既有 Command Classification、Risk Classification、Approval Policy 或 Repository Protection 規則。
- 直接進行 production deployment、credential / KMS provisioning、production data access 或 destructive operation。
- 在本報告中核准新的 Architecture、ADR、Standard、Catalog Entry 或 production rollout。

## 3. Repository Baseline

| Repository | `main` HEAD / 狀態 | 既有重點 | Local-first 相關風險 |
|-----------|-------------------|----------|----------------------|
| AEOS | `6984a9820922d8b056f69d2cea1d8d4acbe04d46`；GitHub default branch 目前不是 `main`；`main` 未啟用 branch protection | AEOS-ARCH-013 與 AEOS-ADR-003 已建立 Control Plane / Execution Plane、Runtime Neutrality、Execution Contract、routing、approval、audit、budget 邊界；AEOS-ARCH-014 已建立 productization boundary | Default branch 與 `PROJECT_STATE.md` Source of Truth 不一致；`main` 未保護會降低治理 gate 的技術強度；local-first tier / telemetry 尚未正式化 |
| YEOS | `e5144f099c585e278a9cff7390420d50adcd2eba`；default branch `main`；`main` protected，required check 為 `Validate repository documents` | ENG-STD-008 已定義 C1-C4 Command Classification、L/M/H/C Risk Classification、Approval Policy、Repository Protection 不可繞過 | Local agent routing 必須採用既有標準，不得另行降低 approval；YEOS repo 缺少薄型 `PROJECT_STATE.md` / `AGENTS.md` 對 agent 啟動不利 |
| yeelight-ai-crm | `366689077da441eb173d81b789ae241baa66caf1`；default branch `main`；`main` 未保護；有 draft PR / open issues | `AGENTS.md` 要求 Production First、Scope First、Evidence First、Security by Default、Human-controlled Release；`.ai/PROJECT.md` 明確要求 provider-neutral、adapter boundary、planned capability 不得描述為 production | 產品 repo 可作 reference implementation，但未保護 main；若引入 local runtime / model PoC，必須限定在 adapter / provider 邊界且不得觸及 production secrets / data |

## 4. Existing Architecture Coverage Matrix

| Capability | AEOS | YEOS | yeelight-ai-crm | Gap |
|------------|------|------|-----------------|-----|
| Agent Control Plane | Covered：AEOS-ARCH-013 §4-§6、AEOS-ADR-003 D-01/D-02 | Partially covered through governance standards, not architecture ownership | Partially covered through repository instructions and workflow | Need local-first routing policy as Control Plane responsibility profile |
| Agent Execution Plane | Covered：Harness / Runtime / Provider / Adapter boundary defined | Not primary scope | Partially present as implementation repo | Need candidate execution contract profile for deterministic / local / cloud tiers |
| Runtime Neutrality | Covered and explicit | Covered by tool-independent approval standard | Covered by provider-neutral project instructions | Need ensure local LLM names remain adapter/provider examples only |
| Local AI Runtime | Allowed as implementation option | Not defined | Not implemented as governed capability | Need Tier 1 semantics without naming a mandatory runtime/model |
| Model Routing | Covered generally by capability, risk, cost, data boundary | Not a YEOS concern except command approval impact | Not formalized | Need routing matrix: Risk x Complexity x Confidence x Cost x Latency x Data Sensitivity |
| Escalation | Covered generally in failure policy | Covered for command risk and approval escalation | Workflow contains human-controlled release | Need explicit tier exit criteria and escalation reason taxonomy |
| Risk Classification | Architecture references policy context; not command taxonomy owner | Covered by ENG-STD-008 | Repository-specific instructions elevate production/security care | Must reuse YEOS classification; no redesign |
| Approval | Covered as Control Plane responsibility | Covered by ENG-STD-008 | Human-controlled release stated | Need local-first rules saying local execution cannot bypass approval |
| Audit | Covered: audit correlation, evidence, denied operations | Covered: traceability required | Evidence First principle | Need per-tier execution evidence fields and local/cloud/frontier/human ratios |
| Cost Governance | Budget and token/cost limits exist in AEOS-ARCH-013 / AEOS-STD-007 | Not primary scope | Not formalized | Need cost telemetry: tokens, provider tier, retries, escalation reason, success rate |
| Observability | Covered as evidence plane | Traceability baseline | CI and evidence workflow present | Need metrics schema for routing outcomes and tier effectiveness |

## 5. Gap Analysis

### 5.1 Confirmed Strengths

- AEOS already prevents product-centric agent architecture by separating Control Plane authority from Harness / Runtime execution authority.
- AEOS already supports routing by capability, risk, cost, data boundary, availability and budget without binding to a specific model provider.
- YEOS already defines command and risk classifications with mandatory Human Review / Approval boundaries.
- CRM already contains repository-local instructions that protect production behavior, secrets, provider neutrality and human-controlled release.

### 5.2 Primary Gaps

| Gap ID | Gap | Impact | Suggested Owner |
|--------|-----|--------|-----------------|
| GAP-001 | Local-first tier model is not formally defined | Teams may treat local runtime choice as architecture, or may escalate to cloud by habit | AEOS Architecture |
| GAP-002 | Entry / exit / escalation criteria are not explicit per tier | Routing decisions are hard to audit and tune | AEOS Specification / Standard |
| GAP-003 | Cost telemetry and success ratio schema are missing | Cannot prove reduced cloud tokens or human intervention | AEOS Standard or reusable capability spec |
| GAP-004 | Local AI approval boundary is not explicitly stated | A local agent could be mistakenly treated as lower governance risk | AEOS + YEOS mapping |
| GAP-005 | CRM reference implementation path is not separated from platform architecture | PoC could accidentally introduce product-specific assumptions into Platform Core | AEOS-ARCH-014 mapping + CRM SPEC |
| GAP-006 | Repository protection is inconsistent across repos | AEOS and CRM `main` can be changed without equivalent technical gate | Repository Owner / Governance |

### 5.3 Safety / Governance Blockers

No blocker was found for creating this Draft report and opening a Draft PR.

The following actions remain approval-bound and are not performed by this report:

- Changing repository default branch or branch protection settings.
- Merging architecture changes into `main`.
- Creating or activating production local agent runtimes.
- Accessing production data, credentials, KMS, customer data or secret material.
- Changing YEOS command approval policy.

## 6. Target Architecture Tiers

Local-first execution MUST be a Control Plane routing policy over neutral Execution Plane providers. Tier names define governance semantics, not products.

| Tier | Name | Typical Entry Criteria | Exit / Escalation Criteria | Governance / Audit Requirements |
|------|------|------------------------|----------------------------|---------------------------------|
| Tier 0 | Deterministic Rule | Static validation, schema checks, formatting, policy lookup, classification, CI, deterministic automation | Rule conflict, unknown input, insufficient deterministic evidence, policy ambiguity | Log rule version, input class, output, confidence as deterministic / exact, failure reason |
| Tier 1 | Local AI | Low-risk summarization, classification, extraction, retrieval, drafts, log / code review preprocessing, routine verifiable reasoning | Low confidence, context overflow, repeated failure, unsupported modality, high sensitivity mismatch, task complexity above local profile | Log local provider class, model capability class, prompt/context budget, confidence, validation evidence; must preserve approval scope |
| Tier 2 | Low-cost Cloud AI | Local tier insufficient, task still low/medium risk, larger context or stronger reasoning needed, data policy permits cloud | Confidence still insufficient, complex architecture/debugging, cross-repo reasoning, high business impact | Log provider tier, tokens, cost estimate/actual, data sensitivity decision, escalation reason |
| Tier 3 | Frontier Cloud AI | Architecture reasoning, complex debugging, high-context review, cross-repo decision support, high ambiguity | Approval required, destructive/protected operation, credential/production access, irreversible or high-risk decision | Log premium model tier use, justification, alternatives attempted, cost, human-readable evidence |
| Tier 4 | Human Approval | Production deploy/activation, destructive ops, credential/KMS, sensitive data, irreversible ops, high-risk business decision, mandatory approval under YEOS | Human approves, rejects, requests changes or delegates bounded execution | Human approval evidence, approver identity/reference, policy version, decision, authorization scope |

### 6.1 Routing Inputs

Control Plane routing SHOULD evaluate at minimum:

`Risk x Complexity x Confidence x Cost x Latency x Data Sensitivity`

Where:

- Risk MUST inherit or map to the repository's approved risk and approval policy.
- Complexity SHOULD consider required reasoning depth, cross-repository scope, context size and ambiguity.
- Confidence SHOULD be measured from deterministic evidence, validation results, model self/critic confidence where available, and retry history.
- Cost SHOULD include local compute class, cloud token estimate, observed token usage, retry count and opportunity cost.
- Latency SHOULD include user-facing responsiveness and queue constraints.
- Data Sensitivity MUST gate whether cloud tiers are allowed at all.

### 6.2 Non-bypass Rule

Local execution lowers cost and latency; it does not lower governance.

A Tier 0 or Tier 1 execution MUST NOT:

- bypass YEOS ENG-STD-008 command approval requirements;
- weaken repository protection;
- self-approve M/H/C risk actions;
- expand tool, model, data, memory or credential scope beyond the Execution Contract;
- convert a provider-specific local runtime configuration into AEOS architecture authority.

## 7. Implementation Plan

### 7.1 Recommended PR Order

| Order | Repository | Change | Files / Artifacts | Validation |
|-------|------------|--------|-------------------|------------|
| 1 | AEOS | Add this Draft gap analysis report | `docs/reports/AEOS-RPT-004-Local-first-AI-Agent-Execution-Architecture-Gap-Analysis.md` | Markdown / metadata review; Architecture Review as RPT candidate assessment |
| 2 | AEOS | Create Draft Specification for Local-first Routing Profile | `docs/specifications/AEOS-SPEC-001-Local-first-AI-Agent-Execution-Routing-Profile.md` | Check consistency with AEOS-ADR-003, AEOS-ARCH-013, AEOS-STD-007 |
| 3 | AEOS | Decide whether approved content should amend AEOS-ARCH-013 | Architecture amendment or follow-up ADR only if review requires it | Architecture Owner / Repository Owner review |
| 4 | YEOS | Map ENG-STD-008 command/risk classifications into local-first routing adoption note, without redesign | Existing standards or supplemental engineering note | Confirm no approval downgrade and repository protection still required |
| 5 | yeelight-ai-crm | Add PoC SPEC for adapter-bound local-first execution | CRM docs/spec; implementation behind config / adapter boundary | CI, no production data/secrets, no production activation |
| 6 | yeelight-ai-crm | Implement deterministic and local-preprocessing PoC | Tests/scripts under existing repo patterns | Tests prove Tier 0/Tier 1 outputs are verifiable and escalation reasons are recorded |

### 7.2 First Minimal Implementation

This Draft report is the first minimal implementation artifact. It is intentionally narrow because it crosses architecture governance territory but does not itself approve a new architecture. It provides a reviewable basis for `AEOS-SPEC-001` and follow-up Architecture Review.

### 7.3 ADR Requirement Assessment

A new ADR is likely required only if AEOS chooses one of the following:

- Make local-first routing mandatory for all agent executions.
- Add a new Control Plane decision model that materially changes AEOS-ADR-003.
- Define cost governance as a new enterprise policy authority.

If the next change only adds a compatible routing profile under AEOS-ARCH-013's existing Control Plane routing and budget responsibilities, `AEOS-SPEC-001` may be sufficient.

## 8. Validation

This report was prepared against current `main` branch evidence on 2026-08-27:

| Check | Result |
|-------|--------|
| AEOS `main` inspected | Pass |
| YEOS `main` inspected | Pass |
| yeelight-ai-crm `main` inspected | Pass |
| Runtime / model neutrality preserved | Pass |
| YEOS command approval redesign avoided | Pass |
| Production / credential / destructive action avoided | Pass |
| Report status kept Draft | Pass |
| EWO-AEOS-0046 traceability added | Pass |

## 9. References

| 文件 | 型別 | 用途 |
|------|------|------|
| EWO-AEOS-0046 — Local-first AI Agent Execution Routing Profile | EWO | Work source and scope |
| AEOS-ARCH-001 — Architecture Baseline | Architecture | AEOS architecture register and baseline authority |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Control Plane / Execution Plane / Runtime Neutrality authority |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Approved decision for authority separation |
| AEOS-ARCH-014 — Productizable Platform Architecture | Architecture | Productization and reference implementation boundary |
| AEOS-STD-005 — Review Standard | Standard | Draft PR / Architecture Review requirements |
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard | Standard | Token budget, routing and context governance |
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile | Specification | Routing profile draft specification |
| YEOS ENG-STD-008 — AI Agent Command Approval Standard | Standard | Command classification, risk classification and approval policy |
| yeelight-ai-crm `AGENTS.md` | Repository instruction | Production-first and human-controlled release guardrails |
| yeelight-ai-crm `.ai/PROJECT.md` | Project context | CRM scope, provider-neutral and adapter boundary guidance |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-27 | Initial Draft baseline, coverage matrix, gap analysis, target local-first tiers and implementation plan; aligned to EWO-AEOS-0046 and AEOS-SPEC-001 | Codex |
