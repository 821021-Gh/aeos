---
doc-id: AEOS-STD-002
doc-name: Metadata Standard
doc-type: Standard
repository: AEOS
version: 1.1.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0009
  - SR-AEOS-0009-R1
  - AEOS-ARCH-001
  - AEOS-DIA-001
  - AEOS-CON-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-ADR-002
  - WA-001
---

# AEOS-STD-002 — Metadata Standard

> EWO-AEOS-0009：依 AEOS-ADR-002、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001 與 AEOS-STD-001 建立 AEOS 之 Metadata Standard。本文件為所有正式文件之唯一 Metadata 規範；不是 Frontmatter Template，不是 Documentation Format。

## Executive Summary

本文件定義 AEOS 正式治理文件之 Metadata 標準，涵蓋 Metadata Model、Mandatory Fields、Optional Fields、Field Definitions、Validation Rules、Lifecycle 與 Compliance；為所有正式文件之唯一 Metadata 規範（Single Source of Truth）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-002 |
| 文件名稱 | Metadata Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0009、SR-AEOS-0009-R1、AEOS-STD-001（Approved v1.1.0）、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-GOV-001（Approved v1.2.0）、AEOS-ARCH-001、AEOS-ADR-002（WA-001 Fact Authority Transition） |
| 關聯文件 | EWO-AEOS-0009、SR-AEOS-0009-R1、AEOS-STD-001、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件定義 AEOS 正式治理文件之 Metadata 標準，其目的為：

- 作為所有正式文件（Architecture、Constitution、Governance、Standard、Policy、Catalog、ADR 等）之唯一 Metadata 規範。
- 確保 Metadata 完整、一致、可稽核且可追溯。
- 履行 AEOS-GOV-001 §4 Planned Standards 之 Metadata Standard 項目。

本文件不是 Frontmatter Template，也不是 Documentation Format；文件格式以 AEOS-STD-001 為準，文件體系結構以 AEOS-DIA-001 為準。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- 正式文件 frontmatter 之 Metadata 欄位定義與規則。
- Metadata Model、Mandatory Fields、Optional Fields、Field Definitions、Validation Rules、Lifecycle 與 Compliance。

### 2.2 Out of Scope

本標準明確不涵蓋：

- 文件格式（Document Structure、Section Rules、Tables；由 AEOS-STD-001 定義）。
- Documentation Architecture、Taxonomy、命名與生命週期（由 AEOS-DIA-001 定義）。
- Repository 治理原則與變更管理（由 AEOS-CON-001 定義）。
- Governance Roadmap 內容與優先序（由 AEOS-GOV-001 定義）。
- Repository Foundation 文件（README、CHANGELOG、CONTRIBUTING 等）。

## 3. Metadata Model

- Metadata 以 YAML frontmatter 呈現，位於檔案第一行，以 `---` 包覆（依 AEOS-STD-001 §3）。
- Metadata 分為 Mandatory Fields（§4）與 Optional Fields（§5）。
- 每個欄位依 §6 Field Definitions 定義型別、格式、必填、命名規則、唯一性與更新規則。
- 每個欄位以 Field ID（§6，MF／OF）識別，供 Review、Compliance 與 Validator 引用。
- Metadata 為文件身分之正式宣告；doc-id 一經發布即穩定（依 AEOS-DIA-001 EP-002）。

## 4. Mandatory Fields

所有正式文件 MUST 包含下列 10 個 Mandatory Fields：

| # | 欄位 | 用途 |
|---|------|------|
| MF-01 | doc-id | 文件唯一身分。 |
| MF-02 | doc-name | 文件名稱。 |
| MF-03 | doc-type | 文件型別（依 AEOS-DIA-001 Taxonomy）。 |
| MF-04 | repository | 所屬 Repository。 |
| MF-05 | version | 文件版本（SemVer）。 |
| MF-06 | status | 文件狀態（依 AEOS-DIA-001 §8）。 |
| MF-07 | owner | 文件擁有者。 |
| MF-08 | created | 建立日期。 |
| MF-09 | updated | 最後更新日期。 |
| MF-10 | related | 關聯文件（來源 EWO、Review、上位文件等）。 |

## 5. Optional Fields

下列欄位為 Optional；適用時 MUST 依 §6 定義填寫：

| # | 欄位 | 用途 |
|---|------|------|
| OF-01 | supersedes | 宣告被取代之 doc-id（依 AEOS-ARCH-003 §6.1）。 |
| OF-02 | superseded-by | 宣告取代本文件之 doc-id（依 AEOS-ARCH-003 §6.1）。 |
| OF-03 | decision-status | ADR 決策狀態（依 AEOS-ARCH-003 §8）。 |
| OF-04 | decision-owner | ADR 決策擁有者（依 AEOS-ARCH-003 §5）。 |
| OF-05 | decision-date | ADR 決策日期（依 AEOS-ARCH-003 §5）。 |

## 6. Field Definitions

| Field ID | 欄位 | 型別 | 格式 | 必填 | 命名規則 | 唯一性 | 更新規則 |
|----------|------|------|------|------|----------|--------|----------|
| MF-01 | doc-id | string | `AEOS-<TYPE>-<###>`（例如 `AEOS-STD-002`） | 必填 | 依 AEOS-DIA-001 Taxonomy 分類碼與三位流水號 | 全 Repository 唯一 | MUST NOT 變更 |
| MF-02 | doc-name | string | 正式文件名稱（Kebab-Case 或既有慣例） | 必填 | 與 doc-id 對應 | — | 變更視為重大變更，需 EWO |
| MF-03 | doc-type | string | Taxonomy 型別名稱（Architecture、Constitution、Standard、Governance、Policy、Specification、Capability、ADR、Reference、Index、Template、Information Architecture 等） | 必填 | 依 AEOS-DIA-001 Taxonomy | — | 型別變更需 EWO 與 Review |
| MF-04 | repository | string | `AEOS` | 必填 | 固定值 | — | MUST NOT 變更 |
| MF-05 | version | string | SemVer `MAJOR.MINOR.PATCH` | 必填 | 依 SemVer | — | 依 §8 Metadata Lifecycle 更新 |
| MF-06 | status | string | `Draft`／`Review`／`Approved`／`Released`／`Deprecated`／`Archived` | 必填 | 依 AEOS-DIA-001 §8 | — | 依生命週期轉換 |
| MF-07 | owner | string | 角色名稱 | 必填 | 依 AEOS-DIA-001 §6 角色 | — | 變更需更新 Revision History |
| MF-08 | created | date | `YYYY-MM-DD` | 必填 | ISO 8601 日期 | — | MUST NOT 變更 |
| MF-09 | updated | date | `YYYY-MM-DD` | 必填 | ISO 8601 日期 | — | 每次內容變更 MUST 更新 |
| MF-10 | related | list | doc-id 清單 | 必填 | 至少包含來源 EWO；Review（如適用） | — | 隨內容變更更新，不得保留失效引用 |
| OF-01 | supersedes | list | doc-id 清單 | 選用 | 依 §5 | — | 依 AEOS-ARCH-003 §6.1 更新 |
| OF-02 | superseded-by | list | doc-id 清單 | 選用 | 依 §5 | — | 依 AEOS-ARCH-003 §6.1 更新 |
| OF-03 | decision-status | string | `Proposed`／`Approved`／`Rejected`／`Superseded`／`Deprecated`／`Archived` | 選用（ADR） | 依 AEOS-ARCH-003 §8 | — | 依 AEOS-ARCH-003 §3 更新 |
| OF-04 | decision-owner | string | 角色名稱 | 選用（ADR） | 依 AEOS-DIA-001 §6 角色 | — | 變更需更新 Revision History |
| OF-05 | decision-date | date | `YYYY-MM-DD` | 選用（ADR） | ISO 8601 日期 | — | 決策變更時更新 |

## 7. Metadata Validation Rules

| # | 規則 |
|---|------|
| V-001 | 必填欄位：§4 之 10 個 Mandatory Fields MUST 全部存在且非空。 |
| V-002 | 日期格式：`created`／`updated` MUST 為 `YYYY-MM-DD`，且 `updated` MUST NOT 早於 `created`。 |
| V-003 | Version：MUST 為 SemVer（`MAJOR.MINOR.PATCH`）；不得為 `0.0.0` 或非數值。 |
| V-004 | Status：MUST 為 AEOS-DIA-001 §8 定義之狀態之一。 |
| V-005 | Repository：MUST 為 `AEOS`。 |
| V-006 | doc-id 唯一性：doc-id MUST 於全 Repository 唯一；不得與已發布文件重複。 |
| V-007 | doc-id 格式：MUST 符合 `AEOS-<TYPE>-<###>`。 |
| V-008 | owner：MUST 為 AEOS-DIA-001 §6 定義之角色。 |
| V-009 | related：MUST 使用 doc-id 引用；不得包含失效引用。 |
| V-010 | 一致性：frontmatter 之 `version`／`status` MUST 與文件資訊表格及 Revision History 最新列一致。 |

### 7.1 Cross-field Validation

| # | 規則 |
|---|------|
| CF-001 | `updated` MUST NOT 早於 `created`（`updated` ≥ `created`）。 |
| CF-002 | `status=Approved` 時，`version` MUST 大於或等於 `1.0.0`。 |
| CF-003 | `status=Archived` 時，MUST NOT 更新 `updated`。 |
| CF-004 | `repository` 之值 MUST 與 `doc-id` 前綴一致（`repository=AEOS` ↔ `doc-id` 以 `AEOS-` 開頭）。 |

## 8. Metadata Lifecycle

| 階段 | Metadata 更新規則 |
|------|--------------------|
| Draft | `status=Draft`；`version=0.x.0`；`created` 於建立時設定且 MUST NOT 變更；`updated` 隨每次修改更新。 |
| Review | `status=Review`；RC 修正更新 minor 版本（`0.x.y` → `0.x.y+1`）；`updated` 更新。 |
| Approved | `status=Approved`；首次核准升版至 `1.0.0`；Review 修正依變更大小更新 minor；`updated` 更新。 |
| Released | 合併至 main 後進入 Released 階段；本 Repository 以 `status=Approved` 標示已發布文件；`version` 不因合併變更；`updated` 記錄合併日期（如適用）。 |
| Deprecated | `status=Deprecated`；文件已由新文件取代；`doc-id`、`version` 與歷史保留；不得新增內容；`updated` 記錄 Deprecation 日期。 |
| Archived | `status=Archived`；`doc-id` 與 `version` 保留；不得再更新內容或 `updated`。 |

規則：

- 狀態轉換順序 MUST 為 Draft → Review → Approved → Released →（Deprecated）→ Archived；Deprecated 為選擇性階段。
- 狀態轉換 MUST 依 AEOS-DIA-001 §8 進行；不得跳過未核准之轉換。
- 每次 Metadata 變更 MUST 同步更新文件資訊表格與 Revision History。

## 9. Compliance

### 9.1 Metadata Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| 必填欄位 | 10 個 Mandatory Fields 全部存在且非空。 |
| 日期格式 | `created`／`updated` 為 `YYYY-MM-DD`；`updated` 不早於 `created`。 |
| Version | SemVer 格式；與文件資訊表格一致。 |
| Status | 有效狀態；與文件資訊表格及 Revision History 最新列一致。 |
| Repository | 值為 `AEOS`。 |
| doc-id 唯一性 | 全 Repository 無重複；格式符合 `AEOS-<TYPE>-<###>`。 |
| related | 引用有效，無失效 doc-id。 |
| Metadata Consistency | Frontmatter、文件資訊表格與 Revision History 之 doc-id、version、status 等欄位一致。 |
| No Placeholder | Metadata 無 TBD、TODO、XXX、待補 等未完成值。 |

規則：

- 適用文件之 Metadata MUST 符合 §7 Validation Rules。
- 不合規之 Metadata MUST NOT 合併至 main（依 AEOS-CON-001 GP-009）。
- 本標準之變更 MUST 經 EWO 與 Review 後合併。
- 與 AEOS-STD-001／AEOS-DIA-001／AEOS-CON-001 衝突時，以上位文件為準（依 Governance Hierarchy）。

## 10. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | Taxonomy、狀態與生命週期 |
| REF-004 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 治理基線 |
| REF-005 | AEOS-GOV-001 — Enterprise Governance Roadmap（Approved 1.2.0） | Governance | Planned Standards 與優先序 |
| REF-006 | AEOS-STD-001 — Documentation Format Standard（Approved 1.1.0） | Standard | 文件格式與 Mandatory Sections |
| REF-007 | AEOS-ARCH-003 — Architecture Decision Record System | Architecture | ADR Optional Fields |
| REF-008 | EWO-AEOS-0009 — Metadata Standard | EWO | 本文件之工作來源 |
| REF-009 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

本標準（AEOS-STD-002）為 AEOS 唯一 Metadata 標準來源（Single Source of Truth）；其他文件 MUST NOT 定義相異之 Metadata 欄位規則。

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 3（AEOS-ADR-002 已核准）：執行 Governance Authority Transition——WA-001 分類為歷史來源（Historical Reference）；References 重錨至 AEOS-ARCH-001／Approved 架構載體（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | 依 Standard Review（SR-AEOS-0009-R1）修正：狀態升版至 Approved 1.0.0；Metadata Lifecycle 新增 Deprecated（Draft → Review → Approved → Released → Deprecated → Archived）；Field Definitions 新增 Field ID（MF／OF）；Validation Rules 新增 Cross-field Validation；Compliance Checklist 新增 Metadata Consistency；References 宣告本標準為唯一 Metadata 標準來源 | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Metadata Model、Mandatory Fields、Optional Fields、Field Definitions、Validation Rules、Lifecycle 與 Compliance（EWO-AEOS-0009） | Codex |
