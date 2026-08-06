---
doc-id: AEOS-CAT-001
doc-name: Platform Catalog
doc-type: Catalog
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0024
  - EWO-AEOS-0025
  - CM-AEOS-0025-R1
  - AEOS-STD-006
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - WA-001
---

# AEOS-CAT-001 — Platform Catalog

> EWO-AEOS-0024：依 AEOS-STD-006 與 AEOS-ARCH-005 建立 Platform Catalog，登錄已核准 Platform 之權威事實。本文件為 AI Engineering Workspace 已核准 Platform 之唯一登錄來源；不創造架構事實。

## Executive Summary

本文件建立 AEOS Platform Catalog，依 AEOS-STD-006 之 Catalog Schema、Entry ID、Lifecycle、Traceability 與一致性規則，登錄可追溯至 Approved Architecture 或正式決策之具名 Platform。本版登錄具名條目為 **0**：現行 Approved Architecture（AEOS-ARCH-004、AEOS-ARCH-005）未核准任何具名 Platform，WA-001 之 Platform Topology 為外部權威來源且其具名清單未於 AEOS 發布；依 Fact Authority 原則，Catalog 不得臆造 Platform，故不為數量新增候選條目。本文件建立權威結構、Entry Schema 與治理規則，供後續正式核准之 Platform 登錄。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-CAT-001 |
| 文件名稱 | Platform Catalog |
| 型別 | Catalog |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0024、EWO-AEOS-0025、CM-AEOS-0025-R1、AEOS-STD-006（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-ARCH-005（Approved 1.0.0）、AEOS-ARCH-001、WA-001（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0025、CM-AEOS-0025-R1、AEOS-ARCH-001、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-STD-001～AEOS-STD-006、WA-001 |

## 1. Purpose

本文件之目的為：

- 建立已核准 Platform 之權威登錄來源（AEOS-ARCH-005 §11）。
- 依 AEOS-STD-006 統一 Schema 登錄 Platform 之架構事實，僅登錄可追溯至 Approved Architecture 或正式決策之事實。
- 提供 Platform 身分、邊界、分類、Owner 與生命週期之可稽核紀錄。
- 為後續 Ownership Matrix、Dependency Matrix 與 Workspace Catalog 提供 Platform Entry 身分依據。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Platform Catalog 之權威角色、Entry Schema 與登錄規則。
- Platform Entry ID（`PLT-###`）之使用與管理。
- Platform 條目之 Lifecycle、Traceability、Change 與一致性規則。
- 已核准 Platform 之登錄事實。

### 2.2 Out of Scope

本文件不涵蓋：

- 建立 Capability Catalog、Repository Catalog、Workspace Catalog、Ownership Matrix 或 Dependency Matrix。
- 新增 Ownership 或 Dependency Relationship。
- 未經核准之具名 Platform 條目或候選清單。
- 個別 Platform 之內部技術架構、部署拓撲或實作設計。
- 工具配置、Runtime Topology 或 Source Code Implementation。

## 3. Catalog Authority and Compliance

Platform Catalog 依下列權威順序運作：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| P0 | WA-001 | Platform Topology 之唯一來源 |
| P1 | AEOS-ARCH-001 | 將 WA-001 納入 AEOS Architecture Baseline |
| P2 | AEOS-ARCH-004 | Platform Architecture 於 Enterprise Architecture 之定位 |
| P3 | AEOS-ARCH-005 | Platform 身分、邊界、分類與登錄規則 |
| P4 | AEOS-STD-006 | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| P5 | AEOS-CAT-001（本文件） | 已核准 Platform 之權威登錄 |

規則：

- 本文件之型別為 Catalog（CAT），置於 `docs/catalogs/`，doc-id 為 `AEOS-CAT-001`（AEOS-STD-006 §4）。
- 條目 Schema 依 AEOS-STD-006 §5 統一定義；本文件僅套用，不重複設計。
- Platform Entry ID MUST 使用 `PLT-###`（AEOS-STD-006 §6.1），唯一、穩定且不可重用。

## 4. Entry Schema

Platform Entry MUST 具備下列欄位（依 AEOS-STD-006 §5.1 與 AEOS-ARCH-005 §5、§11.2）：

| 欄位 | 必要性 | 定義 |
|------|--------|------|
| Entry ID | MUST | `PLT-###`；全 Workspace 唯一、穩定且不可重用 |
| Entry Name | MUST | 正式名稱；命名變更不得改變 Entry ID |
| Type／Classification | MUST | 依 AEOS-ARCH-005 §7 分類（Enterprise／Engineering／Business／Product／Shared Service） |
| Status | MUST | 依 §6 條目 Lifecycle 狀態 |
| Owner | MUST | accountable Platform Owner |
| Architecture Reference | MUST | 核准此 Platform 身分與邊界之 Approved Architecture／ADR |
| Validated Facts | MUST | Mission、Boundary、Authority（AEOS-ARCH-005 §5、§6）；Capability References、Repository References、Interface／Dependency References（AEOS-ARCH-005 §11.2） |
| Related Entries | MUST | 引用其他 Catalog Entry ID；本版無 |
| Version／Review Date | MUST | 條目版本與最近 Review 日期 |
| Change Record | MUST | 條目新增、修改、移除之歷程 |

規則：

- 條目之 Validated Facts MUST 可追溯至 Architecture Reference；無參考之條目 MUST NOT 登錄（AEOS-STD-006 §7）。
- 條目欄位不得以自由文字取代可解析之引用（AEOS-STD-006 §8）。

## 5. Registered Entries

本版登錄具名 Platform 條目：**0**。

登錄結論與理由：

- 現行 Approved Architecture（AEOS-ARCH-004、AEOS-ARCH-005）未核准任何具名 Platform；AEOS-ARCH-005 明示「不在未經核准的情況下新增具名 Platform 條目」。
- WA-001 之 Platform Topology 為外部權威來源；其具名 Platform 清單未於 AEOS 內發布或經 Architecture Review 核准。
- 依 AEOS-STD-006 §3.2 Fact Authority，Catalog MUST NOT 自行創造 Platform；故本版不登錄任何條目，亦不為追求數量新增候選條目。
- 後續具名 Platform MUST 經正式 Architecture Review／WA-001 內容核准後，依 §7 登錄。

## 6. Document Lifecycle vs Entry Lifecycle

本文件明確區分兩種生命週期：

| 生命週期 | 對象 | 狀態 |
|----------|------|------|
| 文件 Lifecycle | AEOS-CAT-001 本文件 | Draft → Approved → Deprecated → Retired（AEOS-STD-006 §9.1） |
| 條目 Lifecycle | 個別 Platform Entry | Candidate → Active → Deprecated → Retired（AEOS-STD-006 §9.2） |

規則：

- 文件狀態與條目狀態 MUST 分開管理；文件 Approved 不代表其中條目全部 Active（AEOS-STD-006 §9.3）。
- 本文件現為 Draft（文件 Lifecycle），不影響日後已核准條目之登錄程序。

## 7. Traceability and Change Rules

### 7.1 Traceability

- 每個 Platform Entry MUST 宣告 Architecture Reference；無參考即不得登錄（AEOS-STD-006 §7）。
- 條目事實與 Architecture 衝突時，以 Architecture 為準並啟動修正。

### 7.2 Change Rules

- 條目新增（Candidate → Active）：MUST 具 Architecture Reference 並經 Review（AEOS-STD-006 §10.2）。
- 條目修改：變更 Identity、Boundary、Classification、Owner 視為重大變更，MUST 經 Review。
- 條目移除：MUST 依 Deprecated → Retired 流程執行，不得直接刪除。
- 本 EWO 不新增任何 Ownership 或 Dependency Relationship。

## 8. Consistency

Platform Catalog MUST 符合 AEOS-STD-006 §11 一致性規則：

- 無重複 Platform Entry ID。
- 無孤兒引用：Validated Facts 中引用之 Capability／Repository 條目存在於對應 Catalog（登錄時驗證）。
- 與 AEOS-ARCH-001 §8 Register 及相關 Architecture 之狀態一致。
- 變更後 MUST 重新執行格式、Metadata、Cross-reference、Placeholder 與一致性驗證。

## 9. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0） | Architecture Source | Platform Topology 之唯一來源 |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 架構基線與 Register |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | Platform Architecture 之定位 |
| REF-004 | [AEOS-ARCH-005 — Platform Architecture](../architecture/AEOS-ARCH-005-Platform-Architecture.md)（Approved 1.0.0） | Architecture | Platform 身分、邊界、分類與登錄規則 |
| REF-005 | [AEOS-STD-006 — Enterprise Architecture Catalog and Matrix Standard](../standards/AEOS-STD-006-Enterprise-Architecture-Catalog-and-Matrix-Standard.md)（Approved 1.0.0） | Standard | Catalog Schema、Entry ID、Lifecycle、Review 與一致性規則 |
| REF-006 | EWO-AEOS-0024 | EWO | 本文件之工作來源 |

## 10. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | Catalog／Matrix Review 核准並合併；狀態更新為 Approved，成為 AEOS Platform Catalog 正式登錄來源（EWO-AEOS-0025；CM-AEOS-0025-R1）；具名 Platform 條目維持 0 | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 AEOS-STD-006 建立 Platform Catalog 權威結構、Entry Schema、Lifecycle、Traceability、Change 與一致性規則；本版登錄具名 Platform 條目為 0（現行 Approved Architecture 未核准任何具名 Platform）（EWO-AEOS-0024） | Codex |
