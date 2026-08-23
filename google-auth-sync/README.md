# Google 登入與 Firebase 雲端同步模組 (SillyTavern 特化)

本模組為 `st-tool` 專案的純前端擴充套件，專門展示如何透過 **Firebase Authentication**（Google OAuth 2.0 登入）與 **Cloud Firestore**（雲端 NoSQL 資料庫）實現角色專屬樣式與正則規則的跨裝置即時同步。

---

## 🌟 模組功能特點

1. **純前端無伺服器架構**：完全相容 GitHub Pages 靜態託管，無需自行架設 Node.js / Python 後端。
2. **Google 一鍵登入**：支援 Google 帳號授權，自動獲取唯一的使用者 ID (`UID`)。
3. **角色維度雲端 CRUD**：
   - 支援將特定角色（如 `伏雷`、`蘭`）的主題色、專屬 CSS 樣式與正則替換規則陣列儲存至雲端 Firestore。
   - 集合路徑：`/users/{uid}/character_presets/{charName}`。
4. **即時雙向監聽 (`onSnapshot`)**：在電腦端儲存或修改設定後，手機或 iPad 端會即時自動更新。
5. **資料隔離與隱私安全**：每位使用者僅能讀寫自己帳號下的資料庫路徑，絕不干涉他人資料。

---

## 🚀 快速上手與 Firebase 啟用指南 (免費 Spark 方案)

只需 3 分鐘即可於 Firebase Console 建立免費專案：

### 步驟 1：建立 Firebase 專案
1. 前往 [Firebase Console](https://console.firebase.google.com/) 並登入您的 Google 帳號。
2. 點擊「**新增專案**」，輸入專案名稱（例如 `st-tool-sync`）。
3. Google Analytics（分析）可自由選擇是否啟用，接著點擊「建立專案」。

### 步驟 2：啟用 Google 身分驗證 (Authentication)
1. 進入專案控制台，在左側選單點擊「**Build (建構) > Authentication**」。
2. 點擊「開始使用」，切換至「**Sign-in method (登入方式)**」分頁。
3. 選擇 **Google** 提供者，點擊「啟用 (Enable)」，填寫專案支援電子郵件後點擊「儲存」。

### 步驟 3：建立 Cloud Firestore 資料庫
1. 在左側選單點擊「**Build (建構) > Firestore Database**」。
2. 點擊「**建立資料庫**」，資料庫位置可選擇 `asia-east1 (台灣)` 或預設位置。
3. 安全性規則選擇「**以測試模式啟動 (Start in test mode)**」即可快速開始。

#### 🔒 推薦的 Firestore 安全性規則 (Security Rules)
前往 Firestore 的「**規則 (Rules)**」分頁，貼上以下規則以確保只有本人可以存取自己的設定：

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 限制每位使用者只能讀寫自己 uid 底下的資料
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### 步驟 4：設定授權網域 (Authorized Domains)
回到 **Authentication > Settings > Authorized domains**，確認或新增以下允許登入的網域：
- `localhost`
- `127.0.0.1`
- `boundaryofvoid.github.io` (您的 GitHub Pages 網址)

### 步驟 5：取得 `firebaseConfig` 並連線
1. 點擊專案首頁齒輪圖示 ➔ **專案設定 (Project settings)**。
2. 捲動至最下方「**您的應用程式**」，點擊網頁圖示 `</>` (Web)。
3. 輸入應用程式暱稱並註冊，畫面會顯示 `firebaseConfig`：
```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "st-tool-sync.firebaseapp.com",
  projectId: "st-tool-sync",
  storageBucket: "st-tool-sync.appspot.com",
  messagingSenderId: "...",
  appId: "..."
};
```
4. 開啟本模組的 `index.html`，將上述 JSON 物件貼入步驟 1 的設定框中，點擊「初始化 Firebase 連線」即可開始使用！

---

## 📁 檔案結構

- `index.html`：完整功能的 Google 登入、設定管理與 Firestore CRUD 展示網頁。
- `firebase-config.example.js`：專案設定檔結構範本。
- `README.md`：本設定說明文件。
