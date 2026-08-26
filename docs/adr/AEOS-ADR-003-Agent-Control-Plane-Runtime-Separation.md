---
doc-id: AEOS-ADR-003
doc-name: Agent Control Plane and Runtime Separation Decision
doc-type: ADR
repository: AEOS
version: 0.1.0
status: Proposed
owner: Architecture Owner
created: 2026-08-26
updated: 2026-08-26
related:
  - EWO-AEOS-0044
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-012
  - AEOS-ARCH-013
---

# AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision

## 1. Context

AEOS 已正式定義 Platform、Layer、Capability、Repository、Dependency、Workspace 與 Architecture Principles，但尚未有一項明確決策界定 Enterprise AI Agent 的治理與執行責任。

若 Agent orchestration、policy、approval、tool access、model selection、memory access 與 execution runtime 被實作在同一產品邊界內，Enterprise Architecture 將容易產生下列風險：

- Runtime 或 framework 被誤認為 Enterprise Architecture 本身。
- 更換 Agent runtime 時必須同步改變治理語意與權限模型。
- Runtime 可在缺乏上位授權時自行擴張工具、模型、資料或憑證權限。
- 特定產品成為不可替換的架構前提。
- Audit、approval 與 execution evidence 無法跨 runtime 維持一致。

EWO-AEOS-0044 因此需要一項正式 Architecture Decision，建立 Control Plane 與 Runtime 之間的穩定責任邊界。

## 2. Decision

### D-01 — Logical Separation

AEOS SHALL 將 **Agent Control Plane** 與 **Agent Runtime Plane** 定義為邏輯上分離的責任平面。

此分離是 Enterprise Architecture boundary，不要求兩者必須部署於不同 process、host、cluster 或產品；實作可共置，但其 authority、contract 與責任 MUST 可被清楚區分。

### D-02 — Control Plane Authority

Agent Control Plane SHALL 擁有或協調下列 Enterprise-level responsibilities：

- Agent identity 與 lifecycle policy。
- Task / intent admission 與 orchestration policy。
- Policy evaluation 與 policy context delivery。
- Approval state 與 human / system authorization evidence。
- Tool permission、model permission、memory/data access scope。
- Execution contract 建立與版本治理。
- Runtime selection / routing policy。
- Audit correlation、observability requirements 與 execution evidence requirements。
- Cancellation、revocation、quarantine 與 failure containment policy。

Control Plane MAY 委派局部執行，但 SHALL NOT 將最終 Enterprise governance authority 隱性轉移給 Runtime。

### D-03 — Runtime Responsibility

Agent Runtime Plane SHALL 僅在有效 Execution Contract 與授權範圍內執行 Agent workload。

Runtime MUST NOT：

- 自行建立或提升 Enterprise approval。
- 覆寫 Control Plane policy。
- 擴張未授權的 tool、model、memory、data 或 credential scope。
- 將 runtime-local configuration 提升為 Enterprise Architecture authority。
- 以技術可行性推定治理允許性。

### D-04 — Runtime Neutrality

Enterprise AI Agent Architecture SHALL 保持 **Runtime Neutral**。

任何 Agent runtime、framework、model provider、tool provider、memory provider 或 orchestration implementation 均 SHALL 被視為可替換實作，除非另有經核准的 Architecture / ADR 明確賦予更高階身分。

AEOS Architecture MUST NOT 以特定產品名稱作為 mandatory component、mandatory dependency 或唯一合法實作。

### D-05 — Stable Execution Contract

Control Plane 與 Runtime 之間 SHALL 透過受版本治理的 Execution Contract 互動。契約至少包含：

- execution / task identity；
- agent identity / role reference；
- requested intent / task；
- policy context reference；
- approval state / authorization evidence reference；
- permitted tool scopes；
- permitted model scopes；
- permitted memory / data scopes；
- runtime constraints（budget、timeout、cancellation、sandbox / isolation requirements）；
- audit correlation identity；
- execution result / status / evidence requirements。

Runtime-specific parameters MAY 存在於 adapter extension，但 MUST NOT 改變 Enterprise contract 的治理語意。

## 3. Consequences

### Positive

- Runtime 可替換而不重寫 Enterprise governance semantics。
- Agent policy、approval 與 audit 可跨實作保持一致。
- 降低供應商或 framework lock-in。
- 下位 Implementation 無法正當化對上位 Architecture 的反向控制。
- 可建立一致的 security、observability 與 execution evidence boundary。

### Trade-offs

- 需要明確 adapter / interface design。
- Control Plane 與 Runtime contract 需要版本治理。
- 可能增加 implementation integration complexity。
- 某些單體 Agent framework 必須透過 logical separation 才能符合 Enterprise Architecture。

## 4. Alternatives Considered

### A. Product-Centric Agent Platform

將特定 Agent framework 直接定義為企業 Agent Architecture 核心。

**Rejected**：會將實作選擇提升為架構事實，違反 Capability Driven、Layer separation 與 Runtime Neutral 方向。

### B. Runtime-Owned Governance

由各 Runtime 自行處理 policy、approval、tool permission 與 audit。

**Rejected**：導致治理語意分裂、runtime replacement 成本高，且形成下位層反向控制上位責任的風險。

### C. No Formal Separation

僅以 deployment architecture 描述 Agent components，不建立 Control Plane / Runtime authority boundary。

**Rejected**：無法形成可驗證的 Enterprise responsibility model。

## 5. Architecture Alignment

- AEOS-ARCH-004：維持 Platform Oriented、Capability Driven、Dependency Explicit。
- AEOS-ARCH-005：不將產品或單一 runtime 自動等同 Platform。
- AEOS-ARCH-006：Control Plane governance semantics 不得被 L6 Implementation 反向控制。
- AEOS-ARCH-007：Agent capabilities 以企業能力描述，不以 runtime 實作替代。
- AEOS-ARCH-009：Runtime、tool、model、memory provider 依賴須為顯式、受治理依賴。
- AEOS-ARCH-012：後續 Agent implementation MUST 遵循既有 Architecture Principles。

## 6. Status and Approval

本 ADR 目前為 **Proposed**。

在 Architecture / ADR Review 完成並獲核准前：

- 不得視為 Approved Architecture Fact。
- 不得據此修改 AEOS-ARCH-001 Approved Architecture Register。
- 可作為 EWO-AEOS-0044 Draft implementation 與 review package 的候選決策載體。

## 7. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-26 | 建立 Agent Control Plane / Runtime logical separation、Runtime Neutral 與 Execution Contract 候選決策 | ChatGPT |
