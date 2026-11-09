# AGENTS.md — LLM & Agent 實戰知識庫開發規範

## 1. 專案目的 (Project Purpose)
本專案是一個採用純靜態 HTML5 與 CSS3 建構的「LLM 與 AI Agent 技術講義網站」（部署於 GitHub Pages）。
設計目標是建立一套模組化、零外部框架依賴的教材系統，讓開發者能透過 AI 代理（AI Agents）快速將零散的技術筆記轉換為結構統一、具備一致視覺體驗的線上講義頁面。

## 2. 檔案結構與命名規則 (Directory & Naming Conventions)
所有檔案必須嚴格遵守以下目錄結構與命名規範：

```text
/
├── AGENTS.md                   # AI 協作與擴充規則文件（本檔案）
├── index.html                  # 網站首頁（包含全站側邊欄與講義卡片索引）
├── assets/
│   └── style.css               # 全站唯一樣式表（禁止在 HTML 內寫 inline style）
└── pages/                      # 存放所有單元講義頁面的資料夾
    ├── 01-agent-workflow.html  # 單元 01：Multi-Agent 架構導論
    ├── 02-rag-architecture.html# 單元 02：RAG 檢索增強生成架構
    └── [NN]-[kebab-case].html  # 新增講義命名格式：兩位數編號 + 英文小寫連字號
```

* **命名強制規定**：`pages/` 下的所有新講義必須以 `兩位數序號-英文主題名稱.html` 命名（例如 `03-prompt-caching.html`、`04-tool-calling.html`），序號需依照現有檔案順延。

## 3. 視覺風格與專用 CSS 元件 (Design System & Components)
網站採用深色現代技術文件風格（Dark Tech Documentation），所有頁面共用 `assets/style.css`。AI 在撰寫講義內容時，**必須使用以下預定義的語意化元件 Class**，禁止自行發明未定義的 Class：

1. **章節卡片容器 (`.doc-section`)**：每個主要段落需包在 `<section class="doc-section">` 中。
2. **重點提示框 (`.callout`)**：用於強調核心觀念或注意事項：
   * 觀念提示：`<div class="callout callout-info"><strong>💡 核心觀念：</strong>內容...</div>`
   * 警告提醒：`<div class="callout callout-warn"><strong>⚠️ 注意事項：</strong>內容...</div>`
3. **步驟流程列表 (`.step-list`)**：用於呈現有順序的操作或架構流程，使用 `<ol class="step-list">`。
4. **標籤徽章 (`.badge`)**：用於標示技術關鍵字或單元編號，使用 `<span class="badge">標籤文字</span>`。

## 4. 新增講義頁面的標準 HTML 骨架 (Page Template)
當使用者要求「新增一篇關於 [主題] 的講義」時，AI 必須在 `pages/` 資料夾下建立新檔案，並**嚴格套用以下 HTML 骨架**：

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[單元名稱] | LLM & Agent 實戰知識庫</title>
  <link rel="stylesheet" href="../assets/style.css">
</head>
<body>
  <div class="layout">
    <!-- 左側導覽列：新增頁面時必須同步更新所有頁面的 nav 清單 -->
    <aside class="sidebar">
      <div class="brand">🤖 LLM & Agent 講義</div>
      <nav class="nav-links">
        <a href="../index.html" class="nav-item">🏠 知識庫首頁</a>
        <div class="nav-group-title">核心講義單元</div>
        <a href="01-agent-workflow.html" class="nav-item">01. Multi-Agent 架構導論</a>
        <a href="02-rag-architecture.html" class="nav-item">02. RAG 檢索增強生成</a>
        <!-- 新頁面連結請依序加在此處，當前頁面需加上 active class -->
      </nav>
    </aside>

    <!-- 右側主要講義內容區 -->
    <main class="content">
      <header class="page-header">
        <div class="meta">
          <span class="badge">Unit [NN]</span>
          <span class="badge">[技術分類]</span>
        </div>
        <h1>[單元主標題]</h1>
        <p class="subtitle">[一句話總結本單元的核心學習目標]</p>
      </header>

      <section class="doc-section">
        <h2>1. 概念背景與核心問題</h2>
        <p>[正文內容...]</p>
        <div class="callout callout-info">
          <strong>💡 核心觀念：</strong>[重點總結]
        </div>
      </section>

      <section class="doc-section">
        <h2>2. 架構流程與實作解析</h2>
        <ol class="step-list">
          <li><strong>步驟一：</strong>[說明]</li>
          <li><strong>步驟二：</strong>[說明]</li>
        </ol>
        <pre><code>// 範例程式碼或設定檔區塊</code></pre>
      </section>

      <footer class="page-footer">
        <a href="../index.html" class="btn-back">← 返回知識庫首頁</a>
      </footer>
    </main>
  </div>
</body>
</html>
```

## 5. AI 新增或修改內容的標準作業流程 (SOP for AI Agents)
當收到「新增一個頁面/單元」的指令時，AI **必須依序完成以下 3 個步驟**，缺一不可：

1. **步驟 1 — 建立講義檔案**：依照第 2 節的命名規則與第 4 節的 HTML 骨架，在 `pages/` 目錄下建立新的 `.html` 檔案，並確保每個單元至少包含 1 個 `.callout` 提示框與 1 個程式碼或結構區塊。
2. **步驟 2 — 同步更新側邊欄 (`.nav-links`)**：將新頁面的連結同步加入 `index.html` 以及 `pages/` 下現有頁面的 `<nav class="nav-links">` 區塊中，確保全站導覽列一致。
3. **步驟 3 — 在首頁新增課程卡片 (`.card-grid`)**：打開 `index.html`，在 `<div class="card-grid">` 區塊內新增一張對應的 `<a class="topic-card">` 卡片，讓使用者能從首頁點擊進入新講義。

## 6. 嚴格禁止事項 (Strict Constraints)
* **禁止**使用任何行內樣式（`style="..."`）或在 HTML 檔案中插入 `<style>` 標籤，所有視覺變動一律透過 `assets/style.css` 控管。
* **禁止**引入外部大型前端框架（如 Bootstrap、Tailwind CDN），以維持程式碼乾淨與高可讀性。
* **禁止**破壞相對路徑：在 `index.html` 引用頁面時使用 `pages/XX.html`；在 `pages/` 內部引用樣式或返回首頁時，必須使用 `../assets/style.css` 與 `../index.html`。