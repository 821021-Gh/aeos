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

## 執行摘要

本 ADR 建立 AEOS 的 Productization Boundary，正式區分 `Company-specific Reference Implementation`、`Reusable Enterprise Capability`、`Platform Core` 與 `Commercial Product / Solution Packaging` 四個邏輯層級。

決策目的不是把 AEOS 本身強制定義為可銷售 SaaS，而是確保目前以公司內部需求驗證出的能力，可以在不污染 Platform Core、不改寫 Enterprise 治理語意的前提下，逐步抽離為可重用能力並組裝成對外商業產品。

## 1. 背景

AEOS 已建立 Platform、Layer、Capability、Dependency、Workspace 與 Enterprise AI Agent Architecture，並已確立 Runtime Neutral、Harness Neutral、Provider Adapter Boundary 與 Agent Control Plane / Execution Plane 分離。

目前仍存在一個尚未正式治理的邊界：內部正式環境實作與未來可商品化平台之間的責任分離。

若缺少此邊界，內部 CRM、客服、ERP/Ragic、通道行為、領域工作流程、商品知識與公司特定資料模型，可能逐步被寫入平台核心，導致第二家公司導入時只能複製既有內部系統，而無法重用通用能力。

## 2. 決定

AEOS SHOULD 採用以下四層 Productization Model：

### 2.1 公司特定參考實作

- 承載單一企業或單一部署特有的領域邏輯、工作流程、資料對應、品牌、通道行為、整合對應與 operational 設定。
- MAY 在正式環境使用並成為參考實作。
- MUST NOT 被視為 Platform Core 的事實來源。
- MUST NOT 形成 Platform Core 對其結構描述、工作流程或 vendor-specific 行為的反向依賴。

### 2.2 可重複使用的企業能力

- 承載可跨企業或跨產品重用的 business / technical 能力。
- MUST 具有明確契約、設定邊界、轉接器邊界與符合性 test。
- SHOULD 保持 provider-neutral、integration-neutral 或透過轉接器隔離具名供應商。
- 只有通過提升準則的內部能力 MAY 被提升到本層。

### 2.3 平台核心

- 承載跨產品穩定的治理語意、control contracts、能力組成、政策邊界、執行契約、身分 / 授權語意、可觀測性契約與轉接器 interfaces。
- MUST 不依賴 Company-specific 實作。
- MUST 不把客戶特定工作流程、結構描述、品牌、通道或供應商選擇視為核心前提。
- MAY 被多個商業產品共用。

### 2.4 商業產品/解決方案包裝

- 由 Platform Core、Reusable Enterprise Capabilities 與客戶 / deployment-specific 設定、adapters、UX、品牌、工作流程 package 組成。
- MAY 形成不同 SKU、industry 解決方案、single-tenant 或 multi-tenant 部署。
- MUST NOT 透過產品包裝改寫 Platform Core 的 Enterprise 治理語意。

## 3. 依賴方向

允許的主要依賴方向：

`Commercial Product / Solution` → `Reusable Enterprise Capability` → `Platform Core`

`Company-specific Reference Implementation` MAY 使用 `Reusable Enterprise Capability` 與 `Platform Core`，但 Platform Core MUST NOT 依賴 Company-specific 實作。

Customer-specific 轉接器 MAY 實作 Platform / Capability 所定義的 interface；interface 歸屬 MUST 位於可重用或平台層，而非客戶特定層。

## 4. 能力提升決策

Internal 能力要從 Company-specific 層提升為 Reusable Enterprise Capability，至少 MUST 滿足：

1. Use 案例不再依賴單一公司名稱、商品、資料表或工作流程。
2. Public / 內部契約已明確版本化。
3. Company-specific 對應可由設定或轉接器注入。
4. Provider / 通道 / 整合差異可被邊界隔離。
5. 有跨至少兩種實作設定檔的測試或可驗證替換證據。
6. Failure、稽核、授權與可觀測性語意不因替換部署而消失。
7. Architecture Review 確認沒有 reverse 相依性回滲 Platform Core。

## 5. 後果

### 積極

- 內部正式環境 usage 可以繼續快速迭代，而不必先完成完整 SaaS 化。
- 可逐步辨識真正值得抽離的通用能力。
- 第二家公司導入時可以替換領域、工作流程、資料對應、通道、品牌與整合。
- AEOS 可維持 Architecture / Governance 權限，而非被迫等同單一產品。

### 權衡

- Productization 需要額外契約、轉接器、設定與符合性 testing 成本。
- 並非所有內部 feature 都值得提升為可重用能力。
- 初期可能存在參考實作與可重用能力並行演進的重複成本。

## 6. Rejected 替代方案

### A. 直接把內部 CRM 複製成對外產品
拒絕。會將公司特定結構描述、工作流程與整合 assumption 固化為產品核心。

### B. 一開始就把所有內部 feature 泛化
拒絕。會增加不必要抽象化與開發成本，且缺少真實第二 use 案例驗證。

### C. 將 AEOS 本身直接定義為 SaaS Product
拒絕。AEOS 的首要角色是 Enterprise Architecture / Governance 權限；是否包裝為商業框架或平台產品應由後續產品策略決定。

## 7. 狀態與核准

本 ADR 目前為 **Approved 1.0.0**。

EWO-AEOS-0045 Review Package 已完成合併，本文正式作為內部參考實作、可重用能力、Platform Core 與商業產品封裝分離之 Architecture Decision。

核准後：

- 可作為後續產品化、能力提升與商業封裝架構的決策權威。
- AEOS-ARCH-014 依本文建立 Productizable Platform Architecture 的 Approved Architecture 定義。
- AEOS-ARCH-001 Approved Architecture Register 應登錄 AEOS-ARCH-014。

## 8. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0045 Post-Merge Closure Verification：將 Productization Boundary Decision 升級為 Approved Architecture Decision | ChatGPT |
| 0.1.0 | 2026-08-27 | 建立 Productization Boundary、四層 Productization Model、相依性方向與能力提升準則候選決策 | ChatGPT |
