# Cable Atlas

纜繩動作圖鑑 + 接頭對照 + PPL 課表 + 訓練紀錄。單一資料夾、純靜態、可安裝成手機 App。

## 檔案

| 檔案 | 用途 |
|---|---|
| `index.html` | 全部內容與程式碼（圖鑑 / 課表 / 訓練三個分頁） |
| `manifest.json` | PWA 設定：App 名稱、圖示、主題色 |
| `sw.js` | Service Worker，負責離線快取 |
| `icon-192.png` `icon-512.png` | App 圖示 |
| `icon-512-maskable.png` | Android 自適應圖示 |

## 部署到 GitHub Pages

1. 建一個新的 repo，例如 `cable-atlas`
2. 把這五個檔案全部放進 repo **根目錄**（不要放在子資料夾）
3. `git add . && git commit -m "init" && git push`
4. repo → **Settings** → **Pages** → Source 選 `main` 分支、資料夾 `/ (root)` → Save
5. 等 1–3 分鐘，網址會是 `https://<你的帳號>.github.io/cable-atlas/`

> Service Worker 只在 **https** 或 `localhost` 下運作，所以一定要走上面的流程，直接打開本機 html 檔（`file://`）不會有離線功能。

## 安裝到手機

**iPhone（Safari）** — 開網址 → 分享 → 加入主畫面
**Android（Chrome）** — 開網址 → 右上選單 → 安裝應用程式（或等待自動提示）

裝好後從主畫面開啟，沒有網址列，離線也能用。

## 本機測試

```bash
cd cable-atlas
python3 -m http.server 8000
```

打開 `http://localhost:8000`。`localhost` 被視為安全來源，Service Worker 與 localStorage 都能正常運作。

## 改內容

- **改課表**：搜尋 `var PPL =`，每個動作是一個物件
  `{zh:'中文名', en:'English Name', s:組數, r:'次數區間', n:'備註'}`
  改完課表分頁與訓練分頁會同步更新，不用改兩次。
- **改動作圖鑑**：直接編輯 HTML 裡的 `<table>`。
- **改配色**：檔案最上方 `:root` 的 CSS 變數。

改完檔案後，把 `sw.js` 裡的 `cable-atlas-v1` 改成 `v2`（每次改都 +1），使用者下次開啟才會拿到新版，否則會讀到舊快取。

## 資料儲存

訓練紀錄存在瀏覽器的 `localStorage`，key 為 `cba:sessions`。

- **只存在這台裝置**，不會上傳到任何地方
- 換手機、清除瀏覽資料會消失 → 定期用「匯出 JSON」備份
- 想跨裝置同步的話，需要接後端（Supabase 或 Firebase 免費方案即可）

## 下一步可以加什麼

| 功能 | 難度 | 效益 |
|---|---|---|
| 個人紀錄（PR）自動標記 | 低 | 高 |
| 訓練量趨勢圖（Chart.js） | 中 | 中 |
| 匯入 JSON 還原備份 | 低 | 中 |
| 跨裝置同步（Supabase） | 高 | 視需求 |

## 與 Dumbbell Atlas 的關係

兩者是姐妹站，架構與配色一致，但變因不同：

| | Dumbbell Atlas | Cable Atlas |
|---|---|---|
| 核心變因 | bench 角度 | 接頭 × 滑輪高度 × 站位 |
| 資料 key | `dba:sessions` | `cba:sessions` |
| 快取名 | `dumbbell-atlas-vN` | `cable-atlas-vN` |

**兩者的 localStorage 與快取完全獨立**，可以同時安裝在手機上，訓練紀錄不會互相干擾。
