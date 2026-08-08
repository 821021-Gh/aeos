---
doc-id: AEOS-ADR-002
doc-name: WA-001 Fact Authority Transition
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
decision-status: Approved
decision-owner: Architecture Owner
decision-date: 2026-08-08
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0040
  - EWO-AEOS-0041
  - EWO-AEOS-0042
  - AD-AEOS-0041-R1
  - AR-AEOS-0041-R2
  - AR-AEOS-0041-R3
  - AR-AEOS-0041-R4
  - AEOS-ADR-001
  - AEOS-ARCH-001
  - AEOS-ARCH-003
  - AEOS-ARCH-011
  - AEOS-ARCH-012
---

# AEOS-ADR-002 — WA-001 Fact Authority Transition

> EWO-AEOS-0040：記錄 WA-001 Fact Authority Transition 之正式決策，定義 WA-001 之 Authority Classification、WA-001 與 AEOS Approved Architecture Artifacts 之權威關係、Fact Authority Transition 之適用範圍、既有引用之最小重錨原則、歷史參考之轉換、禁止事項、後續執行順序與生效條件。本 ADR 為 EWO-AEOS-0040 執行 Blocker 解除後之第一項恢復交付；本 ADR 之決策已依 ADR Lifecycle Completion 核准（Approved 1.0.0）。

## Executive Summary

本 ADR 記錄 WA-001 Fact Authority Transition 決策（`decision-status`：`Approved`）：WA-001 保留為歷史來源、背景材料與不可獨立驗證之歷史引用，MUST NOT 繼續作為 Enterprise Meta-Architecture、Architecture Principles 或其他已經由 Approved AEOS Artifact 承載之事實之正式權威來源；正式事實之權威由現行 Approved Artifacts 承載（AEOS-ARCH-011、AEOS-ARCH-012、AEOS-ADR-001，並由 AEOS-ARCH-001 Register 登錄）。Transition 僅允許最小必要之 Authority Classification 更新與 Reference Re-anchoring，且必須於本 ADR 核准並生效後依既定順序執行；MUST NOT 重建、改寫、反向推導 WA-001，MUST NOT 建立新 Platform、Capability、Ownership、Dependency、Mapping、Placement 或 Matrix，MUST NOT 開始 Candidate Assessment、執行 PC-003 或處理 Catalog Registration。本 ADR 已依 ADR Lifecycle Completion 升版至 1.0.0 / Approved；Transition 決策依 §2.6 生效條件正式生效。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ADR-002 |
| 文件名稱 | WA-001 Fact Authority Transition |
| 型別 | ADR（Architecture Decision Record） |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Decision Status | Approved |
| Decision Owner | Architecture Owner |
| Decision Date | 2026-08-08 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0040、EWO-AEOS-0041、EWO-AEOS-0042、AD-AEOS-0041-R1、AR-AEOS-0041-R2、AR-AEOS-0041-R3、AR-AEOS-0041-R4、AEOS-ADR-001（Approved 1.0.0）、AEOS-ARCH-001（Approved 1.3.0）、AEOS-ARCH-003（Approved 1.0.0） |
| 關聯文件 | AEOS-ADR-001、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-011（Approved 1.0.0）、AEOS-ARCH-012（Approved 1.0.0）、AEOS-DIA-001、AEOS-CON-001、AEOS-STD-001～005、WA-001 |

## 1. Context

### 1.1 EWO-AEOS-0040 恢復依據

- EWO-AEOS-0040 之正式 Lifecycle Status 為 **In Progress**（依 YEOS EWO-SYS-001 §4 Lifecycle；Repository 未定義 Paused／Blocked／Stopped／Resumed／Unblocked／Active 等正式狀態）。本次不建立新的 Lifecycle Status，不建立獨立 Blocker Resolution EWO，不建立新 EWO ID。
- EWO-AEOS-0040 前次停滯原因（Blocker／Status Reason）：AEOS-ARCH-001 §4 之 Enterprise Meta Architecture 與 Architecture Principles 缺少可解析之 Approved Architecture Definition；EWO-AEOS-0040 必須等待 EWO-AEOS-0041 完成 Review、合併並達成生效條件後恢復（依 AEOS-ADR-001 Context 既有紀錄）。
- 本階段經 Chief Architect 之 **Post-Merge Approval Effectiveness 與 Blocker Resolution Readiness Review** 判定所有原始必要條件均已 Satisfied，正式解除 EWO-AEOS-0040 之執行 Blocker，恢復既有 In Progress 工作。
- 本 ADR 為 EWO-AEOS-0040 執行 Blocker 解除後之第一項恢復交付，屬既有 EWO-AEOS-0040 之恢復交付，非新 EWO。
- Post-Merge Readiness Review 僅為 Blocker 解除條件之判定，不構成、亦不得被記錄為 Architecture Review、Approval Review 或 ADR Review；本 ADR 之正式 Review 於後續流程另行執行（依 §2.6）。

### 1.2 Blocker 解除證據（Repository 可驗證事實）

- PR #41 已合併至 `main`，merge commit 為 `64aceba`（GitHub state：MERGED；mergedAt：2026-08-06T12:06:05Z）。
- PR #42 已取得非作者 Reviewer `yeelightpro`（GitHub 角色：COLLABORATOR）之 GitHub `APPROVED`（submittedAt：2026-08-06T12:31:22Z）。
- PR #42 已合併至 `main`，merge commit 為 `329cdc0`（mergedAt：2026-08-06T12:31:56Z）。
- AEOS-ADR-001 已為 `1.0.0 / Approved`，且 `decision-status` 為 `Approved`。
- AEOS-ARCH-011 已為 `1.0.0 / Approved`。
- AEOS-ARCH-012 已為 `1.0.0 / Approved`。
- AEOS-ARCH-001 已為 `1.2.0 / Approved`，且 §8 Architecture Register 已同步（AEOS-ARCH-011／012 登錄為 Approved）。
- AD-AEOS-0041-R1、AR-AEOS-0041-R2、AR-AEOS-0041-R3 與 AR-AEOS-0041-R4 均沿用既有可驗證紀錄（分別見 AEOS-ADR-001 與 AEOS-ARCH-001 之 Revision History）。
- Post-Merge Readiness Review 判定所有必要條件均已滿足；本 ADR 不為該 Readiness Review 新增 Review ID（Repository 中無真實、可驗證且經正式指派之對應 Review ID）。
- 因 Repository 未定義 `Blocked` 或 `Resumed` 為正式 Lifecycle Status，EWO-AEOS-0040 繼續保持 `In Progress`，僅解除執行層面之 Blocker。

### 1.3 決策需求

本 ADR 之決策需求為正式決定：

1. WA-001 之 Authority Classification；
2. WA-001 與 AEOS Approved Architecture Artifacts 之權威關係；
3. Fact Authority Transition 之適用範圍；
4. 既有引用應如何最小重錨；
5. 哪些內容可以轉換為歷史參考；
6. 哪些內容不得由 WA-001 繼續提供正式架構權威；
7. 如何避免重建、改寫或反向推導 WA-001；
8. 如何避免循環引用與雙重權威；
9. 後續修改 AEOS-ARCH-001、AEOS-CON-001、AEOS-ARCH-004～010、AEOS-RPT-001／002 時之執行邊界。

### 1.4 決策邊界與階段區分

- 「Blocker 解除」與「Fact Authority Transition 實際執行」為兩件不同之事：
  - **Blocker 解除**：本階段已依 §1.2 證據完成（執行層面恢復既有 In Progress 工作）。
  - **Fact Authority Transition 實際執行**：僅在本 ADR 依 §2.6 達成生效條件後，依 §2.4 執行順序另行執行。
- 本 ADR 之決策已核准（`decision-status`：`Approved`）並依 §2.6 生效條件正式生效；本 ADR 不宣告 Transition 已完成，不宣告 EWO-AEOS-0040 已完成。
- 本 ADR 不修改任何既有 Artifact 之 Authority Classification 或 References；不執行引用重錨；不建立後續 Architecture Artifact。

## 2. Decision

> 本節為 Approved 決策內容（`decision-status`：`Approved`）。本決策依 §2.6 生效條件正式生效。

### 2.1 WA-001 Authority Classification

- WA-001 保留為：歷史來源（Historical Source）、背景材料（Background Material）與不可獨立驗證之歷史引用（Non-independently Verifiable Historical Reference）。
- WA-001 MUST NOT 繼續作為 Enterprise Meta-Architecture、Architecture Principles 或其他已經由 Approved AEOS Artifact 承載之事實之正式權威來源。
- MUST NOT 修改、重建、補寫、重新命名或反向生成 WA-001 原文。
- MUST NOT 將後續 AEOS 內容偽裝成 WA-001 之原始內容。
- Repository 不持有 WA-001 原文。
- WA-001 僅作為歷史參考來源。
- Repository 不保證 WA-001 原文之完整性、真實性或權威性。
- Repository 不因引用而構成對 WA-001 原文之擁有或重建。

### 2.2 Approved Fact Authority Baseline

正式事實之權威承載如下；所有權威映射均取自當前 Approved Artifacts，不得自行創造：

| 事實領域 | 正式 Definition／Decision 載體 |
|----------|-------------------------------|
| Enterprise Meta Architecture 之正式定義 | AEOS-ARCH-011 — Enterprise Meta Architecture（Approved 1.0.0） |
| Architecture Principles 之正式定義 | AEOS-ARCH-012 — Architecture Principles（Approved 1.0.0） |
| Architecture Definition Carrier Decision 之正式決策 | AEOS-ADR-001 — Architecture Definition Carrier Decision（Approved 1.0.0；`decision-status`：`Approved`） |
| 其他架構領域之定義 | 依 AEOS-ARCH-001 §8 Architecture Register 已登錄之對應 Approved Architecture Artifact（AEOS-ARCH-004～010） |

- AEOS-ARCH-001 §8 Architecture Register 之角色：記錄 Architecture Artifact（登錄、狀態與版本）；Register 只記錄 Artifact，MUST NOT 取代正式 Definition。
- ADR 之角色：只承載 Decision；ADR MUST NOT 取代 Architecture Definition（依 AEOS-ARCH-003 §1／§6）。
- 正式事實之權威鏈向上追溯至 AEOS-ARCH-001 Architecture Baseline；WA-001 不再作為已由 Approved AEOS Artifact 承載之事實之正式權威來源。

### 2.3 Transition 規則（允許範圍與禁止事項）

允許範圍：

- 後續僅進行最小必要之 Authority Classification 更新與 Reference Re-anchoring（依 §2.4 執行順序）。

禁止事項（MUST NOT）：

- 不得藉 Transition 改寫既有 Architecture Definition。
- 不得把 WA-001 內容反向寫入 AEOS-ARCH-011／012。
- 不得建立新的 Platform、Capability、Ownership、Dependency、Mapping、Placement 或 Matrix。
- 不得開始 Candidate Assessment。
- 不得執行 PC-003。
- 不得處理 Catalog Registration。
- 不得擴張到後續 Architecture Artifact。
- 無法由 Approved Artifact 驗證之 WA-001 內容 MUST NOT 自動遷移為正式事實。
- 遇內容缺口、衝突或語意不確定時，MUST 停止遷移並進入後續獨立評估，MUST NOT 自行補寫。

### 2.4 後續執行順序

1. 核准並使本 Transition ADR 於 `main` 生效；
2. 建立受影響文件與引用之精確清單；
3. 執行最小 Authority Classification 更新；
4. 執行最小 Reference Re-anchoring；
5. 驗證無循環引用、雙重權威或內容漂移；
6. 完成 EWO-AEOS-0040 之 Transition 驗證；
7. Transition 完成後，始可另行判斷 Candidate Assessment、PC-003 或後續 Artifact 是否開始。

### 2.5 後續修改既有 Artifact 之執行邊界

- 本 ADR 生效前：MUST NOT 修改 AEOS-ARCH-001、AEOS-CON-001、AEOS-ARCH-004～010、AEOS-RPT-001／002 之 Authority Classification 或 References；MUST NOT 將 WA-001 內容寫入 AEOS-ARCH-011／012。
- 本 ADR 生效後：對上述文件之修改僅限於 §2.3／§2.4 允許之最小 Authority Classification 更新與最小 Reference Re-anchoring；MUST NOT 改寫既有 Architecture Definition 內容、不得擴張文件 Scope、不得新增未經核准之事實。
- AEOS-CON-001：本 ADR 不授權修改；如後續評估認定必須調整，須另循 Repository Constitution 治理流程（EWO／Review）辦理，不得以 Transition 之名直接修改。
- AEOS-RPT-001／002：不得作為 Definition、Decision 或 Catalog Entry 之替代載體；如 Transition 影響其 Fact Authority 引用，僅可依 §2.4 進行最小 Reference Re-anchoring。
- 任何超出上述邊界之需求 MUST 停止並另立獨立評估或 EWO，不得自行擴張範圍。

### 2.6 生效條件

本 ADR 之決策正式生效至少須依序達成下列條件：

1. 合法之 Decision／Architecture Review（依 AEOS-STD-005 §4，ADR 文件適用 ADR Review，Review Type 為 `AD`）；
2. 所有 Blocking Finding 完成處理；
3. 取得非作者 Reviewer 之 GitHub `APPROVED`；
4. Repository Owner 最終核准；
5. PR 合併至 `main`；
6. 後續獨立 Approval 動作（若 Repository 現行 Lifecycle 流程要求）；
7. Approved 狀態可由 `main` 驗證。

- 本 ADR（`status`：`Approved`；`decision-status`：`Approved`）之 Transition 決策依 §2.6 生效條件正式生效。

## 3. Alternatives

| 替代方案 | 內容 | 未採用理由 |
|----------|------|------------|
| A | 維持 WA-001 為正式 Fact Authority（現況） | WA-001 之完整 Approved 原文不可取得或不可驗證（EWO-AEOS-0039 停止結果），無法作為可解析之正式事實權威；將與 Approved AEOS Artifacts 形成雙重權威 |
| B | 刪除或全面移除 WA-001（自 Register 與 References 移除） | 將遺失歷史來源與追溯依據；WA-001 保留為歷史參考仍具可追溯價值；既有引用應以最小重錨處理而非刪除 |
| C | 重建／反向寫入 WA-001 內容至 AEOS-ARCH-011／012 | 違反「不得重建、轉錄或恢復外部歷史架構來源」之既有治理規則（AEOS-ARCH-011／012 Scope）；有虛構原始內容之風險 |
| D | 本階段即全面執行 Authority Classification 與 Reference Re-anchoring | 未經核准之 ADR 不得執行 Transition；需先定義範圍與順序，再以最小增量執行，避免內容漂移 |

## 4. Consequences

- 正式確立 WA-001 Fact Authority Transition 之決策基準與執行邊界（生效後）。
- 使後續 EWO-AEOS-0040 恢復工作有明確之最小執行順序（§2.4）、禁止事項（§2.3）與停止條件。
- 本階段不改變任何既有 Artifact 之 Authority Classification、References 或內容。
- WA-001 保留為歷史參考，不因本 ADR 而刪除或降版。
- 生效前，Repository 既有引用與 Register 保持不變；生效後僅依核准範圍執行最小重錨。
- 避免循環引用與雙重權威：本 ADR 僅建立「ADR → 既有 Approved Artifact」之引用，不建立反向依賴。

## 5. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | EWO-AEOS-0040 | EWO | 本文件之工作來源與恢復之 EWO |
| REF-002 | EWO-AEOS-0041 | EWO | Blocker 解除前置 EWO（Definition Carrier 決策與 Definition Artifacts 建立） |
| REF-003 | EWO-AEOS-0042 | EWO | Blocker 解除前置 EWO（Definition Artifacts Approval） |
| REF-004 | AD-AEOS-0041-R1、AR-AEOS-0041-R2／R3／R4 | Review Record | 既有可驗證 Review 紀錄（PR 為載體）；Blocker 解除證據 |
| REF-005 | [AEOS-ADR-001 — Architecture Definition Carrier Decision](AEOS-ADR-001-Architecture-Definition-Carrier-Decision.md)（Approved 1.0.0） | ADR | 決定 Definition Carrier；宣告 WA-001 Fact Authority Transition 由本 ADR 處理 |
| REF-006 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.3.0） | Architecture Entry Document | §4 六項組成、§8 Architecture Register、References 與 Revision History |
| REF-007 | [AEOS-ARCH-003 — Architecture Decision Record System](../architecture/AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved 1.0.0） | Architecture | ADR Framework、Template、Lifecycle、Status Model 與 ADR Register |
| REF-008 | [AEOS-ARCH-011 — Enterprise Meta Architecture](../architecture/AEOS-ARCH-011-Enterprise-Meta-Architecture.md)（Approved 1.0.0） | Architecture | Enterprise Meta Architecture 正式定義載體 |
| REF-009 | [AEOS-ARCH-012 — Architecture Principles](../architecture/AEOS-ARCH-012-Architecture-Principles.md)（Approved 1.0.0） | Architecture | Architecture Principles 正式定義載體 |
| REF-010 | [AEOS-ARCH-002 — Enterprise Governance Architecture](../architecture/AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved 1.1.0） | Architecture | Governance Hierarchy（ADR H6、Review Records H7） |
| REF-011 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved 1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-012 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md)（Approved 3.2.0） | Information Architecture | Taxonomy、Lifecycle 與目錄組織 |
| REF-013 | [AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md)（Approved 1.3.0） | Standard | Review Types 與 Review ID 規則（ADR 適用 AD） |
| REF-014 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Architecture Source（外部） | 歷史來源／背景材料；不作為正式 Fact Authority |
| REF-015 | [AEOS-RPT-001 — M5 Catalog／Matrix Readiness Assessment](../reports/AEOS-RPT-001-M5-Catalog-Matrix-Readiness-Assessment.md)（Approved 1.0.0） | Report | 正文 §2.5 執行邊界直接引用之既有 Artifact |
| REF-016 | [AEOS-RPT-002 — Platform Architecture Candidate Assessment](../reports/AEOS-RPT-002-Platform-Architecture-Candidate-Assessment.md)（Draft 0.1.0） | Report | 正文 §2.5 執行邊界直接引用之既有 Artifact |

## 6. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-08 | 依 EWO-AEOS-0040 執行 ADR Lifecycle Completion（ADR Lifecycle Consistency Adjudication：Case A）：status Draft→Approved、version 0.1.0→1.0.0、decision-status Proposed→Approved、記錄 decision-date 2026-08-08（依 AEOS-ARCH-003 §3／§8、AEOS-STD-002 §8／CF-002／OF-03／OF-05）；本 PR 為 AD Review 載體，Review ID 依 AEOS-STD-005 R-003 於 Review 通過後記入 related／Revision History | Codex |
| 0.1.0 | 2026-08-07 | Review Findings 修正（PR #43 Review Report）：F-001 補充 Repository 對 WA-001 原文之立場；F-002 References 補列 AEOS-RPT-001／002，與正文直接引用一致（EWO-AEOS-0040） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：定義 WA-001 Fact Authority Transition 決策（Proposed）——Authority Classification、Approved Fact Authority Baseline、Transition 規則、後續執行順序、執行邊界與生效條件；記錄 EWO-AEOS-0040 Blocker 解除證據（EWO-AEOS-0040） | Codex |
