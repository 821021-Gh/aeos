---
doc-id: AEOS-RPT-004
doc-name: Local-first AI Agent Execution Architecture Gap Analysis
doc-type: Report
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-08-27
updated: 2026-08-28
related:
  - EWO-AEOS-0046
  - AEOS-ARCH-001
  - AEOS-ARCH-013
  - AEOS-ARCH-014
  - AEOS-ADR-003
  - AEOS-STD-005
  - AEOS-STD-007
  - AEOS-SPEC-001
  - YEOS-ENG-STD-008
  - YEELIGHT-AI-CRM-AGENTS
  - AR-AEOS-0046-R1
---

# AEOS-RPT-004 — Local-first AI Agent Execution Architecture Gap Analysis

> EWO-AEOS-0046：本報告依 2026-08-27 使用者工作要求，檢查 AEOS、YEOS 與 yeelight-ai-crm `main` 分支之現況，評估 Local-first AI Agent Execution Architecture 的既有覆蓋與差距。本文件已完成 Architecture Review（AR-AEOS-0046-R1）並核准為 Approved Report；不取代 AEOS-ARCH-013、AEOS-ADR-003、YEOS ENG-STD-008 或任何已核准 Approval Policy。

## 執行摘要

AEOS `main` 已具備 Enterprise AI Agent Architecture 的核心治理骨架：Agent Control Plane 與 Agent Execution Plane 已分離，Runtime / Harness / Provider Neutrality 已明確成立，Execution Contract、路由、核准、授權、稽核、預算與失敗政策均有架構責任歸屬。

本次差距不在於缺少代理架構，而在於尚未把「Local-first 執行」操作化為可審查的路由層級、entry / exit 準則、cost 遙測資料、升級處理原因與 cross-repository 採用 plan。建議第一階段不重寫既有 YEOS 命令核准與 risk 政策，而是在 AEOS 中補上 local-first 路由與 cost 治理的候選規格，並由 CRM 作為參考實作 PoC 的候選場景。

本報告提出的最小下一步已由 `AEOS-SPEC-001` 實作：將 Tier 0 到 Tier 4 路由模型、cost 遙測資料與升級處理原因操作化為正式 Specification，並保持所有具名執行環境、本機模型、雲端模型、Harness 與工具僅位於 Adapter / Provider / Reference Implementation 邊界。

AR-AEOS-0046-R1 已完成 Architecture Review，Decision 為 APPROVED；Repository Owner 最終核准由 PR #60 Review Package 承載。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-RPT-004 |
| 文件名稱 | Local-first AI Agent Execution Architecture Gap Analysis |
| 型別 | Report |
| 用途分類 | Architecture Gap Analysis / Candidate Assessment |
| 狀態 | Approved |
| 版本 | 1.0.0 |
|儲存庫 | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-27 |
| 最後更新 | 2026-08-28 |
| 依據文件 | EWO-AEOS-0046、AEOS-ARCH-013、AEOS-ADR-003、AEOS-ARCH-014、AEOS-STD-007、YEOS ENG-STD-008、yeelight-ai-crm AGENTS.md / .ai/PROJECT.md |
| 關聯文件 | AEOS-ARCH-001、AEOS-STD-005、AEOS-STD-007、AEOS-SPEC-001、YEOS ENG-STD-008、yeelight-ai-crm AGENTS.md、AR-AEOS-0046-R1 |

## 1. 目的

本報告目的為：

- 建立 AEOS / YEOS / yeelight-ai-crm 三個儲存庫的 Local-first AI Agent Execution Architecture 基準。
- 評估既有架構對 Agent Control Plane、Execution Plane、Runtime Neutrality、路由、核准、稽核、cost 治理與可觀測性的覆蓋程度。
- 識別尚未正式定義的缺口。
- 提出不繞過 YEOS 命令核准、不綁定特定 LLM / 執行環境 / Harness 的 target 架構 tiers。
- 建議後續 repo-first 實作順序、文件位置、PR order 與驗證方式。

## 2.範圍

### 2.1 在範圍內

- AEOS `main` 上已核准 Agent Architecture 與 Productizable Platform Architecture。
- YEOS `main` 上已釋出的 AI Agent Command Approval Standard。
- yeelight-ai-crm `main` 上的代理操作守則、project 上下文、CI / 儲存庫基準與實作邊界。
- Local-first 執行層：確定性規則、本機 AI、低成本雲 AI、Frontier Cloud AI、人工核准。
- Routing、升級處理、cost 遙測資料、核准、稽核與可觀測性的 gap analysis。

### 2.2 超出範圍

- 選定或背書任何具名本機 LLM、雲端模型、代理框架、Harness、執行環境、向量資料庫或工具供應商。
- 修改 YEOS 既有 Command Classification、Risk Classification、Approval Policy 或 Repository Protection 規則。
- 直接進行正式環境部署、憑證 / KMS provisioning、正式環境資料存取或 destructive 操作。
- 在本報告中核准新的 Architecture、ADR、Standard、Catalog Entry 或正式環境 rollout。

## 3. 儲存庫基線

| Repository | `main` HEAD / 狀態 | 既有重點 | Local-first 相關風險 |
|-----------|-------------------|----------|----------------------|
| AEOS | `6984a9820922d8b056f69d2cea1d8d4acbe04d46`；GitHub 預設分支目前不是 `main`；`main` 未啟用分支 protection | AEOS-ARCH-013 與 AEOS-ADR-003 已建立 Control Plane / Execution Plane、Runtime Neutrality、Execution Contract、路由、核准、稽核、預算邊界；AEOS-ARCH-014 已建立 productization 邊界 | Default 分支與 `PROJECT_STATE.md` Source of Truth 不一致；`main` 未保護會降低治理關卡的技術強度；local-first 層級 / 遙測資料尚未正式化 |
| YEOS | `e5144f099c585e278a9cff7390420d50adcd2eba`；預設分支 `main`；`main` protected，required check 為 `Validate repository documents` | ENG-STD-008 已定義 C1-C4 Command Classification、L/M/H/C Risk Classification、Approval Policy、Repository Protection 不可繞過 | Local 代理路由必須採用既有標準，不得另行降低核准；YEOS repo 缺少薄型 `PROJECT_STATE.md` / `AGENTS.md` 對代理啟動不利 |
| yeelight-ai-crm | `366689077da441eb173d81b789ae241baa66caf1`；預設分支 `main`；`main` 未保護；有 draft PR / open issues | `AGENTS.md` 要求 Production First、Scope First、Evidence First、Security by Default、Human-controlled Release；`.ai/PROJECT.md` 明確要求 provider-neutral、轉接器邊界、planned 能力不得描述為正式環境 | 產品 repo 可作參考實作，但未保護 main；若引入本機執行環境 / 模型 PoC，必須限定在轉接器 / 供應商邊界且不得觸及正式環境 secrets / 資料 |

## 4.現有架構覆蓋矩陣

|能力| AEOS | YEOS | yeelight-ai-crm |差距|
|------------|------|------|-----------------|-----|
| Agent Control Plane |涵蓋：AEOS-ARCH-013 §4-§6、AEOS-ADR-003 D-01/D-02 |部分透過治理標準涵蓋，而非架構所有權 |透過儲存庫說明與工作流程部分涵蓋 |需要 local-first 路由策略作為 Control Plane 責任設定檔 |
| Agent Execution Plane |涵蓋：Harness / Runtime / Provider / Adapter 邊界定義|非主要範圍 |部分作為實施回購協議出現 |需要確定性/本機/雲層的候選執行契約設定文件 |
|執行環境中立性 |涵蓋且明確 |涵蓋獨立於工具的核准標準 |包含 provider-neutral 項目說明|需要確保本機 LLM 名稱僅保留轉接器/供應商範例 |
|本機 AI 執行環境 |允許作為實施選項 |未定義 |未作為受管能力實施 |需要第 1 層語意而不命名強制執行環境/模型 |
|模型路由|一般涵蓋能力、風險、成本、資料邊界 |除了指揮核准影響之外，不是 YEOS 問題 |未正式化 |需要路由矩陣：風險 x 複雜性 x 信心水準 x 成本 x 延遲 x 資料敏感性|
|升級 |失敗政策中普遍涵蓋 |涵蓋命令風險和核准升級|工作流程包含由人員控制的發佈|需要明確的層級離開條件和升級原因分類 |
|風險分類|架構參考政策背景；不是命令分類擁有者|涵蓋 ENG-STD-008 |儲存庫特定指令提升生產/安全保障 |必須重複使用 YEOS 分類；無需重新設計|
|核准|涵蓋 Control Plane 責任 |涵蓋 ENG-STD-008 |人員控制釋放聲明|需要 local-first 規則規定本機執行不能繞過核准 |
|稽核|涵蓋：稽核相關性、證據、被拒絕的操作 |涵蓋：需要可追溯性|證據第一原則|需要每層執行證據欄位和本機/雲/前沿/人員比率 |
|成本治理|預算與Token/成本限制存在於 AEOS-ARCH-013 / AEOS-STD-007 |非主要範圍 |未正式化 |需要成本遙測：Token、供應商層、重試、升級原因、成功率 |
|可觀察性|作為證據平面覆蓋 |追溯基線| CI 和證據工作流程呈現 |需要路由結果和層有效性的指標架構 |

## 5. 差距分析

### 5.1 確認的優勢

- AEOS 已經透過將 Control Plane 權限與 Harness / Runtime 執行權限分開來防止以產品為中心的代理架構。
- AEOS 已經支援按能力、風險、成本、資料邊界、可用性和預算進行路由，而無需綁定到特定的模型供應商。
- YEOS 已經定義了具有強制性人工審查/核准邊界的命令和風險分類。
- CRM 已經包含儲存庫本機指令，用於保護正式環境操作、秘密、供應商中立性和人員控制的發布。

### 5.2 主要差距

|間隙 ID |差距|影響 |建議業主 |
|--------|-----|--------|-----------------|
| GAP-001 | Local-first 層模型尚未正式定義 |團隊可能會將本機執行環境選擇視為架構，或者可能會根據習慣升級到雲端 | AEOS 架構 |
| GAP-002 |每層的進入/離開／升級條件並不明確 |路由決策很難稽核和調整| AEOS 規格/標準 |
| GAP-003 |成本遙測與成功率模式缺失 |無法證明減少的雲端Token或人員幹預 | AEOS 標準或可重複使用的功能規格 |
| GAP-004 | 本機 AI 核准邊界未明確說明 | 本機代理可能被誤認為治理風險較低 | AEOS + YEOS 對應 |
| GAP-005 | CRM 參考實作路徑與平台架構未分離 | PoC 可能意外將產品特定假設帶入 Platform Core | AEOS-ARCH-014 對應 + CRM SPEC |
| GAP-006 |各個儲存庫的儲存庫保護不一致 | AEOS 與 CRM `main` 無需同等技術門即可更改 | Repository Owner / 治理 |

### 5.3 安全/治理障礙

在 Draft 創建和審查過程中未發現任何攔截器。 AR-AEOS-0046-R1 和 SP-AEOS-0046-R2 有已完成和 APPROVED 決策；下面的生產和受保護操作邊界保持不變。

以下操作仍然需要核准，且本報告不會執行：

- 更改儲存庫預設分支或分支保護設定。
- 將架構變更合併為`main`。
- 建立或啟動生產本機代理執行環境。
- 存取正式環境資料、憑證、KMS、客戶資料或秘密資料。
- 更改 YEOS 命令核准政策。

## 6. 目標架構層

Local-first 執行 MUST 是中立 Execution Plane 供應商的 Control Plane 路由策略。層名稱定義治理語意，而不是產品。

|等級 |名稱 |典型的進入標準 |離開／升級條件 |治理/稽核要求|
|------|------|------------------------|----------------------------|---------------------------------|
| 0 級 |確定性規則|靜態驗證、模式檢查、格式化、策略查找、分類、CI、確定性自動化 |規則衝突、未知輸入、確定性證據不足、政策模糊 |記錄規則版本、輸入類別、輸出、確定性/精確的信心水準、失敗原因 |
| Tier 1 | Local AI | 低風險摘要、分類、擷取、檢索、草稿、記錄／程式碼審查預處理、一般可驗證推理 | 信心水準低、上下文溢位、重複失敗、不支援的模態、高敏感度不符、任務複雜度超出本機設定檔 | 記錄本機供應商類別、模型能力類別、提示詞／上下文預算、信心水準、驗證證據；必須保留核准範圍 |
| 2 級 |低成本雲 AI|本機層不足，任務仍為低/中風險，需要更大的背景或更強大的推理，資料策略允許雲 |信心依然不足，架構/除錯複雜，跨 repo 推理，業務影響大 |日誌供應商層、Token、成本估算/實際、資料敏感性決策、升級原因 |
|第 3 級 |Frontier Cloud AI|架構推理、複雜除錯、高上下文審查、跨儲存庫決策支援、高模糊性 |需要核准、破壞性/受保護的操作、憑證/正式環境存取、不可逆轉或高風險的決策 |記錄高階模型層的使用、理由、嘗試的替代方案、成本、人員可讀的證據 |
|第 4 級 |人員核准|正式環境部署/啟動、破壞性操作、憑證/KMS、敏感資料、不可逆操作、高風險業務決策、YEOS 下的強制核准 |人員核准、拒絕、請求更改或委託有限執行 |人工核准證據、核准人身分/參考、政策版本、決策、授權範圍 |

### 6.1 路由輸入

Control Plane 路由 SHOULD 至少評估：

`Risk x Complexity x Confidence x Cost x Latency x Data Sensitivity`

其中：

- 風險 MUST 繼承或對應到儲存庫的已核准風險和審查策略。
- 複雜性 SHOULD 考慮所需的推理深度、跨儲存庫範圍、上下文大小和歧義性。
- 信心水準 SHOULD 透過確定性證據、驗證結果、可用的模型自我/批評者信心水準以及重試歷史來衡量。
- 成本 SHOULD 包括本機計算類別、雲端Token估計、觀察到的Token使用情況、重試計數和機會成本。
- 延遲 SHOULD 包括面向使用者的回應能力和佇列限制。
- 資料敏感度 MUST 決定是否允許使用雲層。

### 6.2 不可繞過規則

本機執行降低成本和延遲；它不會降低治理。

第 0 層或第 1 層執行 MUST NOT：

- 繞過 YEOS ENG-STD-008 命令核准要求；
- 削弱儲存庫保護；
- 自我核准 M/H/C 風險行動；
- 將工具、模型、資料、記憶體或憑證範圍擴展到 Execution Contract 之外；
- 將特定於供應商的本機執行環境設定轉換為 AEOS 架構權限。

## 7. 實施計劃

### 7.1 建議 PR 順序

|訂單|儲存庫 |改變 |文件/產出物|驗證 |
|-------|------------|--------|-------------------|------------|
| 1 | AEOS |建立差距分析報告（本審查包中的已完成）| `docs/reports/AEOS-RPT-004-Local-first-AI-Agent-Execution-Architecture-Gap-Analysis.md` |元資料+架構回顧（AR-AEOS-0046-R1） |
| 2 | AEOS |建立 Local-first 路由設定檔規範（本審查包中的已完成）| `docs/specifications/AEOS-SPEC-001-Local-first-AI-Agent-Execution-Routing-Profile.md` |一致性檢查+規格審查（SP-AEOS-0046-R2） |
| 3 | AEOS |決定已核准內容是否修改 AEOS-ARCH-013 |僅當審查需要時才進行架構修改或後續 ADR | Architecture Owner / Repository Owner 審查 |
| 4 | YEOS |將 ENG-STD-008 指令/風險分類對應至 local-first 路由採用說明，無需重新設計 |現有標準或補充工程說明|確認無需核准降級且仍需要儲存庫保護 |
| 5 | yeelight-ai-crm |新增轉接器綁定 local-first 執行的 PoC SPEC | CRM 文件/規範；設定/轉接器邊界背後的實作 | CI，沒有正式環境資料/秘密，沒有生產啟動 |
| 6 | yeelight-ai-crm |實作確定性和本機預處理 PoC |現有回購模式下的測試/腳本 |測試證明第 0 層/第 1 層輸出是可驗證的，並記錄了升級原因 |

### 7.2 第一個最小實作

該報告是第一個最小實作產出物。它故意狹窄，因為它跨越架構治理領域，但本身並不核准新的架構產出物。它與已核准 `AEOS-SPEC-001` 一起，為下游 YEOS 採用對應和任何後續架構修改決策提供了正式基礎。

### 7.3 ADR 需求評估

只有當 AEOS 選擇以下選項之一時，才可能需要新的 ADR：

- 使 local-first 路由對於所有代理執行都是強制的。
- 增加一個新的 Control Plane 決策模型，該模型會對 AEOS-ADR-003 產生重大影響。
- 將成本治理定義為新的企業政策權威。

如果下一次更改僅在 AEOS-ARCH-013 現有的 Control Plane 路由和預算職責下添加相容的路由設定文件，則`AEOS-SPEC-001`可能就足夠了。

## 8. 驗證

本報告是根據2026 年8 月27 日的`main`分支證據和2026 年8 月28 日的已完成審查編寫的：

|檢查 |結果 |
|-------|--------|
| AEOS `main` 檢查 |通過 |
| YEOS `main` 檢查 |通過 |
| yeelight-ai-crm`main`已檢查 |通過 |
|保留執行環境/模型中立性 |通過 |
| YEOS 避免了命令審查重新設計 |通過 |
| 已避免正式環境／憑證／破壞性操作 | 通過 |
|架構回顧 AR-AEOS-0046-R1 | APPROVED |
|規格審查 SP-AEOS-0046-R2 | APPROVED |
| EWO-AEOS-0046 新增可追溯性 |通過 |

## 9. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| EWO-AEOS-0046 — Local-first AI Agent Execution Routing Profile | EWO | 工作來源與範圍 |
| AEOS-ARCH-001 — Architecture Baseline |架構| AEOS 架構註冊與基準權限 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Control Plane / Execution Plane / 執行環境中立性權威 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Approved 分權決定 |
| AEOS-ARCH-014 — Productizable Platform Architecture |架構|產品化與參考實作邊界|
| AEOS-STD-005 — Review Standard |標準|審查工作流程與決策要求 |
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard |標準| Token 預算，路由與上下文治理 |
| AEOS-SPEC-001 — Local-first AI Agent Execution Routing Profile |規格| Approved 路由設定檔規範 |
| YEOS ENG-STD-008 — AI 代理命令核准標準 |標準|指揮分級、風險分級及審查政策|
| yeelight-ai-crm`AGENTS.md` |儲存庫說明 |生產優先、人員控制的釋放護欄 |
| yeelight-ai-crm`.ai/PROJECT.md` |專案背景| CRM 範圍、provider-neutral 與轉接器邊界指導 |
| AR-AEOS-0046-R1 |審查紀錄 |架構審查 APPROVED；PR #60 |

## 10. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-08-28 | AR-AEOS-0046-R1 APPROVED；依 PR #60 Repository Owner 最終核准將報告由 Draft 0.1.0 升級為 Approved 1.0.0；核心分析結論與治理邊界未變更 | ChatGPT |
| 0.1.0 | 2026-08-27 |初始 Draft 基線、覆蓋矩陣、差距分析、目標 local-first 層和實施計劃；與 EWO-AEOS-0046 和 AEOS-SPEC-001 對齊 | Codex |
