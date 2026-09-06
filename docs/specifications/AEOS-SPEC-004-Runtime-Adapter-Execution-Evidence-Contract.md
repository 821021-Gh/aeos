---
doc-id: AEOS-SPEC-004
doc-name: Runtime Adapter Execution Evidence Contract
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-069
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
---

# AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract

## 執行摘要

本規格定義 Agent Runtime 轉接器在完成 AEOS 執行要求後必須回傳的 provider-neutral 執行證據契約。

Runtime 轉接器執行證據的目的，是讓 Agent Control Plane、Gate 執行器、Audit / Observability 邊界與後續驗證能判斷執行環境做了什麼、用了哪些受授權能力、結果狀態為何、是否有錯誤、成本與延遲為何，以及是否符合證據最小化。它不是來源驗證、記憶體 confirmation、learning 案例 publication、human 核准、正式環境授權或品質關卡 pass。

本文件不選定任何執行環境、Harness、供應商、模型、工具平台、工作流程引擎或產品儲存庫；不實作轉接器；不定義 CRM 領域能力；不授權 Production action。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-004 |
| 文件名稱 | Runtime Adapter Execution Evidence Contract |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #69、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-003 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003 |

## 1. 目的

本規格目的為：

- 定義 Runtime 轉接器執行要求 / response 邊界。
- 定義 provider-neutral 執行證據 minimum fields。
- 定義 cost、token、延遲、工具參考等 optional 證據 fields。
- 定義 forbidden 資料規則。
- 定義供應商 / 模型 / 執行環境中繼資料擴充封裝。
- 防止執行環境成功被 Control Plane 或 Gate 執行器誤解為驗證 pass、權限決策或核准結果。

## 2.範圍

### 2.1 在範圍內

- Runtime 轉接器執行回應邊界。
- 執行證據最小欄位。
- 執行環境、轉接器、能力、供應商/模型、執行身分。
- 開始/結束時間、持續時間、結果、錯誤類別。
- 可選成本、Token、延遲、工具參考。
- 禁止資料規則。
- 供應商/模型/執行環境元資料擴展信封。
- 執行環境證據解釋規則。

### 2.2 超出範圍

- Runtime 轉接器實作。
- API endpoint、SDK、資料庫結構描述或遙測資料 pipeline 實作。
- Source 權限、品質關卡結果、記憶體提升、learning 案例 publication、human 核准或正式環境授權。
- CRM 領域能力。
- 任何命名的執行環境、供應商、模型、工作流程引擎、工具平台、向量資料庫或產品選擇。

## 3. 管理機構

|權威|角色 |
|---|---|
| AEOS-ADR-005 | AEOS 擁有 runtime-neutral Agent Collaboration 治理 |
| AEOS-ADR-003 | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 | Execution Contract，執行環境邊界與證據邊界 |
| AEOS-SPEC-002 |協作追蹤信封與品質關卡語意 |
| AEOS-SPEC-003 | Runtime 轉接器執行請求邊界與 Gate 執行器解釋 |

規則：

- 執行環境僅執行授權的請求。
- Runtime 轉接器證據報告執行事實，而不是治理決策。
- 執行環境失敗 MUST NOT 被解釋為驗證通過。
- 執行環境成功 MUST NOT 被解釋為事實來源驗證成功、記憶體確認、案例發布、人員核准或正式環境授權。

## 4. Runtime Adapter 邊界

Runtime 轉接器是 AEOS Control Plane 執行請求與具體執行環境/工具/供應商實作之間的 provider-neutral 邊界。

Runtime 轉接器 MAY 將要求欄位轉換為供應商特定的呼叫，但 MUST 保留 AEOS Execution Contract 語意，且 MUST NOT 擴大權限。

Runtime 轉接器回應 MUST 被視為執行證據。它可以提供給 Gate 執行器和驗證，但它本身並不是 Gate 決策。

## 5. 執行證據最小欄位

每個執行環境轉接器回應 MUST 包含以下最小欄位或等效結構化證據：

|領域|要求 |
|---|---|
| `evidence_id` |穩定證據記錄身分|
| `execution_id` |來自授權請求的執行身分 |
| `execution_request_id` | Runtime 轉接器請求身分 |
| `trace_id` |協作 Trace Envelope 身分 |
| `runtime_id` |執行環境或Harness 實作識別/類別 |
| `adapter_id` |轉接器身分|
| `adapter_version` |轉接器合約/實作版本 |
| `capability_id` |執行的能力或能力類別 |
| `provider_id` |供應商身分或供應商類別（如果可用）|
| `model_id` |模型識別或模型能力等級（如果適用）|
| `started_at` |執行開始時間戳|
| `ended_at` |執行結束時間戳記 |
| `outcome` | 已完成 / failed / cancelled / timed_out / 已拒絕 / unsupported |
| `error_class` | outcome 不為已完成時的穩定錯誤類別 |
| `evidence_summary` |適合稽核的最小執行摘要 |

如果某個欄位不適用，則回應 MUST 表示不適用，而不是省略與治理相關的含義。

## 6. 可選證據欄位

在可用且政策允許的情況下，執行環境轉接器回應 SHOULD 包括：

|領域|意義|
|---|---|
| `duration_ms` |執行環境執行持續時間 |
| `latency_ms` |觀察到的供應商/模型/工具延遲 |
| `token_input` |輸入 Token計數或估計 |
| `token_output` |輸出 Token計數或估計 |
| `token_total` |Token 總數或估計 |
| `cost_estimate` |供應商/執行環境成本估算 |
| `cost_actual` |已知的實際成本 |
| `tool_references` |工具呼叫參考，不是原始秘密或完整有效負載 |
| `artifact_refs` |輸出產出物參考|
| `retry_count` |執行環境級重試計數 |
| `cancellation_seen` |是否收到取消/撤銷 |
| `provider_request_ref` |特定於供應商的請求參考（如果可以安全保留）|

可選欄位 MUST 遵循§8 中的禁止資料規則。

## 7. 結果與錯誤語意

Runtime 轉接器結果僅描述執行狀態。

|結果|意義|
|---|---|
| 已完成 | 執行環境在授權範圍內完成要求的執行 |
| failed | 執行環境已嘗試執行，但執行失敗 |
| cancelled | 因取消／撤銷而停止執行 |
| timed_out | 執行超出時間限制 |
| 已拒絕 | 執行環境因政策、授權、驗證或本機防護規則而拒絕要求 |
| unsupported | 執行環境不支援契約版本、能力、工具、結構描述或必要證據 |

錯誤類別 SHOULD 維持穩定且 provider-neutral，包括：

|錯誤類別 |意義|
|---|---|
| `contract_invalid` |請求合約缺失或無效 |
| `contract_unsupported` |合約版本或強制語意不受支援 |
| `capability_unsupported` |請求的功能不可用 |
| `tool_unauthorized` |工具範圍未經授權或無法表達 |
| `model_unavailable` |模型能力不可用 |
| `provider_error` |供應商失敗或回傳不可用 |
| `timeout` |執行超出時間限制 |
| `cancelled` |已申請取消/撤銷 |
| `forbidden_data_detected` |回應將違反禁止資料規則 |
| `evidence_incomplete` |無法出示所需證據 |

執行環境`completed`結果 MUST NOT 被視為品質關卡`PASS`，無需 Gate 執行器評估。

## 8. 禁止資料規則

Runtime 轉接器證據 MUST NOT 包括：

- 秘密、API 金鑰、Token、密碼或原始憑證資料；
- 完整的提示或隱藏/系統說明內容，除非政策明確允許保留；
- 思考鍊或私人推理痕跡；
- 未經核准的原始客戶內容；
- 不必要的敏感業務資料；
- 當安全參考足夠時，包含受保護資料的原始工具有效負載；
- 繞過證據最小化的特定於供應商的除錯轉儲；
- 授權 Execution Contract 範圍以外的資料。

當稽核不需要完整內容時，證據 SHOULD 使用參考文獻、摘要、編輯標記、雜湊或產出物 ID。

若無法在不包含禁止資料的情況下產生必要證據，Runtime 轉接器 MUST 回傳 `rejected` 或 `failed`，並附上 `forbidden_data_detected`／`evidence_incomplete`，不得洩漏該資料。

## 9. 元資料擴充封套

供應商、模型、執行環境或特定於工具的元資料 MAY 僅包含在擴展信封內。

擴展信封 SHOULD 包括：

|領域|意義|
|---|---|
| `extension_namespace` |供應商/執行環境/工具命名空間 |
| `extension_version` |擴充架構版本 |
| `metadata_class` |執行環境/供應商/模型/工具/診斷|
| `metadata` |最小化後供應商特定的元資料 |
| `redaction_applied` |敏感欄位是否被刪除 |

擴充元資料 MUST NOT 覆蓋穩定的核心欄位。若擴充元資料與核心證據衝突，則以核心證據與 Control Plane 政策為準，衝突 SHOULD 升級。

## 10. 證據解釋規則

Control Plane 和 Gate 執行器 MUST 根據以下規則解釋執行環境證據：

- 執行環境成功只是執行成功。
- 執行環境失敗不是驗證通過。
- 執行環境回應 MUST NOT 被視為來源權限。
- 執行環境回應 MUST NOT 確認記憶體提升。
- 執行環境回應 MUST NOT 發布學習案例或權威知識。
- 執行環境回應 MUST NOT 滿足人員核准，除非它引用來自授權核准機構的有效核准證據。
- 執行環境回應 MUST NOT 授權正式環境部署、破壞性操作或客戶資料突變。
- 缺少強制證據 SHOULD 導致 Gate 執行器根據 AEOS-SPEC-002 / AEOS-SPEC-003 政策退回`NEEDS_WORK`、`HUMAN_APPROVAL`、`REJECTED`或`BLOCKED`。

## 11. 走線對齊

Runtime 轉接器證據 MUST 可連結到 AEOS-SPEC-002 協作追蹤信封和 AEOS-SPEC-003 執行環境執行請求追蹤。

最小追蹤連動：

|追蹤欄位|要求 |
|---|---|
| `trace_id` | MUST 匹配協作追蹤信封 |
| `execution_request_id` | MUST 匹配執行環境執行請求 |
| `execution_id` | MUST 保留 Execution Contract 身分 |
| `capability_id` | MUST 對應到授權能力 |
| `tool_references` | MUST 在適用時連接工具執行以運行追蹤 |
| `artifact_refs` | SHOULD 識別輸出產出物而不洩漏禁止資料 |

## 12. 一致性檢查表

採用 Runtime 轉接器執行證據契約時，SHOULD 證明：

1. 證據包括執行環境 id、轉接器版本、能力 id、供應商/模型 id、執行 id、開始/結束時間、結果和錯誤類別。
2. 證據包括成本、Token、延遲以及適用和允許的工具參考。
3. 證據排除秘密、完整提示、思維鍊和未經核准的原始客戶內容。
4.執行環境不回傳權威決策、記憶確認、學習案例發布或人工核准結果。
5. 執行環境失敗不能解釋為驗證通過。
6. 特定於供應商的元資料保留在擴充封套內。
7. 擴充元資料不能覆蓋穩定的核心證據欄位。
8. 證據連結到協作追蹤信封和執行環境執行請求追蹤。

## 13. 狀態與核准

本規格目前為 **Approved 1.0.0**。

PR #77 已合併至 `main`，merge commit 為 `856f694079e59757b98eb4f7249b6100b68db01a`。此合併作為 Repository Owner 最終核准證據，正式核准本規格為 Runtime Adapter Execution Evidence Contract。

核准後：

- Runtime 轉接器執行證據可作為 Agent Control Plane、Gate 執行器、Audit / Observability 邊界與後續驗證的 provider-neutral 執行證據。
- Runtime 證據不得被解讀為來源驗證、記憶體 confirmation、learning 案例 publication、human 核准、正式環境授權或品質關卡 pass。
- #70 / #71 可在本規格邊界下分別處理能力 / 工具登錄表轉接器契約與 provider-neutral 符合性 tests。
- 本規格不授權執行環境實作、Production 部署、客戶資料 mutation 或 YCRM 下游採用。

## 14. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #69 | GitHub Issue | Runtime Adapter Execution Evidence Contract 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS 擁有 Agent Collaboration 治理 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境邊界 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Execution Contract 與執行環境責任 |
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec |規格|追蹤封裝與門語意 |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension |規格|執行環境執行請求邊界和 Gate 執行器語意 |

## 15. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 PR #77 merge 證據（856f694079e59757b98eb4f7249b6100b68db01a）升級為 Approved Specification；正式核准 Runtime Adapter Execution Evidence Contract，作為 #70 / #71 後續登錄表轉接器與符合性工作的執行環境證據邊界依據 | Codex |
| 0.1.0 | 2026-09-05 | 建立 Runtime Adapter Execution Evidence Contract Draft，定義 provider-neutral 執行環境證據 minimum fields、optional cost/token/延遲/工具參考、forbidden 資料規則、擴充封裝、證據 interpretation 規則與 trace alignment | Codex |
