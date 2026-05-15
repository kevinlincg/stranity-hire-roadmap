# 入職第一個月：站穩腳跟

## 這個月的核心目標

> 不是「做出大功能」，是「能在這裡工作」。

具體來說：

- 能在本機跑起一個 Stranity 主力服務
- 看得懂 / 改得動現有 code
- 能 debug 並修小型 bug
- 能交出小型 PR 並過 review
- 知道誰負責什麼、誰問什麼會回

---

# 第一週：環境與認人

## 工作日 1（Day 1）

### 早上

- 收到帳號（GitHub、Slack / Discord、公司 mail、VPN）
- 跟 mentor 約一次 1:1（半小時）
- mentor 會帶你看：
  - 公司大致產品線（eltrade / gobe / paraninja / sikax）
  - 你會負責的主要 repo
  - 平常上下班節奏、會議、線上工具

### 下午

- 把開發機弄好：
  - clone 你要負責的 repo
  - 跑 `make dev` / `docker compose up` / 該 repo 的啟動指令
  - 連得到 staging DB（如果有）
- 把 `CLAUDE.md` / `AGENTS.md` / `README.md` 讀完
- 在 issue tracker / Notion 找一個「good first issue」標記的任務（或請 mentor 指派）

### 下班前

- 寫第 1 天 daily note：
  - 我裝起什麼
  - 我卡在哪
  - 我問了誰、誰回了我什麼

---

## 工作日 2 至 5（Day 2 至 Day 5）

### 任務目標

- 完成 1 至 2 個 **微型 PR**：
  - 改錯字 / 文件
  - 補 i18n
  - 加一個 console log
  - 修一個顯而易見的小 bug（例如：日期格式不對）

### 學習目標

- **讀程式**：每天花至少一小時讀 repo。順序：
  1. 路由表（`router.go` / `routes.tsx`）
  2. 認證流程（login → token → middleware → handler）
  3. 主要 domain 模組（例如 strategy、order）
- 用 AI 協助理解：把不懂的檔案貼給 AI，請它「逐行解釋這檔案的作用」
- 開始建 **個人術語表**：產品內部術語、簡寫、人名都記下來

### 文化目標

- 主動參加 standup / weekly sync
- 在頻道發第一則訊息（自我介紹、或問個問題都好）
- 跟前後端、PM 各自 1:1 至少一次

---

# 第二週：第一個有意義的 PR

## 任務範圍

由 mentor 指派一個 **小型功能或 bug fix**，例如：

- 後台多加一個欄位（DB → API → 前端表單與顯示）
- 一個現有列表加一個篩選條件
- 一個現有頁面補上空資料狀態
- 修一個從上週遺留下來的 bug

預期：**1 至 4 天能完成**。

## 工作流程（盡量遵守）

1. **理解需求**
   - 用兩三句話寫下你以為的需求
   - 找 mentor / PM 確認，不確定的列出來問
2. **看程式**
   - 找出要改的檔案範圍
   - 跟 mentor 對齊「打算改哪些檔」
3. **動手**
   - 開分支：`feature/xxx` 或 `fix/xxx`
   - 小步 commit，每段都跑得起來
   - 允許用 AI 協助（見 [`11_ai_agent_workflow.md`](11_ai_agent_workflow.md)）
4. **自己 review**
   - 跑 lint、test、build
   - 自己看一次 diff，刪沒用的 console.log、TODO、註解
5. **開 PR**
   - 描述：改了什麼、為什麼、怎麼測試、有沒有 screenshot
   - tag mentor
6. **接 review**
   - 不防衛、不爭辯
   - 不懂就問「你的意思是 X 嗎？」
   - 改完 push 新 commit，不 force push

---

# 第三、四週：站穩腳跟

## 任務目標

- 接 2 至 3 個小型任務（每個 2 至 5 天）
- 至少有一個任務 **完整經歷**：需求討論 → 設計 → 實作 → review → merge → staging 驗證

## 技術目標

### Backend

- 能讀一個 API 的 request flow（router → middleware → handler → service → repository → DB）
- 能寫 simple CRUD endpoint
- 能 trace 一個 bug：log 看哪、Postman 怎麼打、DB 怎麼 query
- 知道 transaction 該在哪層包

### Frontend

- 能讀一個頁面的 data flow（route → RTK Query → component → store）
- 能加 / 改 form 欄位（RHF + Zod）
- 能修 UI bug：開 devtools、看 network、看 console
- 知道何時要打開 React Profiler

---

# 一個月內 **不建議** 一開始就碰的東西

- **trading engine / order matching**：影響交易，出 bug 是真錢
- **WebSocket core / pub-sub**：concurrency 複雜
- **架構大改 / 跨服務重構**：先理解才能動
- **DB schema 大改 / 不可逆 migration**：先看別人改過幾次
- **production 部署、CI 變更**：先觀察流程

這些不是「永遠不能碰」，是「不要在 onboarding 階段第一個碰」。

---

# 第一個月評估重點

## 1. Debug 能力

是否會：

- 看 log 找原因（不是只看 error message）
- 用工具：`curl`、Postman、devtools network、`psql`
- reproduce issue 寫成步驟
- 用二分法縮小範圍（前後端誰錯？哪段 commit 開始錯？）

## 2. 問問題能力

好問題的長相：

> 「我在做 X。看到 Y 現象。我預期是 Z。
> 我已經試了 A、B，A 結果是 ...，B 結果是 ...
> 我懷疑是 C，但不確定。請問你會建議從哪先看？」

爛問題的長相：

> 「不能用，求救」
> 「為什麼這樣？」（沒貼錯誤、沒貼 code）
> 「在嗎」

## 3. Code Quality

是否：

- 命名清楚（不只是「能跑」）
- 結構與現有 pattern 一致
- 有處理錯誤、邊界
- commit message 看得懂

## 4. 文化適應

是否：

- 早上有開 Slack、能找得到人
- 卡關超過 1 至 2 小時主動求救
- daily note 有持續寫
- 接到 review 不拖延、不防衛

---

# 第一個月 Deliverables

希望你能完成：

- [ ] 至少 3 至 5 個小型 PR merge 進 main
- [ ] 至少 1 個小功能從需求到 staging 驗證
- [ ] 至少 1 次線上 / 客戶 bug 的初步調查
- [ ] 開始能在 standup 講出本週做了什麼、卡在哪
- [ ] 寫了至少 4 週的 daily note

---

# 對 mentor 的提醒

如果你是 mentor，第一個月你要做的事：

- 第 1 天親自帶 setup 環境，至少跑通一次
- 每週至少一次 1:1，重點問「你卡在哪」與「我哪裡幫得上忙」
- 任務派發要 **明確、可完成、可驗收**，不要「你看著做」
- review PR 要 **及時、具體、有建設性**
- 主動分享你怎麼想一個問題（讓他學你思考方式，不只是答案）

新人 onboarding 卡關，多半是 mentor 沒投資夠時間，不是新人不努力。
