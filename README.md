# bravo913 GitHub Pages 設定指南

將此資料夾的內容部署到 `https://bravo913.github.io/`，供 App 的 Universal Links 與著陸頁使用。

## 步驟

1. 至 GitHub 建立 Organization：`bravo913`
2. 在該 org 下建立 repo：**`bravo913.github.io`**（名稱必須完全一致才會成為 org 的 Pages 站）
3. 把本資料夾（`docs/github-pages-templates/`）的全部內容推上去
4. Settings → Pages → Source 選 `main` branch / root，等待部署
5. 驗證 AASA 可存取：
   ```
   curl -I https://bravo913.github.io/.well-known/apple-app-site-association
   ```
   應回傳 `200 OK`，Content-Type 為 `application/json`（GitHub Pages 會自動判定）
6. 使用 Apple 驗證工具：
   ```
   https://search.developer.apple.com/appsearch-validation-tool/?domain=bravo913.github.io
   ```
   或 AASA validator：https://branch.io/resources/aasa-validator/

## 檔案說明

- `.well-known/apple-app-site-association` — Apple Universal Links 驗證檔（無副檔名）
  - Team ID：`437N34D3P6`
  - Bundle ID：`tw.bravo913.radio`
  - 接管路徑：`/program/*`、`/unit/*`、`/episode/*`
- `index.html` — 沒裝 App 的訪客看到的著陸頁，含 `bravo://` 手動 fallback
- `logo.png` — 著陸頁上方的 βravo 手形標誌（同 AppIcon / BravoLogoStacked 3x），必須放在 repo root

## 上架 App Store 後

編輯 `index.html` 的 `btn-secondary` 區塊，填入 App Store 連結並取消註解。
同時到 [ShareManager.swift:41](../../BravoRadio/Services/ShareManager.swift) 填入 `appStoreURL`。

## 注意事項

- AASA 檔名**不能**加 `.json` 副檔名
- 變更 AASA 後，iOS 會快取舊版本，重新安裝 App 或重啟裝置會強制刷新
- 目前只支援單一 App ID；若未來有其他 bundle ID（例如 macOS/tvOS），擴充 `appIDs` 陣列
