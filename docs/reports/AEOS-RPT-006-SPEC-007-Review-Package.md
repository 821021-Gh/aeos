---
doc-id: AEOS-RPT-006
doc-name: AEOS-SPEC-007 Review Package
doc-type: Report
repository: AEOS
version: 0.2.0
status: Candidate
owner: Architecture Owner
created: 2026-09-17
updated: 2026-09-17
work-package: AEOS-ACC-ACP-C1
related:
  - AEOS-SPEC-007
  - AEOS-ACC-ACP-C1
---

# AEOS-RPT-006 — AEOS-SPEC-007 Review Package

## 1. 文件資訊

| 項目 | 內容 |
|------|------|
| 審查文件 | AEOS-SPEC-007 — External Control Plane Integration Contract |
| 文件型別 | Specification |
| 文件狀態 | Candidate 0.2.0 |
| 儲存庫 | AEOS |
| 工作包 | AEOS-ACC-ACP-C1 |
| 工作包授權者 | 系統架構－總控層06 |
| Review Package 版本 | 0.2.0 |

## 2. 工作包參考

本 Review Package 依工作包 **AEOS-ACC-ACP-C1** 建立。

工作包授權範圍：
- 建立 AEOS ↔ ACC（External Control Plane）整合契約
- 定義七個整合互動點的正式契約格式
- 定義資源衝突控制與冪等性契約
- 確保 transport neutrality 與 runtime/agent neutrality
- 建立 Review Package 與 Draft PR

工作包明確排除：
- 程式實作
- Production 操作
- Transport / Protocol 選型
- 任何執行環境或 Agent 產品綁定

## 3. 基線參考

### 3.1 AEOS Baseline

| 項目 | 內容 |
|------|------|
| Repository | 821021-Gh/aeos |
| Branch | `main` |
| HEAD commit | `284f4a780a770501f5ae4c5a9d9bc02f713ae491` |
| HEAD message | `docs: localize Markdown documentation to Traditional Chinese (#83)` |
| 驗證時間 | 2026-09-17 |

### 3.2 ACC Baseline

| 項目 | 內容 |
|------|------|
| Repository | 821021-Gh/ai-control-center |
| Branch | `main` |
| HEAD commit | `604bee64ae91eed76d1baba9fedcb544bb2d08f7` |
| HEAD message | `Merge pull request #60: ACC-ARCH-002 Minimum Orchestration Architecture` |
| 驗證時間 | 2026-09-17 |

### 3.3 依據文件基線

| 文件 | 狀態 | 版本 |
|------|------|------|
| AEOS-ADR-003 | Approved | 1.0.0 |
| AEOS-ADR-004 | Approved | 1.0.0 |
| AEOS-ADR-005 | Approved | 1.0.0 |
| AEOS-ARCH-013 | Approved | — |
| AEOS-SPEC-001 | Approved | 1.0.0 |
| AEOS-SPEC-002 | Approved | 1.0.0 |
| AEOS-SPEC-003 | Approved | 1.0.0 |
| AEOS-SPEC-004 | Approved | 1.0.0 |
| AEOS-SPEC-005 | Approved | 1.0.0 |
| AEOS-SPEC-006 | Approved | 1.0.0 |
| ACC-ADR-001 | Approved | 1.0.0 |
| ACC-ARCH-002 | Candidate | 0.2.0 |

## 4. 缺口分析結果摘要

### 4.1 AEOS 現有契約覆蓋範圍

AEOS SPEC-001 至 SPEC-006 已完整定義：
- Execution Contract 最小欄位（ADR-003 D-07、ARCH-013 §9）
- Task Admission / Authorization / Approval（SPEC-003）
- Runtime Adapter 執行請求邊界（SPEC-003 §13）
- 執行證據契約（SPEC-004）
- 能力/工具發現（SPEC-005）
- 協作追蹤信封（SPEC-002 §10.1）
- 關卡結果契約（SPEC-002 §9.1）
- 路由設定檔（SPEC-001）
- 符合性測試（SPEC-006）

### 4.2 識別之正式缺口

| 缺口 | 說明 | AEOS-SPEC-007 覆蓋 |
|------|------|-------------------|
| G-1 | External Control Plane Integration Contract | §5-§6 |
| G-2 | Human Approval Bidirectional Protocol | §7 |
| G-3 | Cross-system Correlation Model | §9 |
| G-4 | Resource Collision Control | §12 |
| G-5 | Idempotency Contract | §13 |
| G-6 | Transport / Protocol Neutrality | §14 |

### 4.3 文件落地決策

- **決策**：建立 AEOS-SPEC-007（新 SPEC）
- **理由**：現有 SPEC-001~006 定義 AEOS 內部治理邊界。外部整合契約是不同關注點，不應修改現有 SPEC 範圍。
- **權威位置**：AEOS（依 ACC-ADR-001 D-02、AEOS-ADR-003）
- **ACC 側**：reference / adopt，不建立平行規範

## 5. 關鍵設計決策

| # | 決策 | 理由 |
|---|------|------|
| DD-1 | 建立新 SPEC（SPEC-007）而非修改現有 SPEC | 外部整合契約與內部治理契約是不同關注點 |
| DD-2 | 定義 Integration Intent Envelope 作為整合邊界格式 | 隔離 ECP 側格式與 AEOS 內部格式 |
| DD-3 | 七個整合互動點各自獨立定義 | 每個互動點有不同的生命週期與語意 |
| DD-4 | Cross-system Correlation Model 以 ECP correlation_id 為根 | ECP 發起整合，AEOS 內部 trace 綁定至 ECP correlation_id |
| DD-5 | Resource Collision 由 AEOS 偵測與處理 | 治理權威在 AEOS，ECP 不自行解決 |
| DD-6 | Idempotency 由 ECP 提供 key，AEOS 偵測重複 | 標準 idempotency pattern |
| DD-7 | Transport Neutrality 明確不綁定特定 transport | 為後續實作保留彈性 |
| DD-8 | 整合契約不改變 AEOS 治理權威歸屬 | 繼承 ACC-ADR-001 D-03、AEOS-ADR-003 |

## 6. 權威邊界確認

### 6.1 AEOS 保有

| 治理權力 | 來源 |
|----------|------|
| 任務准入 | AEOS-ARCH-013 §6 |
| 授權 | AEOS-ADR-003 D-02 |
| 執行契約 | AEOS-ARCH-013 §9 |
| Runtime/Harness 路由 | AEOS-SPEC-001 |
| 企業執行證據要求 | AEOS-SPEC-004 |
| 撤銷/取消治理 | AEOS-ARCH-013 §6.10-6.12 |
| Resource Collision 處理 | AEOS-SPEC-007 §12 |

### 6.2 ECP 負責

| 責任 | 說明 |
|------|------|
| 操作意圖接收 | ECP 面向使用者的介面 |
| 意圖分類 | Query / Command 初步分流 |
| 專案上下文組裝 | Derived Context，不取得 Fact Authority |
| 核准互動介面 | 呈現核准請求、收集 Owner 決策 |
| 證據呈現 | 顯示 AEOS 證據，不製造證據 |

### 6.3 不變量

- ECP 不得直接建立或簽發企業執行契約。
- ECP 不得直接授權 Runtime/Harness、工具、模型、資料或憑證。
- ECP 不得把自然語言命令視為已核准命令。
- ECP 不得把 Owner 的互動操作視為無限制執行授權。
- ECP 不得繞過 AEOS Agent Control Plane。
- ECP 不得把衍生上下文提升為事實來源。
- ECP 不得降低 fail-closed 控制。

## 7. 已知 TBD / BLOCKED 項目

| # | 項目 | 狀態 | 說明 |
|---|------|------|------|
| TBD-1 | Transport / Protocol 選型 | NOT AUTHORIZED | AEOS-SPEC-007 §14 明確不綁定。需後續工作包授權。 |
| TBD-2 | AEOS Orchestrator service 實作 | NOT AUTHORIZED | 本工作包為文件工作，不含實作。 |
| TBD-3 | AEOS Runtime Adapter 實作 | NOT AUTHORIZED | 本工作包為文件工作，不含實作。 |
| TBD-4 | ECP 側 Adapter 實作 | NOT AUTHORIZED | 需 ACC 側獨立工作包授權。 |
| TBD-5 | 具體 API endpoint / SDK | NOT AUTHORIZED | Transport 選型後才能定義。 |
| TBD-6 | 認證機制細節 | NOT AUTHORIZED | Transport 選型後才能定義。 |

## 8. 未實作確認

本工作包 **明確確認** 以下項目未實作：

- [x] 無任何程式碼變更
- [x] 無任何 API endpoint 實作
- [x] 無任何 service 實作
- [x] 無任何 adapter 實作
- [x] 無任何 SDK / client library 實作
- [x] 無任何 database schema 實作
- [x] 無任何 Production 操作
- [x] 無任何 transport / protocol 選型
- [x] 無任何執行環境或 Agent 產品綁定

本工作包僅包含架構/契約文件工作。

## 9. 與現有 AEOS 規格一致性檢查

### 9.1 不重定義現有邊界

| 現有規格 | 邊界 | SPEC-007 是否重定義 |
|----------|------|-------------------|
| AEOS-SPEC-002 | Collaboration Model、Gate Result、Trace Envelope | 否。引用 `trace_id`、`correlation_id`、`gate_results`。 |
| AEOS-SPEC-003 | Task Admission、Runtime Adapter Request | 否。定義 ECP → AEOS 的整合邊界，AEOS 內部映射由 AEOS 負責。 |
| AEOS-SPEC-004 | Execution Evidence | 否。定義 Evidence Reference Envelope 作為 ECP 側的證據接收格式。 |
| AEOS-SPEC-005 | Capability Discovery | 否。不涉及。 |
| AEOS-SPEC-006 | Conformance Tests | 否。不涉及。 |

### 9.2 引用而不修改

SPEC-007 引用以下現有概念但不修改其定義：
- `trace_id`（SPEC-002 §10.1）
- `correlation_id`（SPEC-002 §10.1）
- `execution_id`（SPEC-003）
- `execution_request_id`（SPEC-003 §13）
- `evidence_id`（SPEC-004 §5）
- Gate Result states（SPEC-002 §9.1）
- Execution outcome states（SPEC-004 §7）

### 9.3 新增概念

SPEC-007 新增以下概念（不與現有概念衝突）：
- Integration Intent Envelope（§5）
- Integration Admission Response（§6）
- Integration Approval Request/Response（§7）
- Integration State Change Envelope（§8）
- Evidence Reference Envelope（§10）
- Terminal State Envelope（§11）
- Resource Collision Control（§12）
- Idempotency Contract（§13）
- Transport Neutrality（§14）

## 10. 自我審查檢查表

### 10.1 格式檢查

- [x] Frontmatter 完整（doc-id, doc-name, doc-type, repository, version, status, owner, created, updated, work-package, authority, related）
- [x] 執行摘要存在
- [x] 文件資訊表格存在
- [x] 章節編號正確
- [x] 繁體中文為主，專有名詞/技術術語/欄位名稱保留英文

### 10.2 內容檢查

- [x] 目的明確
- [x] 範圍明確（在範圍內 / 超出範圍）
- [x] 管理機構完整（權威來源、規則）
- [x] 設計原則明確
- [x] 契約格式定義完整（每個 envelope 的 REQUIRED/OPTIONAL 欄位）
- [x] Fail-closed 規則明確
- [x] 一致性檢查表存在
- [x] 參考文獻完整

### 10.3 權威檢查

- [x] 不重定義 AEOS 內部治理契約
- [x] 不改變 AEOS 治理權威歸屬
- [x] 不綁定特定 transport
- [x] 不綁定特定執行環境或 Agent 產品
- [x] 不授權任何實作
- [x] 明確標示 TBD / BLOCKED 項目

### 10.4 跨 Repository 檢查

- [x] AEOS-SPEC-007 權威位於 AEOS Repository
- [x] ACC-ARCH-002 已更新以引用 AEOS-SPEC-007
- [x] 不建立 ACC 側平行規範
- [x] 權威矩陣明確

### 10.5 安全檢查

- [x] 反繞過不變量繼承自 ACC-ADR-001 D-13
- [x] Fail-closed 規則完整
- [x] Forbidden data rules 引用 AEOS-SPEC-004
- [x] 不降低任何安全控制

## 11. 語言合規確認

本文件與 AEOS-SPEC-007 遵循以下語言規則：

- 敘述性內容使用繁體中文
- 專有名詞保留英文（AEOS、ACC、Agent Control Plane、Execution Contract 等）
- 技術術語保留英文（transport、protocol、envelope、idempotency 等）
- 欄位名稱保留英文（`integration_intent_id`、`trace_id` 等）
- 識別碼保留英文（AEOS-SPEC-007、ACC-ARCH-002 等）

## 12. ACC-ARCH-002 更新摘要

配合 AEOS-SPEC-007 建立，ACC-ARCH-002 已進行以下更新：

| 更新項目 | 變更內容 |
|----------|----------|
| 版本 | 0.1.0 → 0.2.0 |
| Frontmatter related | 新增 AEOS-SPEC-007 |
| §1 狀態 | 新增 v0.2.0 更新說明 |
| §1.1 授權邊界 | D2 更新為 `SPEC CANDIDATE AUTHORIZED` |
| §1.2 決策摘要 | D2 更新為 `SPEC CANDIDATE EXISTS`；D4 更新為 `RESOLVED` |
| §6.1 架構圖 | AEOS boundary 文字更新 |
| §6.6 AEOS Adapter | 狀態更新、整合契約依賴表格新增、解除條件更新 |
| §10 TBD/BLOCKED | T3/T8/T10 更新為反映 SPEC-007 候選存在 |

## 13. 後續工作建議

本工作包完成後，建議後續工作包：

1. **AEOS-SPEC-007 Review & Approval**：完成 Architecture Review，將 SPEC-007 從 Candidate 升級為 Approved。
2. **Transport Selection**：授權 transport / protocol 選型工作包。
3. **AEOS Orchestrator Service**：授權 AEOS 側 orchestrator service 實作。
4. **ACC Adapter Implementation**：授權 ACC 側 Adapter 實作（依賴 SPEC-007 Approved + Transport 選型）。
5. **Integration Testing**：授權端到端整合測試。

## 14. 狀態與核准

本 Review Package 為 **Candidate 0.2.0**。

R1 Review 已完成，識別 2 項 Minor Finding，已於 R2 全部修正。待 AEOS-SPEC-007 Architecture Review 完成後更新。

## 15. R1 Review 發現與 R2 修正

### 15.1 R1 Review 參考

| 項目 | 內容 |
|------|------|
| Review 輪次 | R1 |
| Reviewed HEAD | `2f2e890cde4cb6bf14665d2260e892e1afc8620d` |
| Review PR | #85 |
| Review 結果 | 2 項 Minor Finding |

### 15.2 R1 發現

| # | 嚴重度 | 發現 | 說明 |
|---|--------|------|------|
| F-001 | Minor | AEOS-ADR-004 未列入 frontmatter authority 與依據文件 | AEOS-ADR-004（Productization Boundary）在 §3 管理機構中已作為權威來源引用，但 frontmatter `authority` 與文件資訊表「依據文件」均未列入，僅出現在 `related` 與「關聯文件」。 |
| F-002 | Minor | §11.2 缺少 AEOS-SPEC-004 部分 outcome 的終止狀態映射 | AEOS-SPEC-004 定義 `timed_out`、`rejected`、`unsupported` 三種 outcome，但 §11.2 Terminal States 僅映射 `completed`、`failed`、`cancelled`，缺少前述三種 outcome 至 `FAILED` 的明確映射。 |

### 15.3 R2 修正內容

工作包：AEOS-ACC-ACP-C1-R2

| Finding | 修正內容 | 影響檔案 |
|---------|----------|----------|
| F-001 | Frontmatter `authority` 新增 AEOS-ADR-004；文件資訊表「依據文件」新增 AEOS-ADR-004（Approved 1.0.0）；「關聯文件」移除 AEOS-ADR-004 | AEOS-SPEC-007 |
| F-002 | §11.2 Terminal States 表格 `FAILED` 行擴展為 `failed`、`timed_out`、`rejected`、`unsupported`；新增完整 AEOS-SPEC-004 outcome → AEOS-SPEC-007 終止狀態映射表 | AEOS-SPEC-007 |

### 15.4 版本治理

依 AEOS-CON-001「版本依 SemVer 管理：Review 修正更新 minor」，AEOS-SPEC-007 版本由 0.1.0 更新至 0.2.0。AEOS-RPT-006 同步更新至 0.2.0。

### 15.5 R2 基線參考

| 項目 | 內容 |
|------|------|
| R2 HEAD | 待 commit |
| R2 Branch | `aeos-acc-acp-c1/integration-contract` |
| R1 Reviewed HEAD | `2f2e890cde4cb6bf14665d2260e892e1afc8620d` |
| R1 vs R2 diff | F-001（authority 提升）、F-002（§11.2 映射補齊）、版本號更新 |

## 16. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-09-17 | 建立 AEOS-SPEC-007 Review Package，涵蓋文件資訊、工作包參考、基線參考、缺口分析摘要、設計決策、權威邊界確認、TBD/BLOCKED 項目、未實作確認、一致性檢查、自我審查檢查表 | Codex |
| 0.2.0 | 2026-09-17 | R1 Review 修正：新增 §15 記錄 F-001（AEOS-ADR-004 authority 提升）與 F-002（§11.2 SPEC-004 outcome 映射補齊）之發現與修正；版本同步更新至 0.2.0 | Codex |
