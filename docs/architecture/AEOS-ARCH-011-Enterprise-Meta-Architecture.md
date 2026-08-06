---
doc-id: AEOS-ARCH-011
doc-name: Enterprise Meta Architecture
doc-type: Architecture
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0041
  - EWO-AEOS-0042
  - AR-AEOS-0041-R2
  - AEOS-ADR-001
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-ARCH-004
  - AEOS-ARCH-006
---

# AEOS-ARCH-011 — Enterprise Meta Architecture

> EWO-AEOS-0041：正式定義 Enterprise Meta Architecture，作為 AEOS Architecture Baseline 之獨立組成（AEOS-ARCH-001 §4.2、AEOS-ADR-001）。本文件整合現行 Approved AEOS Artifacts 之既有架構事實，建立 Meta Architecture 之正式定義；非任何外部歷史架構來源之重建、轉錄、替代原文或恢復版本。

## Executive Summary

Enterprise Meta Architecture 定義 AEOS Enterprise Architecture 之「上層結構與關係規則」：架構自身之抽象框架、權威階層、領域模型與跨領域／跨層規則之治理邊界。本文件依現行 Approved AEOS Artifacts（AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-006）之既有事實整合定義；Layer Architecture（AEOS-ARCH-006）承載具體責任分層模型，本文件承載 Meta 層級之上層結構與關係規則，兩者分工不重疊、不循環。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-011 |
| 文件名稱 | Enterprise Meta Architecture |
| 型別 | Architecture（Enterprise Meta Architecture Definition） |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0041、EWO-AEOS-0042、AR-AEOS-0041-R2、AEOS-ADR-001、AEOS-ARCH-001（Approved 1.0.0）、AEOS-ARCH-002（Approved 1.0.0）、AEOS-ARCH-003（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-ARCH-006（Approved 1.0.0） |
| 關聯文件 | EWO-AEOS-0042、AR-AEOS-0041-R2、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005～010、AEOS-ADR-001、AEOS-STD-001～006 |

## 1. Purpose

本文件之目的為：

- 正式定義 Enterprise Meta Architecture，使 AEOS-ARCH-001 §4.2 獲得可解析之 Architecture Definition。
- 定義架構之「上層結構與關係規則」：架構之抽象框架、權威階層、領域模型與跨領域／跨層規則之治理邊界。
- 明確 Meta Architecture 與 Layer Architecture（AEOS-ARCH-006）、Enterprise Architecture Overview（AEOS-ARCH-004）之分工與權威邊界。
- 提供後續架構決策、ADL 與架構演進之 Meta 層級規則依據。

## 2. Scope

### 2.1 In Scope

- Enterprise Meta Architecture 之正式定義、Authority Scope、Boundary 與 Responsibilities。
- 架構之結構元素（Structural Elements）與權威階層。
- 與架構領域、Layer Architecture 之關係及跨領域／跨層規則。
- Conformance 規則。

### 2.2 Out of Scope

- 具體責任分層模型與依賴方向規則（由 AEOS-ARCH-006 Layer Architecture 承載）。
- 各架構領域之具體定義（由 AEOS-ARCH-005～010 承載）。
- Enterprise Architecture Overview 之統合視圖職責（由 AEOS-ARCH-004 承載）。
- 重建、轉錄或恢復任何外部歷史架構來源；本文件不以任何不可驗證來源為權威。

## 3. Definition

**Enterprise Meta Architecture 是 AEOS Enterprise Architecture 之 Meta 層級定義：定義架構自身之上層結構與關係規則，包括架構之抽象框架、權威階層、領域模型，以及跨領域與跨層關係之治理邊界。**

Meta Architecture 不定義具體領域內容（Platform、Capability、Repository 等），而定義：

- 架構「如何被組織」：抽象框架（Architecture Authority、Enterprise Architecture Model）。
- 架構「如何被治理」：權威階層與決策治理（ADR System）。
- 架構「如何相互關聯」：跨領域關係模型與跨層規則之治理邊界。

事實分類：

- **既有事實**：取自現行 Approved AEOS Artifacts（AEOS-ARCH-002 §3/§5、AEOS-ARCH-003、AEOS-ARCH-004 §3/§5/§7/§8、AEOS-ARCH-006 §4/§5/§6）。
- **整合定義**：本文件將上述既有事實統合為 Meta Architecture 之正式定義（上層結構與關係規則）。
- **新增規範性內容**：本文件首次明確定義 Meta Architecture 之定位、Authority Scope、與 Layer Architecture 之分工（§4、§10），作為 AEOS Architecture Fact。

## 4. Authority Scope

- Enterprise Meta Architecture 之權威範圍：**架構之上層結構與關係規則**（抽象框架、權威階層、領域模型、跨領域／跨層規則之治理邊界）。
- 本文件不擁有各架構領域之定義權威；領域定義由對應 Approved Artifact 承載（AEOS-ARCH-005～010）。
- 本文件不擁有具體責任分層模型之定義權威；分層模型由 AEOS-ARCH-006 承載。
- 本文件不取代 AEOS-ARCH-004 之 Overview 職責；不取代 AEOS-ARCH-003 之決策治理。
- 本文件之權威僅於其經 Review 核准並成為 Approved 後生效。

## 5. Boundary

| 面向 | 內容 |
|------|------|
| 涵蓋 | 抽象框架、權威階層、領域模型、跨領域／跨層規則之治理邊界 |
| 不涵蓋 | 具體領域定義（ARCH-005～010）、具體分層模型（ARCH-006）、治理文件與決策紀錄（ARCH-002/003） |
| 上層 | 架構權威來源與 Architecture Baseline（AEOS-ARCH-001） |
| 下層 | 各架構領域 Artifact（AEOS-ARCH-005～010） |

規則：

- Meta Architecture MUST NOT 重新定義或改寫各領域 Artifact 之內容。
- Meta Architecture MUST NOT 自行承載具體分層模型（屬 Layer Architecture）。

## 6. Responsibilities

Enterprise Meta Architecture 之責任：

- 定義架構之抽象框架（Authority 階層、領域模型、資產分類）。
- 定義跨領域與跨層關係之治理規則與邊界。
- 確保各架構領域 Artifact 符合 Meta 層級之上層結構與關係規則。
- 提供架構演進與決策之 Meta 層級依據。

## 7. Structural Elements

Enterprise Meta Architecture 之結構元素（整合自現行 Approved AEOS Artifacts）：

| 元素 | 定義 | 既有來源 |
|------|------|----------|
| Architecture Authority 階層 | 架構資產之權威順序（A0 架構權威來源至 A5 Repository 層級） | AEOS-ARCH-004 §3 |
| Enterprise Architecture Model | 六個架構領域（Platform、Layer、Capability、Repository、Dependency、Workspace）之整體模型 | AEOS-ARCH-004 §5 |
| Architecture Assets 分類 | Architecture／Catalog／Matrix 三類資產 | AEOS-ARCH-004 §8 |
| Governance Hierarchy | 治理文件與決策之階層（H0～H7） | AEOS-ARCH-002 §5 |
| Decision Governance | 架構決策紀錄機制（ADR System） | AEOS-ARCH-003 |
| 跨領域關係模型 | 領域間之關係類型與規則 | AEOS-ARCH-004 §7 |

## 8. Architecture Authority Relationship

- Meta Architecture 位於 Architecture Authority 之 Meta 層級：定義權威階層與關係規則，不凌駕於具體領域定義之上。
- 具體領域 Artifact 之權威以其各自 Approved 狀態為準；Meta Architecture 不得覆寫或改寫之。
- 架構決策依 AEOS-ARCH-003 之 ADR 機制記錄；Meta Architecture 提供決策之 Meta 層級上下文。

## 9. Relationship to Architecture Domains

- Meta Architecture 定義六個架構領域之整體框架與相互關係（AEOS-ARCH-004 §5、§7）。
- 各領域之具體定義由對應 Approved Artifact 承載（Platform→ARCH-005；Capability→ARCH-007；Repository→ARCH-008；Dependency→ARCH-009；Workspace→ARCH-010；Layer→ARCH-006）。
- 領域間之關係 MUST 符合跨領域關係模型；新增或變更領域關係 MUST 依 ADR／架構治理程序。

## 10. Relationship to Layer Architecture

分工規則（本文件之新增規範性內容）：

- **Enterprise Meta Architecture 定義上層結構與關係規則**（抽象框架、權威階層、領域模型、跨領域／跨層規則之治理邊界）。
- **Layer Architecture（AEOS-ARCH-006）承載具體責任分層模型與依賴方向規則**（Governance／Enterprise Architecture／Platform／Capability／Repository／Implementation 六層）。
- 兩者互補不重疊：Meta 定義「架構如何被組織與治理」，Layer 定義「架構責任如何分層」。
- 本文件 MUST NOT 複製或取代 AEOS-ARCH-006 之分層模型與規則。

## 11. Cross-domain／Cross-layer Rules

- 跨領域關係 MUST 符合 AEOS-ARCH-004 §7 關係模型（方向、類型、依據）。
- 跨層依賴 MUST 符合 AEOS-ARCH-006 §5 方向規則（上位約束、下位依賴；禁止未核准反向控制與跨層繞過）。
- 跨領域／跨層變更 MUST 依 AEOS-ARCH-003 判斷 ADR 需求，並經 Architecture Review。

## 12. Exclusions

- 具體責任分層模型（AEOS-ARCH-006）。
- 各架構領域之具體定義與規則（AEOS-ARCH-005～010）。
- Governance Architecture 之治理內容與階層細節（AEOS-ARCH-002）。
- 架構決策之具體紀錄（ADR 文件）。
- 任何不可驗證外部來源之內容重建或恢復。

## 13. Conformance Rules

- 各架構領域 Artifact MUST 符合 Meta Architecture 之抽象框架與關係規則。
- 各架構領域 Artifact MUST 宣告其於 Authority 階層中之定位。
- 新架構資產（Architecture／Catalog／Matrix）MUST 依 Meta Architecture 之資產分類與登錄規則建立。
- 違反本文件之架構資產 MUST NOT 被視為 AEOS 正式架構資產。

## 14. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 基線與 Register；§4.2 組成 |
| REF-002 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved 1.0.0） | Architecture | Governance Hierarchy 既有事實 |
| REF-003 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved 1.0.0） | Architecture | Decision Governance 既有事實 |
| REF-004 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | Authority 階層、領域模型、關係模型既有事實 |
| REF-005 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved 1.0.0） | Architecture | 具體分層模型（分工對象） |
| REF-006 | AEOS-ARCH-005、AEOS-ARCH-007～010（Approved 1.0.0） | Architecture | 各架構領域 Artifact |
| REF-007 | [AEOS-ADR-001 — Architecture Definition Carrier Decision](../adr/AEOS-ADR-001-Architecture-Definition-Carrier-Decision.md)（本 EWO，Draft） | ADR | 本文件之建立依據 |
| REF-008 | EWO-AEOS-0041 | EWO | 本文件之工作來源 |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | Lifecycle Approval（EWO-AEOS-0042）：經 AR-AEOS-0041-R2（Reviewer APPROVED）與 PR #41 合併後，狀態更新為 Approved 1.0.0，成為 AEOS Enterprise Meta Architecture 正式 Definition | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：整合現行 Approved AEOS Artifacts 之既有事實，正式定義 Enterprise Meta Architecture（上層結構與關係規則）；明確與 Layer Architecture、Overview 之分工（EWO-AEOS-0041；AR-AEOS-0041-R2） | Codex |
