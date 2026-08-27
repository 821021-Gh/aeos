---
doc-id: AEOS-ADR-004
doc-name: Productization Boundary Decision
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-27
related:
  - EWO-AEOS-0045
  - AEOS-ARCH-001
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-012
  - AEOS-ARCH-013
  - AEOS-ARCH-014
---

# AEOS-ADR-004 — Productization Boundary Decision

## Executive Summary

本 ADR 建立 AEOS 的 Productization Boundary，正式區分 `Company-specific Reference Implementation`、`Reusable Enterprise Capability`、`Platform Core` 與 `Commercial Product / Solution Packaging` 四個邏輯層級。

決策目的不是把 AEOS 本身強制定義為可銷售 SaaS，而是確保目前以公司內部需求驗證出的能力，可以在不污染 Platform Core、不改寫 Enterprise governance semantics 的前提下，逐步抽離為可重用能力並組裝成對外商業產品。

## 1. Context

AEOS 已建立 Platform、Layer、Capability、Dependency、Workspace 與 Enterprise AI Agent Architecture，並已確立 Runtime Neutral、Harness Neutral、Provider Adapter Boundary 與 Agent Control Plane / Execution Plane separation。

目前仍存在一個尚未正式治理的邊界：內部 production implementation 與未來可商品化平台之間的責任分離。

若缺少此邊界，內部 CRM、客服、ERP/Ragic、channel behavior、domain workflow、商品知識與公司特定資料模型，可能逐步被寫入平台核心，導致第二家公司導入時只能複製既有內部系統，而無法重用通用 capability。

## 2. Decision

AEOS SHOULD 採用以下四層 Productization Model：

### 2.1 Company-specific Reference Implementation

- 承載單一企業或單一部署特有的 domain logic、workflow、data mapping、branding、channel behavior、integration mapping 與 operational configuration。
- MAY 在 production 使用並成為 reference implementation。
- MUST NOT 被視為 Platform Core 的事實來源。
- MUST NOT 形成 Platform Core 對其 schema、workflow 或 vendor-specific behavior 的反向依賴。

### 2.2 Reusable Enterprise Capability

- 承載可跨企業或跨產品重用的 business / technical capability。
- MUST 具有明確 contract、configuration boundary、adapter boundary 與 conformance test。
- SHOULD 保持 provider-neutral、integration-neutral 或透過 adapter 隔離具名 provider。
- 只有通過 promotion criteria 的 internal capability MAY 被提升至本層。

### 2.3 Platform Core

- 承載跨產品穩定的 governance semantics、control contracts、capability composition、policy boundary、execution contract、identity / authorization semantics、observability contract 與 adapter interfaces。
- MUST 不依賴 Company-specific implementation。
- MUST 不把客戶特定 workflow、schema、branding、channel 或 vendor selection 視為核心前提。
- MAY 被多個 commercial product 共用。

### 2.4 Commercial Product / Solution Packaging

- 由 Platform Core、Reusable Enterprise Capabilities 與 customer / deployment-specific configuration、adapters、UX、branding、workflow package 組成。
- MAY 形成不同 SKU、industry solution、single-tenant 或 multi-tenant deployment。
- MUST NOT 透過產品包裝改寫 Platform Core 的 Enterprise governance semantics。

## 3. Dependency Direction

允許的主要依賴方向：

`Commercial Product / Solution` → `Reusable Enterprise Capability` → `Platform Core`

`Company-specific Reference Implementation` MAY 使用 `Reusable Enterprise Capability` 與 `Platform Core`，但 Platform Core MUST NOT 依賴 Company-specific implementation。

Customer-specific adapter MAY 實作 Platform / Capability 所定義的 interface；interface ownership MUST 位於可重用或平台層，而非 customer-specific layer。

## 4. Capability Promotion Decision

Internal capability 要從 Company-specific layer 提升為 Reusable Enterprise Capability，至少 MUST 滿足：

1. Use case 不再依賴單一公司名稱、商品、資料表或 workflow。
2. Public / internal contract 已明確版本化。
3. Company-specific mapping 可由 configuration 或 adapter 注入。
4. Provider / channel / integration 差異可被 boundary 隔離。
5. 有跨至少兩種 implementation profile 的測試或可驗證替換證據。
6. Failure、audit、authorization 與 observability semantics 不因替換 deployment 而消失。
7. Architecture Review 確認沒有 reverse dependency 回滲 Platform Core。

## 5. Consequences

### Positive

- 內部 production usage 可以繼續快速迭代，而不必先完成完整 SaaS 化。
- 可逐步辨識真正值得抽離的通用 capability。
- 第二家公司導入時可以替換 domain、workflow、data mapping、channel、branding 與 integration。
- AEOS 可維持 Architecture / Governance authority，而非被迫等同單一產品。

### Trade-offs

- Productization 需要額外 contract、adapter、configuration 與 conformance testing 成本。
- 並非所有 internal feature 都值得提升為 reusable capability。
- 初期可能存在 reference implementation 與 reusable capability 並行演進的重複成本。

## 6. Rejected Alternatives

### A. 直接把內部 CRM 複製成對外產品
拒絕。會將 company-specific schema、workflow 與 integration assumption 固化為產品核心。

### B. 一開始就把所有 internal feature 泛化
拒絕。會增加不必要抽象化與開發成本，且缺少真實第二 use case 驗證。

### C. 將 AEOS 本身直接定義為 SaaS Product
拒絕。AEOS 的首要角色是 Enterprise Architecture / Governance authority；是否包裝為商業 framework 或 platform product 應由後續產品策略決定。

## 7. Status and Approval

本 ADR 目前為 **Approved 1.0.0**。

EWO-AEOS-0045 Review Package 已完成合併，本文正式作為 internal reference implementation、reusable capability、Platform Core 與 commercial product packaging 分離之 Architecture Decision。

核准後：

- 可作為後續產品化、capability promotion 與 commercial packaging architecture 的決策權威。
- AEOS-ARCH-014 依本文建立 Productizable Platform Architecture 的 Approved Architecture 定義。
- AEOS-ARCH-001 Approved Architecture Register 應登錄 AEOS-ARCH-014。

## 8. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0045 Post-Merge Closure Verification：將 Productization Boundary Decision 升級為 Approved Architecture Decision | ChatGPT |
| 0.1.0 | 2026-08-27 | 建立 Productization Boundary、四層 Productization Model、dependency direction 與 capability promotion criteria 候選決策 | ChatGPT |
