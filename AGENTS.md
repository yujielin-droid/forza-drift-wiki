# AGENTS.md - Forza Horizon RWD 甩尾調校知識庫開發指南

## 1. 專案目的 (Project Purpose)
本專案為「Forza Horizon 純後驅 (RWD) 甩尾調校知識庫 (Forza Drift Tuning Wiki)」。
旨在透過標準化、模組化的靜態網頁結構，紀錄並展示各車款的純後驅甩尾底盤參數（胎壓、齒比、外傾角、防傾桿、差速器等）與調校物理邏輯，讓 AI 代理（Agents）在未來新增車款頁面或底盤教學時，能維持 100% 一致的視覺風格與程式碼結構。

## 2. 網頁整體風格與視覺規範 (Design & Style Guidelines)
- **主題配色 (Racing Dark Mode)**：
  - 主背景色：`#121212`
  - 卡片與區塊背景色：`#1e1e1e`
  - 邊框與分隔線：`#2e2e2e`
  - 主文字顏色：`#e0e0e0`，次要文字：`#a0a0a0`
  - 強調色 (霓虹綠)：`#00ff66`（用於數據指標、標籤與標題左邊框）
- **字體規範**：
  - 內文：`'Segoe UI', 'Noto Sans TC', sans-serif`
  - 數據與參數：`'Consolas', 'Courier New', monospace`（確保數值對齊易讀）
- **技術限制**：
  - 嚴禁引入外部 CSS 框架（如 Bootstrap、Tailwind）。所有樣式必須使用原生 CSS 撰寫，以確保單檔部署的高效性與低耦合。

## 3. 頁面結構規範 (Page Structure)
每個頁面必須嚴格遵守以下 HTML5 語意化結構：
1. `<header class="site-header">`：包含網站主標題、副標題與導覽列 `<nav class="site-nav">`。
2. `<main class="container">`：最大寬度 `1000px` 置中，內部使用 `.grid-layout` 排列內容卡片。
3. `<article class="tuning-card">`：核心內容單元。每張卡片必須包含：
   - `<div class="card-header">`：放置標題 `<h2>` 與屬性標籤 `<span class="badge">`。
   - `<p class="card-desc">`：簡短物理邏輯與動態反應說明。
   - `<table class="param-table">`：標準雙欄參數表（左欄為「設定項目」，右欄為 `.val` 數值）。
4. `<footer class="site-footer">`：標註專案版本與 `AGENTS.md` 規範遵循聲明。

## 4. 檔案命名與目錄結構 (File Naming & Directory Rules)
- 所有檔案名稱一律使用 **kebab-case（全小寫英數字，單字間以連字號 `-` 連接）**，嚴禁空格、底線或中文檔名。
- **目錄結構**：
  - `/index.html`：知識庫首頁（核心調校通則、導覽目錄與示範車款設定）。
  - `/cars/[brand]-[model]-drift.html`：個別車款專屬調校頁（例如：`aston-martin-valkyrie-amr-pro-drift.html`、`nissan-silvia-s15-drift.html`）。
  - `/guides/[topic].html`：底盤物理專題頁（例如：`anti-roll-bars.html`、`alignment-camber.html`）。

## 5. 新增內容格式與 AI 開發守則 (Rules for AI Agents)
當使用者要求 AI「新增一台車的調校」或「新增一篇底盤教學頁面」時，AI 必須嚴格執行以下步驟：
1. **結構複製**：完整保留 `<head>` 中的 CSS 樣式表與 `<header>`、`<footer>` 結構，確保全站長相完全一致。
2. **純後驅 (RWD) 物理檢查**：檢查輸入參數是否符合 RWD 甩尾邏輯（例如：前輪外傾角必須為較大負值如 `-4.5°` 至 `-5.0°` 以維持大角度橫移抓地力；防傾桿數值為固定實數設定而非百分比；後差速器加速鎖定率應趨近 `100%`）。
3. **卡片模組化**：新增的調校數據必須封裝在 `<article class="tuning-card">` 內，數值必須加上 `<td class="val">` 標籤以套用螢光綠等寬字體。
4. **雙向導覽更新**：若建立新檔案（如 `/cars/xxx.html`），AI 必須主動提供 `index.html` 的 `<nav class="site-nav">` 更新片段，將新頁面的超連結加入導覽列中，避免產生孤島頁面。
