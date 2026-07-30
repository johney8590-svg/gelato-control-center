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

## 功能備忘（M3：儀表板直接作業版）
- **登入**：共用密碼（後端 Script Properties 設定）；token 6 小時過期會自動跳回登入頁提示，重登即可。
- **所有日常輸入都在儀表板完成**，Google Sheet 只當資料庫＋計算引擎（不需開啟；誤改可用 Sheets 版本記錄還原）；每筆寫入都記到 Sheet 的「異動紀錄」分頁。
- **總覽**：KPI 六卡＋「系統判讀」（卡關警示、定價/損益成本率落差、季節係數檢查、兩平安全墊）。
- **甘特**：時間軸圖＋「＋新增任務」＋每列「編輯」（狀態/完成%/日期/前置/備註）。
- **配方**：「＋新增配方版本」（動態原料明細表單）＋狀態下拉（測試中/定案/封存）＋製程參數（產出率/Overrun/密度/展售損耗）編輯。
- **原料**：新分頁——改價、改損耗率、改成分、新增原料；存檔後全部口味成本→定價→損益自動重算。
- **定價**：每品項「編輯」（售價/外送價/抽成/佔比）＋冰體克數編輯（BOM）。
- **損益**：18 個參數（三情境）直接改＋「儲存變更」；季節係數 12 格可改；三情境卡＋12 個月圖表。
- **?demo=1**：內建完整本機計算引擎（與 Sheet 公式同邏輯），不登入即可試玩所有編輯功能。
- **行動版**：底部分頁導航；表格橫向捲動（卡片化列入 backlog）。
