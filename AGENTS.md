# AGENTS.md — 底層技術與逆向工程筆記庫開發規範

## 1. 專案目的 (Project Purpose)
本專案為個人資安與底層系統學習筆記網站（Static Knowledge Base），用於系統化整理逆向工程工具、組合語言指令解析與系統除錯筆記。
本文件的目標是讓 AI 助手在無需人工反覆說明的前提下，能夠依照統一的視覺風格、HTML 結構與檔案命名規則，快速新增或維護筆記頁面。

## 2. 目錄結構與檔案命名規範 (Directory & Naming)
- `index.html`：首頁與文章索引目錄，所有新筆記都必須在此註冊卡片。
- `css/style.css`：全站唯一樣式表，所有頁面共用此檔案。
- `notes/`：存放所有獨立筆記頁面。
  - **命名規則**：`NN-kebab-case-topic.html`（兩位數流水號 + 小寫英文連字號主題），例如 `03-gdb-peda-guide.html`。
  - **禁止事項**：不可在根目錄直接散落筆記 HTML，也不可使用中文或空白作為檔名。

## 3. 網頁整體風格與設計規範 (Design System)
- **風格定位**：GitHub Dark 風格之技術文件（高對比、低視覺疲勞、程式碼清晰）。
- **色彩變數（定義於 `css/style.css`）**：
  - 背景主色：`--bg-main: #0d1117`
  - 卡片與區塊底色：`--bg-card: #161b22`
  - 邊框色：`--border: #30363d`
  - 主強調色：`--accent: #58a6ff`
  - 標籤綠：`--tag-green: #3fb950`
- **樣式守則**：禁止在 HTML 中寫 `style="..."` 行內樣式，必須使用 `css/style.css` 預先定義的語意化 class。

## 4. 頁面 HTML 結構與元件規範 (Page Template & Components)
每篇位於 `notes/` 的筆記頁面**必須**包含以下結構與元件：
1. **引入樣式**：`<link rel="stylesheet" href="../css/style.css">`（注意相對路徑）。
2. **頂部導覽列 (`<nav class="topbar">`)**：必須包含返回首頁連結 `<a href="../index.html">← 返回筆記目錄</a>`。
3. **文章標頭 (`<header class="article-header">`)**：包含分類標籤 `<span class="badge">`、主標題 `<h1>` 與摘要 `<p class="subtitle">`。
4. **內容可用元件**：
   - 重點提示框：`<div class="callout">...</div>`
   - 指令／暫存器對照表：`<table class="spec-table">...</table>`
   - 程式碼／組合語言區塊：`<pre><code>...</code></pre>`

## 5. 新增筆記頁面的標準流程 (SOP for AI)
當使用者要求「新增一篇關於 [主題] 的筆記」時，AI 必須依序執行以下三個步驟：
1. **建立檔案**：在 `notes/` 下建立符合 `NN-topic.html` 命名規則的檔案，並完整套用第 4 節的 HTML 骨架。
2. **撰寫內容**：使用 `<table class="spec-table">` 整理指令或參數，並使用 `<div class="callout">` 總結關鍵觀念或常見陷阱。
3. **更新首頁索引**：開啟 `index.html`，在 `<section id="notes-grid" class="grid">` 區塊最下方，依照既有 `<a class="card">` 格式新增該篇筆記的連結卡片，確保首頁可直接點擊進入。

## 6. AI 修改與開發守則 (AI Guardrails)
- **零外部依賴**：不引入 Bootstrap、Tailwind、jQuery 或任何前端框架，維持純靜態 HTML/CSS 以確保 GitHub Pages 即時部署。
- **路徑一致性**：從 `index.html` 連往筆記使用 `notes/檔名.html`；從筆記連回首頁使用 `../index.html`。
- **增量修改原則**：修改現有頁面時，僅更新目標區塊內容，不可隨意更改既有的 class 名稱或移除導覽列。