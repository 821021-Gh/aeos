---
doc-id: AEOS-STD-005
doc-name: Review Standard
doc-type: Standard
repository: AEOS
version: 1.4.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0012
  - EWO-AEOS-0022
  - EWO-AEOS-0033
  - EWO-AEOS-0037
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-CON-001
  - AEOS-DIA-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-STD-002
  - AEOS-STD-003
  - AEOS-STD-004
  - AEOS-ADR-002
  - WA-001
---

# AEOS-STD-005 — Review Standard

> EWO-AEOS-0012：依 AEOS-ADR-002、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、AEOS-GOV-001、AEOS-STD-001、AEOS-STD-002、AEOS-STD-003 與 AEOS-STD-004 建立 AEOS 之 Review Standard。本文件為 AEOS Repository 所有正式治理資產之唯一 Review 規範；不是 Documentation Format，不是 Metadata，不是 Cross-reference，不是 Naming。

## Executive Summary

本文件定義 AEOS 正式治理資產之 Review 標準，涵蓋 Review Model、Review Types、Review Workflow、Review Rules、Review Decision Model、Review Validation 與 Compliance；為所有 Architecture、Constitution、Governance、Standards、Policies、Catalog、Specifications、Capabilities、ADR、EWO 等文件之唯一 Review 規範（Single Source of Truth）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-005 |
| 文件名稱 | Review Standard |
| 型別 | Standard |
| 狀態 | Approved |
| 版本 | 1.4.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0012、AEOS-STD-001（Approved 1.1.0）、AEOS-STD-002（Approved 1.1.0）、AEOS-STD-003（Approved 1.1.0）、AEOS-STD-004（Approved 1.3.0）、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-GOV-001（Approved 1.2.0）、AEOS-ARCH-001、AEOS-ADR-002（WA-001 Fact Authority Transition） |
| 關聯文件 | EWO-AEOS-0012、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001、AEOS-DIA-001、AEOS-GOV-001、AEOS-STD-001、AEOS-STD-002、AEOS-STD-003、AEOS-STD-004、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件定義 AEOS 正式治理資產之 Review 標準，其目的為：

- 作為所有 Architecture、Constitution、Governance、Standards、Policies、Catalog、Specifications、Capabilities、ADR、EWO 等文件之唯一 Review 規範。
- 使 Review 流程一致、可追溯且可稽核，並於合併至 main 前強制執行（依 AEOS-CON-001 GP-009）。
- 履行 AEOS-GOV-001 §4 Planned Standards 之 Review Standard 項目（P2）。

本文件不是 Documentation Format（文件格式由 AEOS-STD-001 定義），不是 Metadata（Metadata 由 AEOS-STD-002 定義），不是 Cross-reference（引用形式由 AEOS-STD-003 定義），不是 Naming（命名規則由 AEOS-STD-004 定義）；本文件僅定義 Review 之型別、流程、規則與決策。

## 2. Scope

### 2.1 In Scope

本標準涵蓋：

- AEOS 正式治理資產合併至 main 前之 Review：Architecture、Constitution、Governance、Standards、Policies、Catalog、Specifications、Capabilities、ADR、EWO 等。
- Review Model、Review Types、Review Workflow、Review Rules、Review Decision Model、Review Validation 與 Compliance。
- Review ID 與 Review Comment（RC）之格式與追溯。

### 2.2 Out of Scope

本標準明確不涵蓋：

- 文件格式、章節規則與撰寫規則（由 AEOS-STD-001 定義）。
- Metadata 欄位與填寫規則（由 AEOS-STD-002 定義）。
- 跨文件引用形式與維護規則（由 AEOS-STD-003 定義）。
- 命名規則與識別子格式（由 AEOS-STD-004 定義）。
- 文件分類、目錄與生命週期（由 AEOS-DIA-001 定義）。
- Repository 治理原則與變更管理（由 AEOS-CON-001 定義）。
- YEOS Engineering Workflow 之重述或取代（依 AEOS-CON-001 §4.2）。

## 3. Review Model

- Review 為正式治理資產合併至 main 前之強制 Gate（依 AEOS-CON-001 GP-009 Governance by Review）。
- Review 由三個組成要素定義：Review Subject、Review Owner、Review Decision。

| 組成 | 定義 | 規則 |
|------|------|------|
| Review Subject | 被審查之資產（文件、EWO 交付物、ADR 等） | 每份正式文件合併至 main 前 MUST 經 Review |
| Review Owner | 依資產型別指派之審查角色（依 AEOS-DIA-001 §6、AEOS-CON-001 §9） | Review Owner MUST 與作者為不同角色；最終核准由 Repository Owner 執行 |
| Review Decision | 審查結果（APPROVED／REQUEST CHANGES／REJECTED／SUPERSEDED） | MUST 依 §7 宣告並記錄於 PR |

- Review 以 Pull Request 為載體：Draft PR → Ready PR → Review → Decision → Merge／Close。
- Review 鏈 MUST 可追溯：EWO → 文件 → Draft PR → Review ID → RC → Revision（Commit）→ Decision（依 §6 R-003）。
- Review 不改變文件內容之權威來源；審查僅依上位文件與標準驗證合規（依 AEOS-ARCH-002 §5）。

### 3.1 Review Hierarchy

Review 依 Governance Hierarchy（依 AEOS-ARCH-002 §5）自上而下分層：

| 層級 | Review 類別 | 說明 |
|------|-------------|------|
| Enterprise Review | Workspace 層級 Review（外部架構來源之核准，如 WA-001） | 最高層級；AEOS MUST 遵循 |
| Repository Review | Repository 層級 Review（CON、治理文件、標準等） | 依 AEOS-CON-001 §9 由 Repository Owner 最終核准 |
| Document Review | 單份文件層級 Review（ARCH、STD、POL、SPEC、CAP、ADR 等） | 依 §4 Review Types 指派 Review Owner |

規則：

- Review MUST 遵循 Governance Hierarchy，不得由下層 Review 覆蓋上層已核准決策。
- 上層已核准之決策（如 WA-001 架構內容）MUST NOT 由下層 Review 變更或推翻。
- 衝突時以上位文件與上位決策為準（依 AEOS-ARCH-002 §5）。
- 本節層級與 §4 Review Types 對應：Repository Review 層級由 RR 執行；Document Review 層級由 AR、SR、PR、SP、AD、CA、GR 等依文件型別執行。

## 4. Review Types

Review Type 為 Review 之正式分類；Review MUST 依 Review Subject 之型別宣告對應 Review Type。

| Review Type | Review ID 前綴 | 適用資產 | 審查重點 |
|-------------|----------------|----------|----------|
| Architecture Review | AR | ARCH、DIA 文件；Architecture Candidate Assessment RPT（依 AEOS-DIA-001 §3） | 架構一致性與來源追溯（AEOS-ARCH-001／Approved 架構載體）；候選識別與 Review Outcome 之 Fact Authority |
| Repository Review | RR | CON 文件與 Repository 治理文件 | Repository 身分、治理原則與變更管理 |
| Standard Review | SR | STD 文件 | 標準完整性、與上位文件一致、未重新定義既有標準 |
| Policy Review | PR | POL 文件 | 政策內容與上位文件一致 |
| Specification Review | SP | SPEC 文件 | 規格完整性與可驗證性 |
| ADR Review | AD | ADR 文件 | 決策背景、影響與決策狀態（依 AEOS-ARCH-003） |
| Capability Review | CA | CAP 文件 | 能力定義與擁有權 |
| Governance Review | GR | GOV 文件 | 治理 Roadmap、領域與優先序 |
| Catalog／Matrix Review | CM | CAT、MAT 文件 | Catalog／Matrix 一致性、條目追溯與關係事實（依 AEOS-STD-006） |
| Report Review | RT | RPT 文件 | 分析完整性、結論 Fact Authority（可追溯至 Approved Architecture 或正式決策）與建議可執行性 |

規則：

- Review ID MUST 依 §6 R-001 使用 Review Type 前綴。
- 既有型別 Constitution Review（CR）、Documentation Review（DR）沿用既有 Review ID（如 `CR-AEOS-0004-R1`、`DR-AEOS-0003-R1`），保留相容。
- 本表未列之 Review Type MUST NOT 自創（依 AEOS-STD-004 R-007 Reserved Words）。
- ReviewType 之完整清單以本標準為唯一權威來源；AEOS-STD-004 §6.3 定義 Review ID 之格式（IR-10），其型別清單由本標準擴充。
- Architecture Candidate Assessment RPT 使用 AR：AR 同時完成該文件之內容審查與 Lifecycle 核准，不另疊加 RT 作為第二核准路徑。
- PR 仍為正式 Review Record 載體；AR 判定可包含 Approved、Approved with Conditions、Rejected、Deferred。
- Architecture Candidate Assessment RPT 應記錄 Review ID 與結果摘要，但不得聲稱取代 Review Record。
- Catalog 登錄之 Traceability 必須包含 Approved Architecture Candidate Assessment RPT 與對應之 AR Review Record。
- 若候選核准會新增或變更既有 Enterprise Architecture，仍須依 AEOS-ARCH-003 判斷是否另行建立 ADR；RPT 與 AR 不得取代必要之 ADR。

## 5. Review Workflow

Review Workflow 為正式 Review 之執行流程；下列步驟 MUST 依序執行。

| # | 步驟 | 定義 | 規則 |
|---|------|------|------|
| 1 | Submission | 提交 Review（Draft PR → Ready PR） | 提交前 MUST 完成 §8 自檢；PR MUST 宣告 EWO、Files Changed、Scope 與 Traceability |
| 2 | Review | Review Owner 依 §4 Review Type 執行審查 | 審查以 PR 與 Review ID 記錄；審查 MUST 涵蓋 §8 Review Validation |
| 3 | Review Comment | 提出 RC（Review Comment） | RC MUST 依 §6 R-002 編號與記錄 |
| 4 | Revision | 依 RC 修正 | 每個 RC MUST 於同一 Review 週期回應；修正後更新 Commit 與 Revision History |
| 5 | Re-review | 修正後重新審查 | 依 §6 R-006 Re-review Rule |
| 6 | Approval | Review Owner 宣告 APPROVED | 依 §7 Review Decision Model；Approval MUST 記錄於 PR |
| 7 | Merge | 合併至 main | 僅 APPROVED 後可 Merge（依 §6 R-005） |
| 8 | Close | 關閉 PR／EWO | 依 §6 R-007 Close Rule |

規則：

- 步驟 MUST 依序執行；不得跳過 Review（步驟 2）直接 Merge。
- 任一 RC 未解決時，MUST NOT 進入 Approval（步驟 6）。
- 每個步驟 MUST 可追溯至前一階段之輸入（依 §6 R-003）。
- 重大 Review（Architecture、Governance、Standard、Policy）無法達成 APPROVED 時，MUST 依 §5.1 啟動 Review Escalation。

### 5.1 Review Escalation

Review Escalation 適用於 Architecture、Governance、Standard、Policy 等重大 Review。

| 機制 | 定義 | 規則 |
|------|------|------|
| Escalation Trigger | 觸發升級之情況（Review 意見分歧、無法達成 APPROVED、範圍超出 Review Owner 權限、上位文件衝突等） | MUST 於 PR 記錄觸發原因 |
| Escalation Authority | 升級之裁決者（Repository Owner；架構相關為 Architecture Owner／Approved 架構載體來源） | Escalation Authority MUST 為 Review Owner 之上一層級（依 §3.1） |
| Escalation Resolution | 升級後之決策（維持、修改或推翻原決策） | Resolution MUST 記錄於 PR，並以 Review ID 或新 Decision 宣告；不得由下層 Review 覆蓋（依 §3.1） |

規則：

- 重大 Review 無法達成 APPROVED 時，MUST 啟動 Escalation；Escalation MUST NOT 跳過既有 Review 紀錄。
- Escalation 之 Decision MUST 依 §7 記錄，並納入 Review Traceability（依 §6 R-003）。

## 6. Review Rules

本節定義 Review 之正式規則；Review MUST 符合下列規則。

| # | 規則 | 核心要求 |
|---|------|----------|
| R-001 | Review ID | Review ID 依 AEOS-STD-004 IR-10 格式 |
| R-002 | RC（Review Comment）格式 | RC 依序編號並記錄 |
| R-003 | Review Traceability | EWO → Review → RC → Commit → Decision 可追溯 |
| R-004 | Approval Rule | 合併前 MUST 取得 APPROVED |
| R-005 | Merge Rule | 僅 APPROVED 且無未解決 RC 可 Merge |
| R-006 | Re-review Rule | 修正後 MUST 重新審查 |
| R-007 | Close Rule | Merge／Reject／Supersede 後 MUST Close |

### 6.1 R-001 — Review ID

- Review ID MUST 符合 `<ReviewType>-AEOS-<####>-R<##>`（依 AEOS-STD-004 §6.3 IR-10）。
- ReviewType MUST 依 §4 Review Types；`<####>` 為來源 EWO 之四位流水號；`R<##>` 為 Review 序號。
- Review ID 一經核發 MUST NOT 變更；同一 EWO 之 Review 序號 MUST 依序遞增（R1、R2…）。

### 6.2 R-002 — RC（Review Comment）格式

- Review Comment（RC）MUST 編號為 `RC-<###>`（三位流水號，例如 `RC-001`）。
- RC MUST 包含：編號、主題、內容（問題說明）與處理要求（修正或說明理由）。
- RC MUST 對應具體之檔案、章節或變更；MUST 可執行、可驗證。
- RC 狀態 MUST 追蹤至 Resolved（已修正）或 Rejected（說明理由）；未解決之 RC MUST 阻止 Approval（依 §5 規則）。

### 6.3 R-003 — Review Traceability

- Review 鏈 MUST 可追溯：EWO → 文件 → Draft PR → Review ID → RC → Revision（Commit）→ Decision。
- 文件 frontmatter 之 related MUST 包含來源 EWO；Review 通過後 MUST 包含 Review ID（依 AEOS-STD-002 MF-10）。
- Revision History MUST 記錄 Review ID 與變更摘要（依 AEOS-STD-001 §5）。
- PR 描述 MUST 宣告 EWO、Review ID（如適用）與 RC 對照。

### 6.4 R-004 — Approval Rule

- 正式文件合併至 main 前 MUST 取得 Review Owner 之 APPROVED 決策（依 AEOS-CON-001 GP-009）。
- Review Owner MUST 與作者為不同角色；Draft PR MUST NOT 被核准合併。
- Approval MUST 以 PR Review 或 PR Comment 記錄，並宣告 Review ID。

### 6.5 R-005 — Merge Rule

- 僅在 APPROVED 且全部 RC 已 Resolved 時，PR 始可 Merge（依 §7 APPROVED）。
- Merge 方式依 Repository 設定（Squash Merge）；Merge 後文件狀態依 AEOS-DIA-001 §8 進入 Released。
- Merge 後 MUST 更新 EWO／PR 狀態並記錄 Merge Commit。

### 6.6 R-006 — Re-review Rule

- 任一 RC 修正後，MUST 進行 Re-review（依 §5 步驟 5）。
- REQUEST CHANGES 後之修正 MUST 經 Re-review；Re-review 以同一 Review ID 之新 R 序號（`R<##>`）記錄。
- Re-review MUST 驗證全部 RC 之 Resolved 狀態與 §8 Review Validation。

### 6.7 R-007 — Close Rule

- APPROVED 且 Merge 完成後，PR MUST Close（自動或手動）。
- REJECTED／SUPERSEDED 之 PR MUST Close 且 MUST NOT Merge。
- Close 之 PR MUST 保留完整 Review 紀錄（Review ID、RC、Decision）；MUST NOT 刪除。
- EWO 於 PR Close 後視為結束；新需求 MUST 以新 EWO 定義。

## 7. Review Decision Model

Review Decision 為 Review 之正式結果；決策 MUST 由 Review Owner 宣告並記錄。

| 決策 | 定義 | 後續動作 |
|------|------|----------|
| APPROVED | Review 通過，資產可合併 | Merge（依 §6 R-005） |
| REQUEST CHANGES | 需修正後重新審查 | 依 RC 修正 → Re-review（依 §6 R-006） |
| REJECTED | 資產不予接受 | Close PR；MUST NOT Merge；重新提交 MUST 以新 EWO 定義 |
| SUPERSEDED | 資產已被取代 | Close PR；取代關係 MUST 依 AEOS-STD-002 OF-01／OF-02 宣告（supersedes／superseded-by） |

規則：

- 決策 MUST 於 PR 記錄，並宣告 Review ID；決策不得以口頭或 PR 外管道認定。
- 決策變更（如 REQUEST CHANGES → APPROVED）MUST 經 Re-review 並以新 R 序號記錄。
- REJECTED 或 SUPERSEDED 之資產 MUST NOT 以相同 PR 重新提交。

### 7.1 Decision Finality

- APPROVED 為正式核准：代表 Review 通過，資產可合併（依 §6 R-005）；未經新 Review 不得撤回。
- REJECTED 保留歷史：被拒絕之資產與 Review 紀錄 MUST 保留；重新提交 MUST 以新 EWO 定義。
- SUPERSEDED 必須引用新 Decision：宣告 SUPERSEDED 時 MUST 於同一 PR 引用取代之新 Decision（Review ID、ADR 或取代文件），並依 AEOS-STD-002 OF-01／OF-02 宣告取代關係。
- REQUEST CHANGES 必須全部 RC 完成後才能重新 Review：全部 RC 達 Resolved 前，MUST NOT 進入 Re-review／Approval（依 §5 規則、§6 R-006）。

## 8. Review Validation

本節定義 Review 有效性之驗證規則；驗證 MUST 於 Review（§5 步驟 2）與 Merge（§5 步驟 7）前執行。

| # | 規則 | 驗證內容 | 不合規處理 |
|---|------|----------|------------|
| V-001 | Review Completeness | Review MUST 涵蓋：Metadata（AEOS-STD-002）、格式（AEOS-STD-001）、引用（AEOS-STD-003）、命名（AEOS-STD-004）與內容完整性 | 記為 incomplete，退回 Review |
| V-002 | RC Traceability | 每個 RC MUST 有編號、狀態與對應修正（Commit）；RC 與 Revision History 可追溯 | 記為不合規，補齊後再審 |
| V-003 | Approval Validation | 合併前 MUST 有 Review Owner 之 APPROVED 決策（依 §6 R-004） | 無 APPROVED 不得 Merge |
| V-004 | Merge Validation | 僅 APPROVED 且無未解決 RC 之 PR 可 Merge（依 §6 R-005） | 拒絕 Merge |
| V-005 | Review History Validation | Review ID、決策、RC 與版本 MUST 記錄於 PR 與文件 Revision History（依 §6 R-003） | 記為不合規，補記錄後再審 |

規則：

- 驗證以 §8.1 Review Consistency Validation 與 §9.1 Review Compliance Checklist 為執行依據；Review Owner MUST 於 Review 時執行驗證。
- 自動化驗證工具 MAY 用於輔助；其結果 MUST 以 §9.1 Checklist 人工確認。

### 8.1 Review Consistency Validation

Review Consistency Validation 驗證 Review 記錄與資產身分、決策及 PR 狀態之一致性：

| # | 檢查項目 | 內容 |
|---|----------|------|
| C-001 | Review ID 唯一 | Review ID MUST 全 Repository 唯一（依 §6 R-001） |
| C-002 | RC 編號唯一 | 同一 Review 之 RC 編號（RC-###）MUST 唯一且依序（依 §6 R-002） |
| C-003 | Review Decision 與 Revision History 一致 | 決策與文件 Revision History 記錄之 Review ID／版本 MUST 一致 |
| C-004 | Review Decision 與 PR 狀態一致 | APPROVED 對應可 Merge；REQUEST CHANGES／REJECTED／SUPERSEDED 對應未 Merge 且 Close（如適用） |
| C-005 | Merge 僅允許 APPROVED | 僅 APPROVED 且無未解決 RC 之 PR 可 Merge（依 §6 R-005、§7.1） |

規則：

- Review Consistency Validation MUST 於 Review 與 Merge 前執行，並併入 §9.1 Checklist。
- 不一致之 Review 記錄 MUST 記為不合規，修正後始可合併。

## 9. Compliance

- 本標準適用之正式治理資產 MUST 符合本標準（依 AEOS-CON-001 §11）。
- 不合規之資產 MUST NOT 合併至 main（依 AEOS-CON-001 GP-009）。
- 本標準之變更 MUST 經 EWO 與 Review 後合併。
- 與 AEOS-DIA-001／AEOS-CON-001／AEOS-ARCH-002 衝突時，以上位文件為準（依 Governance Hierarchy，AEOS-ARCH-002 §5）。
- 本標準 MUST NOT 重新定義 Metadata（AEOS-STD-002）、Naming（AEOS-STD-004）、Cross-reference（AEOS-STD-003）或 Documentation Format（AEOS-STD-001）之規則。

### 9.1 Review Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| Review ID | Review ID 符合 `<ReviewType>-AEOS-<####>-R<##>`；型別依 §4 |
| RC 格式 | RC 依序編號（RC-###）；含主題、內容與處理要求；狀態已追蹤 |
| Traceability | EWO → Review → RC → Commit → Decision 可追溯；frontmatter related 含 EWO／Review ID |
| Approval | Merge 前有 Review Owner 之 APPROVED 決策 |
| Merge 條件 | 僅 APPROVED 且無未解決 RC 之 PR 合併 |
| Review History | Review ID、決策與 RC 記錄於 PR 及 Revision History |
| Decision 宣告 | 決策記錄於 PR，無 PR 外認定 |
| Close | Merge／Reject／Supersede 後 PR 已 Close；紀錄保留 |

### 9.2 Review Integrity Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| 無 Missing RC | 全部 Review 意見均以 RC 編號記錄（依 §6 R-002） |
| 無 Unresolved RC | 全部 RC 達 Resolved 後始可 Approval／Merge（依 §7.1） |
| 無 Duplicate Review ID | Review ID 全 Repository 唯一（依 §8.1 C-001） |
| 無 Invalid Decision | Decision 為 §7 四種之一且宣告於 PR（依 §7） |
| 無 Missing Traceability | EWO → Review → RC → Commit → Decision 可追溯（依 §6 R-003） |

## 10. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Governance Hierarchy 與治理結構 |
| REF-004 | AEOS-ARCH-003 — Architecture Decision Record System | Architecture | ADR Review 與決策狀態之依據 |
| REF-005 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | 治理原則（GP-009）與核准角色 |
| REF-006 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | Review Owner 角色與文件生命週期 |
| REF-007 | AEOS-GOV-001 — Enterprise Governance Roadmap（Approved v1.0.0） | Governance | Planned Standards 與優先序 |
| REF-008 | AEOS-STD-001 — Documentation Format Standard（Approved v1.0.0） | Standard | 文件格式與撰寫規則 |
| REF-009 | AEOS-STD-002 — Metadata Standard（Approved v1.0.0） | Standard | Metadata 與 frontmatter 規則 |
| REF-010 | AEOS-STD-003 — Cross-reference Standard（Approved v1.0.0） | Standard | 引用形式與一致性驗證 |
| REF-011 | AEOS-STD-004 — Naming Standard（Approved v1.0.0） | Standard | Review ID 格式與命名規則 |
| REF-012 | EWO-AEOS-0012 — Review Standard | EWO | 本文件之工作來源 |
| REF-013 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

本標準（AEOS-STD-005）為 AEOS 唯一 Review 標準來源（Single Source of Truth）；其他文件 MUST NOT 定義相異之 Review 規則。

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.4.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 3（AEOS-ADR-002 已核准）：執行 Governance Authority Transition——WA-001 分類為歷史來源（Historical Reference）；AR 來源追溯與 Escalation Authority 重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.3.0 | 2026-08-06 | 依 EWO-AEOS-0037 擴充 Architecture Review（AR）適用資產至 Architecture Candidate Assessment RPT；定義其唯一 Review 路徑（AR 同時完成內容審查與 Lifecycle 核准，不疊加 RT）；明定 PR 為 Review Record 載體、AR 判定（Approved／Approved with Conditions／Rejected／Deferred）、RPT 僅記錄 Review ID 與結果摘要、Catalog 登錄 Traceability 須含 Approved RPT 與 AR Review Record、必要時仍須依 AEOS-ARCH-003 建立 ADR（SR-AEOS-0037-R2） | Codex |
| 1.2.0 | 2026-08-06 | 依 EWO-AEOS-0033 新增 Report Review（RT）：適用 RPT 文件，審查分析完整性、結論 Fact Authority 與建議可執行性（SR-AEOS-0033-R3） | Codex |
| 1.1.0 | 2026-08-06 | 依 EWO-AEOS-0022 新增 Catalog／Matrix Review（CM）：適用 CAT、MAT 文件，審查 Catalog／Matrix 一致性、條目追溯與關係事實（依 AEOS-STD-006） | Codex |
| 1.0.0 | 2026-08-06 | 依 Standard Review（SR-AEOS-0012-R1）修正：狀態升版至 Approved 1.0.0；Review Model 新增 Review Hierarchy（Enterprise／Repository／Document Review；Review MUST 遵循 Governance Hierarchy，不得由下層 Review 覆蓋上層已核准決策）；Review Workflow 新增 Review Escalation（Escalation Trigger／Authority／Resolution；適用於 Architecture、Governance、Standard、Policy 等重大 Review）；Review Decision Model 新增 Decision Finality（APPROVED 為正式核准；REJECTED 保留歷史；SUPERSEDED 必須引用新 Decision；REQUEST CHANGES 必須全部 RC 完成後才能重新 Review）；Validation 新增 Review Consistency Validation（C-001～C-005）；Compliance 新增 Review Integrity Checklist | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Review Model、Review Types、Review Workflow、Review Rules、Review Decision Model、Review Validation 與 Compliance（EWO-AEOS-0012） | Codex |
