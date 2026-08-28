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

## Executive Summary

AEOS 現有正式基線已具備本次治理需求的大部分基礎：`AEOS-ARCH-008` 將 Repository 定義為版本化治理與交付邊界；`AEOS-ARCH-010` 已定義 Formal Enterprise Workspace、Ownership、Membership、Lifecycle，並明確指出 Workspace 不等同 Project；`AEOS-STD-007` 已建立 Repository as Memory、按需 Context Loading、`PROJECT_STATE.md`、Closure Snapshot 與 Conversation Lifecycle，且要求正式決策在對話切換前提升至 Repository。

本次缺口不是缺少另一套 Workspace Architecture，而是缺少「**工具層 Operational Workspace / Project / Work Session 與 Repository Fact Authority 之間的正式邊界與處置規則**」。因此最小必要變更為：

1. **AEOS-ARCH-010 Amendment**：只補充 Formal Enterprise Workspace 與 Operational Workspace / Project / Work Session 的 Authority Boundary，不變更既有 Formal Workspace 身分、Composition、Type、Level 或 Lifecycle。
2. **AEOS-STD-007 Amendment**：在既有 Context / Conversation Lifecycle 上補足 Active / Archive / Delete、Promotion-before-disposal、Shared Project ownership / handoff / branch lineage、Context Continuity 與 Human + AI Agent session hygiene。
3. **不新增平行 AEOS Context / Workspace Standard**；後續 YEOS 只承接 Engineering execution 規則，不重複 AEOS 上位 Authority Boundary。

## 1. Baseline

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

## 2. Existing Coverage

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
- 長對話、工作意圖切換或 Context 膨脹時，應建立 Closure Snapshot 並切換新對話。
- 新對話不得預設搬移完整聊天記錄。
- 正式決策必須先寫入 Repository 的適當權威文件，才能只靠 Snapshot 引用。

這些規則已足以作為本次補強之基底，不應再建立第二套 Context Standard。

## 3. Gap Analysis

| Gap | 現況 | 風險 | 最小補強位置 |
|-----|------|------|--------------|
| G-01 Operational Workspace 身分 | ARCH-010 只說 Formal Workspace 不等同 Project，未定義 Project 類工具空間的治理身分 | 將 ChatGPT Project 誤當 Enterprise Workspace 或 Fact Authority | ARCH-010 Amendment |
| G-02 Authority Hierarchy | STD-007 有 Repository as Memory，但未完整列出 Project / Context / Chat / Archive 的權威關係 | 歷史 Chat、Project Instructions 或摘要凌駕 `main` | ARCH-010 + STD-007 |
| G-03 Session lifecycle overlay | STD-007 有 conversation-switch trigger，但沒有 Create → Load `main` → ... → Archive 的完整 session overlay | 新 session 依歷史聊天重建狀態；Merge 後工作仍長期 Active | STD-007 Amendment |
| G-04 Active / Archive / Delete | 未定義 | Project 持續累積 troubleshooting、重複或已完成 Chat | STD-007 Amendment |
| G-05 Promotion-before-disposal | 已要求正式決策寫入 Repository，但 Archive / Delete 前檢查尚未制度化 | 未提升的 Decision / Evidence 因刪除而遺失 | STD-007 Amendment |
| G-06 Shared Project ownership / handoff | Formal Workspace 有 Owner/Membership，但 Chat/Session 沒有 accountable owner 與 handoff contract | 多人 / 多 Agent 接手時 Authority、Branch、Next Action 混淆 | STD-007 Amendment |
| G-07 Chat Branch semantics | 尚未區分 conversation lineage 與 Git branch | 將 Chat Branch 誤視為 Repository branch 或正式變更線 | ARCH-010 + STD-007 |
| G-08 Context continuity | 有 Closure Snapshot，但未定義 Shared Project handoff 最小欄位 | 交接依靠全文聊天複製 | STD-007 Amendment |
| G-09 Agent session volume | 未定義 Project active-set hygiene | Human + AI Agent 大量產生 session，Project 退化為 chat history database | STD-007 Amendment |
| G-10 Knowledge promotion point | Merge、Closure 已各有流程，但未統合成 session knowledge promotion checkpoint | Working knowledge 留在 Chat 而未轉成 auditable record | STD-007 Amendment；引用既有 Review / Merge / Close authority |

## 4. Authority Boundary Decision Proposal

### 4.1 Formal Authority Model

本次 Amendment SHOULD 採下列工具中立 Authority Model：

| 層級 / 事件 | Tool-neutral 定義 | ChatGPT 對應 | Authority |
|-------------|-------------------|--------------|-----------|
| Repository `main` | System of Record / SSOT | GitHub Repository `main` | **唯一目前工程 Fact Authority**；正式 Architecture、Standard、Code、EWO、Review / Closure evidence 依其治理載體生效 |
| Operational Workspace | Active Collaboration Workspace | ChatGPT Project | 協作與工作聚合空間；**非 Fact Authority** |
| Workspace Context | Operational Workspace 的受控 Context | Project Instructions / Approved Reference | 提供 scope、instructions、reference pointers；屬衍生 Context，MUST NOT 凌駕目前 `main` |
| Work Session | Ephemeral execution / reasoning session | Chat | 暫態工作單位；內容在 Promotion 前不具正式 Fact Authority |
| Session Lineage | Work Session 的分支 / 延伸關係 | Chat Branch | 只表示 conversation lineage；MUST NOT 等同 Git branch、PR 或正式決策線 |
| Knowledge Promotion Point | Working knowledge 提升為正式紀錄的 checkpoint | Merge + Closure | 事件 / Gate，不是獨立 Fact Authority；要求 surviving knowledge 進入 Repository 正式載體 |
| Archived Work Session | Historical Working Record | Archived Chat | 歷史工作紀錄；**非 Fact Authority**，不得覆蓋目前 `main` |

### 4.2 Formal Enterprise Workspace Boundary

`Operational Workspace` 不是 `AEOS-ARCH-010` 的新 Workspace Type。它是 Formal Enterprise Workspace 內或其協作表面上的 **tool-level operational construct**：

- 不具有 Formal Workspace ID / Architecture Reference / Lifecycle authority。
- 不得建立新 Platform、Capability、Repository 或 Architecture fact。
- 不得因工具支援 Project / Shared Project / Chat Branch 而擴張 AEOS Architecture Authority。
- 若未來 Operational Workspace 需要成為正式架構元素，必須另循 Architecture Change；本 EWO 不做此變更。

## 5. Proposed Work Session Lifecycle

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

- YEOS Development Workflow；
- Git branch / commit / PR / merge 規則；
- AEOS Review / Approval / Closure Gate；
- AI Agent command approval。

### 5.1 Stage Rules

| Stage | Minimum Rule |
|-------|--------------|
| Create | Session MUST 綁定明確 work intent / EWO / Issue / PR 或 investigation scope |
| Load `main` Baseline | MUST 取得目前 Repository `main` HEAD 與直接相關正式來源；不得以歷史 Chat 取代 |
| Execute | 只處理已授權 scope；Working Notes 屬暫態資料 |
| Validate | 結論 / 變更 MUST 依適用驗證規則檢查 |
| Review | 正式判斷依既有 Review Authority；Chat 內同意不得冒充正式 Review |
| PR | 變更透過受治理 PR 承載；Conversation Branch 不等同 PR Branch |
| Merge | 合併後 `main` 成為新的目前事實基線 |
| Closure | 產生 Closure Snapshot，確認 Decisions / Evidence / Review / Approval / Validation / Close record 已提升至 Repository |
| Archive | Session 轉為 Historical Working Record；新工作不得依其內容推定目前狀態 |

## 6. Active / Archive / Delete Rules

### 6.1 Active

Work Session SHOULD 僅在符合至少一項情況時維持 Active：

- 對應之 EWO / Issue / PR 正在執行或審查。
- 尚有未完成的 Blocker、Review finding、Approval gate 或 handoff。
- Session 包含仍需提升至 Repository 的重要工作結果，且 promotion 尚未完成。
- 明確被指定為目前 work unit 的 continuity session。

「可能日後有用」本身不足以讓 Session 永久 Active。

### 6.2 Archive

符合下列情況 SHOULD Archive：

- Merge + Closure 已完成，promotion check 通過。
- Handoff 已完成，舊 Session 不再承擔 Active ownership。
- Investigation / design exploration 具有歷史追溯價值，但其 surviving conclusions 已寫入 Repository。
- 工作被正式 supersede / cancelled，但保留歷史脈絡具稽核價值。

Archive 後的 Session 是 Historical Working Record，MUST NOT 作為新工作的 current-state authority。

### 6.3 Delete

只有同時符合下列條件時 MAY Delete：

- Session 主要是 troubleshooting、重複操作、誤建立、重複 Session、無治理價值的嘗試或已被完整取代的低價值工作紀錄。
- Promotion Check 證明不存在尚未寫入 Repository 的重要 Decision、Evidence、Review、Approval、Validation、Blocker、Contract 或 Closure fact。
- 沒有尚未完成的 handoff / ownership responsibility。
- 刪除不違反適用的安全、稽核、法遵或資料保存要求。

若無法證明上述條件，SHOULD Archive 而非 Delete。

## 7. Promotion-before-disposal

Work Session 在 Archive / Delete 前 MUST 執行 Promotion Check。以下資訊若對正式狀態仍有價值，MUST 先提升至 Repository 的適當載體：

| Working Knowledge | 正式載體例 |
|-------------------|------------|
| Architecture / boundary decision | Architecture / ADR / approved specification |
| Engineering requirement / acceptance | EWO / Spec / Issue / contract |
| Implementation result | Commit / PR / code / configuration |
| Validation evidence | CI / test result / review evidence / report |
| Review finding / decision | Formal Review / PR review / governed review artifact |
| Approval | Repository-governed approval record |
| Blocker / deferred work | EWO / Issue / Project State |
| Closure / handoff | EWO Close / Closure Snapshot / Project State / PR references |

Working conversation 本身不是上述正式載體的替代品。

## 8. Shared Project, Ownership and Handoff

### 8.1 Session Ownership

- 每個 Active Work Session MUST 有一個 accountable Session Owner；可由 Human 或受治理角色承擔，但最終 Repository accountability 仍依既有 Governance。
- 多個 Human / AI Agent MAY 協作，但不得出現「所有人都能改、沒有人負責 continuity」的 session。
- Agent identity、session identity、work item 與 Repository scope SHOULD 可追溯。

### 8.2 Handoff Contract

跨 Human、Agent 或新 Chat handoff 時，至少應交接：

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

Handoff SHOULD 使用 Repository references 與 Closure Snapshot，不應以完整 transcript 複製作為主要 continuity mechanism。

### 8.3 Chat Branch / Session Lineage

- Chat Branch 只表示 Work Session lineage / alternative exploration。
- 每個 branch session 在開始實質工作前仍 MUST 重新確認目前 `main` baseline；不得只繼承 parent chat 的舊事實。
- Session branch 的結論若要成為正式決策，必須依同一 Promotion / Review / Merge / Closure 路徑提升。

## 9. Human + AI Agent Session Hygiene

Project / Operational Workspace 應維持 **Minimal Active Working Context**，而不是永久保存全部執行歷史：

- Active set 只保留目前 work items、pending reviews / approvals、blocked work 與有效 handoff。
- 已 Closure 的 sessions SHOULD 轉 Archive；低價值且 promotion-safe 的 sessions MAY Delete。
- Agent automation SHOULD 為 session 記錄 work item、baseline commit、owner / actor、scope、branch / EWO / PR refs、validation 與 promotion status。
- Agent 不得因大量 session 可被保存，就把 Project 當成 long-term engineering memory；long-term engineering memory 仍在 Repository。
- 新 session 預設先重新取得 current `main`，再使用 Project Context / Snapshot 作為輔助。

## 10. AEOS / YEOS Authority Boundary

| 領域 | AEOS | YEOS |
|------|------|------|
| Formal Enterprise Workspace / Repository authority | **Authority** | 引用 / 採用 |
| Operational Workspace / Project / Work Session 的上位 Authority Boundary | **Authority** | 不重複定義 |
| Context loading / Project State / Closure Snapshot / archive-delete principles | **Authority：AEOS-STD-007** | 將規則轉成 Engineering operating controls |
| Development / Git / PR / Merge workflow | 不重新定義 | **Authority：ENG-STD-004 / 005** |
| Human + AI collaboration / traceability | 上位 architecture alignment | **Authority：GOV-POL-005** |
| AI Agent command approval | 不重新定義 | **Authority：ENG-STD-008** |
| Engineering session lifecycle compliance / handoff rules | 提供上位 boundary | **YEOS 新 Standard 承接** |

## 11. Minimum Necessary Change Plan

### Phase A — 本 PR：Review Package

1. 建立 EWO-AEOS-0047 Issue。
2. 建立本 Gap Analysis / Amendment Plan。
3. 不修改 Approved `AEOS-ARCH-010` / `AEOS-STD-007`，避免 Review 前直接改動正式 authority。
4. 提交 Draft PR 進 Architecture / Standard Review。

### Phase B — Owner / Architecture Review 通過後

1. Amend `AEOS-ARCH-010`：新增 Operational Workspace / Project / Work Session Authority Boundary 與 tool-neutral mapping；只做 boundary clarification，不新增 Formal Workspace Type。
2. Amend `AEOS-STD-007`：新增完整 Work Session Lifecycle、Active / Archive / Delete、Promotion-before-disposal、Shared Project ownership / handoff / context continuity、session hygiene。
3. 更新 Revision History、references 與必要 cross-reference。
4. Repository validation + Review + Approval。

### Phase C — YEOS Adoption

由 YEOS 依其 AIG-M7-T001 Future Standard Roadmap 建立 Engineering-level Session Lifecycle Standard，引用 AEOS 上位 boundary，不複製 AEOS 架構定義。

## 12. Validation Checklist for Amendment

| # | Validation | Pass Condition |
|---|------------|----------------|
| V-01 | No Architecture Redesign | Formal Enterprise Workspace identity / type / level / composition 不變 |
| V-02 | Repository Authority | `main` 明確為 current System of Record / SSOT |
| V-03 | Non-authoritative Chat | Project Context、Chat、Archive 均無權覆蓋 `main` |
| V-04 | Fresh Baseline | 新 session / chat branch 先取得目前 `main` |
| V-05 | Promotion | Archive / Delete 前有 promotion check |
| V-06 | Lifecycle | Create → Load `main` → Execute → Validate → Review → PR → Merge → Closure → Archive 完整 |
| V-07 | Shared Collaboration | Owner / handoff / lineage / context continuity 可稽核 |
| V-08 | Agent Scale | Project 維持 minimal active set，不成為永久 chat history database |
| V-09 | No Duplicate Standard | 不新增與 AEOS-STD-007 平行的 Context / Conversation Standard |
| V-10 | YEOS Separation | 不重寫 ENG-STD-004 / 005 / 008 權威範圍 |

## 13. Risks and Review Questions

### Architecture Review 必須確認

1. `Operational Workspace` 作為 tool-level construct 是否足以避免與 `AEOS-ARCH-010` Formal Workspace 混淆。
2. Authority model 是否應被視為 ARCH-010 的 boundary clarification，而非新的 Workspace Type / Architecture entity。
3. `Merge + Closure` 作為 Knowledge Promotion Point 是否只屬 lifecycle checkpoint，而不建立新的 authority layer。

### Standard Review 必須確認

1. Archive / Delete criteria 是否足以避免 unpromoted knowledge loss。
2. Shared Project handoff contract 是否維持 tool-neutral，不綁定 ChatGPT vendor capability。
3. YEOS adoption 是否可以只引用上位 authority，而把 execution compliance 留在 YEOS。

## 14. References

| 文件 | 用途 |
|------|------|
| AEOS-ARCH-001 — Architecture Baseline | Approved architecture entry / authority |
| AEOS-ARCH-008 — Repository Architecture | Repository governance and delivery boundary |
| AEOS-ARCH-010 — Workspace Architecture | Formal Enterprise Workspace identity / boundary / lifecycle |
| AEOS-ARCH-013 — Enterprise AI Agent Architecture | Human + AI Agent architecture alignment |
| AEOS-STD-005 — Review Standard | Review / Approval / Merge / Close authority |
| AEOS-STD-007 — AI Engineering Context and Token Budget Standard | Repository as Memory、Context、Project State、Closure Snapshot、Conversation Lifecycle |
| EWO-AEOS-0047 / Issue #61 | 本次工作來源 |

## 15. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 0.1.0 | 2026-08-28 | 初版：完成 `main` 基線盤點、Gap Analysis、Authority Boundary、Minimum Necessary Change 與 Amendment Execution Plan；尚未修改 Approved Architecture / Standard | ChatGPT |
