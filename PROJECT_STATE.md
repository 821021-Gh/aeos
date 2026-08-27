# AEOS Project State

> 更新日期：2026-08-27
> 用途：提供 Agent 啟動與長對話切換所需的最小狀態；不得取代 EWO、Review、ADR 或正式治理文件。

| 項目 | 目前狀態 |
|------|----------|
| Repository | AEOS |
| Source of Truth | `main` |
| Baseline HEAD | `6984a9820922d8b056f69d2cea1d8d4acbe04d46` |
| Working Branch | `agent/ewo-aeos-0046-local-first-agent-execution-routing-profile` |
| Current Milestone | Enterprise Local-first AI Agent Execution Architecture |
| Current EWO／Issue | EWO-AEOS-0046 — Local-first AI Agent Execution Routing Profile（Issue #59）— Open |
| Current PR | #60 — Draft Review Package |
| Blocker | 無 |
| Next Action | Review EWO-AEOS-0046 Draft package；確認 AEOS-SPEC-001 是否足以作為 Local-first routing profile，或是否需後續 AEOS-ARCH-013 amendment / ADR |

## 目前 Draft 交付

- `AEOS-RPT-004 — Local-first AI Agent Execution Architecture Gap Analysis`：Draft 0.1.0。
- `AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile`：Draft 0.1.0。
- EWO-AEOS-0046 已開立為 Issue #59。
- PR #60 已開立為 Draft Review Package。
- PR #58 為初始草稿，已由 #60 取代並關閉。

## 已完成交付

- `AEOS-ADR-004 — Productization Boundary Decision`：Approved 1.0.0。
- `AEOS-ARCH-014 — Productizable Platform Architecture`：Approved 1.0.0。
- `AEOS-ARCH-001 — Architecture Baseline`：1.5.0，登錄 AEOS-ARCH-014 為 Approved Architecture。
- 四層 Productization Model 已成為正式架構基線：Company-specific Reference Implementation、Reusable Enterprise Capability、Platform Core、Commercial Product / Solution Packaging。
- Platform Core MUST NOT 依賴 company-specific schema、workflow、brand、channel 或 single-customer integration。
- Internal capability 需經 Evidence → Decoupling → Contract → Adapter / Configuration Isolation → Portability Validation → Architecture Review → Catalog / Baseline Admission，才可提升為 reusable / platform 候選。
- Internal production system 被定義為 reference implementation，不自動等同 future commercial product。
- AEOS 保持 Enterprise Architecture / Governance authority，不強制等同單一 SaaS 商品。

## 已完成基線

- `AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision`：Approved 1.0.0。
- `AEOS-ARCH-013 — Enterprise AI Agent Architecture`：Approved 1.0.0。
- `AEOS-ADR-004 — Productization Boundary Decision`：Approved 1.0.0。
- `AEOS-ARCH-014 — Productizable Platform Architecture`：Approved 1.0.0。
- `AEOS-ARCH-001 — Architecture Baseline`：1.5.0。
- EWO-AEOS-0044 已 Closed。
- EWO-AEOS-0045 已 Closed。
- PR #55 已 Merged，merge commit `3409a16a5138808e27f10f8ceccca7dec26efbf3`。
- PR #57 已 Merged，merge commit `6984a9820922d8b056f69d2cea1d8d4acbe04d46`。

## 最近重要決策與工作原則

- `main` 為唯一事實來源；不得因 repository default branch 或工作分支狀態改變正式基線判斷。
- Repository 作為工程記憶的權威來源；對話只承載當前意圖、決策與 Delta。
- Context 預設按需載入，不得以整個 Repository 或完整歷史對話作為啟動 Context。
- Markdown 與敘述性文件原則上使用繁體中文；必要英文技術內容保留原名。
- 正式治理內容仍依 AEOS Governance Workflow、EWO、Review、ADR 與 `main` 基線生效。
- Local-first execution 降低成本與延遲，不降低 command approval、repository protection、authorization、audit 或 human approval boundary。
- AEOS core MUST remain runtime / harness / provider / model neutral；具名 local LLM、cloud model、runtime、harness 或 provider 只能位於 adapter、provider、reference implementation 或 PoC 邊界。
