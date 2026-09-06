---
doc-id: AEOS-ADR-005
doc-name: Agent Collaboration Ownership Decision
doc-type: ADR
repository: AEOS
version: 1.0.0
status: Approved
owner: Architecture Owner
created: 2026-09-05
updated: 2026-09-05
related:
  - AEOS-ISSUE-067
  - AEOS-ISSUE-066
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-009
  - AEOS-ARCH-013
  - AEOS-SPEC-002
---

# AEOS-ADR-005 — Agent Collaboration Ownership Decision

## 執行摘要

本 ADR 決定：**AEOS owns runtime-neutral Agent Collaboration**。

Agent Collaboration 是企業層級的多代理治理、角色/能力組成、任務委派、Separation of Duties、獨立驗證、品質關卡、證據、核准與生命週期責任模型。它不是任一產品儲存庫、代理 Harness、執行環境、模型供應商、工具供應商、工作流程引擎、向量資料庫或下游採用的局部實作責任。

Agent Control Plane 執行 AEOS 政策，並建立受治理的協作要求 / 執行契約。Agent Runtime 僅執行已授權要求，並回傳執行證據。產品儲存庫僅能承接採用對應，不得建立平行多代理治理。

## 1. 背景

AEOS 已透過 AEOS-ADR-003 與 AEOS-ARCH-013 建立 Agent Control Plane、Agent Execution Plane、Agent Harness / Orchestration Boundary、Agent Runtime、Execution Contract、Runtime Neutral、Harness Neutral 與 Provider Boundary。

目前 Multi-Agent Collaboration 仍需要一項明確歸屬決策。若協作治理由產品儲存庫、執行環境、Harness、供應商或工作流程實作各自定義，將產生下列風險：

- 不同產品建立互不相容的角色、能力、委派、驗證與核准語意。
- Harness / Runtime 因具備編排能力而被誤認為具有 Enterprise 治理權限。
- Product 儲存庫將領域工作流程採用誤升格為 enterprise 多代理治理。
- Provider-specific 工具、模型、記憶體或工作流程能力反向塑造 AEOS 政策。
- Independent 驗證、品質關卡、證據 / 來源 / confidence 與升級處理規則無法跨執行環境 / 產品維持一致。

AEOS Issue #67 因此要求先做歸屬決策，再由 Issue #66 建立 Agent Collaboration Model Architecture Spec。

## 2. 決定

### D-01 — AEOS 擁有 Agent Collaboration 治理

AEOS MUST 擁有 runtime-neutral Agent Collaboration 治理。

Agent Collaboration 治理至少包含：

- 角色 archetype 與代理設定檔的企業語意；
- 能力、skill、工具、政策與任務分解的關係；
- 委派、Separation of Duties與獨立驗證要求；
- 品質關卡、證據、confidence、來源與稽核 expectations；
- information 生命週期、記憶體/資料權限與保留邊界；
- human 核准、升級處理與已封鎖狀態語意；
- runtime-neutral 執行邊界與產品採用對應規則。

### D-02 — Agent Control Plane 執行 AEOS 政策

Agent Control Plane MUST execute AEOS 政策 for 協作准入、組成、委派、授權、核准、品質關卡、證據要求、撤銷、失敗 handling 與升級處理。

Control Plane MAY 依 AEOS-ADR-003 與 AEOS-ARCH-013，將執行協調委派給 Agent Harness／Orchestration 實作，但 MUST NOT 隱含委派企業治理權限。

### D-03 — Agent Runtime 僅執行授權請求

Agent Runtime MUST 僅執行授權協作請求或委託執行合約。

執行環境職責僅限於：

- 驗證合約完整性和支援的語意；
- 在授權的角色、任務、工具、模型、記憶體/資料、憑證、預算和環境範圍內執行；
- 保留委託約束；
- 報告結果、狀態、錯誤、決策證據、工具/模型/記憶體呼叫、信心水準/出處訊號和稽核相關證據。

執行環境 MUST NOT 創建、擴張或覆蓋 AEOS 協作策略、核准、角色權限、品質關卡結果或產品採用權限。

### D-04 — 產品儲存庫僅提供採用對應

產品儲存庫 MAY 將 AEOS Agent Collaboration Model 對應到產品特定的工作流程、網域角色、資料來源、通路、整合、UI 或操作設定。

產品儲存庫 MUST NOT：

- 創建平行企業多代理治理；
- 重新定義 AEOS 角色原型、Separation of Duties、驗證、核准或證據語意；
- 將特定於產品的工作流程視為 AEOS 政策權威；
- 利用下游採用壓力來繞過 AEOS 架構審查；
- 在 AEOS 產出物存在並且為已核准之前，將 AEOS 級協作決策標記為已完成。

下游問題，包括 YCRM 採用項目，SHOULD 被視為被 AEOS 阻止，直到相關 AEOS ADR / SPEC / 架構產出物為已核准或以其他方式聲明可採用。

### D-05 — 執行環境、框架、供應商和產品中立

Agent Collaboration MUST 保留 runtime-neutral、harness-neutral、provider-neutral 和 product-neutral。

任何特定的執行環境、Harness、供應商、模型、工具平台、向量資料庫、工作流程引擎、CRM 儲存庫或下游產品，均不 MAY 被要求作為 Agent Collaboration 治理的權威 definition carrier。

### D-06 — Specification Carrier

AEOS-SPEC-002 SHOULD 依本 ADR 的歸屬決策，承載 AEOS Issue #66 的第一版 Agent Collaboration Model Architecture Spec 草案。

AEOS-SPEC-002 可以定義模型概念和一致性期望，但 MUST 保持在 AEOS-ARCH-013 權限邊界內，並且 MUST NOT 引入執行環境實作、SDK、部署拓撲、正式環境操作或供應商選擇。

## 3. 後果

### 積極

- 多代理治理有一個企業所有者和一個 runtime-neutral 定義路徑。
- 產品採用可以透過對應來進行，而不是重新定義治理。
- Harness／執行環境實作仍然是可替換和可組合的。
- 獨立驗證、品質關卡控、證據和升級可跨產品移植。
- YCRM 下游採用可以明確引用 AEOS 作為其阻塞架構權限。

### 權衡

- 產品團隊必須等待 AEOS 等級的定義才能宣稱治理完成。
- 執行環境和工具需要轉接器或契約對應來表達 AEOS 協作語意。
- 協作策略需要正式的生命週期和審查，而不是臨時的產品設定。

## 4. 考慮的替代方案

### A. YCRM 擁有 Multi-Agent Collaboration 治理

Rejected。 YCRM 可以採用 CRM 工作流程的協作規則，但 CRM 域採用不是 enterprise 架構權限。

### B. Hermes、OpenClaw、DeepSeek Harness、OpenAI、Gemini、n8n、Qdrant 或其他執行環境/供應商擁有的治理

Rejected。這些系統可能提供執行環境、編排、模型、工作流程、工具、記憶體或整合能力，但供應商能力不會創造 AEOS 治理權威。

### C. 每個產品定義自己的治理

Rejected。這會產生碎片化的語意，並阻止跨 AI Engineering Workspace 的一致證據、核准、驗證和執行環境替換。

### D. 否 AEOS 所有權決定

Rejected。如果沒有正式的決定，下游採用和執行環境實作將繼續模糊架構權威與實作便利性。

## 5. 架構對齊

- AEOS-ADR-003：保留 Control Plane / Execution Plane 分離與執行環境 / Harness neutrality。
- AEOS-ARCH-013：Agent Collaboration 使用 Agent Control Plane 策略、Execution Contract、委託規則與證據邊界。
- AEOS-ARCH-007：協作能力是能力優先，而非實施優先。
- AEOS-ARCH-009：執行環境、供應商、工具、記憶體和產品依賴關係必須保持明確和受管控。
- AEOS-ARCH-014：產品化或下游包裝不得重寫平台治理語意。

## 6. 狀態與核准

本 ADR 目前為 **Approved 1.0.0**。

PR #72 已合併至 `main`，merge commit 為 `03d800b741b663309cd8afa11075beaed30d6e27`。此合併作為 Repository Owner 最終核准證據，正式核准本 ADR 為 AEOS Agent Collaboration 歸屬的 Architecture Decision。

核准後：

- AEOS 擁有 runtime-neutralAgent Collaboration 治理。
- 產品儲存庫僅能承接採用對應，不建立平行多代理治理。
- Agent Control Plane 執行 AEOS 政策。
- Agent Runtime 僅執行已授權協作要求 / 已委派執行契約，並回傳執行證據。
- AEOS-SPEC-002 可依本 ADR 作為 Agent Collaboration Model Architecture Spec 的 Draft 規格載體；其是否升級為 Approved 由後續 Specification Review 或 closure 決策決定。

## 7. 參考文獻

| 文件 | 型別 | 用途 |
|------|------|------|
| AEOS Issue #67 | GitHub Issue | Agent Collaboration 歸屬決策工作來源 |
| AEOS Issue #66 | GitHub Issue | Agent Collaboration Model Architecture Spec 工作來源 |
| AEOS-ARCH-001 — Architecture Baseline |架構| AEOS 架構入口及註冊權限 |
| AEOS-ADR-003 — Agent Control Plane and Runtime Separation Decision | ADR | Control Plane / 執行環境分離與執行環境中立 |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |架構| Agent Control Plane、Execution Contract、執行環境中立與證據邊界 |
| AEOS-ARCH-014 — Productizable Platform Architecture |架構|產品/平台邊界與下游採用護欄|

## 8. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.0.0 | 2026-09-05 | 依 PR #72 merge 證據（03d800b741b663309cd8afa11075beaed30d6e27）升級為 Approved Architecture Decision；正式確認 AEOS owns runtime-neutral Agent Collaboration 治理，並確認產品儲存庫僅做採用對應 | Codex |
| 0.1.0 | 2026-09-05 | 建立 Agent Collaboration 歸屬 draft：AEOS owns runtime-neutral Agent Collaboration；產品儲存庫僅做採用對應；Runtime 只執行已授權要求並回傳證據 | Codex |
