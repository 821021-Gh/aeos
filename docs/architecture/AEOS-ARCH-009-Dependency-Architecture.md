---
doc-id: AEOS-ARCH-009
doc-name: Dependency Architecture
doc-type: Architecture
repository: AEOS
version: 1.1.0
status: Approved
owner: Architecture Owner
created: 2026-08-06
updated: 2026-08-08
related:
  - EWO-AEOS-0018
  - EWO-AEOS-0019
  - AR-AEOS-0019-R1
  - AEOS-ADR-002
  - WA-001
  - AEOS-ARCH-001
  - AEOS-ARCH-004
  - AEOS-ARCH-005
  - AEOS-ARCH-006
  - AEOS-ARCH-007
  - AEOS-ARCH-008
---

# AEOS-ARCH-009 — Dependency Architecture

## Executive Summary

本文件依 AEOS-ADR-002 與 AEOS-ARCH-004 建立 AI Engineering Workspace 的正式 Dependency Architecture，定義 Dependency 的識別、類型、方向、邊界、來源與目標，涵蓋 Platform、Layer、Capability、Repository 與 Implementation 之間的依賴關係，並定義依賴強度、Ownership、Lifecycle、變更影響、循環依賴與違規治理規則。Dependency 是正式架構關係而非僅為技術套件連結；依賴方向以「上位約束、下位依賴」為原則，禁止未核准的反向控制與跨層繞過。本文件不重新設計既定 Architecture、不建立具名 Dependency Matrix 或實際依賴清單、不建立實作設計，也不開始 Workspace Architecture。

## 文件資訊

| 項目 | 內容 |
|------|------|
| 文件代號 | AEOS-ARCH-009 |
| 文件名稱 | Dependency Architecture |
| 型別 | Architecture（Dependency Architecture） |
| 狀態 | Approved |
| 版本 | 1.1.0 |
| Repository | AEOS |
| 擁有者 | Architecture Owner |
| 建立日期 | 2026-08-06 |
| 最後更新 | 2026-08-08 |
| 依據文件 | EWO-AEOS-0018、EWO-AEOS-0019、AR-AEOS-0019-R1、AEOS-ADR-002（WA-001 Fact Authority Transition）、AEOS-ARCH-001（Approved 1.3.0）、AEOS-ARCH-004（Approved 1.1.0）、AEOS-ARCH-005（Approved 1.1.0）、AEOS-ARCH-006（Approved 1.1.0）、AEOS-ARCH-007（Approved 1.1.0）、AEOS-ARCH-008（Approved 1.1.0） |
| 關聯文件 | EWO-AEOS-0019、AR-AEOS-0019-R1、AEOS-ARCH-001、AEOS-ARCH-002、AEOS-ARCH-003、AEOS-ARCH-004、AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007、AEOS-ARCH-008、AEOS-STD-001～AEOS-STD-005、AEOS-ADR-002、WA-001（歷史來源） |

## 1. Purpose

本文件之目的為：

- 將依賴視為正式架構關係，而非僅為技術套件連結（AEOS-ARCH-004 §6.5）。
- 識別 Architecture、Governance、Capability、Repository、Data、Service 與 Delivery Dependency（AEOS-ARCH-004 §6.5）。
- 定義允許、受限與禁止之依賴方向（AEOS-ARCH-004 §6.5）。
- 定義依賴之識別、類型、方向、邊界、來源與目標，涵蓋 Platform、Layer、Capability、Repository 與 Implementation 之依賴關係。
- 定義依賴強度、Ownership、Lifecycle、變更影響、循環依賴與違規治理規則。
- 維持「上位約束、下位依賴」原則，禁止未核准的反向控制與跨層繞過（AEOS-ARCH-006 §5.2、§5.3）。
- 為 Dependency Matrix 提供正式架構依據，支援影響分析與演進決策（AEOS-ARCH-004 §6.5、§7）。

## 2. Scope

### 2.1 In Scope

本文件涵蓋：

- Dependency 之正式定義、架構身分與必要屬性。
- Dependency 類型、方向、邊界、來源與目標。
- Platform、Layer、Capability、Repository 與 Implementation 之間的依賴關係。
- 依賴強度、Ownership、Lifecycle 與變更影響。
- 循環依賴與違規治理規則。
- Dependency Matrix 之定位（不建立條目）。

### 2.2 Out of Scope

本文件不涵蓋：

- 重新定義或修改既定 Architecture（AEOS-ARCH-001、AEOS-ARCH-004～AEOS-ARCH-008）。
- 具名 Dependency Matrix 或實際依賴清單之建立（Matrix 應作為後續獨立架構資產）。
- Workspace Architecture 之建立（屬後續獨立 EWO）。
- 個別 Repository、Platform、Capability 之內部技術依賴或實作設計。
- Runtime Topology、Deployment Architecture、Infrastructure Design 或 Source Code Implementation。

## 3. Architecture Authority

Dependency Architecture 適用下列權威順序：

| 層級 | 資產 | 權威角色 |
|------|------|----------|
| D0 | AEOS-ARCH-001 | 最高架構權威 |
| D1 | AEOS-ARCH-001 | 架構 Entry Document 與 Architecture Register |
| D2 | AEOS-ARCH-004 | 定義 Dependency Architecture 在 Enterprise Architecture 中的定位與 MUST |
| D3 | AEOS-ARCH-005、AEOS-ARCH-006、AEOS-ARCH-007、AEOS-ARCH-008 | 定義 Platform、Layer、Capability、Repository 之邊界與依賴規則 |
| D4 | AEOS-ARCH-009（本文件） | 定義 Dependency 模型、方向、強度與治理規則 |
| D5 | Dependency Matrix、Repository 層級文件 | 登錄已核准依賴事實並落實 |

規則：

- 下位資產 MUST 符合上位資產，且 MUST NOT 隱性建立新的依賴事實或改寫既定架構。
- Dependency 必須明確、可驗證且方向合規（AEOS-ARCH-004 §7）；不得只存在於非正式敘述或實作中。
- 發現 Approved 架構載體未涵蓋的 Dependency 需求時，MUST 先透過正式架構變更處理。

## 4. Dependency Definition

### 4.1 Formal Definition

Dependency 是架構元素之間具備方向、類型、強度與治理邊界的正式依賴關係。Dependency 描述一個架構元素（Source）對另一個架構元素（Target）的正式依賴，涵蓋 Platform、Layer、Capability、Repository、Implementation 等架構元素。

一個依賴關係只有在同時符合下列條件時，才可被認定為正式 Dependency：

- 具有可被唯一識別且跨版本延續的 Dependency ID。
- 具有明確的 Source 與 Target。
- 具有明確的類型（§6）與方向（§7）。
- 具有明確的強度（§9）與 Owner（§10）。
- 可追溯至既有 Architecture 或經核准之架構決策。
- 生命週期由 Enterprise Architecture 管理，而非隨單一實作任意建立或移除。

### 4.2 Dependency Is Not

| 架構元素 | 與 Dependency 的區別 |
|----------|----------------------|
| 技術套件連結 | 技術套件連結是實作細節；Dependency 是正式架構關係 |
| 資料流／訊息流 | 資料或訊息流是執行時期行為；Dependency 是架構依賴關係 |
| Capability Relationship | Capability 關係描述能力組合；Dependency 描述依賴方向與強度 |
| Repository 引用 | Repository 引用是文件層級關聯；Dependency 是具治理邊界的正式依賴 |
| Implementation 耦合 | 實作耦合屬 L6 範圍；Dependency 跨越架構元素並受治理 |

## 5. Dependency Identity Model

每個 Dependency MUST 具備下列權威屬性：

| 屬性 | 必要性 | 定義 |
|------|--------|------|
| Dependency ID | MUST | 全 Workspace 唯一且不可重複使用的穩定識別碼 |
| Dependency Name | MUST | 正式名稱；命名變更不得改變 Dependency ID |
| Type | MUST | 依 §6 指定之 Dependency 類型 |
| Direction | MUST | 依 §7 指定之依賴方向（Source → Target） |
| Source | MUST | 依賴來源之架構元素引用 |
| Target | MUST | 依賴目標之架構元素引用 |
| Strength | MUST | 依 §9 指定之依賴強度 |
| Boundary | MUST | 依賴包含與排除的範圍 |
| Owner | MUST | 對依賴必要性、風險與相容性負責的角色 |
| Lifecycle Status | MUST | Candidate、Active、Deprecated 或 Retired |
| Architecture Reference | MUST | 核准此依賴之 Architecture／ADR |

Dependency ID、Direction、Type 或 Strength 的實質變更 MUST 經 Architecture Review；不得只修改 Matrix 條目完成架構變更。

## 6. Dependency Types

Dependency 依其治理意義分為下列類型（AEOS-ARCH-004 §6.5）：

| 類型 | 定義 | 治理意義 |
|------|------|----------|
| Architecture Dependency | 架構資產或架構決策之間的依賴 | 影響架構一致性與變更影響範圍 |
| Governance Dependency | 治理文件、權威或核准路徑之間的依賴 | 影響治理階層與核准順序 |
| Capability Dependency | Capability 之間的依賴 | 影響能力組合、結果與演進 |
| Repository Dependency | Repository 之間的依賴 | 影響交付、版本與變更協調 |
| Data Dependency | 架構元素之間的資料依賴 | 影響資料擁有權與一致邊界 |
| Service Dependency | 服務或介面提供與消費之依賴 | 影響 Interface 契約與版本相容性 |
| Delivery Dependency | 交付或釋出流程之間的依賴 | 影響釋出順序與相依交付 |

規則：

- 每個 Dependency MUST 指定一個 Primary Type。
- 類型判定 MUST 以架構關係之治理意義為依據，MUST NOT 單憑技術實作決定。
- 一項依賴同時具備多種治理意義時，MUST 以 Primary Type 為主並記錄次要特性，不得以多重類型模糊治理邊界。

## 7. Dependency Direction and Boundary

### 7.1 Direction Principle

依賴方向以「上位約束、下位依賴」為原則（AEOS-ARCH-006 §5.2）：

- 上位 Layer 約束並定義下位 Layer 之允許行為；下位 Layer 依賴並遵循上位 Layer。
- 下位 Layer 對上位 Layer 之引用僅限追溯，MUST NOT 改寫或覆寫上位內容。
- 下位 Layer MUST NOT 反向控制上位 Layer，亦不得跨層繞過上位治理（AEOS-ARCH-006 §5.3）。

### 7.2 Direction Rules

| 依賴方向 | 允許性 | 說明 |
|----------|--------|------|
| 上位 Layer → 下位 Layer（約束／定義） | 允許 | 上位定義規則與邊界，下位遵循 |
| 下位 Layer → 上位 Layer（引用／追溯） | 允許（僅追溯） | 下位引用上位權威內容，MUST NOT 改寫 |
| 下位 Layer → 上位 Layer（控制／覆寫） | 禁止 | 未核准之反向控制與覆寫 |
| 同層架構元素之間 | 受限 | 依其領域關係（AEOS-ARCH-004 §7）互動，MUST 有明確方向、類型與依據 |
| 跨層跳級依賴 | 受限 | 跳級依賴 MUST 有上位文件依據，否則視為繞過治理 |

### 7.3 Boundary Rules

- 每項 Dependency MUST 明確宣告 Source、Target 與 Boundary，不得以模糊敘述取代架構定義。
- Dependency MUST NOT 擴張 Source 或 Target 之既有 Authority。
- 依賴目標超出 Source 之治理邊界時，MUST 先完成 Boundary 決議，不得隱性建立新依賴。

## 8. Dependency Across Architecture Domains

Dependency Architecture 涵蓋 Platform、Layer、Capability、Repository 與 Implementation 之間的依賴關係：

| 來源 | 依賴 | 目標 | 允許方向 |
|------|------|------|----------|
| Layer Architecture | 約束 | Platform、Capability、Repository、Implementation | 上位約束下位 |
| Platform Architecture | 承載 | Capability | Platform → Capability |
| Capability Architecture | 分配責任至 | Ownership Matrix | Capability → Ownership |
| Repository Architecture | 實現／治理 | Platform 與 Capability | Repository → Platform／Capability |
| Implementation | 落實 | Repository | Implementation → Repository |
| Dependency Architecture | 連結並約束 | Platform、Layer、Capability、Repository | 依 §7 方向規則 |

規則：

- 任一架構元素之依賴 MUST 可追溯至其 Layer 歸屬（AEOS-ARCH-006 §6.2）。
- 跨域依賴 MUST 符合 §7.2 方向規則；未核准之反向控制與跨層繞過一律禁止。
- 跨域依賴之變更 MUST 依 §12 執行變更影響分析。

## 9. Dependency Strength

### 9.1 Strength Definition

依賴強度描述依賴對 Source 或 Target 之治理與運作影響程度，用於風險分級與變更影響評估。

| 強度 | 定義 | 治理意義 |
|------|------|----------|
| Mandatory | Target 為 Source 正常運作或交付之必要條件 | 變更 MUST 進行完整影響分析並協調雙方 Lifecycle |
| Controlled | Target 為 Source 之受控依賴，可由替代方案替換 | 變更 MUST 評估相容性與替代路徑 |
| Optional | Target 為 Source 之可選依賴 | 變更影響有限，仍需登錄與追溯 |

### 9.2 Strength Rules

- 每個 Dependency MUST 指定單一 Strength。
- Strength 判定 MUST 以治理與運作影響為依據，MUST NOT 單憑技術耦合強度決定。
- Strength 變更視為 Architecture Change（§12），MUST 經 EWO 與 Architecture Review。

## 10. Dependency Ownership

### 10.1 Roles

| 角色 | 責任 |
|------|------|
| Architecture Owner | 維護 Dependency 一致性、裁決方向與邊界衝突及核准重大變更 |
| Dependency Owner | 對依賴之必要性、風險與相容性負最終責任 |
| Source Owner | 維護依賴來源元素之邊界與依賴正確性 |
| Target Owner | 維護依賴目標元素之邊界與相容性 |
| Review Owner | 依 AEOS-STD-005 確認 Dependency 變更已完成正式 Review |

### 10.2 Accountability Rules

- 每個 Active Dependency MUST 有且只有一個 accountable Dependency Owner 角色。
- Dependency Owner 可委派執行工作，但 MUST NOT 委派最終 Accountability。
- Source Owner 與 Target Owner 之責任 MUST 分別記錄；跨域依賴之協調責任歸 Dependency Owner。
- Ownership 缺失或責任重疊時，Dependency MUST NOT 進入 Active 狀態。

## 11. Dependency Lifecycle

| 狀態 | 定義 | 必要條件 |
|------|------|----------|
| Candidate | 已提出但尚未成為正式 Dependency | 具 Source、Target、Type、Direction、提案 Owner 與 Architecture Reference |
| Active | 已核准並承擔正式架構關係 | 完整 Identity、Boundary、Strength、Owner 與相容性記錄 |
| Deprecated | 仍受支援但不得承接新的依賴負載 | 具替代方向、Migration Plan、期限與受影響項目 |
| Retired | 已停止承擔正式依賴關係 | 相關依賴、介面與生命週期已移轉或終止 |

允許的主要狀態轉移為：

- Candidate → Active：完成 Architecture Review 並登錄 Dependency Matrix。
- Candidate → Retired：提案撤回或核准不成立，保留決策紀錄。
- Active → Deprecated：核准取代或合併策略並建立 Migration Plan。
- Deprecated → Active：取代決策撤銷且重新完成 Architecture Review。
- Deprecated → Retired：Migration 完成且無未處理依賴。

任何跳過 Deprecated 的 Active → Retired 轉移 MUST 具有緊急理由、影響分析與 Architecture Owner 核准。

## 12. Change Impact and Circular Dependency

### 12.1 Change Impact Analysis

下列變更 MUST 執行依賴影響分析並經 EWO 與 Architecture Review：

- 建立、移除或變更 Dependency 之 Type、Direction、Strength 或 Boundary。
- 變更 Source 或 Target 之架構身分、邊界或 Authority。
- 引入 Breaking Dependency 或改變依賴方向。

影響分析 MUST：

1. 識別直接與間接受影響之 Platform、Layer、Capability、Repository 與 Implementation。
2. 評估依賴強度、相容性、替代路徑與回滾策略。
3. 更新 Dependency Matrix 與相關 Architecture、Catalog。
4. 依重大程度建立或引用 ADR。

### 12.2 Circular Dependency

- 循環治理關係 MUST NOT 被允許（AEOS-ARCH-006 §5.3、AEOS-ARCH-007 §8.2、AEOS-ARCH-008 §8.2）。
- 循環技術或服務依賴 MUST 由本文件明確評估；未經核准不得建立。
- 發現循環依賴時，MUST 先完成方向或邊界調整（或取得 Architecture Owner 核准），不得以 Matrix 註記取代架構修正。

## 13. Violation Governance

下列行為屬於依賴違規，MUST 依 AEOS-CON-001 變更管理與 AEOS-STD-005 Review 流程處理：

- 未核准之反向控制：下位元素控制或覆寫上位架構。
- 跨層繞過：跳級依賴缺乏上位文件依據，繞過上位治理。
- 未登錄依賴：正式架構關係未登錄於 Dependency Matrix 即被實作使用。
- 未核准循環依賴：循環依賴未經 Dependency Architecture 評估與核准。
- 未經核准之強度或方向變更。

違規處理 MUST：

1. 記錄違規事實、影響與受影響元素。
2. 於下一個 EWO 或 RC 完成修正或核准。
3. 更新 Dependency Matrix 與相關架構資產。
4. 未完成修正前，MUST NOT 以違規依賴作為正式架構依據。

## 14. Compliance

Dependency Architecture 合規檢查至少包含：

| 檢查領域 | 合規要求 |
|----------|----------|
| Authority | Dependency 可追溯至 AEOS-ARCH-001 與 Approved 架構載體 |
| Identity | 具唯一、穩定且不可重用的 Dependency ID |
| Type | 具有單一 Primary Type 且判定依據成立 |
| Direction | 符合「上位約束、下位依賴」原則，無未核准反向控制或跨層繞過 |
| Source／Target | Source 與 Target 明確且可追溯 |
| Strength | 具有單一 Strength 且判定依據成立 |
| Ownership | 具有唯一 accountable Dependency Owner |
| Lifecycle | 狀態、轉移條件、Migration 與替代關係完整 |
| Change Impact | 重大變更完成影響分析並更新 Matrix |
| Circular Dependency | 無未核准循環依賴 |
| Matrix Readiness | 未經核准不建立具名依賴條目；Matrix 待本文件核准後另立 |
| Asset Consistency | Architecture、Register、Catalog 與 Matrix 狀態一致 |
| Review Evidence | 變更具備 EWO、Review Decision、Revision History 與 Merge 證據 |

不符合本文件之專項 Architecture、Catalog、Matrix 或 Repository Architecture MUST NOT 被視為 AEOS 正式架構資產。

## 15. References

| ID | 文件 | 型別 | 用途 |
|----|------|------|------|
| REF-001 | WA-001 — AI Engineering Workspace Architecture（Approved v1.0.0，外部） | Historical Reference（External） | 歷史來源；不作為正式 Fact Authority（AEOS-ADR-002 §2.1） |
| REF-002 | [AEOS-ARCH-001 — Architecture Baseline](AEOS-ARCH-001-Architecture-Baseline.md)（Approved 1.3.0） | Architecture | 架構基線與 Architecture Register |
| REF-003 | [AEOS-ARCH-002 — Enterprise Governance Architecture](AEOS-ARCH-002-Enterprise-Governance-Architecture.md)（Approved 1.1.0） | Architecture | Governance 階層與權威順序 |
| REF-004 | [AEOS-ARCH-003 — Architecture Decision Record System](AEOS-ARCH-003-Architecture-Decision-Record-System.md)（Approved 1.1.0） | Architecture | 重大架構決策紀錄機制 |
| REF-005 | [AEOS-ARCH-004 — AI Enterprise Architecture Overview](AEOS-ARCH-004-AI-Enterprise-Architecture-Overview.md)（Approved 1.1.0） | Architecture | Dependency Architecture 的上位定位與 MUST |
| REF-006 | [AEOS-ARCH-005 — Platform Architecture](AEOS-ARCH-005-Platform-Architecture.md)（Approved 1.1.0） | Architecture | Platform 依賴與 Interface 規則 |
| REF-007 | [AEOS-ARCH-006 — Layer Architecture](AEOS-ARCH-006-Layer-Architecture.md)（Approved 1.1.0） | Architecture | 「上位約束、下位依賴」與依賴方向規則 |
| REF-008 | [AEOS-ARCH-007 — Capability Architecture](AEOS-ARCH-007-Capability-Architecture.md)（Approved 1.1.0） | Architecture | Capability 依賴與登錄規則 |
| REF-009 | [AEOS-ARCH-008 — Repository Architecture](AEOS-ARCH-008-Repository-Architecture.md)（Approved 1.1.0） | Architecture | Repository 依賴與登錄規則 |
| REF-010 | [AEOS-CON-001 — Repository Constitution](../constitution/AEOS-CON-001-Repository-Constitution.md)（Approved v1.0.0） | Constitution | Repository 治理基線與變更管理 |
| REF-011 | [AEOS-DIA-001 — Documentation Information Architecture](../documentation/AEOS-DIA-001-Documentation-Information-Architecture.md) | Information Architecture | 文件分類、組織與生命週期 |
| REF-012 | [AEOS-STD-001 — Documentation Format Standard](../standards/AEOS-STD-001-Documentation-Format-Standard.md)、[AEOS-STD-002 — Metadata Standard](../standards/AEOS-STD-002-Metadata-Standard.md)、[AEOS-STD-003 — Cross-reference Standard](../standards/AEOS-STD-003-Cross-reference-Standard.md)、[AEOS-STD-004 — Naming Standard](../standards/AEOS-STD-004-Naming-Standard.md)、[AEOS-STD-005 — Review Standard](../standards/AEOS-STD-005-Review-Standard.md) | Standards | 文件格式、Metadata、Cross-reference、Naming 與 Review 規則 |
| REF-013 | EWO-AEOS-0018 | EWO | 本文件之工作來源 |
| REF-014 | AEOS-ADR-002 — WA-001 Fact Authority Transition | ADR | WA-001 Authority Classification 與 Approved Fact Authority Baseline |

## 16. Revision History

| 版本 | 日期 | 變更摘要 | 作者 |
|------|------|----------|------|
| 1.1.0 | 2026-08-08 | 依 EWO-AEOS-0040 Wave 2（AEOS-ADR-002 已核准）：執行 Architecture Transition——WA-001 分類為歷史來源（Historical Reference）；Authority 階層（D0）與對應來源重錨至 AEOS-ARCH-001／Approved 架構載體；References 重錨（EWO-AEOS-0040） | Codex |
| 1.0.0 | 2026-08-06 | Architecture Review 核准並合併；狀態更新為 Approved，成為 AEOS Dependency Architecture 正式定義（EWO-AEOS-0019；AR-AEOS-0019-R1） | Codex |
| 0.1.0 | 2026-08-06 | 初版建立：依 WA-001 與 AEOS-ARCH-004 定義 Dependency Identity、Type、Direction、Boundary、Strength、Ownership、Lifecycle、Change Impact、Circular Dependency 與 Violation Governance（EWO-AEOS-0018） | Codex |
