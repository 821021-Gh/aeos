---
doc-id: AEOS-SPEC-003
doc-name: Agent Control Plane Orchestration Extension
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-068
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-001
  - AEOS-SPEC-002
---

# AEOS-SPEC-003 — Agent Control Plane Orchestration Extension

## 執行摘要

本規格定義 AEOS Agent Control Plane 的 Agent Collaboration 編排擴充。它將 AEOS-SPEC-002 的 Agent Collaboration Model 操作化為 Control Plane 可治理的准入、任務分解、設定檔 resolution、能力 dispatch、品質關卡執行、`NEEDS_WORK` 回傳 loop、human 核准升級處理、執行環境轉接器執行要求與協作 trace emission。

本規格不實作 Control Plane、不定義執行環境轉接器程式、不選擇任何 Harness、執行環境、模型、供應商、工作流程引擎、向量資料庫或產品儲存庫。它只定義 Control Plane 在執行 AEOS 政策時必須持有或協調的編排語意。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-003 |
| 文件名稱 | Agent Control Plane Orchestration Extension |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #68、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-002 |
| 關聯文件 | AEOS-SPEC-001、AEOS-SPEC-002 |

## 1. 目的

本規格目的為：

- 定義 Agent Control Plane 如何依 AEOS Agent Collaboration Model 執行編排。
- 定義任務分解、角色選擇、設定檔 resolution 與能力 dispatch 的治理邊界。
- 定義 Gate 執行器與共通關卡決策結構描述。
- 定義 `NEEDS_WORK` 回傳 loop 與 `HUMAN_APPROVAL` 升級處理語意。
- 定義 Runtime 轉接器執行要求的最小邊界。
- 定義 Agent Collaboration Run、Role Assignment 與 Gate Decision trace emission。

## 2.範圍

### 2.1 在範圍內

- 協調者的職責。
- Agent Profile 解析度。
- 能力派遣。
- 角色選擇。
- 關卡執行器。
- 門決策模式。
- `NEEDS_WORK`回傳循環。
- 人工核准升級。
- Runtime 轉接器執行請求邊界。
- Agent Collaboration 運行追蹤。
- 角色分配追蹤。
- 門決策追蹤。

### 2.2 超出範圍

- 原始碼、SDK、API endpoint 或資料庫結構描述實作。
- Runtime 轉接器實作。
- CRM Memory、Learning Case、Knowledge 或任何產品領域能力實作。
- Production 部署、客戶資料 mutation、憑證 provisioning 或執行環境 operations。
- 任一具名執行環境、Harness、供應商、模型、工作流程引擎、工具平台或向量資料庫選擇。
- 將 Control Plane 變成產品事實來源。

## 3. 管理機構

|權威|角色 |
|---|---|
| AEOS-ADR-005 | Agent Collaboration 歸屬決策 |
| AEOS-ADR-003 | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 | Enterprise AI Agent Architecture、Execution Contract 與證據邊界 |
| AEOS-SPEC-001 | Local-first 路由設定檔、升級與路由遙測 |
| AEOS-SPEC-002 | Agent Collaboration Model，關卡結果合約與追蹤封裝 |

規則：

- Agent Control Plane 執行 AEOS 政策。
- Agent Runtime 僅執行授權請求。
- 執行環境成功不是事實來源驗證成功、記憶體升級、核准或關卡通過。
- 產品儲存庫僅進行採用對應。

## 4. 協調者職責

Agent Control Plane Orchestrator 是邏輯上的 Control Plane 責任。它 MAY 由一個或多個服務或元件實作，但其權限仍然是 AEOS-ARCH-013 下的 Control Plane 權限。

協調者 MUST：

1. 接收任務/意圖和情境；
2.評估錄取政策；
3. 將風險、資料敏感度、複雜性和核准要求分類；
4. 將承認的任務分解為受治理的子任務；
5. 選擇所需的角色原型；
6. 解決合格的代理資料；
7. 派遣授權能力；
8. 建立執行環境轉接器執行請求；
9. 運行或協調品質關卡；
10. 處理`NEEDS_WORK`回傳迴圈；
11.升級`HUMAN_APPROVAL`和封鎖狀態；
12. 發出 Agent Collaboration 運行、角色分配和門決策追蹤。

協調者 MUST NOT：

- 成為產品事實來源；
- 取代事實來源驗證；
- 覆蓋 AEOS-ADR-005 所有權；
- 讓執行環境輸出直接決定核准、真相、記憶提升或最終關卡結果；
- 利用下游產品採用壓力來繞過 AEOS 政策。

## 5.編排生命週期

AEOS 管理的編排 SHOULD 遵循以下生命週期：

1. **接收**：接收任務、情境和請求者身分。
2. **准入**：評估 AEOS 政策、風險、資料敏感度、審查和權限。
3. **規劃**：建立有界任務分解。
4. **角色選擇**：選擇每個子任務所需的角色原型。
5. **設定檔解析**：解析符合條件的代理程式設定檔。
6. **能力調度**：選擇能力等級和允許的技能/工具範圍。
7. **執行請求**：在 Execution Contract 約束下建立執行環境轉接器請求。
8. **證據收集**：收集執行環境執行證據和來源/出處引用。
9. **門評估**：運行政策、證據、驗證、信心和核准門。
10. **退貨/升級**：退貨`NEEDS_WORK`、升級`HUMAN_APPROVAL`、根據需要拒絕或阻止。
11. **完成**：發出最終痕跡和結果證據。

## 6.任務分解與角色選擇

每個分解的子任務 MUST 保留父級授權範圍，MUST NOT 引入更廣泛的工具、模型、記憶體/資料、憑證、執行環境或產品權限。

每個子任務 SHOULD 定義：

|領域|意義|
|---|---|
| `sub_task_id` |穩定的子任務身分 |
| `parent_task_id` |父任務或協作運行識別 |
| `goal` |預期結果 |
| `role_archetype_id` |必需的 AEOS 角色原型 |
| `capability_class` |所需能力等級|
| `input_scope` |允許的上下文與資料邊界 |
| `output_scope` |預期結果邊界|
| `tool_scope` |允許的技能/工具操作範圍 |
| `quality_gates` |所需的登機口 ID 和通過標準 |
| `evidence_requirements` |所需證據與出處|
| `escalation_triggers` |人員核准或封鎖狀態的條件 |

角色選擇 MUST 是基於任務需求、策略、風險、資料範圍、所需能力和職責分離約束。角色選擇 MUST NOT 僅基於產品角色命名或執行環境可用性。

## 7. Agent Profile 分辨率

設定檔解析將所需的角色原型和能力類別對應到符合條件的代理程式設定檔。

設定檔解析度輸入 SHOULD 包括：

|輸入 |要求 |
|---|---|
|任務/子任務範圍 | MUST 適合設定檔允許的責任 |
|角色原型| MUST 被設定檔允許 |
|能力等級| MUST 在設定檔能力範圍內 |
|工具/技能範圍| MUST 取得操作類別授權 |
|資料範圍 | MUST 滿足資料敏感度與保留限制 |
|政策背景| MUST 滿足 AEOS 政策及審查規則 |
|執行環境限制 | MUST 適合允許的執行環境類別、隔離、預算和證據要求 |
| Separation of 職責 | MUST 避免不相容的角色/設定檔組合 |

如果不存在符合條件的設定文件，Orchestrator MUST 根據策略回傳 `BLOCKED` 或 `HUMAN_APPROVAL`。它 MUST NOT 默默地回退到更廣泛的設定檔。

## 8.能力調度

能力調度為子任務選擇授權的能力類別和允許的技能/工具操作。

發送 MUST 驗證：

- 子任務所需的能力；
- 選擇 Agent Profile 可以執行該能力；
- 要求的技能/工具操作在授權操作範圍內；
- 滿足資料、模型、記憶體和憑證邊界；
- 執行環境類別可以表達所需的證據和取消語意；
- 未知功能、未知架構或不支援的版本失敗即封鎖。

執行環境或工具發現的能力可用性 MUST NOT 意味著政策允許。

## 9. 跑門者

Gate 執行器是 Control Plane 的職責，評估或協調 AEOS-SPEC-002 定義的品質關卡。

Gate 執行器 MUST 至少支援：

|關卡類型|要求 |
|---|---|
|政策之門|驗證政策、核准與授權 |
|證據門|驗證所需證據是否存在 |
|驗證關卡|確認獨立驗證結果 |
|範圍門|確認輸出保持在任務和產品邊界內 |
|信心水準關卡|檢查信心水準門檻或升級 |
|來源關卡|確認來源、執行、決策與記憶體來源 |
|人員核准門|需要時確認範圍內的人員決策 |

Gate 執行器 MAY 消耗執行環境證據，但 MUST NOT 將執行環境成功視為關卡通過本身。

## 10. 門決策模式

每個門決策 MUST 發出 provider-neutral 門決策軌跡，至少：

|領域|意義|
|---|---|
| `gate_decision_id` |穩定的門決策身分|
| `trace_id` |協作追蹤身分|
| `gate_id` |門身份|
| `gate_type` |政策/證據/驗證/範圍/信心水準/出處/人員核准|
| `state` | PASS / NEEDS_WORK / HUMAN_APPROVAL / REJECTED / BLOCKED |
| `evaluator_role` |角色原型或執行評估的人員權威|
| `evaluated_output_ref` |評估的輸出或產出物 |
| `evidence_refs` |用於決定的證據 |
| `source_of_truth_refs` |適用時的真相參考來源 |
| `confidence` |信心水準值與理由|
| `reason` |人員可讀的決策原因 |
| `next_action` |繼續/回傳/升級/停止/暫停 |
| `created_at` |決策時間戳|

`HUMAN_APPROVAL` MUST NOT 透過模型文字、執行環境元資料或本機Harness設定重寫為 `PASS`。只有經過授權的人員/系統核准機構才能滿足人員核准門。

## 11. NEEDS_WORK 回傳循環

當關卡回傳 `NEEDS_WORK` 時，Orchestrator MAY 僅在原始或縮小後的授權範圍內，將工作退回已指派的角色／能力。

退貨要求 MUST 包括：

- 失敗的關卡 ID 和決策 ID；
- 糾正指導；
- 目標角色原型和合格的設定檔限制；
- 允許的能力和工具範圍；
- 重新評估所需的證據；
- 重試/預算限制；
- 升級觸發。

當超過策略定義的重試、信賴度、預算或品質閾值時，重複`NEEDS_WORK`MUST 升級。

## 12. 人工審查升級

`HUMAN_APPROVAL`是一種治理狀態。 MUST NOT 可以透過執行環境輸出、模型信心水準、供應商成功或工具完成單獨解決。

升級請求 SHOULD 包括：

|領域|意義|
|---|---|
| `approval_request_id` |穩定的核准請求身分 |
| `trace_id` |協作追蹤身分|
| `reason_code` |核准或升級原因 |
| `requested_decision` |需要什麼決定|
| `scope` | Authorized 範圍 if 已核准 |
| `evidence_refs` |可供核准者使用的證據 |
| `risk_classification` |風險基礎|
| `data_sensitivity` |資料邊界|
| `expiry_or_revalidation` |需要重新驗證的時間或條件 |

在受影響的執行繼續之前附上核准證據 MUST。

## 13. Runtime Adapter 執行請求邊界

Runtime 轉接器請求是發送到執行環境／Harness/轉接器的 Control Plane 發出或 Control Plane 授權執行請求。

最小請求欄位 SHOULD 包括：

|領域|意義|
|---|---|
| `execution_request_id` |穩定的請求身分 |
| `trace_id` |協作追蹤身分|
| `sub_task_id` |目標子任務 |
| `agent_profile_ref` |授權 Agent Profile 參考|
| `role_archetype_id` |分配的角色原型|
| `capability_class` |授權能力|
| `tool_scope` |允許經營範圍|
| `model_scope` |允許的模型能力等級 |
| `memory_data_scope` |讀/寫/保留邊界|
| `credential_scope` |憑證或委派能力邊界 |
| `runtime_constraints` |逾時、預算、重試、隔離與取消 |
| `evidence_requirements` |所需的執行環境證據 |

本規範未完整定義 Runtime 轉接器回應；provider-neutral 執行證據預計由後續執行環境證據合約問題管轄。

## 14. 微量發射

Orchestrator MUST 發出或協調與 AEOS-SPEC-002 協作追蹤信封對齊的追蹤記錄。

最小痕跡類別：

|追蹤|目的|
|---|---|
| Agent Collaboration 奔跑 |記錄運行身分、請求者、策略情境、最終狀態和完成證據 |
|角色指派|記錄選定的角色原型、設定檔解析與職責分離基礎 |
|能力派遣|記錄所選功能、工具範圍和策略允許/拒絕決策 |
|執行環境執行請求|記錄傳送到執行環境/轉接器的授權請求邊界|
|門決定|記錄關卡結果與下一步行動|
|核准升級 |記錄人員核准要求和結果參考|

追蹤 MUST 最大限度地減少敏感資料，MUST NOT 包括秘密、原始憑證、不必要的完整提示、思路或未經核准的原始客戶內容。

## 15. 一致性檢查表

聲稱符合此規範的 Control Plane 編排實作 SHOULD 演示：

1. Orchestrator 根據任務、風險、資料範圍和策略選擇角色/設定檔/能力。
2. 設定檔決議從來不會默默擴大權限。
3. 能力調度將可用性與策略允許/拒絕分開。
4. Gate 執行器採用 provider-neutral 門決策模式。
5. `NEEDS_WORK` 傳回具有重試和升級約束的有界角色/能力。
6. `HUMAN_APPROVAL`不能被模型或執行環境輸出覆蓋。
7. Runtime 轉接器請求受 Execution Contract 和證據要求的限制。
8. 路由、門和核准追蹤與 AEOS-SPEC-002 追蹤封裝線對齊。
9. 執行環境成功不解釋為驗證通過。
10. 產品儲存庫對應不會成為 Control Plane 事實來源。

## 16. 狀態與核准

本規格目前為 **Approved 1.0.0**。

PR #75 已合併至 `main`，merge commit 為 `515f9fb663f2f1d27e66e68165ef34c03e9a3e42`。此合併作為 Repository Owner 最終核准證據，正式核准本規格為 Agent Control Plane Orchestration Extension 的 Specification。

核准後：

- Agent Control Plane 可依本規格執行 Agent Collaboration 編排語意。
- #69 / #70 / #71 可在本規格邊界下分別處理執行環境證據契約、能力 / 工具登錄表轉接器契約與 provider-neutral 符合性 tests。
- 本規格不授權執行環境實作、Production 部署、客戶資料 mutation 或 YCRM 下游採用。

## 17. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #68 | GitHub Issue | Agent Control Plane Orchestration Extension 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS 擁有 runtime-neutral Agent Collaboration 治理 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境分離 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Agent Control Plane、Execution Contract 與執行環境邊界 |
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile |規格|路由與升等參考|
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec |規格|協作模型、關卡結果契約與追蹤封裝 |

## 18. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 PR #75 merge 證據（515f9fb663f2f1d27e66e68165ef34c03e9a3e42）升級為 Approved Specification；正式核准 Agent Control Plane Orchestration Extension 規格，作為 #69 / #70 / #71 後續執行環境轉接器與符合性工作的 Control Plane 邊界依據 | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Control Plane Orchestration Extension Draft，定義任務分解、設定檔 resolution、能力 dispatch、關卡執行器、關卡決策結構描述、NEEDS_WORK 回傳 loop、human 核准升級處理、執行環境轉接器要求邊界與協作 trace emission | Codex |
