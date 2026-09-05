---
doc-id: AEOS-ADR-005
doc-name: Agent Collaboration Ownership Decision
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-067
  - AEOS-ISSUE-066
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-013
  - AEOS-SPEC-002
---

# AEOS-ADR-005 — Agent Collaboration Ownership Decision

## Executive Summary

本 ADR 決定：**AEOS owns runtime-neutral Agent Collaboration**。

Agent Collaboration 是企業層級的 multi-agent governance、role/capability composition、task delegation、separation of duties、independent verification、quality gate、evidence、approval 與 lifecycle responsibility model。它不是任一產品 repository、agent harness、runtime、model provider、tool provider、workflow engine、vector database 或 downstream adoption 的局部實作責任。

Agent Control Plane 執行 AEOS policy，並建立受治理的 collaboration request / execution contract。Agent Runtime 僅執行已授權 request，並回傳 execution evidence。Product repositories 僅能承接 adoption mapping，不得建立平行 multi-agent governance。

## 1. Context

AEOS 已透過 AEOS-ADR-003 與 AEOS-ARCH-013 建立 Agent Control Plane、Agent Execution Plane、Agent Harness / Orchestration Boundary、Agent Runtime、Execution Contract、Runtime Neutral、Harness Neutral 與 Provider Boundary。

目前 Multi-Agent Collaboration 仍需要一項明確 ownership 決策。若 collaboration governance 由 product repository、runtime、harness、provider 或 workflow implementation 各自定義，將產生下列風險：

- 不同產品建立互不相容的 role、capability、delegation、verification 與 approval 語意。
- Harness / Runtime 因具備 orchestration 能力而被誤認為具有 Enterprise governance authority。
- Product repository 將 domain workflow adoption 誤升格為 enterprise multi-agent governance。
- Provider-specific tool、model、memory 或 workflow capability 反向塑造 AEOS policy。
- Independent verification、quality gates、evidence / provenance / confidence 與 escalation rules 無法跨 runtime / product 維持一致。

AEOS Issue #67 因此要求先做 ownership decision，再由 Issue #66 建立 Agent Collaboration Model Architecture Spec。

## 2. Decision

### D-01 — AEOS Owns Agent Collaboration Governance

AEOS MUST own runtime-neutral Agent Collaboration governance。

Agent Collaboration governance 至少包含：

- role archetype 與 agent profile 的企業語意；
- capability、skill、tool、policy 與 task decomposition 的關係；
- delegation、separation of duties 與 independent verification requirements；
- quality gates、evidence、confidence、provenance 與 audit expectations；
- information lifecycle、memory/data authority 與 retention boundary；
- human approval、escalation 與 blocked state semantics；
- runtime-neutral execution boundary 與 product adoption mapping rules。

### D-02 — Agent Control Plane Executes AEOS Policy

Agent Control Plane MUST execute AEOS policy for collaboration admission、composition、delegation、authorization、approval、quality gates、evidence requirements、revocation、failure handling 與 escalation。

Control Plane MAY delegate execution coordination to Agent Harness / Orchestration implementation under AEOS-ADR-003 and AEOS-ARCH-013, but MUST NOT delegate enterprise governance authority implicitly。

### D-03 — Agent Runtime Executes Authorized Requests Only

Agent Runtime MUST only execute authorized collaboration requests or delegated execution contracts。

Runtime responsibilities are limited to:

- validating contract completeness and supported semantics;
- executing within authorized role、task、tool、model、memory/data、credential、budget and environment scope;
- preserving delegated constraints;
- reporting result、status、error、decision evidence、tool/model/memory calls、confidence/provenance signals and audit correlation evidence。

Runtime MUST NOT create、inflate or override AEOS collaboration policy、approval、role authority、quality gate outcome or product adoption authority。

### D-04 — Product Repositories Provide Adoption Mapping Only

Product repositories MAY map AEOS Agent Collaboration Model to product-specific workflow、domain role、data source、channel、integration、UI or operational configuration。

Product repositories MUST NOT:

- create parallel enterprise multi-agent governance;
- redefine AEOS role archetype、separation of duties、verification、approval or evidence semantics;
- treat product-specific workflow as AEOS policy authority;
- use downstream adoption pressure to bypass AEOS architecture review;
- mark AEOS-level collaboration decisions as completed before AEOS artifacts exist and are approved。

Downstream issues, including YCRM adoption items, SHOULD be treated as blocked by AEOS until the relevant AEOS ADR / SPEC / Architecture artifacts are approved or otherwise declared adoptable。

### D-05 — Runtime, Harness, Provider and Product Neutrality

Agent Collaboration MUST remain runtime-neutral、harness-neutral、provider-neutral and product-neutral。

No specific runtime, harness, provider, model, tool platform, vector database, workflow engine, CRM repository or downstream product MAY be required as the authoritative definition carrier for Agent Collaboration governance。

### D-06 — Specification Carrier

AEOS-SPEC-002 SHOULD carry the first draft of the Agent Collaboration Model Architecture Spec for AEOS Issue #66, under the ownership decision of this ADR。

AEOS-SPEC-002 may define model concepts and conformance expectations, but MUST stay within AEOS-ARCH-013 authority boundaries and MUST NOT introduce runtime implementation, SDK, deployment topology, production operations or provider selection。

## 3. Consequences

### Positive

- Multi-agent governance has one enterprise owner and one runtime-neutral definition path。
- Product adoption can proceed through mapping instead of redefining governance。
- Harness / Runtime implementations remain replaceable and composable。
- Independent verification、quality gates、evidence and escalation become portable across products。
- YCRM downstream adoption can clearly reference AEOS as its blocking architecture authority。

### Trade-offs

- Product teams must wait for AEOS-level definitions before claiming governance completion。
- Runtimes and harnesses need adapters or contract mapping to express AEOS collaboration semantics。
- Collaboration policies require formal lifecycle and review, not ad hoc product configuration。

## 4. Alternatives Considered

### A. YCRM Owns Multi-Agent Collaboration Governance

Rejected。YCRM may adopt collaboration rules for CRM workflows, but CRM domain adoption is not enterprise architecture authority。

### B. Hermes, OpenClaw, DeepSeek Harness, OpenAI, Gemini, n8n, Qdrant or Another Runtime / Provider Owns Governance

Rejected。These systems may provide runtime, orchestration, model, workflow, tool, memory or integration capability, but provider capability does not create AEOS governance authority。

### C. Each Product Defines Its Own Governance

Rejected。This creates fragmented semantics and prevents consistent evidence, approval, verification and runtime substitution across the AI Engineering Workspace。

### D. No AEOS Ownership Decision

Rejected。Without a formal decision, downstream adoption and runtime implementation would continue to blur architecture authority with implementation convenience。

## 5. Architecture Alignment

- AEOS-ADR-003：保留 Control Plane / Execution Plane separation 與 runtime / harness neutrality。
- AEOS-ARCH-013：Agent Collaboration uses Agent Control Plane policy, Execution Contract, delegation rules and evidence boundary。
- AEOS-ARCH-007：Collaboration capabilities are capability-first, not implementation-first。
- AEOS-ARCH-009：Runtime、provider、tool、memory and product dependencies must remain explicit and governed。
- AEOS-ARCH-014：Productization or downstream packaging must not rewrite platform governance semantics。

## 6. Status and Approval

本 ADR 目前為 **Approved 1.0.0**。

PR #72 已合併至 `main`，merge commit 為 `03d800b741b663309cd8afa11075beaed30d6e27`。此合併作為 Repository Owner final approval evidence，正式核准本 ADR 為 AEOS Agent Collaboration ownership 的 Architecture Decision。

核准後：

- AEOS owns runtime-neutral Agent Collaboration governance。
- Product repositories 僅能承接 adoption mapping，不建立平行 multi-agent governance。
- Agent Control Plane 執行 AEOS policy。
- Agent Runtime 僅執行已授權 collaboration request / delegated execution contract，並回傳 execution evidence。
- AEOS-SPEC-002 可依本 ADR 作為 Agent Collaboration Model Architecture Spec 的 Draft 規格載體；其是否升級為 Approved 由後續 Specification Review 或 closure decision 決定。

## 7. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #67 | GitHub Issue | Agent Collaboration ownership decision 工作來源 |
| AEOS Issue #66 | GitHub Issue | Agent Collaboration Model Architecture Spec 工作來源 |
| AEOS-ARCH-001 — Architecture Baseline | Architecture | AEOS architecture entry and register authority |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / Runtime separation and runtime neutrality |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Architecture | Agent Control Plane、Execution Contract、Runtime Neutrality and evidence boundary |
| AEOS-ARCH-014 — Productizable Platform Architecture | Architecture | Product / platform boundary and downstream adoption guardrails |

## 8. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 PR #72 merge evidence（03d800b741b663309cd8afa11075beaed30d6e27）升級為 Approved Architecture Decision；正式確認 AEOS owns runtime-neutral Agent Collaboration governance，並確認 Product repositories 僅做 adoption mapping | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Collaboration ownership draft：AEOS owns runtime-neutral Agent Collaboration；Product repositories 僅做 adoption mapping；Runtime 只執行已授權 request 並回傳 evidence | Codex |
