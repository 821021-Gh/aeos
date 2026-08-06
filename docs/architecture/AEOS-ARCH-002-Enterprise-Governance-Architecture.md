---
doc-id: AEOS-ARCH-002
doc-name: Enterprise Governance Architecture
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0005
  - AR-AEOS-0005-R1
  - AEOS-ARCH-001
  - AEOS-DIA-001
  - AEOS-CON-001
  - WA-001
---

# AEOS-ARCH-002 — Enterprise Governance Architecture

> EWO-AEOS-0005：依 WA-001、AEOS-ARCH-001、AEOS-DIA-001 與 AEOS-CON-001 建立 AEOS 之 Enterprise Governance Architecture。本文件為 Enterprise Governance 之正式 Architecture，不是 Governance Standard、Governance Policy 或 ADR。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-002 |
| 文件名稱 | Enterprise Governance Architecture |
| 型別 | Architecture |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0005、AR-AEOS-0005-R1、WA-001（Approved v1.0.0）、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0005、AR-AEOS-0005-R1、AEOS-ARCH-001、AEOS-DIA-001、AEOS-CON-001、WA-001 |

## 1. Governance Vision

本文件定義 AEOS 之 Enterprise Governance Architecture，其願景為：

- AI Engineering Workspace 之企業層級治理以正式 Architecture 承載：結構、領域、階層與關係一致，決策可追溯，演進可控。
- 治理架構使 Enterprise Architecture（以 WA-001 為唯一架構來源）、Platform Governance 與 Capability Management 於 AEOS 內以一致之方式落地與演進。
- 治理以正式文件為載體、以 Review 為強制機制、以 EWO 為變更單位。

本文件定義治理「如何組織」，不定義治理「內容」；政策、標準、規格與決策由下位文件承載。

## 2. Governance Scope

### 2.1 In Scope

本文件涵蓋：

- Enterprise Governance 之結構性定義：Governance Vision、Scope、Layers、Domains、Hierarchy、Relationships、Principles、Ownership、Lifecycle 與 Evolution。
- 治理標的之界定：Enterprise Architecture 維護、Platform Governance、Capability Management 與 Repository Governance 之架構關係。
- 治理文件體系之定位：治理架構與 AEOS-DIA-001 文件體系之關係。

### 2.2 Out of Scope

本文件明確不涵蓋：

- Governance Standard／Governance Policy 之內容（由後續 POL／STD／SPEC／GOV 文件承載）。
- 單一治理決策之記錄（由 ADR 承載）。
- Repository Governance 細則（Repository 身分、角色職責、變更規則；由 AEOS-CON-001 承載）。
- WA-001／AEOS-ARCH-001 已定義之架構內容之重述或修改。

## 3. Governance Layers

Enterprise Governance 分為四個層級：

| 層級 | 名稱 | 角色 | 權威文件 |
|------|------|------|----------|
| L1 | Foundation Governance | 定義 Repository 治理基線（身分、原則、所有權、變更管理） | AEOS-CON-001 |
| L2 | Governance Architecture | 定義治理之結構與關係 | AEOS-ARCH-002（本文件） |
| L3 | Governance Content | 承載治理內容（Policy、Standard、Specification、Governance 文件） | POL／STD／SPEC／GOV 文件 |
| L4 | Governance Decision | 記錄治理決策與變更依據 | ADR、Review 紀錄 |

規則：

- 每一層級之權威文件為該層內容之 Single Source。
- 下層文件引用上層文件，不重述上層內容。
- 治理內容（L3）之變更不改變治理架構（L2）；治理架構之變更需經 EWO 與 Review。

## 4. Governance Domains

Enterprise Governance 分為下列治理領域：

| 領域 | 治理範圍 | 架構來源 |
|------|----------|----------|
| Architecture Governance | 架構基線、架構文件與 Register | WA-001、AEOS-ARCH-001 |
| Documentation Governance | 文件體系、Taxonomy 與文件生命週期 | AEOS-DIA-001 |
| Platform Governance | 平台層級治理內容與決策 | WA-001（正式內容由後續 EWO 建立） |
| Capability Governance | 能力定義、擁有權與管理 | WA-001（正式內容由後續 EWO 建立） |
| Repository Governance | Repository 身分、所有權與變更 | AEOS-CON-001 |
| Decision Governance | ADR、Architecture Decision、Repository Decision、Review Decision 之記錄與管理 | ADR、Review Records（L4 Governance Decision） |

規則：

- 每一治理領域 MUST 有單一權威文件作為其治理依據。
- 領域間之關係依 §6 Governance Relationships 定義。
- 新增治理領域視為本文件之重大變更，需經 EWO 與 Review。

## 5. Governance Hierarchy

治理文件與決策依下列階層排序：

| 階層 | 文件類別 | 角色 |
|------|----------|------|
| H0 | WA-001（外部架構來源） | 最高架構權威 |
| H1 | AEOS-ARCH-001 | 架構 Entry Document |
| H2 | AEOS-CON-001 | Repository 治理基線 |
| H3 | Domain Architecture（AEOS-ARCH-002 等） | 治理／領域結構定義 |
| H4 | Governance Content（POL／STD／SPEC／GOV） | 治理內容 |
| H5 | Governance Decision | 治理決策（Decision Governance） |
| H6 | ADR | Architecture Decision 與 Repository Decision 之正式記錄 |
| H7 | Review Records | Review Decision 之正式記錄（Review 結果與 RC） |

規則：

- 階層愈高，權威愈高；下位文件 MUST NOT 違反上位文件。
- 內容衝突時依階層解析；架構內容以 WA-001／AEOS-ARCH-001 為準。
- 上位文件變更時，下位文件之引用須於同一或後續 EWO 對應更新，不得保留失效引用。

## 6. Governance Relationships

| 來源 | 關係 | 目標 | 用途 |
|------|------|------|------|
| WA-001 | 導入為唯一架構來源 | AEOS-ARCH-001 | 架構基線 |
| AEOS-ARCH-001 | 定義架構 Register 與引用樞紐 | 各 ARCH 文件 | 架構追溯 |
| AEOS-CON-001 | 定義 Repository 治理基線 | 治理文件 | 治理合規依據 |
| AEOS-DIA-001 | 定義文件分類與生命週期 | 治理文件 | 文件治理 |
| AEOS-ARCH-002（本文件） | 定義治理結構 | POL／STD／SPEC／GOV | 治理內容之結構邊界 |
| 治理內容文件 | 產生決策需求 | ADR／Review | 決策與變更記錄 |
| Review 決策 | 回饋修訂 | 治理內容文件 | 治理演進 |

規則：

- 治理關係以引用建立，MUST NOT 以內容複製建立。
- 每一治理文件 MUST 於 References 宣告其上位文件與來源 EWO。

## 7. Governance Principles

| # | 原則 | 說明 |
|---|------|------|
| GA-001 | Governance as Architecture | 治理以結構化 Architecture 定義，不以零散規則運作。 |
| GA-002 | Layered Authority | 治理權威依階層分層；各層擁有明確角色與邊界。 |
| GA-003 | Domain Separation | 治理領域彼此分離，各自擁有明確 Scope 與權威文件。 |
| GA-004 | Single Source of Governance | 每一治理主題僅存在一個權威文件；其他文件以引用取代重述。 |
| GA-005 | Review Enforced | 治理決策於 merge 前經正式 Review 強制執行（依 AEOS-CON-001 GP-009）。 |
| GA-006 | Traceable Decision | 治理決策可追溯至 EWO、來源文件與 Review。 |
| GA-007 | Incremental Evolution | 治理架構以 EWO 為單位增量演進，不做未經定義之全面變更。 |
| GA-008 | Backward Compatible Evolution | 治理架構演進保持向後相容；重大修訂需明確核准。 |
| GA-009 | Governance by Evidence | 治理決策 SHOULD 由可追溯之證據、已核准來源或正式 Review 結果支持。 |

Repository 層級之治理原則以 AEOS-CON-001 為準；本文件不重述。

## 8. Governance Ownership

| 治理資產 | Owner 角色 | 權威來源 |
|----------|------------|----------|
| Governance Architecture（本文件） | Architecture Owner | AEOS-DIA-001 §6 |
| Governance Content 文件 | Document Owner（frontmatter 宣告） | AEOS-DIA-001 §6 |
| Review 流程 | Review Owner | AEOS-DIA-001 §6、AEOS-CON-001 §9 |
| Repository 層級核准 | Repository Owner | AEOS-CON-001 §9 |
| 治理實作 | Engineering Contributor | AEOS-CON-001 §9 |

規則：

- Owner 角色之職責以 AEOS-DIA-001／AEOS-CON-001 為準；本文件不重述。
- 每份治理文件 MUST 於 frontmatter 宣告 owner。

## 9. Governance Lifecycle

治理資產依下列生命週期演進：

| 階段 | 輸入 | Gate | 輸出 |
|------|------|------|------|
| 1. Proposal | 治理需求 | EWO 定義範圍 | 核准之 EWO |
| 2. Definition | EWO | 依 AEOS-DIA-001 建立文件 | Draft 文件 |
| 3. Review | Draft 文件 | 正式 Review（CR） | Review 結果 |
| 4. Revision | Review 結果 | RC 修正 | Approved 文件 |
| 5. Release | Approved 文件 | 合併至 main | Released 文件 |
| 6. Evolution | 新 EWO | 變更管理 | 更新版本 |
| 7. Retirement | 不再有效之治理資產 | Deprecation／Archive 宣告 | Deprecated／Archived Governance Assets |

規則：

- 文件狀態依 AEOS-DIA-001 管理；本文件不重述。
- 治理決策（Review 結果與 RC）記錄於 ADR 或 Review 紀錄。
- 生命週期之每一階段 MUST 可追溯至前一階段之輸入。

## 10. Governance Evolution

- 治理架構之演進以 EWO 為單位，經 Review 後合併（依 AEOS-CON-001 §10）。
- 新增治理領域、層級或階層視為本文件之重大變更，需明確 Review。
- 治理內容（POL／STD／SPEC／GOV）在既有架構內演進，不需變更本文件；架構不足以涵蓋時，先修訂本文件再調整內容。
- 演進保持向後相容（GA-008）：既有 doc-id、路徑與引用不因演進失效。
- 每次變更 MUST 更新 References 與 Revision History。

## 11. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | AEOS 唯一架構來源 |
| REF-002 | AEOS-ARCH-001 — Architecture Baseline | Architecture Entry Document | 架構基線與 Register |
| REF-003 | AEOS-DIA-001 — Documentation Information Architecture | Information Architecture | 文件分類、Ownership 與生命週期 |
| REF-004 | AEOS-CON-001 — Repository Constitution（Approved v1.0.0） | Constitution | Repository 治理基線 |
| REF-005 | EWO-AEOS-0005 — Enterprise Governance Architecture | EWO | 本文件之工作來源 |

## 12. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | 依 Architecture Review（AR-AEOS-0005-R1）修正：狀態升版至 Approved 1.0.0；同步 AEOS-ARCH-001 §8 Architecture Register；Governance Domains 新增 Decision Governance（ADR、Architecture Decision、Repository Decision、Review Decision）；Governance Hierarchy 拆分 ADR（H6）與 Review Records（H7）；Governance Lifecycle 新增 Retirement；新增 GA-009 Governance by Evidence | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 Enterprise Governance 之 Vision、Scope、Layers、Domains、Hierarchy、Relationships、Principles、Ownership、Lifecycle 與 Evolution（EWO-AEOS-0005） | Codex |
