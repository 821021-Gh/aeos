# AEOS AI Engineering 操作入口

本檔案是 Agent 的薄型操作入口，不取代 AEOS Constitution、Governance、DIA 或 Standards；發生衝突時，依既有 Governance Hierarchy 處理。

## 工作啟動

1. 先讀取 [`PROJECT_STATE.md`](PROJECT_STATE.md)。
2. 再讀取目前 EWO／Issue、直接相關的正式規格，以及必要的程式碼與測試。
3. 僅在發現衝突、缺少決策依據或驗證失敗時，擴大讀取 Architecture、ADR、Governance 或其他歷史資料。
4. 不得預設載入整個 Repository 或完整對話歷史。

Context 與 Token Budget 的完整規則見 [`AEOS-STD-007`](docs/standards/AEOS-STD-007-AI-Engineering-Context-and-Token-Budget-Standard.md)。

## 交付輸出

正常情況使用 Delta Output，只回報：

- Files Changed
- Validation
- Risk
- Next Action

只有在 Blocker、驗證失敗、重大架構／安全決策或使用者要求時，才展開完整分析。

## 文件語言

Markdown、說明文字與其他敘述性文件原則上使用繁體中文；必要英文技術名詞、程式碼、API 名稱、檔名、命令、識別碼與外部標準原名保留英文。完整規則見 [`AEOS-STD-001 §6.2`](docs/standards/AEOS-STD-001-Documentation-Format-Standard.md#62-language-and-narrative-documentation)。
