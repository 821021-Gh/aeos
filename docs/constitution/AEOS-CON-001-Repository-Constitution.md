---
doc-id: AEOS-CON-001
doc-name: Repository Constitution
doc-type: Constitution
repository: AEOS
version: 1.0.0
status: Approved
owner: Repository Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0004
  - CR-AEOS-0004-R1
  - AEOS-ARCH-001
  - AEOS-DIA-001
  - WA-001
---

# AEOS-CON-001 — Repository Constitution

> EWO-AEOS-0004：建立 AEOS Repository Constitution，定義 Repository 之正式身分、使命、治理原則、責任邊界與變更管理。本文件僅定義 Repository Governance，不重新設計 Enterprise Architecture，不重複描述 WA-001 或 AEOS-ARCH-001。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-CON-001 |
| 文件名稱 | Repository Constitution |
| 型別 | Constitution |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0004、CR-AEOS-0004-R1、AEOS-ARCH-001、AEOS-DIA-001、WA-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0004、CR-AEOS-0004-R1、AEOS-ARCH-001、AEOS-DIA-001、WA-001 |

## 1. Purpose

本文件為 AEOS（AI Enterprise Operating System）之 Repository Constitution，其目的為：

- 定義 AEOS Repository 之正式身分、使命、治理原則、責任邊界與變更管理，作為 Repository 最高層級之治理文件。
- 建立 Repository Governance 之正式基線，使 Repository 之內容、所有權、變更與合規有一致之決策依據。
- 界定 Repository Governance 與 Enterprise Architecture 之界線：本文件治理「Repository 如何運作與演進」，不重新設計架構，不重複描述 WA-001 或 AEOS-ARCH-001。
- 提供 Repository 層級之文件錨點，使 EWO → 正式文件 → Review → Release 之演進可追溯。

## 2. Repository Identity

- 名稱：AEOS（AI Enterprise Operating System）。
- 定位：AI Engineering Workspace 之 Enterprise Root Repository（依 AEOS-ARCH-001 定義）。
- 正式範疇：Enterprise Architecture（以 WA-001 為唯一架構來源）、Platform Governance 與 Capability Management 之企業層級文件與治理。
- 身分要點：
  - 正式入口：Workspace 企業層級架構與治理文件之正式家園。
  - 文件優先：正式文件為主要交付物。
  - 單一來源：Repository 內容追溯至已核准來源，不重新定義。
  - 分離管理：Repository Foundation 與架構／治理內容分離管理。
  - 正式交付：內容完整、Production Ready，不使用 Placeholder。

Repository Identity 一經建立即穩定；任何身分變更須依 §10 Change Management 之正式程序進行。

## 3. Mission

AEOS 之使命為：

- 作為 AI Engineering Workspace 之企業層級權威 Repository，維護已核准之架構基線與企業層級治理文件。
- 提供 Enterprise Architecture、Platform Governance 與 Capability Management 文件之正式入口與單一參考點。
- 確保 Repository 內容在身分、責任、變更與合規上有一致之治理，支持 Workspace 之長期演進。

## 4. Repository Scope

### 4.1 In Scope

本 Constitution 涵蓋：

- 正式文件體系：依 AEOS-DIA-001 分類與目錄組織管理之正式文件（ARCH、DIA、CON、SPEC、GOV、CAP、POL、STD 等）。
- Repository Foundation：README、VERSION、CHANGELOG、LICENSE、CONTRIBUTING 等基礎文件。
- Repository Governance：Repository 之身分、使命、所有權、變更管理與合規。
- 工程流程採用宣告：AEOS 採用 YEOS Engineering Workflow（見 CONTRIBUTING.md）。

### 4.2 Out of Scope

本 Constitution 明確不涵蓋：

- Business Capability Definition。
- Product Implementation。
- Technical Architecture Design。
- Source Code Implementation。
- Runtime Operations。
- Enterprise Architecture 內容：以 WA-001 為唯一架構來源，AEOS-ARCH-001 為架構 Entry Document；本文件不重新設計、不新增、不重述架構內容。
- Workspace 其他 Repository 之內容與治理細則。
- YEOS Engineering Workflow 之重述或取代。
- 文件資訊架構之細則（由 AEOS-DIA-001 定義）。

## 5. Repository Responsibilities

AEOS Repository 之責任為：

- 維護 AI Engineering Workspace 之正式文件基線，作為架構與治理文件之正式入口。
- 依 AEOS-ARCH-001 維持架構文件之追溯性；架構決策由 WA-001 持有，AEOS 不另行持有。
- 管理 Platform Governance 與 Capability Management 之正式文件。
- 維持 Repository Foundation 與架構／治理內容分離。
- 交付 Production Ready、無 Placeholder 之正式文件。
- 執行 Repository 層級之身分、所有權、變更與合規治理。

## 6. Governance Principles

| # | 原則 | 說明 |
|---|------|------|
| GP-001 | Documentation First | 正式文件為 AEOS 之主要交付物。 |
| GP-002 | Single Source of Truth | 每一主題僅存在一個權威來源；其他文件以引用取代重述。 |
| GP-003 | Specification Driven | Repository 變更先由 EWO 定義範圍，再進行實作。 |
| GP-004 | Architecture Traceability | 架構相關文件 MUST 宣告並追溯至 AEOS-ARCH-001／WA-001。 |
| GP-005 | Content Boundary | 文件內容 MUST NOT 超出其宣告之 Scope；本文件不涵蓋架構內容。 |
| GP-006 | Decoupled Foundation | Repository Foundation 與架構／治理內容分離管理。 |
| GP-007 | Formal by Default | 正式文件以完整內容交付，MUST NOT 使用 Placeholder。 |
| GP-008 | Stable Identity | Repository 身分與 doc-id 一經建立即穩定；變更須經正式程序。 |
| GP-009 | Governance by Review | Repository Governance MUST 透過正式 Review 於 merge 前強制執行。 |

文件體系層級之設計原則與細則依 AEOS-DIA-001；本文件不重述。

## 7. Relationship with WA-001

- WA-001（AI Engineering Workspace Architecture，Approved v1.0.0）為 AI Engineering Workspace 之唯一架構來源，經 AEOS-ARCH-001 正式導入 AEOS。
- 本文件治理 Repository，不治理架構；架構內容之權威來源為 WA-001，Entry Document 為 AEOS-ARCH-001。

規則：

- MUST NOT 重新設計、新增或變更 WA-001 已定義之架構內容。
- 架構內容以 AEOS-ARCH-001 為引用樞紐；本文件不持有架構內容。
- 架構相關文件 MUST 依 AEOS-ARCH-001 宣告其架構來源。
- Repository Governance 與架構內容衝突時，架構內容以 WA-001／AEOS-ARCH-001 為準；本文件僅治理 Repository。

## 8. Relationship with Workspace Repositories

- AEOS 為 AI Engineering Workspace 之 Enterprise Root Repository；Workspace 內其他 Repository 以產品、平台或領域 Repository 形式存在。

邊界規則：

- AEOS 不實作工程交付流程；工程交付依 YEOS Engineering Workflow（見 CONTRIBUTING.md）。
- AEOS 不持有其他 Repository 之內容；其他 Repository 不持有 AEOS 之企業層級架構／治理權威內容（Single Source of Truth）。
- Repository 間之正式關係以各 Repository 之正式文件宣告，不以內容複製建立。
- 其他 Workspace Repository 之治理由其自身 Repository Governance 文件定義；本文件僅治理 AEOS。

## 9. Repository Ownership

| 角色 | 擁有範圍 | 職責 |
|------|----------|------|
| Repository Owner | Repository 層級 | 最終核准者；持有本文件（AEOS-CON-001）與 Repository 層級治理文件。 |
| Architecture Owner | 架構相關文件 | 確保架構文件之內容正確性與追溯性（依 AEOS-ARCH-001／AEOS-DIA-001）。 |
| Document Owner | 單份正式文件 | 由文件 frontmatter 宣告；負責文件之維護與變更。 |
| Review Owner | Review 流程 | 執行 Architecture／Repository Review；不等同 Repository Owner。 |
| Engineering Contributor | EWO 範圍之實作 | 依 EWO 完成 Repository 實作；不具有 Governance Approval 權限。 |

規則：

- Repository Owner 為本文件之擁有者與最終核准者。
- 文件層級之 Ownership Model 細則以 AEOS-DIA-001 為準；本文件不重述。
- 每份正式文件 MUST 於 frontmatter 宣告 owner。

## 10. Change Management

- 本文件之任何變更 MUST 依 YEOS Engineering Workflow 以 EWO 定義範圍，經 Review 後由 Repository Owner 核准。
- 正式文件合併至 main 前 MUST 通過 Review。
- 版本依 SemVer 管理：Review 修正更新 minor，重大變更（身分、使命、Scope、治理原則、所有權）更新 major。
- 每次變更 MUST 更新 §13 Revision History。
- Constitution 變更 SHOULD 保留向後相容性（Backward Compatibility），除非重大治理修訂（major governance revision）已明確核准。
- 本文件之變更 MUST NOT 改變 WA-001／AEOS-ARCH-001 之架構內容；架構變更循架構治理程序。
- 文件狀態與生命週期依 AEOS-DIA-001 管理。

## 11. Compliance

- 本文件為 Repository Governance 之正式基線；Repository 內容 MUST 符合本文件。
- 新增正式文件 MUST：依 AEOS-DIA-001 選擇分類與目錄、宣告唯一 doc-id／owner／status，並於 References 宣告架構來源（如適用）。
- 任何文件 MUST NOT 重新定義 WA-001／AEOS-ARCH-001 之架構內容，MUST NOT 以內容複製取代引用。
- Repository 操作（Branch、Commit、PR、Review）遵循 YEOS Engineering Workflow 與 CONTRIBUTING.md。
- 不合規處理：由 Review Owner 於 Review 指出，以 EWO 修正後再行合併。

## 12. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與引用樞紐 |
| REF-003 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | 文件分類、Ownership 與生命週期 |
| REF-004 | YEOS Engineering Workflow（CONTRIBUTING.md） | Engineering Workflow | AEOS 採用之工程流程 |
| REF-005 | EWO-AEOS-0004 — Repository Constitution | EWO | 本文件之工作來源 |
| REF-006 | README.md | Repository Entry Document | AEOS Repository 入口文件 |

## 13. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | 依 Constitution Review（CR-AEOS-0004-R1）修正：狀態升版至 Approved 1.0.0；Repository Scope 新增 Out of Scope（Business Capability Definition、Product Implementation、Technical Architecture Design、Source Code Implementation、Runtime Operations）；新增 GP-009 Governance by Review；Repository Ownership 新增 Engineering Contributor 角色；Change Management 新增 Backward Compatibility 原則；References 新增 README.md（Repository Entry Document） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Repository 身分、使命、Scope、責任、治理原則、與 WA-001／Workspace Repositories 之關係、所有權、變更管理與合規（EWO-AEOS-0004） | Codex |
