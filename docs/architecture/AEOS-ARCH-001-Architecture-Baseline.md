---
doc-id: AEOS-ARCH-001
doc-name: Architecture Baseline
doc-type: Architecture
repository: AEOS
version: 1.5.0
status: Approved
owner: Architecture Owner
created: 2026-08-05
updated: 2026-08-27
related:
  - EWO-AEOS-0001
  - EWO-AEOS-0044
  - EWO-AEOS-0045
  - AEOS-ADR-002
  - AEOS-ADR-003
  - AEOS-ADR-004
  - AEOS-ARCH-013
  - AEOS-ARCH-014
  - WA-001
---

# AEOS-ARCH-001 — Architecture Baseline

> EWO-AEOS-0001：正式導入 WA-001 AI Engineering Workspace Architecture（Approved v1.0.0）為 AEOS 架構來源之歷史起點。依 AEOS-ADR-002（WA-001 Fact Authority Transition），WA-001 已分類為歷史來源；本文件為 AEOS Architecture 之 Entry Document，正式架構權威由 Approved AEOS Artifacts 承載。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-001 |
| 文件名稱 | Architecture Baseline |
| 型別 | Architecture（Entry Document） |
| 狀態 | Approved |
| 版本 | 1.5.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-05 |
| 最後更新 | 2026-08-27 |
| 依據文件 | AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ADR-003（Approved 1.0.0）、AEOS-ADR-004（Approved 1.0.0）、AEOS-ADR-001（Approved 1.0.0）、AEOS-ARCH-011（Approved 1.0.0）、AEOS-ARCH-012（Approved 1.0.0） |
| 關聯文件 | EWO-AEOS-0001、EWO-AEOS-0044、EWO-AEOS-0045、AEOS-ADR-001、AEOS-ADR-002、AEOS-ADR-003、AEOS-ADR-004、AEOS-ARCH-013、AEOS-ARCH-014、WA-001（歷史來源） |

## 1. Purpose

本文件為 AEOS（AI Enterprise Operating System）之 Architecture Baseline 與 Architecture Entry Document，其目的為：

- AEOS 為 AI Engineering Workspace 之 Enterprise Root Repository。
- 正式導入 AEOS 之架構權威基線：依 AEOS-ADR-002，WA-001 分類為歷史來源，正式架構權威由 Approved AEOS Artifacts 承載。
- 正式登錄 Approved 架構組成：Workspace Architecture、Enterprise Meta Architecture、Platform Topology、Capability Architecture、Capability Ownership 與 Architecture Principles（各組成之正式定義載體見 §8 Architecture Register）。
- 作為後續 Architecture Catalog 之起點，使後續 AEOS 架構文件可追溯至 Approved 架構載體。
- 界定內容邊界與治理規則：不重新設計架構、不得新增未經 Approved 架構載體定義之內容。

## 2. Background

- AEOS 為 AI Engineering Workspace 之 Enterprise Root Repository，涵蓋 Enterprise Architecture、Platform Governance 與 Capability Management。
- WA-001 為外部歷史架構來源（Approved v1.0.0）；依 AEOS-ADR-002，其內容不可獨立驗證，僅保留為歷史參考，不再作為 AEOS 架構之正式權威依據。
- AEOS-ADR-002 已將 WA-001 分類為歷史來源，並確立 Approved Fact Authority Baseline（§2.2）。
- EWO-AEOS-0001 為 AEOS 之第一張 Engineering Work Order，僅涵蓋 Architecture Baseline Import；Repository Foundation 由 EWO-AEOS-0002 獨立交付。

## 3. Historical Architecture Source — WA-001

| 項目 | 內容 |
|------|------|
| 來源文件代號 | WA-001 |
| 來源文件名稱 | AI Engineering Workspace Architecture |
| 狀態 | Approved |
| 版本 | v1.0.0 |
| 角色 | 歷史來源（Historical Source）；不作為正式 Fact Authority（依 AEOS-ADR-002 §2.1） |

規則：

- AEOS 之正式架構權威依 AEOS-ADR-002 §2.2（Approved Fact Authority Baseline）與 §8 Architecture Register；WA-001 僅為歷史參考。
- AEOS 不得重新設計架構。
- AEOS 不得新增未經 Approved 架構載體定義之內容。

## 4. Approved Architecture

本文件登錄 AEOS Architecture Baseline 之六個架構組成。各組成之正式定義由對應 Approved Architecture Artifact 承載（§8 Architecture Register）；WA-001 保留為歷史來源（AEOS-ADR-002 §2.1），其內容不作為正式權威。AEOS 不另行持有或變更 Approved Artifact 之定義。

### 4.1 Workspace Architecture

- 定位：定義 AI Engineering Workspace 之整體架構。
- 內容範圍：AI Engineering Workspace 之結構、邊界與組成，依 AEOS-ARCH-010 定義。
- 權威來源：AEOS-ARCH-010（Approved 1.0.0）。

### 4.2 Enterprise Meta Architecture

- 定位：定義企業層級之 Meta Architecture。
- 內容範圍：架構之抽象層、框架與跨領域關聯，依 AEOS-ARCH-011 定義。
- 權威來源：AEOS-ARCH-011（Approved 1.0.0）。

### 4.3 Platform Topology

- 定位：定義平台之拓撲結構。
- 內容範圍：平台元件、節點與連結關係，依 AEOS-ARCH-005 定義；WA-001 具名 Platform 內容未於 AEOS 發布，不遷移。
- 權威來源：AEOS-ARCH-005（Approved 1.0.0）；WA-001 具名內容僅歷史參考。

### 4.4 Capability Architecture

- 定位：定義 AI Engineering Workspace 之能力架構。
- 內容範圍：能力之結構與能力間關係，依 AEOS-ARCH-007 定義。
- 權威來源：AEOS-ARCH-007（Approved 1.0.0）。

### 4.5 Capability Ownership

- 定位：定義能力之擁有權模型。
- 內容範圍：能力之 Owner、責任與治理歸屬，依 AEOS-ARCH-007 定義；具名 Owner 關係未核准，不遷移。
- 權威來源：AEOS-ARCH-007（Approved 1.0.0）；具名 Owner 內容僅歷史參考。

### 4.6 Architecture Principles

- 定位：定義架構原則。
- 內容範圍：約束後續架構決策之準則，依 AEOS-ARCH-012 定義。
- 權威來源：AEOS-ARCH-012（Approved 1.0.0）。

## 5. Baseline Definition

- AEOS Architecture Baseline 為 Approved 架構組成於 AEOS 內之正式基線（定義載體見 §8 Architecture Register）。
- 本文件（AEOS-ARCH-001）為 Baseline 之 Entry Document，登錄 Approved 架構組成（§4）與 Architecture Register（§8），並記錄來源、範圍與治理規則。
- Baseline 內容與 Approved 架構載體保持單一來源對應；AEOS 文件不持有 Approved 載體以外之架構決策；WA-001 僅為歷史來源。

## 6. Import Method

- 身分對應：每份 AEOS 架構文件 MUST 宣告其對應之 Approved 架構組成（§8 Architecture Register）與定義載體。
- 內容邊界：AEOS 架構文件內容 MUST NOT 超出對應 Approved 架構載體之定義；發現缺口時，依正式架構變更流程處理，再更新 AEOS Baseline。
- 文件形式：AEOS 架構文件以正式 Markdown 文件置於 `docs/architecture/`，遵循 Repository 文件慣例。
- 追溯：每份 AEOS 架構文件於 References 中引用對應 Approved 架構載體及來源 EWO；WA-001 引用僅限歷史參考。

## 7. Governance Rules

- MUST NOT 重新設計 Architecture。
- MUST NOT 新增未經 Approved 架構載體定義之內容。
- Approved 架構載體變更時，AEOS Baseline 以正式 Amendment 對應更新，並記錄於 Revision History；WA-001 不作為變更來源。
- Repository Foundation 不屬於本 Baseline；由 EWO-AEOS-0002 獨立交付。

## 8. Architecture Register

本 Register 為後續 Architecture Catalog 之起點。

| 文件代號 | 文件名稱 | 型別 | 對應來源 | 狀態 |
|----------|----------|------|----------|------|
| WA-001 | AI Engineering Workspace Architecture | Historical Reference（外部） | — | Approved v1.0.0（歷史） |
| AEOS-ARCH-001 | Architecture Baseline | Architecture Entry Document | AEOS-ADR-002（Transition 決策） | Approved |
| AEOS-ARCH-002 | Enterprise Governance Architecture | Architecture | AEOS-ARCH-001（Approved） | Approved |
| AEOS-ARCH-003 | Architecture Decision Record System | Architecture | AEOS-ARCH-001（Approved） | Approved |
| AEOS-ARCH-004 | AI Enterprise Architecture Overview | Enterprise Architecture Entry Document | AEOS-ARCH-001（Approved） | Approved |
| AEOS-ARCH-005 | Platform Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-006 | Layer Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-007 | Capability Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-008 | Repository Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-009 | Dependency Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-010 | Workspace Architecture | Architecture | AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved） | Approved |
| AEOS-ARCH-011 | Enterprise Meta Architecture | Architecture | AEOS-ARCH-004、AEOS-ARCH-006 | Approved |
| AEOS-ARCH-012 | Architecture Principles | Architecture | AEOS-ARCH-004 | Approved |
| AEOS-ARCH-013 | Enterprise AI Agent Architecture | Architecture | AEOS-ADR-003、AEOS-ARCH-001（Approved）、AEOS-ARCH-004（Approved）、AEOS-ARCH-005（Approved）、AEOS-ARCH-006（Approved）、AEOS-ARCH-007（Approved）、AEOS-ARCH-009（Approved）、AEOS-ARCH-010（Approved）、AEOS-ARCH-012（Approved） | Approved |
| AEOS-ARCH-014 | Productizable Platform Architecture | Architecture | AEOS-ADR-004、AEOS-ARCH-001（Approved）、AEOS-ARCH-005（Approved）、AEOS-ARCH-006（Approved）、AEOS-ARCH-007（Approved）、AEOS-ARCH-009（Approved）、AEOS-ARCH-010（Approved）、AEOS-ARCH-012（Approved）、AEOS-ARCH-013（Approved） | Approved |

後續 AEOS 架構文件依 Approved 架構載體建立後，於本 Register 登錄並更新版本。

## 9. Out of Scope

- Repository Foundation（README、VERSION、LICENSE、CHANGELOG、CONTRIBUTING 等）：由 EWO-AEOS-0002 交付。
- Platform Governance 與 Capability Management 之正式文件：由後續 EWO 依 Approved 架構載體建立。
- 任何未經 Approved 架構載體定義之架構內容。
- Implementation、Technical Design 與系統部署內容。

## 10. References

| 文件 | 型別 | 用途 |
|------|------|------|
| WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| AEOS-ADR-001 — Architecture Definition Carrier Decision | ADR | Definition Carrier 決策（AEOS-ARCH-011／012 之承載依據） |
| AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline（§2.1／§2.2） |
| AEOS-ADR-004 — Productization Boundary Decision | ADR | Internal Reference Implementation、Reusable Capability、Platform Core 與 Commercial Product Packaging 分離決策 |
| AEOS-ARCH-005 — Platform Architecture | Architecture | Platform Topology 正式定義載體（AEOS-ARCH-001 §4.3） |
| AEOS-ARCH-007 — Capability Architecture | Architecture | Capability Architecture 與 Capability Ownership 正式定義載體（AEOS-ARCH-001 §4.4／§4.5） |
| AEOS-ARCH-010 — Workspace Architecture | Architecture | Workspace Architecture 正式定義載體（AEOS-ARCH-001 §4.1） |
| AEOS-ARCH-011 — Enterprise Meta Architecture | Architecture | Enterprise Meta Architecture Definition（AEOS-ARCH-001 §4.2） |
| AEOS-ARCH-012 — Architecture Principles | Architecture | Architecture Principles Definition（AEOS-ARCH-001 §4.6） |
| AEOS-ARCH-014 — Productizable Platform Architecture | Architecture | Productization Boundary 與 capability promotion 正式架構載體 |
| EWO-AEOS-0001 — Architecture Baseline Import | EWO | 本文件之工作來源 |

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.5.0 | 2026-08-27 | 依 EWO-AEOS-0045 Post-Merge Closure Verification：登錄 AEOS-ARCH-014（Productizable Platform Architecture）為 Approved Architecture；補入 AEOS-ADR-004（Approved 1.0.0）作為 Productization Boundary 決策依據 | ChatGPT |
| 1.4.0 | 2026-08-27 | 依 EWO-AEOS-0044 Post-Merge Closure Verification：登錄 AEOS-ARCH-013（Enterprise AI Agent Architecture）為 Approved Architecture；補入 AEOS-ADR-003（Approved 1.0.0）作為 Agent Control Plane / Runtime Separation 決策依據 | Codex |
| 1.3.0 | 2026-08-07 | 依 EWO-AEOS-0040 Wave 1（AEOS-ADR-002 已核准並合併至 main）：執行 Authority Rule Transition——WA-001 分類為歷史來源；§3／§4／§5／§6／§7／§8／§9／§10 之架構權威與對應來源重錨至 Approved 架構載體與 AEOS-ADR-002 §2.2 | Codex |
| 1.2.0 | 2026-08-06 | 依 EWO-AEOS-0042 同步 Architecture Register：AEOS-ARCH-011（Enterprise Meta Architecture）與 AEOS-ARCH-012（Architecture Principles）狀態更新為 Approved（AR-AEOS-0041-R2／R3 已通過並合併） | Codex |
| 1.1.0 | 2026-08-06 | 依 EWO-AEOS-0041 同步 Architecture Register：登錄 AEOS-ARCH-011（Enterprise Meta Architecture）與 AEOS-ARCH-012（Architecture Principles）為 Draft；References 新增兩份 Definition 之交叉引用（AR-AEOS-0041-R4） | Codex |
| 1.0.0 | 2026-08-05 | 正式導入 WA-001（Approved v1.0.0）為唯一架構來源；納入已核准架構（Workspace Architecture、Enterprise Meta Architecture、Platform Topology、Capability Architecture、Capability Ownership、Architecture Principles）；確立 Architecture Register；版本升級至 1.0.0 / Approved（AR-AEOS-0001 RC-001） | Codex |
| 0.1.0 | 2026-08-05 | 初版：導入 WA-001 為唯一架構來源，建立 Architecture Baseline（EWO-AEOS-0001） | Codex |
