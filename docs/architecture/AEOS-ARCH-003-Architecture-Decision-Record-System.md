---
doc-id: AEOS-ARCH-003
doc-name: Architecture Decision Record System
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0006
  - AR-AEOS-0006-R1
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-DIA-001
  - AEOS-CON-001
  - WA-001
---

# AEOS-ARCH-003 — Architecture Decision Record System

> EWO-AEOS-0006：依 WA-001、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001 與 AEOS-ARCH-002 建立 AEOS 之 Architecture Decision Record（ADR）System。本文件定義 ADR Framework，不是第一份 ADR。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-003 |
| 文件名稱 | Architecture Decision Record System |
| 型別 | Architecture |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0006、AR-AEOS-0006-R1、WA-001（Approved v1.0.0）、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0）、AEOS-ARCH-002 |
| 關聯文件 | EWO-AEOS-0006、AR-AEOS-0006-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-DIA-001、AEOS-CON-001、WA-001 |

## 1. ADR Architecture

ADR System 為 Enterprise Governance 之 Decision Governance 領域（依 AEOS-ARCH-002 §4）所定義之正式決策記錄系統，其架構為：

- ADR 文件：置於 `docs/adr/` 之正式決策記錄，依 AEOS-DIA-001 Taxonomy 之 ADR 類別管理。
- ADR Framework：本文件，定義 Taxonomy、Lifecycle、Numbering、Template、Governance、Cross-reference 與 Status Model。
- ADR Register：記錄全部 ADR 之編號、名稱與狀態之索引（由後續 IDX 文件建立與維護）。
- ADR 於 Governance Hierarchy 之定位：H6（依 AEOS-ARCH-002 §5），與 Review Records（H7）共同承載 Decision Governance。

ADR 記錄架構決策之背景、決策、理由與影響（依 AEOS-DIA-001 §3 ADR 類別定義）；本文件不記錄任何具體決策。

## 2. ADR Taxonomy

ADR 依決策類型分類；類型決定 ADR 之審查重點與關聯文件：

| ADR 類型 | 決策對象 | 用途 |
|----------|----------|------|
| Architecture Decision | Enterprise Architecture 決策 | 記錄架構基線、架構文件與架構演進之決策。 |
| Repository Decision | Repository 治理決策 | 記錄 Repository 身分、治理原則與變更管理之決策。 |
| Governance Decision | 治理決策 | 記錄 Governance Architecture 與治理內容之決策。 |
| Documentation Decision | 文件體系決策 | 記錄 Documentation Information Architecture 與文件體系之決策。 |
| Platform Decision | 平台層級決策 | 記錄 Platform Governance 相關之決策。 |

規則：

- Review Decision 由 Review Records 承載（依 AEOS-ARCH-002 §5 H7）；ADR 以 Cross-reference 與其關聯，不重複記錄。
- 一個 ADR MUST 僅記錄一個決策；多個決策以多份 ADR 分別記錄。
- Taxonomy 之擴充僅更新本文件；不因 Taxonomy 變更而建立 ADR 文件。實際 ADR 由後續 EWO 依 §5 ADR Template 建立。

## 3. ADR Lifecycle

ADR 依下列流程演進，文件狀態依 AEOS-DIA-001 §8 管理：

| 階段 | 輸入 | Gate | 輸出 |
|------|------|------|------|
| 1. Initiation | 決策需求 | EWO 定義範圍 | 核准之 EWO |
| 2. Draft | EWO | 依 ADR Template（§5）建立 | Proposed ADR |
| 3. Review | Proposed ADR | 正式 Review | Review 結果 |
| 4. Approval | Review 結果 | RC 修正／核准 | Approved ADR |
| 5. Release | Approved ADR | 合併至 main | Released ADR |
| 6. Supersede／Deprecate | 新決策或失效 | 新 ADR／Deprecation 宣告 | Superseded／Deprecated ADR |
| 7. Archive | 不再有效之 ADR | Archive 宣告 | Archived ADR |

規則：

- 每一階段 MUST 可追溯至前一階段之輸入（EWO、Review 或 ADR）。
- ADR 之狀態變更 MUST 更新 ADR Register 與 Revision History。

## 4. ADR Numbering

- 編號格式：`ADR-<###>`（例如 `ADR-001`）；doc-id 格式為 `AEOS-ADR-###`（依 AEOS-DIA-001 命名規則）。
- 檔案命名：`AEOS-ADR-###-Kebab-Case-Name.md`，置於 `docs/adr/`。
- 編號由 ADR Register 管理；編號一經發布即穩定，MUST NOT 重用。
- 新 ADR MUST 依序取得下一個可用編號；被取代或封存之 ADR 保留原編號。

## 5. ADR Template

每份 ADR MUST 依下列 Template 建立：

| 區段 | 內容 |
|------|------|
| Frontmatter | doc-id（AEOS-ADR-###）、doc-name、doc-type（ADR）、repository（AEOS）、version、status、decision-status、decision-owner、decision-date、owner、created、updated、related、supersedes。 |
| 1. Title | ADR 標題：`AEOS-ADR-### — <決策名稱>`。 |
| 2. Context | 決策背景、問題與驅動因素。 |
| 3. Decision | 已核准之決策內容與理由。 |
| 4. Alternatives | 已考量之替代方案與未採用之理由。 |
| 5. Consequences | 決策之影響，包含正面與負面影響。 |
| 6. References | 來源 EWO、架構來源與關聯文件（doc-id 與相對路徑）。 |
| 7. Revision History | 版本、日期、變更摘要與作者。 |

規則：

- Template 之變更視為本文件之變更，需經 EWO 與 Review。
- 實際 ADR 由後續 EWO 依本 Template 建立，不屬於本文件範圍。

## 6. ADR Governance

- ADR 之建立與變更 MUST 依 YEOS Engineering Workflow 以 EWO 提出，經正式 Review 後合併（依 AEOS-CON-001 GP-003／GP-009）。
- 每份 ADR MUST 於 frontmatter 宣告 owner；Document Owner 負責維護，Architecture Owner 負責架構相關 ADR 之內容審查。
- ADR MUST NOT 重述既有文件內容；以 References 引用取代複製。
- ADR MUST NOT 變更 WA-001／AEOS-ARCH-001 已核准之架構內容；決策與架構衝突時，先修訂架構來源，再以 ADR 記錄決策。
- ADR 核准依 Governance Hierarchy（AEOS-ARCH-002 §5）與 Review 流程；核准結果記錄於 Review Records。

### 6.1 Supersede Rules

- 新 ADR 以 `supersedes` 宣告取代之舊 ADR；舊 ADR 以 `superseded-by` 標記被取代關係。
- 舊 ADR MUST NOT 修改已核准之 Decision；決策變更以新 ADR 記錄，舊 ADR 僅更新狀態與 `superseded-by`。
- 被取代之 ADR 狀態轉為 Superseded（依 §8 ADR Status Model），並保留完整歷史內容。
- `supersedes`／`superseded-by` 關係 MUST 記錄於 ADR frontmatter 與 ADR Register。

## 7. ADR Cross-reference

| 關係 | 引用方式 | 用途 |
|------|----------|------|
| ADR ↔ 架構來源 | 引用 WA-001／AEOS-ARCH-001／AEOS-ARCH-002 | 宣告決策之架構依據。 |
| ADR ↔ Constitution | 引用 AEOS-CON-001 | Governance Decision Reference。 |
| ADR ↔ EWO | 引用來源 EWO | 決策可追溯至工作來源。 |
| ADR ↔ ADR | supersedes／superseded-by | 記錄決策取代關係。 |
| ADR ↔ Review Records | 引用 Review 結果與 RC | 記錄核准依據。 |
| ADR ↔ 治理文件 | 引用 POL／STD／SPEC／GOV | 記錄決策影響之治理內容。 |

規則：

- 跨文件引用 MUST 使用 doc-id 與相對路徑，MUST NOT 複製被引用內容（依 AEOS-DIA-001 §7）。
- 被引用文件移動或變更時，ADR 須於同一或後續 EWO 更新引用，不得保留失效引用。

## 8. ADR Status Model

| 狀態 | 定義 | 對應 DIA 文件狀態 |
|------|------|-------------------|
| Proposed | ADR 建立中，決策尚未核准。 | Draft |
| Approved | 決策已核准並發布。 | Approved／Released |
| Rejected | 決策未核准；ADR 保留歷史紀錄。 | Archived |
| Superseded | 決策已由後續 ADR 取代。 | Deprecated |
| Deprecated | 決策不再有效。 | Deprecated |
| Archived | ADR 封存，保留歷史紀錄。 | Archived |

規則：

- ADR 狀態於 frontmatter 以 `decision-status` 記錄；文件狀態以 `status` 記錄（依 AEOS-DIA-001 §8）。
- 狀態轉換 MUST 依 §3 ADR Lifecycle 進行，並更新 ADR Register 與 Revision History。
- Rejected ADR 保留原編號與歷史內容；編號 MUST NOT 重用（依 §4 ADR Numbering）。

## 9. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | 文件分類、命名、生命週期與引用 |
| REF-004 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 治理基線 |
| REF-005 | AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Decision Governance 與 Governance Hierarchy |
| REF-006 | EWO-AEOS-0006 — Architecture Decision Record System | EWO | 本文件之工作來源 |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | 依 Architecture Review（AR-AEOS-0006-R1）修正：狀態升版至 Approved 1.0.0；ADR Taxonomy 新增 Governance Decision、Documentation Decision、Platform Decision；ADR Template 新增 Decision Owner、Decision Date；ADR Governance 新增 Supersede Rules；ADR Cross-reference 新增 ADR ↔ Constitution（Governance Decision Reference）；ADR Status Model 新增 Rejected | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 ADR System 之 ADR Architecture、Taxonomy、Lifecycle、Numbering、Template、Governance、Cross-reference 與 Status Model（EWO-AEOS-0006） | Codex |
