---
doc-id: AEOS-STD-007
doc-name: AI Engineering Context and Token Budget Standard
doc-type: Standard
repository: AEOS
version: 1.1.0
status: Review
owner: Repository Owner
created: 2026-08-22
updated: 2026-08-28
related:
  - EWO-AEOS-0043
  - EWO-AEOS-0047
  - SR-AEOS-0043-R1
  - SR-AEOS-0043-R2
  - AEOS-RPT-005
  - AEOS-ARCH-001
  - AEOS-ARCH-010
  - AEOS-CON-001
  - AEOS-DIA-001
  - AEOS-GOV-001
  - AEOS-STD-001
  - AEOS-STD-002
  - AEOS-STD-003
  - AEOS-STD-004
  - AEOS-STD-005
---

# AEOS-STD-007 — AI Engineering Context and Token Budget Standard

> EWO-AEOS-0043 建立 AEOS AI Engineering Context 與 Token Budget 規範；EWO-AEOS-0047 在不建立平行標準的前提下，補充 Operational Workspace / Work Session Lifecycle、Authority Loading、Active / Archive / Delete、Promotion-before-disposal、Shared Handoff 與 Agent Session Hygiene。本標準不取代既有 Governance Workflow、文件標準、Review、Git Workflow、Command Approval 或安全控制。

## Executive Summary

本標準採用「Repository 為工程記憶與 current-state authority、Operational Workspace 承載少量 Active Context、Work Session 承載當前意圖與執行」的工作模型，定義 AI Engineering 工作的 Context Budget、載入順序、狀態快照、輸出格式、Work Session Lifecycle、模型路由與 Cache 邊界。

Repository `main` 為 System of Record / SSOT；Project Instructions、Approved Reference、Closure Snapshot 與其他 Workspace Context 都是 derived context，不得凌駕 `main`。Chat／Agent Session 是 Ephemeral Work Session；Merge + Closure 是 Knowledge Promotion Point；Archived Session 是 Historical Working Record，非 Fact Authority。目的在降低重複 Context 與無效輸出、避免 Project 退化為永久 Chat History Database，同時保留可追溯性、品質、安全與既有 AEOS / YEOS Governance Workflow。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-STD-007 |
| 文件名稱 | AI Engineering Context and Token Budget Standard |
| 型別 | Standard |
| 狀態 | Review |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Repository Owner |
| 建立日期 | 2026-08-22 |
| 最後更新 | 2026-08-28 |
| 依據文件 | EWO-AEOS-0043、EWO-AEOS-0047、AEOS-RPT-005、AEOS-ARCH-001、AEOS-ARCH-010、AEOS-CON-001、AEOS-DIA-001、AEOS-GOV-001、AEOS-STD-001 |
| 關聯文件 | SR-AEOS-0043-R1、SR-AEOS-0043-R2、AEOS-STD-002～AEOS-STD-005 |

## 1. Purpose

本標準之目的為：

- 建立可量測、可調整且與模型供應商無關的 Context 與 Token Budget。
- 以按需載入取代全 Repository／全對話載入，減少重複閱讀與傳輸。
- 以 Repository `main` 為每個新 Work Session 的 current-state authority。
- 以 `PROJECT_STATE.md` 與 Closure Snapshot 保存最小可恢復狀態，但不把 Snapshot 升為 Fact Authority。
- 建立 Operational Workspace / Work Session 的 Active、Archive、Delete 與 knowledge promotion 操作規則。
- 建立 Shared Project / Human + AI Agent 的 session ownership、handoff、context continuity 與 lineage 規則。
- 以 Delta Output 降低重複說明，同時保留 Files Changed、Validation、Risk 與 Next Action。
- 依任務複雜度與風險進行模型分級，不降低安全、Review 或 Governance 要求。
- 定義 Prompt Cache 與 Semantic Cache 的適用範圍、隔離、失效與禁止事項。

本標準不是模型採購政策、價格表、Prompt 內容庫、Repository 安全政策、Chat storage implementation 或 YEOS Engineering Workflow 的替代品。

## 2. Scope

### 2.1 In Scope

本標準適用於：

- AEOS Repository 內由 Human / AI Agent 協助之搜尋、分析、文件、實作、測試、Review 與交付工作。
- 被其他 Repository 透過自身治理正式採用之 AEOS AI Engineering 工作。
- Context 組裝、Token Budget、狀態保存、輸出、Work Session 切換、模型路由與 Cache。
- Operational Workspace / Project 的 Active Working Context 管理。
- Work Session lifecycle、disposition、promotion、handoff、ownership、lineage 與 agent-session hygiene。

### 2.2 Out of Scope

本標準不涵蓋：

- EWO、Git Branch、Commit、Pull Request、Engineering Review、Approval、Merge 與 Close 的正式流程定義；其流程仍依 YEOS Engineering Workflow、Repository Governance 與 AEOS-STD-005。
- Command Classification、Risk Classification、Repository Protection 或 Human Approval Boundary；若採用 YEOS，仍依 ENG-STD-008。
- Metadata、Naming、Cross-reference 與 Markdown 結構；分別由 AEOS-STD-001～AEOS-STD-004 定義。
- 模型供應商、特定型號、即時價格或合約承諾。
- ChatGPT、特定 Project vendor、Agent runtime、harness、memory backend、vector store 或 Chat Storage implementation。
- 將 Project、Chat、Archive、Cache 或 Snapshot 視為正式 Fact Authority、Review Evidence 或最終決策紀錄。

## 3. Operating Principles

| # | 原則 | 規則 |
|---|------|------|
| CP-001 | Repository as Memory | Repository `main` 的正式內容為工程記憶與 current-state authority；對話 MUST NOT 成為唯一事實來源。 |
| CP-002 | Intent before Context | Agent MUST 先確認目前意圖與交付範圍，再組裝 Context。 |
| CP-003 | Least Context | 初始 Context MUST 僅包含完成目前步驟所需的最少資料。 |
| CP-004 | Progressive Expansion | 只有符合 §5.2 之擴大條件時，才增加 Context。 |
| CP-005 | Delta by Default | 正常輸出 MUST 使用 §7 Delta Output，不重述既有背景。 |
| CP-006 | Governance Preserved | Token 最佳化 MUST NOT 省略安全檢查、驗證、Review、Approval 或追溯性。 |
| CP-007 | Cache is Non-authoritative | Cache 僅為效能輔助；正式判斷 MUST 回到目前 Repository 事實驗證。 |
| CP-008 | Main before Historical Context | 新 Session MUST 先取得目前 `main` baseline，再載入 Closure Snapshot、Project Context 或 Historical Chat。 |
| CP-009 | Promotion before Disposal | Work Session Archive／Delete 前 MUST 確認重要 Decision、Evidence、Review、Approval、Validation 與 Closure 已提升至 Repository 正式載體。 |
| CP-010 | Minimal Active Workspace | Project／Operational Workspace SHOULD 僅保留完成目前工作所需的少量 Active Working Context，不作為永久 Chat History Database。 |

## 4. Context and Token Budget Model

### 4.1 Budget Profiles

每項工作 MUST 先選擇最小可行 Profile。Token 數為單次模型呼叫之初始輸入上限；若模型有效 Context Window 的 35% 更低，取較低者。

| Profile | 初始輸入上限 | 適用工作 | 典型內容 |
|---------|--------------|----------|----------|
| S | 8,000 tokens | 搜尋、分類、格式轉換、單檔修正、狀態回報 | Main baseline、Project State、單一任務、1～3 份相關檔案 |
| M | 24,000 tokens | 一般實作、測試、Review、跨少量模組修改 | Main baseline、EWO／Spec、3～10 份程式碼、直接相關測試 |
| L | 64,000 tokens | Architecture、Security、重大 Blocker、跨 Repository 決策 | 經篩選的正式來源、決策紀錄、必要證據與驗證結果 |

規則：

- Agent MUST 預留至少 20% 有效 Context Window 給輸出、工具結果與修正迴圈。
- `PROJECT_STATE.md` SHOULD 維持約 500～1,500 tokens。
- 初始 Context SHOULD 以 1～3 份規格／治理文件、3～10 份直接相關程式碼及必要測試為上限；數量不是最低讀取要求。
- Agent MUST NOT 因 Context Window 足夠而預設載入整個 Repository、全部 ADR、全部 Standards、完整 Git 歷史或完整對話。
- 無法精確取得 token 數時，MAY 以檔案數、行數與工具回傳大小估算，並優先縮小搜尋範圍。

### 4.2 Budget Escalation

從 S 升級至 M 或 L 前，Agent MUST 記錄至少一項理由：

- 直接來源互相衝突或缺少上位決策。
- 變更跨越多個模組、Repository 或 Governance Domain。
- 安全、隱私、資料一致性或不可逆風險需要更完整證據。
- 驗證失敗且目前 Context 無法定位原因。
- 使用者明確要求完整稽核或深度分析。

Budget 升級只擴大必要來源；MUST NOT 解除 §3 Operating Principles。

## 5. On-demand Context Loading

### 5.1 Loading Order

Agent MUST 依下列順序建立 Context Pack，並在資訊足夠時停止載入：

1. **Current Repository Baseline**：確認目標 Repository、`main` HEAD／等效版本與目前正式狀態；若無法直接讀取 `main`，MUST 明確標示 baseline 未驗證。
2. `PROJECT_STATE.md` 或目前 Closure Snapshot；視為操作快照，不得凌駕步驟 1。
3. 目前使用者意圖與 EWO／Issue／Spec。
4. 直接相關的 Approved Standard、Architecture 或 ADR 引用。
5. 直接受影響的程式碼、設定與測試。
6. 驗證輸出、差異與必要的最近歷史。
7. 只有在前述資訊不足時，才讀取 Historical Chat／Archived Session 的必要片段作為背景或 evidence locator。

Agent SHOULD 先以 Repository 搜尋定位文件，再讀取命中的必要段落或檔案；不得先讀取全部內容後再篩選。

Historical Chat 與 Workspace Context 若與目前 `main` 衝突，MUST 立即失效該衝突 Context，並以 `main` 重新組裝 Context Pack。

### 5.2 Expansion Triggers

只有發生下列情況時，MAY 擴大 Context：

| Trigger | 可擴大範圍 |
|---------|------------|
| 上位規則不明或衝突 | 對應 Constitution、Governance、Standard、Architecture 或 ADR |
| 介面或依賴影響不明 | 直接上游／下游介面、Dependency 與契約測試 |
| 驗證失敗 | 失敗路徑、相關實作、Fixture、Log 與最近差異 |
| 安全／隱私風險 | 適用安全基線、資料分類、威脅與控制證據 |
| 跨 Repository 變更 | 各 Repository 的正式契約與最小狀態快照 |
| Handoff 不完整 | 前一 Session 的 promoted refs、Closure Snapshot 與必要 lineage metadata；不得直接搬移完整 Chat |

擴大前 SHOULD 先使用搜尋、符號定位、Diff 或測試失敗訊息縮小範圍。

## 6. Project State and Closure Snapshot

### 6.1 PROJECT_STATE Contract

Repository SHOULD 於根目錄維護 `PROJECT_STATE.md`，至少包含：

| 欄位 | 規則 |
|------|------|
| Repository | 固定 Repository 名稱 |
| Source of Truth | 固定為 `main` 或 Repository 正式定義之等效 protected baseline |
| Baseline HEAD | 最近一次已驗證 baseline；新 Session MUST 重新確認，不得盲信舊值 |
| Branch | 目前工作 Branch；無 Branch 時為 `main` |
| Current Milestone | 目前 Milestone 或 `—` |
| Current EWO／Issue | 唯一工作識別碼與名稱 |
| Current PR | PR 編號或 `—` |
| Blocker | 無 Blocker 時明確記錄「無」 |
| Next Action | 單一、可執行的下一步 |
| Recent Decisions | 僅保留影響目前工作的少量決策與正式來源引用 |

規則：

- `PROJECT_STATE.md` 是可更新的操作快照，不是正式 Review、ADR、EWO 或歷史資料庫。
- 內容 MUST 使用繁體中文並遵循 AEOS-STD-001 §6.2；識別碼與必要技術名稱保留英文。
- MUST NOT 記錄 Secret、Credential、Token、PII、完整 Log 或未遮罩的客戶資料。
- EWO、Branch、PR、Blocker 或 Next Action 變更時 SHOULD 同步更新。
- 已完成的詳細歷史 MUST 移回正式文件、PR、EWO 或 Git History，不得累積於本檔案。
- 新 Session 讀取 `PROJECT_STATE.md` 後 MUST 以目前 `main` 驗證其 Baseline HEAD、Current EWO／PR 與關鍵狀態；不一致時以 Repository current state 為準。

### 6.2 Closure Snapshot

結束 EWO、Milestone、handoff 或長 Work Session 前，Agent SHOULD 產生可供新 Session 啟動的 Closure Snapshot，格式如下：

```text
Repository: <name>
Baseline: <main sha/ref>
Branch: <branch>
Session Owner: <human/agent accountable actor>
Current Milestone: <milestone>
Last Completed EWO/PR: <id>
Current EWO/Issue: <id>
Blockers: <none or concise list>
Next Action: <one action>
Important Decisions: <promoted doc-id/ADR/PR references only>
Validation: <latest result>
Lineage: <parent session reference if needed>
```

Snapshot SHOULD 控制在 500 tokens 內；詳細證據以連結或識別碼引用，不複製全文。Snapshot 是 handoff aid，不是 Fact Authority；新 Session 仍 MUST 先驗證目前 `main`。

## 7. Delta Output

正常交付回報 MUST 僅包含下列區塊：

| 區塊 | 必要內容 |
|------|----------|
| Files Changed | 新增／修改／刪除之檔案與一句目的；無變更時明確記錄「無」 |
| Validation | 已執行的檢查、測試與結果；未執行項目說明原因 |
| Risk | 剩餘風險、假設或「無已知風險」 |
| Next Action | 單一下一步；完成且無後續時記錄「無」 |

下列情況 MUST 展開必要分析，不受簡短輸出限制：

- Blocker、驗證失敗或部分完成。
- Architecture、ADR、Security、Privacy 或跨 Repository 決策。
- 需要人員核准、例外或不可逆操作。
- 使用者明確要求完整說明、Diff、證據或教學。

Delta Output MUST NOT 省略失敗、風險、未驗證項目或 Scope 偏差。

## 8. Operational Workspace and Work Session Lifecycle

### 8.1 Authority Hierarchy

Work Session 操作 MUST 遵循 AEOS-ARCH-010 所定義之 Authority Boundary：

```text
Repository main = System of Record / SSOT
Operational Workspace / Project = Active Workspace（非 Fact Authority）
Project Instructions / Approved Reference = Workspace Context（Derived Context）
Chat / Agent Session = Ephemeral Work Session
Merge + Closure = Knowledge Promotion Point
Archived Work Session = Historical Working Record（非 Fact Authority）
```

Workspace Context、Closure Snapshot、Cache 或 Historical Chat 不得取代 Repository `main`；若來源衝突，MUST 以目前 `main` 為準。

### 8.2 Work Session Lifecycle

標準 lifecycle overlay 為：

```text
Create → Load main Baseline → Execute → Validate → Review → PR → Merge → Closure → Archive
```

規則：

- 此 lifecycle 描述 **Work Session**，不是重新定義 EWO、Git、PR、Review、Approval 或 Release Workflow。
- `Load main Baseline` MUST 在 Historical Chat 載入之前完成。
- Session MAY 在 Execute / Validate / Review 間迭代；但 Repository mutation、protected operation 或 approval 仍受採用端正式 Workflow 控制。
- Merge 未發生或工作被中止時，Closure MUST 清楚標記 `not merged` / `abandoned` / `blocked`；不得產生虛假的完成狀態。
- Merge 後 SHOULD 執行 Closure，完成 Promotion Check 與 terminal disposition，再將 Session 移出 Active Set。

### 8.3 Active, Archive and Delete

| Disposition | 適用條件 | 要求 |
|-------------|----------|------|
| Active | 有未完成之 Current EWO／Issue／PR、明確 Blocker、待 Review／Approval，或短期內需要同一工作意圖繼續執行 | MUST 有 Session Owner、fresh baseline、單一 Next Action；SHOULD 保持少量 Active Session |
| Archive | 工作已 Merge + Closure、handoff 已完成、長 Session 已由新 Session 接手，或需保留 troubleshooting / decision provenance | MUST 完成 Promotion Check；Archive 後僅為 Historical Working Record |
| Delete | 重複 Session、誤開／空白 Session、無治理價值的臨時嘗試、已被正式 Evidence 完整取代且無 retention 必要的 troubleshooting Session | MUST 先完成 Promotion Check；若存在未 promotion 的重要資訊，MUST NOT Delete |

Additional rules：

- Merge / Closure 後，原 Work Session SHOULD Archive；只有存在明確尚未完成的 follow-up 且仍為同一工作意圖時 MAY 暫留 Active。
- troubleshooting Session 若包含 root cause、reproduction、security finding、正式選擇依據或唯一 Evidence，必須先 promotion，再 Archive／Delete。
- Delete 是資訊處置，不得用來隱藏失敗、Review finding、Approval history、security evidence 或 process deviation。
- 工具不支援 Archive／Delete 時，SHOULD 以命名、標籤、Project 分區或等效方法建立 terminal disposition，避免仍被視為 Active Context。

### 8.4 Promotion-before-disposal Check

Work Session 從 Active 離開前 MUST 檢查以下資訊是否需要提升至 Repository：

| 類別 | Promotion Target 例示 |
|------|-----------------------|
| Architecture / Design Decision | Architecture、ADR、Specification、Approved Report |
| Engineering Decision | EWO、Spec、PR、正式文件或 code/tests |
| Evidence | PR / Issue evidence、test result、review artifact、audit record、必要 log reference |
| Review Finding / Resolution | Review artifact、PR review thread、RC / disposition record |
| Approval / Exception | PR / Issue / governance record；不得只留在 Chat |
| Validation | CI、test report、PR check、closure record |
| Closure / Handoff | `PROJECT_STATE.md`、Completion Report、Closure Snapshot references |

Promotion Check 結果 MUST 為下列之一：

- `PROMOTION_COMPLETE`：需要保留的資訊均已存在 Repository 或正式 external evidence reference。
- `NO_PROMOTION_REQUIRED`：Session 僅含可丟棄的暫時過程，且不存在唯一決策／Evidence。
- `BLOCKED`：尚有重要資訊未 promotion；MUST 保持 Active 或先完成 promotion，不得 Archive／Delete。

### 8.5 Shared Project Ownership and Handoff

Shared Project／多人／多 Agent 工作 MUST 有單一 accountable **Session Owner**。Session Owner 可以是 Human 或被正式授權的 Agent actor，但：

- Session Owner 負責 current intent、baseline freshness、promotion status、handoff completeness 與 terminal disposition。
- Session Owner 不因此取得 Architecture Approval、Repository Owner、Merge 或 protected-operation 權限。
- 同一工作可有多個 contributor session，但 MUST 有一個 accountable owner 或明確 parent session。
- Handoff MUST 以最小 Handoff Contract 進行，不得把完整 Chat 當作必要移交物。

Minimum Handoff Contract：

```text
Repository / main baseline
Current EWO / Issue / PR
Session Owner / next owner
Current branch / working ref
Completed delta
Open blockers / risks
Validation status
Promoted decision/evidence refs
Single Next Action
Parent / child session lineage（若有）
```

接手 Session MUST 重新驗證 `main` 與 PR／Branch 狀態；不得假設 handoff snapshot 仍為最新。

### 8.6 Session Branch and Git Branch Separation

- Chat branch、forked conversation、child agent session 或 parallel reasoning path 統稱 **Session Lineage**。
- Session Lineage MAY 記錄 parent session、fork reason、owner 與 disposition，用於 context continuity。
- Session Lineage MUST NOT 被解讀為 Git branch、Commit lineage、PR relationship 或 merge history。
- Git Branch / PR / Merge Authority 仍由 Repository 與 YEOS Git Workflow（若採用）定義。
- 多個 Session 對同一 Git branch 工作時，MUST 以 Repository branch / commit state 為衝突裁決依據，而不是以哪個 Chat 較新為準。

### 8.7 Human + AI Agent Session Hygiene

為避免 Operational Workspace 退化為無法治理的 Chat History Database：

- 每個 agent execution / work session SHOULD 具有 `session_id`、`task/evidence ref`、`repository`、`baseline`、`owner/actor`、`status`、`promotion_status` 與 `terminal_disposition`。
- Agent SHOULD 優先建立新的 bounded Session，而不是無限延長一個全域 Chat。
- 大量平行 Session SHOULD 以 EWO／Issue／PR 或 task identity 分組，不以自然語言 Chat title 作唯一識別。
- Project Active Set SHOULD 只包含當前 milestone / workstream 必需 Session、待 Review／Approval Session 與未解除 Blocker Session。
- 已 terminal 的 Agent Session MUST 在 promotion 完成後從 Active Set 移除或等效隔離。
- Historical Session MAY 用於 provenance、root-cause research 或 audit，但 MUST NOT 自動注入新 Session 的 Context。

### 8.8 Conversation Switch Triggers

符合任一條件時 SHOULD 建立 Closure Snapshot 並切換至新 Work Session：

- EWO 或 Milestone 已完成，下一項工作具有不同意圖。
- 對話已累積超過 20 個實質工作回合。
- 可估算的 Context 使用量已超過有效 Context Window 的 50%。
- Agent 開始重複載入相同背景、遺漏已知決策或需要反覆摘要才能繼續。
- 工作從一般實作升級為 Architecture、Security 或跨 Repository 決策。
- Session ownership 發生 handoff，且新 owner 需要乾淨、可驗證的 Context Pack。

新 Session MUST 依 §5.1 先載入目前 `main` baseline，再使用 Closure Snapshot 與 `PROJECT_STATE.md`；MUST NOT 預設搬移完整聊天記錄。正式決策 MUST 先寫入 Repository 的適當權威文件，才能只靠 Snapshot 引用。

## 9. Model Tiering

模型路由 MUST 以能力與風險分級，不綁定供應商或型號：

| Tier | 適用工作 | 禁止作為唯一決策者 |
|------|----------|--------------------|
| T1 — Retrieval／Transformation | Repository 搜尋、分類、摘要、格式轉換、簡單 Log 分析 | Architecture、安全判定、Approval、重大重構 |
| T2 — Engineering | 一般實作、測試、Refactor、文件更新、常規 Code Review | 高風險安全例外、企業架構最終決策 |
| T3 — Advanced Reasoning | Architecture、ADR、Security、重大 Blocker、跨系統影響與高風險 Review | 人員 Governance Approval 的替代 |

規則：

- 工作 SHOULD 從能安全完成任務的最低 Tier 開始。
- 來源衝突、兩次修正仍失敗、風險升高或跨 Governance Boundary 時 MUST 升級 Tier。
- 模型降級 MUST NOT 降低驗證、Security、Privacy、Review 或 Approval 要求。
- T1／T2 產出的摘要與推論，供 T3 或人員決策使用前 MUST 回到目前來源驗證。

## 10. Prompt Cache and Semantic Cache

### 10.1 Prompt Cache

Prompt Cache SHOULD 用於穩定且重複的 Prompt Prefix，例如固定 Governance 指示、工具契約與不常變動的 Standard 摘要。

規則：

- 穩定內容 SHOULD 位於 Prompt 前段；當次 EWO、使用者輸入、Diff 與動態資料置於後段。
- Cache Key MUST 至少區分 Repository、Prompt Template Version、模型／能力級別、語言、權限範圍與穩定來源版本或內容雜湊。
- 任一穩定來源、權限、模型行為或 Prompt Template 變更時 MUST 使舊 Cache 失效。
- Prompt Cache 命中 MUST NOT 跳過目前 EWO、Repository State、fresh `main` baseline 或安全邊界驗證。

### 10.2 Semantic Cache

Semantic Cache MAY 用於低風險、可重算且非權威的查詢，例如術語查找、重複分類、靜態文件摘要與已遮罩的常見 Log 模式。

Semantic Cache MUST NOT 直接重用於：

- Architecture／ADR／Security／Privacy 最終決策。
- Review Decision、Approval、Merge、Release 或 EWO Close。
- 目前 Repository 狀態、Branch、PR、Issue、Dependency Version 或其他易變事實。
- 會改變外部狀態、產生不可逆影響或涉及個人化權限的操作。

### 10.3 Security, Isolation and Invalidation

- Secret、Credential、Access Token、未遮罩 PII、客戶內容與受限制資料 MUST NOT 寫入共享 Cache。
- Cache MUST 依 Repository、Tenant、使用者權限與資料分類隔離；不得跨越授權邊界命中。
- Cache Entry MUST 具有 TTL、來源識別碼、來源版本／雜湊、建立時間與失效條件。
- 來源變更、權限撤銷、資料分類升級、Security Incident 或驗證不一致時 MUST 立即失效。
- Cache Miss、失效或不確定時 MUST 回到目前來源重新計算；不得猜測或使用過期結果。

## 11. Prompt Construction Rules

每次工作 Prompt SHOULD 只包含：

1. 目前意圖與完成條件。
2. 已驗證的 Repository `main` baseline reference。
3. `PROJECT_STATE.md` 的必要欄位。
4. 目前 EWO／Spec 與直接相關來源。
5. 預期輸出格式與驗證要求。

Prompt MUST 以引用取代重述既有 Governance 規範。Agent 不得在每個回合重貼 Constitution、Standards 或完整歷史；只有當模型無法直接存取來源時，才提供完成任務所需的最小節錄。

Historical Chat SHOULD NOT 作為固定 Prompt Prefix；需要使用時 MUST 先驗證其引用仍與 `main` 一致。

## 12. Governance and Exceptions

- 本標準不改變 AEOS-ARCH-010 Formal Workspace、AEOS-CON-001、AEOS-DIA-001、AEOS-STD-001～AEOS-STD-005 或 YEOS Engineering Workflow 之權威與責任。
- Context／Token 最佳化造成來源不足、驗證不完整或安全不確定時，Agent MUST 停止最佳化並擴大必要 Context。
- 偏離 Budget Profile、Cache 禁止規則或 Session Lifecycle 時，MUST 於 Risk / Review Evidence 記錄理由、範圍、資料分類、補償控制與解除條件。
- Session Archive／Delete 不得用於規避 retention、legal hold、security evidence、Review、Approval 或 audit requirements；若採用端另有更嚴格保存規則，以較嚴格規則為準。
- 任何例外 MUST NOT 跳過人員 Review、Approval、Branch Protection 或正式決策程序。

## 13. Validation

AI Engineering 工作完成前 MUST 驗證：

| # | 驗證項目 | 通過條件 |
|---|----------|----------|
| V-001 | Intent Scope | 載入內容與目前 EWO／Issue 直接相關 |
| V-002 | Budget Profile | 已使用最小可行 Profile；升級具理由 |
| V-003 | Source Authority | 關鍵結論可追溯至目前 Repository `main` 正式來源 |
| V-004 | State Freshness | `PROJECT_STATE.md` 與 `main`、Branch／EWO／PR／Blocker 一致 |
| V-005 | Delta Completeness | Files Changed、Validation、Risk、Next Action 均已回報 |
| V-006 | Session Handoff | 切換 Session 時具有 Closure Snapshot／Handoff Contract，且無完整歷史複製 |
| V-007 | Model Routing | Tier 與工作風險相符；必要時已升級 |
| V-008 | Cache Safety | Cache Key、隔離、TTL、失效與禁止資料符合 §10 |
| V-009 | Language | Markdown 與敘述內容符合 AEOS-STD-001 §6.2 |
| V-010 | Promotion | Archive／Delete 前為 `PROMOTION_COMPLETE` 或 `NO_PROMOTION_REQUIRED`；不得為 `BLOCKED` |
| V-011 | Disposition | Terminal Session 已明確 Archive／Delete／equivalent isolation，不持續污染 Active Set |
| V-012 | Shared Ownership | Shared Project / multi-agent session 具有 accountable Session Owner 與可驗證 handoff |
| V-013 | Lineage Separation | Session Lineage 未被當作 Git branch / PR / merge authority |

## 14. Compliance

- 本標準生效後，適用之 AI Engineering 工作 MUST 符合 §3～§13。
- 不得以節省 token、方便接續或保留 Chat 歷史為由省略 fresh baseline、必要 Context、驗證、風險揭露或 Governance 步驟。
- Review Owner SHOULD 以 §13 作為 Standard Review 與後續落地檢查依據。
- 本標準之變更 MUST 經 EWO 與 Standard Review；核准與合併依 AEOS-STD-005。
- 與上位文件衝突時，以 AEOS Governance Hierarchy 及 AEOS-ARCH-010 Authority Boundary 為準。

### 14.1 Compliance Checklist

| 檢查項目 | 檢查內容 |
|----------|----------|
| Baseline | 新 Session 先驗證 Repository `main`，再載入 Project State／Historical Context |
| Context | 按 §5 順序載入，未預設載入整個 Repository 或完整對話 |
| Budget | Profile、預留比例與升級理由符合 §4 |
| State | `PROJECT_STATE.md` 精簡、最新且不含敏感資料 |
| Output | 採用 Delta Output，且未隱藏失敗或風險 |
| Session | 達切換條件時已建立 Closure Snapshot；Lifecycle 與 terminal disposition 清楚 |
| Promotion | 重要 Decision、Evidence、Review、Approval、Validation、Closure 不只存在於 Chat |
| Shared Project | 有 accountable Session Owner、Handoff Contract 與 context continuity |
| Active Set | Operational Workspace 僅維持少量 Active Working Context |
| Lineage | Chat / Agent Session Lineage 與 Git branch authority 分離 |
| Model | 使用最低安全 Tier，風險升高時已升級 |
| Cache | 僅快取允許內容，具隔離、TTL 與失效控制 |
| Language | 遵循 AEOS-STD-001 §6.2 的繁體中文原則 |
| Governance | 未省略 EWO、Review、Approval、Branch Protection 或安全控制 |

## 15. References

| # | 文件 | 型別 | 用途 |
|---|------|------|------|
| REF-001 | [AEOS-ARCH-001 — Architecture Baseline](../architecture/AEOS-ARCH-001-Architecture-Baseline.md) | Architecture Entry Document | AEOS 正式架構入口與 Fact Authority |
| REF-002 | [AEOS-ARCH-010 — Workspace Architecture](../architecture/AEOS-ARCH-010-Workspace-Architecture.md) | Architecture | Formal Enterprise Workspace 與 Operational Workspace / Work Session Authority Boundary |
| REF-003 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md) | Constitution | Repository Governance 與變更管理 |
| REF-004 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、位置、生命週期與引用 |
| REF-005 | [AEOS-GOV-001 — Enterprise Governance Roadmap](../governance/AEOS-GOV-001-Enterprise-Governance-Roadmap.md) | Governance | 本標準之 Roadmap 登錄 |
| REF-006 | [AEOS-STD-001 — Documentation Format Standard](AEOS-STD-001-Documentation-Format-Standard.md) | Standard | Markdown、撰寫與繁體中文規則 |
| REF-007 | [AEOS-STD-002 — Metadata Standard](AEOS-STD-002-Metadata-Standard.md) | Standard | Metadata 與 Frontmatter |
| REF-008 | [AEOS-STD-003 — Cross-reference Standard](AEOS-STD-003-Cross-reference-Standard.md) | Standard | 引用與 Single Source of Truth |
| REF-009 | [AEOS-STD-004 — Naming Standard](AEOS-STD-004-Naming-Standard.md) | Standard | 文件、Branch、EWO 與識別碼命名 |
| REF-010 | [AEOS-STD-005 — Review Standard](AEOS-STD-005-Review-Standard.md) | Standard | Review、Approval、Merge 與 Close |
| REF-011 | EWO-AEOS-0043 — AI Engineering Context and Token Budget Standard | EWO | 本文件之原始工作來源 |
| REF-012 | SR-AEOS-0043-R1 — Standard Review | Review | 原始 Standard Review；決策為 REQUEST CHANGES |
| REF-013 | SR-AEOS-0043-R2 — Standard Re-review | Review | 原始 R1 全部 RC Resolved；決策為 APPROVED |
| REF-014 | EWO-AEOS-0047 — AI Workspace / Project / Work Session Lifecycle Governance | EWO | 本次 lifecycle amendment 授權來源 |
| REF-015 | [AEOS-RPT-005 — AI Workspace / Project / Work Session Lifecycle Governance Gap Analysis](../reports/AEOS-RPT-005-AI-Workspace-Project-Work-Session-Lifecycle-Governance-Gap-Analysis.md) | Report | Gap Analysis、Authority Boundary 與 Minimum Necessary Change 依據 |

本標準（AEOS-STD-007）為 AEOS AI Engineering Context、Token Budget 與 Operational Work Session Lifecycle 規範之唯一來源；其他文件 SHOULD 以引用取代重述。

## 16. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-28 | EWO-AEOS-0047 amendment（Review）：新增 Repository `main` first baseline loading、Operational Workspace / Work Session Authority Hierarchy、Create → Load main → Execute → Validate → Review → PR → Merge → Closure → Archive、Active / Archive / Delete、Promotion-before-disposal、Shared Project ownership / handoff / continuity、Session Lineage 與 Agent Session Hygiene；不建立平行 Standard 或 vendor-specific implementation | ChatGPT |
| 1.0.0 | 2026-08-22 | 依 SR-AEOS-0043-R2 完成 Standard Re-review：R1 全部 RC 已 Resolved，Metadata、格式、引用、命名、內容完整性與 Review Traceability 驗證通過；決策為 APPROVED（EWO-AEOS-0043） | Codex |
| 0.2.0 | 2026-08-22 | 依 SR-AEOS-0043-R1（REQUEST CHANGES）進入 Review：記錄 PR #49 於 Draft 狀態合併造成的生命週期不一致，補齊 Review Traceability；文件不得在 R2 APPROVED 前升為 Approved 1.0.0（EWO-AEOS-0043） | Codex |
| 0.1.0 | 2026-08-22 | 初版建立：定義按需載入 Context、Token Budget、Project State／Closure Snapshot、Delta Output、長對話切換、模型分級、Prompt／Semantic Cache、驗證與合規（EWO-AEOS-0043） | Codex |