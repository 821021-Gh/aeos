---
doc-id: AEOS-STD-001
doc-name: Documentation Format Standard
doc-type: Standard
repository: AEOS
version: 1.1.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0008
  - SR-AEOS-0008-R1
  - AEOS-DIA-001
  - AEOS-CON-001
  - AEOS-GOV-001
  - AEOS-ADR-002
  - WA-001
---

# AEOS-STD-001 — Documentation Format Standard

> EWO-AEOS-0008：依 AEOS-ADR-002、AEOS-DIA-001、AEOS-CON-001 與 AEOS-GOV-001 建立 AEOS 之 Documentation Format Standard。本文件定義 AEOS 正式治理文件之格式標準；不是 Markdown 教學，不是 Documentation Architecture。

## Executive Summary

本文件定義 AEOS 正式治理文件之格式標準，涵蓋文件結構、章節規則、必要章節、撰寫規則、表格與合規要求；適用於 `docs/` 下之正式文件。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-001 |
| 文件名稱 | Documentation Format Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0008、SR-AEOS-0008-R1、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-GOV-001（Approved v1.0.0）、AEOS-ARCH-001、AEOS-ADR-002（WA-001 Fact Authority Transition） |
| 關聯文件 | EWO-AEOS-0008、SR-AEOS-0008-R1、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件定義 AEOS 正式治理文件之格式標準，其目的為：

- 提供文件撰寫與 Review 之共同格式依據，使所有正式文件結構一致、可讀且可稽核。
- 將 AEOS-DIA-001 之文件規則操作化為具體格式要求。
- 履行 AEOS-GOV-001 §4 Planned Standards 之 Documentation Format Standard 項目。

本文件不是 Markdown 教學，也不是 Documentation Architecture；Markdown 語法以 CommonMark 與 GitHub Flavored Markdown 為準，文件體系之結構以 AEOS-DIA-001 為準。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- `docs/` 下之正式文件（ARCH、DIA、CON、GOV、STD、POL、SPEC、CAP、ADR、REF、IDX、TPL）。
- 文件格式要求：Document Structure、Section Rules、Mandatory Sections、Writing Rules、Tables、Examples 與 Compliance。

### 2.2 Out of Scope

本標準明確不涵蓋：

- Markdown 語法教學（語法以 CommonMark 與 GitHub Flavored Markdown 為準）。
- Documentation Architecture、Taxonomy 與生命週期（由 AEOS-DIA-001 定義）。
- Repository 治理原則與變更管理（由 AEOS-CON-001 定義）。
- Governance Roadmap 內容與優先序（由 AEOS-GOV-001 定義）。
- Repository Foundation 文件（README、CHANGELOG、CONTRIBUTING 等）。

## 3. Document Structure

每份正式文件 MUST 依下列順序組成：

| 順序 | 組成 | 說明 |
|------|------|------|
| 1 | Frontmatter | YAML metadata，位於檔案開頭。 |
| 2 | Title | H1 標題：`# AEOS-<CODE> — <Document Name>`。 |
| 3 | Executive Summary | 文件摘要，位於 Title 之後、文件資訊之前。 |
| 4 | File Info Table | 文件資訊表格，位於 Executive Summary 之後。 |
| 5 | Body Sections | 依文件 Scope 之編號章節。 |
| 6 | References | 來源與關聯文件清單。 |
| 7 | Revision History | 版本變更紀錄。 |

規則：

- Frontmatter 位於檔案第一行，以 `---` 包覆。
- Title 為唯一 H1，MUST 出現且僅出現一次。
- Body Sections、References 與 Revision History 以 H2 章節呈現。

## 4. Section Rules

- Body Sections 以 H2 呈現，編號格式為 `<number>. <Section Name>`（例如 `## 1. Purpose`）。
- 子章節以 H3 呈現，編號格式為 `<number>.<subnumber> <Section Name>`（例如 `### 4.1 In Scope`）。
- 章節編號依序遞增，MUST NOT 跳號；文件 MUST NOT 使用 H4 以上標題。
- 章節名稱使用正式名稱（Purpose、Scope、References、Revision History 等），跨文件保持一致。
- 每個章節 MUST 有實際內容；不得存在空章節。
- 章節內容與章節名稱 MUST 相符；內容不得超出章節宣告之範圍（依 AEOS-CON-001 GP-005）。

## 5. Mandatory Sections

每份正式文件 MUST 包含下列章節：

| 章節 | 內容要求 |
|------|----------|
| Frontmatter | MUST 包含 doc-id、doc-name、doc-type、repository、version、status、owner、created、updated、related。 |
| Executive Summary | 位於 Title 與文件資訊之間；以簡短段落摘要文件之目的與範圍。 |
| 文件資訊 | 以表格列出文件代號、名稱、型別、狀態、版本、Repository、擁有者、建立日期、最後更新、依據文件、關聯文件。 |
| References | 列出全部來源與關聯文件（doc-id、型別、用途）；MUST 宣告架構來源（如適用）。 |
| Revision History | 記錄每次變更：版本、日期、變更摘要、作者。 |

規則：

- 文件依其型別與 EWO Scope 增加之內容章節，不受本節限制；但 MUST 依 §4 Section Rules 編號。
- Revision History 之最新版本 MUST 與 frontmatter 之 version／status 一致。

## 6. Writing Rules

- 正式文件以 Traditional Chinese 撰寫；doc-id、EWO、Review、Scope 等標準語保留英文。
- 規範強度使用 MUST、MUST NOT、SHOULD、SHOULD NOT 表達；不得混用模糊用語（例如「盡量」、「最好」）。
- 句子 MUST 完整、明確、無歧義；避免行銷用語與冗詞。
- 使用條列式呈現規則與項目；同一文件內條列格式 MUST 一致。
- 跨文件引用 MUST 使用 doc-id 與相對路徑，MUST NOT 複製被引用內容（依 AEOS-DIA-001 §7）。
- 文件 MUST NOT 包含 Placeholder（TBD、TODO、XXX、待補 等）。
- 既有知識以引用取代重述（Single Source of Truth，依 AEOS-CON-001 GP-002）。
- 術語跨文件 MUST 一致；新術語首次使用時 MUST 定義或引用其來源。

### 6.1 Normative Language

| 用語 | 定義 |
|------|------|
| MUST | 絕對要求；規範強制，無例外。 |
| MUST NOT | 絕對禁止；規範禁止，無例外。 |
| SHOULD | 建議要求；有正當理由可偏離，但 MUST 說明理由。 |
| SHOULD NOT | 建議禁止；有正當理由可執行，但 MUST 說明理由。 |
| MAY | 允許；選擇性，不具強制力。 |

規則：

- 上述用語 MUST 以英文大寫呈現，MUST NOT 改寫為其他用語。
- SHALL 不建議使用；一律以 MUST 取代。
- 模糊用語（例如「盡量」、「最好」）MUST NOT 用於規範性要求。

## 7. Tables

- 表格 MUST 使用 Markdown 表格語法，且 MUST 包含表頭列。
- 表頭欄位 MUST 簡潔且與內容一致；同類表格跨文件使用相同欄位名稱。
- 表格 MUST 用於結構化資訊（對照、清單、狀態、關係）；不得將長段落文字塞入儲存格。
- 儲存格不得留空；不適用之欄位以 `—` 標示。
- 表格前 SHOULD 以引導句說明表格用途。
- 表格寬度以可讀為限；欄位過多時應拆分為多個表格。

## 8. Examples

下列範例展示合規格式；範例內容為正式文件之節錄，僅供格式示範。

### 8.1 Frontmatter

```yaml
---
doc-id: AEOS-ARCH-001
doc-name: Architecture Baseline
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-05
updated: 2026-08-05
related:
  - EWO-AEOS-0001
  - AEOS-ADR-002
  - WA-001
---
```

### 8.2 Title 與文件資訊

```markdown
# AEOS-ARCH-001 — Architecture Baseline

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-001 |
| 文件名稱 | Architecture Baseline |
| 型別 | Architecture |
| 狀態 | Approved |
| 版本 | 1.0.0 |
```

### 8.3 編號章節與表格

```markdown
## 5. Governance Hierarchy

治理文件與決策依下列階層排序：

| 階層 | 文件類別 | 角色 |
|------|----------|------|
| H0 | AEOS-ARCH-001 | 最高架構權威 |
| H1 | AEOS-ARCH-001 | 架構 Entry Document |
```

### 8.4 完整最小文件範例

下列為完整最小文件範例（Example Standard），僅供示範格式；不表示該文件已建立。

```markdown
---
doc-id: AEOS-STD-002
doc-name: Example Standard
doc-type: Standard
repository: AEOS
version: 0.1.0
status: Draft
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - AEOS-STD-001
---

# AEOS-STD-002 — Example Standard

## Executive Summary

本文件示範最小合規文件之完整結構，依 AEOS-STD-001 Documentation Format Standard 建立。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-002 |
| 文件名稱 | Example Standard |
| 型別 | Standard |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |

## 1. Purpose

本文件僅作為格式範例，不承載其他內容。

## References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | AEOS-STD-001 — Documentation Format Standard | Standard | 格式依據 |

## Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立（格式範例） | Codex |
```

## 9. Compliance

- 本標準適用之正式文件 MUST 符合本標準。
- Review 檢查項目：frontmatter 完整性、Title 唯一性、章節編號、Mandatory Sections、Placeholder 禁止、表格格式、引用形式（依 AEOS-DIA-001 §7）。
- 不合規之文件 MUST NOT 合併至 main（依 AEOS-CON-001 GP-009）。
- 本標準之變更 MUST 經 EWO 與 Review 後合併。
- 與 AEOS-DIA-001／AEOS-CON-001／AEOS-GOV-001 衝突時，以上位文件為準（依 Governance Hierarchy）。

### 9.1 Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| Metadata | Frontmatter 完整（doc-id、doc-name、doc-type、repository、version、status、owner、created、updated、related），且與文件資訊一致。 |
| Mandatory Sections | 包含 Executive Summary、文件資訊、References、Revision History 等必要章節。 |
| Section Numbering | 章節依序編號、無跳號、無 H4 以上標題。 |
| References | 引用使用 doc-id 與相對路徑；來源與關聯文件完整宣告。 |
| Revision History | 最新版本與 frontmatter 之 version／status 一致；變更已記錄。 |
| No Placeholder | 無 TBD、TODO、XXX、待補 等未完成內容。 |

## 10. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | 文件體系、Taxonomy 與引用規則 |
| REF-004 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 治理基線 |
| REF-005 | AEOS-GOV-001 — Enterprise Governance Roadmap（Approved v1.0.0） | Governance | Planned Standards 與優先序 |
| REF-006 | EWO-AEOS-0008 — Documentation Format Standard | EWO | 本文件之工作來源 |
| REF-007 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 3（AEOS-ADR-002 已核准）：執行 Governance Authority Transition——WA-001 分類為歷史來源（Historical Reference）；References 重錨至 AEOS-ARCH-001／Approved 架構載體；格式範例同步（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | 依 Standard Review（SR-AEOS-0008-R1）修正：狀態升版至 Approved 1.0.0；Mandatory Sections 新增 Executive Summary（Title 與文件資訊之間）；Writing Rules 新增 Normative Language（MUST／MUST NOT／SHOULD／SHOULD NOT／MAY；SHALL 不建議使用）；Compliance 新增 Compliance Checklist；References 新增 AEOS-ARCH-001（Architecture Entry Document）；Examples 新增完整最小文件範例 | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Purpose、Scope、Document Structure、Section Rules、Mandatory Sections、Writing Rules、Tables、Examples 與 Compliance（EWO-AEOS-0008） | Codex |
