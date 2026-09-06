---
doc-id: AEOS-SPEC-002
doc-name: Agent Collaboration Model Architecture Spec
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-066
  - AEOS-ISSUE-067
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-001
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-013
  - AEOS-SPEC-001
---

# AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec

## 執行摘要

本規格定義 AEOS 的 runtime-neutral Agent Collaboration Model。它建立 Role Archetype、Agent Profile、Capability、Skill / Tool、Policy、Task Decomposition、Separation of Duties、Independent Verification / Reality Checker、Quality Gates、Evidence / Confidence / Provenance、Information Lifecycle、Human Approval / Escalation 與 Runtime Neutral 執行邊界的共同語意。

本規格承接 AEOS-ADR-005 的歸屬決策：Agent Collaboration 治理歸屬於 AEOS。Agent Control Plane 執行 AEOS 政策；Agent Runtime 僅執行已授權要求並回傳執行證據；產品儲存庫僅做採用對應，不建立平行多代理治理。

本文件不指定任何具名執行環境、Harness、模型供應商、工具供應商、工作流程引擎、向量資料庫、CRM 產品、部署拓撲、SDK、API 實作或正式環境操作。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-002 |
| 文件名稱 | Agent Collaboration Model Architecture Spec |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-05 |
| 最後更新 | 2026-09-05 |
| 依據文件 | AEOS Issue #66、AEOS Issue #67、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013 |
| 關聯文件 | AEOS-ARCH-001、AEOS-ARCH-007、AEOS-ARCH-009、AEOS-SPEC-001 |

## 1. 目的

本規格目的為：

- 建立 AEOS Agent Collaboration 的共同概念模型。
- 定義多代理協作的治理責任與執行邊界。
- 支援任務分解、角色指派、獨立驗證、品質關卡與升級處理。
- 確保協作治理不依賴任一執行環境、Harness、供應商、工作流程引擎或產品儲存庫。
- 提供下游產品儲存庫的採用對應基準。

## 2.範圍

### 2.1 在範圍內

- 角色原型。
- Agent Profile。
- 能力。
- 技能/工具。
- 政策。
- 任務分解。
- Separation of Duties。
- 獨立驗證/事實查核。
- 品質關卡。
- 證據/信心/出處。
- 資訊生命週期。
- 人員核准/升級。
- 執行環境中立執行邊界。
- 產品採用對應約束。

### 2.2 超出範圍

- 原始碼、SDK、API、結構描述實作或執行環境轉接器實作。
- Production 部署、啟用、操作、監控設定或基礎設施設計。
- 任何具名執行環境、Harness、供應商、模型、工具平台、向量資料庫、工作流程引擎或 CRM 儲存庫選擇。
- 產品特定提示詞、角色設定、領域工作流程、銷售流程、客戶服務腳本或 UI 行為。
- YCRM 下游採用執行；此類採用 MUST 在 AEOS 核准前持續維持封鎖此模型已核准或另行宣告可採用。

## 3. 管理機構

|權威|角色 |
|---|---|
| AEOS-ADR-005（Approved 1.0.0） | Agent Collaboration 歸屬決策 |
| AEOS-ADR-003 | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 | Enterprise AI Agent Architecture、Execution Contract 與證據邊界 |
| AEOS-ARCH-007 |能力優先的架構規則|
| AEOS-ARCH-009 |依賴方向與明確依賴治理 |
| AEOS-SPEC-001 | Local-first 路由設定檔與升級證據參考 |

規則：

- AEOS 擁有 Agent Collaboration 治理權（AEOS-ADR-005 Approved 1.0.0）。
- Agent Control Plane 執行 AEOS 政策。
- Agent Runtime 僅執行授權請求。
- 產品儲存庫僅負責採用對應；他們不會重新定義治理。

## 4.概念模型

### 4.1 角色原型

Role Archetype 是 AEOS 定義的抽象協作角色，用於描述代理在協作中的責任型態，而非具名角色設定、提示詞、模型或執行環境。

角色原型 SHOULD 定義：

|領域|意義|
|---|---|
|角色原型 ID |穩定的角色識別碼 |
|使命 |協作中的角色目的 |
|允許的責任 |該角色可能承擔的職責 |
|禁止的責任 |這個角色不該承擔的責任|
|所需能力|執行該角色所需的能力等級 |
|獨立性要求|適用時的分離或衝突規則 |
|證據責任|該角色必須提供的證據 |

最小原型 MAY 包括計劃者、執行者、審查者、事實查核者、核准者助理、協調者和觀察者，但產品採用 MUST 將這些對應為 AEOS 原型，而不是重新定義治理。

### 4.2 Agent Profile

Agent Profile 將一個邏輯代理身分與允許的角色原型、能力範圍、策略約束和證據義務綁定。

Agent Profile MUST NOT 被視為執行環境進程、模型身分、提示檔案或供應商帳戶。

Agent Profile SHOULD 包括：

|領域|意義|
|---|---|
| Agent Profile ID |穩定的邏輯同一性 |
|所有者/責任參考|負責任的所有者或治理參考|
|允許的角色原型 |此設定檔可能承擔的角色 |
|能力範圍| Approved 能力等級 |
|工具/技能範圍|依操作允許的工具或技能類別 |
|記憶體/資料範圍|讀/寫/保留約束|
|核准要求 |人工或系統核准依賴關係 |
|證據要求|日誌、引文、測試、審查筆記或其他證據 |
| 生命週期狀態 |候選項目 / Active / Deprecated / 退休 |

### 4.3 能力

能力描述了代理可以在策略下負責任地執行什麼，獨立於執行環境如何實作它。

能力 MUST 在對應到技能、工具、模型、供應商或產品工作流程之前在能力層級進行定義。

能力分類 SHOULD 至少區分：

|能力等級|意義|
|---|---|
|推理|根據政策分析、推論、比較、決策 |
|規劃|分解工作、排序任務、辨識依賴關係 |
|執行 |透過工具或工作流程執行授權操作 |
|驗證|驗證輸出、檢查證據、挑戰假設 |
|檢索|存取已核准知識或資料來源|
|記憶體管理|讀/寫受控記憶體及其來源 |
|通訊 |準備面向使用者或系統導向的訊息 |
|升級 |觸發人工審查或封鎖狀態 |

### 4.4 技能/工具

技能/工具是代理在 Execution Contract 約束下可用的可執行或可呼叫的能力表面。

工具可用性 MUST NOT 表示授權。

技能/工具對應 MUST 定義：

- 操作類別：觀察/讀取、建立/寫入、更新/變異、刪除/破壞、執行、部署/啟動、憑證/權限管理；
- 允許的角色原型和代理設定檔；
- 需要核准或品質關卡；
- 資料敏感度和保留限制；
- 呼叫後回傳的證據；
- 當特定於產品或特定於供應商時，供應商/轉接器邊界。

### 4.5 政策

策略定義了由 Agent Control Plane 評估或執行的協作規則。

保單 SHOULD 保障範圍：

- 錄取標準；
- 角色分配限制；
- Separation of Duties;
- 能力和工具範圍；
- 模型/執行環境/供應商類別約束；
- 資料敏感度和資訊生命週期；
- 核准和升級；
- 品質關卡；
- 證據和來源；
- 撤銷、取消和封鎖狀態。

執行環境本機設定 MAY 增加更嚴格的保障措施，但 MUST NOT 放寬 AEOS 政策。

## 5. 協作生命週期

AEOS 管理的協作 SHOULD 遵循以下生命週期：

1. 攝取：Control Plane 接收任務/意圖和情境。
2. 准入：Control Plane 評估政策、風險、資料敏感度、權威性和準備度。
3.分解：計劃者或協調員在授權範圍內分解工作。
4. 指派：Control Plane 或授權 Harness 分配角色原型和代理設定檔。
5. 執行：Runtime / Harness 僅執行授權的子請求。
6. 驗證：獨立驗證者/事實查核者驗證產出和證據。
7. Quality Gate：Control Plane 或授權審查邊界評估完成標準。
8.升級/核准：當政策需要或信心不足時，需要人工核准。
9. 完成：回傳結果、證據、來源和信心水準。
10. 資訊生命週期處理：產出物、記憶體寫入、保留上下文和處置遵循策略。

## 6.任務分解

任務分解將任務轉化為具有角色、能力、依賴性、品質和證據要求的有界子任務。

每個治理子任務 SHOULD 定義：

|領域|意義|
|---|---|
|子任務 ID |稽核關聯的穩定身分 |
|父任務 ID |連結到父任務 |
|目標|預期結果 |
|指派的角色原型|所需的協作角色 |
| Agent Profile |授權的設定檔或選擇限制|
|所需能力|能力等級|
|授權技能/工具範圍|允許的操作 |
|輸入邊界 |允許資料和上下文 |
|輸出邊界 |預期結果形狀 |
| Quality Gate |完成/驗收標準|
|證據要求 |所需的證據、引用、測試、審查或產出物 |
|升級觸發|需要人工或更高階別審查的條件 |

任務分解 MUST NOT 將權限擴展到父級 Execution Contract 之外。

## 7. Separation of Duties

Separation of Duties 防止一個代理、角色或執行環境在同一治理決策中承擔不相容的職責。

AEOS 協作策略 SHOULD 定義不相容的組合，包括：

|組合 |預設規則 |
|---|---|
|策劃者與最終審查者 | SHOULD 因高風險工作而被隔離 |
|執行者與獨立驗證者 | MUST 需要獨立驗證時分開 |
|證據製作者與唯一證據法官| SHOULD 分開 |
|工具變異者和破壞性操作核准者| MUST 因高風險或不可逆轉的行為而被分離 |
|產品採用作者與 AEOS 治理審查者 | MUST 分開 |

如果無法實作分離，任務 MUST 升級或記錄已核准異常，範圍和證據有限。

## 8. 獨立驗證/現實檢查

獨立驗證驗證輸出是否真實、完整、授權和有證據支援。

Reality Checker 是一個專門從事挑戰、矛盾檢測、來源驗證、假設測試和風險顯現的角色原型。

驗證 SHOULD 檢查：

- 事實可追溯到已核准來源或明確標記為不確定；
- 聲稱的工作已實際完成；
- 證據支援結論；
- 沒有繞過任何政策、核准或範圍邊界；
- 產出符合品質要求；
- 當 AEOS 權限尚未已核准時，下游採用主張被阻止或限制。

當需要獨立性時，事實查核者 MUST NOT 與執行者俱有相同的邏輯決策權限。

## 9. 品質關卡

品質關卡定義了協作結果在被接受、提升、合併、用於下游採用或完成升級之前必須通過的條件。

Quality Gate 類型 MAY 包括：

|關卡類型|最低限度的意義|
|---|---|
|政策之門|政策、審查、授權均符合 |
|證據門|所需證據已存在且可追蹤 |
|驗證門|獨立驗證已通過或異常已核准 |
|範圍門|輸出保持在授權任務和產品範圍內 |
|信心水準關卡|信心達到門檻或升級|
|來源關卡|記錄來源、執行環境、工具和記憶體沿襲 |
|人員核准門|附上所需的人員核准並確定範圍 |

Quality Gate 失敗 MUST 產生失敗證據以及升級或阻止狀態。

### 9.1 關卡結果合約

每一個 Quality Gate MUST 回傳 one of the following stable 結果 states：

|狀態|意義|所需的下一步行動|
|---|---|---|
| PASS | 有充分證據滿足關卡要求 | 繼續下一個關卡或完成作業 |
| NEEDS_WORK | 輸出不完整、無效或不充分，但可在授權範圍內修正 | 將範圍受限的修正要求退回已指派角色／子任務 |
| HUMAN_APPROVAL | 缺少人員決策或權限時，關卡無法通過 | 附上決策範圍與證據，升級為人員核准 |
| REJECTED |輸出違反政策、範圍、證據或品質要求，不應根據目前請求進行修復 |停止受影響的路徑並記錄拒絕證據|
| BLOCKED | 無法取得必要輸入、權限、事實來源、相依項目、核准或能力 | 暫停受影響的路徑，並記錄阻礙、擁有者及解除條件 |

關卡結果 MUST 包括門 ID、評估者角色、證據參考、信心水準、原因和下一步。

### 9.2 重試/回傳循環

僅當父級 Execution Contract 仍允許更正時，`NEEDS_WORK`MAY 才回傳計劃者、執行者、研究者、生成者或其他授權角色。

重試/回傳循環 MUST 保留：

- 父任務和子任務關聯識別；
——原授權範圍；
- 品質關卡失敗原因；
- 最大重試或預算限制；
- 重新評估所需的證據；
- 多次修正失敗時觸發升級。

執行環境／Harness MAY 執行重試請求，但 MUST NOT 決定失敗的關卡已通過，除非 Control Plane 策略明確授權或指派的驗證權限。

## 10. 證據/信心水準/出處

每個受治理的協作 MUST 都會產生足夠的證據來審查已決定、執行、驗證和升級的內容。

最低證據 SHOULD 包括：

- 任務、子任務和執行相關 ID；
- 使用的角色原型和代理設定檔；
- 政策背景和核准狀態；
- 分解決策與分配理由；
- 工具/模型/記憶體/資料使用情況匯總；
- 來源參考與出處；
- 信賴度和理由；
——驗證結果；
- 品質關卡狀態；
- 升級、失敗、封鎖或人工核准證據。

信心水準 MUST 有證據支援或明確標記為未知。信心水準 MUST NOT 用於推翻核准或政策要求。

出處 SHOULD 區分：

|來源類型 |意義|
|---|---|
|來源出處|事實或內容來自哪裡 |
|執行來源|執行哪個執行環境/Harness/供應商類別 |
|決策來源|哪個政策、角色或核准導致了決策 |
|記憶來源|讀取、寫入、保留或丟棄的內容 |
|產品對應來源 |應用了哪種特定於產品的對應 |

### 10.1 協作追蹤信封

每個受治理的協作 SHOULD 都會產生一個協作追蹤信封或等效的稽核記錄。

最小追蹤封裝欄位 SHOULD 包括：

|領域|意義|
|---|---|
| `trace_id` |協作的穩定追蹤身分 |
| `parent_task_id` |父任務或工單身分 |
| `sub_task_id` |適用時的子任務識別 |
| `correlation_id` |跨執行環境/跨Harness稽核關聯身分|
| `role_archetype_id` | AEOS 使用的角色原型 |
| `agent_profile_id` |邏輯 Agent Profile 恆等或選擇參考|
| `capability_class` |能力等級運動 |
| `policy_context_ref` | AEOS 政策/治理背景參考 |
| `execution_contract_ref` | Execution Contract 或委託合約參考 |
| `source_of_truth_refs` | Approved 用於驗證的來源參考 |
| `tool_model_memory_summary` |工具、模型、記憶體和資料使用情況總結 |
| `gate_results` | 使用 §9.1 定義的 Quality Gate 狀態 |
| `confidence` |信心水準值與理由|
| `provenance_refs` |來源、執行、決策、記憶體與產品對應出處 |
| `approval_evidence_ref` |需要時提供人/系統核准證據 |
| `final_status` | 已完成 / needs_work / escalated / 已拒絕 / 已封鎖 / cancelled |

Trace Envelope MUST NOT 包含不必要的秘密、原始憑證資料或未經核准的敏感資料。

### 10.2 事實來源驗證

獨立驗證 MUST 當存在事實來源時，根據已核准事實來源檢查聲明。

如果不存在事實來源，則驗證結果 MUST 說明缺失的權威並根據策略回傳`BLOCKED`、`HUMAN_APPROVAL`或明確範圍的不確定性結果。

## 11. 資訊生命週期

合作資訊 MUST 從獲取到處置或推廣均受到管理。

資訊生命週期狀態 SHOULD 包括：

|狀態|意義|
|---|---|
|攝取背景|為任務收到的使用者、系統或儲存庫上下文 |
|工作環境|執行期間使用的臨時情境 |
|證據神器|稽核或審查所需的保留證據 |
|候選項目記憶|擬議的記憶或知識更新待驗證|
| Approved 記憶/知識 |具有權威和出處的促進記憶或知識|
|廢棄/編輯的上下文 |根據政策刪除上下文 |

執行環境本機上下文、聊天歷史記錄、檢索的內容或向量狀態 MUST NOT 自動成為企業知識。提升需要符合政策的出處、權限和核准。

## 12. 人員核准/升級

人員核准是一項具有範圍、權威和證據的治理決策；它不僅僅是一個 UI 互動或執行環境標誌。

升級 MUST 發生在：

- 政策需要人的核准；
- 風險、資料敏感度、權限或核准狀態未知；
- 信心水準低於閾值；
- 品質關卡失敗；
- Separation of Duties無法滿足；
- 驗證與執行器輸出相矛盾；
- 請求的行動涉及破壞性、正式環境、憑證性、受保護性或不可逆轉的邊界；
- 產品採用取決於未經核准的 AEOS 治理產出物。

升級證據 SHOULD 包括原因碼、阻塞條件、請求決策、授權範圍已核准以及後續執行約束。

## 13. 執行環境中立執行邊界

Agent Collaboration MUST 透過 AEOS-ARCH-013 相容 Execution Contract 語意執行。

Control Plane 職責：

- 評估准入、角色分配、分解政策、品質關卡和核准要求；
- 建立或授權協作請求/子請求；
- 定義證據、信賴度、出處和生命週期要求；
- 需要時撤銷或取消執行權限。

執行環境／Harness職責：

- 僅執行授權請求或委託子請求；
- 保留父約束；
- 強製或拒絕不受支援的合約語意；
- 避免擴大角色、能力、工具、模型、記憶體、資料或憑證範圍；
- 傳回執行證據、狀態、錯誤、信心水準和出處訊號。

執行環境／Harness MUST NOT：

- 建立企業協作治理；
- 涵蓋 AEOS 政策；
- 核准其自身的高風險產出；
- 決定真相、記憶提升、核准或最終關卡結果，除非透過 AEOS 政策和 Execution Contract 明確指定該權限；
- 將產品工作流程轉化為 AEOS 權限；
- 將供應商能力視為許可。

## 14. 產品採用對應

產品儲存庫 MAY 建立從 AEOS Agent Collaboration Model 到產品工作流程的採用對應。

採用對應 SHOULD 包括：

|測繪區|要求 |
|---|---|
|產品作用|對應到 AEOS 角色原型 |
|產品代理設定 |對應到 AEOS Agent Profile 約束 |
|產品工作流程步驟|對應到任務分解子任務|
|產品工具/整合|對應到技能/工具範圍和供應商邊界|
|產品認可|對應到 AEOS 核准和升級語意|
|產品證據 |對應到 AEOS 證據/信賴度/出處要求 |
|產品記憶體/資料|對應到資訊生命週期和權限邊界|

當 AEOS 產出物保持 Draft 或未核准時，採用對應 MUST 識別 AEOS 依賴關係和阻塞狀態。

## 15. 一致性檢查表

聲稱符合本規範的採用或實作 SHOULD 表明：

1. AEOS 仍然是協作治理的所有者。
2.角色原型和 Agent Profile 與執行環境、模型和供應商身分分離。
3. 能力在技能/工具/供應商對應之前定義。
4. 任務分解保留父級授權範圍。
5. 定義並執行或升級 Separation of Duties 規則。
6. 獨立驗證/事實查核器在需要時是獨立的。
7. 品質關卡產生通過/失敗/阻止的證據。
8. 收集證據、信賴度和來源以供檢討。
9. 資訊生命週期防止執行環境本機記憶體自動成為權威。
10. 人員核准已經確定了決策證據的範圍。
11. Runtime / Harness 僅執行授權請求。
12. 除非明確授權，否則執行環境／Harness 不會決定真相、記憶體提升、核准或最終關卡結果。
13. Gate 狀態和重試/回傳循環使用§9.1 和§9.2 中的穩定合約。
14. 存在協作追蹤信封或同等審查記錄。
15. 產品儲存庫提供採用對應，而非並行治理。

## 16. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #66 | GitHub Issue | Agent Collaboration Model Architecture Spec 工作來源 |
| AEOS Issue #67 | GitHub Issue | Agent Collaboration 歸屬決策工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR |所有權與權限決定|
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Runtime-neutral Control Plane / 執行環境分離 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Execution Contract、Control Plane 與執行環境邊界 |
| AEOS-ARCH-007 — Capability Architecture |架構|能力優先架構權威|
| AEOS-ARCH-009 — Dependency Architecture |架構|明確依賴與反反向控制權限|
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile |規格|路由、升等與證據參考 |

## 17. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 #66 post-#67 提升：在 AEOS-ADR-005 Approved 1.0.0 歸屬決策下，升級為 Approved Specification；補明 Gate Result Contract、Retry / Return Loop、Collaboration Trace Envelope、Source of Truth Verification 與 Runtime 不得決定 truth / 記憶體提升 / 核准 / 最終關卡結果的邊界 | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Collaboration Model Architecture Spec draft，涵蓋角色 archetype、代理設定檔、能力、skill/工具、政策、任務分解、Separation of Duties、獨立驗證、品質關卡、證據/confidence/來源、information 生命週期、human 核准/升級處理與 runtime-neutral 執行邊界 | Codex |
