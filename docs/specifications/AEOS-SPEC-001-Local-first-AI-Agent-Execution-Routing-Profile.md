---
doc-id: AEOS-SPEC-001
doc-name: Local-first AI Agent Execution Routing Profile
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-28
related:
  - EWO-AEOS-0046
  - AEOS-RPT-004
  - AEOS-ARCH-001
  - AEOS-ARCH-013
  - AEOS-ARCH-014
  - AEOS-ADR-003
  - AEOS-STD-005
  - AEOS-STD-007
  - YEOS-ENG-STD-008
  - SP-AEOS-0046-R2
---

# AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile

> EWO-AEOS-0046：建立 AEOS Local-first AI Agent Execution Routing Profile。本文件已完成 Specification Review（SP-AEOS-0046-R2）並核准為 Approved Specification；定義路由設定檔、層級語意、entry / exit 準則、升級處理原因、遙測資料與驗證要求。本文件不指定任何具名本機 LLM、雲端模型、執行環境、Harness、框架或供應商。

## 執行摘要

本規格將 AEOS-ARCH-013 已核准的 Agent Control Plane 路由、預算、失敗政策、核准、授權與稽核 responsibilities 操作化為 Local-first 執行設定檔。

Local-first 執行的目標是優先使用 deterministic rule 與本機 AI 完成低風險、可驗證、低敏感度的工作，只有在本機 confidence、上下文、能力或治理條件不足時，才升級至雲端模型、frontier 模型或 human 核准。

Local-first 是路由政策，不是執行環境架構；任何具名本機模型、雲端模型、代理 Harness 或供應商均只能出現在 Adapter / Provider / Reference Implementation / PoC 邊界，不得成為 AEOS 核心相依性。

SP-AEOS-0046-R2 已完成 Specification Review，Decision 為 APPROVED；Repository Owner 最終核准由 PR #60 Review Package 承載。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-001 |
| 文件名稱 | Local-first AI Agent Execution Routing Profile |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-27 |
| 最後更新 | 2026-08-28 |
| 依據文件 | EWO-AEOS-0046、AEOS-RPT-004、AEOS-ARCH-013、AEOS-ADR-003、AEOS-ARCH-014、AEOS-STD-007、YEOS ENG-STD-008 |
| 關聯文件 | AEOS-ARCH-001、AEOS-STD-005、AEOS-RPT-004、SP-AEOS-0046-R2 |

## 1. 目的

本規格目的為：

- 定義 Local-first AI Agent Execution 的層級模型。
- 定義每一層級的 entry 準則、exit 準則、升級處理準則、治理 guardrails 與稽核證據。
- 定義 Control Plane 路由 inputs 與路由決策輸出。
- 定義 cost / token / 延遲 / 成功 / 升級處理遙測資料的最低欄位。
- 明確保留 YEOS ENG-STD-008 的命令分類、risk 分類、核准政策與儲存庫 protection 要求。
- 明確保留 AEOS 執行環境 / Harness / 供應商 neutrality。

## 2.範圍

### 2.1 在範圍內

- Agent Control Plane 路由設定檔。
- 確定性、本機 AI、雲 AI、Frontier AI 和人工核准層。
- local-first 路由所需的 Execution Contract 擴充欄位。
- 成本治理和可觀測性遙測最小欄位。
- 升級原因分類。
- 實施儲存庫採用的驗證清單。

### 2.2 超出範圍

- 任何命名的執行環境、工具、模型、供應商、向量資料庫、工作流程引擎或工具選擇。
- 正式環境部署拓撲、主機規模、網路拓撲、KMS 或憑證設定。
- 修改 YEOS ENG-STD-008 指令/風險/核准語意。
- 產品特定的 CRM 工作流程、提示、角色、通路行為或領域模型。
- 定價、SKU、銷售或商業包裝決策。

## 3. 指導原則

| ID | 原則 | 要求 |
|----|-----------|-------------|
| LFR-001 | Local-first，僅限本機 | Control Plane SHOULD 首先嘗試最低的足夠層級，但當證據顯示較低層級不足時，MAY 升級。 |
| LFR-002 |保留治理|路由 MUST NOT 降低審查、儲存庫保護、授權、資料敏感度或稽核要求。 |
| LFR-003 |執行環境中立 |層級定義 MUST NOT 命名或要求特定的本機 LLM、雲端模型、工具、框架或供應商。 |
| LFR-004 |證據驅動的路由 |層選擇、重試與升級 MUST 產生可審查的證據。 |
| LFR-005 |成本意識執行 |當風險和品質限制允許時，路由 SHOULD 最大限度地減少雲端Token成本和人員幹預。 |
| LFR-006 |因含糊不清而失敗即封鎖 |未知的風險、未知的資料敏感性或缺少核准 MUST 升級到更安全的層級或人員核准。 |

## 4. 執行層

|等級 |名稱 |權威意義|典型用途|
|------|------|-------------------|-------------|
| 0 等級 |確定性規則|確定性規則與策略檢查下的非模型執行 |模式驗證、靜態檢查、格式化、分類、CI、基於規則的路由 |
| 1 級 |本機 AI|有界 Execution Contract 下的本機模型/本機推理供應商 |總結、提取、draft 生成、預處理、檢索輔助低風險推理 |
| 2 級 |低成本雲 AI|本機不足時選擇雲模型類進行經濟有效的推理 |在資料策略允許雲的情況下，提供更大的背景、更強的推理、低/中風險任務支援 |
|第 3 級 |Frontier Cloud AI|用於複雜推理和高上下文決策支援的最高能力模型類 |架構推理、除錯複雜、跨儲存庫分析、評審困難 |
|第 4 級 |人員核准|人員決策/核准邊界|正式環境啟用、破壞性操作、憑證、敏感資料、不可逆轉或高風險決策 |

層名稱定義路由語意和證據要求。他們不定義產品。

## 5. 路由輸入

Control Plane MUST 在選擇執行層之前評估以下輸入：

|輸入 |值/形狀|要求 |
|-------|----------------|-------------|
|風險|儲存庫-approved 風險分類，或當 YEOS 適用時對應 L/M/H/C | MUST 保留儲存庫核准基線。 |
|複雜性 |低/中/高/關鍵，或儲存庫定義的等效項 | SHOULD 考慮歧義性、所需的推理深度、跨儲存庫範圍和上下文大小。 |
|信心|確定性/高/中/低/未知| MUST 有驗證證據或明確的不確定性支援。 |
|成本|本機計算類別、Token估計、Token實際、成本估計/實際 |當治理和品質相同時，SHOULD 喜歡較低的成本。 |
|延遲 |預期/實際持續時間和佇列概況| SHOULD 滿足面向使用者和工作流程的限制。 |
|資料敏感度|公用/內部/機密/受限/未知，或儲存庫定義的等效項 | MUST 控制是否允許雲層。 |
|核准狀態 |不需要 / 待定 / 已核准 / 已拒絕 / 未知 | MUST 在執行前滿足儲存庫策略。 |
|授權範圍 |工具、模型類別、資料範圍、憑證範圍、環境範圍 | MUST 以 Execution Contract 為界。 |

## 6. 等級進入與離開條件

### 6.1 第 0 層 — 確定性規則

進入條件：

- 任務可以透過顯式規則、模式、靜態分析、策略尋找或確定性自動化來實作已完成。
- 不需要 LLM 推理。
- 輸入資料敏感度允許本機確定性處理。

離開／升級條件：

- 規則衝突或缺少規則。
- 未知的輸入類型。
- 政策模糊。
- 確定性驗證失敗並需要推理。

所需證據：

- 規則識別符/版本；
- 輸入類別；
- 輸出/決策；
- 驗證結果；
- 適用時的升級原因。

### 6.2 Tier 1 — Local AI

進入條件：

- 任務是低風險或有限的中風險準備工作。
- 工作可透過確定性檢查、審查、測試或受限輸出模式進行驗證。
- 所需資料可以保留在已核准局部邊界內。
- 本機模型能力等級足以滿足預期的複雜性。

離開／升級條件：

- 信心水準低於閾值；
- 重複無效輸出；
- 背景超越本機概況；
- 所需的方式或推理超越了本機的能力；
- 資料政策需要不同的處理路徑；
- 指揮核准要求超出本機自治當局的要求。

所需證據：

- 本機供應商類別，而不是作為架構要求的產品名稱；
- 模型能力等級；
- 提示/背景預算估算；
- 輸出模式驗證結果；
- 信心和理性；
- 重試次數；
- 適用時的升級原因。

### 6.3 Tier 2 — Low-cost Cloud AI

進入條件：

- 第 0 層/第 1 層證據顯示能力、背景或信心不足。
- 資料敏感度策略允許雲端處理。
- 任務保持在允許的風險和核准範圍內。
- 相對於預期值，成本狀況是可以接受的。

離開／升級條件：

- 雲端信心依然不足；
- 任務需要前沿推理或跨儲存庫架構判斷；
- 政策、核准或資料敏感性阻礙雲端處理；
- 成本預算將超出。

所需證據：

- 供應商等級；
- Token估算及實際使用情況（如有）；
- 成本估算與實際成本（如果有）；
- 資料敏感度決策；
- 核准狀態；
- 適用時的升級原因。

### 6.4 Tier 3 — Frontier Cloud AI

進入條件：

- 複雜的架構、偵錯、跨儲存庫推理或高上下文審查需要最高的模型能力。
- 資料敏感度與核准政策允許使用 Frontier Cloud。
- 嘗試失敗後，較低層級的能力不足或不具成本效益。

離開／升級條件：

- 請求的行動需要人工核准；
- 操作涉及正式環境、破壞性作業、憑證／KMS、敏感資料或不可逆變更；
- 對自主執行的信心仍然不足；
- 治理需要人的責任。

所需證據：

- 邊界層理由；
- 較低層嘗試或明確的繞過原因；
- Token 與成本遙測資料；
- 結果證據；
- 適用時的人員交接原因。

### 6.5 第 4 層 — 人員核准

進入條件：

- 儲存庫策略需要人工審查、人員核准或 Repository Owner 授權。
- 操作涉及正式環境部署／啟用、破壞性作業、憑證／KMS、敏感資料、不可逆變更或高風險業務決策。
- 風險、資料敏感度或核准狀態未知。

離開條件：

- 人員核准，授權範圍有限；
- 人員拒絕；
- 人員要求變更；
- 人員依據新的或修訂後的 Execution Contract，委派風險較低且範圍受限的執行。

所需證據：

- 審查者身分或審查參考資料；
- 決策時間戳記；
- 核准政策參考；
- 授權範圍；
- 決策結果；
- 適用時跟進執行契約。

## 7. 升級原因分類

|代碼|原因 |描述 |
|------|--------|-------------|
| ESC-RULE-UNKNOWN | 缺少確定性規則 | Tier 0 無法分類或完成任務。 |
| ESC-CONFIDENCE-LOW | 信心水準低於門檻 | 輸出信心水準不足以執行要求的操作。 |
| ESC-CONTEXT-LIMIT | 超出上下文限制 | 較低層級無法處理必要的上下文。 |
| ESC-CAPABILITY-GAP | 能力缺口 | 不具備必要的推理、模態或工具能力。 |
| ESC-VALIDATION-FAIL | 驗證失敗 | 輸出未通過結構描述、測試、政策檢查或審查檢查。 |
| ESC-DATA-SENSITIVITY | 資料敏感度關卡 | 資料政策阻止使用目前層級。 |
| ESC-COST-BUDGET | 成本預算關卡 | 目前或下一層級將超出預算。 |
| ESC-APPROVAL-REQUIRED | 需要核准 | 儲存庫政策要求人員審查／核准。 |
| ESC-PROTECTED-OP | 受保護的操作 | 操作涉及受保護的儲存庫或正式環境邊界。 |
| ESC-UNKNOWN-RISK | 未知風險 | 無法安全地分類風險。 |

## 8. Execution Contract 擴展

local-first Execution Contract SHOULD 包括以下附加欄位：

|領域|必填|目的|
|-------|----------|---------|
| `routing_profile_id` |是的 |識別此路由設定檔版本。 |
| `initial_tier` |是的 |為第一次嘗試所選的等級。 |
| `current_tier` |是的 |用於目前執行嘗試的層。 |
| `max_allowed_tier` | 是 | 政策與核准允許的最高層級。 |
| `risk_classification` |是的 |儲存庫-approved 風險分類或對應。 |
| `complexity_classification` |是的 |用於路由的複雜性。 |
| `data_sensitivity_classification` |是的 |資料邊界決策。 |
| `confidence_threshold` |是的 |自主完成所需的最低信心水準。 |
| `cost_budget` |是的 |Token/供應商/執行環境消耗的預算。 |
| `latency_budget` |沒有 |時間或佇列目標。 |
| `approval_state_ref` |是的 |核准證據或政策參考。 |
| `escalation_policy_ref` |是的 |管理重試與升級的策略。 |

## 9. 遙測最小欄位

每次執行嘗試 SHOULD 都會發出一個審查/可觀察事件，至少包含：

|領域|目的|
|-------|---------|
| `execution_id` |關聯一次執行的所有嘗試。 |
| `task_id` |關聯任務或工作訂單。 |
| `repository` |來源儲存庫或實作儲存庫。 |
| `routing_profile_id` |路由設定檔版本。 |
| `tier` |用於此嘗試的圖層。 |
| `provider_class` |確定性/本機/低成本雲端/Frontier Cloud端/人力。 |
| `model_capability_class` |功能等級無需特定於產品的型號識別。 |
| `risk_classification` |風險基礎。 |
| `complexity_classification` |複雜性基礎。 |
| `data_sensitivity_classification` |資料邊界基礎。 |
| `approval_state` |執行時的核准狀態。 |
| `token_estimate` |預期的Token使用情況（如果適用）。 |
| `token_actual` |實際Token使用情況（如果有）。 |
| `cost_estimate` |適用時的預期成本。 |
| `cost_actual` |實際成本（如有）。 |
| `latency_actual` |觀察到的執行延遲。 |
| `retry_count` |嘗試計數。 |
| `validation_status` |通過/失敗/跳過/不適用。 |
| `confidence` |確定性/高/中/低/未知。 |
| `escalation_reason` |升級時第 7 條之一。 |
| `final_status` | 已完成/升級/已拒絕/失敗/取消。 |

## 10. 成本治理指標

採用此設定檔的儲存庫 SHOULD 至少彙總：

|公制|描述 |
|--------|-------------|
|本機完成率|第 0 層或第 1 層的執行百分比已完成。 |
|雲端升級比例|百分比升級至第 2 級或第 3 級。 |
|前沿使用率|需要第 3 級的百分比。 |
|人員支援率|需要 Tier 4 的百分比。 |
|按層級劃分的Token消費 |按層分組的估計和實際Token使用情況。 |
|按等級劃分的成本 |按層級分組的估計和實際供應商成本。 |
|重試率|每次已完成執行的嘗試次數。 |
|升級原因分佈 | §7 原因代碼的頻率。 |
|驗證失敗率|失敗的驗證依層和任務類型分組。 |
|核准受阻率 |因未核准或已拒絕核准而阻止執行。 |

## 11. YEOS 核准對應

當儲存庫採用 YEOS ENG-STD-008 或等效策略時，路由 MUST 保留以下最低行為：

| YEOS 風險 |預設路由約束|
|-----------|----------------------------|
|左 |若另有允許且可審查，第 0 層或第 1 層 MAY 會自主執行。 |
|中號 |在執行變更本機狀態或準備影響儲存庫的變更之前需要進行人工審查。 |
|哈 |在儲存庫層級狀態變更之前需要人工核准。 |
| C |需要人員核准加上 Repository Owner 授權；未分類的命令在分類之前被視為 C。 |

此對應不會取代 YEOS ENG-STD-008。若有衝突，則以儲存庫-approved 政策為準，MAY 實施更嚴格的控制。

## 12. 執行環境中立性要求

實作 MUST：

- 透過供應商/轉接器邊界公開本機 AI、雲 AI、工具、記憶體和工作流程引擎；
- 避免將任何命名的執行環境、工具、本機模型、雲端模型或框架硬式編碼到 AEOS 核心中；
- 將特定於供應商的設定描述為實作或參考實作細節；
- 跨供應商替換保留 Execution Contract 語意；
- 將模型身分與代理身分、策略權限和核准權限分開。

## 13. 採用驗證清單

採用此設定檔的實作儲存庫 SHOULD 示範：

|檢查 |要求 |
|-------|-------------|
|層級覆蓋 |第 0 層至第 4 層處理已記錄或有意不提供理由支援。 |
|不可繞過 |本機執行無法降低審查和儲存庫保護規則。 |
|證據|每次執行嘗試都會發出最少的遙測或等效的稽核記錄。 |
|升級 |升級原因使用穩定的分類法。 |
|成本指標|可以報告本機/雲端/前沿/人員比率和Token/成本使用。 |
|轉接器邊界 |特定於供應商的程式碼與架構/策略核心隔離。 |
|資料敏感度|依資料分類，雲路由被阻止或已核准。 |
|人工切換 |第 4 層產生決策證據和有限的後續授權。 |

## 14. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS-ARCH-001 — Architecture Baseline |架構|架構入口及登記機構|
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Control Plane、Execution Plane、執行環境中立性和 Execution Contract 權威 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Approved Control Plane 決策 / 執行環境分離 |
| AEOS-ARCH-014 — Productizable Platform Architecture |架構|參考實作與平台核心邊界|
| AEOS-STD-005 — Review Standard |標準|審查工作流程與決策要求 |
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard |標準|上下文、token 預算與模型路由基準 |
| AEOS-RPT-004 — Local-first AI Agent Execution Architecture Gap Analysis |報告|基準、差距分析與實施計畫 |
| YEOS ENG-STD-008 — AI 代理命令核准標準 |外部標準|指揮分級、風險分級及核准政策|
| SP-AEOS-0046-R2 |審查紀錄 |規格審查 APPROVED；PR #60 |

## 15. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-28 | SP-AEOS-0046-R2 APPROVED；依 PR #60 Repository Owner 最終核准將規格由 Draft 0.1.0 升級為 Approved 1.0.0；路由語意未變更 | ChatGPT |
| 0.1.0 | 2026-08-27 | EWO-AEOS-0046 的初始 Draft local-first 路由設定檔規格 | Codex |
