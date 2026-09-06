---
doc-id: AEOS-ARCH-014
doc-name: Productizable Platform Architecture
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
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

## 執行摘要

本文件定義 AEOS 的 Productizable Platform Architecture，建立從內部正式環境參考實作演進至可重用 enterprise 能力、平台核心與商業產品 / 解決方案封裝的正式邏輯邊界。

核心原則是：**先以真實內部需求驗證能力，再透過明確提升準則抽離可重用部分；不得為了商品化而提前泛化所有功能，也不得讓公司特定邏輯回滲 Platform Core。**

本架構不規定商業模式、定價、SKU、multi-tenant 或 single-tenant 的最終選型；它規範的是在任何產品化路徑下都必須維持的相依性、契約、設定、轉接器與治理不變量。

本文件目前為 Approved 1.0.0，應納入 `AEOS-ARCH-001` Approved Architecture Register，作為 Productizable Platform Architecture 的正式定義載體。

## 1. 目的

本文件之目的為：

- 定義內部參考實作與 productizable 平台的正式邏輯邊界。
- 防止單一公司領域邏輯、結構描述、工作流程、通道或供應商 assumption 污染 Platform Core。
- 建立可重用能力提升準則。
- 定義商業產品如何組合 Platform Core、Reusable Capabilities 與客戶特定設定 / adapters。
- 使第二家公司導入時可以更換領域、工作流程、資料對應、通道、品牌、身分、整合與基礎設施設定檔，而不重寫平台核心治理語意。
- 明確區分 AEOS Architecture / Governance IP 與對外商品本身。

## 2.範圍

### 2.1 在範圍內

- 產品化邏輯層。
- 內部參考實作邊界。
- 可重複使用的能力提升/降級標準。
- 平台核心責任邊界。
- Customer / deployment-specific 設定與轉接器隔離。
- Product 組成與封裝不變量。
- Dependency 方向與 anti-coupling 規則。
- Contract 歸屬、versioning 與 compatibility 要求。
- Conformance 驗證與 portability 證據。
- 與 Agent Architecture、Platform、Layer、Capability、Dependency、Workspace Architecture 的對應。

### 2.2 超出範圍

- 定價、授權、銷售、行銷、SKU 策略。
- 最終 multi-tenant / single-tenant 部署決策。
- Production 基礎設施 sizing、network topology、KMS 或憑證 provisioning。
- 任何具名 CRM、ERP、Agent 框架、LLM、資料庫、雲端或整合供應商選擇。
- 客戶 onboarding 實作、migration project 或正式環境 rollout。

## 3. 架構權威

|權威|角色 |
|---|---|
| AEOS-ARCH-001 | Architecture Baseline 與正式架構入口 |
| AEOS-ARCH-005 | Platform 邊界與 product-neutral 平台身分 |
| AEOS-ARCH-006 | Layer 責任、相依性方向與 anti-bypass 規則 |
| AEOS-ARCH-007 | Capability-first definition 與實作分離 |
| AEOS-ARCH-009 |依賴治理 |
| AEOS-ARCH-010 |工作區/儲存庫執行邊界 |
| AEOS-ARCH-012 | Architecture Principles |
| AEOS-ARCH-013 | Agent Control Plane / Execution Plane、執行環境／Harness／供應商中立 |
| AEOS-ADR-004 |產品化邊界 Approved 決策|

## 4. 產品化模式

### 4.1 公司特定參考實作

此層代表單一企業、單一品牌或單一部署的真實正式環境實作。

可包含：

- 公司特定的領域模型；
- 產品/服務知識；
- CRM / ERP 欄位對應；
- 工作流程與核准設定檔；
- 通路行為；
- 客戶特定的提示/政策；
- 品牌、UX 設定；
- 特定於整合的對應；
- 特定於部署的基礎架構設定。

規則：

- MAY 使用 Platform Core 與 Reusable Enterprise Capabilities。
- MAY 作為新能力的驗證來源。
- MUST NOT 成為 Platform Core 的相依性 target。
- MUST NOT 將單一公司結構描述、供應商 API、通道 event format 或工作流程 hard-code 為平台核心契約。

### 4.2 可重複使用的企業能力

此層代表經過抽離、可跨公司或跨產品重用的能力。

最低要求：

- 穩定、版本化契約；
- 明確 input / 輸出 / error 語意；
- 設定邊界；
- 轉接器/供應商隔離；
- 授權、稽核、可觀察性要求；
- 一致性測試；
- 不依賴單一 company 身分或單一客戶結構描述。

典型候選可包括客戶記憶體、conversation 編排、human takeover、核准編排、稽核證據、工具存取 control、整合轉接器 pattern，但任何具體能力是否正式列入 Catalog 仍需獨立 Review / Approval。

### 4.3 平台核心

Platform Core 承載跨產品穩定且具 Enterprise 權限的結構，包括：

- 治理語意；
- 政策/核准/授權合約；
- 能力構成規則；
- 執行/控制合約；
- 身分/範圍/撤銷語意；
- 稽核/可觀察性合約；
- 轉接器/供應商介面所有權；
- compatibility 與生命週期規則。

平台核心：

- MUST 不依賴 Company-specific Reference Implementation。
- MUST 不要求單一客戶結構描述、工作流程、通道或供應商才能成立。
- MUST 維持 product-neutral、provider-neutral 或透過轉接器隔離具名實作。
- SHOULD 允許多個商業產品 / 解決方案共用。

### 4.4 商業產品/解決方案包裝

商業產品層負責把平台能力形成客戶可購買、部署與使用的解決方案。

典型組成：

`Platform Core + Reusable Capabilities + Product UX + Customer Configuration + Adapters + Deployment Profile`

規則：

- MAY 形成不同 vertical 解決方案、edition、SKU 或部署設定檔。
- MAY 針對產業預設工作流程 / 結構描述設定檔。
- MUST 透過設定 / 擴充 / 轉接器邊界客製化。
- MUST NOT 以 fork Platform Core 治理語意作為正常客製方式。

## 5. 依賴規則

### 5.1 允許方向

主要依賴方向：

`Commercial Product / Solution` → `Reusable Enterprise Capability` → `Platform Core`

`Company-specific Reference Implementation` → `Reusable Enterprise Capability` / `Platform Core`

### 5.2 禁止的反向依賴

下列依賴 MUST NOT 發生：

- 平台核心→公司特定的資料庫架構；
- 平台核心→公司特定工作流程；
- 平台核心→公司特定品牌/產品知識；
- 平台核心→單一客戶整合實作；
- 可重複使用能力合約→單一客戶欄位名稱；
- 企業治理語意→商業 SKU/UI 行為。

### 5.3 介面所有權

若客戶特定轉接器實作某 interface：

- interface / 契約歸屬 MUST 位於 Platform Core 或 Reusable Capability 層；
- 轉接器實作 MAY 位於 company / 產品層；
- adapter-specific 設定 MUST NOT 改寫 interface 語意。

## 6. 能力提升管道

Company-specific 能力提升為 Reusable Enterprise Capability 應依序通過：

### P1 — 證據
確認能力已在真實 use 案例中產生穩定價值，而不是純假設抽象。

### P2 — 解耦
移除 company name、產品 knowledge、客戶結構描述、工作流程、通道與 vendor-specific hard 相依性。

### P3 — 合約
建立版本化契約、error 模型、授權 / 稽核語意與生命週期。

### P4 — 轉接器/設定隔離
將客戶 / 供應商差異移入轉接器、設定、政策設定檔或擴充 point。

### P5 — 可攜性驗證
至少以兩種實作 / 部署設定檔驗證核心契約不需修改。

### P6 — 架構回顧
確認相依性方向、權限邊界、security、可觀測性與失敗行為均符合 AEOS。

### P7 — 目錄/基線入場
若需成為正式 Enterprise Capability 或 Platform 事實，再依既有 Catalog / Architecture 治理流程獨立登錄。

## 7. 商業化邊界不變量

無論產品最終採 SaaS、single-tenant、managed 服務、on-premise 或 hybrid 部署，以下不變量 MUST 維持：

1. Enterprise 治理權限不因產品封裝改變。
2. Company-specific 邏輯不得反向成為 Platform Core 相依性。
3. Customer-specific 整合必須透過轉接器 / 擴充邊界接入。
4. Configuration 不得用來繞過授權、核准、稽核或 security 政策。
5. Provider / 執行環境替換不得破壞核心治理語意。
6. 產品特定 UX 不得成為核心能力契約的唯一入口。
7. Audit 證據、身分、授權範圍與撤銷語意必須在產品邊界上保持可追蹤。

## 8. 與 Enterprise AI Agent Architecture 的關係

`AEOS-ARCH-013` 已規定 Agent Control Plane、Agent Execution Plane、Harness / Runtime Neutral 與 Provider Adapter Boundary。

本架構補充其產品化邊界：

- Agent Harness / Runtime 實作 MAY 存在於公司特定或 product-specific 部署設定檔。
- Agent Execution Contract、治理語意與核心授權 / 核准規則 SHOULD 保持於 Platform Core。
- 可重用 Agent 能力 MAY 依 Promotion Pipeline 提升為 Reusable Enterprise Capability。
- Commercial Product MAY 組合不同 Harness / Runtime / 模型 / 工具供應商，但不得因此取得或修改 Enterprise 治理權限。

## 9. 參考實作規則

Internal 正式環境 system SHOULD 被視為 **參考實作**，而非自動等同 future 商業產品。

Reference 實作的角色是：

- 驗證能力是否真正需要；
- 提供失敗 / 邊界案例證據；
- 驗證契約與治理；
- 發現哪些 concern 必須留在客戶特定層；
- 為提升提供實證。

只有經過 Promotion Pipeline 的能力，才應被視為可進入可重用 / 平台層的候選。

## 10. 一致性檢查表

任何宣稱可商品化或可重用的能力 SHOULD 至少回答：

- 是否仍引用單一公司名稱、結構描述、工作流程、通道或供應商？
- 是否有版本化契約？
- 客戶特定對應是否可外置？
- 供應商 / 整合是否可替換？
- 授權 / 核准 / 稽核語意是否穩定？
- 是否有 portability / substitution 證據？
- 是否存在 Platform Core → 公司特定 reverse 相依性？
- 第二家公司導入是否需要修改 Platform Core？若需要，原因是否經 Architecture Review 接受？

## 11. 生命週期

- 本文件目前為 Approved 1.0.0，作為 Productizable Platform Architecture 的正式架構定義。
- 後續產品化經驗若改變邊界，必須透過正式 EWO / Review / Amendment 修改。
- 任何可重用能力或 Platform 事實的具名准入，仍須依 Catalog / Architecture 治理流程獨立核准。

## 12. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-27 | 依 EWO-AEOS-0045 Post-Merge Closure Verification：升級為 Approved Productizable Platform Architecture | ChatGPT |
| 0.1.0 | 2026-08-27 | 建立 Productizable Platform Architecture：四層 Productization Model、相依性方向、提升 pipeline、commercialization 不變量與 Agent Architecture 對應 | ChatGPT |
