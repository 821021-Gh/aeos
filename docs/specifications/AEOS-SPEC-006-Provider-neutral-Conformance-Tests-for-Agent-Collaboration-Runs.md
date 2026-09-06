---
doc-id: AEOS-SPEC-006
doc-name: Provider-neutral Conformance Tests for Agent Collaboration Runs
doc-type: Specification
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-06
updated: 2026-09-06
related:
  - AEOS-ISSUE-071
  - AEOS-ADR-005
  - AEOS-ADR-003
  - AEOS-ARCH-013
  - AEOS-SPEC-002
  - AEOS-SPEC-003
  - AEOS-SPEC-004
  - AEOS-SPEC-005
---

# AEOS-SPEC-006 — Provider-neutral Conformance Tests for Agent Collaboration Runs

## 執行摘要

本規格定義 AEOS Agent Collaboration Runs 的 provider-neutral 符合性 test 要求。其目的不是建立可執行測試程式，而是定義任何執行環境 / 供應商轉接器若宣稱符合 AEOS Agent Collaboration 治理，至少必須通過哪些測試資料、invariant 與證據最小化檢查。

同一 AEOS 協作 plan 在不同執行環境轉接器下，MUST preserve 關卡語意、驗證語意、角色/設定檔權限、記憶體提升邊界、核准邊界、證據最小化、擴充封裝隔離與 fail-closed 行為。

Runtime、供應商、模型、Harness、工具或工作流程引擎均為 replaceable 邊界。更換邊界 MUST NOT 改變 AEOS 角色、truth、記憶體提升、核准或品質關卡語意。

本文件不要求所有執行環境功能等價、不測真實 Production action、不使用真實客戶資料或 secrets、不實作測試程式、不推進 YCRM 下游採用。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-SPEC-006 |
| 文件名稱 | Provider-neutral Conformance Tests for Agent Collaboration Runs |
| 型別 | Specification |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-09-06 |
| 最後更新 | 2026-09-06 |
| 依據文件 | AEOS Issue #71、AEOS-ADR-005、AEOS-ADR-003、AEOS-ARCH-013、AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004、AEOS-SPEC-005 |
| 關聯文件 | AEOS-SPEC-002、AEOS-SPEC-003、AEOS-SPEC-004、AEOS-SPEC-005 |

## 1. 目的

本規格目的為：

- 定義 Agent Collaboration Run 符合性測試資料。
- 定義執行環境成功 / 失敗分類不變量。
- 定義關卡結果不變量。
- 定義驗證語意不變量。
- 定義證據最小化 and forbidden-data 符合性檢查。
- 定義供應商特定中繼資料擴充封裝檢查。
- 定義 unknown 結構描述 / version / 能力 fail-closed 檢查。

## 2.範圍

### 2.1 在範圍內

- Provider-neutral 符合夾具要求。
- Runtime 轉接器行為分類。
- 跨執行環境轉接器的關卡結果不變量。
- 驗證語意和 Source of Truth 驗證邊界。
- 證據最小化和禁止的資料測試。
- 特定於供應商的元資料擴充封套檢查。
- 未知架構、版本、能力失敗-closed 案例。
- 執行環境/供應商轉接器的可替換邊界斷言。

### 2.2 超出範圍

- 可執行的測試程式碼、測試框架選擇、CI 設定或自動化實作。
- 要求所有執行環境具有同等的能力。
- 測試真實的正式環境操作。
- 使用真實的客戶資料、秘密、憑證材料或受保護的操作有效負載。
- CRM 領域能力實作。
- 任何命名的執行環境、供應商、模型、Harness、工作流程引擎、工具平台或向量資料庫選擇。
- YCRM 下游採用。

## 3. 管理機構

|權威|角色 |
|---|---|
| AEOS-ADR-005 | AEOS 擁有 runtime-neutral Agent Collaboration 治理 |
| AEOS-ADR-003 | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 |執行環境／Harness/供應商中立的執行邊界 |
| AEOS-SPEC-002 |協作模型、關卡結果契約與追蹤封裝 |
| AEOS-SPEC-003 | Control Plane 編排、Gate 執行器與執行環境請求邊界 |
| AEOS-SPEC-004 |執行環境執行證據和禁止資料規則 |
| AEOS-SPEC-005 |能力/工具登錄表、架構版本控制與失敗即封鎖法則 |

規則：

- 執行環境/供應商轉接器是一個可替換的邊界。
- 執行環境成功並不等於驗證通過。
- 模型文字輸出不能直接改變品質關卡狀態。
- 登錄表發現不授予授權。
- 禁止資料 MUST NOT 以證據或痕跡形式發出。

## 4. 一致性夾具

每個一致性運行 SHOULD 都使用合成的 Agent Collaboration 運行夾具，其中不包含真實的客戶資料，沒有秘密，也沒有正式環境操作。

最少夾具組件：

|組件|要求 |
|---|---|
|合作計劃 |具有父任務、子任務、角色原型和能力類別的穩定計劃 |
| Execution Contract |授權請求範圍、執行環境約束與證據要求 |
|事實來源存根|綜合已核准事實進行驗證 |
| Tool Registry 夾具 |綜合能力/工具發現結果與模式版本|
| Runtime Adapter 設定檔 |至少兩個 provider-neutral 轉接器設定檔或模擬設定檔 |
|預期關卡結果 |預期 PASS / NEEDS_WORK / HUMAN_APPROVAL / REJECTED / BLOCKED 狀態 |
|禁止資料樣本 |代表秘密、原始客戶內容和思想鏈的合成標記 |
|擴充封套樣本|特定於供應商的元資料範例不得覆蓋穩定的核心欄位 |

測試資料 MUST 具有足夠的確定性，可以跨執行環境轉接器比較語意。

## 5. 執行環境成功/失敗分類

執行環境結果分類 MUST 遵循 AEOS-SPEC-004。

一致性檢查 SHOULD 包括：

|案例 |預期結果 |
|---|---|
|執行環境已完成授權請求|執行證據結果可能為`completed`；門仍需關卡執行器評估|
|執行環境請求失敗 | Gate 執行器 MUST NOT 將失敗解釋為驗證通過 |
|執行環境逾時 | Gate 執行器依政策回傳 `NEEDS_WORK`、`BLOCKED` 或升等 |
|執行環境已拒絕不支援的合約 | Control Plane 視為不支援/阻塞，而不是成功執行 |
|執行環境回傳不完整的證據 | Gate 執行器根據策略回傳 `NEEDS_WORK`、`HUMAN_APPROVAL`、`REJECTED` 或 `BLOCKED` |

執行環境失敗 MUST NOT 自動降級為驗證成功。

## 6. 關卡結果不變量

當給定等效的授權輸入和證據時，相同的協作計劃 SHOULD 在執行環境轉接器之間產生一致的門決策語意。

一致性 MUST 驗證：

- `PASS`需要門證據，而不僅僅是模型文本。
- `NEEDS_WORK` 傳回具有重試約束的有界角色/能力。
- `HUMAN_APPROVAL`不能被執行環境輸出、模型信心水準或供應商元資料覆蓋。
- `REJECTED` 停止受影響的路徑並記錄拒絕證據。
- `BLOCKED` 記錄缺失的依賴性、權限、模式、能力、核准或事實來源。
- 門運行者在 AEOS-SPEC-003 下保留門狀態的權限。

模型文字輸出 MUST NOT 直接改變品質關卡狀態。

## 7. 驗證語意不變量

驗證語意 MUST 保持獨立於執行環境成功。

一致性 MUST 驗證：

- 真實來源驗證使用已核准來源參考。
- 缺失事實來源會產生 `BLOCKED`、`HUMAN_APPROVAL` 或範圍不確定性結果。
- 執行環境輸出是證據輸入，而不是真理權威。
- 記憶體升級需要符合政策的來源和核准。
- 人員核准需要授權機構的核准證據。
- 僅靠執行環境證據無法產生學習案例或權威知識出版物。

## 8. 證據最小化和禁止資料測試

證據最小化測試 MUST 驗證執行環境證據和追蹤輸出排除 AEOS-SPEC-004 下的禁止資料。

禁止資料檢查 SHOULD 包括：

|禁止資料 |預期行為 |
|---|---|
|秘密標記|已編輯、省略、安全引用或已拒絕 |
|原始憑證標記 |未發出|
|完整的提示標記|除非政策明確允許，否則不會發出 |
|思想鏈標記|未發出|
|原始客戶內容標記 |除非明確已核准並且最小化，否則不會發出 |
|受保護的工具有效負載標記|盡可能以安全參考取代 |

如果沒有禁止資料就無法產生證據，轉接器 MUST 回傳失敗或已拒絕證據狀態而不是洩漏資料。

## 9. 擴充封裝檢查

特定於供應商的元資料 MUST 保留在 AEOS-SPEC-004 和 AEOS-SPEC-005 下的擴充封套內。

一致性 MUST 驗證擴充元資料不能覆寫：

- 角色原型；
- Agent Profile;
- 策略允許/拒絕；
- 核准狀態；
- 能力 ID；
- 工具操作類別；
- 關卡結果；
- 證據最小化；
- 禁止資料規則。

衝突的擴展元資料 SHOULD 導致升級或失敗即封鎖行為。

## 10. 未知架構/版本/功能失敗-Closed 案例

一致性測試 MUST 包括失敗-closed 情況：

|未知/不受支援的案例 |預期行為 |
|---|---|
|未知能力 |拒絕出貨或退貨 `BLOCKED` / `HUMAN_APPROVAL` |
|未知工具 |拒絕使用工具 |
|未知的輸入模式 |在架構已知之前拒絕使用工具 |
|未知的輸出模式 |拒絕接受工具結果 |
|不支援的架構版本 |拒絕或要求遷移 |
|不支援合約版本 |執行環境拒絕或 Control Plane 阻止 |
|缺少證據支援|拒絕、縮小範圍或回傳 `NEEDS_WORK` / `BLOCKED` |

未知架構/版本/功能 MUST 失敗即封鎖。

## 11. 可替換邊不變量

執行環境/供應商轉接器 MUST 仍然是可替換的邊界。

一致性 MUST 驗證更換執行環境/供應商轉接器不會改變：

- 角色原型分配語意；
- Agent Profile 權威；
- 策略允許/拒絕含義；
- 事實來源驗證邊界；
- 記憶提升邊界；
- 人員核准邊界；
- 品質關卡狀態語意；
- 證據最小化和禁止資料規則；
- 擴充封裝隔離；
- 追蹤身分和關聯語意。

能力不匹配 MAY 產生`BLOCKED`或`HUMAN_APPROVAL`；它 MUST NOT 改變 AEOS 治理語意。

## 12. 一致性結果記錄

每次一致性運行 SHOULD 都會產生一個 provider-neutral 一致性結果記錄。

最小欄位：

|領域|意義|
|---|---|
| `conformance_run_id` |穩定的一致性運行身分|
| `fixture_id` |燈具識別|
| `collaboration_plan_id` |合作計畫識別 |
| `runtime_adapter_profile_id` |正在測試的執行環境/供應商轉接器設定檔|
| `contract_version` | Execution Contract 或同等版本 |
| `spec_refs` | AEOS-SPEC-002/003/004/005/006 參考文獻 |
| `case_results` |按案例劃分的通過/失敗/阻止結果 |
| `gate_result_comparison` |預期與實際門語意 |
| `forbidden_data_result` |證據最小化結果|
| `extension_envelope_result` |擴充隔離結果|
| `fail_closed_result` |未知架構/版本/功能結果 |
| `final_status` |符合/不符合/阻止/不適用|

## 13. 一致性檢查表

宣稱符合規範的實作或轉接器 SHOULD 示範：

1. 相同的協作計畫在執行環境轉接器之間產生一致的關卡決策語意。
2.執行環境失敗不會自動降級為驗證成功。
3.模型文字輸出不能直接改變品質關卡狀態。
4. 證據最小化和禁止資料檢查通過。
5.執行環境/供應商轉接器仍然是可替換的邊界。
6. 未知架構、版本和能力失敗即封鎖。
7. 特定於供應商的元資料保留在擴充封套內。
8. 擴充元資料不能覆蓋穩定的核心治理欄位。
9. 事實來源驗證邊界仍在執行環境證據之外。
10. 不需要任何實際的正式環境操作、客戶資料或秘密來確保一致性。

## 14. 狀態與核准

本規格目前為 **Approved 1.0.0**。

PR #81 已合併至 main，merge commit 為 `d11da71ccae27e68bed6de7875c2bc9671be2f47`。此合併作為 Repository Owner 最終核准證據，正式核准本規格為 Provider-neutral Conformance Tests for Agent Collaboration Runs。

本次核准確認以下治理邊界：

- 同一 AEOS 協作 plan 在不同執行環境轉接器下必須維持一致關卡語意與驗證語意。
- Runtime 失敗不可自動降級為 successful 驗證。
- Model text 輸出不可直接改變品質關卡狀態。
- 需要證據最小化和禁止資料檢查。
- 執行環境/供應商轉接器保持可替換邊界。
- 未知架構、版本、能力失敗即封鎖。
- 本規格不授權 executable test 實作、正式環境 actions、real 客戶資料/secrets 或 YCRM 下游採用。

## 15. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #71 | GitHub Issue | Provider-neutral Conformance Tests for Agent Collaboration Runs 工作來源 |
| AEOS-ADR-005 — Agent Collaboration Ownership Decision | ADR | AEOS 擁有 Agent Collaboration 治理 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境邊界 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構|執行環境／Harness／供應商中立的執行邊界|
| AEOS-SPEC-002 — Agent Collaboration Model Architecture Spec |規格|關卡結果合約與追蹤信封 |
| AEOS-SPEC-003 — Agent Control Plane Orchestration Extension |規格|關卡執行器和執行環境執行請求邊界 |
| AEOS-SPEC-004 — Runtime Adapter Execution Evidence Contract |規格|執行環境證據和禁止資料規則|
| AEOS-SPEC-005 — Capability / Tool Registry Adapter Contract |規格|能力發現、模式版本控制與失敗即封鎖法則 |

## 16. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-06 | 依 PR #81 merge 證據（`d11da71ccae27e68bed6de7875c2bc9671be2f47`）升級為 Approved Specification；正式核准 Provider-neutral Conformance Tests for Agent Collaboration Runs，作為 AEOS Agent Collaboration 執行環境/供應商 replaceable 邊界符合性邊界 | Codex |
| 0.1.0 | 2026-09-06 | 建立 Provider-neutral Conformance Tests for Agent Collaboration Runs Draft，定義測試資料要求、執行環境成功/失敗分類、關卡結果不變量、驗證語意、證據最小化、擴充封裝檢查、fail-closed cases 與 replaceable 邊界 invariant | Codex |
