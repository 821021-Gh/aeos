---
doc-id: AEOS-ARCH-010
doc-name: Workspace Architecture
doc-type: Architecture
repository: AEOS
version: 1.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0020
  - EWO-AEOS-0021
  - AR-AEOS-0021-R1
  - AEOS-ADR-002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-008
  - AEOS-ARCH-009
---

# AEOS-ARCH-010 — Workspace Architecture

## Executive Summary

本文件依 AEOS-ADR-002、AEOS-ARCH-001 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Workspace Architecture，將 Approved 架構載體既定設計正式轉化為 AEOS 內可治理、可追溯的架構定義，涵蓋 Workspace 的目的、識別、組成、責任邊界、類型與層級，以及與 Platform、Capability、Repository、Dependency、Implementation 的關係。本文件亦定義 Workspace 的 Ownership、Membership、Lifecycle、Provisioning、Change、Access 與治理規則。Workspace 是所有 Platform、Capability、Repository 與治理資產共同形成的整體，不等同 Repository、Platform、Project 或 Runtime Environment。本文件不重新設計既定 Architecture、不建立具名 Workspace Catalog 或實際 Workspace 清單、不建立特定工具配置，也不提前開始 M5 Catalog／Matrix 資產。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-010 |
| 文件名稱 | Workspace Architecture |
| 型別 | Architecture（Workspace Architecture） |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0020、EWO-AEOS-0021、AR-AEOS-0021-R1、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001（Approved v1.0.0）、AEOS-ARCH-004（Approved v1.0.0）、AEOS-ARCH-005（Approved v1.0.0）、AEOS-ARCH-006（Approved v1.0.0）、AEOS-ARCH-007（Approved v1.0.0）、AEOS-ARCH-008（Approved v1.0.0）、AEOS-ARCH-009（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0021、AR-AEOS-0021-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007、AEOS-ARCH-008、AEOS-ARCH-009、AEOS-STD-001～AEOS-STD-005、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件之目的為：

- 以 Approved 架構載體之 Workspace Architecture 為來源，將其既定設計正式轉化為 AEOS Workspace Architecture（AEOS-ARCH-004 §6.6）。
- 定義 Workspace Boundary、Composition、Interaction 與 Shared Governance（AEOS-ARCH-004 §6.6）。
- 統合 Platform、Capability、Repository 與 Dependency View，作為跨 Repository 協作、Workspace 演進與整體一致性檢查之架構依據（AEOS-ARCH-004 §6.6）。
- 定義 Workspace 的目的、識別、組成、責任邊界、類型與層級。
- 定義 Workspace 與 Platform、Capability、Repository、Dependency、Implementation 的關係。
- 定義 Workspace Ownership、Membership、Lifecycle、Provisioning、Change、Access 與治理規則。
- 清楚區分 Workspace 與 Repository、Platform、Project、Runtime Environment。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Workspace 之正式定義、架構身分與必要屬性。
- Workspace 目的、組成、責任邊界、類型與層級。
- Workspace 與 Platform、Capability、Repository、Dependency、Implementation 之關係。
- Workspace Ownership、Membership、Lifecycle、Provisioning、Change 與 Access。
- Workspace 治理規則與合規要求。
- Workspace Catalog 之定位（不建立條目）。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改既定 Architecture（AEOS-ARCH-001、AEOS-ARCH-004～AEOS-ARCH-009）。
- 具名 Workspace Catalog、實際 Workspace 清單或特定工具配置之建立。
- M5 Catalog／Matrix 資產之提前建立（Platform、Capability、Repository、Dependency、Workspace Catalog 與 Ownership、Dependency Matrix 均屬後續 EWO）。
- 個別 Platform、Capability、Repository 之內部技術架構、部署拓撲或實作設計。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。

## 3. Architecture Authority

Workspace Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| W0 | AEOS-ARCH-001 | 最高架構權威 |
| W1 | AEOS-ARCH-001 | 架構 Entry Document 與 Architecture Register |
| W2 | AEOS-ARCH-004 | 定義 Workspace Architecture 在 Enterprise Architecture 中的定位與 MUST |
| W3 | AEOS-ARCH-005～AEOS-ARCH-009 | 定義 Platform、Layer、Capability、Repository、Dependency 之邊界與關係 |
| W4 | AEOS-ARCH-010（本文件） | 定義 Workspace 模型、組成、邊界與治理規則 |
| W5 | Workspace Catalog、Repository 層級文件 | 登錄已核准 Workspace 事實並落實 |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的 Workspace 事實或改寫既定架構。
- Workspace 與 Platform、Capability、Repository MUST 分離定義：Workspace 是統合整體之邊界，Platform 是承載邊界，Capability 是能力定義，Repository 是治理與交付邊界。
- 發現 Approved 架構載體未涵蓋的 Workspace 需求時，MUST 先透過正式架構變更處理。

## 4. Workspace Definition

### 4.1 Formal Definition

Workspace 是 AI Engineering Workspace 之正式企業架構邊界：所有 Platform、Capability、Repository、治理資產與依賴共同形成的整體。Workspace 定義跨 Repository 協作、共享治理與整體一致性之邊界，並作為 Enterprise Architecture 之統合視圖。

一個架構元素只有在同時符合下列條件時，才可被認定為 Workspace：

- 具有可被唯一識別且跨版本延續的 Workspace ID。
- 具有明確的 Purpose／Mission、Boundary 與 Composition。
- 由受治理之 Platform 構成，並統合 Capability、Repository、Dependency 與 Ownership。
- 具有明確 Workspace Owner 與共享治理責任。
- 提供跨 Repository 協作與整體一致性檢查之架構依據。
- 生命週期由 Enterprise Architecture 管理，而非隨單一實作任意建立或移除。

### 4.2 Workspace Is Not

| 架構元素 | 與 Workspace 的區別 |
|----------|----------------------|
| Repository | Repository 是版本化治理與交付邊界；Workspace 是所有 Repository 共同形成之整體 |
| Platform | Platform 是承載一組 Capability 的穩定邊界；Workspace 由多個受治理之 Platform 構成 |
| Project | Project 是短期交付或專案邊界；Workspace 是持續之企業架構邊界 |
| Runtime Environment | Runtime Environment 是執行時期部署環境；Workspace 是企業架構層級之整體邊界 |
| Team／Organization | Team 是人員組織；Workspace Membership 可由人員或 Team 承擔，但 Workspace 不等同組織圖 |

## 5. Workspace Identity Model

每個 Workspace MUST 具備下列權威屬性：

| 屬性 | 必要性 | 定義 |
|------|--------|------|
| Workspace ID | MUST | 全 Workspace 唯一且不可重複使用的穩定識別碼 |
| Workspace Name | MUST | 正式名稱；命名變更不得改變 Workspace ID |
| Purpose／Mission | MUST | Workspace 存在目的與預期企業結果 |
| Boundary | MUST | Workspace 包含與排除的責任範圍 |
| Type／Level | MUST | 依 §7 指定之 Workspace 類型與層級 |
| Composition | MUST | 構成 Workspace 之 Platform、Capability、Repository 與治理資產引用 |
| Owner | MUST | 對 Workspace 完整性與演進負責的 accountable Owner |
| Membership | MUST | 具備正式身分之成員（Owner、Architecture Owner、Platform Owner、Repository Owner 等） |
| Lifecycle Status | MUST | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | MUST | 核准 Workspace 身分與邊界的 Architecture／ADR |

Workspace ID、Boundary 或 Composition 的實質變更 MUST 經 Architecture Review；不得只修改 Catalog 條目完成架構變更。

## 6. Workspace Boundary, Composition and Responsibility

### 6.1 Boundary Dimensions

Workspace Boundary MUST 同時從下列面向定義：

| 面向 | 必須回答的問題 |
|------|----------------|
| Purpose Boundary | Workspace 存在目的與預期企業結果為何？ |
| Composition Boundary | 哪些 Platform、Capability、Repository 與治理資產屬於 Workspace，哪些明確不屬於它？ |
| Governance Boundary | Workspace 層級共享治理涵蓋哪些範圍，哪些保留於 Repository 層級？ |
| Membership Boundary | 哪些角色與成員具備 Workspace 正式身分？ |
| Interaction Boundary | 跨 Repository、跨 Platform 之互動如何發生與治理？ |
| Information Boundary | 哪些架構資訊由 Workspace 擁有、提供或僅引用？ |

### 6.2 Composition

Workspace 由下列正式元素構成：

- Platform（受治理之承載邊界，AEOS-ARCH-005）。
- Capability（能力定義與結果，AEOS-ARCH-007）。
- Repository（版本化治理與交付邊界，AEOS-ARCH-008）。
- Dependency（正式依賴關係，AEOS-ARCH-009）。
- 治理資產（Architecture、Constitution、Standards、Policies、ADR 等）。

### 6.3 Boundary Rules

- 每項 Workspace Responsibility MUST 能對應至 Purpose／Mission 與至少一項正式組成元素。
- 同一責任不得由多個 Workspace 或元素在無 Ownership 決議的情況下同時宣稱最終 Authority。
- Repository 層級治理（AEOS-CON-001）保留於 Repository；Workspace 層級治理 MUST NOT 取代或繞過 Repository Governance。
- Workspace MUST NOT 直接管理超出其 Boundary 之實作細節或工具配置。

## 7. Workspace Types and Levels

### 7.1 Types

Approved 架構載體已核准之 Workspace 類型為 **Enterprise Workspace（AI Engineering Workspace）**，為目前唯一已核准類型。其他 Workspace 類型 MUST 先經正式架構變更與 Architecture Review 核准，不得由本文件或下位文件自行建立。

### 7.2 Levels

Workspace 依其整合層級分為三個正式層級：

| 層級 | 名稱 | 角色 |
|------|------|------|
| WS-1 | Workspace Level | 統合全部 Platform、Capability、Repository、Dependency 與治理資產之整體邊界 |
| WS-2 | Platform Level | 承載一組 Capability 並協調多個 Repository 之穩定邊界（AEOS-ARCH-005） |
| WS-3 | Repository Level | 版本化治理與交付之個別邊界（AEOS-ARCH-008） |

規則：

- 每一層級之責任 MUST 對應至 Layer Architecture 之責任分層（AEOS-ARCH-006 §4.2）。
- 層級間之互動 MUST 遵循 Dependency Architecture 之方向規則（AEOS-ARCH-009 §7）。

## 8. Workspace Relationship Model

### 8.1 Relationship Types

| 關係 | 語意 | 要求 |
|------|------|------|
| Composes | Workspace 由受治理之 Platform 構成（AEOS-ARCH-004 §7） | MUST 對應已核准 Platform |
| Integrates | Workspace 統合 Capability、Repository 與治理資產 | MUST 對應已核准架構資產 |
| Governs | Workspace 層級共享治理涵蓋跨元素治理面向 | MUST 限定範圍，不得取代 Repository Governance |
| Provides Context | Workspace 為跨 Repository 協作提供共同架構脈絡 | MUST 可追溯至 Platform、Capability、Repository、Ownership 與 Dependency |
| Enforces | Workspace 層級執行整體一致性檢查 | MUST 依正式 Architecture 與 Compliance 規則 |
| Collaborates With | Workspace 內之元素共同完成結果 | MUST 明確指定各方責任與 accountable Owner |

### 8.2 Relationship Rules

- Workspace View MUST 能追溯至 Platform、Capability、Repository、Ownership 與 Dependency 之正式資產（AEOS-ARCH-004 §7）。
- `Governs` 不得推定全面控制；Workspace 層級治理 MUST NOT 取代或繞過 Repository Governance。
- 跨 Repository、跨 Platform 之互動 MUST 透過正式 Interface 與 Dependency 發生（AEOS-ARCH-005 §9、AEOS-ARCH-009）。
- 關係 MUST 有方向、類型、Owner、依據與生命週期狀態。

## 9. Workspace Ownership and Membership

### 9.1 Roles

| 角色 | 責任 |
|------|------|
| Workspace Owner | 對 Workspace Purpose、Boundary、Composition 與演進負最終責任 |
| Architecture Owner | 維護 Enterprise Architecture 一致性、裁決跨元素衝突及核准重大變更 |
| Platform Owner | 維護 Platform 邊界與 Platform Catalog 條目一致性 |
| Capability Owner | 維護 Capability 定義、結果及演進 |
| Repository Owner | 確保 Repository 符合 Workspace 與 Enterprise Architecture |
| Review Owner | 依 AEOS-STD-005 確認 Workspace 變更已完成正式 Review |

### 9.2 Membership Rules

- 每個 Active Workspace MUST 有且只有一個 accountable Workspace Owner 角色。
- Membership MUST 具有正式身分與治理責任；未經核准之成員不具 Workspace 架構權限。
- Workspace Owner 可委派執行工作，但 MUST NOT 委派最終 Accountability。
- Ownership 缺失、重疊或無法履行時，Workspace MUST NOT 進入 Active 狀態。

## 10. Workspace Lifecycle

| 狀態 | 定義 | 必要條件 |
|------|------|----------|
| Candidate | 已提出但尚未成為正式 Workspace | 具 Purpose、初步 Boundary、提案 Owner 與 Architecture Reference |
| Active | 已核准並承擔正式 Workspace 責任 | 完整 Identity、Composition、Owner、Membership 與治理記錄 |
| Deprecated | 仍受支援但不得承接新的策略性責任 | 具替代方向、Migration Plan、期限與受影響項目 |
| Retired | 已停止承擔正式責任 | 相關 Platform、Capability、Repository 與依賴已移轉或終止 |

允許的主要狀態轉移為：

- Candidate → Active：完成 Architecture Review 並登錄 Workspace Catalog。
- Candidate → Retired：提案撤回或核准不成立，保留決策紀錄。
- Active → Deprecated：核准取代或合併策略並建立 Migration Plan。
- Deprecated → Active：取代決策撤銷且重新完成 Architecture Review。
- Deprecated → Retired：Migration 完成且無未處理責任或依賴。

任何跳過 Deprecated 的 Active → Retired 轉移 MUST 具有緊急理由、影響分析與 Architecture Owner 核准。

## 11. Provisioning

Workspace Provisioning 指 Workspace 層級元素（Platform、Repository、治理資產）之建立與啟用程序，其規則為：

- 建立 Platform、Repository 或治理資產 MUST 依其對應 Architecture 之 Lifecycle 與 Change 規則進行，並追溯至 Workspace Composition。
- Provisioning MUST NOT 繞過 Architecture Review 或 Repository Governance；不得以工具配置取代架構核准。
- 本文件不建立特定工具配置；工具與平台選擇屬 Production Repositories 責任（AEOS-ARCH-004 §3.3.2）。
- Provisioning 完成後 MUST 同步更新 Workspace Composition 與相關 Register。

## 12. Change and Access

### 12.1 Change Rules

下列變更屬於 Architecture Change，MUST 經 EWO 與 Architecture Review：

- 建立、合併、拆分、取代或退役 Workspace。
- 變更 Workspace Purpose、Type／Level、Boundary 或 Composition。
- 移轉關鍵 Platform、Capability、Repository 或 Workspace Ownership。
- 變更 Workspace 層級共享治理範圍。

每項變更 MUST：

1. 說明原因、預期結果與不變範圍。
2. 識別受影響之 Platform、Capability、Repository、Owner 與 Dependency。
3. 評估責任重疊、孤立能力、循環依賴及相容性風險。
4. 定義 Migration、Rollback、版本與生命週期策略。
5. 更新 Architecture、Register、Workspace Catalog 與相關 Matrix。
6. 依重大程度建立或引用 ADR。

### 12.2 Access Rules

- Workspace 層級架構資產之存取與變更 MUST 依正式 Membership 與 Review 規則執行。
- 未具備正式身分之成員 MUST NOT 變更 Workspace 層級架構資產。
- 存取控制之具體實作屬 Production Repositories 與工具配置責任；本文件僅定義治理邊界。

## 13. Compliance

Workspace Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Workspace 可追溯至 AEOS-ARCH-001 與 Approved 架構載體 |
| Identity | 具唯一、穩定且不可重用的 Workspace ID |
| Composition | 組成元素（Platform、Capability、Repository、Dependency）已識別且可追溯 |
| Boundary | Purpose、責任、包含與排除範圍明確 |
| Type／Level | 類型與層級明確，無未經核准之新類型 |
| Relationship | Workspace View 可追溯至全部架構資產 |
| Ownership | 具有唯一 accountable Workspace Owner，無未解決責任重疊 |
| Membership | 成員具正式身分與治理責任 |
| Lifecycle | 狀態、轉移條件、Migration 與替代關係完整 |
| Provisioning | 元素建立循對應 Architecture 與治理程序，無繞過 |
| Change／Access | 變更經 EWO 與 Review；存取依正式 Membership |
| Catalog Readiness | 未經核准不建立具名 Workspace 條目；Catalog 與 M5 資產待後續 EWO |
| Asset Consistency | Architecture、Register、Catalog 與 Matrix 狀態一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 14. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved v1.0.0） | Architecture | 架構基線與 Architecture Register |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved v1.0.0） | Architecture | Governance 階層與領域 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved v1.0.0） | Architecture | 重大架構決策紀錄機制 |
| REF-005 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Workspace Architecture 的上位定位與 MUST |
| REF-006 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved v1.0.0） | Architecture | Platform 承載與組成關係 |
| REF-007 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved v1.0.0） | Architecture | 責任分層與依賴方向規則 |
| REF-008 | [AEOS-ARCH-007 — Capability Architecture](AEOS-ARCH-007-Capability-Architecture.md)（Approved v1.0.0） | Architecture | Capability 定義與關係 |
| REF-009 | [AEOS-ARCH-008 — Repository Architecture](AEOS-ARCH-008-Repository-Architecture.md)（Approved v1.0.0） | Architecture | Repository 治理與交付邊界 |
| REF-010 | [AEOS-ARCH-009 — Dependency Architecture](AEOS-ARCH-009-Dependency-Architecture.md)（Approved v1.0.0） | Architecture | 依賴方向與治理規則 |
| REF-011 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved v1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-012 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-013 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-014 | EWO-AEOS-0020 | EWO | 本文件之工作來源 |
| REF-015 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 2（AEOS-ADR-002 已核准）：執行 Architecture Transition——WA-001 分類為歷史來源（Historical Reference）；Authority 階層（W0）與對應來源重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併；狀態更新為 Approved，成為 AEOS Workspace Architecture 正式定義（EWO-AEOS-0021；AR-AEOS-0021-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Workspace Identity、Composition、Boundary、Type／Level、Relationship、Ownership、Membership、Lifecycle、Provisioning、Change、Access 與 Compliance（EWO-AEOS-0020） | Codex |
