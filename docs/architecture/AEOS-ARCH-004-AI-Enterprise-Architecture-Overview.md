---
doc-id: AEOS-ARCH-004
doc-name: AI Enterprise Architecture Overview
doc-type: Architecture
repository: AEOS
version: 1.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-Architecture-0001
  - AEOS-ADR-002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
---

# AEOS-ARCH-004 — AI Enterprise Architecture Overview

## Executive Summary

本文件依 AEOS-ADR-002 建立 AEOS Enterprise Architecture 的總架構入口，將 Platform Architecture、Layer Architecture、Capability Architecture、Repository Architecture、Dependency Architecture 與 Workspace Architecture 統合為單一可追溯架構視圖，並界定後續 Platform Catalog、Capability Catalog、Ownership Matrix 與 Dependency Matrix 的定位與引用關係。本文件不重新設計 Approved 架構載體、不建立實作設計，也不取代各專項架構文件。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-004 |
| 文件名稱 | AI Enterprise Architecture Overview |
| 型別 | Architecture（Enterprise Architecture Entry Document） |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-Architecture-0001、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001（Approved 1.3.0） |
| 關聯文件 | AEOS-ARCH-002、AEOS-ARCH-003、AEOS-DIA-001、AEOS-CON-001、AEOS-STD-001～AEOS-STD-005、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件作為 AEOS Enterprise Architecture 的總架構入口，其目的為：

- 將已核准之 AI Engineering Workspace Architecture 正式轉化為 AEOS Enterprise Architecture 的整體視圖。
- 定義各架構領域在同一 Enterprise Architecture 中的定位、邊界與關係。
- 建立後續專項架構文件與 Catalog／Matrix 資產的共同上位引用來源。
- 維持 Architecture Baseline、專項架構與治理資產之間的可追溯性。
- 確保後續架構演進不重複定義、不形成平行架構來源，也不超出 Approved 架構載體已核准範圍。

## 2. Scope

### 2.1 In Scope

本文件涵蓋下列 Enterprise Architecture 領域：

- Platform Architecture。
- Layer Architecture。
- Capability Architecture。
- Repository Architecture。
- Dependency Architecture。
- Workspace Architecture。
- Platform Catalog、Capability Catalog、Ownership Matrix 與 Dependency Matrix 在整體架構中的定位。
- 上述領域之間的關係、引用方向與治理邊界。

### 2.2 Out of Scope

本文件明確不涵蓋：

- 重新定義或修改 Approved 架構載體已核准架構。
- 個別 Platform、Capability 或 Repository 的詳細設計。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。
- Platform Catalog、Capability Catalog、Ownership Matrix 與 Dependency Matrix 的實際條目。
- 專項架構文件應承載的完整規則、模型與生命週期。
- Standards、Policies、Specifications 或 ADR 的內容定義。

## 3. Architecture Authority

AEOS Enterprise Architecture 依下列權威順序建立：

| 層級 | 文件 | 架構角色 |
|------|------|----------|
| A0 | AEOS-ARCH-001 — Architecture Baseline | 最高架構權威 |
| A1 | AEOS-ARCH-001 — Architecture Baseline | 架構基線與 Architecture Register（Entry Document） |
| A2 | AEOS-ARCH-004 — AI Enterprise Architecture Overview | Enterprise Architecture 總入口與整體關係模型 |
| A3 | 專項 Architecture | 定義各架構領域的正式模型與邊界 |
| A4 | Catalog／Matrix | 記錄受架構治理的實體、責任與依賴關係 |
| A5 | Repository Architecture／Specification／ADR | 將 Enterprise Architecture 套用至 Repository 與具體決策 |

規則：

- 下位架構資產 MUST 符合上位架構資產。
- 下位架構資產 MUST NOT 覆蓋或改寫 AEOS-ARCH-001／Approved 架構載體已建立之架構權威。
- 發現架構缺口或衝突時，MUST 透過正式 EWO、Review 與 ADR 流程處理，不得在 Catalog、Matrix 或 Repository 文件中隱性建立新架構。

## 4. Enterprise Architecture Vision

AEOS 將 AI Engineering Workspace 視為由平台、架構層、能力、Repository、依賴與 Workspace 組成的整體企業工程系統。Enterprise Architecture 的責任不是實作個別系統，而是建立跨 Repository 的共同結構、責任邊界、依賴規則與治理語言，使各 Repository 能在一致的架構基線下獨立演進。

此架構具備下列特性：

- **Architecture First**：Repository 與實作決策 MUST 可追溯至正式架構。
- **Platform Oriented**：Platform 作為能力承載與 Repository 協作的穩定邊界。
- **Capability Driven**：能力描述系統能提供什麼，而不是預先指定如何實作。
- **Repository Governed**：Repository 是架構資產與工程交付的正式治理單位。
- **Dependency Explicit**：跨層、跨平台、跨能力與跨 Repository 的依賴 MUST 被識別與管理。
- **Workspace Integrated**：各 Repository 共同構成單一 AI Engineering Workspace，而非彼此孤立的專案集合。

## 5. Enterprise Architecture Model

AEOS Enterprise Architecture 由六個相互關聯的正式架構領域構成：

| 架構領域 | 核心問題 | 正式責任 | 主要輸出 |
|----------|----------|----------|----------|
| Platform Architecture | Workspace 由哪些平台構成？ | 定義 Platform 邊界、角色、互動與治理關係 | Platform Architecture、Platform Catalog |
| Layer Architecture | 架構責任如何分層？ | 定義各層責任、允許關係與跨層約束 | Layer Model、Layer Rules |
| Capability Architecture | Workspace 必須具備哪些能力？ | 定義 Capability、組合、關係與演進邊界 | Capability Architecture、Capability Catalog |
| Repository Architecture | 架構如何落實於 Repository？ | 定義 Repository 類型、責任、邊界與映射 | Repository Architecture、Repository Mapping |
| Dependency Architecture | 架構元素如何依賴？ | 定義依賴方向、類型、約束與治理方式 | Dependency Architecture、Dependency Matrix |
| Workspace Architecture | 所有元素如何形成整體？ | 定義 Workspace 邊界、組成、協作與整合關係 | Workspace Architecture、Workspace View |

六個領域共同構成完整架構，不得被視為六套彼此獨立的架構。任何專項架構文件 MUST 說明其與其他領域的輸入、輸出及依賴關係。

## 6. Architecture Domains

### 6.1 Platform Architecture

Platform Architecture 定義 AI Engineering Workspace 中 Platform 的正式邊界、角色與互動關係，並回答能力由何種 Platform 承載、Platform 之間如何協作，以及 Platform 如何受到 Enterprise Governance 管理。

Platform Architecture MUST：

- 以 AEOS-ARCH-005 之 Platform Topology 為來源。
- 定義 Platform Identity、Boundary、Responsibility 與 Relationship。
- 將 Platform 與 Capability、Repository、Dependency 及 Ownership 建立可追溯映射。
- 以 Platform Catalog 作為已核准 Platform 實體之登錄來源。

### 6.2 Layer Architecture

Layer Architecture 定義 Enterprise Architecture 的責任分層與允許關係，使治理、平台、能力、Repository 與實作責任保持清楚邊界。

Layer Architecture MUST：

- 定義每一架構層的責任與禁止承載之內容。
- 定義相鄰層與跨層依賴的允許方向。
- 防止下位層繞過上位治理或反向控制上位架構。
- 為 Repository Architecture 與 Dependency Architecture 提供共同分層基準。

### 6.3 Capability Architecture

Capability Architecture 定義 AI Engineering Workspace 所需能力、能力群組及能力之間的關係，並將能力與承載 Platform、責任 Owner 及實現 Repository 建立連結。

Capability Architecture MUST：

- 以 AEOS-ARCH-007 之 Capability Architecture 與 Capability Ownership 為來源。
- 以能力責任與企業結果描述 Capability，不以特定實作取代能力定義。
- 定義 Capability 之邊界、關係、依賴與生命週期。
- 以 Capability Catalog 登錄已核准能力，並以 Ownership Matrix 記錄責任歸屬。

### 6.4 Repository Architecture

Repository Architecture 定義 Repository 在 Enterprise Architecture 中的角色、責任邊界及與 Platform／Capability 的映射，使每個 Repository 具有明確的架構身分。

Repository Architecture MUST：

- 定義 Repository 類型、責任、Authority 與 Boundary。
- 指明 Repository 所屬 Platform、支援 Capability 與允許依賴。
- 區分 Enterprise Root Repository、Platform Repository、Capability／Domain Repository 與 Product／Implementation Repository 的治理責任。
- 確保 Repository 自身架構不得與 Enterprise Architecture 衝突。

### 6.5 Dependency Architecture

Dependency Architecture 定義 Platform、Layer、Capability、Repository 與 Workspace 元素之間的依賴類型、方向與治理約束。

Dependency Architecture MUST：

- 將依賴視為正式架構關係，而非僅為技術套件連結。
- 識別 Architecture、Governance、Capability、Repository、Data、Service 與 Delivery Dependency。
- 定義允許、受限與禁止之依賴方向。
- 以 Dependency Matrix 記錄已核准依賴，並支援影響分析與演進決策。

### 6.6 Workspace Architecture

Workspace Architecture 定義所有 Platform、Capability、Repository、治理資產與依賴如何共同形成 AI Engineering Workspace。

Workspace Architecture MUST：

- 以 AEOS-ARCH-010 之 Workspace Architecture 為來源。
- 定義 Workspace Boundary、Composition、Interaction 與 Shared Governance。
- 統合 Platform、Capability、Repository 與 Dependency View。
- 作為跨 Repository 協作、Workspace 演進與整體一致性檢查的架構依據。

## 7. Architecture Relationship Model

各架構領域之主要關係如下：

| 來源 | 關係 | 目標 | 治理意義 |
|------|------|------|----------|
| Workspace Architecture | 組成 | Platform Architecture | Workspace 由受治理的 Platform 構成 |
| Layer Architecture | 約束 | 全部架構領域 | 所有元素依共同分層規則配置責任與依賴 |
| Platform Architecture | 承載 | Capability Architecture | Platform 提供 Capability 的組織與運作邊界 |
| Capability Architecture | 分配責任至 | Ownership Matrix | 每項 Capability 具有明確責任歸屬 |
| Repository Architecture | 實現／治理 | Platform 與 Capability | Repository 承載架構資產或實作責任 |
| Dependency Architecture | 連結並約束 | Platform、Layer、Capability、Repository | 依賴必須明確、可驗證且方向合規 |
| Catalog／Matrix | 登錄 | 已核准架構元素與關係 | 提供可查詢、可追溯的架構事實 |

關係規則：

- Platform 與 Capability MUST 分離定義：Platform 是承載與治理邊界，Capability 是可提供之能力。
- Capability 與 Repository MUST 分離定義：Capability 描述能力責任，Repository 描述治理與交付邊界。
- Ownership MUST 指向明確 Architecture Owner、Platform Owner、Capability Owner 或 Repository Owner。
- Dependency MUST 由 Dependency Architecture 定義，並由 Dependency Matrix 記錄；不得只存在於非正式敘述或實作中。
- Workspace View MUST 能追溯至 Platform、Capability、Repository、Ownership 與 Dependency 的正式資產。

## 8. Architecture Assets

後續架構資產分為 Architecture、Catalog 與 Matrix 三類：

| 資產類型 | 資產 | 角色 | 上位引用 |
|----------|------|------|----------|
| Architecture | Platform Architecture | Platform 邊界、角色與關係之正式定義 | AEOS-ARCH-004 |
| Architecture | Layer Architecture | 架構責任分層與依賴方向之正式定義 | AEOS-ARCH-004 |
| Architecture | Capability Architecture | Capability 結構、關係與治理之正式定義 | AEOS-ARCH-004 |
| Architecture | Repository Architecture | Repository 身分、責任與映射之正式定義 | AEOS-ARCH-004 |
| Architecture | Dependency Architecture | 跨架構元素依賴模型之正式定義 | AEOS-ARCH-004 |
| Architecture | Workspace Architecture | Workspace 組成與整合關係之正式定義 | AEOS-ARCH-004 |
| Catalog | Platform Catalog | 已核准 Platform 的權威清單 | Platform Architecture、AEOS-ARCH-004 |
| Catalog | Capability Catalog | 已核准 Capability 的權威清單 | Capability Architecture、AEOS-ARCH-004 |
| Matrix | Ownership Matrix | Platform、Capability、Repository 與 Owner 的責任映射 | Capability Architecture、Repository Architecture |
| Matrix | Dependency Matrix | 架構元素之已核准依賴映射 | Dependency Architecture、AEOS-ARCH-004 |

Catalog 與 Matrix 只登錄由正式 Architecture 核准的事實，MUST NOT 在未經 Architecture Review 的情況下創造新 Platform、Capability、Ownership 或 Dependency。

## 9. Ownership and Governance

| 角色 | 核心責任 |
|------|----------|
| Architecture Owner | 維護 Enterprise Architecture 一致性、核准架構邊界與處理跨領域衝突 |
| Platform Owner | 維護 Platform 邊界、Platform Catalog 條目與 Platform 內部架構一致性 |
| Capability Owner | 維護 Capability 定義、結果、依賴與生命週期 |
| Repository Owner | 確保 Repository Architecture 與 Enterprise Architecture 一致 |
| Review Owner | 依 AEOS-STD-005 執行 Architecture Review 並確認 Review Decision 完整 |

治理規則：

- 新增或變更 Enterprise Architecture 資產 MUST 透過 EWO、Review 與 Merge 流程。
- 重大架構決策 MUST 依 AEOS-ARCH-003 建立 ADR。
- 架構文件格式、Metadata、引用、命名與 Review MUST 分別遵循 AEOS-STD-001～AEOS-STD-005。
- Architecture、Catalog 與 Matrix 之狀態 MUST 保持一致；不得引用未核准或已失效之架構事實作為正式依據。

## 10. Architecture Evolution

AEOS Enterprise Architecture 採增量演進：

1. 由本文件建立整體架構入口與領域關係。
2. 依 EWO 逐一建立專項 Architecture。
3. 專項 Architecture 核准後建立對應 Catalog 與 Matrix。
4. Repository 依正式架構完成映射與合規檢查。
5. 透過 Dependency Matrix 執行變更影響分析。
6. 重大變更以 ADR 記錄，並同步更新受影響之 Architecture、Catalog 與 Matrix。

演進規則：

- 每次演進 MUST 維持對 AEOS-ARCH-001 與 Approved 架構載體的追溯。
- 新架構資產 MUST 先定義責任與邊界，再建立 Catalog／Matrix 條目。
- 架構變更 MUST 評估 Platform、Capability、Repository、Ownership、Dependency 與 Workspace 的連鎖影響。
- 已被取代之架構資產 MUST 依文件生命週期保留歷史與替代關係。

## 11. Compliance

本文件及其下位架構資產 MUST 符合下列要求：

| 檢查項目 | 合規要求 |
|----------|----------|
| Architecture Authority | 可追溯至 AEOS-ARCH-001 與 Approved 架構載體，未建立平行架構來源 |
| Domain Boundary | Platform、Layer、Capability、Repository、Dependency、Workspace 之責任明確且不互相取代 |
| Relationship Integrity | 架構元素之關係具有明確方向、類型與權威來源 |
| Ownership | Platform、Capability、Repository 與架構決策具有明確 Owner |
| Dependency | 跨元素依賴可識別、可追溯且符合允許方向 |
| Asset Consistency | Architecture、Catalog 與 Matrix 狀態及內容一致 |
| Repository Alignment | Repository Architecture 與 Enterprise Architecture 無衝突 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 12. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md) | Architecture Entry Document | AEOS 架構基線與 Architecture Register |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md) | Architecture | Enterprise Governance 架構與治理層級 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md) | Architecture | 架構決策與 ADR 治理 |
| REF-005 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-006 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md) | Constitution | Repository 最高治理原則 |
| REF-007 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md) | Standard | 文件格式規範 |
| REF-008 | [AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md) | Standard | Metadata 規範 |
| REF-009 | [AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md) | Standard | Cross-reference 規範 |
| REF-010 | [AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md) | Standard | 命名規範 |
| REF-011 | [AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standard | Architecture Review 規範 |
| REF-012 | EWO-AEOS-Architecture-0001 — AI Enterprise Architecture Overview | EWO | 本文件之工作來源 |
| REF-013 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 13. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 2（AEOS-ADR-002 已核准）：執行 Architecture Transition——WA-001 分類為歷史來源（Historical Reference）；Authority 階層（A0）與對應來源重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併（PR #13）；狀態更新為 Approved，成為 AEOS Enterprise Architecture 正式總入口 | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 建立 AEOS Enterprise Architecture 總入口，定義 Platform、Layer、Capability、Repository、Dependency、Workspace 六個架構領域及 Platform Catalog、Capability Catalog、Ownership Matrix、Dependency Matrix 的定位與關係（EWO-AEOS-Architecture-0001） | Codex |
