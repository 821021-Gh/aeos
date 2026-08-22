---
doc-id: AEOS-GOV-001
doc-name: Enterprise Governance Roadmap
doc-type: Governance
repository: AEOS
version: 1.3.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-22
related:
  - EWO-AEOS-0007
  - EWO-AEOS-0022
  - EWO-AEOS-0043
  - GR-AEOS-0007-R1
  - GR-AEOS-0043-R1
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-CON-001
  - AEOS-DIA-001
  - AEOS-STD-007
  - AEOS-ADR-002
  - WA-001
---

# AEOS-GOV-001 — Enterprise Governance Roadmap

> EWO-AEOS-0007：依 AEOS-ADR-002、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001 與 AEOS-DIA-001 建立 AEOS 之 Enterprise Governance Roadmap。本文件定義 AEOS Repository 自身之治理建設 Roadmap，作為後續 EWO Planning 之正式依據；不是 Product Roadmap、Platform Roadmap 或 Workspace Roadmap。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-GOV-001 |
| 文件名稱 | Enterprise Governance Roadmap |
| 型別 | Governance |
| 狀態 | Approved |
| 版本 | 1.3.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-22 |
| 依據文件 | EWO-AEOS-0007、EWO-AEOS-0043、GR-AEOS-0007-R1、GR-AEOS-0043-R1、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001（Approved v1.0.0）、AEOS-DIA-001 |
| 關聯文件 | EWO-AEOS-0007、EWO-AEOS-0043、GR-AEOS-0007-R1、GR-AEOS-0043-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-CON-001、AEOS-DIA-001、AEOS-STD-007、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Current Foundation

本 Roadmap 以下列已完成之治理基線為起點；本文件不重新定義其內容：

| 文件 | 型別 | 狀態 | 角色 |
|------|------|------|------|
| AEOS-CON-001 — Repository Constitution | Constitution | Approved 1.0.0 | Repository 治理基線 |
| AEOS-ARCH-001 — Architecture Baseline | Architecture | Approved | 架構 Entry Document 與 Register |
| AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | Approved | 文件分類、生命週期與引用 |
| AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Approved | 治理結構（Layers、Domains、Hierarchy） |
| AEOS-ARCH-003 — Architecture Decision Record System | Architecture | Approved | ADR Framework（Decision Governance） |
| Repository Foundation（README、CHANGELOG 等） | Foundation | Released | Repository 入口與基礎 |

規則：已完成文件之內容以各文件為準；Roadmap 僅引用，不重述、不重新定義。

## 2. Governance Milestones

| Milestone | 目標 | 涵蓋 | 狀態 |
|-----------|------|------|------|
| M1 — Governance Foundation | 建立 Repository 治理基線 | AEOS-CON-001、AEOS-ARCH-001、AEOS-DIA-001 | 已完成 |
| M2 — Governance Architecture | 建立治理結構與 ADR System | AEOS-ARCH-002、AEOS-ARCH-003 | 已完成 |
| M3 — Governance Content | 建立治理內容基礎（Standards、Policies） | §4 Planned Standards、§5 Planned Policies | 待執行 |
| M4 — Governance Catalogs | 建立索引與目錄 | §6 Planned Catalogs | 待執行 |
| M5 — Governance Operationalization | 治理營運化（Review Framework、Domain 治理內容） | §7 Planned Frameworks、§3 Planned Domains | 待執行 |

Current Phase：AEOS 目前位於 M2 — Governance Architecture（已完成）；下一階段為 M3 — Governance Content（待執行）。

### 2.1 Milestone Naming Alignment

本 Roadmap 之 M1～M5 為治理里程碑命名；架構 EWO 系列使用獨立里程碑標籤（M4 — Enterprise Architecture Foundation、M5 — Enterprise Architecture Catalogs）。兩套命名互不取代：

| 命名體系 | 里程碑 | 涵蓋 |
|----------|--------|------|
| 本 Roadmap（治理） | M4 — Governance Catalogs | §6 Planned Catalogs（Architecture Catalog、ADR Register、Document Index、Governance Catalog） |
| 本 Roadmap（治理） | M5 — Governance Operationalization | §7 Planned Frameworks、§3 Planned Domains |
| 架構 EWO 系列 | M4 — Enterprise Architecture Foundation | AEOS-ARCH-005～010（Platform、Layer、Capability、Repository、Dependency、Workspace Architecture） |
| 架構 EWO 系列 | M5 — Enterprise Architecture Catalogs | Platform／Capability／Repository／Workspace Catalog 與 Ownership／Dependency Matrix（依 AEOS-STD-006） |

規則：

- 本 Roadmap 之里程碑以本文件為準；架構系列里程碑以對應 EWO／PR 為準。
- M5 — Enterprise Architecture Catalogs 對應本 Roadmap §6 Planned Catalogs 之架構資產子集，其統一 Schema 依 AEOS-STD-006。

## 3. Planned Domains

依 AEOS-ARCH-002 §4 Governance Domains，尚未具備正式內容之領域：

| Domain | 目的 | 來源 | 狀態 |
|--------|------|------|------|
| Platform Governance | 建立平台層級治理之正式內容與決策 | 正式內容由後續 EWO 建立（不依 WA-001） | Planned |
| Capability Governance | 建立能力定義、擁有權與管理之正式內容 | 正式內容由後續 EWO 建立（不依 WA-001） | Planned |

Architecture Governance、Documentation Governance、Repository Governance 與 Decision Governance 已分別由 AEOS-ARCH-001／AEOS-DIA-001／AEOS-CON-001／AEOS-ARCH-003 建立，不列入 Planned。

## 4. Planned Standards

| Standard | 目的 | 優先序 |
|----------|------|--------|
| Documentation Format Standard | 定義 AEOS 正式文件之格式、章節與撰寫慣例 | P1 |
| Metadata Standard | 定義 frontmatter 欄位、值域與填寫規則 | P1 |
| Cross-reference Standard | 定義跨文件引用之形式與維護規則 | P1 |
| Naming Standard | 定義文件、目錄與檔案命名之標準 | P1 |
| Review Standard | 定義 Review 程序、CR／RC 與核准之標準 | P2 |
| AI Engineering Context and Token Budget Standard | 定義按需 Context、Token Budget、狀態快照、Delta Output、模型分級與 Cache 原則 | P1 |

規則：Standards 以 AEOS-DIA-001 為上位依據，將既有文件規則操作化；不重新定義已完成文件。

## 5. Planned Policies

| Policy | 目的 | 優先序 |
|--------|------|--------|
| Repository Policy | 定義 Repository 內容與文件之治理政策 | P1 |
| Architecture Policy | 定義架構基線維護與變更之政策 | P1 |
| Change Management Policy | 定義變更管理程序之政策 | P1 |
| Documentation Policy | 定義文件建立、維護與發布之政策 | P2 |

規則：Policies 以 AEOS-CON-001 與 AEOS-ARCH-002 為上位依據；不重述 Constitution 內容。

## 6. Planned Catalogs

| Catalog | 目的 | 優先序 |
|---------|------|--------|
| Architecture Catalog | 依 AEOS-ARCH-001 §8 Register 擴充為完整架構目錄 | P2 |
| ADR Register | 記錄全部 ADR 之編號、名稱與狀態（依 AEOS-ARCH-003 §1） | P2 |
| Document Index | 全部正式文件之總覽索引 | P2 |
| Governance Catalog | 管理 Standards、Policies、Frameworks、Reviews 之索引 | P2 |

## 7. Planned Frameworks

| Framework | 目的 | 優先序 |
|-----------|------|--------|
| Governance Review Framework | 定義 Review Records、CR／RC 與核准之操作框架 | P2 |
| Capability Management Framework | 定義 Capability 管理之操作框架（依 Approved 架構載體） | P3 |
| Platform Governance Framework | 定義 Platform 治理之操作框架（依 Approved 架構載體） | P3 |

## 8. Dependencies

| # | 依賴 | 影響 | 狀態 |
|---|------|------|------|
| DEP-001 | AEOS-ARCH-001／Approved 架構載體 | Platform／Capability 領域內容 | 已核准 |
| DEP-002 | AEOS-ARCH-001 | Architecture Catalog、架構文件 | 已完成 |
| DEP-003 | AEOS-ARCH-002 | Governance Domains、Review Framework | 已完成 |
| DEP-004 | AEOS-ARCH-003 | ADR Register、Review Records | 已完成 |
| DEP-005 | AEOS-CON-001 | 治理原則、變更管理 | 已完成 |
| DEP-006 | AEOS-DIA-001 | Standards、Policies 之文件形式 | 已完成 |
| DEP-007 | YEOS Engineering Workflow（CONTRIBUTING.md） | 所有 EWO 交付流程 | 已採用 |
| DEP-008 | Approved Reviews | 已完成項目（Current Foundation）之核准與 Merge 依據 | 已實施 |

規則：Planned 項目之 EWO MUST 宣告其 Dependencies；依賴未完成前，不啟動依賴者。已完成項目 MUST 建立於已完成之正式 Review 並已 Merge（DEP-008）。

## 9. Priority

| 優先序 | 涵蓋 | 原則 |
|--------|------|------|
| P0 | 已完成項目（M1、M2） | 已交付，納入 Current Foundation |
| P1 | Planned Standards、Planned Policies（M3） | 治理內容基礎，優先於目錄與框架 |
| P2 | Planned Catalogs、Governance Review Framework（M4、M5 部分） | 依 P1 文件穩定後建立 |
| P3 | Platform／Capability Governance 內容（M5） | 依 Approved 架構載體與 P1／P2 成果展開 |

規則：

- 依 EWO 一次一件執行；高優先未完成前，不展開低優先項目。
- 優先序調整 MUST 經本 Roadmap 之變更程序（§10）。

## 10. Evolution Strategy

- 本 Roadmap 為後續 EWO Planning 之正式依據；新治理需求 MUST 先納入 Roadmap，或依既有 Roadmap 項目規劃 EWO。
- 本 Roadmap 之更新（新增／調整 Milestone、優先序或 Planned 項目）MUST 經 EWO 與 Review（依 AEOS-CON-001 §10）。
- 已完成項目移入 §1 Current Foundation；Roadmap 不重新定義已完成文件。
- 本 Roadmap 不承諾時程；以 Milestone 與 Priority 管理演進順序。
- 本 Roadmap SHOULD 於每個已完成之 Milestone 後進行 Review。
- 每次變更 MUST 更新 References 與 Revision History。

## 11. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-ARCH-002 — Enterprise Governance Architecture | Architecture | Governance Domains 與治理結構 |
| REF-004 | AEOS-ARCH-003 — Architecture Decision Record System | Architecture | ADR System 與 ADR Register |
| REF-005 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 治理基線 |
| REF-006 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | 文件分類、生命週期與引用 |
| REF-007 | EWO-AEOS-0007 — Enterprise Governance Roadmap | EWO | 本文件之工作來源 |
| REF-008 | AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard | Standard | Catalog／Matrix 型別、Schema 與治理規則 |
| REF-009 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |
| REF-010 | AEOS-STD-007 — AI Engineering Context and Token Budget Standard（Review 0.2.0） | Standard | 按需 Context、Token Budget、狀態快照、Delta Output、模型分級與 Cache 原則 |
| REF-011 | GR-AEOS-0043-R1 — Governance Review | Review | Roadmap 修訂與 PR #49 生命週期追溯 Review；決策為 REQUEST CHANGES |

## 12. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.3.0 | 2026-08-22 | 依 EWO-AEOS-0043 將 AI Engineering Context and Token Budget Standard 納入 §4 Planned Standards，作為按需 Context、狀態快照、Delta Output、模型分級與 Cache 治理之 Roadmap 依據；GR-AEOS-0043-R1 記錄 PR #49 合併後之 Review Traceability 修正，等待 R2 | Codex |
| 1.2.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 3（AEOS-ADR-002 已核准）：執行 Governance Authority Transition——WA-001 分類為歷史來源（Historical Reference）；Governance Domains、Dependency、References 重錨至 AEOS-ARCH-001／Approved 架構載體（EWO-AEOS-0040） | Codex |
| 1.1.0 | 2026-08-06 | 依 EWO-AEOS-0022 新增 Milestone Naming Alignment（§2.1）：釐清本 Roadmap 治理里程碑（M4 Governance Catalogs、M5 Governance Operationalization）與架構 EWO 系列里程碑（M4 Enterprise Architecture Foundation、M5 Enterprise Architecture Catalogs）之命名差異與對應關係；M5 Catalogs 之 Schema 依 AEOS-STD-006 | Codex |
| 1.0.0 | 2026-08-06 | 依 Governance Review（GR-AEOS-0007-R1）修正：狀態升版至 Approved 1.0.0；Governance Milestones 新增 Current Phase（M2 已完成、M3 為下一階段）；Planned Standards 新增 Review Standard、Naming Standard；Planned Catalogs 新增 Governance Catalog（管理 Standards、Policies、Frameworks、Reviews）；Dependencies 新增 DEP-008 Approved Reviews；Evolution Strategy 新增 Milestone 完成後 Review | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Current Foundation、Governance Milestones、Planned Domains、Planned Standards、Planned Policies、Planned Catalogs、Planned Frameworks、Dependencies、Priority 與 Evolution Strategy（EWO-AEOS-0007） | Codex |
