---
doc-id: AEOS-ARCH-008
doc-name: Repository Architecture
doc-type: Architecture
repository: AEOS
version: 1.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0016
  - EWO-AEOS-0017
  - AR-AEOS-0017-R1
  - AEOS-ADR-002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
---

# AEOS-ARCH-008 — Repository Architecture

## Executive Summary

本文件依 AEOS-ADR-002 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Repository Architecture，定義 Repository 的識別、責任邊界、類型、角色、與 Platform／Capability 的關係、Ownership、Dependency、Lifecycle、Change 與治理規則。Repository 是版本化治理與交付邊界：一個 Platform 可由一個或多個 Repository 支援，Repository 實現 Capability 並承載架構資產或實作責任，但 Repository 不等同 Platform、Capability 或 Implementation。本文件不重新設計既定 Architecture、不建立具名 Repository Catalog 或實際 Repository 清單、不建立實作設計，也不開始 Dependency Architecture。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-008 |
| 文件名稱 | Repository Architecture |
| 型別 | Architecture（Repository Architecture） |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0016、EWO-AEOS-0017、AR-AEOS-0017-R1、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001（Approved v1.0.0）、AEOS-ARCH-004（Approved v1.0.0）、AEOS-ARCH-005（Approved v1.0.0）、AEOS-ARCH-006（Approved v1.0.0）、AEOS-ARCH-007（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0017、AR-AEOS-0017-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007、AEOS-STD-001～AEOS-STD-005、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件之目的為：

- 定義 Repository 類型、責任、Authority 與 Boundary（AEOS-ARCH-004 §6.4）。
- 指明 Repository 所屬 Platform、支援 Capability 與允許依賴（AEOS-ARCH-004 §6.4）。
- 區分 Enterprise Root Repository、Platform Repository、Capability／Domain Repository 與 Product／Implementation Repository 的治理責任（AEOS-ARCH-004 §6.4）。
- 確保 Repository 自身架構不得與 Enterprise Architecture 衝突（AEOS-ARCH-004 §6.4）。
- 清楚區分 Repository、Platform、Capability 與 Implementation，維持版本化治理邊界、能力責任、平台承載邊界與實作責任的分離。
- 為後續 Repository Catalog 與 Repository Mapping 提供正式架構依據（AEOS-ARCH-004 §8）。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Repository 之正式定義、架構身分與必要屬性。
- Repository Boundary、Responsibility 與 Authority。
- Repository 類型與角色，及其治理責任。
- Repository 與 Platform、Capability、Implementation 之關係與區別。
- Repository Ownership、Dependency 與治理規則。
- Repository 生命週期、變更控制與合規要求。
- Repository Catalog 與 Repository Mapping 之定位（不建立條目）。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改既定 Architecture（AEOS-ARCH-001、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007）。
- 具名 Repository Catalog 或實際 Repository 清單之建立（Catalog 應作為後續獨立架構資產）。
- Repository Mapping 之實際條目。
- Dependency Architecture 之建立（屬後續獨立 EWO）。
- 個別 Repository 之內部技術架構、部署拓撲或實作設計。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。

## 3. Architecture Authority

Repository Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| R0 | AEOS-ARCH-001 | 最高架構權威 |
| R1 | AEOS-ARCH-001 | 架構 Entry Document 與 Architecture Register |
| R2 | AEOS-ARCH-004 | 定義 Repository Architecture 在 Enterprise Architecture 中的定位與 MUST |
| R3 | AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007 | 定義 Platform 承載、L5 Repository 層級與 Capability 實現關係 |
| R4 | AEOS-ARCH-008（本文件） | 定義 Repository 模型、類型、邊界與治理規則 |
| R5 | Repository Catalog、Repository Mapping、Repository 層級文件 | 登錄已核准 Repository 事實並落實 |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的 Repository 或改寫既定架構。
- Repository 與 Platform、Capability MUST 分離定義：Platform 是承載與治理邊界，Capability 是可提供之能力，Repository 是版本化治理與交付邊界（AEOS-ARCH-004 §7、AEOS-ARCH-005 §4.2）。
- 發現 Approved 架構載體未涵蓋的 Repository 需求時，MUST 先透過正式架構變更處理。

## 4. Repository Definition

### 4.1 Formal Definition

Repository 是 AI Engineering Workspace 內具備持續身分、明確責任與治理權威的版本化治理與交付邊界。Repository 承載架構資產或實作責任，支援一個或多個 Platform 與 Capability，並透過受治理的依賴與其他 Repository 協作。

一個架構元素只有在同時符合下列條件時，才可被認定為 Repository：

- 具有可被唯一識別且跨版本延續的 Repository ID。
- 具有明確的 Mission、Boundary、Responsibility 與 Authority。
- 具有明確之類型（§7）與所屬 Platform、支援 Capability。
- 具有明確 Repository Owner 與治理責任。
- 其版本、變更與交付由 Repository 治理管理，而非隨實作任意建立或移除。
- 與 Platform、Capability、Dependency 及其他 Repository 之關係可被登錄與追溯。

### 4.2 Repository Is Not

| 架構元素 | 與 Repository 的區別 |
|----------|----------------------|
| Platform | Platform 是承載一組 Capability 的穩定企業架構邊界；一個 Platform 可由一個或多個 Repository 支援，但 Repository 不等同 Platform |
| Capability | Capability 描述 Workspace 能提供什麼；Repository 實現 Capability，但 Repository 不等同 Capability |
| Implementation | Implementation 是實作、工具與部署細節（L6）；Repository 是版本化治理與交付邊界（L5），包含但不等同其實作內容 |
| Product | Product 面向特定使用者或市場結果；Repository 是治理與交付邊界 |
| Service／Application | Service 或 Application 是可部署或可使用的實作單位；Repository 是較高階的治理邊界 |
| Team／Organization | Team 是人員組織；Repository Ownership 可由 Team 承擔，但 Repository 不等同組織圖 |

## 5. Repository Identity Model

每個 Repository MUST 具備下列權威屬性：

| 屬性 | 必要性 | 定義 |
|------|--------|------|
| Repository ID | MUST | 全 Workspace 唯一且不可重複使用的穩定識別碼 |
| Repository Name | MUST | 正式名稱；命名變更不得改變 Repository ID |
| Type | MUST | 依 §7 指定之 Repository 類型 |
| Boundary | MUST | Repository 包含與排除的責任範圍 |
| Authority | MUST | Repository 可作出決策與治理的範圍 |
| Platform Reference | MUST | 所屬之已核准 Platform 引用 |
| Capability References | MUST | 支援之已核准 Capability 引用；可為一個或多個 |
| Repository Owner | MUST | 對 Repository 完整性與演進負責的角色 |
| Dependencies | MUST | 依 Dependency Architecture 管理之正式依賴引用 |
| Lifecycle Status | MUST | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | MUST | 核准 Repository 身分與邊界的 Architecture／ADR |

Repository ID、Type、Boundary 或 Authority 的實質變更 MUST 經 Architecture Review；不得只修改 Catalog 條目完成架構變更。

## 6. Repository Boundary and Responsibility

### 6.1 Boundary Dimensions

Repository Boundary MUST 同時從下列面向定義：

| 面向 | 必須回答的問題 |
|------|----------------|
| Responsibility Boundary | Repository 對哪些企業結果、架構資產或交付負責？ |
| Authority Boundary | Repository 可制定哪些規則、介面與內部決策？ |
| Platform Boundary | Repository 屬於哪些 Platform，哪些明確不屬於其範圍？ |
| Capability Boundary | Repository 支援哪些 Capability，各自承擔何種角色？ |
| Dependency Boundary | Repository 可接受、提供或禁止哪些依賴？ |
| Information Boundary | 哪些架構資訊由 Repository 擁有、提供或僅引用？ |

### 6.2 Boundary Rules

- 每項 Repository Responsibility MUST 能對應至 Mission 與至少一項正式架構資產或 Capability。
- 同一責任不得由多個 Repository 在無 Ownership 決議的情況下同時宣稱最終 Authority。
- Repository MAY 委派執行工作，但 MUST 保留其治理責任與可追溯性。
- Repository 位於 Platform Boundary 內，不代表其全部內容皆由 Platform 擁有；實際責任以 Repository Architecture 與 Mapping 為準（AEOS-ARCH-005 §6.2）。
- Repository MUST NOT 直接管理超出其 Boundary 的 Platform、Capability 或架構決策。

## 7. Repository Types and Roles

Repository 依其治理責任分為下列類型（AEOS-ARCH-004 §6.4）：

| 類型 | 核心責任 | 治理意義 |
|------|----------|----------|
| Enterprise Root Repository | 承載 Workspace 層級之企業架構、治理與共同控制資產 | 跨 Repository 之最高治理與單一來源 |
| Platform Repository | 承載 Platform 之架構資產與交付 | 支援 Platform 邊界與 Platform Catalog 一致性 |
| Capability／Domain Repository | 承載特定 Capability 或領域之架構資產與交付 | 支援 Capability 定義、結果與演進 |
| Product／Implementation Repository | 承載產品或實作之交付 | 落實實作責任，受上位治理約束 |

規則：

- 每個 Repository MUST 指定一個 Primary Type。
- Repository 類型 MUST NOT 單憑 Repository 名稱、部署方式、供應商產品或組織單位決定。
- 類型變更視為 Architecture Change（§12），MUST 經 EWO 與 Architecture Review。
- Repository 之角色與責任 MUST 可追溯至其類型與所屬 Platform、支援 Capability。

## 8. Repository Relationship Model

### 8.1 Relationship Types

| 關係 | 語意 | 要求 |
|------|------|------|
| Belongs To | Repository 屬於某 Platform，支援其邊界與交付 | MUST 對應已核准 Platform |
| Supports | Repository 支援某 Capability，承擔實現或治理角色 | MUST 對應已核准 Capability |
| Implements | Repository 落實上位架構規則或實作責任 | MUST 符合上位邊界，不擴張 Authority |
| Consumes | Repository 使用其他 Repository 或資源提供之能力 | MUST 記錄 Dependency 與責任邊界 |
| Collaborates With | 多個 Repository 共同完成結果，但維持各自 Authority | MUST 明確指定各方責任與 accountable Owner |
| Governs | 一個 Repository 對另一 Repository 之特定治理面向具有正式權威 | MUST 限定治理範圍，不得推定全面控制 |
| Supersedes | 新 Repository 正式取代既有 Repository 之責任 | MUST 具有 Migration、替代關係與退役計畫 |

### 8.2 Relationship Rules

- Repository Relationship MUST 有方向、類型、Owner、依據與生命週期狀態。
- `Belongs To` 不轉移 Platform Ownership；Repository 仍保有自身治理責任。
- `Supports` 不取代 Capability Ownership；Repository 實現能力但不等同能力定義。
- `Consumes` 不轉移 Repository Accountability。
- 跨 Repository 之實際 Dependency MUST 登錄於 Dependency Matrix。
- 循環治理關係 MUST NOT 被允許；循環技術或服務依賴必須由 Dependency Architecture 明確評估。

## 9. Repository Dependency

### 9.1 Dependency Definition

Repository Dependency 是 Repository 之間、或 Repository 與 Platform／Capability／Layer 之間之正式依賴關係。依賴 MUST 以架構資產或交付責任為基礎描述，不以特定實作或技術套件取代。

### 9.2 Dependency Rules

- 每項 Repository Dependency MUST 有方向、類型、Owner、依據與生命週期狀態。
- Dependency MUST 由 Dependency Architecture 定義並登錄於 Dependency Matrix；不得只存在於非正式敘述或實作中（AEOS-ARCH-004 §7）。
- 依賴方向 MUST 符合 Layer Architecture 之分層規則（AEOS-ARCH-006 §5.2）：上位約束、下位依賴；禁止反向控制。
- 新增、移除或變更重大 Dependency MUST 經 Architecture Change（§12）。
- 循環依賴 MUST 由 Dependency Architecture 明確評估，未經核准不得建立。

## 10. Repository Ownership

### 10.1 Roles

| 角色 | 責任 |
|------|------|
| Architecture Owner | 維護 Repository 與 Enterprise Architecture 一致性、裁決邊界衝突及核准重大變更 |
| Repository Owner | 對 Repository Mission、Boundary、Authority 與生命週期負最終責任 |
| Platform Owner | 維護所屬 Platform 之邊界與 Platform Catalog 條目一致性 |
| Capability Owner | 維護所支援 Capability 之定義、結果及演進 |
| Dependency Owner | 維護跨 Repository Dependency 之必要性、風險與相容性 |
| Review Owner | 依 AEOS-STD-005 確認 Repository 變更已完成正式 Review |

### 10.2 Accountability Rules

- 每個 Active Repository MUST 有且只有一個 accountable Repository Owner 角色。
- Repository Owner 可委派執行工作，但 MUST NOT 委派最終 Accountability。
- Repository Owner 與 Platform Owner 不必為同一角色；兩者責任 MUST 分別記錄（AEOS-ARCH-005 §10.2）。
- 跨 Repository 之 Capability 或架構資產 MUST 在 Ownership Matrix 中明確區分 accountable、responsible、consulted 與 informed 關係。
- Ownership 缺失、重疊或無法履行時，Repository MUST NOT 進入 Active 狀態。

## 11. Repository Lifecycle

| 狀態 | 定義 | 必要條件 |
|------|------|----------|
| Candidate | 已提出但尚未成為正式 Repository | 具 Mission、初步 Boundary、提案 Owner 與 Architecture Reference |
| Active | 已核准並承擔正式 Workspace 責任 | 完整 Identity、Type、Owner、Platform、Capability 與 Dependency 記錄 |
| Deprecated | 仍受支援但不得承接新的策略性責任 | 具替代方向、Migration Plan、期限與受影響項目 |
| Retired | 已停止承擔正式責任 | 相關架構資產、Capability 與依賴已移轉或終止 |

允許的主要狀態轉移為：

- Candidate → Active：完成 Architecture Review 並登錄 Repository Catalog。
- Candidate → Retired：提案撤回或核准不成立，保留決策紀錄。
- Active → Deprecated：核准取代或合併策略並建立 Migration Plan。
- Deprecated → Active：取代決策撤銷且重新完成 Architecture Review。
- Deprecated → Retired：Migration 完成且無未處理責任或依賴。

任何跳過 Deprecated 的 Active → Retired 轉移 MUST 具有緊急理由、影響分析與 Architecture Owner 核准。

## 12. Change and Evolution

下列變更屬於 Architecture Change，MUST 經 EWO 與 Architecture Review：

- 建立、合併、拆分、取代或退役 Repository。
- 變更 Repository Type、Mission、Boundary 或 Authority。
- 移轉關鍵 Capability、Platform 歸屬或 Repository Ownership。
- 新增或移除重大 Repository Relationship／Dependency。
- 引入 Breaking Dependency 或改變依賴方向。

每項變更 MUST：

1. 說明原因、預期結果與不變範圍。
2. 識別受影響之 Repository、Platform、Capability、Owner、Interface 與 Dependency。
3. 評估責任重疊、孤立能力、循環依賴及相容性風險。
4. 定義 Migration、Rollback、版本與生命週期策略。
5. 更新 Architecture、Register、Repository Catalog 與相關 Matrix。
6. 依重大程度建立或引用 ADR。

Repository Catalog 之建立屬後續獨立架構資產，MUST 於本文件核准後另循 EWO 與 Architecture Review 建立；本文件不建立任何具名 Repository 條目。

## 13. Compliance

Repository Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Repository 可追溯至 AEOS-ARCH-001 與 Approved 架構載體 |
| Identity | 具唯一、穩定且不可重用的 Repository ID |
| Boundary | Mission、責任、Authority、包含與排除範圍明確 |
| Type | 具有單一 Primary Type 且判定依據成立 |
| Platform／Capability | 所屬 Platform 與支援 Capability 已識別，且未將 Repository 等同 Platform 或 Capability |
| Ownership | 具有唯一 accountable Repository Owner，無未解決責任重疊 |
| Dependency | 依賴方向、類型、風險與 Owner 可追溯，符合 Layer Architecture 分層規則 |
| Lifecycle | 狀態、轉移條件、Migration 與替代關係完整 |
| Catalog Readiness | 未經核准不建立具名 Repository 條目；Catalog 待本文件核准後另立 |
| Asset Consistency | Architecture、Register、Catalog 與 Matrix 狀態一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 14. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved v1.0.0） | Architecture | 架構基線與 Architecture Register |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved v1.0.0） | Architecture | Governance Domains 與治理階層 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved v1.0.0） | Architecture | 重大架構決策紀錄機制 |
| REF-005 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Repository Architecture 的上位定位與 MUST |
| REF-006 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved v1.0.0） | Architecture | Platform 承載與 Repository 邊界關係 |
| REF-007 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved v1.0.0） | Architecture | L5 Repository 層級與依賴方向規則 |
| REF-008 | [AEOS-ARCH-007 — Capability Architecture](AEOS-ARCH-007-Capability-Architecture.md)（Approved v1.0.0） | Architecture | Capability 實現與 Repository 角色 |
| REF-009 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved v1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-010 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-011 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-012 | EWO-AEOS-0016 | EWO | 本文件之工作來源 |
| REF-013 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 2（AEOS-ADR-002 已核准）：執行 Architecture Transition——WA-001 分類為歷史來源（Historical Reference）；Authority 階層（R0）與對應來源重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併；狀態更新為 Approved，成為 AEOS Repository Architecture 正式定義（EWO-AEOS-0017；AR-AEOS-0017-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Repository Identity、Boundary、Type、Relationship、Dependency、Ownership、Lifecycle、Change 與 Compliance（EWO-AEOS-0016） | Codex |
