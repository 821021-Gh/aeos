# AEOS Project State

> 更新日期：2026-08-28
> 用途：提供 Agent 啟動、handoff 與長 Work Session 切換所需的最小狀態；不得取代 EWO、Review、ADR、PR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `da8cf7cfbe8c5ab3fbf02a43285e5f5e1edabafe` |
| Working Branch | `docs/EWO-AEOS-0047-workspace-session-lifecycle-amendments` |
| Current Milestone | Workspace Lifecycle Governance |
| Current EWO／Issue | EWO-AEOS-0047 — AI Workspace / Project / Work Session Lifecycle Governance（Issue #61）— In Progress |
| Current PR | #63 — Workspace / Work Session Lifecycle Amendments — Review |
| Blocker | Final Architecture Review、Standard Review 與 Repository Owner Approval 尚未完成 |
| Next Action | 完成 PR #63 validation 與 AR-AEOS-0047-R1 / SR-AEOS-0047-R1 Review，解決全部 RC 後進入 Owner final approval |

## 本次已完成

- PR #62（Gap Analysis / Amendment Plan）已由 Repository Owner 核准並 squash merge；merge commit `da8cf7cfbe8c5ab3fbf02a43285e5f5e1edabafe`。
- `AEOS-RPT-005 — AI Workspace / Project / Work Session Lifecycle Governance Gap Analysis` 已進入 `main`。
- Amendment Plan 已核准採 Minimum Necessary Change：只 amendment `AEOS-ARCH-010` 與 `AEOS-STD-007`。
- PR #63 已建立，候選文件目前維持 Review 狀態，尚未成為正式 Fact Authority。

## Candidate Amendments（PR #63）

- `AEOS-ARCH-010 — Workspace Architecture`：1.2.0 / Review。
  - 澄清 Formal Enterprise Workspace 與 Operational Workspace / Project / Work Session 邊界。
  - Repository `main` = System of Record / SSOT。
  - Operational Workspace / Project = Active Workspace，非 Fact Authority。
  - Workspace Context 不得凌駕 `main`。
  - Chat / Agent Session = Ephemeral Work Session。
  - Merge + Closure = Knowledge Promotion Point。
  - Archived Work Session = Historical Working Record，非 Fact Authority。
- `AEOS-STD-007 — AI Engineering Context and Token Budget Standard`：1.1.0 / Review。
  - 新 Session 先 Load `main` Baseline，再載入最小必要 Context。
  - Work Session Lifecycle：Create → Load main Baseline → Execute → Validate → Review → PR → Merge → Closure → Archive。
  - 定義 Active / Archive / Delete、Promotion-before-disposal、Session Owner / Handoff、Session Lineage 與 Human + AI Agent Session Hygiene。

## 最近重要決策與工作原則

- `main` 為唯一 current-state Fact Authority；未合併 Branch、PR、Project Instructions、Closure Snapshot、Chat、Archive 或 Cache 都不得凌駕 `main`。
- 新 Work Session MUST 重新取得最新 `main` baseline，不依賴大量 Historical Chat 重建狀態。
- Operational Workspace 應維持少量 Active Working Context；已完成 Session 在 Promotion Check 後應離開 Active Set。
- 未寫入 Repository 的重要 Decision、Evidence、Review、Approval、Validation 與 Closure 不得因 Archive / Delete 遺失。
- Chat / Agent Session Lineage 與 Git Branch / PR / Merge History 必須分離。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- PR #63 未取得正式 Architecture Review、Standard Review 與 Repository Owner final approval 前，`AEOS-ARCH-010` 1.2.0 / `AEOS-STD-007` 1.1.0 僅為 Candidate Review 內容，不得視為已核准基線。
