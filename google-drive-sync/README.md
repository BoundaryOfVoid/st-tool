# Google Drive 雲端硬碟同步模組 (SillyTavern 特化)

本模組展示如何透過 **Google Identity Services (GIS)** 與 **Google Drive REST API v3**，在純靜態前端網頁中實現**「零伺服器代管・100% 存放於使用者個人雲端硬碟」**的角色專屬樣式與正則規則跨裝置同步。

---

## 🌟 核心設計與安全優勢

1. **零資料庫代管・零隱私責任**：
   - 傳統方案會將資料存於開發者的雲端資料庫；本模組則透過 Google 官方的 `drive.appdata` 授權，直接將資料寫入**使用者個人的 Google 雲端硬碟隱藏應用程式目錄**。
   - 身為網站作者或任何第三方，物理上完全無法接觸、讀取或攔截使用者的任何對話紀錄與設定。
2. **零伺服器費用**：儲存空間直接使用每位使用者 Google 帳號自帶的 15 GB 免費額度，專案維護費用永遠為 0 元。
3. **極簡權限（最小權限原則）**：
   - 僅請求 `drive.appdata` 權限，本工具**完全看不到**使用者雲端硬碟中原本存放的照片、文件或私密檔案。
4. **跨裝置無縫同步**：在電腦、iPad 或手機上登入同一個 Google 帳號，即可隨時抓取（Fetch）最新角色設定。

---

## 🚀 1 分鐘取得 Google OAuth Client ID 指南 (完全免費)

要在您的網頁啟用 Google 登入連線，只需至 Google Cloud 建立一組免費的 Client ID：

### 步驟 1：建立 Google Cloud 專案
1. 前往 [Google Cloud Console](https://console.cloud.google.com/) 並登入 Google 帳號。
2. 點擊頂端專案選單 ➔ 點擊「**新增專案**」，輸入專案名稱（例如 `st-tool-sync`）並點擊「建立」。

### 步驟 2：設定 OAuth 同意畫面 (OAuth Consent Screen)
1. 在左側選單進入「**API 和服務 > OAuth 同意畫面**」。
2. 使用者類型選擇「**外部 (External)**」，點擊「建立」。
3. 填寫應用程式名稱（例如 `ST Tool Sync`）與您的支援電子郵件，滑到最下方點擊「儲存並繼續」。
4. 範圍（Scopes）頁面直接點擊「儲存並繼續」。
5. 測試使用者（Test users）頁面可將您自己的 Gmail 帳號新增為測試人員，點擊「儲存並繼續」。

### 步驟 3：建立 OAuth 用戶端 ID (Client ID)
1. 在左側選單進入「**API 和服務 > 憑證 (Credentials)**」。
2. 點擊頂部「**+ 建立憑證 > OAuth 用戶端 ID**」。
3. 應用程式類型選擇「**網頁應用程式 (Web application)**」。
4. **已授權的 JavaScript 來源 (Authorized JavaScript origins)** 填入以下允許連線的網址：
   - `http://localhost`
   - `http://127.0.0.1`
   - `https://boundaryofvoid.github.io` (您的 GitHub Pages 網址)
5. 點擊「建立」，畫面會立即顯示您的 **用戶端 ID (Client ID)**（格式如：`1234567890-abcdefg.apps.googleusercontent.com`）。

### 步驟 4：開始使用
1. 開啟本模組的 `index.html`。
2. 將剛才取得的 Client ID 貼入步驟 1 的設定框中，點擊「儲存並啟用連線」。
3. 點擊「授權連線 Google 雲端硬碟」，即可在彈出視窗完成授權並開始雙向同步！

---

## 📁 檔案說明

- `index.html`：包含 Google Drive AppData 搜尋、讀取、更新 (PATCH)、多區塊上傳 (Multipart Upload) 與角色 CRUD 完整實作。
- `README.md`：本設定與架構說明手冊。
