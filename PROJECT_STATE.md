# AEOS Project State

> 更新日期：2026-08-28
> 用途：提供 Agent 啟動、handoff 與長 Work Session 切換所需的最小狀態；不得取代 EWO、Review、ADR、PR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `11b6bb86cf8b278b8f8f4250ad1f712214b19a59` |
| Working Branch | 無 |
| Current Milestone | Workspace Lifecycle Governance — Completed |
| Current EWO／Issue | EWO-AEOS-0047 — AI Workspace / Project / Work Session Lifecycle Governance（Issue #61）— Closed candidate |
| Current PR | #62 — Merged；#63 — Merged |
| Blocker | 無 |
| Next Action | 下一個 AEOS 工作必須從最新 `main` 重新載入 baseline；不得以本次 Chat / Closure Snapshot 取代 Repository current state |

## 本次已完成

- PR #62（Gap Analysis / Amendment Plan）已 squash merge；merge commit `da8cf7cfbe8c5ab3fbf02a43285e5f5e1edabafe`。
- `AEOS-RPT-005 — AI Workspace / Project / Work Session Lifecycle Governance Gap Analysis` 已進入 `main`。
- `AR-AEOS-0047-R1` 與 `SR-AEOS-0047-R1` 已由獨立 Human Reviewer `yeelightpro` APPROVED。
- Repository Owner 已完成 final approval。
- PR #63 已 squash merge；merge commit `11b6bb86cf8b278b8f8f4250ad1f712214b19a59`。
- 無 unresolved RC / review thread。

## 正式基線

- `AEOS-ARCH-010 — Workspace Architecture`：1.2.0 / Approved。
  - Formal Enterprise Workspace 與 Operational Workspace / Project / Work Session Authority Boundary 已正式建立。
  - Repository `main` = System of Record / SSOT。
  - Operational Workspace / Project = Active Workspace，非 Fact Authority。
  - Workspace Context 不得凌駕 `main`。
  - Chat / Agent Session = Ephemeral Work Session。
  - Merge + Closure = Knowledge Promotion Point。
  - Archived Work Session = Historical Working Record，非 Fact Authority。
- `AEOS-STD-007 — AI Engineering Context and Token Budget Standard`：1.1.0 / Approved。
  - 新 Session 先 Load `main` Baseline，再載入最小必要 Context。
  - Work Session Lifecycle：Create → Load main Baseline → Execute → Validate → Review → PR → Merge → Closure → Archive。
  - Active / Archive / Delete、Promotion-before-disposal、Session Owner / Handoff、Session Lineage、Human + AI Agent Session Hygiene 已正式化。

## Closure / Promotion Status

- Architecture Decision / Boundary：已 promotion 至 `AEOS-ARCH-010` 1.2.0。
- Operating Rules：已 promotion 至 `AEOS-STD-007` 1.1.0。
- Gap Analysis / Amendment Plan：已 promotion 至 `AEOS-RPT-005`。
- Review / Approval Evidence：已保留於 PR #63、`AR-AEOS-0047-R1`、`SR-AEOS-0047-R1` 與 Repository history。
- Merge Evidence：PR #62 / #63 與各自 merge commit 已存在於 `main`。
- Promotion Check：`PROMOTION_COMPLETE`。

## 最近重要決策與工作原則

- `main` 為唯一 current-state Fact Authority；未合併 Branch、PR、Project Instructions、Closure Snapshot、Chat、Archive 或 Cache 都不得凌駕 `main`。
- 新 Work Session MUST 重新取得最新 `main` baseline，不依賴大量 Historical Chat 重建狀態。
- Operational Workspace 應維持少量 Active Working Context；已完成 Session 在 Promotion Check 後應離開 Active Set。
- 未寫入 Repository 的重要 Decision、Evidence、Review、Approval、Validation 與 Closure 不得因 Archive / Delete 遺失。
- Chat / Agent Session Lineage 與 Git Branch / PR / Merge History 必須分離。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。

## Work Session Disposition

EWO-AEOS-0047 已完成 Merge + Closure 的 knowledge promotion。本工作 Session 在本 Closure correction 合併並關閉 Issue #61 後符合 **Archive** 條件；後續若重新討論相同主題，應建立新 Work Session 並先重新讀取最新 `main`，不得把本次歷史 Session 當作 current-state authority。
