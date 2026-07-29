# Gelato 中控系統 前端 部署 SOP

單檔 `index.html`，依 UG 設計規範（深綠×米白、無印風、表格為主、紅橙綠狀態燈）。

## 1. 本機預覽（不用部署就能看）
直接雙擊 `index.html` 後在網址列加上 `?demo=1` → 用內建示範資料（2026-07-29 中控檔的真實數字）預覽全部五個分頁。

## 2. 接上後端
用記事本／VS Code 打開 `index.html`，把最上面這行換成你的 GAS 部署網址：
```js
const API_URL = "https://script.google.com/macros/s/XXXX/exec";
```

## 3. 部署 GitHub Pages（跟營運儀表板同一套流程）
```bash
cd "前端資料夾"
git init
git add .
git commit -m "Gelato 中控前端 v1"
gh repo create johney8590-svg/gelato-control-center --public --source . --push
gh api repos/johney8590-svg/gelato-control-center/pages -X POST -f build_type=legacy -f "source[branch]=main" -f "source[path]=/"
```
完成後網址：`https://johney8590-svg.github.io/gelato-control-center/`
（repo 公開但無任何敏感值——只有 GAS URL；資料要密碼登入才拿得到。）

## 功能備忘
- **登入**：共用密碼（後端 Script Properties 設定）；token 6 小時過期會自動跳回登入頁提示，重登即可（同營運儀表板的 session 行為）。
- **總覽**：KPI 六卡＋「系統判讀」（自動產生：卡關警示、定價/損益成本率落差、季節係數檢查、兩平安全墊）＋階段進度＋警示任務。
- **甘特**：時間軸條狀圖（紅=卡關、深綠=完成、紅線=今天）。
- **配方**：全部版本的 Mix 成本／成品每克成本／固糖脂平衡 ✓⚠。
- **定價**：店內＋外送成本率紅橙綠、加權平均。
- **損益**：三情境卡＋12 個月推演（圖表＋表格）。
- **行動版**：底部分頁導航；表格橫向捲動（進一步卡片化列入 backlog）。
- 資料流是**唯讀**：輸入與調參數都在 Google Sheet 做，前端看板即時反映。
