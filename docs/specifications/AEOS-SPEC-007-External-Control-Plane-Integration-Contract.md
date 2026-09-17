---
doc-id: AEOS-SPEC-007
doc-name: External Control Plane Integration Contract
doc-type: Specification
repository: AEOS
version: 0.2.0
status: Candidate
owner: Architecture Owner
created: 2026-09-17
updated: 2026-09-17
work-package: AEOS-ACC-ACP-C1
review-pr: TBD
review-head: TBD
review-result: TBD
authority:
  - AEOS-ADR-003
  - AEOS-ADR-004
  - AEOS-ADR-005
  - AEOS-ARCH-013
related:
  - AEOS-ACC-ACP-C1
  - AEOS-ADR-003
  - AEOS-ADR-004
  - AEOS-ADR-005
  - AEOS-ARCH-013
  - AEOS-SPEC-001
  - AEOS-SPEC-002
  - AEOS-SPEC-003
  - AEOS-SPEC-004
  - AEOS-SPEC-005
  - AEOS-SPEC-006
  - ACC-ADR-001
  - ACC-ARCH-002
---

# AEOS-SPEC-007 — External Control Plane Integration Contract

## 執行摘要

本規格定義外部控制平面（External Control Plane）與 AEOS Agent Control Plane 之間的整合契約。

外部控制平面是獨立的系統，提供面向使用者的操作介面，並需要將執行意圖提交至 AEOS Agent Control Plane 進行治理。外部控制平面不等於 Agent Control Plane，不擁有治理權威，不自行建立執行契約。

本規格定義七個整合互動點的正式契約：Intent Submission、Admission / Authorization Response、Human Approval Escalation Protocol、State Reporting Protocol、Cross-system Correlation Model、Evidence Ingestion Boundary、Terminal State Protocol。同時定義 Resource Collision Control、Idempotency Contract 與 Transport Neutrality。

本文件不實作任何服務、API、protocol adapter 或程式碼。不選定任何執行環境、供應商、模型、工具或 transport。不授權 Production 操作。不建立 marketplace 或商業包裝。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-007 |
| 文件名稱 | External Control Plane Integration Contract |
| 型別 | Specification |
| 狀態 | Candidate 0.2.0 |
| 儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-17 |
| 最後更新 | 2026-09-17 |
| 工作包 | AEOS-ACC-ACP-C1 |
| 依據文件 | AEOS-ACC-ACP-C1、AEOS-ADR-003（Approved 1.0.0）、AEOS-ADR-004（Approved 1.0.0）、AEOS-ADR-005（Approved 1.0.0）、AEOS-ARCH-013（Approved）、AEOS-SPEC-002（Approved 1.0.0）、AEOS-SPEC-003（Approved 1.0.0）、AEOS-SPEC-004（Approved 1.0.0） |
| 關聯文件 | AEOS-SPEC-001、AEOS-SPEC-005、AEOS-SPEC-006、ACC-ADR-001、ACC-ARCH-002 |

## 1. 目的

本規格目的為：

- 定義外部控制平面與 AEOS Agent Control Plane 之間的整合契約邊界。
- 定義七個整合互動點的正式契約格式與語意。
- 定義跨系統關聯模型與 ID 傳播規則。
- 定義資源衝突控制與冪等性契約。
- 確保整合契約不綁定特定 transport 或 protocol。
- 確保外部控制平面不因整合而取得 AEOS 治理權威。
- 提供可重用的整合契約模式，供多個外部控制平面採用。

本文件不重新定義 AEOS 內部治理契約。AEOS-SPEC-002 至 AEOS-SPEC-006 的內部邊界保持不變。本文件僅定義「外部如何接入 AEOS 治理邊界」。

## 2. 範圍

### 2.1 在範圍內

- 外部控制平面整合邊界定義。
- Integration Intent Envelope（ACC Execution Intent → AEOS Task Admission 映射）。
- Integration Admission Response（AEOS → 外部控制平面的准入/授權結果）。
- Human Approval Escalation Protocol（雙向核准傳播）。
- State Reporting Protocol（狀態變更通知）。
- Cross-system Correlation Model（跨系統 ID 關聯）。
- Evidence Ingestion Boundary（執行證據 → 外部控制平面證據映射）。
- Terminal State Protocol（完成/失敗/取消/撤銷傳播）。
- Resource Collision Control（跨 agent 資源衝突）。
- Idempotency Contract（意圖提交冪等語意）。
- Transport Neutrality（不綁定特定 transport）。
- Authority Matrix（跨 Repository 權威位置）。
- Fail-closed Invariants（整合層不變量）。

### 2.2 超出範圍

- AEOS 內部治理契約重定義（由 AEOS-SPEC-002 至 AEOS-SPEC-006 定義）。
- 外部控制平面內部元件設計（由各外部控制平面自行定義）。
- Transport / Protocol 實作（API、SDK、message broker、event bus）。
- Runtime Adapter 實作（由 AEOS-SPEC-003、AEOS-SPEC-004 定義）。
- 任何命名的執行環境、供應商、模型、工具、Harness 或工作流程引擎選擇。
- 服務端點、網路位址、認證機制或部署拓撲。
- Production 部署、啟用、操作或監控。
- 下游產品採用或商業包裝（由 AEOS-ADR-004 定義）。
- CRM、RBOS、YEOS 或其他產品/系統內部邏輯。

## 3. 管理機構

| 權威 | 角色 |
|------|------|
| AEOS-ADR-003（Approved 1.0.0） | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ADR-005（Approved 1.0.0） | AEOS 擁有 runtime-neutral Agent Collaboration 治理 |
| AEOS-ADR-004（Approved 1.0.0） | Productization Boundary — 整合契約屬於 Reusable Enterprise Capability / Platform Core 層級 |
| AEOS-ARCH-013（Approved） | Enterprise AI Agent Architecture、Control Plane 責任、Execution Contract、Runtime/Harness 邊界 |
| AEOS-SPEC-002（Approved 1.0.0） | Agent Collaboration Model、Quality Gate Result Contract、Collaboration Trace Envelope |
| AEOS-SPEC-003（Approved 1.0.0） | Control Plane Orchestration、Task Admission、Runtime Adapter Execution Request Boundary |
| AEOS-SPEC-004（Approved 1.0.0） | Runtime Adapter Execution Evidence Contract |

規則：

- 外部控制平面是 AEOS Agent Control Plane 的整合消費者，不是治理共同擁有者。
- 整合契約定義接入邊界，不重新定義 AEOS 內部治理。
- AEOS Agent Control Plane 保有全部 17 項治理權力（依 ACC-ADR-001 D-02 列舉）。
- 外部控制平面 MAY 呈現治理結果、收集人工核准、傳播決策，但 SHALL NOT 成為治理權威。
- 整合契約 MUST 維持 transport neutrality。
- 整合契約 MUST 維持 runtime / agent neutrality。

## 4. 整合邊界原則

### 4.1 外部控制平面定義

外部控制平面（External Control Plane, ECP）是一個獨立的系統，具備以下特徵：

- 提供面向使用者（Owner / Operator）的操作介面。
- 接收自然語言或結構化的操作意圖。
- 需要將執行意圖提交至 AEOS Agent Control Plane 進行治理。
- 不擁有 AEOS Agent Control Plane 的治理權威。
- 可能擁有自己的專案上下文、對話管理、證據呈現與核准互動介面。

目前唯一已識別的外部控制平面為 AI Control Center（ACC）。本規格設計為可重用模式，不限於 ACC。

### 4.2 控制介面不等於 Agent Control Plane

繼承 ACC-ADR-001 D-03 與 AEOS-ADR-003：

**外部控制平面 ≠ Agent Control Plane**

整合契約的存在不改變治理權威歸屬。外部控制平面 MAY 作為 Agent Control Plane 面向使用者的主要操作入口，但介面的存在不得被解讀為治理權的移轉。

### 4.3 Transport Neutrality

整合契約 MUST NOT 綁定特定 transport 或 protocol。

| 候選 Transport | 狀態 |
|----------------|------|
| HTTP REST API | NOT SELECTED |
| gRPC | NOT SELECTED |
| Event Bus / Message Broker | NOT SELECTED |
| Shared Library | NOT SELECTED |
| File-based | NOT SELECTED |

整合契約定義訊息語意與契約格式。Transport 選型由後續實作工作包決定，不得反向影響契約語意。

### 4.4 Runtime / Agent Neutrality

整合契約 MUST NOT 綁定特定執行環境、Agent 產品、模型或工具。

- 不得在契約中硬編碼 Qoder、Codex、OpenClaw 或任何其他產品名稱。
- 不得假定特定 agent dispatch protocol。
- 不得假定特定 agent capability surface。

### 4.5 Authority Preservation

整合契約 MUST 維持以下權威分離：

| 責任 | 權威位置 |
|------|----------|
| 操作意圖接收 | ECP |
| 意圖分類（Query / Command） | ECP |
| 專案上下文組裝 | ECP（Derived Context） |
| 核准互動介面 | ECP |
| 任務准入 | AEOS Agent Control Plane |
| 授權 | AEOS Agent Control Plane |
| 執行契約 | AEOS Agent Control Plane |
| Runtime / Harness 路由 | AEOS Agent Control Plane |
| 企業執行證據要求 | AEOS Agent Control Plane |
| 撤銷 / 取消治理 | AEOS Agent Control Plane |

## 5. Intent Submission Contract

### 5.1 目的

定義外部控制平面提交執行意圖至 AEOS Agent Control Plane 的契約格式。

### 5.2 Integration Intent Envelope

外部控制平面提交意圖時，MUST 使用 Integration Intent Envelope。此 envelope 是 ECP 側意圖與 AEOS Task Admission 之間的整合邊界格式。

| 欄位 | 要求 | 說明 |
|------|------|------|
| `integration_intent_id` | REQUIRED | 整合意圖的穩定識別。由 ECP 生成。 |
| `intent_idempotency_key` | REQUIRED | 冪等性金鑰。相同 key 的重複提交 MUST 被 AEOS 識別為同一意圖。由 ECP 生成。 |
| `external_control_plane_id` | REQUIRED | 提交意圖的外部控制平面識別。 |
| `external_correlation_id` | REQUIRED | ECP 側的關聯識別。用於跨系統追溯。 |
| `submitter_identity` | REQUIRED | 提交者身分識別。MUST 為已認證身分。 |
| `target_repository_ref` | REQUIRED | 目標 Repository 識別。 |
| `target_branch_ref` | OPTIONAL | 目標 Branch 參考。 |
| `instruction` | REQUIRED | 自然語言指令。 |
| `intent_constraints` | REQUIRED | 意圖約束。包含 forbidden_actions、timeout 等。 |
| `evidence_requirements` | REQUIRED | 證據要求。包含 minimum_completeness、required_checks 等。 |
| `human_approval_state` | REQUIRED | 人工核准狀態。初始值 MUST 為 `PENDING`。 |
| `project_context_ref` | OPTIONAL | ECP 側專案上下文參考。 |
| `intent_metadata` | OPTIONAL | ECP 特定的額外中繼資料。MUST 放入 extension envelope。 |
| `submitted_at` | REQUIRED | 提交時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 5.3 映射規則

Integration Intent Envelope → AEOS Task Admission 的映射由 AEOS Agent Control Plane 負責。

映射規則：

- `integration_intent_id` → AEOS 內部 task_id 的來源參考。
- `external_correlation_id` → AEOS Collaboration Trace Envelope `correlation_id`。
- `submitter_identity` → AEOS Agent Profile 解析的輸入之一。
- `target_repository_ref` + `target_branch_ref` → AEOS Execution Contract scope。
- `instruction` → AEOS task decomposition 的輸入。
- `intent_constraints` → AEOS policy evaluation 的輸入。
- `evidence_requirements` → AEOS evidence requirements 的輸入。
- `human_approval_state` → AEOS approval requirement evaluation 的輸入。

ECP 不得假定其意圖格式與 AEOS 內部格式相同。AEOS  MAY 轉換、驗證或拒絕任何欄位。

### 5.4 提交語意

- 意圖提交不代表意圖已被接受。
- 意圖提交不代表執行已授權。
- 意圖提交不代表執行契約已建立。
- AEOS MUST 對每個提交的意圖回傳 Integration Admission Response。

### 5.5 Fail-closed

以下條件 MUST fail-closed：

- 缺少必要欄位 → 拒絕提交。
- 無法識別的外部控制平面 → 拒絕提交。
- 無法驗證的提交者身分 → 拒絕提交。
- 不支援的契約版本 → 拒絕提交。
- 無法解析的 target_repository_ref → 拒絕提交。

## 6. Admission / Authorization Response

### 6.1 目的

定義 AEOS Agent Control Plane 向外部控制平面回傳准入/授權結果的契約格式。

### 6.2 Integration Admission Response Envelope

| 欄位 | 要求 | 說明 |
|------|------|------|
| `admission_id` | REQUIRED | 准入記錄的穩定識別。由 AEOS 生成。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `external_correlation_id` | REQUIRED | ECP 側的關聯識別。 |
| `admission_decision` | REQUIRED | 准入決策：`ADMITTED` / `REJECTED` / `HOLD` / `BLOCKED`。 |
| `decision_reason` | REQUIRED | 決策原因。 |
| `execution_contract_ref` | OPTIONAL | 執行契約參考。僅在 `ADMITTED` 且已建立執行契約時提供。 |
| `approval_requirement` | REQUIRED | 是否需要人工核准：`NOT_REQUIRED` / `REQUIRED` / `ALREADY_PROVIDED`。 |
| `approval_request_ref` | OPTIONAL | 核准請求參考。僅在 `REQUIRED` 時提供。 |
| `authorization_scope_summary` | OPTIONAL | 授權範圍摘要。供 ECP 呈現。 |
| `trace_id` | OPTIONAL | AEOS Collaboration Trace Envelope 識別。 |
| `estimated_complexity` | OPTIONAL | 預估複雜度。供 ECP 呈現。 |
| `responded_at` | REQUIRED | 回應時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 6.3 准入決策語意

| 決策 | 說明 | ECP 預期行為 |
|------|------|-------------|
| `ADMITTED` | 意圖已通過准入評估 | 等待後續授權/核准/執行結果 |
| `REJECTED` | 意圖被拒絕 | 呈現拒絕原因給使用者。不得重試相同意圖 without modification。 |
| `HOLD` | 意圖需要額外資訊或等待 | 呈現等待原因。可補充資訊後重新提交。 |
| `BLOCKED` | 意圖被阻塞（缺少依賴/權限/能力） | 呈現阻塞原因與解除條件。 |

### 6.4 授權語意

准入（Admission）不等於授權（Authorization）。

- `ADMITTED` 表示意圖已通過准入評估，但可能仍需授權。
- 授權結果透過 State Reporting Protocol 傳播。
- 執行契約在授權完成後才建立。

### 6.5 Fail-closed

- 缺少 `admission_decision` → ECP MUST 視為 `BLOCKED`。
- 無法識別的 `admission_decision` → ECP MUST 視為 `BLOCKED`。
- 回應逾時 → ECP MUST 視為 `BLOCKED`。

## 7. Human Approval Escalation Protocol

### 7.1 目的

定義 AEOS Agent Control Plane 與外部控制平面之間的人工核准雙向傳播協議。

### 7.2 設計原則

- AEOS 決定是否需要人工核准（依 AEOS-SPEC-003 §8）。
- ECP 提供核准互動介面（依 ACC-ADR-001 D-06）。
- ECP 收集 Owner 決策並傳播回 AEOS。
- ECP 的核准互動介面不等於核准權威。
- Owner 決策仍需與正確任務、證據、權限綁定。

### 7.3 Integration Approval Request（AEOS → ECP）

| 欄位 | 要求 | 說明 |
|------|------|------|
| `approval_request_id` | REQUIRED | 核准請求的穩定識別。由 AEOS 生成。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `execution_contract_ref` | REQUIRED | 執行契約參考。 |
| `trace_id` | REQUIRED | AEOS Collaboration Trace Envelope 識別。 |
| `decision_scope` | REQUIRED | 決策範圍。定義 Owner 可以核准/拒絕什麼。 |
| `affected_resources` | REQUIRED | 受影響的資源列表。 |
| `risk_classification` | REQUIRED | 風險分類。 |
| `evidence_summary` | REQUIRED | 支援決策的證據摘要。 |
| `approval_options` | REQUIRED | 可用選項（如 `APPROVE` / `REJECT` / `MODIFY`）。 |
| `expiry_time` | OPTIONAL | 核准請求的過期時間。 |
| `created_at` | REQUIRED | 請求建立時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 7.4 Integration Approval Response（ECP → AEOS）

| 欄位 | 要求 | 說明 |
|------|------|------|
| `approval_request_id` | REQUIRED | 對應的核准請求識別。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `approval_decision` | REQUIRED | Owner 決策：`APPROVED` / `REJECTED` / `MODIFIED` / `EXPIRED`。 |
| `decision_scope` | REQUIRED | 決策範圍。MUST 不超過原始請求的 decision_scope。 |
| `owner_identity` | REQUIRED | 做出決策的 Owner 身分。 |
| `decision_timestamp` | REQUIRED | 決策時間戳。 |
| `decision_evidence_ref` | OPTIONAL | 決策證據參考。 |
| `modification_details` | OPTIONAL | 修改細節。僅在 `MODIFIED` 時適用。 |
| `responded_at` | REQUIRED | 回應時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 7.5 核准語意

- Owner 核准 ≠ 執行授權。Owner 核准是 AEOS 授權流程的輸入之一。
- ECP SHALL NOT 將 Owner 核准直接轉換為執行授權。
- AEOS MAY 在收到 Owner 核准後仍需進行額外授權評估。
- AEOS MAY 拒絕已核准的意圖（例如政策變更、證據不足）。

### 7.6 核准過期

- 若 `expiry_time` 已過且 ECP 未回應，AEOS MAY 視為 `EXPIRED`。
- ECP MAY 在過期後仍收集 Owner 決策，但 MUST 標記為 late response。
- AEOS 决定是否接受 late response。

### 7.7 Fail-closed

- 缺少 `approval_decision` → AEOS MUST 視為未核准。
- `decision_scope` 超過原始請求 → AEOS MUST 拒絕。
- 無法驗證的 `owner_identity` → AEOS MUST 拒絕。
- 核准傳播逾時 → AEOS MUST 依政策處理（可能視為 `BLOCKED` 或取消）。

## 8. State Reporting Protocol

### 8.1 目的

定義 AEOS Agent Control Plane 向外部控制平面通知狀態變更的契約格式。

### 8.2 Integration State Change Envelope

| 欄位 | 要求 | 說明 |
|------|------|------|
| `state_change_id` | REQUIRED | 狀態變更記錄的穩定識別。由 AEOS 生成。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `execution_id` | OPTIONAL | 執行識別。僅在已進入執行階段時提供。 |
| `trace_id` | REQUIRED | AEOS Collaboration Trace Envelope 識別。 |
| `external_correlation_id` | REQUIRED | ECP 側的關聯識別。 |
| `previous_state` | REQUIRED | 前一個狀態。 |
| `new_state` | REQUIRED | 新狀態。 |
| `state_change_reason` | REQUIRED | 狀態變更原因。 |
| `gate_results` | OPTIONAL | 觸發此狀態變更的關卡結果。 |
| `evidence_ref` | OPTIONAL | 相關證據參考。 |
| `changed_at` | REQUIRED | 狀態變更時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 8.3 Integration State Taxonomy

| 狀態 | 說明 |
|------|------|
| `SUBMITTED` | 意圖已提交，等待准入評估。 |
| `UNDER_ADMISSION` | 准入評估進行中。 |
| `ADMITTED` | 已通過准入。 |
| `REJECTED` | 准入被拒絕。 |
| `HOLD` | 需要額外資訊。 |
| `BLOCKED` | 被阻塞。 |
| `PENDING_AUTHORIZATION` | 等待授權。 |
| `AUTHORIZED` | 已授權。 |
| `PENDING_HUMAN_APPROVAL` | 等待人工核准。 |
| `HUMAN_APPROVED` | 人工已核准。 |
| `HUMAN_REJECTED` | 人工已拒絕。 |
| `EXECUTING` | 執行中。 |
| `COMPLETED` | 執行完成。 |
| `FAILED` | 執行失敗。 |
| `CANCELLED` | 已取消。 |
| `REVOKED` | 已撤銷。 |

### 8.4 狀態傳播語意

- AEOS MUST 通知所有 terminal state 變更。
- AEOS SHOULD 通知關鍵中間狀態變更（如 `PENDING_HUMAN_APPROVAL`、`AUTHORIZED`、`EXECUTING`）。
- AEOS MAY 不通知每個內部子任務的狀態變更。
- ECP MUST NOT 自行推測 AEOS 狀態。
- ECP MUST NOT 將執行狀態解讀為治理決策。

### 8.5 Fail-closed

- 狀態通知遺失 → ECP MUST 不假設最終狀態。
- 無法識別的狀態 → ECP MUST 視為 `BLOCKED`。
- 狀態通知逾時 → ECP MUST 呈現資料不足狀態。

## 9. Cross-system Correlation Model

### 9.1 目的

定義跨 ECP / AEOS / Agent 的關聯模型與 ID 傳播規則。

### 9.2 ID 生成責任

| ID | 生成者 | 說明 |
|----|--------|------|
| `integration_intent_id` | ECP | 整合意圖識別 |
| `intent_idempotency_key` | ECP | 冪等性金鑰 |
| `external_correlation_id` | ECP | ECP 側關聯識別 |
| `admission_id` | AEOS | 准入記錄識別 |
| `trace_id` | AEOS | Collaboration Trace Envelope 識別 |
| `execution_id` | AEOS | 執行識別 |
| `execution_request_id` | AEOS | Runtime Adapter 執行請求識別 |
| `evidence_id` | Runtime Adapter | 執行證據識別 |
| `approval_request_id` | AEOS | 核准請求識別 |
| `state_change_id` | AEOS | 狀態變更記錄識別 |

### 9.3 關聯模型

```
ECP Domain:
  integration_intent_id (root)
    └── external_correlation_id

AEOS Domain:
  admission_id
    └── trace_id
         ├── execution_id (1:N)
         │    └── execution_request_id (1:N)
         │         └── evidence_id (1:N)
         └── approval_request_id (0:N)

Cross-domain Binding:
  integration_intent_id ←→ admission_id（1:1）
  external_correlation_id ←→ trace_id.correlation_id（1:1）
```

### 9.4 傳播規則

- `external_correlation_id` MUST 在 Integration Intent Envelope 中提交。
- AEOS MUST 將 `external_correlation_id` 存入 Collaboration Trace Envelope 的 `correlation_id`。
- AEOS MUST 在所有 State Change Envelope 中回傳 `external_correlation_id`。
- AEOS MUST 在所有 Approval Request/Response 中回傳 `integration_intent_id` 和 `external_correlation_id`。
- ECP MUST 使用 `external_correlation_id` 作為其主要查詢鍵。

### 9.5 Multi-trace 模型

當一個整合意圖觸發多個 AEOS trace 時：

- 所有 trace MUST 共享相同的 `correlation_id`（= `external_correlation_id`）。
- ECP MAY 需要處理多個 `trace_id` 對應同一 `integration_intent_id`。
- AEOS MUST 確保每個 trace 可獨立追溯。

## 10. Evidence Ingestion Boundary

### 10.1 目的

定義 AEOS 執行證據如何映射至外部控制平面的證據呈現。

### 10.2 映射原則

- AEOS 執行證據由 AEOS-SPEC-004 定義。
- ECP MAY 將 AEOS 證據映射至 ECP 側的證據格式。
- ECP 的證據映射 SHALL NOT 改變 AEOS 證據的原始語意。
- ECP SHALL NOT 自行宣告缺少證據的工作已完成。
- ECP SHALL NOT 將部分成功呈現為完整成功。
- ECP SHALL NOT 隱藏失敗、阻塞、撤銷或資料不足狀態。

### 10.3 Evidence Reference Envelope（AEOS → ECP）

| 欄位 | 要求 | 說明 |
|------|------|------|
| `evidence_ref_id` | REQUIRED | 證據參考識別。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `execution_id` | REQUIRED | 執行識別。 |
| `trace_id` | REQUIRED | Collaboration Trace Envelope 識別。 |
| `evidence_id` | REQUIRED | AEOS-SPEC-004 執行證據識別。 |
| `evidence_completeness` | REQUIRED | 證據完整性：`COMPLETE` / `PARTIAL` / `INSUFFICIENT`。 |
| `gate_results_summary` | OPTIONAL | 關卡結果摘要。 |
| `artifact_refs` | OPTIONAL | 產出物參考。 |
| `provided_at` | REQUIRED | 提供時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 10.4 ECP 側映射

ECP MAY 將 Evidence Reference Envelope 映射至 ECP 側的證據格式。映射時 MUST 保留：

- `evidence_id` 的原始參考。
- `evidence_completeness` 的原始語意。
- `gate_results_summary` 的原始狀態。
- 所有來源、SHA、驗證結果與稽核參照。

### 10.5 Forbidden Data

ECP 從 AEOS 接收的證據 MUST 遵循 AEOS-SPEC-004 的 forbidden data rules。

ECP SHALL NOT 要求 AEOS 提供：
- 秘密、API 金鑰、Token、密碼或原始憑證。
- 完整提示或系統說明內容。
- 思考鏈或私人推理痕跡。
- 未經核准的原始客戶內容。

## 11. Terminal State Protocol

### 11.1 目的

定義執行終止狀態的傳播契約。

### 11.2 Terminal States

| 終止狀態 | 說明 | AEOS-SPEC-004 對應 |
|----------|------|-------------------|
| `COMPLETED` | 執行完成 | `completed`（仍需 gate 評估） |
| `FAILED` | 執行失敗 | `failed`、`timed_out`、`rejected`、`unsupported` |
| `CANCELLED` | 已取消 | `cancelled` |
| `REVOKED` | 已撤銷 | N/A（AEOS 治理決策） |
| `BLOCKED` | 永久阻塞 | N/A（AEOS 治理決策） |

AEOS-SPEC-004 outcome 至 AEOS-SPEC-007 終止狀態的完整映射：

| AEOS-SPEC-004 outcome | AEOS-SPEC-007 終止狀態 | 映射說明 |
|------------------------|----------------------|----------|
| `completed` | `COMPLETED` | 執行完成，但仍需 gate 評估 |
| `failed` | `FAILED` | 執行環境嘗試後失敗 |
| `timed_out` | `FAILED` | 執行超出時間限制，歸類為失敗 |
| `rejected` | `FAILED` | 因政策、授權、驗證或防護規則被拒絕，歸類為失敗 |
| `unsupported` | `FAILED` | 執行環境不支援契約版本、能力、工具、結構描述或必要證據，歸類為失敗 |
| `cancelled` | `CANCELLED` | 因取消或撤銷而停止執行 |

### 11.3 Terminal State Envelope

| 欄位 | 要求 | 說明 |
|------|------|------|
| `terminal_state_id` | REQUIRED | 終止狀態記錄識別。 |
| `integration_intent_id` | REQUIRED | 對應的整合意圖識別。 |
| `execution_id` | REQUIRED | 執行識別。 |
| `trace_id` | REQUIRED | Collaboration Trace Envelope 識別。 |
| `terminal_state` | REQUIRED | 終止狀態。 |
| `terminal_reason` | REQUIRED | 終止原因。 |
| `final_evidence_ref` | OPTIONAL | 最終證據參考。 |
| `final_gate_results` | OPTIONAL | 最終關卡結果。 |
| `resource_cleanup_status` | OPTIONAL | 資源清理狀態。 |
| `terminated_at` | REQUIRED | 終止時間戳。 |
| `contract_version` | REQUIRED | 整合契約版本。 |

### 11.4 終止語意

- `COMPLETED` 不代表驗證通過。ECP MUST NOT 將 `COMPLETED` 解讀為品質關卡 PASS。
- `FAILED` MUST 包含失敗原因與失敗證據。
- `CANCELLED` 可由 ECP 請求或 AEOS 自行發起。
- `REVOKED` 只能由 AEOS 發起。ECP 可呈現撤銷狀態但不得自行撤銷。
- `BLOCKED` MUST 包含阻塞原因與解除條件。

### 11.5 Fail-closed

- 未收到終止狀態 → ECP MUST NOT 假設執行已完成。
- 無法識別的終止狀態 → ECP MUST 視為 `BLOCKED`。
- 終止狀態與證據不一致 → ECP MUST 呈現不一致狀態。

## 12. Resource Collision Control

### 12.1 目的

定義多個整合意圖操作相同資源時的衝突處理契約。

### 12.2 衝突偵測責任

- AEOS Agent Control Plane MUST 在准入或授權階段偵測資源衝突。
- ECP SHALL NOT 自行偵測 AEOS 側的資源衝突。
- ECP MAY 提供額外上下文幫助 AEOS 偵測衝突。

### 12.3 衝突類型

| 衝突類型 | 說明 |
|----------|------|
| Repository conflict | 多個意圖操作同一 Repository |
| Branch conflict | 多個意圖操作同一 Branch |
| File conflict | 多個意圖操作同一檔案 |
| Credential conflict | 多個意圖需要互斥的憑證 |
| Resource lock conflict | 多個意圖需要獨佔資源 |

### 12.4 衝突處理語意

當 AEOS 偵測到資源衝突時：

- AEOS MAY 將後提交的意圖置為 `HOLD` 或 `BLOCKED`。
- AEOS MAY 要求先提交的意圖完成後再處理。
- AEOS MAY 要求 ECP 提供額外指示。
- AEOS MUST 在 Admission Response 或 State Change 中通知衝突。
- ECP MUST NOT 繞過 AEOS 的衝突處理。

### 12.5 ECP 側呈現

ECP 收到衝突通知時：

- MUST 呈現衝突資訊給 Owner。
- MAY 提供 Owner 調整意圖的操作介面。
- SHALL NOT 自行解決 AEOS 側的衝突。

## 13. Idempotency Contract

### 13.1 目的

定義整合意圖提交的冪等性契約。

### 13.2 冪等性保證

- ECP MUST 在每次提交時提供 `intent_idempotency_key`。
- AEOS MUST 使用 `intent_idempotency_key` 偵測重複提交。
- 相同 `intent_idempotency_key` 的重复提交 MUST 回傳相同的 `admission_id`。
- AEOS MUST NOT 為相同 `intent_idempotency_key` 建立多個准入記錄。

### 13.3 重試語意

- ECP MAY 在收到網路錯誤或逾時時重試提交。
- 重試 MUST 使用相同的 `intent_idempotency_key`。
- AEOS MUST 將相同 `intent_idempotency_key` 的重試視為同一意圖。
- AEOS SHOULD 回傳原始的 Admission Response。

### 13.4 冪等性金鑰規則

- `intent_idempotency_key` MUST 由 ECP 生成。
- `intent_idempotency_key` MUST 在 ECP 側唯一。
- `intent_idempotency_key` MUST NOT 被重複用於不同意圖。
- AEOS MAY 設定冪等性金鑰的有效期間。

## 14. Transport Neutrality

### 14.1 目的

確保整合契約不綁定特定 transport 或 protocol。

### 14.2 契約與 Transport 分離

- 本規格定義的 envelope 是訊息語意定義，不是 transport 定義。
- 每個 envelope MUST 可被序列化為多種格式（JSON、Protobuf、XML 等）。
- 每個 envelope MUST 可透過多種 transport 傳輸（HTTP、gRPC、event bus 等）。
- Transport 選型不得改變 envelope 語意。

### 14.3 Transport 選型約束

後續 transport 實作 MUST：

- 支援所有 REQUIRED 欄位。
- 維持欄位語意不變。
- 支援版本控制（`contract_version`）。
- 支援認證與授權。
- 支援加密傳輸。
- 支援稽核日誌。

### 14.4 非目標

本規格不定義：

- 具體 API endpoint。
- 具體 HTTP method。
- 具體 message broker topic。
- 具體 event schema。
- 具體 SDK 或 client library。

## 15. Authority Matrix

### 15.1 跨 Repository 權威位置

| 資訊類別 | 正式權威位置 | 說明 |
|----------|-------------|------|
| AEOS 內部治理契約 | AEOS（SPEC-002~006） | 不變 |
| 整合契約 | AEOS（SPEC-007） | 本文件 |
| ECP 內部元件設計 | ECP Repository | 各 ECP 自行定義 |
| ECP Execution Intent schema | ECP Repository | ECP 側產品邏輯 |
| Intent → Task Admission 映射 | AEOS（SPEC-007 §5） | AEOS 定義如何接收 |
| Human Approval 雙向協議 | AEOS（SPEC-007 §7）+ ECP | AEOS 定義協議，ECP 定義 UI |
| Cross-system correlation | AEOS（SPEC-007 §9） | AEOS 定義 ID 規則 |
| Resource collision | AEOS（SPEC-007 §12） | AEOS 治理責任 |
| ECP 側 evidence 映射 | ECP Repository | ECP 側 ingest 邏輯 |

### 15.2 跨系統不變量

- AEOS 治理權威 SHALL NOT 因整合而轉移至 ECP。
- ECP 操作介面 SHALL NOT 被解讀為治理權威。
- ECP 衍生上下文 SHALL NOT 被提升為事實來源。
- ECP 核准互動 SHALL NOT 被解讀為執行授權。

## 16. Fail-closed Invariants

### 16.1 整合層 Fail-closed

以下條件 MUST fail-closed：

| 條件 | ECP 行為 | AEOS 行為 |
|------|----------|-----------|
| 整合契約版本不相容 | 不得提交 | 拒絕提交 |
| 無法識別的外部控制平面 | — | 拒絕提交 |
| 無法驗證的提交者身分 | — | 拒絕提交 |
| 缺少必要欄位 | 不得提交 | 拒絕提交 |
| 准入回應逾時 | 視為 BLOCKED | — |
| 狀態通知逾時 | 呈現資料不足 | — |
| 核准傳播失敗 | 呈現傳播失敗 | 視為未核准 |
| 證據不完整 | 呈現不完整狀態 | 依政策處理 |
| 終止狀態未收到 | 不假設完成 | — |
| 跨系統 ID 不一致 | 呈現不一致狀態 | — |

### 16.2 繼承自 ACC-ADR-001 D-13 的不變量

當 ECP 為 ACC 時，以下不變量 MUST 維持：

1. ECP 不得直接建立或簽發企業執行契約。
2. ECP 不得直接授權 Runtime/Harness、工具、模型、資料或憑證。
3. ECP 不得把自然語言命令視為已核准命令。
4. ECP 不得把 Owner 的互動操作視為無限制執行授權。
5. ECP 不得繞過 AEOS Agent Control Plane、YEOS 工程治理或目標 Repository Protection。
6. ECP 不得把衍生上下文、快取或對話紀錄提升為事實來源。
7. ECP 不得降低 Human Approval、Evidence、SHA binding、credential isolation 或 fail-closed 控制。
8. ECP 不得因跨專案總控責任而取得各產品、平台或業務領域的內部權威。

## 17. 一致性檢查表

宣稱符合本規格的外部控制平面整合實作 SHOULD 示範：

1. 整合意圖使用 Integration Intent Envelope 格式。
2. 准入/授權回應使用 Integration Admission Response Envelope 格式。
3. 人工核准使用雙向 Approval Request/Response Envelope。
4. 狀態變更使用 Integration State Change Envelope。
5. 跨系統 ID 遵循 §9 的生成責任與傳播規則。
6. 證據映射保留 AEOS 證據原始語意。
7. 終止狀態使用 Terminal State Envelope。
8. 資源衝突由 AEOS 偵測與處理。
9. 意圖提交具備冪等性。
10. 整合契約不綁定特定 transport。
11. 外部控制平面不取得 AEOS 治理權威。
12. 所有 fail-closed 條件正確處理。

## 18. 狀態與核准

本文件為 **Candidate 0.1.0**。

本文件由工作包 AEOS-ACC-ACP-C1 授權建立。本文件目前為架構/契約文件工作，不是程式實作授權。

本文件待 Review 與核准後升級為 Approved。核准前不得作為實作依據。

核准後：

- 外部控制平面可依據本規格定義的整合契約與 AEOS Agent Control Plane 整合。
- 整合契約不改變 AEOS 治理權威歸屬。
- 整合契約不綁定特定 transport 或執行環境。
- 本規格不授權任何程式實作、Production 操作或治理權限變更。

## 19. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS-ACC-ACP-C1 | 工作包 | 整合契約定義工作授權 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境分離 |
| AEOS-ADR-004 — Productization Boundary Decision | ADR | 整合契約層級定位 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS 擁有 Agent Collaboration 治理 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | 架構 | Control Plane 責任、Execution Contract、Runtime/Harness 邊界 |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec | 規格 | Gate Result Contract、Trace Envelope |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension | 規格 | Task Admission、Runtime Adapter Request Boundary |
| AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract | 規格 | 執行證據契約 |
| AEOS-SPEC-005 — Capability / Tool Registry Adapter Contract | 規格 | 能力發現契約 |
| AEOS-SPEC-006 — Provider-neutral Conformance Tests | 規格 | 符合性測試邊界 |
| ACC-ADR-001 — 對話式控制介面與 Agent Control Plane 分離決策 | ADR | ACC 定位、權威分離、反繞過不變量 |
| ACC-ARCH-002 — 最小協調能力架構 | 架構 | ACC 元件設計、整合依賴 |

## 20. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-09-17 | 建立 External Control Plane Integration Contract Candidate，定義七個整合互動點、資源衝突控制、冪等性契約、transport neutrality、權威矩陣與 fail-closed 不變量 | Codex |
| 0.2.0 | 2026-09-17 | R1 Review 修正：(F-001) 將 AEOS-ADR-004 從關聯文件提升為依據文件並加入 frontmatter authority；(F-002) 補齊 §11.2 AEOS-SPEC-004 outcome 完整映射（timed_out→FAILED、rejected→FAILED、unsupported→FAILED） | Codex |
