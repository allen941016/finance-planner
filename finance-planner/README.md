# 理財目標規劃

用 PV（現值）、FV（終值）、PMT（每期付款）規劃理財目標的靜態網站。設定目標金額、年限與報酬率後，可以看到每月該存多少、目前計畫能走到哪裡、怎麼補上缺口。

純 HTML、CSS、JavaScript，沒有建置步驟，也沒有後端。資料只存在使用者瀏覽器的 localStorage。

## 專案結構

```
finance-planner/
├─ public/
│  ├─ index.html     網站本體
│  └─ _headers       Cloudflare 安全標頭
├─ wrangler.jsonc    Cloudflare Pages 設定
├─ package.json      本機預覽與部署指令
└─ README.md
```

## 本機預覽

```
npm install
npm run dev
```

也可以直接用瀏覽器開啟 `public/index.html`。

## 上傳到 GitHub

```
cd finance-planner
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<你的帳號>/finance-planner.git
git push -u origin main
```

先在 GitHub 網站建立一個空的儲存庫（不要勾選 README 或 .gitignore），再執行上面的指令。

## 部署到 Cloudflare Pages

### 方法一：連接 GitHub（推薦，之後 push 就自動更新）

1. Cloudflare Dashboard，進入 Workers & Pages，選 Create，再選 Pages，然後選 Connect to Git。
2. 選擇剛建立的儲存庫。
3. 設定：
   - Framework preset：None
   - Build command：留空
   - Build output directory：`public`
4. 按 Save and Deploy，完成後會得到 `專案名稱.pages.dev` 網址。

### 方法二：指令直接部署

```
npm install
npx wrangler login
npm run deploy
```

## 自訂網域

部署完成後，在該 Pages 專案的 Custom domains 加入你的網域即可。

## 計算方式

年報酬率以名目利率換算成月複利（月利率 = 年報酬率 ÷ 12），期數以月計。

- PMT = (FV − PV × (1+i)^n) × i ÷ ((1+i)^n − 1)
- FV = PV × (1+i)^n + PMT × ((1+i)^n − 1) ÷ i
- PV = FV ÷ (1+i)^n

每月月初存入（期初）時，年金項目再乘以 (1+i)。目標金額可選擇以今天購買力計算，系統會依通膨率換算成到期時的金額。

## 免責聲明

本工具為教育用試算，不構成投資建議。報酬率與通膨率都是假設值，實際結果會波動，也可能不如預期。
