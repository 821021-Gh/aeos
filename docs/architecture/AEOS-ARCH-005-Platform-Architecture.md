---
doc-id: AEOS-ARCH-005
doc-name: Platform Architecture
doc-type: Architecture
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-Architecture-0002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-004
---

# AEOS-ARCH-005 — Platform Architecture

## Executive Summary

本文件依 WA-001 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Platform Architecture，定義 Platform 的身分、邊界、責任、分類、關係、擁有權、生命週期及治理規則。Platform 是承載一組連貫 Capability 並協調多個 Repository 的穩定企業架構邊界，不等同於單一 Repository、產品、服務、部署環境或技術元件。本文件建立後續 Platform Catalog 的權威結構，但不在未經核准的情況下新增具名 Platform 條目。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-005 |
| 文件名稱 | Platform Architecture |
| 型別 | Architecture（Platform Architecture） |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-Architecture-0002、WA-001（Approved v1.0.0）、AEOS-ARCH-004（Approved v1.0.0） |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-STD-001～AEOS-STD-005 |

## 1. Purpose

本文件之目的為：

- 將 WA-001 的 Platform Topology 轉化為 AEOS 內可治理、可追溯的 Platform Architecture。
- 建立 Platform 的共同語言、識別規則與責任邊界。
- 定義 Platform 與 Workspace、Layer、Capability、Repository、Ownership 及 Dependency 的正式關係。
- 建立 Platform Catalog 的結構、准入條件與維護規則。
- 支援 Platform 的建立、變更、取代與退役，同時維持 Workspace 整體一致性。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Platform 定義、架構身分與必要屬性。
- Platform Boundary、Responsibility 與 Authority。
- Platform 分類與分類判定規則。
- Platform 之間及其與其他架構元素的關係。
- Platform Ownership 與治理責任。
- Platform Catalog 的權威角色與最小資料模型。
- Platform 生命週期、變更控制與合規要求。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改 WA-001 的 Platform Topology。
- 未經 WA-001 或正式 Architecture Review 核准的具名 Platform 清單。
- 個別 Platform 的內部技術架構、部署拓撲或實作設計。
- Capability Catalog、Repository Mapping、Ownership Matrix 或 Dependency Matrix 的實際條目。
- Product、Service、Application、Infrastructure 或 Runtime Component 的詳細分類。
- Platform Roadmap、預算、組織編制或交付排程。

## 3. Architecture Authority

Platform Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| P0 | WA-001 | Platform Topology 與 Workspace Architecture 的唯一來源 |
| P1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline |
| P2 | AEOS-ARCH-004 | 定義 Platform Architecture 在 Enterprise Architecture 中的定位 |
| P3 | AEOS-ARCH-005 | 定義 Platform 模型、邊界、關係與治理規則 |
| P4 | Platform Catalog | 登錄已核准 Platform 及其權威屬性 |
| P5 | Platform／Repository Architecture | 將 Platform 規則套用至具體架構與 Repository |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的 Platform Topology。
- Platform Catalog MUST 僅登錄經正式 Architecture Review 核准的 Platform。
- Platform 內部設計不得擴張其 Enterprise Boundary 或覆寫其他 Platform 的 Authority。
- 發現 WA-001 未涵蓋的 Platform 需求時，MUST 先透過正式架構變更處理。

## 4. Platform Definition

### 4.1 Formal Definition

Platform 是 AI Engineering Workspace 內具備持續身分、明確企業責任與治理權威的穩定架構邊界。Platform 承載一組具共同結果與治理需求的 Capability，協調一個或多個 Repository，並透過受治理的 Interface 與 Dependency 與其他 Platform 協作。

一個架構元素只有在同時符合下列條件時，才可被認定為 Platform：

- 具有可被唯一識別且跨版本延續的 Platform Identity。
- 具有明確的 Mission、Boundary、Responsibility 與 Authority。
- 承載至少一項正式 Capability，且其責任不只是單一交付項目。
- 具有明確 Platform Owner 與治理責任。
- 與 Repository、Dependency 及其他 Platform 的關係可被登錄與追溯。
- 生命週期由 Enterprise Architecture 管理，而非隨單一實作任意建立或移除。

### 4.2 Platform Is Not

| 架構元素 | 與 Platform 的區別 |
|----------|--------------------|
| Repository | Repository 是版本化治理與交付邊界；一個 Platform 可由一個或多個 Repository 支援 |
| Capability | Capability 描述 Workspace 能提供什麼；Platform 描述能力由何種穩定邊界承載與治理 |
| Product | Product 面向特定使用者或市場結果；Platform 可支援多個 Product，但不因單一 Product 自動成立 |
| Service／Application | Service 或 Application 是可部署或可使用的實作單位；Platform 是較高階的企業架構邊界 |
| Infrastructure | Infrastructure 提供運算、儲存或網路資源；只有符合 Platform 判定條件時才具有 Platform 身分 |
| Team／Organization | Team 是人員組織；Platform Ownership 可由 Team 承擔，但 Platform 不等同組織圖 |

## 5. Platform Identity Model

每個 Platform MUST 具備下列權威屬性：

| 屬性 | 必要性 | 定義 |
|------|--------|------|
| Platform ID | MUST | 全 Workspace 唯一且不可重複使用的穩定識別碼 |
| Platform Name | MUST | 正式名稱；命名變更不得改變 Platform ID |
| Mission | MUST | Platform 存在目的與預期企業結果 |
| Classification | MUST | 依 §7 指定之 Platform 類別 |
| Boundary | MUST | Platform 包含與排除的責任範圍 |
| Authority | MUST | Platform 可作出決策與治理的範圍 |
| Platform Owner | MUST | 對 Platform 完整性與演進負責的角色 |
| Capabilities | MUST | Platform 承載之已核准 Capability 引用 |
| Repositories | MUST | 支援 Platform 的 Repository 引用；可為一個或多個 |
| Interfaces | MUST | 對其他 Platform 或 Workspace 暴露的正式互動邊界 |
| Dependencies | MUST | 依 Dependency Architecture 管理的正式依賴引用 |
| Lifecycle Status | MUST | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | MUST | 核准 Platform 身分與邊界的 Architecture／ADR |

Platform ID、Mission 或 Boundary 的實質變更 MUST 經 Architecture Review；不得只修改 Catalog 條目完成架構變更。

## 6. Platform Boundary and Responsibility

### 6.1 Boundary Dimensions

Platform Boundary MUST 同時從下列面向定義：

| 面向 | 必須回答的問題 |
|------|----------------|
| Responsibility Boundary | Platform 對哪些企業結果與能力負責？ |
| Authority Boundary | Platform 可制定哪些規則、介面與內部決策？ |
| Capability Boundary | 哪些 Capability 由 Platform 承載，哪些明確不屬於它？ |
| Repository Boundary | 哪些 Repository 支援 Platform，各自承擔何種角色？ |
| Interface Boundary | 哪些互動必須透過正式 Interface 發生？ |
| Dependency Boundary | Platform 可接受、提供或禁止哪些依賴？ |
| Information Boundary | 哪些架構資訊由 Platform 擁有、提供或僅引用？ |

### 6.2 Boundary Rules

- 每項 Platform Responsibility MUST 能對應至 Mission 與至少一項 Capability。
- 同一責任不得由多個 Platform 在無 Ownership 決議的情況下同時宣稱最終 Authority。
- Platform MAY 委派實作責任，但 MUST 保留其架構責任與可追溯性。
- Repository 位於 Platform Boundary 內，不代表其全部內容皆由 Platform 擁有；實際責任以 Repository Architecture 與 Mapping 為準。
- Shared Capability MUST 具有單一 accountable Owner，並明確記錄其他 Platform 的使用或協作關係。
- Platform MUST NOT 直接管理超出其 Boundary 的 Repository、Capability 或架構決策。

## 7. Platform Classification

Platform 分類用於描述其主要企業責任，不代表技術堆疊或組織層級。

| 類別 | 核心責任 | 判定原則 |
|------|----------|----------|
| Enterprise Platform | 提供跨 Workspace 的企業架構、治理或共同控制能力 | Authority 橫跨多個 Platform，且責任不可由單一領域取代 |
| Engineering Platform | 提供工程交付、品質、標準化或開發生命週期能力 | 主要結果服務多個 Repository 的工程活動 |
| Business Platform | 提供跨產品或跨流程的企業業務能力 | 主要結果為可重用的業務運作或管理能力 |
| Product Platform | 提供一組面向產品或客戶結果的共同能力 | 支援一個或多個相關 Product，並維持共同平台邊界 |
| Shared Service Platform | 提供可被多個 Platform 重用的共同服務能力 | 具獨立生命週期、Owner 與正式服務介面 |

分類規則：

- 每個 Platform MUST 指定一個 Primary Classification。
- Platform MAY 記錄 Secondary Characteristics，但不得以多重分類模糊主要責任。
- 分類不得單憑 Repository 名稱、部署方式、供應商產品或組織單位決定。
- 無法清楚分類通常代表 Boundary 尚未完成，MUST 在登錄前解決。

## 8. Platform Relationship Model

### 8.1 Relationship Types

| 關係 | 語意 | 要求 |
|------|------|------|
| Governs | 一個 Platform 對另一 Platform 的特定治理面向具有正式權威 | MUST 限定治理範圍，不得推定全面控制 |
| Enables | 一個 Platform 提供另一 Platform 所需的能力或基礎 | MUST 對應正式 Capability 與 Interface |
| Consumes | 一個 Platform 使用另一 Platform 提供的能力 | MUST 記錄 Dependency 與責任邊界 |
| Collaborates With | 多個 Platform 共同完成結果，但維持各自 Authority | MUST 明確指定各方責任與 accountable Owner |
| Constrains | 一個 Platform 透過企業規則限制另一 Platform 的允許行為 | MUST 具有上位 Architecture 或 Policy 依據 |
| Supersedes | 新 Platform 正式取代既有 Platform 的責任 | MUST 具有 Migration、替代關係與退役計畫 |

### 8.2 Relationship Rules

- Platform Relationship MUST 有方向、類型、Owner、依據與生命週期狀態。
- `Enables` 不等同 `Governs`；能力提供者不得由技術依賴推定治理權威。
- `Consumes` 不轉移 Capability Ownership 或 Platform Accountability。
- `Collaborates With` 不得用來掩蓋責任重疊。
- 跨 Platform 的實際 Dependency MUST 登錄於 Dependency Matrix。
- 循環治理關係 MUST NOT 被允許；循環技術或服務依賴必須由 Dependency Architecture 明確評估。

## 9. Platform Interfaces

Platform Interface 是 Platform 對外提供或接受 Capability、資訊、治理要求或協作責任的正式邊界。

每個 Interface SHOULD 記錄：

- Interface ID 與名稱。
- Provider Platform 與 Consumer Platform。
- 支援的 Capability 或治理目的。
- Interface Owner。
- 輸入、輸出與責任界線。
- 適用 Contract、Policy、Specification 或 ADR。
- 相依性、版本與生命週期狀態。

規則：

- Platform 間互動 MUST 優先透過已定義 Interface，而非直接耦合內部實作。
- Interface 只公開履行責任所需內容，MUST NOT 洩漏不必要的內部結構。
- Interface 變更 MUST 評估所有 Consumer Platform、Repository 與 Capability 的影響。
- Breaking Change MUST 具有版本策略、Migration Path 與核准紀錄。

## 10. Platform Ownership

### 10.1 Roles

| 角色 | 責任 |
|------|------|
| Architecture Owner | 維護跨 Platform 一致性、裁決邊界衝突及核准重大變更 |
| Platform Owner | 對 Platform Mission、Boundary、Capability、Interface 與生命週期負最終責任 |
| Capability Owner | 維護 Platform 所承載 Capability 的定義、結果及演進 |
| Repository Owner | 確保支援 Repository 符合 Platform 與 Enterprise Architecture |
| Dependency Owner | 維護跨 Platform Dependency 的必要性、風險與相容性 |
| Review Owner | 依 AEOS-STD-005 確認 Platform Architecture 變更已完成正式 Review |

### 10.2 Accountability Rules

- 每個 Active Platform MUST 有且只有一個 accountable Platform Owner 角色。
- Platform Owner 可委派執行工作，但 MUST NOT 委派最終 Accountability。
- Platform Owner 與 Repository Owner 不必為同一角色；兩者責任 MUST 分別記錄。
- 跨 Platform Capability MUST 在 Ownership Matrix 中明確區分 accountable、responsible、consulted 與 informed 關係。
- Ownership 缺失、重疊或無法履行時，Platform MUST NOT 進入 Active 狀態。

## 11. Platform Catalog

### 11.1 Catalog Authority

Platform Catalog 是 AI Engineering Workspace 已核准 Platform 的權威登錄來源。Catalog 記錄架構事實，不創造架構事實；Platform 的身分與邊界必須先由 Architecture 或 ADR 核准。

### 11.2 Minimum Record

每個 Catalog 條目 MUST 至少包含：

| 欄位 | 內容 |
|------|------|
| Platform ID／Name | 唯一識別碼與正式名稱 |
| Mission／Classification | 存在目的與主要分類 |
| Boundary／Authority | 責任範圍與決策權威摘要 |
| Owner | accountable Platform Owner |
| Capability References | 已核准 Capability 引用 |
| Repository References | 支援 Repository 引用 |
| Interface／Dependency References | 對外介面與依賴引用 |
| Lifecycle Status | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | 核准此條目的 Architecture／ADR |
| Version／Review Date | 條目版本與最近審查日期 |

### 11.3 Catalog Rules

- Platform Catalog MUST NOT 登錄缺少 Architecture Reference 的 Platform。
- Catalog 變更 MUST 與相關 Architecture、Capability Catalog、Ownership Matrix 及 Dependency Matrix 保持一致。
- 名稱變更 MUST 保留 Platform ID；退役 Platform ID MUST NOT 重複使用。
- Catalog MUST 保留 Deprecated 與 Retired Platform 的歷史及替代關係。
- Catalog 條目與 Platform Architecture 衝突時，以正式 Architecture 為準並啟動修正。

## 12. Platform Lifecycle

| 狀態 | 定義 | 必要條件 |
|------|------|----------|
| Candidate | 已提出但尚未成為正式 Platform | 具 Mission、初步 Boundary、提案 Owner 與 Architecture Reference |
| Active | 已核准並承擔正式 Workspace 責任 | 完整 Identity、Owner、Capability、Repository、Interface 與 Dependency 記錄 |
| Deprecated | 仍受支援但不得承接新的策略性責任 | 具替代方向、Migration Plan、期限與受影響項目 |
| Retired | 已停止承擔正式責任 | Capability、Repository、Interface 與 Dependency 已移轉或終止 |

允許的主要狀態轉移為：

- Candidate → Active：完成 Architecture Review 並登錄 Catalog。
- Candidate → Retired：提案撤回或核准不成立，保留決策紀錄。
- Active → Deprecated：核准取代或合併策略並建立 Migration Plan。
- Deprecated → Active：取代決策撤銷且重新完成 Architecture Review。
- Deprecated → Retired：Migration 完成且無未處理責任或依賴。

任何跳過 Deprecated 的 Active → Retired 轉移 MUST 具有緊急理由、影響分析與 Architecture Owner 核准。

## 13. Change and Evolution

下列變更屬於 Architecture Change，MUST 經 EWO 與 Architecture Review：

- 建立、合併、拆分、取代或退役 Platform。
- 變更 Mission、Primary Classification、Boundary 或 Authority。
- 移轉關鍵 Capability 或 Platform Ownership。
- 新增或移除重大跨 Platform Relationship／Interface。
- 引入 Breaking Dependency 或改變依賴方向。

每項變更 MUST：

1. 說明原因、預期結果與不變範圍。
2. 識別受影響 Platform、Capability、Repository、Owner、Interface 與 Dependency。
3. 評估責任重疊、孤立能力、循環依賴及相容性風險。
4. 定義 Migration、Rollback、版本與生命週期策略。
5. 更新 Architecture、Catalog 與相關 Matrix。
6. 依重大程度建立或引用 ADR。

## 14. Compliance

Platform Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Platform 可追溯至 WA-001、AEOS-ARCH-001 與 AEOS-ARCH-004 |
| Identity | 具唯一、穩定且不可重用的 Platform ID |
| Boundary | Mission、責任、Authority、包含與排除範圍明確 |
| Classification | 具有單一 Primary Classification 且判定依據成立 |
| Capability | 每項 Platform Responsibility 可追溯至正式 Capability |
| Repository | 支援 Repository 與角色已識別，但未將 Repository 等同 Platform |
| Ownership | 具有唯一 accountable Platform Owner，無未解決責任重疊 |
| Interface | 跨 Platform 互動具有正式 Interface 與 Owner |
| Dependency | 依賴方向、類型、風險與 Owner 可追溯 |
| Lifecycle | 狀態、轉移條件、Migration 與替代關係完整 |
| Asset Consistency | Architecture、Platform Catalog 與相關 Matrix 一致 |

不符合項目 MUST 在進入 Active 狀態或核准重大變更前完成修正；不得以 Catalog 註記取代架構修正。

## 15. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Workspace Architecture 與 Platform Topology 的唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved v1.0.0） | Architecture | 將 WA-001 正式納入 AEOS |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Platform Architecture 的上位定位與關係模型 |
| REF-004 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved v1.0.0） | Architecture | Enterprise、Platform 與 Capability 治理關係 |
| REF-005 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved v1.0.0） | Architecture | 重大 Platform 決策紀錄機制 |
| REF-006 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-007 | EWO-AEOS-Architecture-0002 | EWO | 本文件之工作來源 |

## 16. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Platform Identity、Boundary、Classification、Relationship、Interface、Ownership、Catalog、Lifecycle、Change 與 Compliance（EWO-AEOS-Architecture-0002） | Codex |
