---
doc-id: AEOS-CAT-002
doc-name: Capability Catalog
doc-type: Catalog
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0026
  - AEOS-STD-006
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-007
  - WA-001
---

# AEOS-CAT-002 — Capability Catalog

> EWO-AEOS-0026：依 AEOS-STD-006 與 AEOS-ARCH-007 建立 Capability Catalog，登錄已核准 Capability 之權威事實。本文件為 AI Engineering Workspace 已核准 Capability 之唯一登錄來源；不創造架構事實。

## Executive Summary

本文件建立 AEOS Capability Catalog，依 AEOS-STD-006 之 Catalog Schema、Entry ID、Lifecycle、Traceability 與一致性規則，登錄可追溯至 Approved Architecture 或正式決策之具名 Capability。本版登錄具名條目為 **0**：現行 Approved Architecture（AEOS-ARCH-004、AEOS-ARCH-007）未核准任何具名 Capability；治理領域（如 Capability Governance）與架構領域（如 Layer、Domain）屬抽象分類，依規範不得直接視為具名 Capability。本文件建立權威結構、Entry Schema 與治理規則，供後續正式核准之 Capability 登錄。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-CAT-002 |
| 文件名稱 | Capability Catalog |
| 型別 | Catalog |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0026、AEOS-STD-006（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-ARCH-007（Approved 1.0.0）、AEOS-ARCH-001、WA-001（Approved v1.0.0） |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-004、AEOS-ARCH-007、AEOS-STD-001～AEOS-STD-006、WA-001 |

## 1. Purpose

本文件之目的為：

- 建立已核准 Capability 之權威登錄來源（AEOS-ARCH-004 §6.3、AEOS-ARCH-007 §5）。
- 依 AEOS-STD-006 統一 Schema 登錄 Capability 之架構事實，僅登錄可追溯至 Approved Architecture 或正式決策之事實。
- 提供 Capability 身分、Outcome、邊界、分類、Owner 與生命週期之可稽核紀錄。
- 為後續 Ownership Matrix、Dependency Matrix 與 Workspace Catalog 提供 Capability Entry 身分依據。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Capability Catalog 之權威角色、Entry Schema 與登錄規則。
- Capability Entry ID（`CPB-###`）之使用與管理。
- Capability 條目之 Lifecycle、Traceability、Change 與一致性規則。
- 已核准 Capability 之登錄事實。

### 2.2 Out of Scope

本文件不涵蓋：

- 建立 Repository Catalog、Workspace Catalog、Ownership Matrix 或 Dependency Matrix。
- 新增或推測 Capability Ownership、Dependency 或跨 Capability Relationship。
- 未經核准之具名 Capability 條目或候選清單。
- 將抽象分類、Layer、Domain、責任描述或候選概念視為具名 Capability。
- 個別 Capability 之實作細節、部署拓撲或工具配置。

## 3. Catalog Authority and Compliance

Capability Catalog 依下列權威順序運作：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| C0 | WA-001 | Capability Architecture 與 Capability Ownership 之唯一來源 |
| C1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline |
| C2 | AEOS-ARCH-004 | Capability Architecture 於 Enterprise Architecture 之定位 |
| C3 | AEOS-ARCH-007 | Capability 身分、Outcome、邊界、分類與登錄規則 |
| C4 | AEOS-STD-006 | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| C5 | AEOS-CAT-002（本文件） | 已核准 Capability 之權威登錄 |

規則：

- 本文件之型別為 Catalog（CAT），置於 `docs/catalogs/`，doc-id 為 `AEOS-CAT-002`（AEOS-STD-006 §4）。
- 條目 Schema 依 AEOS-STD-006 §5 統一定義；本文件僅套用，不重複設計。
- Capability Entry ID MUST 使用 `CPB-###`（AEOS-STD-006 §6.1），唯一、穩定且不可重用。

## 4. Entry Schema

Capability Entry MUST 具備下列欄位（依 AEOS-STD-006 §5.1 與 AEOS-ARCH-007 §5）：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Entry ID | MUST | `CPB-###`；全 Workspace 唯一、穩定且不可重用 |
| Entry Name | MUST | 正式名稱；命名變更不得改變 Entry ID |
| Type／Classification | MUST | 依 AEOS-ARCH-007 §7 分類維度 |
| Status | MUST | 依 §6 條目 Lifecycle 狀態 |
| Owner | MUST | accountable Capability Owner |
| Architecture Reference | MUST | 核准此 Capability 身分與邊界之 Approved Architecture／ADR |
| Validated Facts | MUST | Outcome、Boundary（AEOS-ARCH-007 §5、§6）；Platform Reference、Repository References（AEOS-ARCH-007 §5） |
| Related Entries | MUST | 引用其他 Catalog Entry ID；無合規引用時為空（AEOS-STD-006 §5.1「無則為空」） |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 條目新增、修改、移除之歷程 |

規則：

- 條目之 Validated Facts MUST 可追溯至 Architecture Reference；無參考之條目 MUST NOT 登錄（AEOS-STD-006 §7）。
- Related Entries 僅能引用已存在且可解析之正式 Entry ID；本版無合規引用，故為空。
- 抽象分類、Layer、Domain、責任描述與候選概念 MUST NOT 登錄為具名 Capability。

## 5. Registered Entries

本版登錄具名 Capability 條目：**0**。

登錄結論與理由：

- 現行 Approved Architecture（AEOS-ARCH-004、AEOS-ARCH-007）未核准任何具名 Capability；AEOS-ARCH-007 明示「具名 Capability 條目與 Capability Catalog 之建立（Catalog 應作為後續獨立架構資產）」不屬該 EWO。
- 「Platform Governance」「Capability Governance」等為治理領域（AEOS-GOV-001 §3、AEOS-ARCH-002 §4），屬抽象分類，依要求不得視為具名 Capability。
- 依 AEOS-STD-006 §3.2 Fact Authority，Catalog MUST NOT 自行創造 Capability；故本版不登錄任何條目，亦不為追求數量新增候選條目。
- 後續具名 Capability MUST 經正式 Architecture Review／WA-001 內容核准後，依 §7 登錄。

## 6. Document Lifecycle vs Entry Lifecycle

本文件明確區分兩種生命週期：

| 生命週期 | 對象 | 狀態 |
|----------|------|------|
| 文件 Lifecycle | AEOS-CAT-002 本文件 | Draft → Approved → Deprecated → Retired（AEOS-STD-006 §9.1） |
| 條目 Lifecycle | 個別 Capability Entry | Candidate → Active → Deprecated → Retired（AEOS-STD-006 §9.2） |

規則：

- 文件狀態與條目狀態 MUST 分開管理；文件 Approved 不代表其中條目全部 Active（AEOS-STD-006 §9.3）。
- 本文件現為 Draft（文件 Lifecycle），不影響日後已核准條目之登錄程序。

## 7. Traceability and Change Rules

### 7.1 Traceability

- 每個 Capability Entry MUST 宣告 Architecture Reference；無參考即不得登錄（AEOS-STD-006 §7）。
- 條目事實與 Architecture 衝突時，以 Architecture 為準並啟動修正。

### 7.2 Change Rules

- 條目新增（Candidate → Active）：MUST 具 Architecture Reference 並經 Review（AEOS-STD-006 §10.2）。
- 條目修改：變更 Identity、Outcome、Boundary、Classification 或 Owner 視為重大變更，MUST 經 Review。
- 條目移除：MUST 依 Deprecated → Retired 流程執行，不得直接刪除。
- 本 EWO 不建立或推測 Capability Ownership、Dependency 或跨 Capability Relationship。

## 8. Consistency

Capability Catalog MUST 符合 AEOS-STD-006 §11 一致性規則：

- 無重複 Capability Entry ID。
- 無孤兒引用：Validated Facts 中引用之 Platform／Repository 條目存在於對應 Catalog（登錄時驗證）。
- 與 AEOS-ARCH-001 §8 Register 及相關 Architecture 之狀態一致。
- 變更後 MUST 重新執行格式、Metadata、Cross-reference、Placeholder 與一致性驗證。

## 9. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Capability Architecture 與 Capability Ownership 之唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 架構基線與 Register |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | Capability Architecture 之定位 |
| REF-004 | [AEOS-ARCH-007 — Capability Architecture](../architecture/AEOS-ARCH-007-Capability-Architecture.md)（Approved 1.0.0） | Architecture | Capability 身分、Outcome、邊界、分類與登錄規則 |
| REF-005 | [AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard](../standards/AEOS-STD-006-Enterprise-Architecture-Catalog-and-Matrix-Standard.md)（Approved 1.0.0） | Standard | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| REF-006 | EWO-AEOS-0026 | EWO | 本文件之工作來源 |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立：依 AEOS-STD-006 建立 Capability Catalog 權威結構、Entry Schema、Lifecycle、Traceability、Change 與一致性規則；本版登錄具名 Capability 條目為 0（現行 Approved Architecture 未核准任何具名 Capability；治理／架構領域屬抽象分類不登錄）（EWO-AEOS-0026） | Codex |
