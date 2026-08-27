---
doc-id: AEOS-ARCH-014
doc-name: Productizable Platform Architecture
doc-type: Architecture
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-27
related:
  - EWO-AEOS-0045
  - AEOS-ADR-004
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-010
  - AEOS-ARCH-012
  - AEOS-ARCH-013
---

# AEOS-ARCH-014 — Productizable Platform Architecture

## Executive Summary

本文件定義 AEOS 的 Productizable Platform Architecture，建立從 internal production reference implementation 演進至 reusable enterprise capability、platform core 與 commercial product / solution packaging 的正式邏輯邊界。

核心原則是：**先以真實內部需求驗證能力，再透過明確 promotion criteria 抽離可重用部分；不得為了商品化而提前泛化所有功能，也不得讓 company-specific logic 回滲 Platform Core。**

本架構不規定商業模式、定價、SKU、multi-tenant 或 single-tenant 的最終選型；它規範的是在任何產品化路徑下都必須維持的 dependency、contract、configuration、adapter 與 governance invariants。

本文件目前為 Draft 0.1.0，尚未納入 `AEOS-ARCH-001` Approved Architecture Register。

## 1. Purpose

本文件之目的為：

- 定義 internal reference implementation 與 productizable platform 的正式邏輯邊界。
- 防止單一公司 domain logic、schema、workflow、channel 或 vendor assumption 污染 Platform Core。
- 建立 reusable capability promotion criteria。
- 定義 commercial product 如何組合 Platform Core、Reusable Capabilities 與 customer-specific configuration / adapters。
- 使第二家公司導入時可以更換 domain、workflow、data mapping、channel、branding、identity、integration 與 infrastructure profile，而不重寫平台核心治理語意。
- 明確區分 AEOS Architecture / Governance IP 與對外商品本身。

## 2. Scope

### 2.1 In Scope

- Productization logical layers。
- Internal reference implementation boundary。
- Reusable capability promotion / demotion criteria。
- Platform Core responsibility boundary。
- Customer / deployment-specific configuration 與 adapter isolation。
- Product composition 與 packaging invariants。
- Dependency direction 與 anti-coupling rules。
- Contract ownership、versioning 與 compatibility requirements。
- Conformance validation 與 portability evidence。
- 與 Agent Architecture、Platform、Layer、Capability、Dependency、Workspace Architecture 的 mapping。

### 2.2 Out of Scope

- Pricing、licensing、sales、marketing、SKU strategy。
- 最終 multi-tenant / single-tenant deployment decision。
- Production infrastructure sizing、network topology、KMS 或 credential provisioning。
- 任何具名 CRM、ERP、Agent framework、LLM、database、cloud 或 integration vendor selection。
- 客戶 onboarding 實作、migration project 或 production rollout。

## 3. Architecture Authority

| Authority | Role |
|---|---|
| AEOS-ARCH-001 | Architecture Baseline 與正式架構入口 |
| AEOS-ARCH-005 | Platform boundary 與 product-neutral platform identity |
| AEOS-ARCH-006 | Layer responsibility、dependency direction 與 anti-bypass rules |
| AEOS-ARCH-007 | Capability-first definition 與 implementation separation |
| AEOS-ARCH-009 | Dependency governance |
| AEOS-ARCH-010 | Workspace / repository execution boundary |
| AEOS-ARCH-012 | Architecture Principles |
| AEOS-ARCH-013 | Agent Control Plane / Execution Plane、Runtime / Harness / Provider Neutrality |
| AEOS-ADR-004 | Productization Boundary 候選決策 |

## 4. Productization Model

### 4.1 Company-specific Reference Implementation

此層代表單一企業、單一品牌或單一部署的真實 production implementation。

可包含：

- company-specific domain model；
- product / service knowledge；
- CRM / ERP field mapping；
- workflow 與 approval profile；
- channel behavior；
- customer-specific prompts / policies；
- branding、UX configuration；
- integration-specific mapping；
- deployment-specific infrastructure configuration。

規則：

- MAY 使用 Platform Core 與 Reusable Enterprise Capabilities。
- MAY 作為新 capability 的 validation source。
- MUST NOT 成為 Platform Core 的 dependency target。
- MUST NOT 將單一公司 schema、vendor API、channel event format 或 workflow hard-code 為平台核心契約。

### 4.2 Reusable Enterprise Capability

此層代表經過抽離、可跨公司或跨產品重用的 capability。

最低要求：

- 穩定、版本化 contract；
- 明確 input / output / error semantics；
- configuration boundary；
- adapter / provider isolation；
- authorization、audit、observability requirements；
- conformance tests；
- 不依賴單一 company identity 或單一 customer schema。

典型候選可包括 customer memory、conversation orchestration、human takeover、approval orchestration、audit evidence、tool access control、integration adapter pattern，但任何具體 capability 是否正式列入 Catalog 仍需獨立 Review / Approval。

### 4.3 Platform Core

Platform Core 承載跨產品穩定且具 Enterprise authority 的結構，包括：

- governance semantics；
- policy / approval / authorization contract；
- capability composition rules；
- execution / control contracts；
- identity / scope / revocation semantics；
- audit / observability contract；
- adapter / provider interface ownership；
- compatibility 與 lifecycle rules。

Platform Core：

- MUST 不依賴 Company-specific Reference Implementation。
- MUST 不要求單一 customer schema、workflow、channel 或 provider 才能成立。
- MUST 維持 product-neutral、provider-neutral 或透過 adapter 隔離具名 implementation。
- SHOULD 允許多個 commercial product / solution 共用。

### 4.4 Commercial Product / Solution Packaging

商業產品層負責把平台能力形成客戶可購買、部署與使用的 solution。

典型組成：

`Platform Core + Reusable Capabilities + Product UX + Customer Configuration + Adapters + Deployment Profile`

規則：

- MAY 形成不同 vertical solution、edition、SKU 或 deployment profile。
- MAY 針對產業預設 workflow / schema profile。
- MUST 透過 configuration / extension / adapter boundary 客製化。
- MUST NOT 以 fork Platform Core governance semantics 作為正常客製方式。

## 5. Dependency Rules

### 5.1 Allowed Direction

主要依賴方向：

`Commercial Product / Solution` → `Reusable Enterprise Capability` → `Platform Core`

`Company-specific Reference Implementation` → `Reusable Enterprise Capability` / `Platform Core`

### 5.2 Forbidden Reverse Dependencies

下列依賴 MUST NOT 發生：

- Platform Core → company-specific database schema；
- Platform Core → company-specific workflow；
- Platform Core → company-specific brand / product knowledge；
- Platform Core → single customer integration implementation；
- Reusable Capability contract → single customer field names；
- Enterprise governance semantics → commercial SKU / UI behavior。

### 5.3 Interface Ownership

若 customer-specific adapter 實作某 interface：

- interface / contract ownership MUST 位於 Platform Core 或 Reusable Capability layer；
- adapter implementation MAY 位於 company / product layer；
- adapter-specific configuration MUST NOT 改寫 interface semantics。

## 6. Capability Promotion Pipeline

Company-specific capability 提升為 Reusable Enterprise Capability 應依序通過：

### P1 — Evidence
確認 capability 已在真實 use case 中產生穩定價值，而不是純假設抽象。

### P2 — Decoupling
移除 company name、product knowledge、customer schema、workflow、channel 與 vendor-specific hard dependency。

### P3 — Contract
建立版本化 contract、error model、authorization / audit semantics 與 lifecycle。

### P4 — Adapter / Configuration Isolation
將 customer / provider 差異移入 adapter、configuration、policy profile 或 extension point。

### P5 — Portability Validation
至少以兩種 implementation / deployment profile 驗證核心 contract 不需修改。

### P6 — Architecture Review
確認 dependency direction、authority boundary、security、observability 與 failure behavior 均符合 AEOS。

### P7 — Catalog / Baseline Admission
若需成為正式 Enterprise Capability 或 Platform fact，再依既有 Catalog / Architecture governance 流程獨立登錄。

## 7. Commercialization Boundary Invariants

無論產品最終採 SaaS、single-tenant、managed service、on-premise 或 hybrid deployment，以下 invariants MUST 維持：

1. Enterprise governance authority 不因 product packaging 改變。
2. Company-specific logic 不得反向成為 Platform Core dependency。
3. Customer-specific integration 必須透過 adapter / extension boundary 接入。
4. Configuration 不得用來繞過 authorization、approval、audit 或 security policy。
5. Provider / runtime 替換不得破壞核心 governance semantics。
6. Product-specific UX 不得成為核心 capability contract 的唯一入口。
7. Audit evidence、identity、authorization scope 與 revocation semantics 必須在產品邊界上保持可追蹤。

## 8. Relationship with Enterprise AI Agent Architecture

`AEOS-ARCH-013` 已規定 Agent Control Plane、Agent Execution Plane、Harness / Runtime Neutral 與 Provider Adapter Boundary。

本架構補充其產品化邊界：

- Agent Harness / Runtime implementation MAY 存在於 company-specific 或 product-specific deployment profile。
- Agent Execution Contract、governance semantics 與 core authorization / approval rules SHOULD 保持於 Platform Core。
- 可重用 Agent capability MAY 依 Promotion Pipeline 提升為 Reusable Enterprise Capability。
- Commercial Product MAY 組合不同 Harness / Runtime / model / tool provider，但不得因此取得或修改 Enterprise governance authority。

## 9. Reference Implementation Rule

Internal production system SHOULD 被視為 **reference implementation**，而非自動等同 future commercial product。

Reference implementation 的角色是：

- 驗證 capability 是否真正需要；
- 提供 failure / edge case evidence；
- 驗證 contract 與 governance；
- 發現哪些 concern 必須留在 customer-specific layer；
- 為 promotion 提供實證。

只有經過 Promotion Pipeline 的能力，才應被視為可進入 reusable / platform 層的候選。

## 10. Conformance Checklist

任何宣稱可商品化或可重用的 capability SHOULD 至少回答：

- 是否仍引用單一公司名稱、schema、workflow、channel 或 vendor？
- 是否有版本化 contract？
- customer-specific mapping 是否可外置？
- provider / integration 是否可替換？
- authorization / approval / audit semantics 是否穩定？
- 是否有 portability / substitution evidence？
- 是否存在 Platform Core → company-specific reverse dependency？
- 第二家公司導入是否需要修改 Platform Core？若需要，原因是否經 Architecture Review 接受？

## 11. Lifecycle

- Draft：候選架構，不得視為 Approved baseline。
- Review：執行 Architecture / ADR / Governance Review。
- Approved：經核准後可納入 `AEOS-ARCH-001` Architecture Register。
- Amendment：後續產品化經驗若改變 boundary，必須透過正式 EWO / Review 修改。

## 12. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-27 | 建立 Productizable Platform Architecture：四層 Productization Model、dependency direction、promotion pipeline、commercialization invariants 與 Agent Architecture mapping | ChatGPT |
