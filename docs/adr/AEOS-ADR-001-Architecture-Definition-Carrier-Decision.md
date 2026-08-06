---
doc-id: AEOS-ADR-001
doc-name: Architecture Definition Carrier Decision
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
decision-status: Approved
decision-owner: Architecture Owner
decision-date: 2026-08-06
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-06
related:
  - EWO-AEOS-0041
  - EWO-AEOS-0042
  - AD-AEOS-0041-R1
  - AEOS-ARCH-001
  - AEOS-ARCH-003
  - AEOS-ARCH-004
  - AEOS-ARCH-006
---

# AEOS-ADR-001 — Architecture Definition Carrier Decision

> EWO-AEOS-0041：記錄「Enterprise Meta Architecture 與 Architecture Principles 保留為正式架構組成，並分別以獨立 Architecture Definition Artifact 承載」之架構決策。本 ADR 僅決定 Definition Carrier，不處理 WA-001 Fact Authority Transition（由 EWO-AEOS-0040 之獨立 ADR 處理）。

## Executive Summary

AEOS-ARCH-001 §4 所列六項架構組成中，Enterprise Meta Architecture 與 Architecture Principles 缺少可解析之 Approved Architecture Definition。本 ADR 決定：該兩項組成**保留為 AEOS Architecture Baseline 之獨立組成**，並**分別以獨立 Architecture Definition Artifact（AEOS-ARCH-011、AEOS-ARCH-012）承載**；不由 AEOS-ARCH-004、AEOS-ARCH-006 或其他現有 Artifact 擴張 Scope 兼任；不因 WA-001 無法驗證而刪除、合併或重新命名。本 ADR 不決定 WA-001 之撤銷、降級或轉換（屬 EWO-AEOS-0040 之獨立 ADR 範圍）。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ADR-001 |
| 文件名稱 | Architecture Definition Carrier Decision |
| 型別 | ADR（Architecture Decision Record） |
| 狀態 | Approved |
| 版本 | 1.0.0 |
| Decision Status | Approved |
| Decision Owner | Architecture Owner |
| Decision Date | 2026-08-06 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-06 |
| 依據文件 | EWO-AEOS-0041、EWO-AEOS-0042、AD-AEOS-0041-R1、AEOS-ARCH-003（ADR System）、AEOS-ARCH-001（Architecture Baseline） |
| 關聯文件 | EWO-AEOS-0042、AD-AEOS-0041-R1、AEOS-ARCH-001、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-006 |

## 1. Context

- AEOS-ARCH-001 §4 將六項架構組成列入 Architecture Baseline：Workspace Architecture、Enterprise Meta Architecture、Platform Topology、Capability Architecture、Capability Ownership、Architecture Principles。
- EWO-AEOS-0039 停止結果：WA-001 之完整 Approved 原文不可取得或不可驗證；不得以其作為可解析之 Architecture Fact Authority。
- EWO-AEOS-0040 六組成審計結果：六項組成中，Enterprise Meta Architecture（§4.2）與 Architecture Principles（§4.6）**缺少對應之 Approved Architecture Definition**——現行 Approved Artifacts 中無任何文件以該組成名稱為其定義範圍。
- **EWO-AEOS-0040 之正式 Lifecycle Status：In Progress**（依 YEOS EWO-SYS-001 §4 Lifecycle；現行 Lifecycle 無 Paused／Blocked／Stopped 正式狀態）。**Blocker／Status Reason**：AEOS-ARCH-001 §4 中 Enterprise Meta Architecture 與 Architecture Principles 缺少可解析之 Approved Architecture Definition；EWO-AEOS-0040 必須等待 EWO-AEOS-0041 完成 Review、合併並達成生效條件後才能恢復。
- 本 ADR 之決策需求：決定該兩項組成之 Definition 承載方式，使 AEOS-ARCH-001 §4 基線獲得可解析之定義載體。
- 決策邊界：本 ADR 不決定 WA-001 Fact Authority 之撤銷、降級或轉換；不決定 WA-001 引用重錨；不決定新 Architecture Baseline Authority Model；不處理 Platform Candidate Assessment、PC-003、Catalog Registration 或 Platform Entry。

## 2. Decision

**AEOS 將 Enterprise Meta Architecture 與 Architecture Principles 保留為正式架構組成，並分別以獨立 Architecture Definition Artifact 承載（AEOS-ARCH-011、AEOS-ARCH-012）。**

決策內容：

- 兩項組成 MUST 保留為 AEOS Architecture Baseline 之獨立組成（不刪除、不合併、不重新命名）。
- 兩項組成各自使用獨立 Architecture Definition Artifact 承載：AEOS-ARCH-011 — Enterprise Meta Architecture；AEOS-ARCH-012 — Architecture Principles。
- 不由 AEOS-ARCH-004、AEOS-ARCH-006 或其他現有 Artifact 擴張 Scope 後兼任該兩項 Definition。
- 不因 WA-001 無法驗證而刪除、合併或重新命名該兩項組成。
- 本決策僅授權 Definition Carrier 之建立；不授權新增或修改 Architecture Facts 以外之內容（新 Definition 之架構事實須依其各別 EWO 範圍與 Review 核准）。

## 3. Alternatives

| 替代方案 | 內容 | 未採用理由 |
|----------|------|------------|
| A | 由 AEOS-ARCH-004（Overview）或 AEOS-ARCH-006（Layer）擴張 Scope 兼任 Definition | 將造成文件 Scope 膨脹、Overview／Layer 定位混淆，且有「把 Overview 誤當 Meta、把特性誤當原則」之風險 |
| B | 刪除或合併缺少 Definition 之組成 | 將造成 Architecture Baseline 組成不完整、已採納架構事實流失 |
| C | 以 Report、Index 或 Catalog 承載 Definition | 違反「不得以非 Architecture Definition 取代 Architecture Definition」之治理原則 |

## 4. Consequences

- 建立 AEOS-ARCH-011 與 AEOS-ARCH-012 兩份獨立 Architecture Definition Artifact。
- 同步更新 AEOS-ARCH-001 §8 Architecture Register、References 與 Revision History。
- 解除 EWO-AEOS-0040 之 Blocker 之前置條件（須待本 EWO 完成 Review、合併並達成生效條件後，EWO-AEOS-0040 方恢復）。
- 本 ADR 不改變 WA-001 之權威地位；WA-001 Fact Authority Transition 由 EWO-AEOS-0040 恢復後之獨立 ADR 處理。
- 新 Definition 之生效條件：本 ADR 通過適用 Review、AEOS-ARCH-011／012 通過 Architecture Review、Repository 驗證通過、PR 合併至 main，並經後續核准 EWO 使其 Lifecycle 成為 Approved 後，始為有效基線。

## 5. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.0.0） | Architecture Entry Document | §4 六項組成與 §8 Register |
| REF-002 | [AEOS-ARCH-003 — Architecture Decision Record System](../architecture/AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved 1.0.0） | Architecture | ADR Framework 與 Template |
| REF-003 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](../architecture/AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.0.0） | Architecture | 現有 Artifact 之定位（不兼任 Definition） |
| REF-004 | [AEOS-ARCH-006 — Layer Architecture](../architecture/AEOS-ARCH-006-Layer-Architecture.md)（Approved 1.0.0） | Architecture | 現有 Artifact 之定位（不兼任 Definition） |
| REF-005 | [AEOS-ARCH-011 — Enterprise Meta Architecture](../architecture/AEOS-ARCH-011-Enterprise-Meta-Architecture.md)（本 EWO 建立，Draft） | Architecture | 本決策所建立之 Definition 之一 |
| REF-006 | [AEOS-ARCH-012 — Architecture Principles](../architecture/AEOS-ARCH-012-Architecture-Principles.md)（本 EWO 建立，Draft） | Architecture | 本決策所建立之 Definition 之二 |
| REF-007 | EWO-AEOS-0041 | EWO | 本文件之工作來源 |

## 6. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-06 | Lifecycle Approval（EWO-AEOS-0042）：經 AD-AEOS-0041-R1（Reviewer APPROVED）與 PR #41 合併後，文件狀態更新為 Approved 1.0.0；Decision Status 更新為 Approved（決策已核准） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：記錄 Definition Carrier Decision——Enterprise Meta Architecture 與 Architecture Principles 保留為獨立組成並分別以 AEOS-ARCH-011／AEOS-ARCH-012 承載（EWO-AEOS-0041；AD-AEOS-0041-R1） | Codex |
