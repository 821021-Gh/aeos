# AEOS Project State

> 更新日期：2026-08-27
> 用途：提供 Agent 啟動與長對話切換所需的最小狀態；不得取代 EWO、Review、ADR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `26aad75af9fa6e58ea9e30f228d55252cb909c15` |
| Working Branch | 無 |
| Current Milestone | Enterprise AI Agent Architecture Foundation |
| Current EWO／Issue | EWO-AEOS-0044 — Enterprise AI Agent Architecture Foundation（Issue #51）— Closed |
| Current PR | #52 — Merged |
| Blocker | 無 |
| Next Action | 開始下一個 AEOS Agent Architecture EWO |

## 已完成交付

- `AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision`：Approved 1.0.0。
- `AEOS-ARCH-013 — Enterprise AI Agent Architecture`：Approved 1.0.0。
- `AEOS-ARCH-001 — Architecture Baseline`：已登錄 AEOS-ARCH-013 為 Approved Architecture。
- 核心方向：Enterprise governance authority 與 Agent Execution Plane 邏輯分離；Runtime Neutral、Harness Neutral；透過版本化 Execution Contract 與 Provider Adapter Boundary 解耦。
- Agent Harness / Orchestration implementation 可替換、並存或組合，並可接受 delegated execution control，但不得因此取得 Enterprise governance authority。
- Composite Harness / Runtime Chain 必須維持 approval、authorization、scope、revocation 與 audit evidence continuity。
- AEOS 不預先指定任何具名 implementation 必須擔任 supervisor、runtime、model router 或其他唯一角色。
- 本輪不新增任何具名 Platform、Agent framework、Harness、runtime、model provider、memory provider 或 tool product 為架構必要元件。
- EWO-AEOS-0044 已完成 closure；後續 Agent Architecture 工作應以 `main` 上 Approved ADR / Architecture / Register 為基線。

## 最近重要決策與工作原則

- `main` 為唯一事實來源；不得因 repository default branch 或工作分支狀態改變正式基線判斷。
- Repository 作為工程記憶的權威來源；對話只承載當前意圖、決策與 Delta。
- Context 預設按需載入，不得以整個 Repository 或完整歷史對話作為啟動 Context。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- 正式治理內容仍依 AEOS Governance Workflow、EWO、Review、ADR 與 `main` 基線生效。
