---
doc-id: AEOS-RPT-005
doc-name: AI Workspace Project Work Session Lifecycle Governance Gap Analysis
doc-type: Report
repository: AEOS
version: 0.1.0
status: Draft
owner: Architecture Owner
created: 2026-08-28
updated: 2026-08-28
related:
  - EWO-AEOS-0047
  - AEOS-ARCH-001
  - AEOS-ARCH-008
  - AEOS-ARCH-010
  - AEOS-ARCH-013
  - AEOS-STD-005
  - AEOS-STD-007
---

# AEOS-RPT-005 — AI Workspace / Project / Work Session Lifecycle Governance Gap Analysis

> EWO-AEOS-0047：以 `main` 為唯一事實來源，盤點 AEOS 對 Operational Workspace、Project Context 與 Work Session Lifecycle 的既有涵蓋與缺口，提出不重複既有架構與標準的 Minimum Necessary Change。本報告為 Review Package，不自行改寫 Approved Architecture 或 Standard。

## 執行摘要

AEOS 現有正式基線已具備本次治理需求的大部分基礎：`AEOS-ARCH-008` 將 Repository 定義為版本化治理與交付邊界；`AEOS-ARCH-010` 已定義 Formal Enterprise Workspace、Ownership、Membership、Lifecycle，並明確指出 Workspace 不等同 Project；`AEOS-STD-007` 已建立 Repository as Memory、按需 Context Loading、`PROJECT_STATE.md`、Closure Snapshot 與 Conversation Lifecycle，且要求正式決策在對話切換前提升到 Repository。

本次缺口不是缺少另一套 Workspace Architecture，而是缺少「**工具層 Operational Workspace / Project / Work Session 與 Repository Fact Authority 之間的正式邊界與處置規則**」。因此最小必要變更為：

1. **AEOS-ARCH-010 Amendment**：只補充 Formal Enterprise Workspace 與 Operational Workspace / Project / Work Session 的 Authority Boundary，不變更既有 Formal Workspace 身分、Composition、Type、Level 或 Lifecycle。
2. **AEOS-STD-007 Amendment**：在既有 Context / Conversation Lifecycle 上補足 Active / Archive / Delete、Promotion-before-disposal、Shared Project 歸屬 / 交接 / 分支沿革、Context Continuity 與 Human + AI Agent 工作階段 hygiene。
3. **不新增平行 AEOS Context / Workspace Standard**；後續 YEOS 只承接 Engineering 執行規則，不重複 AEOS 上位 Authority Boundary。

## 1. 基線

| 項目 | `main` 正式狀態 |
|------|-----------------|
| AEOS `main` HEAD | `6984a9820922d8b056f69d2cea1d8d4acbe04d46` |
| AEOS-ARCH-001 | Approved 1.5.0 |
| AEOS-ARCH-008 | Approved 1.1.0 |
| AEOS-ARCH-010 | Approved 1.1.0 |
| AEOS-ARCH-013 | Approved 1.0.0 |
| AEOS-STD-005 | Approved |
| AEOS-STD-007 | Approved 1.0.0 |

規則：本報告只使用上述 `main` 正式基線作為判斷依據；Chat、Project 歷史、先前對話摘要或未合併分支不得作為正式 Fact Authority。

## 2. 現有覆蓋範圍

### 2.1 Repository Authority 已存在

`AEOS-ARCH-008` 已將 Repository 定義為版本化治理與交付邊界，並將 Enterprise Root Repository 定位為跨 Repository 的治理與 Single Source 邊界。這已提供「正式狀態必須落在受版本控制 Repository」的架構基礎。

### 2.2 Formal Enterprise Workspace 已存在

`AEOS-ARCH-010` 已明確：

- Workspace 是跨 Platform、Capability、Repository、Dependency 與治理資產的 Formal Enterprise Architecture Boundary。
- Workspace 不等同 Repository、Platform、Project、Runtime Environment 或 Team。
- Workspace 有正式 Owner、Membership、Lifecycle 與 Architecture Reference。
- Workspace 層 Shared Governance 不得取代 Repository Governance。

因此 ChatGPT Project 類工具空間不得直接被重新命名或視為 `AEOS-ARCH-010` 所定義的 Formal Enterprise Workspace。

### 2.3 Context / Conversation Lifecycle 已存在

`AEOS-STD-007` 已正式規範：

- Repository as Memory：Repository 的 Approved 文件、目前 EWO 與程式碼為工程記憶；Conversation 不得成為唯一事實來源。
- 新工作 Context 先載入 `PROJECT_STATE.md` / Closure Snapshot，再載入 EWO / Issue / Spec 與直接相關 Approved sources。
- 不得因 Context Window 足夠而載入完整 Repository 或完整對話。
- `PROJECT_STATE.md` 是操作快照，不是 ADR、Review、EWO 或歷史資料庫。
- 長對話、工作意圖切換或 Context 擴張時，應建立 Closure Snapshot 並切換新對話。
- 新對話不得預設搬移完整聊天記錄。
- 正式決策必須先寫入 Repository 的適當權威文件，才能只靠 Snapshot 引用。

這些規則已足以作為本次補強之基底，不應再建立第二套 Context Standard。

## 3. 差距分析

| Gap | 現況 | 風險 | 最小補強位置 |
|-----|------|------|--------------|
| G-01 Operational Workspace 身分 | ARCH-010 只說 Formal Workspace 不等同 Project，未定義 Project 類工具空間的治理身分 | 將 ChatGPT Project 誤當 Enterprise Workspace 或 Fact Authority | ARCH-010 Amendment |
| G-02 Authority Hierarchy | STD-007 有 Repository as Memory，但未完整列出 Project / Context / Chat / Archive 的權威關係 | 歷史 Chat、Project Instructions 或摘要凌駕 `main` | ARCH-010 + STD-007 |
| G-03 Session 生命週期 overlay | STD-007 有 conversation-switch trigger，但沒有 Create → Load `main` → ... → Archive 的完整工作階段 overlay | 新工作階段依歷史聊天重建狀態；Merge 後工作仍長期 Active | STD-007 Amendment |
| G-04 Active / Archive / Delete | 未定義 | Project 持續累積疑難排解、重複或已完成 Chat | STD-007 Amendment |
| G-05 Promotion-before-disposal | 已要求正式決策寫入 Repository，但 Archive / Delete 前檢查尚未制度化 | 未提升的 Decision / Evidence 因刪除而遺失 | STD-007 Amendment |
| G-06 Shared Project 歸屬 / 交接 | Formal Workspace 有 Owner/Membership，但 Chat/Session 沒有負責擁有者與交接契約 | 多人 / 多 Agent 接手時 Authority、Branch、Next Action 混淆 | STD-007 Amendment |
| G-07 Chat Branch 語意 | 尚未區分 conversation 沿革與 Git 分支 | 將 Chat Branch 誤視為 Repository 分支或正式變更線 | ARCH-010 + STD-007 |
| G-08 Context 延續性 | 有 Closure Snapshot，但未定義 Shared Project 交接最小欄位 | 交接依靠全文聊天複製 | STD-007 Amendment |
| G-09 Agent 工作階段 volume | 未定義 Project active-set hygiene | Human + AI Agent 大量產生工作階段，Project 退化為 chat 歷史資料庫 | STD-007 Amendment |
| G-10 Knowledge 提升 point | Merge、Closure 已各有流程，但未統合成工作階段 knowledge 提升 checkpoint | Working knowledge 留在 Chat 而未轉成 auditable record | STD-007 Amendment；引用既有 Review / Merge / Close 權限 |

## 4.權限邊界決策建議

### 4.1 正式權威模型

本次 Amendment SHOULD 採下列工具中立 Authority Model：

| 層級 / 事件 | Tool-neutral 定義 | ChatGPT 對應 | Authority |
|-------------|-------------------|--------------|-----------|
| Repository `main` | System of Record / SSOT | GitHub Repository `main` | **唯一目前工程 Fact Authority**；正式 Architecture、Standard、Code、EWO、Review / Closure 證據依其治理載體生效 |
| Operational Workspace | Active Collaboration Workspace | ChatGPT Project | 協作與工作聚合空間；**非 Fact Authority** |
| Workspace Context | Operational Workspace 的受控 Context | Project Instructions / Approved Reference | 提供範圍、instructions、參考 pointers；屬衍生 Context，MUST NOT 凌駕目前 `main` |
| Work Session | Ephemeral 執行 / 推理工作階段 | Chat | 暫態工作單位；內容在 Promotion 前不具正式 Fact Authority |
| Session Lineage | Work Session 的分支 / 延伸關係 | Chat Branch | 只表示 conversation 沿革；MUST NOT 等同 Git 分支、PR 或正式決策線 |
| Knowledge Promotion Point | Working knowledge 提升為正式紀錄的 checkpoint | Merge + Closure | 事件 / Gate，不是獨立 Fact Authority；要求 surviving knowledge 進入 Repository 正式載體 |
| Archived Work Session | Historical Working Record | Archived Chat | 歷史工作紀錄；**非 Fact Authority**，不得覆蓋目前 `main` |

### 4.2 正式的企業工作空間邊界

`Operational Workspace` 不是 `AEOS-ARCH-010` 的新 Workspace Type。它是 Formal Enterprise Workspace 內或其協作表面上的 **tool-level operational construct**：

- 不具有 Formal Workspace ID / Architecture Reference / Lifecycle 權限。
- 不得建立新 Platform、Capability、Repository 或 Architecture 事實。
- 不得因工具支援 Project / Shared Project / Chat Branch 而擴張 AEOS Architecture Authority。
- 若未來 Operational Workspace 需要成為正式架構元素，必須另循 Architecture Change；本 EWO 不做此變更。

## 5. 提議的 Work Session 生命週期

```text
Create
  → Load `main` Baseline
  → Execute
  → Validate
  → Review
  → PR
  → Merge
  → Closure
  → Archive
```

此流程是 **Work Session Lifecycle Overlay**，用途是治理 Chat / Agent Session 的 Context 與狀態，不重新定義：

- YEOS 開發流程；
- Git 分支 / commit / PR / merge 規則；
- AEOS 審查/核准/關閉門；
- AI 代理指令核准。

### 5.1 階段規則

|舞台|最低規則|
|-------|--------------|
| Create | Session MUST 綁定明確 work 意圖 / EWO / Issue / PR 或 investigation 範圍 |
| Load `main` Baseline | MUST 取得目前 Repository `main` HEAD 與直接相關正式來源；不得以歷史 Chat 取代 |
| Execute | 只處理已授權範圍；Working Notes 屬暫態資料 |
| Validate | 結論 / 變更 MUST 依適用驗證規則檢查 |
| Review | 正式判斷依既有 Review Authority；Chat 內同意不得冒充正式 Review |
| PR | 變更透過受治理 PR 承載；Conversation Branch 不等同 PR Branch |
| Merge | 合併後 `main` 成為新的目前事實基線 |
| Closure | 產生 Closure Snapshot，確認 Decisions / Evidence / Review / Approval / Validation / Close record 已提升到 Repository |
| Archive | Session 轉為 Historical Working Record；新工作不得依其內容推定目前狀態 |

## 6. Active / 檔案 / 刪除規則

### 6.1 Active

Work Session SHOULD 僅在符合至少一項情況時維持 Active：

- 對應之 EWO / Issue / PR 正在執行或審查。
- 尚有未完成的 Blocker、Review finding、Approval 關卡或交接。
- Session 包含仍需提升到 Repository 的重要工作結果，且提升尚未完成。
- 明確被指定為目前 work unit 的延續性工作階段。

「可能日後有用」本身不足以讓 Session 永久 Active。

### 6.2 存檔

符合下列情況 SHOULD Archive：

- Merge + Closure 已完成，提升 check 通過。
- Handoff 已完成，舊 Session 不再承擔 Active 歸屬。
- Investigation / design exploration 具有歷史追溯價值，但其 surviving conclusions 已寫入 Repository。
- 工作被正式 supersede / cancelled，但保留歷史脈絡具稽核價值。

Archive 後的 Session 是 Historical Working Record，MUST NOT 作為新工作的目前狀態權限。

### 6.3 刪除

只有同時符合下列條件時 MAY Delete：

- Session 主要是疑難排解、重複操作、誤建立、重複 Session、無治理價值的嘗試或已被完整取代的低價值工作紀錄。
- Promotion Check 證明不存在尚未寫入 Repository 的重要 Decision、Evidence、Review、Approval、Validation、Blocker、Contract 或 Closure 事實。
- 沒有尚未完成的交接 / 歸屬責任。
- 刪除不違反適用的安全、稽核、法遵或資料保存要求。

若無法證明上述條件，SHOULD Archive 而非 Delete。

## 7. 處置前提升

Work Session 在 Archive / Delete 前 MUST 執行 Promotion Check。以下資訊若對正式狀態仍有價值，MUST 先提升到 Repository 的適當載體：

| Working Knowledge | 正式載體例 |
|-------------------|------------|
|架構/邊界決策|架構 / ADR / 已核准規格 |
|工程要求/驗收| EWO / 規格 / 問題 / 合約 |
|實施結果 |提交/PR/代碼/設定|
|驗證證據| CI / 測試結果 / 審查證據 / 報告 |
|審查結果/決定 |正式審查/PR 審查/治理審查神器|
|核准|儲存庫管理的核准記錄 |
|阻礙/延期工作| EWO / 問題 / 專案狀態 |
|關閉/切換| EWO 關閉/關閉快照/專案狀態/PR 參考資料 |

Working conversation 本身不是上述正式載體的替代品。

## 8. 共享項目、所有權和移交

### 8.1 會話所有權

- 每個 Active Work Session MUST 有一個負責 Session Owner；可由 Human 或受治理角色承擔，但最終 Repository accountability 仍依既有 Governance。
- 多個 Human / AI Agent MAY 協作，但不得出現「所有人都能改、沒有人負責延續性」的工作階段。
- Agent 身分、工作階段身分、work item 與 Repository 範圍 SHOULD 可追溯。

### 8.2 交接合約

跨 Human、Agent 或新 Chat 交接時，至少應交接：

```text
Repository
Authoritative main commit
Working branch（若有）
Current EWO / Issue
Current PR
Session owner / next owner
Scope
Open decisions / unresolved findings
Latest validation
Blockers
Promotion status
Next action
```

Handoff SHOULD 使用 Repository 參考與 Closure Snapshot，不應以完整 transcript 複製作為主要延續性 mechanism。

### 8.3 聊天分支/會話沿襲

- Chat Branch 只表示 Work Session 沿革 / alternative exploration。
- 每個分支工作階段在開始實質工作前仍 MUST 重新確認目前 `main` 基準；不得只繼承父項 chat 的舊事實。
- Session 分支的結論若要成為正式決策，必須依同一 Promotion / Review / Merge / Closure 路徑提升。

## 9. 人員 + AI 代理會話衛生

Project / Operational Workspace 應維持 **Minimal Active Working Context**，而不是永久保存全部執行歷史：

- Active set 只保留目前 work items、pending reviews / approvals、已封鎖 work 與有效交接。
- 已 Closure 的 sessions SHOULD 轉 Archive；低價值且 promotion-safe 的 sessions MAY Delete。
- Agent automation SHOULD 為工作階段記錄 work item、基準 commit、擁有者 / actor、範圍、分支 / EWO / PR refs、驗證與提升狀態。
- Agent 不得因大量工作階段可被保存，就把 Project 當成 long-term engineering 記憶體；long-term engineering 記憶體仍在 Repository。
- 新工作階段預設先重新取得目前 `main`，再使用 Project Context / Snapshot 作為輔助。

## 10. AEOS / YEOS 權限邊界

| 領域 | AEOS | YEOS |
|------|------|------|
| Formal Enterprise Workspace / Repository 權限 | **Authority** | 引用 / 採用 |
| Operational Workspace / Project / Work Session 的上位 Authority Boundary | **Authority** | 不重複定義 |
| Context loading / Project State / Closure Snapshot / archive-delete principles | **Authority：AEOS-STD-007** | 將規則轉成 Engineering operating controls |
| Development / Git / PR / Merge 工作流程 | 不重新定義 | **Authority：ENG-STD-004 / 005** |
| Human + AI 協作 / traceability | 上位架構 alignment | **Authority：GOV-POL-005** |
| AI Agent 命令核准 | 不重新定義 | **Authority：ENG-STD-008** |
| Engineering 工作階段生命週期 compliance / 交接規則 | 提供上位邊界 | **YEOS 新 Standard 承接** |

## 11. 最低必要變更計劃

### Phase A — 本 PR：Review Package

1. 建立 EWO-AEOS-0047 Issue。
2. 建立本 Gap Analysis / Amendment Plan。
3. 不修改 Approved `AEOS-ARCH-010` / `AEOS-STD-007`，避免 Review 前直接改動正式權限。
4. 提交 Draft PR 進 Architecture / Standard Review。

### Phase B — Owner / Architecture Review 通過後

1. Amend `AEOS-ARCH-010`：新增 Operational Workspace / Project / Work Session Authority Boundary 與 tool-neutral 對應；只做邊界 clarification，不新增 Formal Workspace Type。
2. Amend `AEOS-STD-007`：新增完整 Work Session Lifecycle、Active / Archive / Delete、Promotion-before-disposal、Shared Project 歸屬 / 交接 / 上下文延續性、工作階段 hygiene。
3. 更新 Revision History、參考與必要 cross-reference。
4. 儲存庫驗證+審查+核准。

### C 階段 — YEOS 採用

由 YEOS 依其 AIG-M7-T001 Future Standard Roadmap 建立 Engineering-level Session Lifecycle Standard，引用 AEOS 上位邊界，不複製 AEOS 架構定義。

## 12. 修改驗證清單

| ＃|驗證 |通過條件 |
|---|------------|----------------|
| V-01 | No Architecture Redesign | Formal Enterprise Workspace 身分 / 類型 / 層級 / 組成不變 |
| V-02 | Repository Authority | `main` 明確為目前 System of Record / SSOT |
| V-03 | Non-authoritative Chat | Project Context、Chat、Archive 均無權覆蓋 `main` |
| V-04 | Fresh Baseline | 新工作階段 / chat 分支先取得目前 `main` |
| V-05 | Promotion | Archive / Delete 前有提升 check |
| V-06 | Lifecycle | Create → Load `main` → Execute → Validate → Review → PR → Merge → Closure → Archive 完整 |
| V-07 | Shared Collaboration | Owner / 交接 / 沿革 / 上下文延續性可稽核 |
| V-08 | Agent Scale | Project 維持 minimal active set，不成為永久 chat 歷史資料庫 |
| V-09 | No Duplicate Standard | 不新增與 AEOS-STD-007 平行的 Context / Conversation Standard |
| V-10 | YEOS Separation | 不重寫 ENG-STD-004 / 005 / 008 權威範圍 |

## 13. 風險與審查問題

### Architecture Review 必須確認

1. `Operational Workspace` 作為 tool-level construct 是否足以避免與 `AEOS-ARCH-010` Formal Workspace 混淆。
2. Authority 模型是否應被視為 ARCH-010 的邊界 clarification，而非新的 Workspace Type / Architecture entity。
3. `Merge + Closure` 作為 Knowledge Promotion Point 是否只屬生命週期 checkpoint，而不建立新的權限層。

### Standard Review 必須確認

1. Archive / Delete 準則是否足以避免 unpromoted knowledge loss。
2. Shared Project 交接契約是否維持 tool-neutral，不綁定 ChatGPT 供應商能力。
3. YEOS 採用是否可以只引用上位權限，而把執行 compliance 留在 YEOS。

## 14. 參考文獻

| 文件 | 用途 |
|------|------|
| AEOS-ARCH-001 — Architecture Baseline | Approved 架構入門/權威|
| AEOS-ARCH-008 — Repository Architecture |儲存庫治理與交付邊界 |
| AEOS-ARCH-010 — Workspace Architecture |正式的企業工作空間認同/邊界/生命週期|
| AEOS-ARCH-013 — Enterprise AI Agent Architecture |人員 + AI 代理架構對齊 |
| AEOS-STD-005 — Review Standard |審查/核准/合併/關閉權限|
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard |儲存庫作為記憶體、上下文、專案狀態、關閉快照、會話生命週期 |
| EWO-AEOS-0047 / Issue #61 | 本次工作來源 |

## 15. 修訂歷史

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-28 | 初版：完成 `main` 基線盤點、Gap Analysis、Authority Boundary、Minimum Necessary Change 與 Amendment Execution Plan；尚未修改 Approved Architecture / Standard | ChatGPT |
