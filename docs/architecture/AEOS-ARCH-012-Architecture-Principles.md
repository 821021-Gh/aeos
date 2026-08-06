---
doc-id: AEOS-ARCH-012
doc-name: Architecture Principles
doc-type: Architecture
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0041
  - AEOS-ADR-001
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-003
  - AEOS-ARCH-004
  - AEOS-CON-001
---

# AEOS-ARCH-012 — Architecture Principles

> EWO-AEOS-0041：正式定義 Architecture Principles，作為 AEOS Architecture Baseline 之獨立組成（AEOS-ARCH-001 §4.6、AEOS-ADR-001）。本文件經逐項分析後，將 AEOS-ARCH-004 §4 之架構特性升格並重新表述為正式 Architecture Principles；新原則屬 AEOS Architecture Facts，非任何外部歷史架構來源之原始原則、重建或恢復版本。

## Executive Summary

Architecture Principles 定義約束 AEOS 架構決策之正式準則。本文件建立原則模型，並依逐項分析將 AEOS-ARCH-004 §4 之六項架構特性（Architecture First、Platform Oriented、Capability Driven、Repository Governed、Dependency Explicit、Workspace Integrated）升格並重新表述為六項正式 Architecture Principle，每項含 Statement、Rationale、Implications 與 Conformance。本文件亦界定原則之適用範圍、決策與 Review 用法、例外／偏差治理，以及與 Governance Principles、Repository Principles 之關係。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-012 |
| 文件名稱 | Architecture Principles |
| 型別 | Architecture（Architecture Principles Definition） |
| 狀態 | Draft |
| 版本 | 0.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0041、AEOS-ADR-001、AEOS-ARCH-001（Approved 1.0.0）、AEOS-ARCH-002（Approved 1.0.0）、AEOS-ARCH-004（Approved 1.0.0）、AEOS-CON-001（Approved 1.0.0） |
| 關聯文件 | AEOS-ARCH-001～010、AEOS-ADR-001、AEOS-CON-001、AEOS-STD-001～006 |

## 1. Purpose

本文件之目的為：

- 正式定義 Architecture Principles，使 AEOS-ARCH-001 §4.6 獲得可解析之 Architecture Definition。
- 建立原則模型與原則清單（每項含 Statement、Rationale、Implications、Conformance）。
- 界定原則之適用範圍、決策與 Review 用法、例外／偏差治理。
- 明確與 Governance Principles、Repository Principles 之關係與邊界。

## 2. Scope

### 2.1 In Scope

- Architecture Principles 之正式定義、Authority Scope 與 Principle Model。
- 六項 Architecture Principle 之定義與逐項升格分析。
- 原則適用、決策／Review 用法、例外／偏差治理與 Conformance。

### 2.2 Out of Scope

- Governance Principles（由 AEOS-ARCH-002 承載）。
- Repository Principles（由 Repository 層級治理承載）。
- 各架構領域之具體定義與規則。
- 任何不可驗證外部來源之原則重建或恢復。

## 3. Definition

**Architecture Principles 是約束 AEOS 架構決策之正式準則：每一項原則以 Statement 表述其約束，以 Rationale 說明其理由，以 Implications 界定其決策影響，以 Conformance 定義其可稽核之驗證方式。**

事實分類：

- **既有事實**：AEOS-ARCH-004 §4 之六項架構特性（願景特性）。
- **升格與重新表述**：本文件依逐項分析（§6）將六項特性升格為正式 Architecture Principle，並以原則格式重新表述（新增 Statement／Rationale／Implications／Conformance）。
- **新增規範性內容**：原則之正式地位、適用範圍、決策與 Review 用法、例外／偏差治理（本文件首次定義），屬 AEOS Architecture Facts。

## 4. Authority Scope

- Architecture Principles 之權威範圍：**約束 AEOS 架構決策**（架構資產之建立、變更、演進與審查）。
- 本文件不擁有 Governance Principles（AEOS-ARCH-002）或 Repository Principles（Repository 治理）之定義權威。
- 本文件不取代各架構領域之具體規則。
- 原則之權威僅於其經 Review 核准並成為 Approved 後生效。

## 5. Principle Model

每項 Architecture Principle MUST 具備下列組成：

| 組成 | 定義 |
|------|------|
| Name | 原則名稱 |
| Statement | 原則之正式約束表述 |
| Rationale | 原則存在之理由與依據 |
| Implications | 原則對架構決策之影響 |
| Conformance | 原則之可稽核驗證方式（供 Architecture Review 使用） |

原則之升格程序：候選原則 MUST 經逐項分析（§6）確認其具有明確決策約束、可被 Architecture Review 驗證、且不與既有 Governance／Repository Principles 重複後，始得納入正式原則清單。

## 6. Principles

### 6.1 Principle AP-01 — Architecture First

| 組成 | 內容 |
|------|------|
| Statement | Repository 與實作決策 MUST 可追溯至正式架構。 |
| Rationale | 維持單一權威與可追溯性，防止未經治理之架構演進。 |
| Implications | 新增或變更架構資產須經正式流程；實作不得先於架構核准。 |
| Conformance | 架構資產與實作決策之追溯關係可稽核。 |

升格分析：適合升格（具明確決策約束、可驗證、與 Governance／Repository Principles 不重複）；自 AEOS-ARCH-004 §4 特性重新表述為原則格式。

### 6.2 Principle AP-02 — Platform Oriented

| 組成 | 內容 |
|------|------|
| Statement | Platform 作為能力承載與 Repository 協作之穩定架構邊界。 |
| Rationale | 以穩定邊界組織能力承載與跨 Repository 協作，避免元素分類混淆。 |
| Implications | 元素分類與邊界決策須依 Platform Architecture（AEOS-ARCH-005）。 |
| Conformance | Platform／Capability／Repository 之分類與邊界可稽核。 |

升格分析：適合升格（具明確決策約束、可依 ARCH-005 驗證、不重複）；自特性重新表述。

### 6.3 Principle AP-03 — Capability Driven

| 組成 | 內容 |
|------|------|
| Statement | 能力以責任與企業結果描述，不預先指定實作。 |
| Rationale | 保持能力定義與實作解耦，支援長期演進。 |
| Implications | Capability 定義不得以特定實作或 Repository 取代。 |
| Conformance | Capability 定義不含實作細節（依 AEOS-ARCH-007 驗證）。 |

升格分析：適合升格（具決策約束、可驗證、不重複）；自特性重新表述。

### 6.4 Principle AP-04 — Repository Governed

| 組成 | 內容 |
|------|------|
| Statement | Repository 是架構資產與工程交付之正式治理單位。 |
| Rationale | 以 Repository 為治理與交付邊界，確保架構資產可管理、可追溯。 |
| Implications | 架構資產之建立與變更須以 Repository 為治理單位；不重述 Repository Governance 程序。 |
| Conformance | Repository 之架構身分與交付可稽核。 |

升格分析：適合升格（架構層面具決策約束）；與 Repository Governance（AEOS-CON-001）之關係為「架構原則約束架構層面，治理程序由 CON-001 承載」，**不重複、不取代**；自特性重新表述並聚焦架構層面。

### 6.5 Principle AP-05 — Dependency Explicit

| 組成 | 內容 |
|------|------|
| Statement | 跨層、跨平台、跨能力與跨 Repository 之依賴 MUST 被識別與管理。 |
| Rationale | 依賴明確化使影響分析與演進決策可執行。 |
| Implications | 依賴須依 Dependency Architecture（AEOS-ARCH-009）登錄與治理。 |
| Conformance | 依賴之識別、方向與登錄可稽核（依 AEOS-ARCH-009）。 |

升格分析：適合升格（具決策約束、可依 ARCH-009 驗證、不重複）；自特性重新表述。

### 6.6 Principle AP-06 — Workspace Integrated

| 組成 | 內容 |
|------|------|
| Statement | 各 Repository 共同構成單一 AI Engineering Workspace，而非彼此孤立之專案集合。 |
| Rationale | 維持跨 Repository 之整體一致性與共享治理。 |
| Implications | 跨 Repository 架構資產須可追溯至 Workspace 基線（AEOS-ARCH-010）。 |
| Conformance | 跨 Repository 資產之 Workspace 追溯可稽核。 |

升格分析：適合升格（具決策約束、可依 ARCH-010 驗證、不重複）；自特性重新表述。

## 7. Principle Applicability

- 六項原則適用於全部 AEOS 架構資產與架構決策（Architecture、Catalog、Matrix、ADR）。
- 原則之適用優先序：原則間衝突時，以 Conformance 最嚴格者為準；仍無法解決時，依 AEOS-ARCH-003 以 ADR 裁決。

## 8. Decision and Review Usage

- 架構決策 MUST 於決策紀錄（ADR）中宣告其遵循之原則。
- Architecture Review MUST 依原則之 Conformance 驗證候選架構資產。
- 新增或變更原則 MUST 經 EWO 與 Architecture Review；本文件為原則之唯一權威清單。

## 9. Exceptions／Deviation Governance

- 對原則之例外或偏差 MUST 以 ADR 記錄，說明理由、影響與期限。
- 例外不得常態化；原則偏差 MUST 定期複審。
- 未經 ADR 核准之偏差視為不合規。

## 10. Relationship to Governance Principles

- Governance Principles（AEOS-ARCH-002 GA-001～）約束治理結構與治理決策；Architecture Principles 約束架構決策。
- 兩者互補不重疊：本文件之原則聚焦架構資產與架構演進；治理原則聚焦治理機制。

## 11. Relationship to Repository Principles

- Repository Principles（Repository 層級治理）適用於個別 Repository 之運作；Architecture Principles 為上位架構準則。
- 衝突時以 Architecture Principles 為準（架構權威高於 Repository 層級原則），但不得取代 Repository Governance 程序。

## 12. Exclusions

- Governance Principles（AEOS-ARCH-002）。
- Repository Principles（Repository 治理）。
- 各架構領域之具體規則與定義。
- 任何不可驗證外部來源之原則內容重建或恢復。

## 13. Conformance Rules

- 全部架構決策 MUST 遵循本文件六項原則。
- 架構資產之 Review MUST 依原則 Conformance 驗證。
- 新增或變更原則清單 MUST 經本文件之 Amendment（EWO 與 Review）。
- 違反原則且未經 ADR 例外核准之架構資產 MUST NOT 被視為 AEOS 正式架構資產。

## 14. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | 基線與 Register；§4.6 組成 |
| REF-002 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved 1.0.0） | Architecture | Governance Principles（GA-001～） |
| REF-003 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved 1.0.0） | Architecture | 原則例外／偏差之 ADR 治理 |
| REF-004 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | §4 六項特性（升格來源） |
| REF-005 | [AEOS-ARCH-005～010](AEOS-ARCH-005-Platform-Architecture.md)（Approved 1.0.0） | Architecture | 原則 Conformance 之領域依據 |
| REF-006 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved 1.0.0） | Constitution | Repository Governance 與 Repository Principles 邊界 |
| REF-007 | [AEOS-ADR-001 — Architecture Definition Carrier Decision](../adr/AEOS-ADR-001-Architecture-Definition-Carrier-Decision.md)（本 EWO，Draft） | ADR | 本文件之建立依據 |
| REF-008 | EWO-AEOS-0041 | EWO | 本文件之工作來源 |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-06 | 初版建立：定義 Architecture Principles（原則模型、六項原則及逐項升格分析、適用範圍、決策／Review 用法、例外治理、與 Governance／Repository Principles 邊界）（EWO-AEOS-0041；AR-AEOS-0041-R3） | Codex |
