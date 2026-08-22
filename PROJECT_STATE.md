# AEOS Project State

> 更新日期：2026-08-22
> 用途：提供 Agent 啟動與長對話切換所需的最小狀態；不得取代 EWO、Review、ADR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Branch | `agent/ewo-aeos-0043-lifecycle-correction` |
| Current Milestone | Governance Content 增量 |
| Current EWO／Issue | EWO-AEOS-0043 — AI Engineering Context and Token Budget Standard |
| Current PR | #50 — Ready（PR #49 已合併） |
| Blocker | 無 |
| Next Action | Repository Owner 確認並合併 PR #50 |

## 最近重要決策

- Repository 作為工程記憶的權威來源；對話只承載當前意圖、決策與 Delta。
- Context 預設按需載入，不得以整個 Repository 或完整歷史對話作為啟動 Context。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- 正式治理內容仍依 AEOS Governance Workflow、EWO、Review 與 main 基線生效。
- PR #49 已在 Review 完成前合併；SR-AEOS-0043-R1 與 GR-AEOS-0043-R1 均為 REQUEST CHANGES，須以修正 PR 完成生命週期收斂。
- PR #50 已完成 SR-AEOS-0043-R2 與 GR-AEOS-0043-R2；兩份 Re-review 決策均為 APPROVED。
