---
doc-id: AEOS-ARCH-006
doc-name: Layer Architecture
doc-type: Architecture
repository: AEOS
version: 1.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-Architecture-0003
  - EWO-AEOS-0013
  - AR-AEOS-0013-R1
  - AEOS-ADR-002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-002
  - AEOS-ARCH-004
---

# AEOS-ARCH-006 — Layer Architecture

## Executive Summary

本文件依 AEOS-ADR-002 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Layer Architecture，定義企業架構責任的分層模型、每一層的責任與禁止承載內容、相鄰層與跨層依賴的允許方向，以及防止下位層繞過上位治理或反向控制上位架構的規則。Layer Architecture 是全部架構領域的共同分層基準，使治理、平台、能力、Repository 與實作責任保持清楚邊界，並為 Repository Architecture 與 Dependency Architecture 提供統一的分層與依賴方向依據。本文件不重新設計 Approved 架構載體、不建立實作設計，也不取代任何專項架構或治理文件。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-006 |
| 文件名稱 | Layer Architecture |
| 型別 | Architecture（Layer Architecture） |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-Architecture-0003、EWO-AEOS-0013、AR-AEOS-0013-R1、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001（Approved v1.0.0）、AEOS-ARCH-002（Approved v1.0.0）、AEOS-ARCH-004（Approved v1.0.0） |
| 關聯文件 | EWO-AEOS-0013、AR-AEOS-0013-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-STD-001～AEOS-STD-005、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件之目的為：

- 將 Workspace Architecture 與 Enterprise Meta Architecture 轉化為 AEOS 內可治理、可追溯的 Layer Architecture。
- 定義企業架構責任的分層模型，使治理、平台、能力、Repository 與實作責任保持清楚邊界（AEOS-ARCH-004 §6.2）。
- 定義每一架構層的責任與禁止承載之內容（AEOS-ARCH-004 §6.2）。
- 定義相鄰層與跨層依賴的允許方向（AEOS-ARCH-004 §6.2）。
- 防止下位層繞過上位治理或反向控制上位架構（AEOS-ARCH-004 §6.2）。
- 為 Repository Architecture 與 Dependency Architecture 提供共同分層基準（AEOS-ARCH-004 §6.2）。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Layer 之正式定義、架構身分與層級結構。
- 各層之 Responsibility、Boundary 與 Forbidden Content。
- 相鄰層與跨層依賴之允許、受限與禁止方向。
- Layer 與 Governance、Platform、Capability、Repository、Dependency、Workspace 之關係與治理意義。
- Layer 之擁有權、變更控制與合規要求。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改 Approved 架構載體之 Workspace Architecture 或 Enterprise Meta Architecture。
- 個別 Platform、Capability、Repository 或 Workspace 的內部設計。
- Governance Layers（AEOS-ARCH-002 §3）與文件權威階層（AEOS-ARCH-004 §3）之重述或取代。
- Catalog、Matrix 之實際條目。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。

## 3. Architecture Authority

Layer Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| L0 | AEOS-ARCH-001 | 最高架構權威 |
| L1 | AEOS-ARCH-001 | 架構 Entry Document 與 Architecture Register |
| L2 | AEOS-ARCH-002 | 定義 Governance Layers 與治理階層 |
| L3 | AEOS-ARCH-004 | 定義 Layer Architecture 在 Enterprise Architecture 中的定位與 MUST |
| L4 | AEOS-ARCH-006（本文件） | 定義 Layer 模型、責任邊界與依賴方向規則 |
| L5 | 專項 Architecture、Catalog／Matrix、Repository Architecture | 依本文件之分層基準落實 |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的 Layer 或依賴方向。
- Layer Architecture 與 Governance Layers（AEOS-ARCH-002 §3）互補：前者定義架構責任分層，後者定義治理權威分層；兩者不得互相取代。
- 發現 Approved 架構載體未涵蓋的 Layer 需求時，MUST 先透過正式架構變更處理。

## 4. Layer Model

### 4.1 Formal Definition

Layer 是企業架構中具備明確責任、邊界與依賴方向的穩定架構分層。每一 Layer 定義一組架構責任與禁止承載之內容，並以允許之依賴方向與其他 Layer 連結。Layer 不是組織單位、不是技術堆疊層級、也不是部署邊界。

一個架構分層只有在同時符合下列條件時，才可被認定為正式 Layer：

- 具有明確且跨版本延續的 Layer 名稱與責任定義。
- 具有明確的 Responsibility 與 Forbidden Content。
- 具有定義清楚的允許依賴方向（上位約束、下位依賴）。
- 其規則可被 Platform、Capability、Repository、Dependency 與 Workspace 架構追溯與遵循。
- 變更受 Enterprise Architecture 管理，而非隨單一實作任意增刪。

### 4.2 Layer Structure

企業架構責任分為六個正式層級：

| 層級 | 名稱 | 核心責任 | 禁止承載 |
|------|------|----------|----------|
| L1 | Governance | 定義治理原則、規則與核准路徑；承載 Policy、Standard、Review 與決策紀錄 | 不得定義實作細節或未經核准之架構事實 |
| L2 | Enterprise Architecture | 定義 Workspace 層級之架構結構、領域邊界與 Register | 不得重新設計 Approved 架構載體、不得建立平行架構來源 |
| L3 | Platform | 定義 Platform 身分、邊界、分類、關係與生命週期 | 不得將 Repository、Capability、Product 或 Service 等同 Platform |
| L4 | Capability | 定義能力之責任、組合、關係與擁有權 | 不得以特定實作或 Repository 取代能力定義 |
| L5 | Repository | 定義 Repository 身分、類型、責任與映射 | 不得繞過上位治理或擴張架構 Authority |
| L6 | Implementation | 落實實作、工具與部署（屬 Production Repositories 責任） | 不得反向控制上位層或建立未經核准之架構事實 |

規則：

- 每一 Layer MUST 具有明確責任與禁止承載內容，且 MUST 可追溯至 AEOS-ARCH-001／Approved 架構載體已核准內容。
- L1～L5 之權威內容由 AEOS 正式文件承載；L6 之實作責任保留於 Production Repositories（AEOS-ARCH-004 §3.3.2）。
- Layer 之禁止承載內容 MUST NOT 以任何形式落入下位文件或實作。

### 4.3 Layer Is Not

| 架構元素 | 與 Layer 的區別 |
|----------|----------------|
| Governance Layer（AEOS-ARCH-002 §3） | 治理層級定義治理權威之分層；本 Layer Model 定義架構責任分層；兩者互補 |
| Architecture Authority（AEOS-ARCH-004 §3） | 權威順序定義文件階層（A0～A5）；本 Layer Model 定義責任與依賴方向 |
| Platform | Platform 是承載 Capability 之穩定邊界（L3）；Layer 是責任分層 |
| Repository | Repository 是版本化治理與交付邊界（L5）；Layer 是責任分層 |
| 技術堆疊（OS／Middleware／Application） | 技術分層描述實作部署（L6 範圍）；本 Layer Model 描述企業架構責任 |

## 5. Layer Rules

### 5.1 Responsibility Rules

- 每一 Layer 之責任 MUST 有明確邊界，且 MUST 對應至 AEOS-ARCH-004 §6 之架構領域或 AEOS-ARCH-002 §3 之治理層級。
- Layer MUST NOT 承載其 Forbidden Content（§4.2）。
- 下位 Layer 之內容 MUST 可追溯至上位 Layer；上位 Layer 之規則不得被下位 Layer 重述、改寫或取代。
- 任一 Layer 之責任不明確時，MUST 於登錄或交付前解決，不得以模糊敘述取代架構定義。

### 5.2 Dependency Direction Rules

依賴方向以「上位約束、下位依賴」為原則：

| 依賴方向 | 允許性 | 說明 |
|----------|--------|------|
| 上位 Layer → 下位 Layer（約束／定義） | 允許 | 上位定義規則與邊界，下位遵循 |
| 下位 Layer → 上位 Layer（引用／追溯） | 允許（僅追溯） | 下位引用上位權威內容，MUST NOT 改寫 |
| 下位 Layer → 上位 Layer（控制／覆寫） | 禁止 | 下位不得反向控制或覆寫上位架構 |
| 同層元素之間 | 受限 | 同層元素依其領域關係（AEOS-ARCH-004 §7）互動，MUST 有明確方向、類型與依據 |
| 跨層跳級依賴 | 受限 | 跳級依賴 MUST 有上位文件依據，否則視為繞過治理 |

### 5.3 Bypass and Reverse-Control Prevention

- 下位 Layer MUST NOT 繞過上位治理（例如直接修改 Register、Catalog 或已核准架構事實以取代 Architecture Review）。
- 下位 Layer MUST NOT 反向控制上位 Layer（例如以實作決策覆寫架構邊界、以 Repository 政策取代 Enterprise Architecture）。
- 循環治理關係 MUST NOT 被允許；循環技術或服務依賴必須由 Dependency Architecture 明確評估。
- 發現繞過或反向控制情事時，MUST 依 AEOS-CON-001 變更管理與 AEOS-STD-005 Review 流程處理。

## 6. Cross-Layer Relationships

### 6.1 Relationship Types

| 關係 | 語意 | 要求 |
|------|------|------|
| Constrains | 上位 Layer 定義下位 Layer 之允許行為邊界 | MUST 具有上位 Architecture 或 Policy 依據 |
| Defines | 上位 Layer 建立下位 Layer 之模型、結構與規則 | MUST 對應正式 Architecture 資產 |
| Carries | Platform 承載 Capability（L3 → L4） | MUST 對應正式 Capability 與 Interface |
| Implements | Repository／實作落實上位規則（L5／L6） | MUST 符合上位邊界，不擴張 Authority |
| Traces To | 下位 Layer 引用上位 Layer 之權威內容 | MUST 保持引用有效且不重述 |
| Governs | 一 Layer 對另一 Layer 之特定治理面向具有正式權威 | MUST 限定治理範圍，不得推定全面控制 |

### 6.2 Relationship Rules

- 每一 Layer Relationship MUST 有方向、類型、依據與生命週期狀態。
- `Traces To` 不構成控制權；引用上位內容不授予下位 Layer 修改權。
- `Implements` 不轉移架構 Authority；實作層 MUST 保留對上位架構的遵循與可追溯性。
- 跨 Layer 之實際依賴 MUST 由 Dependency Architecture 定義並登錄於 Dependency Matrix。
- Layer Architecture 約束全部架構領域（AEOS-ARCH-004 §7）：Platform、Capability、Repository、Dependency 與 Workspace 架構 MUST 宣告其 Layer 歸屬與依賴方向。

## 7. Ownership and Governance

### 7.1 Roles

| 角色 | 責任 |
|------|------|
| Architecture Owner | 維護 Layer 模型一致性、裁決層級衝突及核准 Layer 變更 |
| Review Owner | 依 AEOS-STD-005 確認 Layer Architecture 變更已完成正式 Review |
| 各 Layer 權威文件 Owner | 維護該 Layer 之責任定義、邊界與禁止內容 |

### 7.2 Accountability Rules

- 每一 Layer MUST 有且只有一個 accountable Owner（以其權威文件之 owner 為準）。
- Layer 之新增、合併、拆分或移除視為 Enterprise Architecture 重大變更。
- 重大變更 MUST 依 AEOS-ARCH-003 建立或引用 ADR（如適用），並同步更新受影響之 Architecture、Catalog 與 Matrix。

## 8. Change and Evolution

下列變更屬於 Architecture Change，MUST 經 EWO 與 Architecture Review：

- 新增、合併、拆分或移除 Layer。
- 變更 Layer 之核心責任或 Forbidden Content。
- 變更允許之依賴方向或引入跳級依賴。
- 改變 Layer 與治理層級、架構權威之對應關係。

每項變更 MUST：

1. 說明原因、預期結果與不變範圍。
2. 識別受影響之 Platform、Capability、Repository、Dependency、Workspace 與治理文件。
3. 評估繞過治理、循環依賴與責任重疊之風險。
4. 定義 Migration、Rollback、版本與生命週期策略。
5. 更新本文件、Architecture Register 與相關 Matrix。

## 9. Compliance

Layer Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Layer 可追溯至 AEOS-ARCH-001 與 Approved 架構載體 |
| Responsibility | 每層之核心責任與禁止承載內容明確，且與上位文件一致 |
| Dependency Direction | 依賴方向符合「上位約束、下位依賴」原則，無反向控制 |
| Bypass Prevention | 無繞過上位治理之變更路徑 |
| Coverage | Platform、Capability、Repository、Dependency、Workspace 均宣告 Layer 歸屬 |
| Ownership | 每一 Layer 具有單一 accountable Owner |
| Asset Consistency | Architecture、Register、Catalog 與 Matrix 狀態一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 10. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved v1.0.0） | Architecture | 架構基線與 Architecture Register |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved v1.0.0） | Architecture | Governance Layers 與治理階層 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved v1.0.0） | Architecture | 重大架構決策紀錄機制 |
| REF-005 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved v1.0.0） | Architecture | Layer Architecture 的上位定位與 MUST |
| REF-006 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved v1.0.0） | Architecture | Platform 領域（L3）之責任與關係 |
| REF-007 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved v1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-008 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-009 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-010 | EWO-AEOS-Architecture-0003 | EWO | 本文件之工作來源 |
| REF-011 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 11. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 2（AEOS-ADR-002 已核准）：執行 Architecture Transition——WA-001 分類為歷史來源（Historical Reference）；Authority 階層（L0）與對應來源重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併；狀態更新為 Approved，成為 AEOS Layer Architecture 正式定義（EWO-AEOS-0013；AR-AEOS-0013-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Layer Model、Layer Rules、Cross-Layer Relationships、Ownership、Change 與 Compliance（EWO-AEOS-Architecture-0003） | Codex |
