---
doc-id: AEOS-STD-006
doc-name: Enterprise Architecture Catalog and Matrix Standard
doc-type: Standard
repository: AEOS
version: 1.0.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0022
  - EWO-AEOS-0023
  - SR-AEOS-0023-R1
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-008
  - AEOS-ARCH-009
  - AEOS-ARCH-010
  - AEOS-DIA-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-STD-002
  - AEOS-STD-003
  - AEOS-STD-004
  - AEOS-STD-005
  - WA-001
---

# AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard

> EWO-AEOS-0022：依 Chief Architect 裁定，建立 Catalog（CAT）與 Matrix（MAT）之正式文件型別、統一 Schema、Entry ID／Relationship ID 命名框架、Lifecycle、Review、Approval、Change、Traceability 與一致性規則。本文件為 AEOS 所有 Catalog 與 Matrix 之唯一規範（Single Source of Truth）；不是 Catalog 內容，不是 Matrix 內容，不是 Documentation Format。

## Executive Summary

本文件定義 AEOS Enterprise Architecture Catalog 與 Matrix 之標準，涵蓋文件型別（CAT／MAT）、目錄歸屬、統一 Schema、Entry ID 與 Relationship ID 命名框架、Traceability、Lifecycle、Review、Approval、Change 與一致性規則。Catalog 與 Matrix 只登錄可追溯至 Approved Architecture 或正式決策之事實，禁止自行創造新的 Platform、Capability、Repository、Workspace、Ownership 或 Dependency。本文件不建立任何 Catalog 或 Matrix，不登錄任何具名條目，也不定義工具配置。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-006 |
| 文件名稱 | Enterprise Architecture Catalog and Matrix Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0022、EWO-AEOS-0023、SR-AEOS-0023-R1、AEOS-DIA-001、AEOS-STD-001～AEOS-STD-005、AEOS-ARCH-001、AEOS-ARCH-004～AEOS-ARCH-010、AEOS-GOV-001、WA-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0023、SR-AEOS-0023-R1、AEOS-ARCH-001～AEOS-ARCH-010、AEOS-CON-001、AEOS-DIA-001、AEOS-GOV-001、AEOS-STD-001～AEOS-STD-005、WA-001 |

## 1. Purpose

本文件之目的為：

- 建立 Catalog（CAT）與 Matrix（MAT）為正式且相互獨立之文件型別，並定義其目錄歸屬與 doc-id 格式。
- 定義 Catalog／Matrix 之統一 Schema、Metadata、Entry ID、Relationship ID、Ownership、Lifecycle、Review、Approval、Change、Traceability 與一致性規則。
- 確保 Catalog 與 Matrix 只登錄可追溯至 Approved Architecture 或正式決策之事實（AEOS-ARCH-004 §8）。
- 使 Matrix 引用正式 Catalog Entry，不以無法解析之自由文字作為架構元素身分。
- 解決 M5 — Enterprise Architecture Catalogs 與 AEOS-GOV-001 既有里程碑命名之歧義（對應 AEOS-GOV-001 §2.1）。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- Catalog（CAT）與 Matrix（MAT）文件型別、目錄歸屬與 doc-id 格式。
- Catalog／Matrix 之統一 Schema 與 Metadata。
- Catalog Entry ID 與 Ownership／Dependency Relationship ID 之命名框架。
- Traceability 與 Fact Authority 規則。
- Matrix 引用規則。
- 文件與條目之 Lifecycle、Review、Approval 與 Change 規則。
- 一致性規則與合規檢查。

### 2.2 Out of Scope

本標準明確不涵蓋：

- 建立任何 Catalog 或 Matrix 文件（Platform、Capability、Repository、Workspace Catalog 與 Ownership、Dependency Matrix 均屬後續 EWO）。
- 登錄任何具名 Catalog Entry 或關係條目。
- 各 Catalog 之專屬欄位設計（專屬擴充 MUST 依 §5.2 經本標準核准）。
- 工具配置、Runtime Topology、Deployment Architecture 或 Source Code Implementation。
- Catalog 內容之重新設計或既定 Architecture 之改寫。

## 3. Catalog and Matrix Model

### 3.1 Definitions

| 資產 | 定義 |
|------|------|
| Catalog | 登錄已核准架構或治理實體之正式目錄（如 Platform、Capability、Repository、Workspace）。 |
| Matrix | 登錄已核准關係之正式矩陣（如 Ownership、Dependency）。 |
| Entry | Catalog 內登錄之單一已核准實體。 |
| Relationship | Matrix 內登錄之單一已核准關係。 |
| Fact Authority | 可登錄事實之唯一來源：Approved Architecture 或正式決策（ADR／Review Decision）。 |

### 3.2 Fact Authority Principle

- Catalog 與 Matrix 只登錄可追溯至 Fact Authority 之事實（AEOS-ARCH-004 §8）。
- Catalog 與 Matrix MUST NOT 自行創造新的 Platform、Capability、Repository、Workspace、Ownership 或 Dependency。
- 條目事實與 Architecture 衝突時，以 Architecture 為準並啟動修正；不得以 Catalog 註記取代架構修正。

### 3.3 Catalog vs Index

- Catalog（CAT）為正式且獨立之文件型別，MUST NOT 沿用 Index（IDX）型別（Chief Architect 裁定）。
- Index（IDX）僅承載不登錄具名條目之總覽清單（如 Document Index）；登錄具名架構或治理實體之目錄一律使用 CAT 型別。

## 4. Document Types and Directory Placement

### 4.1 Document Types

| 型別 | 用途 | 目錄 | doc-id 格式 |
|------|------|------|-------------|
| CAT | Catalog（登錄已核准架構或治理實體） | `docs/catalogs/` | `AEOS-CAT-###` |
| MAT | Matrix（登錄已核准關係） | `docs/matrices/` | `AEOS-MAT-###` |

### 4.2 Rules

- CAT 與 MAT 為相互獨立之型別；一項資產 MUST NOT 同時以 CAT 與 MAT 承載。
- Catalog／Matrix 文件 MUST 置於其對應目錄；目錄僅在實際文件需求存在時建立（AEOS-DIA-001 §5）。
- doc-id 流水號 MUST 各型別獨立編號（AEOS-STD-004 §6.1）。
- CAT／MAT 之 Schema 由本標準統一定義，MUST NOT 在各 Catalog 重複設計。

## 5. Unified Catalog Schema

### 5.1 Mandatory Entry Fields

每個 Catalog Entry MUST 具備下列欄位：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Entry ID | MUST | 依 §6 命名框架之唯一識別碼 |
| Entry Name | MUST | 正式名稱；命名變更不得改變 Entry ID |
| Type／Classification | MUST | 依對應 Architecture 之類型或分類 |
| Status | MUST | 依 §9.2 條目 Lifecycle 狀態 |
| Owner | MUST | 對條目完整性與演進負責之 accountable Owner |
| Architecture Reference | MUST | 核准此條目之 Approved Architecture／ADR |
| Validated Facts | MUST | 可追溯之事實欄位（依對應 Architecture 定義） |
| Related Entries | MUST | 引用其他 Catalog Entry ID；無則為空 |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 條目新增、修改、移除之歷程 |

### 5.2 Extension Rules

- 各 Catalog MAY 增加專屬欄位，但 MUST 經本標準之 Amendment 或於該 Catalog 之 EWO 中經 Review 核准，且不得與本節 Mandatory Fields 衝突。
- 專屬欄位 MUST NOT 取代統一 Schema；統一欄位之定義以本標準為唯一權威。

### 5.3 Matrix Record Fields

每個 Matrix Relationship MUST 具備下列欄位：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Relationship ID | MUST | 依 §6.2 命名框架之唯一識別碼 |
| Relationship Type | MUST | Ownership、Dependency 等正式類型 |
| Direction | MUST（Dependency） | Source → Target；依 AEOS-ARCH-009 §7 |
| Source Entry Reference | MUST | 正式 Catalog Entry ID |
| Target Entry Reference | MUST | 正式 Catalog Entry ID |
| Strength／RACI | MUST（依類型） | Dependency Strength 或 Ownership RACI |
| Owner | MUST | 對關係負責之 accountable Owner |
| Status | MUST | 依 §9.2 條目 Lifecycle 狀態 |
| Architecture Reference | MUST | 核准此關係之 Approved Architecture／ADR |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 關係新增、修改、移除之歷程 |

## 6. Entry ID and Relationship ID Naming Framework

### 6.1 Entry ID Framework

Catalog Entry ID MUST 依下列框架命名（格式範例，非實際條目）：

| Catalog | Entry 類型 | 前綴 | 格式 |
|---------|-----------|------|------|
| Platform Catalog | Platform | `PLT` | `PLT-###` |
| Capability Catalog | Capability | `CPB` | `CPB-###` |
| Repository Catalog | Repository | `REP` | `REP-###` |
| Workspace Catalog | Workspace | `WS` | `WS-###` |

### 6.2 Relationship ID Framework

Matrix Relationship ID MUST 依下列框架命名（格式範例，非實際條目）：

| Matrix | 關係類型 | 前綴 | 格式 |
|--------|----------|------|------|
| Ownership Matrix | Ownership | `OWN` | `OWN-###` |
| Dependency Matrix | Dependency | `DEP` | `DEP-###` |

### 6.3 ID Rules

- Entry ID 與 Relationship ID MUST 全 Workspace 唯一。
- Entry ID／Relationship ID 一經發布 MUST NOT 變更或重用；Retired 後永久保留（AEOS-STD-004 §6.5）。
- ID 格式 MUST 為 `<PREFIX>-###`；`###` 零填充至三位。
- 前綴 MUST 依本節表格使用；MUST NOT 被其他類別佔用或仿冒。
- 新類型之 Entry／Relationship ID 框架 MUST 經本標準之 Amendment，不得由各 Catalog／Matrix 自行定義。

## 7. Traceability and Fact Authority

### 7.1 Traceability Requirements

- 每個 Catalog Entry 與 Matrix Relationship MUST 宣告 Architecture Reference（Approved Architecture 或 ADR／Review Decision）。
- 無 Architecture Reference 之條目 MUST NOT 登錄。
- 條目之 Validated Facts 與 Related Entries MUST 可追溯至既有已核准事實。

### 7.2 Forbidden Facts

Catalog 與 Matrix MUST NOT 自行創造下列事實：

- 新的 Platform、Capability、Repository 或 Workspace 身分。
- 新的 Ownership 或 Dependency 關係。
- 未經 Architecture Review 核准之邊界、分類、強度或方向變更。

發現未經核准事實時，MUST 依 AEOS-ARCH-009 §13 Violation Governance 與 AEOS-CON-001 變更管理處理。

## 8. Matrix Reference Rules

- Matrix MUST 以正式 Catalog Entry ID 引用架構元素身分。
- Matrix MUST NOT 使用無法解析之自由文字作為架構元素身分。
- 引用之 Entry MUST 存在於對應 Catalog；引用 Retired Entry MUST 明確標註歷史用途。
- 無法解析之引用視為一致性違規（§11）。

## 9. Lifecycle

### 9.1 Document Lifecycle

Catalog／Matrix 文件之狀態依下列規則：

| 狀態 | 定義 |
|------|------|
| Draft | 文件建立中，尚未提交 Review。 |
| Approved | Review 通過，文件內容正式核定。 |
| Deprecated | 文件已由新文件取代；保留既有引用，不得再新增內容。 |
| Retired | 文件停止承擔正式責任；保留歷史。 |

### 9.2 Entry Lifecycle

Catalog Entry 與 Matrix Relationship 之狀態依下列規則：

| 狀態 | 定義 |
|------|------|
| Candidate | 已提出但尚未成為正式條目。 |
| Active | 已核准並承擔正式架構事實。 |
| Deprecated | 仍受支援但不得承接新的策略性責任。 |
| Retired | 已停止承擔正式責任；ID 永久保留。 |

### 9.3 Lifecycle Rules

- 每次狀態變更 MUST 更新 Change Record、Version 與 Review Date。
- Deprecated 條目 MUST 保留歷史與替代關係；Retired 條目 MUST NOT 被重新啟用或重用 ID。
- 文件狀態與條目狀態 MUST 分開管理；文件 Approved 不代表其中條目全部 Active。

## 10. Review, Approval and Change

### 10.1 Document Review

- Catalog／Matrix 文件合併至 main 前 MUST 經 Review（AEOS-STD-005 §3）。
- 文件 Review 使用 Catalog／Matrix Review（Review ID 前綴 `CM`，依 AEOS-STD-005 §4、AEOS-STD-004 §6.3）。

### 10.2 Entry Change Review

下列變更 MUST 經 EWO 與 Review：

- 條目新增（Candidate → Active）：MUST 具 Architecture Reference 並經 Architecture Review。
- 條目修改：變更 Identity、Boundary、Type／Classification、Owner 或 Strength 視為重大變更，MUST 經 Review。
- 條目移除：MUST 依 Deprecated → Retired 流程執行，不得直接刪除。
- 無 Architecture Reference 之任何新增或修改 MUST NOT 登錄。

### 10.3 Relationship Change Review

- 新增、修改或移除 Ownership／Dependency 關係 MUST 經 Review。
- Dependency 關係變更 MUST 依 AEOS-ARCH-009 §12 執行變更影響分析。
- 未核准之循環依賴 MUST NOT 登錄（AEOS-ARCH-009 §12.2）。

### 10.4 Approval

- 最終核准由 Repository Owner 執行（Human Final Decision，AEOS-STD-005 §3）。
- 文件升版至 Approved 依 AEOS-STD-005 Review Workflow 執行。

## 11. Consistency Rules

Catalog 與 Matrix MUST 符合下列一致性規則：

- 無重複 Entry ID／Relationship ID。
- 無孤兒引用：Matrix 引用之 Entry 存在於對應 Catalog。
- 無未核准循環依賴；循環依賴 MUST 有 Architecture 核准依據。
- Workspace Catalog 之 Composition 引用之元素存在於對應 Catalog。
- 與 AEOS-ARCH-001 §8 Register 及相關 Architecture 之狀態一致。
- 變更後 MUST 重新執行一致性驗證（格式、Metadata、Cross-reference、Placeholder）。

## 12. Compliance

Catalog／Matrix 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Type & Directory | 文件型別為 CAT／MAT 且置於對應目錄 |
| Schema | 條目符合 §5 統一 Schema，無各 Catalog 重複設計 |
| ID Framework | Entry／Relationship ID 符合 §6 命名框架且唯一 |
| Traceability | 每個條目具 Architecture Reference，無未核准事實 |
| Matrix Reference | Matrix 引用正式 Catalog Entry ID，無自由文字身分 |
| Lifecycle | 文件與條目狀態依 §9 管理，Change Record 完整 |
| Review | 新增／修改／移除與關係變更經 Review（CM） |
| Consistency | 無孤兒引用、重複 ID、未核准循環依賴；Register 一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本標準之 Catalog 或 Matrix MUST NOT 被視為 AEOS 正式架構資產。

## 13. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md) | Architecture Entry Document | Architecture Register 與架構基線 |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Catalog／Matrix 資產定位（§8） |
| REF-004 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved v1.0.0） | Architecture | Platform Catalog 欄位與 Lifecycle |
| REF-005 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved v1.0.0） | Architecture | 依賴方向與責任分層 |
| REF-006 | [AEOS-ARCH-007 — Capability Architecture](AEOS-ARCH-007-Capability-Architecture.md)（Approved v1.0.0） | Architecture | Capability Catalog 與 Ownership |
| REF-007 | [AEOS-ARCH-008 — Repository Architecture](AEOS-ARCH-008-Repository-Architecture.md)（Approved v1.0.0） | Architecture | Repository Catalog 與類型 |
| REF-008 | [AEOS-ARCH-009 — Dependency Architecture](AEOS-ARCH-009-Dependency-Architecture.md)（Approved v1.0.0） | Architecture | Dependency Matrix 與依賴規則 |
| REF-009 | [AEOS-ARCH-010 — Workspace Architecture](AEOS-ARCH-010-Workspace-Architecture.md)（Approved v1.0.0） | Architecture | Workspace Catalog 與 Composition |
| REF-010 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | Taxonomy 與 Directory Organization |
| REF-011 | [AEOS-GOV-001 — Enterprise Governance Roadmap](../governance/AEOS-GOV-001-Enterprise-Governance-Roadmap.md)（Approved v1.0.0） | Governance | M5 里程碑命名對齊 |
| REF-012 | [AEOS-STD-001 — Documentation Format Standard](AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-013 | EWO-AEOS-0022 | EWO | 本文件之工作來源 |

## 14. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | Standard Review 核准並合併；狀態更新為 Approved，成為 AEOS Enterprise Architecture Catalog and Matrix Standard 正式定義（EWO-AEOS-0023；SR-AEOS-0023-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 Chief Architect 裁定定義 CAT／MAT 型別、目錄歸屬、統一 Schema、Entry ID／Relationship ID 命名框架、Traceability、Lifecycle、Review、Approval、Change 與一致性規則（EWO-AEOS-0022） | Codex |
