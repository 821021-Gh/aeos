---
doc-id: AEOS-STD-004
doc-name: Naming Standard
doc-type: Standard
repository: AEOS
version: 1.1.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0011
  - EWO-AEOS-0022
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-CON-001
  - AEOS-DIA-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-STD-002
  - AEOS-STD-003
  - WA-001
---

# AEOS-STD-004 — Naming Standard

> EWO-AEOS-0011：依 WA-001、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001、AEOS-STD-001、AEOS-STD-002 與 AEOS-STD-003 建立 AEOS 之 Naming Standard。本文件為 AEOS Repository 所有正式治理資產之唯一 Naming 規範；不是 Metadata Standard，不是 Cross-reference Standard，不是 Documentation Information Architecture。

## Executive Summary

本文件定義 AEOS 正式治理資產之 Naming 標準，涵蓋 Naming Model、Naming Categories、Naming Rules、Identifier Rules、Validation Rules、Naming Lifecycle 與 Compliance；為所有文件、目錄、EWO、ADR、Standards、Policies、Catalog、Specifications、Capabilities、Review、Branch、Commit、Pull Request 等命名之唯一標準來源（Single Source of Truth）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-004 |
| 文件名稱 | Naming Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0011、AEOS-STD-001（Approved v1.0.0）、AEOS-STD-002（Approved v1.0.0）、AEOS-STD-003（Approved v1.0.0）、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-GOV-001（Approved v1.0.0）、AEOS-ARCH-001、WA-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0011、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001、AEOS-DIA-001、AEOS-GOV-001、AEOS-STD-001、AEOS-STD-002、AEOS-STD-003、WA-001 |

## 1. Purpose

本文件定義 AEOS 正式治理資產之 Naming 標準，其目的為：

- 作為所有文件、目錄、EWO、ADR、Standards、Policies、Catalog、Specifications、Capabilities、Review、Branch、Commit、Pull Request 等命名之唯一標準來源。
- 確保命名一致、可預測、可解析且可稽核，使命名不因資產演進而失效。
- 履行 AEOS-GOV-001 §4 Planned Standards 之 Naming Standard 項目（P1）。

本文件不是 Metadata Standard（Metadata 由 AEOS-STD-002 定義），不是 Cross-reference Standard（引用形式由 AEOS-STD-003 定義），不是 Documentation Information Architecture（Taxonomy、目錄與生命週期由 AEOS-DIA-001 定義）；本文件僅定義命名之形式、規則與維護方式。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- AEOS Repository 之正式治理資產命名：Repository、文件、doc-id、目錄、檔案、Branch、Commit、Pull Request、EWO、ADR、Review、Catalog。
- Naming Model、Naming Categories、Naming Rules、Identifier Rules、Validation Rules、Naming Lifecycle 與 Compliance。
- 正式識別子（doc-id、EWO ID、ADR 編號、Review ID）之格式與唯一性。

### 2.2 Out of Scope

本標準明確不涵蓋：

- Metadata 欄位、值域與填寫規則（由 AEOS-STD-002 定義）。
- 跨文件引用形式與維護規則（由 AEOS-STD-003 定義）。
- 文件格式、章節規則與撰寫規則（由 AEOS-STD-001 定義）。
- Taxonomy、目錄組織與文件生命週期（由 AEOS-DIA-001 定義）。
- Repository 治理原則與變更管理（由 AEOS-CON-001 定義）。
- Workspace 其他 Repository 之命名細則（由其自身治理文件定義）。

## 3. Naming Model

- Naming 為正式治理資產之身分宣告；每個資產依其類別（§4）由「前綴 + 識別子 + 序列」組成名稱。
- 名稱由五個組成要素定義：類別、前綴、識別子、序列、格式（Case Style 與 Separator）。

| 組成 | 定義 | 規則 |
|------|------|------|
| 類別 | 資產之分類（§4 Naming Categories） | 每個資產 MUST 歸屬且僅歸屬一個類別 |
| 前綴 | 類別之固定字首（如 `AEOS-`、`EWO-`） | MUST 依 §5 R-001 使用固定前綴 |
| 識別子 | 資產之唯一身分主體（如 TYPE、流水號） | MUST 依 §6 Identifier Rules |
| 序列 | 流水號或序號 | MUST 依 §5 R-003 依序且唯一 |
| 格式 | Case Style 與 Separator | MUST 依 §5 R-005、R-006 |

- 名稱一經發布即穩定；識別子 MUST NOT 變更（依 AEOS-DIA-001 EP-002）。
- 全部名稱與識別子 MUST 全 Repository 唯一（依 §5 R-008、§7 V-002）。

### 3.1 Naming Hierarchy

命名依治理階層自上而下分層（依 AEOS-ARCH-002 §5 Governance Hierarchy）：

| 層級 | 命名類別 | 說明 |
|------|----------|------|
| Enterprise Naming | Workspace 層級命名（如 WA-001、外部架構來源） | 最高層級命名權威；AEOS MUST 遵循 |
| Repository Naming | Repository 身分命名（`AEOS`） | 依 AEOS-CON-001 §2 固定 |
| Document Naming | 文件、目錄、檔案命名 | 依 §4 Naming Categories |
| Identifier Naming | doc-id、EWO、ADR、Review 等識別子 | 依 §6 Identifier Rules |

規則：

- Naming MUST 自上而下保持一致；下層命名 MUST 依循上層命名之格式與前綴，MUST NOT 與上層衝突。
- Enterprise Naming 之來源（WA-001）為最高命名權威（依 AEOS-ARCH-002 §5 H0）；AEOS 命名 MUST NOT 與其衝突。
- 上層命名變更時，下層命名 MUST 於同一或後續 EWO 對應更新。

## 4. Naming Categories

Naming Categories 為命名之正式分類；資產 MUST 依其類別套用對應命名規則。

| # | 類別 | 適用資產 | 名稱形式 |
|---|------|----------|----------|
| NC-01 | Repository Naming | Repository 身分 | `AEOS` |
| NC-02 | Document Naming | 正式文件之 doc-name | `<Kebab-Case-Name>` |
| NC-03 | doc-id Naming | 正式文件之 doc-id | `AEOS-<TYPE>-<###>` |
| NC-04 | Directory Naming | `docs/` 下目錄 | `<分類目錄>`（依 AEOS-DIA-001 §5） |
| NC-05 | File Naming | 正式文件檔案 | `<doc-id>-<Kebab-Case-Name>.md` |
| NC-06 | Branch Naming | Git Branch | `agent/ewo-aeos-<####>-<kebab-slug>` |
| NC-07 | Commit Naming | Git Commit Message | `<type>(<scope>): <subject> (<EWO-AEOS-####>)` |
| NC-08 | Pull Request Naming | PR Title | `EWO-AEOS-<####> <Document Name>` |
| NC-09 | EWO Naming | Engineering Work Order | `EWO-AEOS-<####>` |
| NC-10 | ADR Naming | ADR 文件 | `AEOS-ADR-<###>`（依 AEOS-ARCH-003 §4） |
| NC-11 | Review Naming | Review ID 與 RC | `<ReviewType>-AEOS-<####>-R<##>`／`RC-<###>` |
| NC-12 | Catalog Naming | Catalog／Index 文件 | `AEOS-IDX-<###>` |

### 4.1 NC-01 — Repository Naming

- Repository 名稱 MUST 為 `AEOS`；正式名稱為 AI Enterprise Operating System（依 AEOS-CON-001 §2）。
- Repository 名稱 MUST NOT 變更；變更視為 Repository 身分變更，需依 AEOS-CON-001 §10 程序。

### 4.2 NC-02 — Document Naming

- 正式文件之 doc-name MUST 使用 Kebab-Case 英文名稱（例如 `Architecture Baseline`、`Cross-reference Standard`）。
- doc-name MUST 與 doc-id 對應（依 AEOS-STD-002 MF-02）；變更視為重大變更，需 EWO。

### 4.3 NC-03 — doc-id Naming

- doc-id MUST 符合 `AEOS-<TYPE>-<###>` 格式（依 AEOS-STD-002 MF-01）；TYPE 依 AEOS-DIA-001 §3 Taxonomy。
- doc-id MUST 全 Repository 唯一且穩定；一經發布 MUST NOT 變更。

### 4.4 NC-04 — Directory Naming

- `docs/` 下目錄 MUST 依 AEOS-DIA-001 §5 Directory Organization 命名：`architecture`、`adr`、`documentation`、`specifications`、`constitution`、`policies`、`standards`、`governance`、`capability`、`references`、`indexes`、`templates`。
- 目錄名稱 MUST 為小寫；MUST NOT 建立空目錄或 Placeholder 目錄。
- 新增目錄 MUST 先擴充 AEOS-DIA-001 Taxonomy 與 Directory Organization（經 EWO 與 Review）。

### 4.5 NC-05 — File Naming

- 正式文件檔案 MUST 命名為 `<doc-id>-<Kebab-Case-Name>.md`（例如 `AEOS-STD-004-Naming-Standard.md`）。
- 檔案 MUST 置於其 doc-id 對應之目錄（依 AEOS-DIA-001 §5）。
- 檔案移動或更名 MUST 依 AEOS-STD-003 §7.4 Redirect Rule 同步更新全部引用。

### 4.6 NC-06 — Branch Naming

- EWO 實作 Branch MUST 命名為 `agent/ewo-aeos-<####>-<kebab-slug>`（例如 `agent/ewo-aeos-0011-naming-standard`）。
- Branch 名稱 MUST 僅使用小寫字母、數字、連字號與斜線；MUST NOT 包含空格、底線或其他符號。
- 非 EWO 之維護 Branch 由 Repository Owner 核准後建立，並記錄於 PR 描述。

### 4.7 NC-07 — Commit Naming

- Commit Message MUST 使用 Conventional Commits 格式：`<type>(<scope>): <subject> (<EWO-AEOS-<####>>)`。
- type MUST 為 `docs`、`chore`、`feat`、`fix`、`refactor` 等 Conventional Commits 類型；scope 為變更範圍（如 `standards`、`architecture`、`governance`、`constitution`、`documentation`、`adr`）。
- EWO 相關 Commit MUST 於 subject 結尾宣告來源 `EWO-AEOS-<####>`。

### 4.8 NC-08 — Pull Request Naming

- PR Title MUST 命名為 `EWO-AEOS-<####> <Document Name>`（例如 `EWO-AEOS-0011 Naming Standard`）。
- PR Title MUST 對應其 EWO 標題；MUST NOT 使用與 EWO 無關之標題。

### 4.9 NC-09 — EWO Naming

- EWO ID MUST 命名為 `EWO-AEOS-<####>`（四位流水號，例如 `EWO-AEOS-0011`）。
- EWO ID MUST 全 Repository 唯一；一經發布 MUST NOT 變更或重用。

### 4.10 NC-10 — ADR Naming

- ADR doc-id MUST 命名為 `AEOS-ADR-<###>`；檔案命名為 `AEOS-ADR-###-<Kebab-Case-Name>.md`（依 AEOS-ARCH-003 §4）。
- ADR 編號由 ADR Register 管理；編號一經發布即穩定，MUST NOT 重用（依 AEOS-ARCH-003 §4）。

### 4.11 NC-11 — Review Naming

- Review ID MUST 命名為 `<ReviewType>-AEOS-<####>-R<##>`（例如 `SR-AEOS-0011-R1`）；ReviewType 依 §6 Review ID。
- Review 修正項目（Review Correction）MUST 編號為 `RC-<###>`（例如 `RC-001`）。
- Review ID MUST 全 Repository 唯一；一經核發 MUST NOT 變更。

### 4.12 NC-12 — Catalog Naming

- Catalog／Index 文件 doc-id MUST 命名為 `AEOS-IDX-<###>`（依 AEOS-DIA-001 §3 IDX 類別）；檔案命名為 `AEOS-IDX-###-<Kebab-Case-Name>.md`。
- Catalog 適用範圍：Architecture Catalog（依 AEOS-ARCH-001 §8 Register）、Governance Catalog（依 AEOS-GOV-001 §6）、Capability Catalog（依後續 Capability 文件）。
- Catalog 項目 MUST 使用目標資產之正式 doc-id 登錄（依 AEOS-STD-003 §4.9），MUST NOT 另立代號。

## 5. Naming Rules

本節定義命名之正式規則；所有正式資產之命名 MUST 符合下列規則。

| # | 規則 | 核心要求 |
|---|------|----------|
| R-001 | Prefix | 使用類別固定前綴 |
| R-002 | Identifier | 識別子依 §6 Identifier Rules |
| R-003 | Sequence | 流水號依序、唯一、固定位數 |
| R-004 | Version | 版本依 SemVer |
| R-005 | Case Style | 正式名稱使用 Kebab-Case；識別子使用大寫 |
| R-006 | Separator | 連字號為正式分隔符 |
| R-007 | Reserved Words | 保留字不得挪用 |
| R-008 | Uniqueness | 識別子全 Repository 唯一 |

### 5.1 R-001 — Prefix

- 每個類別 MUST 使用其固定前綴：`AEOS-`（文件）、`EWO-`（工作單）、`WA-`（外部架構來源）、ReviewType（Review）。
- 前綴 MUST NOT 混用或替換；前綴一經發布即穩定。
- 外部來源（如 `WA-001`）MUST NOT 被賦予 AEOS 前綴（依 AEOS-STD-003 §4.8）。

### 5.2 R-002 — Identifier

- 識別子為名稱之身分主體，由 TYPE 與流水號組成（如 `ARCH-001`、`EWO-0011`）。
- 識別子 MUST 依 §6 Identifier Rules 定義之格式；MUST NOT 自創格式。
- 識別子 MUST 一經發布即穩定（依 AEOS-DIA-001 EP-002）。

### 5.3 R-003 — Sequence

- 流水號 MUST 依序遞增取得；MUST NOT 重用已發布之號碼。
- 流水號 MUST 零填充至固定位數：文件 `###`（三位）、EWO `####`（四位）、Review `R<##>`（二位）。
- 流水號之分配由對應 Register 或 EWO 管理（如 ADR 依 AEOS-ARCH-003 §4）。

### 5.4 R-004 — Version

- 版本 MUST 使用 SemVer `MAJOR.MINOR.PATCH`（依 AEOS-STD-002 MF-05）。
- 狀態與版本 MUST 對應：Draft 為 `0.x.0`；首次核准升版至 `1.0.0`（依 AEOS-STD-002 §8）。
- 版本命名 MUST NOT 使用 `v` 前綴或其他格式（外部來源版本除外，如 `WA-001 v1.0.0`）。

### 5.5 R-005 — Case Style

- 正式文件名稱與檔案名稱 MUST 使用 Kebab-Case（小寫、單字以連字號分隔）。
- doc-id、EWO ID、Review ID、TYPE 與前綴 MUST 使用大寫（Upper Case）。
- MUST NOT 混用 Snake_Case、camelCase 或空格作為正式識別子格式。

### 5.6 R-006 — Separator

- 正式名稱 MUST 以連字號 `-` 分隔單字與識別子組成。
- `/` 僅用於 Branch 路徑與目錄路徑。
- 正式識別子 MUST NOT 包含空格、底線 `_`、點 `.` 或其他符號。

### 5.7 R-007 — Reserved Words

- 保留字 MUST NOT 用於其定義用途以外之命名：`AEOS`、`EWO`、`WA`、Taxonomy TYPE（`ARCH`、`DIA`、`SPEC`、`CON`、`GOV`、`CAP`、`ADR`、`POL`、`STD`、`REF`、`IDX`、`TPL`、`CAT`、`MAT`）、ReviewType（`AR`、`CR`、`DR`、`GR`、`SR`、`CM`）、`R`、`RC`。
- 保留前綴 MUST NOT 被其他類別佔用或仿冒。
- 既有資產使用保留字作為名稱主體時，MUST 依本標準重新命名（經 EWO 與 Review）。

### 5.8 R-008 — Uniqueness

- doc-id、EWO ID、ADR 編號、Review ID MUST 全 Repository 唯一。
- 重複命名 MUST 拒絕（依 §7 V-002）；不得以相似名稱規避唯一性。
- 被 Deprecated 或 Archived 之識別子 MUST 保留且 MUST NOT 重用。

## 6. Identifier Rules

本節定義正式識別子之格式；識別子 MUST 依下列規則命名。

| # | 識別子 | 格式 | 範例 |
|---|--------|------|------|
| IR-01 | AEOS-ARCH-### | Architecture 文件 doc-id | AEOS-ARCH-001 |
| IR-02 | AEOS-CON-### | Constitution 文件 doc-id | AEOS-CON-001 |
| IR-03 | AEOS-GOV-### | Governance 文件 doc-id | AEOS-GOV-001 |
| IR-04 | AEOS-STD-### | Standard 文件 doc-id | AEOS-STD-004 |
| IR-05 | AEOS-POL-### | Policy 文件 doc-id | AEOS-POL-001 |
| IR-06 | AEOS-SPEC-### | Specification 文件 doc-id | AEOS-SPEC-001 |
| IR-07 | AEOS-CAP-### | Capability 文件 doc-id | AEOS-CAP-001 |
| IR-08 | AEOS-ADR-### | ADR 文件 doc-id（依 AEOS-ARCH-003 §4） | AEOS-ADR-001 |
| IR-09 | EWO-AEOS-#### | Engineering Work Order ID | EWO-AEOS-0011 |
| IR-10 | Review ID | `<ReviewType>-AEOS-<####>-R<##>` | SR-AEOS-0011-R1 |
| IR-11 | PR Title | `EWO-AEOS-<####> <Document Name>` | EWO-AEOS-0011 Naming Standard |
| IR-12 | AEOS-CAT-### | Catalog 文件 doc-id（依 AEOS-STD-006） | AEOS-CAT-001 |
| IR-13 | AEOS-MAT-### | Matrix 文件 doc-id（依 AEOS-STD-006） | AEOS-MAT-001 |

### 6.1 IR-01～IR-08、IR-12～IR-13 — 文件 doc-id

- 全部文件 doc-id MUST 符合 `AEOS-<TYPE>-<###>`；TYPE 依 AEOS-DIA-001 §3 Taxonomy（ARCH、DIA、SPEC、CON、GOV、CAP、ADR、POL、STD、REF、IDX、TPL、CAT、MAT）。
- Catalog（CAT）與 Matrix（MAT）之 doc-id 依 AEOS-STD-006 §4 規範。
- doc-id MUST 與檔案名稱及 frontmatter 一致（依 AEOS-STD-003 §6.1 C-002、C-003）。
- 各 TYPE 之流水號 MUST 獨立編號；不同 TYPE 不共用流水號序列。

### 6.2 IR-09 — EWO-AEOS-####

- EWO ID MUST 使用四位流水號：`EWO-AEOS-<####>`。
- EWO ID 依發行順序遞增；一經發布 MUST NOT 變更或重用。
- 正式文件 frontmatter 之 related MUST 宣告來源 EWO（依 AEOS-STD-002 MF-10）。

### 6.3 IR-10 — Review ID

- Review ID MUST 符合 `<ReviewType>-AEOS-<####>-R<##>`。
- ReviewType MUST 為下列固定值之一：

| ReviewType | 含義 |
|------------|------|
| AR | Architecture Review |
| CR | Constitution Review |
| DR | Documentation Review |
| GR | Governance Review |
| SR | Standard Review |
| CM | Catalog／Matrix Review |

- `R<##>` 為 Review 序號（例如 R1、R2）；同一 EWO 之 Review 序號 MUST 依序遞增。
- Review 修正項目 MUST 編號為 `RC-<###>`，並於 PR 描述與 Revision History 宣告。

### 6.4 IR-11 — PR Title

- PR Title MUST 命名為 `EWO-AEOS-<####> <Document Name>`。
- PR Title MUST 與 EWO 標題對應；Draft PR 與正式 PR 使用相同命名規則。

### 6.5 Identifier Reservation

| 機制 | 定義 | 規則 |
|------|------|------|
| Reserved Prefix | 保留前綴（`AEOS-`、`EWO-`、`WA-`、ReviewType 等） | MUST 依 §5 R-007 使用；MUST NOT 被其他類別佔用 |
| Reserved Identifier | 已保留但尚未發布之識別子（如固定值 `AEOS`、`WA-001`、規劃中流水號） | MUST NOT 分配給其他資產（依 §8 Reserved） |
| Retired Identifier | 已停用並永久保留之識別子（Deprecated／Superseded） | 保留歷史；MUST NOT 重新使用或重新配置 |

規定：

- Identifier 一經 Released，不得重新使用；Released 後之 Deprecated／Superseded 識別子 MUST 永久保留。
- 識別子之保留與釋出 MUST 記錄於對應 Register（如 ADR Register 依 AEOS-ARCH-003 §4）或 EWO。
- MUST NOT 以重用已發布識別子之方式修正命名錯誤（依 §7 V-002）。

## 7. Validation Rules

本節定義命名有效性之驗證規則；驗證 MUST 於 Review 時執行（依 AEOS-CON-001 GP-009）。

| # | 規則 | 驗證內容 | 不合規處理 |
|---|------|----------|------------|
| V-001 | Identifier Format | 識別子 MUST 符合 §6 格式（前綴、TYPE、位數、大小寫） | 記為不合規，依 EWO 修正 |
| V-002 | Duplicate Identifier | 全 Repository MUST 無重複 doc-id、EWO ID、ADR 編號、Review ID | 記為不合規，重新命名後 Review |
| V-003 | Reserved Prefix | 保留前綴 MUST NOT 被錯誤使用或佔用 | 記為不合規，重新命名後 Review |
| V-004 | Case Validation | Case Style MUST 依 §5 R-005 | 記為不合規，依 EWO 修正 |
| V-005 | File Name Validation | 檔案名稱 MUST 依 §4.5（doc-id + Kebab-Case + `.md`，置於正確目錄） | 記為不合規，依 EWO 修正 |
| V-006 | Branch Name Validation | Branch 名稱 MUST 依 §4.6 | 記為不合規，於 PR Review 時修正 |

規則：

- 驗證以 §7.1 Naming Consistency Validation 與 §9.1 Naming Compliance Checklist 為執行依據；Review Owner MUST 於 Review 時執行驗證。
- 自動化驗證工具 MAY 用於輔助；其結果 MUST 以 §9.1 Checklist 人工確認。

### 7.1 Naming Consistency Validation

Naming Consistency Validation 驗證命名與資產身分、Metadata、引用及 EWO 之一致性：

| # | 檢查項目 | 內容 |
|---|----------|------|
| C-001 | Identifier 與 File Name 一致 | 識別子 MUST 與檔案名稱一致（`AEOS-<TYPE>-<###>-<Kebab-Case-Name>.md`） |
| C-002 | Identifier 與 Metadata 一致 | 識別子 MUST 與 frontmatter doc-id、文件資訊表格及 Revision History 一致（依 AEOS-STD-002 V-010） |
| C-003 | Identifier 與 References 一致 | 正文與 References 宣告之識別子 MUST 一致（依 AEOS-STD-003 §6.1 C-001） |
| C-004 | Branch 與 EWO Identifier 一致 | Branch 名稱 MUST 包含並對應其 EWO Identifier（`agent/ewo-aeos-<####>-<kebab-slug>`） |
| C-005 | PR Title 與 EWO Title 一致 | PR Title MUST 對應其 EWO 標題（`EWO-AEOS-<####> <Document Name>`） |

規則：

- Naming Consistency Validation MUST 於 Review 時執行，並併入 §9.1 Checklist。
- 不一致之命名 MUST 記為不合規，依 §7 修正後始可合併。

## 8. Naming Lifecycle

命名狀態隨資產之生命週期（依 AEOS-DIA-001 §8）演進；本節定義命名本身之狀態。

| 狀態 | 定義 | 規則 |
|------|------|------|
| Proposed | 提案中之命名（EWO 範圍內），尚未發布 | MUST 於 EWO 中宣告；可於 Review 前調整 |
| Active | 已發布且使用中之命名（合併至 main 或已核發） | MUST 保持唯一、穩定且符合本標準 |
| Deprecated | 已被取代或停用之命名 | 保留歷史；MUST NOT 重用；不得再建立新引用（依 AEOS-STD-003 §8） |
| Superseded | 已由新識別子正式取代之命名 | 保留歷史；MUST NOT 重新配置；取代關係 MUST 依 AEOS-STD-002 OF-01／OF-02 宣告（supersedes／superseded-by） |
| Reserved | 保留或固定之命名（如 `AEOS`、`WA-001`、保留前綴） | MUST NOT 被分配給其他資產 |

規則：

- 命名狀態轉換順序 MUST 為 Proposed → Active → Deprecated → Superseded → Reserved。
- Superseded Identifier 保留歷史，不得重新配置。
- 命名狀態變更 MUST 記錄於對應文件之 Revision History（如適用）。
- 命名生命週期與文件生命週期（AEOS-DIA-001 §8）一致；本文件不重述文件生命週期。

## 9. Compliance

- 本標準適用之正式資產 MUST 符合本標準（依 AEOS-CON-001 §11）。
- 不合規之命名 MUST NOT 合併至 main（依 AEOS-CON-001 GP-009）。
- 本標準之變更 MUST 經 EWO 與 Review 後合併。
- 與 AEOS-DIA-001／AEOS-CON-001／AEOS-ARCH-002 衝突時，以上位文件為準（依 Governance Hierarchy，AEOS-ARCH-002 §5）。
- 本標準 MUST NOT 重新定義 Metadata（AEOS-STD-002）、Cross-reference（AEOS-STD-003）、Documentation Format（AEOS-STD-001）或 Documentation Information Architecture（AEOS-DIA-001）之規則。

### 9.1 Naming Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| Identifier Format | 全部識別子符合 §6 格式（前綴、TYPE、位數、大小寫） |
| Duplicate Identifier | 全 Repository 無重複 doc-id、EWO ID、ADR 編號、Review ID |
| Reserved Prefix | 保留前綴未被錯誤使用或佔用 |
| Case Validation | 文件名稱與檔案名稱使用 Kebab-Case；識別子使用大寫 |
| File Name Validation | 檔案名稱符合 doc-id + Kebab-Case + `.md`，且置於正確目錄 |
| Branch Name Validation | Branch 名稱符合 `agent/ewo-aeos-<####>-<kebab-slug>` |
| Uniqueness | 識別子全 Repository 唯一；Deprecated／Archived 識別子未重用 |
| Metadata 一致 | doc-id 與 doc-name、檔案名稱、frontmatter、文件資訊一致（依 AEOS-STD-002 V-010） |

### 9.2 Naming Integrity Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| 無 Duplicate Identifier | 全 Repository 無重複識別子（依 §7 V-002） |
| 無 Invalid Prefix | 前綴符合類別固定值（依 §5 R-001、§7 V-003） |
| 無 Invalid Case | Case Style 符合 Kebab-Case／大寫識別子（依 §5 R-005、§7 V-004） |
| 無 Invalid Separator | 分隔符符合連字號規則（依 §5 R-006） |
| 無 Reused Identifier | 已 Released 之識別子未被重新使用（依 §6.5） |
| 無 Broken Naming Chain | 上層命名與下層命名一致，無中斷（依 §3.1） |

## 10. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Governance Hierarchy 與治理結構 |
| REF-004 | AEOS-ARCH-003 — Architecture Decision Record System | Architecture | ADR 編號規則與 ADR Register |
| REF-005 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 身分與治理原則 |
| REF-006 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | Taxonomy、目錄與命名規則 |
| REF-007 | AEOS-GOV-001 — Enterprise Governance Roadmap（Approved v1.0.0） | Governance | Planned Standards 與優先序 |
| REF-008 | AEOS-STD-001 — Documentation Format Standard（Approved v1.0.0） | Standard | 文件格式與撰寫規則 |
| REF-009 | AEOS-STD-002 — Metadata Standard（Approved v1.0.0） | Standard | Metadata 與 frontmatter 規則 |
| REF-010 | AEOS-STD-003 — Cross-reference Standard（Approved v1.0.0） | Standard | 引用形式與一致性驗證 |
| REF-011 | EWO-AEOS-0011 — Naming Standard | EWO | 本文件之工作來源 |

本標準（AEOS-STD-004）為 AEOS 唯一 Naming 標準來源（Single Source of Truth）；其他文件 MUST NOT 定義相異之命名規則。

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | 依 Standard Review（SR-AEOS-0011-R1）修正：狀態升版至 Approved 1.0.0；Naming Model 新增 Naming Hierarchy（Enterprise／Repository／Document／Identifier Naming；Naming MUST 自上而下保持一致）；Identifier Rules 新增 Identifier Reservation（Reserved Prefix／Reserved Identifier／Retired Identifier；Identifier 一經 Released 不得重新使用）；Validation Rules 新增 Naming Consistency Validation（C-001～C-005）；Naming Lifecycle 新增 Superseded（Proposed → Active → Deprecated → Superseded → Reserved；Superseded Identifier 保留歷史，不得重新配置）；Compliance 新增 Naming Integrity Checklist | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Naming Model、Naming Categories、Naming Rules、Identifier Rules、Validation Rules、Naming Lifecycle 與 Compliance（EWO-AEOS-0011） | Codex |
