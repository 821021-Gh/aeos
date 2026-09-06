---
doc-id: AEOS-SPEC-005
doc-name: Capability / Tool Registry Adapter Contract
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-06
updated: 2026-09-06
related:
  - AEOS-ISSUE-070
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
  - AEOS-SPEC-004
---

# AEOS-SPEC-005 — Capability / Tool Registry Adapter Contract

## 執行摘要

本規格定義執行環境 / 供應商 / 工具邊界如何向 AEOS 宣告可用能力與工具，同時確保政策 allow / deny 仍由 AEOS Agent Control Plane 決定。

Capability discovery 只代表 availability，不代表授權。Tool 結構描述、供應商特定中繼資料、執行環境能力 details 與 discovery 結果不得污染 AEOS stable 核心語意，也不得讓執行環境決定角色、設定檔、關卡、核准或政策。

本文件不建立工具 marketplace、不授權任何 high-risk action、不選定執行環境/供應商/模型/工具、不實作轉接器、不碰 Production、不推進 YCRM 下游採用。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-005 |
| 文件名稱 | Capability / Tool Registry Adapter Contract |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-06 |
| 最後更新 | 2026-09-06 |
| 依據文件 | AEOS Issue #70、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-003、AEOS-SPEC-004 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004 |

## 1. 目的

本規格目的為：

- 定義 Capability discovery 結果。
- 定義 Tool input / 輸出結構描述 versioning。
- 定義 Policy allow / deny 與 availability 的分離。
- 定義 unknown 能力 / unknown 結構描述 / unsupported version fail-closed。
- 定義工具執行 trace 參考。
- 定義供應商特定中繼資料擴充封裝。

## 2.範圍

### 2.1 在範圍內

- 能力發現結果。
- Tool 登錄表轉接器回應邊界。
- 工具輸入/輸出模式版本控制。
- 能力可用性和策略允許/拒絕分離。
- 未知能力失敗-closed。
- 未知架構/不支援的版本失敗-closed。
- 工具執行追蹤參考。
- 供應商/執行環境/工具元資料擴充封套。

### 2.2 超出範圍

- 工具市場、目錄商務、定價或包裝分發。
- Runtime 轉接器實作、工具執行實作、SDK、API endpoint 或資料庫結構描述。
- 任何高風險行為授權。
- 執行環境角色/設定檔/門決策權限。
- 正式環境部署、客戶資料突變、憑證設定或操作啟動。
- CRM 領域能力實作。
- 任何命名的執行環境、供應商、模型、工作流程引擎、工具平台或向量資料庫選擇。

## 3. 管理機構

|權威|角色 |
|---|---|
| AEOS-ADR-005 | AEOS 擁有 runtime-neutral Agent Collaboration 治理 |
| AEOS-ADR-003 | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 | Execution Contract，供應商轉接器邊界與工具存取邊界 |
| AEOS-SPEC-002 |技能/工具範圍、門語意與追蹤封裝 |
| AEOS-SPEC-003 |能力調度和執行環境執行請求邊界|
| AEOS-SPEC-004 |執行環境執行證據和擴充封裝規則|

規則：

- 能力發現僅報告可用性。
- 策略允許/拒絕由 AEOS Agent Control Plane 決定。
- 工具執行必須連結到 Agent Collaboration 運行追蹤。
- 供應商特定的元資料保留在擴充封套內，不能覆蓋穩定的核心欄位。

## 4. 登錄表轉接器邊界

能力/Tool Registry 轉接器是 AEOS Control Plane 和執行環境/供應商/工具邊界之間的 provider-neutral 發現邊界。

轉接器 MAY 將特定於供應商的發現轉換為 AEOS 穩定欄位，但是 MUST NOT：

- 授予政策許可；
- 分配角色原型或 Agent Profile；
- 決定品質關卡結果；
- 滿足人員的認同；
- 授權生產、破壞性、憑證或客戶資料突變行動；
- 擴展能力、工具、資料、記憶體、模型或憑證範圍。

## 5. 能力發現結果

每個發現結果 SHOULD 包括：

|領域|意義|
|---|---|
| `discovery_id` |穩定的發現記錄身分 |
| `registry_adapter_id` |轉接器身分|
| `registry_adapter_version` |轉接器版本 |
| `runtime_id` |執行環境／Harness身分或類別|
| `provider_id` |供應商身分或類別 |
| `capability_id` |能力識別或類別 |
| `capability_name` |人員可讀的功能名稱 |
| `capability_status` |可用/不可用/deprecated/不支援/未知|
| `tool_ids` |為此功能公開的工具（如果有） |
| `schema_refs` |輸入/輸出架構參考|
| `evidence_requirements_supported` |執行環境/工具可以回傳的證據欄位 |
| `risk_hints` |非權威風險提示（若有提供） |
| `data_handling_hints` |非權威資料處理提示（若有提供）|
| `extension_ref` |供應商特定的擴充封套參考 |
| `discovered_at` |發現時間戳|

`capability_status = available` MUST NOT 解釋為政策允許。

## 6. 工具架構版本控制

工具輸入與輸出模式 MUST 進行版本控制，provider-neutral 處於穩定核心邊界。

各工具架構參考 SHOULD 包括：

|領域|意義|
|---|---|
| `tool_id` |穩定的工具識別或對應的工具類別 |
| `tool_operation_class` |觀察/讀取、建立/寫入、更新/變異、刪除/破壞、執行、部署/啟動、憑證/權限管理 |
| `input_schema_id` |輸入模式識別 |
| `input_schema_version` |輸入架構版本 |
| `output_schema_id` |輸出模式識別 |
| `output_schema_version` |輸出架構版本 |
| `required_evidence_fields` |執行後所需證據 |
| `forbidden_data_profile` |禁止的資料限制 |
| `extension_schema_ref` |特定於供應商時的擴充架構參考 |

架構演進 MUST 保留相容性規則或需要明確遷移。未知架構或不支援的版本 MUST 失敗即封鎖。

## 7. 政策允許/拒絕分離

登錄表轉接器報告存在的內容。 Agent Control Plane 決定允許的內容。

Control Plane 允許/拒絕決定 SHOULD 評估：

- 任務和子任務範圍；
- Agent Profile 和角色原型；
- 能力等級；
- 工具操作類別；
- 資料敏感度；
- 核准狀態；
- 政策背景；
- 執行環境間限制；
- 證據要求；
- 職責分離的限制；
- 禁止的資料設定檔。

發現結果 MAY 通知能力調度，但 MUST NOT 繞過 Control Plane 策略評估。

## 8. 失敗-Closed 規則

以下條件 MUST 將失敗即封鎖，除非明確的 AEOS 策略授予更安全的後備：

|狀況 |所需行為 |
|---|---|
|未知能力 |拒絕出貨或退貨 `BLOCKED` / `HUMAN_APPROVAL` |
|未知工具 |拒絕使用工具 |
|未知架構 |在架構已知之前拒絕使用工具 |
|不支援的架構版本 |拒絕或要求遷移 |
|缺失證據能力 |拒絕或要求縮小範圍 |
|供應商元資料衝突 |優先選擇穩定的核心領域並逐步升級|
|歧義運算類別|視為高風險或拒絕 |
|缺少資料處理提示|使用更嚴格的政策分類|

Fail-closed 結果 MUST 可追溯到 Agent Collaboration 運行。

## 9. 工具執行追蹤參考

每個授權工具執行 SHOULD 都會連結到 Agent Collaboration 運行追蹤。

最小追蹤參考欄位：

|領域|意義|
|---|---|
| `trace_id` |協作 Trace Envelope 身分 |
| `execution_request_id` |執行環境執行請求識別 |
| `tool_execution_ref` |穩定工具執行參考|
| `tool_id` |工具識別或對應工具類別 |
| `capability_id` |能力識別或類別 |
| `policy_decision_ref` | Control Plane 允許/拒絕決策參考 |
| `schema_ref` |工具輸入/輸出架構參考|
| `evidence_ref` |執行環境執行證據參考|
| `extension_ref` |適用時供應商特定的擴充參考 |

工具執行追蹤 MUST NOT 包含秘密、原始憑證、不必要的完整提示、思維鏈、未經核准的原始客戶內容或不必要的敏感負載。

## 10. 元資料擴充封套

特定於供應商、特定於執行環境或特定於工具的元資料 MAY 僅包含在擴展信封內。

擴展信封 SHOULD 包括：

|領域|意義|
|---|---|
| `extension_namespace` |供應商/執行環境/工具命名空間 |
| `extension_version` |擴充架構版本 |
| `extension_kind` |能力/工具/模式/供應商/診斷|
| `metadata` |最小化特定於供應商的元資料 |
| `redaction_applied` |禁止或敏感欄位是否被刪除 |
| `core_mapping_ref` |參考穩定核心場對應|

擴展元資料 MUST NOT 覆蓋：

- `capability_id`;
- `tool_operation_class`;
- 策略允許/拒絕；
- 核准狀態；
- 角色或設定檔分配；
- 關卡結果；
- 證據最小化；
- 禁止資料規則。

## 11. 一致性檢查表

採用 Capability / Tool Registry Adapter 時，SHOULD 示範：

1. 能力發現僅代表可用性，不代表政策允許。
2.策略允許/拒絕由 AEOS Agent Control Plane 決定。
3. 工具輸入/輸出模式為 provider-neutral 且版本化。
4. 未知能力失敗即封鎖。
5. 未知架構和不支援的架構版本失敗即封鎖。
6. 工具執行連結到 Agent Collaboration Run Trace。
7. 特定於供應商的元資料保留在擴充封套內。
8. 擴充元資料不能覆蓋穩定的核心欄位。
9. 執行環境不透過登錄表元資料決定角色、設定檔或關卡結果。
10. 任何高風險行為不得僅憑發現結果而授權。

## 12. 狀態與核准

本規格目前為 **Approved 1.0.0**。

PR #79 已合併至 `main`，merge commit 為 `eb2d8591aa0bfb8aa5a3b23bfacc8cd4747e52b0`。此合併作為 Repository Owner 最終核准證據，正式核准本規格為 Capability / Tool Registry Adapter Contract。

核准後：

- Capability discovery 只代表 availability，不代表政策 allowed。
- Policy allow / deny 仍由 AEOS Agent Control Plane 決定。
- Tool input / 輸出結構描述必須維持 provider-neutral 且版本化。
- Unknown 能力、unknown 結構描述、unsupported version 必須 fail 已關閉。
- Tool 執行必須連到 Agent Collaboration Run trace。
- Provider-specific 中繼資料必須放入擴充封裝，不得污染 stable 核心。
- #71 可在本規格與 AEOS-SPEC-004 邊界下處理 provider-neutral 符合性 tests。
- 本規格不建立工具 marketplace、不授權 high-risk action、不授權執行環境實作、Production 部署、客戶資料 mutation 或 YCRM 下游採用。

## 13. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #70 | GitHub Issue | Capability / Tool Registry Adapter Contract 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS 擁有 Agent Collaboration 治理 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境邊界 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構|供應商轉接器和工具存取邊界|
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec |規格|技能/工具範圍與追蹤封裝 |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension |規格|能力調度與執行環境執行請求邊界|
| AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract |規格|執行環境證據與擴充封裝邊界|

## 14. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-06 | 依 PR #79 merge 證據（eb2d8591aa0bfb8aa5a3b23bfacc8cd4747e52b0）升級為 Approved Specification；正式核准 Capability / Tool Registry Adapter Contract，作為 #71 後續 provider-neutral 符合性 tests 的能力 / 工具登錄表邊界依據 | Codex |
| 0.1.0 | 2026-09-06 | 建立 Capability / Tool Registry Adapter Contract Draft，定義能力 discovery 結果、工具結構描述 versioning、政策 allow/deny 分離、fail-closed 規則、工具執行 trace 參考與中繼資料擴充封裝 | Codex |
