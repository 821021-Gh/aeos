# AEOS

AI 企業作業系統。

## 儲存庫概述

AEOS 是 AI Engineering Workspace 的 Enterprise Root Repository。它是工作區企業級文件與治理的正式所在地，涵蓋 Enterprise Architecture、Platform Governance 與 Capability Management。

## 儲存庫用途

AEOS 提供單一正式儲存庫，用以記錄及維護 AI Engineering Workspace 的企業架構基準、平台治理與能力管理。

## 儲存庫職責

- 維護 AI Engineering Workspace 的正式文件基準。
- 提供架構與治理產出物的入口點（請參閱參考文件）。
- 保持儲存庫基礎與架構內容分離。
- 交付不含佔位符、可立即投入正式環境的正式文件。

## 與 AI Engineering Workspace 的關係

AEOS 是 AI Engineering Workspace 的 Enterprise Root Repository：它代表企業層級的工作區，也是其架構與治理文件的正式入口。AEOS 本身不實作工程工作流程；工程交付遵循 YEOS 工程工作流程（請參閱 CONTRIBUTING.md）。

## 儲存庫原則

- 文件優先 — 正式文件是主要交付成果。
- 規格驅動 — 變更須先由規格與工單定義，再進行實作。
- Single Source of Truth — 儲存庫內容均可追溯至 Approved 來源，無須重複定義。
- 基礎解耦 — 儲存庫基礎與架構分別管理。
- 可投入正式環境 — 交付文件完整且正式，不含佔位符。

## 儲存庫狀態

| 項目 | 狀態 |
|------|------|
| 目前階段 | Foundation |
| 版本 | 1.0.0 |

## 儲存庫結構

- `README.md` — 儲存庫入口點（概述、目的、狀態、結構、工作流程）
- `docs/` — 正式文件（架構、治理、能力，依領域整理）
- `engineering/` — 工程工作流程與工程工單 (EWO)
- `templates/` — 文件與工單模板
- `assets/` — 靜態資源

## 開發工作流程

- 每項變更均由 Engineering Work Order (EWO) 定義，並逐一完成各 EWO。
- 在功能分支進行實作，並以 Draft Pull Request 提交交付內容。
- 工程交付遵循 YEOS 工程工作流程（請參閱 CONTRIBUTING.md）。

## 參考文件

| 文件 | 類型 | 角色 |
|------|------|------|
| [AEOS-ARCH-001 — Architecture Baseline](docs/architecture/AEOS-ARCH-001-Architecture-Baseline.md) | 架構入口文件 | 架構基準與入口點 |
| WA-001 — AI Engineering Workspace Architecture (Approved v1.0.0) | 架構來源 | Approved 架構來源 |
