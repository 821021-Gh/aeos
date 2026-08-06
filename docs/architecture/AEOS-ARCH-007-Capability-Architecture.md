---
doc-id: AEOS-ARCH-007
doc-name: Capability Architecture
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0014
  - EWO-AEOS-0015
  - AR-AEOS-0015-R1
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
---

# AEOS-ARCH-007 — Capability Architecture

## Executive Summary

本文件依 WA-001 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Capability Architecture，定義 Capability 的識別、邊界、分類、關係、Ownership、Dependency、Lifecycle 與治理規則。Capability 以能力責任與企業結果描述，不以特定實作取代能力定義；Platform 提供 Capability 的組織與運作邊界，Repository 實現 Capability，Ownership Matrix 記錄責任歸屬。本文件不重新設計 WA-001、不建立具名 Capability Catalog、不建立實作設計，也不取代任何專項架構或治理文件。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-007 |
| 文件名稱 | Capability Architecture |
| 型別 | Architecture（Capability Architecture） |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0014、EWO-AEOS-0015、AR-AEOS-0015-R1、WA-001（Approved v1.0.0）、AEOS-ARCH-001（Approved v1.0.0）、AEOS-ARCH-004（Approved v1.0.0）、AEOS-ARCH-005（Approved v1.0.0）、AEOS-ARCH-006（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0015、AR-AEOS-0015-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-ARCH-006、AEOS-STD-001～AEOS-STD-005 |

## 1. Purpose

本文件之目的為：

- 將 WA-001 的 Capability Architecture 與 Capability Ownership 轉化為 AEOS 內可治理、可追溯的 Capability Architecture。
- 以能力責任與企業結果描述 Capability，不以特定實作取代能力定義（AEOS-ARCH-004 §6.3）。
- 定義 Capability 之邊界、關係、依賴與生命週期（AEOS-ARCH-004 §6.3）。
- 定義 Capability 的識別、分類、Ownership 與治理規則，使每項 Capability 具有明確責任歸屬（AEOS-ARCH-004 §7）。
- 為後續 Capability Catalog 與 Ownership Matrix 提供正式架構依據（AEOS-ARCH-004 §6.3、§8）。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Capability 之正式定義、架構身分與必要屬性。
- Capability Boundary、Responsibility 與 Authority。
- Capability 分類與分類判定規則。
- Capability 之間及其與 Platform、Repository、Ownership、Dependency 之關係。
- Capability Ownership 與治理責任。
- Capability 生命週期、變更控制與合規要求。
- Capability Catalog 與 Ownership Matrix 之定位（不建立條目）。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改 WA-001 的 Capability Architecture 或 Capability Ownership。
- 具名 Capability 條目與 Capability Catalog 之建立（Catalog 應作為後續獨立架構資產）。
- Ownership Matrix 之實際條目。
- 個別 Platform、Repository 或實作之內部設計。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。
- Capability Management Framework 之操作化（屬 AEOS-GOV-001 §7 Planned Frameworks，後續 EWO）。

## 3. Architecture Authority

Capability Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| C0 | WA-001 | Capability Architecture 與 Capability Ownership 的唯一來源 |
| C1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline |
| C2 | AEOS-ARCH-004 | 定義 Capability Architecture 在 Enterprise Architecture 中的定位與 MUST |
| C3 | AEOS-ARCH-005、AEOS-ARCH-006 | 定義 Platform 承載 Capability 與 Layer 責任分層（L4 Capability） |
| C4 | AEOS-ARCH-007（本文件） | 定義 Capability 模型、邊界、關係與治理規則 |
| C5 | Capability Catalog、Ownership Matrix、Repository Architecture | 登錄已核准 Capability 事實並落實 |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的 Capability 或改寫 WA-001 已核准內容。
- Capability 與 Platform 必須分離定義：Platform 是承載與治理邊界，Capability 是可提供之能力（AEOS-ARCH-004 §7）。
- 發現 WA-001 未涵蓋的 Capability 需求時，MUST 先透過正式架構變更處理。

## 4. Capability Definition

### 4.1 Formal Definition

Capability 是 AI Engineering Workspace 具備之能力，以能力責任與企業結果描述，不預先指定如何實作。Capability 描述 Workspace 能提供什麼；Platform 描述能力由何種穩定邊界承載與治理（AEOS-ARCH-005 §4.2）。

一個架構元素只有在同時符合下列條件時，才可被認定為 Capability：

- 具有可被唯一識別且跨版本延續的 Capability ID。
- 具有明確的企業結果、責任與治理邊界。
- 以能力責任與企業結果描述，不以特定實作、Repository 或技術元件定義。
- 具有明確 Capability Owner 與治理責任。
- 與 Platform、Repository、Ownership 及 Dependency 之關係可被登錄與追溯。
- 生命週期由 Enterprise Architecture 管理，而非隨單一實作任意建立或移除。

### 4.2 Capability Is Not

| 架構元素 | 與 Capability 的區別 |
|----------|----------------------|
| Platform | Platform 是承載一組 Capability 的穩定企業架構邊界；Capability 描述能力本身 |
| Repository | Repository 是版本化治理與交付邊界；Repository 實現 Capability，但不等同 Capability |
| Product | Product 面向特定使用者或市場結果；Capability 描述 Workspace 能提供什麼 |
| Service／Application | Service 或 Application 是可部署或可使用的實作單位；Capability 是能力責任 |
| 技術元件／功能 | 特定功能或元件是實作細節；Capability 不以實作取代能力定義 |
| Team／Organization | Team 是人員組織；Capability Ownership 可由 Team 承擔，但 Capability 不等同組織圖 |

## 5. Capability Identity Model

每個 Capability MUST 具備下列權威屬性：

| 屬性 | 必要性 | 定義 |
|------|--------|------|
| Capability ID | MUST | 全 Workspace 唯一且不可重複使用的穩定識別碼 |
| Capability Name | MUST | 正式名稱；命名變更不得改變 Capability ID |
| Outcome | MUST | 能力預期達成之企業結果 |
| Classification | MUST | 依 §7 指定之 Capability 分類 |
| Boundary | MUST | Capability 包含與排除的責任範圍 |
| Owner | MUST | 對 Capability 完整性與演進負責的 accountable Owner |
| Platform Reference | MUST | 承載此 Capability 之已核准 Platform 引用 |
| Repository References | MUST | 實現此 Capability 之 Repository 引用；可為一個或多個 |
| Dependencies | MUST | 依 Dependency Architecture 管理之正式依賴引用 |
| Lifecycle Status | MUST | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | MUST | 核准 Capability 身分與邊界的 Architecture／ADR |

Capability ID、Outcome 或 Boundary 的實質變更 MUST 經 Architecture Review；不得只修改 Catalog 條目完成架構變更。

## 6. Capability Boundary and Responsibility

### 6.1 Boundary Dimensions

Capability Boundary MUST 同時從下列面向定義：

| 面向 | 必須回答的問題 |
|------|----------------|
| Outcome Boundary | Capability 預期達成哪些企業結果？ |
| Responsibility Boundary | Capability 對哪些責任負責？ |
| Capability Boundary | 哪些子能力或能力群組屬於本 Capability，哪些明確不屬於它？ |
| Platform Boundary | 由哪些 Platform 承載，哪些明確不屬於承載範圍？ |
| Repository Boundary | 哪些 Repository 實現此 Capability，各自承擔何種角色？ |
| Dependency Boundary | Capability 可接受、提供或禁止哪些依賴？ |
| Information Boundary | 哪些架構資訊由 Capability 擁有、提供或僅引用？ |

### 6.2 Boundary Rules

- 每項 Capability Responsibility MUST 能對應至 Outcome 與至少一項可驗證之企業結果。
- 同一責任不得由多個 Capability 在無 Ownership 決議的情況下同時宣稱最終 Authority。
- Capability MAY 委派實現責任，但 MUST 保留其能力責任與可追溯性。
- Shared Capability MUST 具有單一 accountable Owner，並明確記錄其他 Platform 的使用或協作關係（AEOS-ARCH-005 §6.2）。
- Capability MUST NOT 直接管理超出其 Boundary 的 Platform、Repository 或架構決策。

## 7. Capability Classification

Capability 分類用於建立治理、比較與演進決策之共用語言；分類不決定技術堆疊或組織層級。

### 7.1 Classification Dimensions

每個 Capability 依下列維度分類：

| 維度 | 定義 | 值域來源 |
|------|------|----------|
| 治理領域 | Capability 主要服務之治理領域 | AEOS-ARCH-002 §4（Architecture、Documentation、Platform、Capability、Repository、Decision） |
| 架構歸屬 | Capability 於 Enterprise Architecture 之歸屬 | AEOS-ARCH-004 §5（Platform、Layer、Capability、Repository、Dependency、Workspace） |
| 責任層級 | Capability 對應之架構責任層級 | AEOS-ARCH-006 §4.2（L1 Governance～L6 Implementation） |

### 7.2 Classification Rules

- 每個 Capability MUST 指定單一 Primary Classification。
- 分類判定 MUST 以能力責任與企業結果為依據，MUST NOT 單憑 Repository 名稱、部署方式、供應商產品或組織單位決定。
- 具名分類類別 MUST 可追溯至 WA-001 或經 Architecture Review 核准；本文件不建立具名類別清單。
- 無法清楚分類通常代表 Boundary 尚未完成，MUST 在登錄前解決。

## 8. Capability Relationship Model

### 8.1 Relationship Types

| 關係 | 語意 | 要求 |
|------|------|------|
| Carries | Platform 承載 Capability，提供組織與運作邊界 | MUST 對應正式 Platform 與 Interface |
| Composes | Capability 由子能力或能力群組組成 | MUST 明確組合邊界與組成責任 |
| Supports | Repository 實現或支援 Capability | MUST 記錄 Repository 角色與責任 |
| Enables | 一個 Capability 提供另一 Capability 所需之能力或基礎 | MUST 對應正式 Outcome 與 Dependency |
| Consumes | 一個 Capability 使用另一 Capability 提供之能力 | MUST 記錄 Dependency 與責任邊界 |
| Governs | 一個 Capability 對另一 Capability 之特定治理面向具有正式權威 | MUST 限定治理範圍，不得推定全面控制 |
| Supersedes | 新 Capability 正式取代既有 Capability 之責任 | MUST 具有替代關係、Migration 與退役計畫 |

### 8.2 Relationship Rules

- Capability Relationship MUST 有方向、類型、Owner、依據與生命週期狀態。
- `Carries` 不等同 `Governs`；Platform 承載能力不得由技術依賴推定治理權威。
- `Consumes` 不轉移 Capability Ownership 或 Platform Accountability。
- `Composes` 不得用來掩蓋責任重疊或繞過單一 accountable Owner。
- 跨 Capability 之實際 Dependency MUST 登錄於 Dependency Matrix。
- 循環治理關係 MUST NOT 被允許；循環技術或服務依賴必須由 Dependency Architecture 明確評估。

## 9. Capability Dependency

### 9.1 Dependency Definition

Capability Dependency 是 Capability 之間、或 Capability 與 Platform／Repository／Layer 之間之正式依賴關係。依賴 MUST 以能力責任與企業結果為基礎描述，不以特定實作或技術套件取代。

### 9.2 Dependency Rules

- 每項 Capability Dependency MUST 有方向、類型、Owner、依據與生命週期狀態。
- Dependency MUST 由 Dependency Architecture 定義並登錄於 Dependency Matrix；不得只存在於非正式敘述或實作中（AEOS-ARCH-004 §7）。
- 依賴方向 MUST 符合 Layer Architecture 之分層規則（AEOS-ARCH-006 §5.2）：上位約束、下位依賴；禁止反向控制。
- 新增、移除或變更重大 Dependency MUST 經 Architecture Change（§12）。
- 循環依賴 MUST 由 Dependency Architecture 明確評估，未經核准不得建立。

## 10. Capability Ownership

### 10.1 Roles

| 角色 | 責任 |
|------|------|
| Architecture Owner | 維護 Capability 與 Enterprise Architecture 一致性、裁決邊界衝突及核准重大變更 |
| Capability Owner | 對 Capability Outcome、Boundary、依賴與生命週期負最終責任 |
| Platform Owner | 維護承載 Platform 之邊界與 Platform Catalog 條目一致性 |
| Repository Owner | 確保實現 Repository 符合 Capability 與 Enterprise Architecture |
| Dependency Owner | 維護跨 Capability Dependency 之必要性、風險與相容性 |
| Review Owner | 依 AEOS-STD-005 確認 Capability 變更已完成正式 Review |

### 10.2 Accountability Rules

- 每個 Active Capability MUST 有且只有一個 accountable Capability Owner 角色。
- Capability Owner 可委派執行工作，但 MUST NOT 委派最終 Accountability。
- 跨 Platform 或跨 Repository 之 Capability MUST 在 Ownership Matrix 中明確區分 accountable、responsible、consulted 與 informed 關係。
- Ownership 缺失、重疊或無法履行時，Capability MUST NOT 進入 Active 狀態。

## 11. Capability Lifecycle

| 狀態 | 定義 | 必要條件 |
|------|------|----------|
| Candidate | 已提出但尚未成為正式 Capability | 具 Outcome、初步 Boundary、提案 Owner 與 Architecture Reference |
| Active | 已核准並承擔正式 Workspace 責任 | 完整 Identity、Owner、Platform、Repository、Dependency 與 Boundary 記錄 |
| Deprecated | 仍受支援但不得承接新的策略性責任 | 具替代方向、Migration Plan、期限與受影響項目 |
| Retired | 已停止承擔正式責任 | 相關 Outcome、依賴與 Repository 已移轉或終止 |

允許的主要狀態轉移為：

- Candidate → Active：完成 Architecture Review 並登錄 Capability Catalog。
- Candidate → Retired：提案撤回或核准不成立，保留決策紀錄。
- Active → Deprecated：核准取代或合併策略並建立 Migration Plan。
- Deprecated → Active：取代決策撤銷且重新完成 Architecture Review。
- Deprecated → Retired：Migration 完成且無未處理責任或依賴。

任何跳過 Deprecated 的 Active → Retired 轉移 MUST 具有緊急理由、影響分析與 Architecture Owner 核准。

## 12. Change and Evolution

下列變更屬於 Architecture Change，MUST 經 EWO 與 Architecture Review：

- 建立、合併、拆分、取代或退役 Capability。
- 變更 Outcome、Primary Classification、Boundary 或 Ownership。
- 移轉關鍵 Capability 或 Platform 承載關係。
- 新增或移除重大 Capability Relationship／Dependency。
- 引入 Breaking Dependency 或改變依賴方向。

每項變更 MUST：

1. 說明原因、預期結果與不變範圍。
2. 識別受影響之 Capability、Platform、Repository、Owner、Interface 與 Dependency。
3. 評估責任重疊、孤立能力、循環依賴及相容性風險。
4. 定義 Migration、Rollback、版本與生命週期策略。
5. 更新 Architecture、Register、Capability Catalog 與相關 Matrix。
6. 依重大程度建立或引用 ADR。

Capability Catalog 之建立屬後續獨立架構資產，MUST 於本文件核准後另循 EWO 與 Architecture Review 建立。

## 13. Compliance

Capability Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Capability 可追溯至 WA-001、AEOS-ARCH-001 與 AEOS-ARCH-004 |
| Identity | 具唯一、穩定且不可重用的 Capability ID |
| Outcome | 以能力責任與企業結果描述，不以特定實作取代 |
| Boundary | Outcome、責任、包含與排除範圍明確 |
| Classification | 具有單一 Primary Classification 且判定依據成立 |
| Relationship | 與 Platform、Repository、Dependency 之關係具有方向、類型與依據 |
| Ownership | 具有唯一 accountable Capability Owner，無未解決責任重疊 |
| Dependency | 依賴方向、類型、風險與 Owner 可追溯，符合 Layer Architecture 分層規則 |
| Lifecycle | 狀態、轉移條件、Migration 與替代關係完整 |
| Catalog Readiness | 未經核准不建立具名 Capability 條目；Catalog 待本文件核准後另立 |
| Asset Consistency | Architecture、Register、Catalog 與 Matrix 狀態一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 14. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Capability Architecture 與 Capability Ownership 的唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved v1.0.0） | Architecture | 將 WA-001 正式納入 AEOS |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved v1.0.0） | Architecture | Governance Domains 與分類值域來源 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved v1.0.0） | Architecture | 重大架構決策紀錄機制 |
| REF-005 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Capability Architecture 的上位定位與 MUST |
| REF-006 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved v1.0.0） | Architecture | Platform 承載 Capability 之責任與關係 |
| REF-007 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved v1.0.0） | Architecture | L4 Capability 層級與依賴方向規則 |
| REF-008 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved v1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-009 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-010 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-011 | EWO-AEOS-0014 | EWO | 本文件之工作來源 |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併；狀態更新為 Approved，成為 AEOS Capability Architecture 正式定義（EWO-AEOS-0015；AR-AEOS-0015-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Capability Identity、Boundary、Classification、Relationship、Dependency、Ownership、Lifecycle、Change 與 Compliance（EWO-AEOS-0014） | Codex |
