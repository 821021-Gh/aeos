---
doc-id: AEOS-CAT-003
doc-name: Repository Catalog
doc-type: Catalog
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0028
  - AEOS-STD-006
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-008
  - AEOS-CON-001
  - WA-001
---

# AEOS-CAT-003 — Repository Catalog

> EWO-AEOS-0028：依 AEOS-STD-006 與 AEOS-ARCH-008 建立 Repository Catalog，登錄已核准 Repository 之權威事實。本文件為 AI Engineering Workspace 已核准 Repository 之唯一登錄來源；不創造架構事實。

## Executive Summary

本文件建立 AEOS Repository Catalog，依 AEOS-STD-006 之 Catalog Schema、Entry ID、Lifecycle、Traceability 與一致性規則，登錄可追溯至 Approved Architecture 或正式決策之具名 Repository。本版登錄具名條目為 **1**：REP-001 — AEOS（Enterprise Root Repository），其身分由 AEOS-CON-001 §2 正式定義，並由 AEOS-ARCH-001 §1 明示為 AI Engineering Workspace 之 Enterprise Root Repository。其他 Repository（如 YEOS）僅於文件中被引用為工作流程來源，未於 Approved Architecture 中定義正式身分；僅實際存在於 GitHub 不構成登錄依據，故不登錄。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-CAT-003 |
| 文件名稱 | Repository Catalog |
| 型別 | Catalog |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0028、AEOS-STD-006（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-ARCH-008（Approved 1.0.0）、AEOS-ARCH-001（Approved 1.0.0）、AEOS-CON-001（Approved 1.0.0）、WA-001（Approved v1.0.0） |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-004、AEOS-ARCH-008、AEOS-CON-001、AEOS-STD-001～AEOS-STD-006、WA-001 |

## 1. Purpose

本文件之目的為：

- 建立已核准 Repository 之權威登錄來源（AEOS-ARCH-004 §6.4、AEOS-ARCH-008 §5）。
- 依 AEOS-STD-006 統一 Schema 登錄 Repository 之架構事實，僅登錄可追溯至 Approved Architecture 或正式決策之事實。
- 提供 Repository 身分、Type、Boundary、Owner 與生命週期之可稽核紀錄。
- 為後續 Workspace Catalog、Ownership Matrix 與 Dependency Matrix 提供 Repository Entry 身分依據。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Repository Catalog 之權威角色、Entry Schema 與登錄規則。
- Repository Entry ID（`REP-###`）之使用與管理。
- Repository 條目之 Lifecycle、Traceability、Change 與一致性規則。
- 已核准 Repository 之登錄事實。

### 2.2 Out of Scope

本文件不涵蓋：

- 建立 Workspace Catalog、Ownership Matrix、Dependency Matrix 或其他 Catalog／Matrix。
- 新增或推測 Repository Ownership、Dependency、Workspace Placement 或跨 Repository Relationship。
- 未經核准之具名 Repository 條目或候選清單。
- 將 Repository 類型、抽象分類、Layer、責任描述、邏輯邊界或範例視為具名 Repository。
- 僅因 Repository 實際存在於 GitHub 而自動登錄。
- 個別 Repository 之內部技術架構、部署拓撲或實作設計。

## 3. Catalog Authority and Compliance

Repository Catalog 依下列權威順序運作：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| R0 | WA-001 | Workspace Architecture 與 Repository 角色之唯一來源 |
| R1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline；定義 AEOS 為 Enterprise Root Repository |
| R2 | AEOS-ARCH-004 | Repository Architecture 於 Enterprise Architecture 之定位 |
| R3 | AEOS-ARCH-008 | Repository 身分、Type、Boundary 與登錄規則 |
| R4 | AEOS-CON-001 | AEOS Repository 正式身分、使命與治理基線 |
| R5 | AEOS-STD-006 | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| R6 | AEOS-CAT-003（本文件） | 已核准 Repository 之權威登錄 |

規則：

- 本文件之型別為 Catalog（CAT），置於 `docs/catalogs/`，doc-id 為 `AEOS-CAT-003`（AEOS-STD-006 §4）。
- 條目 Schema 依 AEOS-STD-006 §5 統一定義；本文件僅套用，不重複設計。
- Repository Entry ID MUST 使用 `REP-###`（AEOS-STD-006 §6.1），唯一、穩定且不可重用。

## 4. Entry Schema

Repository Entry MUST 具備下列欄位（依 AEOS-STD-006 §5.1 與 AEOS-ARCH-008 §5）：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Entry ID | MUST | `REP-###`；全 Workspace 唯一、穩定且不可重用 |
| Entry Name | MUST | 正式名稱；命名變更不得改變 Entry ID |
| Type／Classification | MUST | 依 AEOS-ARCH-008 §7 類型 |
| Status | MUST | 依 §6 條目 Lifecycle 狀態 |
| Owner | MUST | accountable Repository Owner |
| Architecture Reference | MUST | 核准此 Repository 身分與邊界之 Approved Architecture／ADR |
| Validated Facts | MUST | Boundary、Authority（AEOS-ARCH-008 §5、§6）；Platform Reference、Capability References、Dependencies（AEOS-ARCH-008 §5） |
| Related Entries | MUST | 引用其他 Catalog Entry ID；無合規引用時為空（AEOS-STD-006 §5.1「無則為空」） |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 條目新增、修改、移除之歷程 |

規則：

- 條目之 Validated Facts MUST 可追溯至 Architecture Reference；無參考之條目 MUST NOT 登錄（AEOS-STD-006 §7）。
- Repository 類型（Enterprise Root Repository 等）為抽象分類，不得直接視為具名 Repository；條目以具名身分登錄。
- 僅實際存在於 GitHub 之 Repository，於缺少正式架構或決策依據時 MUST NOT 自動登錄。

## 5. Registered Entries

本版登錄具名 Repository 條目：**1**。

### 5.1 REP-001 — AEOS（AI Enterprise Operating System）

| 欄位 | 內容 |
|------|------|
| Entry ID | REP-001 |
| Entry Name | AEOS（AI Enterprise Operating System） |
| Type／Classification | Enterprise Root Repository（AEOS-ARCH-004 §6.4、AEOS-ARCH-008 §7） |
| Status | Active |
| Owner | Repository Owner（AEOS-CON-001 owner；AEOS-ARCH-004 §9） |
| Architecture Reference | [AEOS-ARCH-001](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0）§1；[AEOS-CON-001](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved 1.0.0）§1、§2；[AEOS-ARCH-004](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0）§6.4 |
| Validated Facts | 定位：AI Engineering Workspace 之 Enterprise Root Repository，承載 Workspace 層級之企業架構、治理與共同控制資產（AEOS-CON-001 §1、AEOS-ARCH-001 §1）；Platform Reference：無（尚未核准具名 Platform）；Capability References：無（尚未核准具名 Capability）；Dependencies：無（尚未核准，不推測） |
| Related Entries | 無（無合規引用；AEOS-STD-006 §5.1 空值表達） |
| Version／Review Date | 1.0.0／2026-08-06 |
| Change Record | 2026-08-06：新增登錄（EWO-AEOS-0028，Draft） |

### 5.2 登錄結論與排除說明

- REP-001 之正式依據：AEOS-CON-001 §2 定義 Repository Identity（名稱：AEOS）；AEOS-ARCH-001 §1 明示「AEOS 為 AI Engineering Workspace 之 Enterprise Root Repository」；AEOS-CON-001 §1、§2 定義使命與治理基線。
- 不登錄 YEOS：AEOS 文件中僅將其引用為 Engineering Workflow 來源（CONTRIBUTING），未於 Approved Architecture 中定義其 Repository 身分、型別或邊界。
- 不登錄其他 Repository：僅實際存在於 GitHub 不構成正式架構或決策依據。
- 後續具名 Repository MUST 經正式 Architecture Review／正式決策核准後，依 §7 登錄。

## 6. Document Lifecycle vs Entry Lifecycle

本文件明確區分兩種生命週期：

| 生命週期 | 對象 | 狀態 |
|----------|------|------|
| 文件 Lifecycle | AEOS-CAT-003 本文件 | Draft → Approved → Deprecated → Retired（AEOS-STD-006 §9.1） |
| 條目 Lifecycle | 個別 Repository Entry | Candidate → Active → Deprecated → Retired（AEOS-STD-006 §9.2） |

規則：

- 文件狀態與條目狀態 MUST 分開管理；文件 Approved 不代表其中條目全部 Active（AEOS-STD-006 §9.3）。
- 本文件現為 Draft（文件 Lifecycle），不影響已登錄條目之既有事實。

## 7. Traceability and Change Rules

### 7.1 Traceability

- 每個 Repository Entry MUST 宣告 Architecture Reference；無參考即不得登錄（AEOS-STD-006 §7）。
- 條目事實與 Architecture 衝突時，以 Architecture 為準並啟動修正。

### 7.2 Change Rules

- 條目新增（Candidate → Active）：MUST 具 Architecture Reference 並經 Review（AEOS-STD-006 §10.2）。
- 條目修改：變更 Identity、Type、Boundary、Authority 或 Owner 視為重大變更，MUST 經 Review。
- 條目移除：MUST 依 Deprecated → Retired 流程執行，不得直接刪除。
- 本 EWO 不建立或推測 Repository Ownership、Dependency、Workspace Placement 或跨 Repository Relationship。

## 8. Consistency

Repository Catalog MUST 符合 AEOS-STD-006 §11 一致性規則：

- 無重複 Repository Entry ID。
- 無孤兒引用：Validated Facts 中引用之 Platform／Capability 條目存在於對應 Catalog（登錄時驗證；本版為空）。
- 與 AEOS-ARCH-001 §8 Register 及相關 Architecture 之狀態一致。
- 變更後 MUST 重新執行格式、Metadata、Cross-reference、Placeholder 與一致性驗證。

## 9. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Workspace Architecture 與 Repository 角色之唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 架構基線；定義 AEOS 為 Enterprise Root Repository |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | Repository Architecture 之定位 |
| REF-004 | [AEOS-ARCH-008 — Repository Architecture](../architecture/AEOS-ARCH-008-Repository-Architecture.md)（Approved 1.0.0） | Architecture | Repository 身分、Type、Boundary 與登錄規則 |
| REF-005 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved 1.0.0） | Constitution | AEOS Repository 正式身分與治理基線 |
| REF-006 | [AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard](../standards/AEOS-STD-006-Enterprise-Architecture-Catalog-and-Matrix-Standard.md)（Approved 1.0.0） | Standard | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| REF-007 | EWO-AEOS-0028 | EWO | 本文件之工作來源 |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立：依 AEOS-STD-006 建立 Repository Catalog 權威結構、Entry Schema、Lifecycle、Traceability、Change 與一致性規則；登錄 REP-001 AEOS（Enterprise Root Repository，依 AEOS-CON-001 §2 與 AEOS-ARCH-001 §1）；其他 Repository 因缺乏正式依據不登錄（EWO-AEOS-0028） | Codex |
