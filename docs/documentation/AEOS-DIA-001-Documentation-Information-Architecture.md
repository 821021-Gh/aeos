---
doc-id: AEOS-DIA-001
doc-name: Documentation Information Architecture
doc-type: Information Architecture
repository: AEOS
version: 3.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-05
updated: 2026-08-06
related:
  - EWO-AEOS-0003
  - EWO-AEOS-0022
  - EWO-AEOS-0033
  - EWO-AEOS-0037
  - AEOS-ARCH-001
  - WA-001
---

# AEOS-DIA-001 — Documentation Information Architecture

> EWO-AEOS-0003：依 AEOS-ARCH-001 與 WA-001 建立 AEOS 之 Enterprise Documentation Information Architecture。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-DIA-001 |
| 文件名稱 | Documentation Information Architecture |
| 型別 | Information Architecture |
| 狀態 | Approved |
| 版本 | 3.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-05 |
| 最後更新 | 2026-08-06 |
| 依據文件 | AEOS-ARCH-001、WA-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0003 |

## 1. Purpose

本文件定義 AEOS 之 Enterprise Documentation Information Architecture（DIA），其目的為：

- 建立 AEOS 正式文件體系之設計原則、分類（Taxonomy）、資訊架構、目錄組織、擁有權、交叉引用、生命週期、治理與擴充規則。
- 使任何文件之建立、定位、引用與演進有一致之決策依據。
- 界定文件資訊架構與 Enterprise Architecture 之界線：本文件管理「文件如何組織」，不重新設計架構，不重複 WA-001 內容。

注意：本文件不是 Repository Structure，也不是 Documentation Standard；其定位為文件體系之 Enterprise Information Architecture。

## 2. Documentation Design Principles

| # | 原則 | 說明 |
|---|------|------|
| DP-001 | Documentation First | 正式文件為 AEOS 之主要交付物。 |
| DP-002 | Single Source of Truth | 每一文件主題僅存在一個權威文件；其他文件以引用取代重述。 |
| DP-003 | Architecture Traceability | 架構相關文件 MUST 宣告其架構來源（AEOS-ARCH-001／WA-001）並可追溯。 |
| DP-004 | Content Boundary | 文件內容 MUST NOT 超出其宣告之 Scope。 |
| DP-005 | Decoupled Foundation | Repository Foundation 與架構／治理內容分離管理。 |
| DP-006 | Formal by Default | 文件以正式內容交付，MUST NOT 使用 Placeholder。 |
| DP-007 | Consistent Identity | 每份正式文件具有唯一 doc-id，命名與目錄位置依本文件定義。 |

## 3. Documentation Taxonomy

AEOS 正式文件依下列分類（Taxonomy）管理；分類碼為 doc-id 之前綴。

| 分類碼 | 類別 | 用途 |
|--------|------|------|
| ARCH | Architecture | 承載 Enterprise Architecture 之正式文件。 |
| DIA | Documentation Information Architecture | 承載文件資訊架構之正式文件（本文件）。 |
| SPEC | Specification | 定義正式規格，支援 Specification Driven 開發。 |
| CON | Constitution | 定義 Repository 身分與治理基礎。 |
| GOV | Governance | 承載 Platform Governance 之正式文件。 |
| CAP | Capability | 承載 Capability Management 之正式文件。 |
| ADR | Architecture Decision Record | 記錄架構決策之背景、決策、理由與影響。 |
| POL | Policy | 承載政策類正式文件（Platform Governance 政策）。 |
| STD | Standard | 承載標準類正式文件（工程、文件、API 等標準）。 |
| REF | Reference | 承載參考資料（術語、對照、外部來源對應等）。 |
| IDX | Index | 承載索引（文件索引、ADR Register 等）；不承載 Catalog／Matrix 文件（CAT／MAT 依 AEOS-STD-006）。 |
| CAT | Catalog | 登錄已核准架構或治理實體之正式目錄；Schema 依 AEOS-STD-006。 |
| MAT | Matrix | 登錄已核准關係（Ownership、Dependency 等）之正式矩陣；Schema 依 AEOS-STD-006。 |
| RPT | Report | 承載正式分析結果（架構盤點、成熟度評估、Readiness Assessment、影響分析等）；Assessment 為 Report 之正式用途分類。 |
| TPL | Template | 承載文件與工作單範本。 |

命名規則：`<分類碼>-<###>-<Kebab-Case-Name>.md`，前置 Repository 識別 `AEOS-`（例如 `AEOS-ARCH-001-Architecture-Baseline.md`）。

Report（RPT）規則：

- 適用範圍：正式分析結果（架構盤點、成熟度評估、Readiness Assessment、影響分析等）。
- 禁止用途：不得承載 Architecture 決策（ADR）、Standard（STD）、Policy（POL）、Catalog（CAT）或 Matrix（MAT）內容；不得以 Report 取代 EWO、Review Record 或正式決策文件。
- Assessment 為 Report 之正式用途分類，不另立獨立 Artifact Type；Readiness Assessment 等以 Report 型別承載。
- Architecture Candidate Assessment 為 Report 之正式用途分類；承載候選識別、來源證據、Scope、Boundary、Responsibilities、Exclusions、重疊分析、條件、拒絕或延後理由及 Review Outcome 摘要。
- Architecture Candidate Assessment RPT 不得承載或取代 Architecture Definition、ADR、Catalog Entry 或正式 Review Record；不得聲稱取代 Review Record。
- Approved Architecture Candidate Assessment RPT 可作為長期分析 Fact Authority；其 Review Decision 必須追溯至對應之 AR Review Record（PR 載體）。
- Architecture Candidate Assessment RPT 之適用 Review Type 為 AR（Architecture Review）；不另疊加 RT 作為第二核准路徑。

## 4. Documentation Information Architecture

AEOS 文件體系分為四個層級：

| 層級 | 角色 | 內容 |
|------|------|------|
| L1 Entry | Repository 入口 | README.md（Purpose、Structure、Workflow、Referenced Documents）。 |
| L2 Baseline | 架構入口 | AEOS-ARCH-001（Architecture Baseline／Entry Document）。 |
| L3 Domain | 分類文件 | 依 Taxonomy 分類之正式文件（SPEC／CON／GOV／CAP 等）。 |
| L4 Reference | 外部來源 | WA-001（Approved v1.0.0）等架構權威來源。 |

資訊架構規則：

- 讀者路徑：README → AEOS-ARCH-001 → L3 分類文件。
- 每份文件 MUST 可經由 doc-id 被唯一識別。
- 文件間以 Reference 連結（§7），不以內容複製連結。
- 架構內容之權威來源為 AEOS-ARCH-001 與 WA-001；L3 文件引用而非重述。

## 5. Directory Organization

正式文件置於 `docs/` 之下，依 Taxonomy 對應目錄：

```
docs/
├── architecture/      — ARCH 文件
├── adr/               — ADR 文件
├── documentation/     — DIA 文件
├── specifications/    — SPEC 文件
├── constitution/      — CON 文件
├── policies/          — POL 文件
├── standards/         — STD 文件
├── governance/        — GOV 文件
├── capability/        — CAP 文件
├── catalogs/          — CAT 文件
├── matrices/          — MAT 文件
├── reports/           — RPT 文件
├── references/        — REF 文件
├── indexes/           — IDX 文件
└── templates/         — TPL 文件
```

規則：

- 文件 MUST 置於其分類對應之目錄。
- 目錄僅在首次存在實際文件需求時建立（不建立空目錄／Placeholder）。
- 檔案路徑變更 MUST 同步更新所有交叉引用（§7）。

## 6. Ownership Model

| 角色 | 擁有範圍 | 職責 |
|------|----------|------|
| Repository Owner | Repository 層級 | 最終核准者；治理與 Foundation 文件之擁有者。 |
| Architecture Owner | ARCH、DIA 文件 | 架構與文件資訊架構文件之內容正確性與維護。 |
| Document Owner | 單份文件 | 由文件 frontmatter 宣告；負責維護、Review 回覆與變更管理。 |
| Review Owner | Review 流程 | 負責 Architecture Review、Repository Review、Specification Review 與 Governance Review；不等同 Repository Owner。 |

規則：

- 每份正式文件 MUST 於 frontmatter 宣告 owner。
- 文件變更由 Document Owner 主導，經 EWO 定義範圍。
- 核准層級：正式文件合併至 main 前 MUST 經 Repository Owner 核准（Review）。
- Review Owner 負責 Review，Repository Owner 負責最終核准；兩者為不同角色。

## 7. Cross-reference Strategy

- 引用形式：跨文件引用 MUST 使用 doc-id 與相對路徑；MUST NOT 複製被引用內容。
- 來源宣告：架構相關文件 MUST 於 References 宣告 AEOS-ARCH-001／WA-001 來源對應。
- 樞紐文件：AEOS-ARCH-001（Architecture Register）為架構文件之引用樞紐。
- 失效處理：被引用文件變更或移動時，引用方 MUST 於同一 EWO 或 RC 更新引用；不得保留失效連結。

## 8. Lifecycle

文件狀態：

| 狀態 | 定義 |
|------|------|
| Draft | 文件建立中，尚未提交 Review。 |
| Review | 已提交 Draft PR，等待 Architecture／Repository Review。 |
| Approved | Review 通過，文件內容正式核定。 |
| Released | 合併至 main，成為正式基線內容。 |
| Archived | 不再有效，保留歷史紀錄。 |
| Deprecated | 文件已由新文件取代；保留既有引用，不得再新增內容。 |

規則：

- 狀態於 frontmatter 記錄（status）。
- 版本依 SemVer 管理；Review 修正（RC）更新 minor，重大變更更新 major。
- 每次狀態或版本變更 MUST 更新 Revision History。

## 9. Documentation Evolution Principles

本文件之長期演進依下列原則：

| # | 原則 | 說明 |
|---|------|------|
| EP-001 | Backward Compatibility | 文件體系之演進須保持向後相容；既有 doc-id、路徑與引用不因演進失效。 |
| EP-002 | Stable Identity | 文件 doc-id 一經建立即穩定不變；內容演進不改變身分。 |
| EP-003 | Incremental Evolution | 文件體系以增量方式演進，每次變更經 EWO 定義並可追溯。 |
| EP-004 | No Duplicate Knowledge | 同一知識僅存在於單一權威文件；以 Deprecate 與引用取代複製。 |

## 10. Governance

- 本文件（Taxonomy、Directory Organization、命名規則）之變更 MUST 經 EWO 與 Review。
- 正式文件合併至 main 前 MUST 通過 Review。
- 文件不得重新定義 AEOS-ARCH-001／WA-001 已定義之架構內容。
- Repository Foundation 文件（README 等）不描述架構內容。

## 11. Extension Rules

- 新增文件：依 Taxonomy 選擇分類 → 產生唯一 doc-id → 置於對應目錄 → 宣告 Owner、Status 與 References。
- 新增分類：需先擴充 §3 Taxonomy 與 §5 Directory Organization（經 EWO）。
- 新增目錄：僅在實際文件需求存在時建立。
- 版本相容：Taxonomy 或 Directory Organization 之變更視為 major 變更。

## 12. References

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | AEOS 架構基線與 Entry Document |
| WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard | Standard | Catalog（CAT）與 Matrix（MAT）型別與 Schema 規範 |
| EWO-AEOS-0003 — Documentation Information Architecture | EWO | 本文件之工作來源 |

## 13. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 3.1.0 | 2026-08-06 | 依 EWO-AEOS-0037 新增 Report 正式用途分類 Architecture Candidate Assessment：定義可承載內容（候選識別、來源證據、Scope、Boundary、Responsibilities、Exclusions、重疊分析、條件、拒絕/延後理由及 Review Outcome 摘要）、禁止用途（不得承載或取代 Architecture Definition、ADR、Catalog Entry 或正式 Review Record）、Fact Authority 定位（Approved RPT 為長期分析依據，Review Decision 追溯至 AR Review Record）與 Review Type（AR，不疊加 RT）（AR-AEOS-0037-R1） | Codex |
| 3.0.0 | 2026-08-06 | 依 EWO-AEOS-0033 擴充 Taxonomy：新增 Report（RPT）型別；Assessment 定義為 Report 之正式用途分類（不另立 Artifact Type）；定義 RPT 適用範圍與禁止用途；Directory Organization 新增 `docs/reports/`（AR-AEOS-0033-R1） | Codex |
| 2.0.0 | 2026-08-06 | 依 EWO-AEOS-0022 擴充 Taxonomy：新增 CAT（Catalog）與 MAT（Matrix）為正式且相互獨立之文件型別；IDX 不再承載 Catalog；Directory Organization 新增 `docs/catalogs/` 與 `docs/matrices/`；Catalog／Matrix Schema 統一由 AEOS-STD-006 規範 | Codex |
| 1.0.0 | 2026-08-05 | 正式定版為 Documentation Information Architecture Foundation：擴充 Taxonomy（ADR／POL／STD／REF／IDX／TPL）、同步 Directory Organization、新增 Review Owner、Lifecycle 新增 Deprecated、新增 Documentation Evolution Principles（DR-AEOS-0003-R1） | Codex |
| 0.1.0 | 2026-08-05 | 初版：建立 AEOS Documentation Information Architecture（EWO-AEOS-0003） | Codex |
