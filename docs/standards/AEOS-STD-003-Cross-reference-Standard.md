---
doc-id: AEOS-STD-003
doc-name: Cross-reference Standard
doc-type: Standard
repository: AEOS
version: 1.1.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-07
related:
  - EWO-AEOS-0010
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-CON-001
  - AEOS-DIA-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-STD-002
  - AEOS-ADR-002
  - WA-001
---

# AEOS-STD-003 — Cross-reference Standard

> EWO-AEOS-0010：依 AEOS-ADR-002、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001、AEOS-STD-001 與 AEOS-STD-002 建立 AEOS 之 Cross-reference Standard。本文件為 AEOS Repository 所有正式治理文件之唯一 Cross-reference 規範；不是 Documentation Architecture，不是 Metadata Standard，不是 Naming Standard。

## Executive Summary

本文件定義 AEOS 正式治理文件之 Cross-reference 標準，涵蓋 Cross-reference Model、Reference Types、Reference Rules、Reference Validation、Broken Reference Management、Cross-reference Lifecycle 與 Compliance；為所有正式文件之唯一 Cross-reference 規範（Single Source of Truth）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-003 |
| 文件名稱 | Cross-reference Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-07 |
| 依據文件 | EWO-AEOS-0010、AEOS-STD-001（Approved v1.0.0）、AEOS-STD-002（Approved v1.0.0）、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-GOV-001（Approved v1.0.0）、AEOS-ARCH-001、AEOS-ADR-002（WA-001 Fact Authority Transition） |
| 關聯文件 | EWO-AEOS-0010、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001、AEOS-DIA-001、AEOS-GOV-001、AEOS-STD-001、AEOS-STD-002、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件定義 AEOS 正式治理文件之 Cross-reference 標準，其目的為：

- 作為所有 Architecture、Constitution、Governance、Standards、Policies、Catalog、ADR、Specification、Capability 等文件之唯一 Cross-reference 規範。
- 確保文件間引用一致、可解析、可追溯且可稽核，使引用不因文件演進而失效。
- 履行 AEOS-GOV-001 §4 Planned Standards 之 Cross-reference Standard 項目（P1）。

本文件不是 Documentation Architecture（文件體系結構由 AEOS-DIA-001 定義），不是 Metadata Standard（Metadata 由 AEOS-STD-002 定義），不是 Naming Standard（命名規則依 AEOS-DIA-001 §3）；本文件僅定義 Cross-reference 之形式、規則與維護方式。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- `docs/` 下之正式文件（ARCH、DIA、CON、GOV、STD、POL、SPEC、CAP、ADR、REF、IDX、TPL）間之 Cross-reference。
- Cross-reference Model、Reference Types、Reference Rules、Reference Validation、Broken Reference Management、Cross-reference Lifecycle 與 Compliance。
- 對外部來源（如 WA-001，歷史參考）之引用規範。

### 2.2 Out of Scope

本標準明確不涵蓋：

- Documentation Architecture、Taxonomy 與文件生命週期（由 AEOS-DIA-001 定義）。
- Metadata 欄位、值域與填寫規則（由 AEOS-STD-002 定義）。
- 文件格式、章節規則與撰寫規則（由 AEOS-STD-001 定義）。
- Repository 治理原則與變更管理（由 AEOS-CON-001 定義）。
- Naming Standard 之內容（由後續 EWO 建立；目前命名規則依 AEOS-DIA-001 §3）。

## 3. Cross-reference Model

- Cross-reference 為「來源文件 → 目標文件」之正式關聯；每份正式文件以 References 章節宣告其直接引用。
- Reference 由五個組成要素定義：來源文件、目標 doc-id、目標相對路徑、Reference Type、用途。

| 組成 | 定義 | 規則 |
|------|------|------|
| 來源文件 | 建立引用之正式文件 | 每份正式文件 MUST 於 References 章節宣告其全部直接引用 |
| 目標 doc-id | 被引用文件之唯一識別 | MUST 依 §5 R-001 使用 doc-id |
| 目標相對路徑 | 目標文件依命名規則對應之檔案路徑 | MUST 依 §5 R-002 可解析至實際檔案 |
| Reference Type | 引用之型別 | MUST 依 §4 宣告型別 |
| 用途 | 引用之原因與角色 | References 章節 MUST 以簡短文字說明用途 |

- 全部文件與其引用形成引用圖（Reference Graph）；引用圖 MUST 為有向無環圖（依 §5 R-006）。
- 正文引用（inline reference）與 References 章節宣告 MUST 一致（依 §6 V-003）。

### 3.1 Reference Direction Model

引用方向依來源文件與目標文件於 Governance Hierarchy（依 AEOS-ARCH-002 §5）之相對位置定義：

| 方向 | 定義 | 範例 |
|------|------|------|
| Upward Reference | 下位文件指向較高階層權威文件之引用，代表權威依賴 | AEOS-STD-003 → AEOS-CON-001（H2）、AEOS-ARCH-001（H1） |
| Lateral Reference | 同一階層或同類文件間之引用，代表平行關聯 | AEOS-STD-003 → AEOS-STD-001、AEOS-STD-002（H4） |
| Downward Reference | 上位文件指向下位文件之引用，代表宣告、登錄或監督 | AEOS-ARCH-001 → AEOS-ARCH-002、AEOS-ARCH-003（Register） |

規則：

- 權威依賴 MUST 以 Upward Reference 表達；下位文件 MUST NOT 以 Downward Reference 規避權威依賴。
- Downward Reference（如 Register 登錄）MUST NOT 被解釋為權威依賴。
- Lateral Reference 不建立權威依賴；仍 MUST 遵守 §5 R-006。
- Cross-reference MUST 維持單向依賴，不得形成循環依賴；引用圖 MUST 為有向無環圖（依 §5 R-006）。

## 4. Reference Types

Reference Type 為引用之正式分類。下列九種型別為 AEOS 引用之基礎分類；引用 MUST 依目標文件之型別宣告對應 Reference Type。

| Reference Type | 目標文件 | 用途 |
|----------------|----------|------|
| Architecture Reference | ARCH 文件 | 架構內容與來源追溯 |
| Governance Reference | GOV 文件、AEOS-ARCH-002 | 治理結構、Roadmap 與優先序 |
| Standard Reference | STD 文件 | 標準遵循宣告 |
| Policy Reference | POL 文件 | 政策遵循宣告 |
| Specification Reference | SPEC 文件 | 規格遵循宣告 |
| ADR Reference | ADR 文件 | 架構決策追溯 |
| EWO Reference | EWO-AEOS-#### | 工作來源與變更依據 |
| External Reference | Repository 外部來源（含歷史來源，如 WA-001） | 外部來源／歷史來源宣告 |
| Catalog Reference | Catalog 文件（Architecture／Governance／Capability Catalog） | 目錄登錄與索引關聯 |

### 4.1 Architecture Reference

- 定義：指向 Architecture 文件（ARCH）或 Approved 架構來源（AEOS-ARCH-001／對應架構載體）之引用。
- 規則：
  - 架構相關文件 MUST 於 References 宣告其架構來源（AEOS-ARCH-001／Approved 架構載體；依 AEOS-DIA-001 DP-003）。
  - 架構內容 MUST 引用權威來源（Approved 架構載體／AEOS-ARCH-001），MUST NOT 重述或重新定義（依 AEOS-ARCH-001 §7）；WA-001 僅可作為歷史來源引用（AEOS-ADR-002 §2.1）。

### 4.2 Governance Reference

- 定義：指向 Governance 文件（GOV）與治理結構文件（AEOS-ARCH-002）之引用。
- 規則：
  - 治理相關內容 MUST 引用治理權威文件（AEOS-CON-001、AEOS-ARCH-002、AEOS-GOV-001），MUST NOT 重述其內容（依 AEOS-ARCH-002 §3）。
  - 優先序或 Roadmap 相關宣告 MUST 引用 AEOS-GOV-001。

### 4.3 Standard Reference

- 定義：指向 Standard 文件（STD）之引用，用於宣告標準遵循。
- 規則：
  - 正式文件 MUST 宣告其遵循之標準（至少 AEOS-STD-001、AEOS-STD-002；本標準適用時含 AEOS-STD-003）。
  - 引用標準之特定章節時，格式為 `doc-id §N`（例如 `AEOS-STD-001 §3`）。

### 4.4 Policy Reference

- 定義：指向 Policy 文件（POL）之引用，用於宣告政策遵循。
- 規則：
  - 引用政策 MUST 指向已存在之 POL 文件；MUST NOT 引用尚未建立之政策（依 §6 V-001）。
  - 政策內容以 POL 文件為準，MUST NOT 於其他文件重述（依 AEOS-CON-001 GP-002）。

### 4.5 Specification Reference

- 定義：指向 Specification 文件（SPEC）之引用，用於宣告規格遵循（Specification Driven，依 AEOS-CON-001 GP-003）。
- 規則：
  - 規格相關內容 MUST 引用 SPEC 權威文件。
  - 規格變更時，引用方 MUST 於同一或後續 EWO 同步更新引用（依 AEOS-ARCH-002 §5）。

### 4.6 ADR Reference

- 定義：指向 Architecture Decision Record（ADR）之引用，用於追溯架構決策之背景、決策與影響（依 AEOS-ARCH-003）。
- 規則：
  - ADR 引用 MUST 使用 ADR 之 doc-id（格式 `AEOS-ADR-###`，依 AEOS-ARCH-003 §4）。
  - 宣告決策狀態時，MUST 依 AEOS-STD-002 OF-03 與 AEOS-ARCH-003 之值域，MUST NOT 自行定義。

### 4.7 EWO Reference

- 定義：指向 Engineering Work Order（EWO-AEOS-####）之引用，用於宣告文件之工作來源與變更依據。
- 規則：
  - 正式文件 frontmatter 之 related MUST 包含來源 EWO（依 AEOS-STD-002 MF-10）。
  - EWO 引用 MUST 使用完整代號 `EWO-AEOS-<####>`（例如 `EWO-AEOS-0010`）。

### 4.8 External Reference

- 定義：指向 Repository 外部來源之引用；外部歷史來源（如 WA-001）僅作為歷史參考，不作為正式 Fact Authority（AEOS-ADR-002 §2.1）。
- 規則：
  - 外部引用 MUST 依 §5 R-007 於 References 完整宣告。
  - 外部來源（如 WA-001）不納入 AEOS Taxonomy 與 doc-id 編號，MUST NOT 被賦予 AEOS 文件之 doc-id。
  - 外部歷史來源（如 WA-001）之引用 MUST 標示為歷史參考，MUST NOT 作為正式架構權威依據（依 AEOS-ADR-002 §2.1）。

### 4.9 Catalog Reference

- 定義：指向 Catalog／Index 文件之引用，用於目錄登錄與索引關聯；適用於 Architecture Catalog、Governance Catalog、Capability Catalog。
- 規則：
  - Architecture Catalog 依 AEOS-ARCH-001 §8 Architecture Register 建立；Governance Catalog 依 AEOS-GOV-001 §6 Planned Catalogs；Capability Catalog 依後續 Capability 文件建立。
  - 文件登錄於 Catalog MUST 使用正式 doc-id，MUST NOT 以內容複製取代登錄。
  - Catalog 項目之 status／version MUST 與目標文件 frontmatter 一致；變更時 MUST 依 §7 同步更新。

## 5. Reference Rules

本節定義 Cross-reference 之正式規則；引用 MUST 符合下列規則。

| # | 規則 | 核心要求 |
|---|------|----------|
| R-001 | doc-id 引用規則 | 引用以 doc-id 為主要識別 |
| R-002 | 相對路徑規則 | 引用可解析至實際檔案路徑 |
| R-003 | Single Source of Truth | 每一主題僅一個權威文件 |
| R-004 | 禁止內容複製 | 以引用取代複製 |
| R-005 | Reference Chain | 引用鏈可追溯且最短 |
| R-006 | Circular Reference 禁止 | 引用圖為有向無環圖（DAG） |
| R-007 | External Reference 規範 | 外部引用完整宣告 |

### 5.1 R-001 — doc-id 引用規則

- 跨文件引用 MUST 以 doc-id 為主要識別；doc-id 格式為 `AEOS-<TYPE>-<###>`，EWO 為 `EWO-AEOS-<####>`，外部來源使用既有代號（如 `WA-001`）。
- MUST NOT 以文件標題或模糊描述取代 doc-id。
- 引用之 doc-id MUST 存在且有效（依 §6 V-001）；MUST NOT 引用尚未建立之文件。
- 正文引用特定章節時，格式為 `doc-id §N`（例如 `AEOS-STD-001 §3`）。

### 5.2 R-002 — 相對路徑規則

- 每份正式文件之相對路徑由 doc-id 依 AEOS-DIA-001 §3 命名規則與 §5 目錄組織唯一決定，格式為 `docs/<分類目錄>/<doc-id>-<Kebab-Case-Name>.md`。
- 引用 MUST 以 doc-id 指向目標文件，且目標文件 MUST 存在於其 doc-id 對應之相對路徑（依 §6 V-002）。
- doc-id 與路徑 MUST 指向同一文件（依 §6 V-003）。
- 目標文件移動或更名時，引用方 MUST 於同一 EWO 或 RC 同步更新引用（依 AEOS-DIA-001 §5、§7）。

### 5.3 R-003 — Single Source of Truth

- 每一主題僅存在一個權威文件（依 AEOS-CON-001 GP-002、AEOS-DIA-001 DP-002、AEOS-ARCH-002 GA-004）。
- 本文件（AEOS-STD-003）為 AEOS 唯一 Cross-reference 標準來源；其他文件 MUST NOT 定義相異之 Cross-reference 形式或規則。
- 需要其他文件內容時，MUST 引用其權威文件，MUST NOT 建立平行定義。

### 5.4 R-004 — 禁止內容複製

- 正式文件 MUST NOT 複製被引用文件之內容章節（依 AEOS-DIA-001 §7、AEOS-CON-001 GP-002）。
- 需要內容時，以引用取代；必要之摘要或對照 MUST 以自身語言撰寫，並於 References 宣告來源。
- Review 發現內容複製時，MUST 依 §7 修正後始可合併。

### 5.5 R-005 — Reference Chain

- 引用鏈為「來源 → 目標 → 目標之引用」之可追溯路徑。
- 引用鏈 MUST 可回溯至權威來源（架構內容至 AEOS-ARCH-001／Approved 架構載體；治理內容至 AEOS-CON-001／AEOS-ARCH-002 等）。
- 引用鏈 SHOULD 保持最短：直接引用權威來源，避免不必要之中間跳轉。
- 鏈上每一 Reference MUST 有效且符合本標準（依 §6）。

### 5.6 R-006 — Circular Reference 禁止

- 文件間 MUST NOT 形成直接或間接循環引用（例如 `A → B → A`、`A → B → C → A`）。
- 引用圖 MUST 為有向無環圖（DAG）。
- 發現循環引用時，MUST 由 Document Owner 重新設計引用關係，經 EWO 修正後依 §7 Review 核准。

### 5.7 R-007 — External Reference 規範

- References 章節 MUST 完整宣告外部來源：來源名稱、文件代號（如有）、型別、狀態、版本、用途與取得日期。
- 外部來源內容以來源文件為準；MUST NOT 以內容複製取代引用。
- 外部來源失效或版本變更時，引用方 MUST 依 §7 Broken Reference Management 處理。
- 外部來源 MUST NOT 被賦予 AEOS 文件之 doc-id（依 §4.8）。

## 6. Reference Validation

本節定義引用有效性之驗證規則；驗證 MUST 於 Review 時執行（依 §7.3、AEOS-CON-001 GP-009）。

| # | 規則 | 驗證內容 | 不合規處理 |
|---|------|----------|------------|
| V-001 | doc-id 存在 | 引用之 doc-id MUST 存在於 Repository（已建立之正式文件或已核准之外部來源） | 記為 Broken Reference，依 §7 修正 |
| V-002 | 路徑有效 | 目標文件 MUST 存在於其 doc-id 對應之相對路徑（`docs/<分類目錄>/<doc-id>-<Kebab-Case-Name>.md`） | 記為 Broken Reference，依 §7 修正 |
| V-003 | 引用一致 | doc-id 與路徑 MUST 指向同一文件；正文引用與 References 宣告 MUST 一致；frontmatter doc-id 與檔案名稱 MUST 一致 | 修正不一致之引用 |
| V-004 | 無 Broken Reference | 全部引用 MUST 通過 V-001～V-003 | 文件不合規，MUST NOT 合併至 main |
| V-005 | 無 Circular Reference | 引用圖 MUST 為有向無環圖（DAG） | 重新設計引用關係後依 §7 Review |

規則：

- 驗證以 §6.1 Reference Consistency Validation 與 §9.1 Compliance Checklist 為執行依據；Review Owner MUST 於 Review 時執行驗證。
- 自動化驗證工具 MAY 用於輔助；其結果 MUST 以 §9.1 Checklist 人工確認。

### 6.1 Reference Consistency Validation

Reference Consistency Validation 驗證引用宣告與文件身分、檔名、Metadata 及型別之一致性：

| # | 檢查項目 | 內容 |
|---|----------|------|
| C-001 | doc-id 與 References 一致 | 正文引用與 References 章節宣告之 doc-id MUST 一致 |
| C-002 | doc-id 與檔名一致 | frontmatter 之 doc-id MUST 與檔案名稱一致（`AEOS-<TYPE>-<###>-<Kebab-Case-Name>.md`） |
| C-003 | doc-id 與 Metadata 一致 | frontmatter 之 doc-id MUST 與文件資訊表格及 Revision History 一致；doc-name、doc-type MUST 一致（依 AEOS-STD-002 V-010） |
| C-004 | Reference Type 與引用文件型別一致 | 引用宣告之 Reference Type MUST 與目標文件之 doc-type 對應（依 §4） |

規則：

- Reference Consistency Validation MUST 於 Review 時執行，並併入 §9.1 Checklist。
- 不一致之引用 MUST 記為不合規，依 §7 修正後始可合併。

## 7. Broken Reference Management

### 7.1 Detection

- 失效引用（Broken Reference）為無法通過 §6 Validation 之引用，包含：doc-id 不存在、路徑無效、引用不一致與循環引用。
- 發現時機：文件建立、內容變更、目標文件移動／更名／刪除、Review 時。
- 發現方式：Review 檢查（§9.1 Checklist）；自動化驗證（如適用）。
- 任何失效引用 MUST 於發現時記錄，並列入同一 EWO 或 RC 之修正範圍。

### 7.2 Resolution

- 失效引用 MUST 於同一 EWO 或 RC 修正（依 AEOS-DIA-001 §7 失效處理）。
- 修正方式：更新相對路徑、更新 doc-id、改引用取代文件、或移除失效引用；目標文件 Deprecated／Archived 時，依 §8 處理。
- MUST NOT 以 Placeholder 或模糊描述暫代（依 AEOS-STD-001 §6）。

### 7.3 Review Requirement

- 引用修正 MUST 經 Review 核准後始可合併至 main（依 AEOS-CON-001 GP-009）。
- PR MUST 於描述中明確宣告其變更之引用；Review Owner MUST 依 §6 驗證。
- 未通過 §6 Validation 之 PR MUST NOT 合併至 main。

### 7.4 Reference Recovery

Reference Recovery 定義引用失效或目標文件演進時之恢復機制，避免 Reference Chain 中斷：

| 機制 | 定義 | 規則 |
|------|------|------|
| Deprecated Replacement | 目標文件 Deprecated 時，以取代文件更新引用 | 引用方 MUST 依取代關係（superseded-by）更新至取代文件；無取代文件時，改引用上位權威文件或移除引用 |
| Redirect Rule | 目標文件移動或更名時之引用重新指向 | MUST 記錄 Redirect（舊 doc-id／舊路徑 → 新 doc-id／新路徑）；引用方 MUST 於同一 EWO 或後續 RC 依 Redirect 更新全部引用 |
| Superseded Document Mapping | 文件被取代時之對照關係 | 取代文件 MUST 宣告 supersedes、被取代文件 MUST 宣告 superseded-by（依 AEOS-STD-002 OF-01／OF-02、AEOS-ARCH-003 §6.1）；引用方 MUST 依 Mapping 更新引用 |

規則：

- 引用鏈上任一環節失效時，MUST 依上表機制恢復，MUST NOT 以 Placeholder 或模糊描述暫代。
- 恢復完成後 MUST 依 §6 重新驗證，並依 §7.3 Review Requirement 核准。
- 未完成恢復之引用 MUST NOT 合併至 main。

## 8. Cross-reference Lifecycle

引用狀態隨目標文件之文件狀態（依 AEOS-DIA-001 §8）演進：

| 狀態 | 定義 | 規則 |
|------|------|------|
| Active Reference | 指向 Draft／Review／Approved／Released 文件之引用 | MUST 保持有效；目標文件變更時 MUST 依 §6 重新驗證 |
| Deprecated Reference | 指向已 Deprecated 文件之引用（目標已由新文件取代） | 既有引用保留歷史；MUST NOT 於新內容建立新引用；引用方 MUST 於下一個相關 EWO 更新至取代文件 |
| Archived Reference | 指向已 Archived 文件之引用 | MUST NOT 於新內容建立新引用；既有引用於文件變更時移除或更新；Archived 文件 MUST NOT 再更新內容（依 AEOS-STD-002 CF-003） |

規則：

- 目標文件狀態變更時，引用方 MUST 檢查其引用狀態，並依上表處理。
- 引用狀態之變更 MUST 記錄於引用方文件之 Revision History（如適用）。

## 9. Compliance

- 本標準適用之正式文件 MUST 符合本標準（依 AEOS-CON-001 §11）。
- 不合規之文件 MUST NOT 合併至 main（依 AEOS-CON-001 GP-009）。
- 本標準之變更 MUST 經 EWO 與 Review 後合併。
- 與 AEOS-DIA-001／AEOS-CON-001／AEOS-ARCH-002 衝突時，以上位文件為準（依 Governance Hierarchy，AEOS-ARCH-002 §5）。
- 本標準 MUST NOT 重新定義 Metadata（AEOS-STD-002）、Documentation Format（AEOS-STD-001）或 Documentation Information Architecture（AEOS-DIA-001）之規則。

### 9.1 Cross-reference Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| doc-id 引用 | 全部跨文件引用使用 doc-id；格式正確；無以標題取代 |
| 相對路徑 | 目標文件存在於其 doc-id 對應之相對路徑，可解析至實際檔案 |
| 引用一致 | doc-id 與路徑指向同一文件；frontmatter doc-id 與檔案名稱一致；正文與 References 一致 |
| 內容複製 | 未複製被引用文件之內容章節 |
| Circular Reference | 引用圖無循環（DAG） |
| Broken Reference | 無失效引用（doc-id／路徑／狀態） |
| 外部引用 | 外部來源完整宣告（名稱、代號、型別、狀態、版本、用途、日期） |
| 標準唯一性 | 未重新定義 Metadata／Documentation Format／Documentation IA 規則 |

### 9.2 Reference Integrity Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| 無 Circular Reference | 引用圖無直接或間接循環（依 §6 V-005） |
| 無 Duplicate Reference | 同一來源文件對同一目標 doc-id 無重複引用 |
| 無 Invalid Reference Type | Reference Type 與目標文件型別一致（依 §6.1 C-004） |
| 無 Missing Target | 全部引用目標存在（依 §6 V-001） |
| 無 Broken Relative Path | 全部相對路徑可解析至實際檔案（依 §6 V-002） |

## 10. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register；Architecture Reference 之引用樞紐 |
| REF-003 | AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Governance Hierarchy 與治理結構 |
| REF-004 | AEOS-ARCH-003 — Architecture Decision Record System | Architecture | ADR Reference 與決策狀態之依據 |
| REF-005 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | 治理原則（GP-002、GP-005、GP-009） |
| REF-006 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | Taxonomy、目錄與 Cross-reference Strategy |
| REF-007 | AEOS-GOV-001 — Enterprise Governance Roadmap（Approved v1.0.0） | Governance | Planned Standards 與優先序 |
| REF-008 | AEOS-STD-001 — Documentation Format Standard（Approved v1.0.0） | Standard | 文件格式與撰寫規則 |
| REF-009 | AEOS-STD-002 — Metadata Standard（Approved v1.0.0） | Standard | Metadata 與 frontmatter 規則 |
| REF-010 | EWO-AEOS-0010 — Cross-reference Standard | EWO | 本文件之工作來源 |
| REF-011 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

本標準（AEOS-STD-003）為 AEOS 唯一 Cross-reference 標準來源（Single Source of Truth）；其他文件 MUST NOT 定義相異之 Cross-reference 規則。

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-07 | 依 EWO-AEOS-0040 Wave 1（AEOS-ADR-002 已核准並合併至 main）：執行 Authority Rule Transition——Architecture Reference 目標限定 ARCH 文件與 Approved 架構來源；WA-001 改列 Historical Reference（External）；External Reference 新增歷史來源規則；Reference Chain 回溯目標重錨至 Approved 架構載體 | Codex |
| 1.0.0 | 2026-08-06 | 依 Standard Review（SR-AEOS-0010-R1）修正：狀態升版至 Approved 1.0.0；Cross-reference Model 新增 Reference Direction Model（Upward／Lateral／Downward Reference；Cross-reference MUST 維持單向依賴，不得形成循環依賴）；Reference Types 新增 Catalog Reference；Reference Validation 新增 Reference Consistency Validation（C-001～C-004）；Broken Reference Management 新增 Reference Recovery（Deprecated Replacement、Redirect Rule、Superseded Document Mapping）；Compliance 新增 Reference Integrity Checklist | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Cross-reference Model、Reference Types、Reference Rules、Reference Validation、Broken Reference Management、Cross-reference Lifecycle 與 Compliance（EWO-AEOS-0010） | Codex |
