# DocScan 部署指南 | Deployment Guide

## 📱 在 iPhone 上使用

1. **開啟網址**（部署後）
2. **允許相機權限**（Safari 會跳出提示）
3. **建議「加到主畫面」** — 這樣就像 App 一樣使用
   - Safari → 分享鈕 → 加到主畫面

## 🚀 部署到 Google Apps Script

### 步驟 1：建立 GAS 專案
1. 到 https://script.google.com/
2. 點「新增專案」

### 步驟 2：新增 HTML 檔
1. 左側「檔案」→ `+` → `HTML`
2. 命名為 `index`
3. 把 `index.html` 全部內容貼上（**要先刪掉預設的 html 標籤**）

### 步驟 3：建立 Code.gs 入口
把 `Code.gs` 內容替換為：

```javascript
function doGet() {
  return HtmlService.createHtmlOutputFromFile('index')
    .setTitle('DocScan · 文件掃描')
    .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
    .addMetaTag('viewport', 'width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover');
}
```

### 步驟 4：部署
1. 右上「部署」→「新增部署作業」
2. 類型：**網頁應用程式**
3. 存取權：**所有人** 或 **任何擁有 Google 帳戶的使用者**（依需求）
4. 點「部署」並授權
5. 複製部署網址

### 步驟 5：在手機開啟
用 iPhone Safari 打開該網址 → 允許相機 → 加到主畫面

---

## ⚠️ 重要注意事項

### Google Apps Script 的限制
GAS 的 `HtmlService` 會把頁面放在 iframe 裡面，**這會影響：**
- **相機權限**：iframe 內要求相機權限時,iOS Safari 較嚴格,可能會遮蔽
- **檔案下載**：在 iframe 內下載時,iOS 會問一次才能開啟

### 📍 建議的替代方案（更順）
如果上述有問題，建議改成這兩種部署方式其中一個：

**A. 直接放 GitHub Pages（推薦）**
1. 建一個 GitHub 倉庫
2. 上傳 `index.html`
3. Settings → Pages → 啟用
4. 網址就是 `https://你的帳號.github.io/倉庫名/`
5. **這樣相機權限最順,下載也直接**

**B. Cloudflare Pages / Netlify / Vercel**
- 拖放 `index.html` 到他們的部署介面,幾秒就好
- 支援 HTTPS（相機 API 必須要 HTTPS）

---

## 🎨 功能清單

✅ 高畫質後置相機拍攝（要求 4K）
✅ 即時邊緣偵測（黃色輪廓疊加在畫面上）
✅ 手動/自動模式切換
✅ 拍完自動跳到裁切編輯,可拖曳四角微調
✅ OpenCV 透視校正
✅ 連續拍攝多頁（右上角計數器）
✅ 回看畫廊（縮圖 + 刪除 + 編輯）
✅ 5 種濾鏡：原始 / 魔法 / 黑白 / 灰階 / 彩色增強
✅ 亮度 / 對比 / 銳化滑桿
✅ 4 種長寬比：Auto / A4 / Letter / 1:1
✅ 旋轉 90°
✅ 閃光燈（手電筒模式,iPhone 支援）
✅ PDF 多頁輸出（→ 檔案 App）
✅ JPG 單頁下載（→ 相簿）
✅ 雙語切換（繁中/EN）
✅ 全 iOS 安全區適配（瀏海 / Home Indicator）

## 🔧 技術棧

- **OpenCV.js 4.8** — 邊緣偵測 + 透視校正
- **jsPDF** — 多頁 PDF 生成
- **Canvas API** — 亮度/對比/濾鏡
- **MediaDevices API** — 相機串流 + 手電筒控制
- **純前端** — 無後端、無追蹤、無 API 金鑰
