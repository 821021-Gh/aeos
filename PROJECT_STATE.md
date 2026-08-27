# AEOS Project State

> 更新日期：2026-08-27
> 用途：提供 Agent 啟動與長對話切換所需的最小狀態；不得取代 EWO、Review、ADR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `563430f576c4bf27e56b48e3db4f4851f6a2358c` |
| Working Branch | `agent/ewo-aeos-0045-productizable-platform-boundary` |
| Current Milestone | Productizable Platform Architecture |
| Current EWO／Issue | EWO-AEOS-0045 — Productizable Platform Boundary（Issue #54） |
| Current PR | #55 — Draft Review Package |
| Blocker | 無 |
| Next Action | 執行 Architecture / ADR / Governance Review |

## 目前候選交付

- `AEOS-ADR-004 — Productization Boundary Decision`：Proposed 0.1.0。
- `AEOS-ARCH-014 — Productizable Platform Architecture`：Draft 0.1.0。
- 四層 Productization Model：Company-specific Reference Implementation、Reusable Enterprise Capability、Platform Core、Commercial Product / Solution Packaging。
- Platform Core MUST NOT 依賴 company-specific schema、workflow、brand、channel 或 single-customer integration。
- Internal capability 需經 Evidence → Decoupling → Contract → Adapter / Configuration Isolation → Portability Validation → Architecture Review → Catalog / Baseline Admission，才可提升為 reusable / platform 候選。
- Internal production system 被定義為 reference implementation，不自動等同 future commercial product。
- AEOS 保持 Enterprise Architecture / Governance authority，不強制等同單一 SaaS 商品。

## 已完成基線

- `AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision`：Approved 1.0.0。
- `AEOS-ARCH-013 — Enterprise AI Agent Architecture`：Approved 1.0.0。
- `AEOS-ARCH-001 — Architecture Baseline`：1.4.0，已登錄 AEOS-ARCH-013。
- EWO-AEOS-0044 已 Closed。

## 最近重要決策與工作原則

- `main` 為唯一事實來源；不得因 repository default branch 或工作分支狀態改變正式基線判斷。
- Repository 作為工程記憶的權威來源；對話只承載當前意圖、決策與 Delta。
- Context 預設按需載入，不得以整個 Repository 或完整歷史對話作為啟動 Context。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- 正式治理內容仍依 AEOS Governance Workflow、EWO、Review、ADR 與 `main` 基線生效。
