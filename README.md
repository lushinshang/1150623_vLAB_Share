# Vlab 1150623 線上直播分享會：iPAS AI 應用規劃師高效備考與 RAG 知識工程專案

這個專案是為 **2026-06-23 Vlab 線上直播分享會** 所建立的「AI 備考方法論、自動化工具鏈與沉浸式簡報播放器」整合專案。旨在幫助講師與考生透過現代化的 **RAG 知識工程** 與 **Agent 協同**，大幅降維備考阻力，實現高效大腦 Fine-tuning。

本檔案 (`README.md`) 專為 AI 代理人 (AI Agent, 如 Gemini, Claude, Cursor) 設計，以結構化的方式描述了專案的目錄架構、檔案關係、核心播放器架構以及備考方法論。

---

## 📁 專案目錄結構與檔案索引 (File Map)

```text
/Users/lanss/projects/2_Practice/readpaper/1150623_Vlab分享
├── README.md                              # 本說明檔 (AI 快速定位指南)
├── index.html                             # 沉浸式簡報播放器 (前端 PPT 播放器，JS 解耦設計)
├── AI_概念統一關聯圖_引導版_v3.md          # 備考思維地圖 (六圖一表，Mermaid 語法)
├── NotebookLM_Gemini_考照工作流.md         # 備考手冊與神級提示詞 Sandbox (含命題王終極指令)
├── sync_session.py                        # 本 Session 對話紀錄同步工具腳本
├── image/                                 # 投影片圖片資料夾
│   ├── 1.png                              # 簡報投影片第 1 頁
│   └── ... 19.png                         # 簡報投影片第 19 頁 (共 19 頁)
└── 參考資料/                              # 收納所有背景文檔、歷史對話與大數據
    ├── session_feb20b12-e2ce-4ed5-8012-...md # 本輪對話的完整紀錄 (由腳本自動同步寫入此處)
    ├── 114年度AI應用規劃師能力鑑定簡章...pdf # 官方簡章
    ├── 【電子書下載】AI產業人才認定指引...pdf # 官方數百頁厚重 PDF 教材 (真理源)
    ├── 月初的簡報.md / .pdf / .pptx        # 月初分享簡報的各種格式 (OCR md 檔)
    ├── 使用AI備考證照.md / .pdf            # 前次分享簡報的各種格式
    ├── 01.txt / 02.txt / 03.txt           # 歷史優化對話紀錄 TXT (已收納)
    ├── chart_06_generation_defense.json   # 移入的圖表提取 JSON 數據 (已收納)
    └── table_01_concept_index.json        # 移入的概念索引表 JSON 數據 (已收納)
```

---

## 💻 播放器架構與二次開發指南 (`index.html`)

播放器採用極簡暗黑美學，並將**「簡報內容 (Data)」與「播放邏輯 (Logic)」進行解耦**，提供極佳的擴充彈性。

### 1. Slide 內容陣列 (JavaScript Data Schema)
在 [index.html](file:///Users/lanss/projects/2_Practice/readpaper/1150623_Vlab分享/index.html) 的 `<script>` 區塊內，定義了 `slides` 陣列。若 AI 需協助使用者插頁、更換順序或插入影片，請直接修改此陣列：

*   **插入圖片**：
    ```javascript
    { type: 'image', src: 'image/new_slide.png', alt: '說明文字' }
    ```
*   **插入影片 (支援自動暫停機制)**：
    ```javascript
    { type: 'video', src: 'image/demo.mp4', poster: 'image/poster.png' }
    ```
*   **嵌入 YouTube/Vimeo 等 iframe**：
    ```javascript
    { type: 'iframe', src: 'https://www.youtube.com/embed/影片ID' }
    ```

### 2. 互動體驗 (UX) 與效能特色
*   **雙向預載 (Preloading)**：背景自動加載「前一張」與「後一張」圖片，完全杜絕切換時的白底或載入延遲。
*   **4D 操控支援**：支援滑鼠點擊、鍵盤左右鍵/空白鍵、全螢幕模式，以及行動裝置觸控左右滑動 (Swipe)。
*   **頁碼醒目指示**：左上角設有高亮毛玻璃頁碼徽章（如 `05 / 19`），底部進度條支援 Timeline 點擊跳轉。

---

## 🧠 備考方法論與知識工程 (Methodology Overview)

專案手冊 [NotebookLM_Gemini_考照工作流.md](file:///Users/lanss/projects/2_Practice/readpaper/1150623_Vlab分享/NotebookLM_Gemini_考照工作流.md) 記載了完整的備考心流，任何 AI Agent 可依據以下核心邏輯協助考生：

### 1. 雙軌備考與 AI 數位轉型
*   **傳統備考路徑**：收集資料 ➡️ 規劃 ➡️ 研讀 ➡️ 考古題 ➡️ 錯題修正 ➡️ 應考。（線性且缺乏驗證工具，容易事倍功半）。
*   **AI 輔助備考路徑**：雲端 Pipeline 採集 ➡️ Agent 雙向校對 ➡️ 多維度 MD 重構 ➡️ 知識全覽圖 ➡️ 智能遞迴刷題。

### 2. 雲端前處理 Pipeline (Google Colab 算力調度)
*   **IBM Docling**：將雜亂 PDF/PPT 一鍵高精度轉換為結構化 Markdown。
*   **yt-dlp 雲端防禦**：使用 `-f bestaudio` 與 `--cookies-from-browser`，抵抗 **429 封 IP** 與 **OOM 記憶體爆炸**，在雲端高速下載影音。
*   **Whisper 轉譯與 OpenCC**：強制 `--language zh` 防止口音誤判，並使用 `OpenCC` 簡轉繁。
*   **自研 Python 腳本**：使用 RegExp 刨除 `.srt` 時間戳與序號，清洗出純文字。

### 3. AI Agent 交叉驗證機制 (Claude Code 協同)
*   **三級真實性驗證**：
    1.  **最優先級**：官方教材為唯一真理源 (Ground Truth)。
    2.  **學術基礎**：權威教授課程內容。
    3.  **交叉比對**：其餘前輩心得與網路分享，經 Agent 比對上述兩者、修正可信度後才採信。
*   AI Agent 自動進行**重點提煉**，並比對官方教材找出網路資料的講錯與過時之處，主動補強知識盲點。

### 4. 關鍵字字典與知識全覽圖
*   交叉驗證後的資料依章節重構為：關聯圖、關鍵字解釋、運用場景、比較表、心智圖。
*   提煉核心關鍵字，建立**「關鍵字字典」**（定義字與字之間的依賴性）。
*   生圖輸出「知識全覽圖」，建立作答的右腦直覺。

### 5. 終極命題王 (Gemini Gems) 與防污染閉環
*   **單庫 All-in-One 備考底座**：封裝三大黃金檔案（官方教材.md、補充教材.md、官方考古題.md）合併為單一文件。
*   **NotebookLM 智庫**：作為嚴謹無塵室大腦，100%防幻覺，並提供雙人 Podcast 聽覺複習。
*   **Gemini Gem 命題王**：利用手冊中的「終極命題王 Prompt」進行高壓情境題模擬與弱點診斷（過/欠擬合）。
*   **防污染回寫**：嚴禁刷題對話同步回 NotebookLM。手動將批改產出的「觀念修正點」Append 貼回 All-in-One 文件最末端，實現自適應備考底座演進。

---

## 🛠️ 開發者與同步指引 (Developer Guides)

### 1. 同步對話紀錄
每當考生與 AI Agent 在本專案中完成新的討論或程式碼修改，請在根目錄執行：
```bash
python3 sync_session.py
```
此腳本會讀取 `transcript.jsonl`，並自動更新根目錄下的 [session_feb20b12-e2ce-4ed5-8012-f7ec20216f7b.md](file:///Users/lanss/projects/2_Practice/readpaper/1150623_Vlab分享/session_feb20b12-e2ce-4ed5-8012-f7ec20216f7b.md)，確保對話歷程始終保持最新。

### 2. 考生與 AI 協同方針
*   **提問優先**：AI 在進行檔案操作、命令執行前，應先向考生確認輸出路徑與預期結果，遵守「不自行決定路徑或檔名」的原則。
*   **繁體中文規範**：一律使用繁體中文（台灣用語），不使用中國大陸慣用語。
*   **地圖更新**：如發掘出新的考點，應同步更新思維地圖 [AI_概念統一關聯圖_引導版_v3.md](file:///Users/lanss/projects/2_Practice/readpaper/1150623_Vlab分享/AI_概念統一關聯圖_引導版_v3.md)。
