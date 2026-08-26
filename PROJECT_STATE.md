# AEOS Project State

> 更新日期：2026-08-26
> 用途：提供 Agent 啟動與長對話切換所需的最小狀態；不得取代 EWO、Review、ADR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `f1cedc5209c9bb4a0840e5988fe399ac216dd240` |
| Working Branch | `agent/ewo-aeos-0044-enterprise-ai-agent-architecture` |
| Current Milestone | Enterprise AI Agent Architecture Foundation |
| Current EWO／Issue | EWO-AEOS-0044 — Enterprise AI Agent Architecture Foundation（Issue #51） |
| Current PR | 待建立 Draft PR |
| Blocker | 無 |
| Next Action | 建立 Review Package，執行 Architecture / ADR Review |

## 目前候選交付

- `AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision`：Proposed 0.1.0。
- `AEOS-ARCH-013 — Enterprise AI Agent Architecture`：Draft 0.1.0。
- 核心方向：Agent Control Plane 與 Runtime Plane 邏輯分離；Runtime Neutral；透過版本化 Execution Contract 與 Provider Adapter Boundary 解耦。
- 本輪不新增任何具名 Platform、Agent framework、runtime、model provider、memory provider 或 tool product 為架構必要元件。
- Draft / Proposed 尚未納入 `AEOS-ARCH-001` Approved Architecture Register；僅在完成 Review 與 Approval 後執行 baseline amendment。

## 最近重要決策與工作原則

- `main` 為唯一事實來源；不得因 repository default branch 或工作分支狀態改變正式基線判斷。
- Repository 作為工程記憶的權威來源；對話只承載當前意圖、決策與 Delta。
- Context 預設按需載入，不得以整個 Repository 或完整歷史對話作為啟動 Context。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- 正式治理內容仍依 AEOS Governance Workflow、EWO、Review、ADR 與 `main` 基線生效。
