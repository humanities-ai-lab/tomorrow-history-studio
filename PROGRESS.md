# Tomorrow History Studio 專案進度表

## 🏐 目前狀態：網站基礎架構完成
已完成官方網站的基礎頁面開發與樣式設計。

## 🔐 2026-02-25 資安掃描與修補記錄
### 本次使用的檢查方式（記錄）
1. 關鍵字掃描（工作區）:
   - `rg -n -i --hidden --glob '!.git/*' --glob '!*.pdf' "(api[_-]?key|secret|token|password|client[_-]?secret|private[_-]?key|AKIA...|sk-...|BEGIN ... KEY)"`
2. 危險模式掃描（前端）:
   - `rg -n --hidden --glob '!.git/*' "(target=\"_blank\"|innerHTML|eval\\(|new Function\\(|document.write\\()"`
3. Git 歷史掃描:
   - `git rev-list --all | xargs -I{} git grep -n -I -E "(AKIA...|sk-...|BEGIN ... KEY|api[_-]?key|client[_-]?secret|authorization: bearer)" {}`
4. 人工逐檔審查:
   - `index.html`、`courses-site/index.html`、`courses.html`、`members.html`
5. 供應鏈完整性處理:
   - 將 `tailwind` 與 `lucide` 改為本地 vendor 檔
   - 以 `curl + openssl` 產生 Font Awesome SRI（SHA-512）

### 修補內容
1. 外部連結防護:
   - 所有 `target="_blank"` 連結已補上 `rel="noopener noreferrer"`
2. CDN 供應鏈風險降低:
   - `index.html` 與 `courses-site/index.html` 改用本地 `assets/vendor/` 腳本
   - `courses.html`、`members.html` 的 Font Awesome 加上 `integrity` 與 `crossorigin`
3. CSP 強化:
   - 各頁 CSP 加入 `base-uri 'self'`、`object-src 'none'`、`connect-src 'none'`、`form-action`、`upgrade-insecure-requests`
4. Header 設定落地（供支援 `_headers` 平台使用）:
   - 新增 `_headers`，集中定義 `X-Frame-Options`、`nosniff`、`Referrer-Policy`、`Permissions-Policy`、CSP
5. 版控衛生:
   - 新增 `.gitignore`（忽略 `.DS_Store`）
   - 已將已追蹤的 `.DS_Store` 從 Git index 移除

### 結果摘要
- 目前未發現明確 API key / token / 私鑰洩漏（含 Git 歷史）。
- 補齊前端常見弱點（tabnabbing、未固定第三方來源）與部署可用的安全標頭設定。

## ✅ 已完成事項
1. **網頁開發**:
   - `index.html`: 首頁架構完成。
   - `courses.html`: 課程列表頁面完成。
   - `members.html`: 成員介紹頁面完成。
   - `style.css`: 全站視覺樣式統一。
2. **課程系統**:
   - `courses-site/`: 子站點架構。
3. **安全審查**:
   - 已產出安全報告 (`security-report-humanities-ai-lab.github.io-2026-02-19.pdf`)。

## 📅 代辦事項 (TODO)
- [ ] **內容填充**: 更新成員具體資料與課程詳細說明。
- [ ] **互動功能**: 增加課程報名或諮詢表單。
- [ ] **SEO 優化**: 針對歷史教育關鍵字進行優化。
- [ ] **響應式檢查**: 確保在行動裝置上的顯示效果。

---
*Last updated by Wilson 🏐*
