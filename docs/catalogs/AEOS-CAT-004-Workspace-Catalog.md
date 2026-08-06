---
doc-id: AEOS-CAT-004
doc-name: Workspace Catalog
doc-type: Catalog
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0030
  - AEOS-STD-006
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-010
  - WA-001
---

# AEOS-CAT-004 — Workspace Catalog

> EWO-AEOS-0030：依 AEOS-STD-006 與 AEOS-ARCH-010 建立 Workspace Catalog，登錄已核准 Workspace 之權威事實。本文件為 AI Engineering Workspace 已核准 Workspace 之唯一登錄來源；不創造架構事實。

## Executive Summary

本文件建立 AEOS Workspace Catalog，依 AEOS-STD-006 之 Catalog Schema、Entry ID、Lifecycle、Traceability 與一致性規則，登錄可追溯至 Approved Architecture 或正式決策之具名 Workspace。本版登錄具名條目為 **1**：WS-001 — AI Engineering Workspace（Enterprise Workspace）。AEOS-ARCH-010 §7.1 明示 WA-001 已核准之 Workspace 類型為 Enterprise Workspace（AI Engineering Workspace），為目前唯一已核准類型；故僅登錄該具名實例。Workspace Entry ID 前綴依 AEOS-STD-006 §6.1 為 `WS`（`WS-###`）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-CAT-004 |
| 文件名稱 | Workspace Catalog |
| 型別 | Catalog |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0030、AEOS-STD-006（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-ARCH-010（Approved 1.0.0）、AEOS-ARCH-001（Approved 1.0.0）、WA-001（Approved v1.0.0） |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-004、AEOS-ARCH-010、AEOS-STD-001～AEOS-STD-006、WA-001 |

## 1. Purpose

本文件之目的為：

- 建立已核准 Workspace 之權威登錄來源（AEOS-ARCH-004 §6.6、AEOS-ARCH-010 §5）。
- 依 AEOS-STD-006 統一 Schema 登錄 Workspace 之架構事實，僅登錄可追溯至 Approved Architecture 或正式決策之事實。
- 提供 Workspace 身分、Purpose、Boundary、Type／Level、Owner 與生命週期之可稽核紀錄。
- 為後續 Workspace Composition、Ownership Matrix 與 Dependency Matrix 提供 Workspace Entry 身分依據。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Workspace Catalog 之權威角色、Entry Schema 與登錄規則。
- Workspace Entry ID（`WS-###`）之使用與管理。
- Workspace 條目之 Lifecycle、Traceability、Change 與一致性規則。
- 已核准 Workspace 之登錄事實。

### 2.2 Out of Scope

本文件不涵蓋：

- 建立 Ownership Matrix、Dependency Matrix 或其他 Catalog／Matrix。
- 新增或推測 Workspace Ownership、Repository Placement、Platform／Capability Mapping、Dependency 或跨 Workspace Relationship。
- 未經核准之具名 Workspace 條目或候選清單。
- 將 Workspace 類型、抽象分類、Layer、責任描述、邏輯邊界、資料夾、Repository、執行環境或範例視為具名 Workspace。
- 僅因某個 Repository、資料夾或開發環境實際存在而自動登錄。
- 個別 Workspace 之內部技術架構、部署拓撲或實作設計。

## 3. Catalog Authority and Compliance

Workspace Catalog 依下列權威順序運作：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| W0 | WA-001 | Workspace Architecture 之唯一來源 |
| W1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline |
| W2 | AEOS-ARCH-004 | Workspace Architecture 於 Enterprise Architecture 之定位 |
| W3 | AEOS-ARCH-010 | Workspace 身分、Purpose、Boundary、Type／Level 與登錄規則 |
| W4 | AEOS-STD-006 | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| W5 | AEOS-CAT-004（本文件） | 已核准 Workspace 之權威登錄 |

規則：

- 本文件之型別為 Catalog（CAT），置於 `docs/catalogs/`，doc-id 為 `AEOS-CAT-004`（AEOS-STD-006 §4）。
- 條目 Schema 依 AEOS-STD-006 §5 統一定義；本文件僅套用，不重複設計。
- Workspace Entry ID MUST 使用 `WS-###`（AEOS-STD-006 §6.1），唯一、穩定且不可重用。
- Workspace Architecture 之正式文件 ID 為 AEOS-ARCH-010（Approved 1.0.0），本文件依其內容運作。

## 4. Entry Schema

Workspace Entry MUST 具備下列欄位（依 AEOS-STD-006 §5.1 與 AEOS-ARCH-010 §5）：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Entry ID | MUST | `WS-###`；全 Workspace 唯一、穩定且不可重用 |
| Entry Name | MUST | 正式名稱；命名變更不得改變 Entry ID |
| Type／Classification | MUST | 依 AEOS-ARCH-010 §7 類型 |
| Status | MUST | 依 §6 條目 Lifecycle 狀態 |
| Owner | MUST | accountable Workspace Owner |
| Architecture Reference | MUST | 核准此 Workspace 身分與邊界之 Approved Architecture／ADR |
| Validated Facts | MUST | Purpose／Mission、Boundary、Composition（AEOS-ARCH-010 §5、§6） |
| Related Entries | MUST | 引用其他 Catalog Entry ID；無合規引用時為空（AEOS-STD-006 §5.1「無則為空」） |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 條目新增、修改、移除之歷程 |

規則：

- 條目之 Validated Facts MUST 可追溯至 Architecture Reference；無參考之條目 MUST NOT 登錄（AEOS-STD-006 §7）。
- Workspace 類型（Enterprise Workspace 等）為抽象分類，不得直接視為具名 Workspace；條目以具名實例登錄。
- 資料夾、Repository、執行環境或範例，於缺少正式架構或決策依據時 MUST NOT 自動登錄。

## 5. Registered Entries

本版登錄具名 Workspace 條目：**1**。

### 5.1 WS-001 — AI Engineering Workspace

| 欄位 | 內容 |
|------|------|
| Entry ID | WS-001 |
| Entry Name | AI Engineering Workspace |
| Type／Classification | Enterprise Workspace（AEOS-ARCH-010 §7.1） |
| Status | Active |
| Owner | Workspace Owner（角色，依 AEOS-ARCH-010 §9.1；具名 incumbent 未核准） |
| Architecture Reference | [AEOS-ARCH-010](../architecture/AEOS-ARCH-010-Workspace-Architecture.md)（Approved 1.0.0）§4.1、§5、§7.1；[AEOS-ARCH-001](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0）§1；[AEOS-ARCH-004](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0）§4；WA-001（Approved v1.0.0） |
| Validated Facts | 定位：AI Engineering Workspace 之正式企業架構邊界，統合 Platform、Capability、Repository、Dependency 與治理資產（AEOS-ARCH-010 §4.1、§6.2）；Type：Enterprise Workspace（AEOS-ARCH-010 §7.1）；Composition：具名組成元素尚未核准登錄（本 EWO 不推測 Repository Placement／Platform-Capability Mapping） |
| Related Entries | 無（無合規引用；AEOS-STD-006 §5.1 空值表達） |
| Version／Review Date | 1.0.0／2026-08-06 |
| Change Record | 2026-08-06：新增登錄（EWO-AEOS-0030，Draft） |

### 5.2 登錄結論與排除說明

- WS-001 之正式依據：AEOS-ARCH-010 §7.1 明示「WA-001 已核准之 Workspace 類型為 Enterprise Workspace（AI Engineering Workspace），為目前唯一已核准類型」；AEOS-ARCH-001 §1 與 AEOS-ARCH-004 §4 亦以「AI Engineering Workspace」為 Workspace 之正式名稱。
- 不登錄其他 Workspace：AEOS-ARCH-010 §7.1 明示其他 Workspace 類型 MUST 先經 WA-001 變更與 Architecture Review 核准；目前無其他具名 Workspace。
- 不登錄資料夾、Repository、執行環境或開發環境：僅實際存在不構成正式架構或決策依據。
- 後續具名 Workspace MUST 經正式 Architecture Review／WA-001 內容核准後，依 §7 登錄。

## 6. Document Lifecycle vs Entry Lifecycle

本文件明確區分兩種生命週期：

| 生命週期 | 對象 | 狀態 |
|----------|------|------|
| 文件 Lifecycle | AEOS-CAT-004 本文件 | Draft → Approved → Deprecated → Retired（AEOS-STD-006 §9.1） |
| 條目 Lifecycle | 個別 Workspace Entry | Candidate → Active → Deprecated → Retired（AEOS-STD-006 §9.2） |

規則：

- 文件狀態與條目狀態 MUST 分開管理；文件 Approved 不代表其中條目全部 Active（AEOS-STD-006 §9.3）。
- 本文件現為 Draft（文件 Lifecycle），不影響已登錄條目之既有事實。

## 7. Traceability and Change Rules

### 7.1 Traceability

- 每個 Workspace Entry MUST 宣告 Architecture Reference；無參考即不得登錄（AEOS-STD-006 §7）。
- 條目事實與 Architecture 衝突時，以 Architecture 為準並啟動修正。

### 7.2 Change Rules

- 條目新增（Candidate → Active）：MUST 具 Architecture Reference 並經 Review（AEOS-STD-006 §10.2）。
- 條目修改：變更 Identity、Purpose、Boundary、Type／Level 或 Owner 視為重大變更，MUST 經 Review。
- 條目移除：MUST 依 Deprecated → Retired 流程執行，不得直接刪除。
- 本 EWO 不建立或推測 Workspace Ownership、Repository Placement、Platform／Capability Mapping、Dependency 或跨 Workspace Relationship。

## 8. Consistency

Workspace Catalog MUST 符合 AEOS-STD-006 §11 一致性規則：

- 無重複 Workspace Entry ID。
- 無孤兒引用：Composition 引用之元素存在於對應 Catalog（登錄時驗證；本版為空）。
- 與 AEOS-ARCH-001 §8 Register 及相關 Architecture 之狀態一致。
- 變更後 MUST 重新執行格式、Metadata、Cross-reference、Placeholder 與一致性驗證。

## 9. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Workspace Architecture 之唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 架構基線；定義 AI Engineering Workspace |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | Workspace Architecture 之定位 |
| REF-004 | [AEOS-ARCH-010 — Workspace Architecture](../architecture/AEOS-ARCH-010-Workspace-Architecture.md)（Approved 1.0.0） | Architecture | Workspace 身分、Purpose、Boundary、Type／Level 與登錄規則 |
| REF-005 | [AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard](../standards/AEOS-STD-006-Enterprise-Architecture-Catalog-and-Matrix-Standard.md)（Approved 1.0.0） | Standard | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| REF-006 | EWO-AEOS-0030 | EWO | 本文件之工作來源 |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立：依 AEOS-STD-006 建立 Workspace Catalog 權威結構、Entry Schema、Lifecycle、Traceability、Change 與一致性規則；登錄 WS-001 AI Engineering Workspace（Enterprise Workspace，依 AEOS-ARCH-010 §7.1）；Entry ID 前綴依 STD-006 §6.1 使用 WS（EWO-AEOS-0030） | Codex |
