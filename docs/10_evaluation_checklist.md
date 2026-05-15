# 評估 Checklist

## 用法

這份 checklist 給 **mentor 與新人雙方** 用：

- **新人**：對著清單自評，知道自己在哪、要往哪
- **mentor**：每月底 1:1 對著清單一起看，標出強項與成長點

不是「全打勾才算合格」。是 **用來對話的工具**，不是考卷。

每項用三段評分：

- ✅ 穩定做到
- 🔶 偶爾或部分做到
- ⬜ 還沒到

---

# Pre-onboarding（入職前）

## 環境與工具

- [ ] 本機能跑 Go、Node 20+、Docker、pnpm
- [ ] Git 基本操作（branch、commit、PR、conflict）熟練
- [ ] 至少用過一次 Claude Code / Cursor 完成有意義的 task

## Pre-onboarding project（[`04_pre_onboarding_project.md`](04_pre_onboarding_project.md)）

- [ ] 專案能用 `docker compose up` 啟動
- [ ] README 寫得讓人能複製貼上跑起來
- [ ] 認證（JWT）功能正常
- [ ] Strategy CRUD 完整（含分頁、篩選、權限）
- [ ] 統一 response 格式
- [ ] git commit 看得懂、一個 commit 一件事
- [ ] 已知限制 / 沒做完的部分有寫在 README

## 學習自評

- [ ] 解釋 JWT 與 refresh token 為什麼存在
- [ ] 解釋 transaction 何時必須包
- [ ] 解釋 useEffect 的 cleanup 何時跑
- [ ] 解釋 RTK Query 的 cache invalidation 機制
- [ ] 知道 AntD 已是 legacy、Stranity 新 repo 用 shadcn

---

# 第一個月

## 技術

### Backend

- [ ] 能讀懂一個 API 的 request flow（router → middleware → handler → service → repository → DB）
- [ ] 能寫簡單 CRUD endpoint
- [ ] 能 debug：看 log、用 curl / Postman、用 psql
- [ ] 知道 transaction 該包在哪層
- [ ] 知道 context 怎麼傳

### Frontend

- [ ] 能讀懂一個頁面的 data flow
- [ ] 能加 / 改 form 欄位（RHF + Zod）
- [ ] 能修 UI bug：開 devtools、看 network、看 console
- [ ] 看得懂 shadcn 元件
- [ ] 用 RTK Query 或 axios 串接過 API

## 工程習慣

- [ ] git commit message 沒出現 `update` / `fix bug`
- [ ] PR 描述至少包含「改了什麼、為什麼、怎麼測試」
- [ ] 跑 lint / test / build 才開 PR
- [ ] review 留言 24 小時內回應

## 文化

- [ ] 主動參加 standup
- [ ] 卡關 1 至 2 小時內找人問
- [ ] 寫 daily note
- [ ] 對 mentor 1:1 有準備（不是空著等）

## Deliverables

- [ ] 至少 3 至 5 個 PR merge 進 main
- [ ] 至少 1 個小功能從需求到 staging 驗證
- [ ] 至少 1 次 bug 初步調查

---

# 第二個月

## 技術

### Backend

- [ ] 能設計簡單 API（endpoint、payload、status code）
- [ ] 能寫多表 JOIN 與 aggregate
- [ ] 能寫可逆 migration、知道 zero-downtime 注意事項
- [ ] 能用 `EXPLAIN ANALYZE` 看 query plan
- [ ] 能寫一個簡單 worker（cron / queue consumer）

### Frontend

- [ ] 能設計可重用 component
- [ ] 能處理多步驟表單、動態欄位
- [ ] 能設計 Redux slice 與 RTK Query endpoint
- [ ] 知道何時 component 重 render，能用 React DevTools Profiler

## 工程習慣

- [ ] 動手前寫一句兩句話的設計確認貼給 mentor
- [ ] 主動找 reviewer，不被動等
- [ ] 同樣的 review 留言不被講第二次
- [ ] 任務估時，每個子任務 1 天內可完成

## 協作

- [ ] 跟 PM / 設計 / 後端 / 前端 各自合作過至少一次
- [ ] Review 過至少 5 個別人 PR，留言有質量
- [ ] 寫過至少一份 design doc / RFC

## 產品理解

- [ ] 能解釋 eltrade / gobe / paraninja / sikax 各自是什麼
- [ ] 能解釋自己負責 repo 在系統的位置
- [ ] 能解釋負責 domain 的 3 個主要 flow

## Deliverables

- [ ] 至少 1 個 feature 從需求到 staging
- [ ] 至少 1 個 feature 到 production
- [ ] 至少 1 份 design doc

---

# 第三個月

## 技術

### Backend

- [ ] 能處理複雜業務邏輯（多模組工作流）
- [ ] 能寫 / 維護背景 worker、event-driven 流程
- [ ] 能讀 production log 並追問題
- [ ] 用過 profiler / pprof 找 hot spot
- [ ] 安全：知道 OWASP top 5、知道 secret 管理

### Frontend

- [ ] UI 三狀態（loading / error / empty）反射動作會處理
- [ ] 能用虛擬列表處理大 list
- [ ] 看得懂 bundle 大小、用過 code splitting
- [ ] 有 RWD / a11y / 效能意識（不必精通）

## 自主性

- [ ] 不需要被派任務也有事做（補測試、refactor、寫文件）
- [ ] 卡關自己研究 + 求救（不只一邊）
- [ ] 學新東西速度比第一個月快很多

## 影響範圍

- [ ] Review 留言被別人引用
- [ ] 有「被信任的領域」
- [ ] 能 mentor 之後的新人（如果有）

## Deliverables

- [ ] 至少 1 個中型 feature 主導（design → production）
- [ ] 至少 1 個 production bug 根因分析
- [ ] 至少 1 份 RFC 被採納
- [ ] Review 至少 15 個別人 PR

---

# 三個月後總評（試用期評估）

## 技術可靠度

- [ ] code 經得起 review、上 production 不爆
- [ ] bug 多半是 corner case，不是低級錯誤
- [ ] 估時 / 估範圍漸準

## 工程習慣

- [ ] git / PR / review 流程都健康
- [ ] 文件能寫
- [ ] 測試該寫的有寫

## 軟實力

- [ ] 溝通清楚、不防衛
- [ ] 提問有 context
- [ ] 接 feedback 有改變

## 文化匹配

- [ ] 主動、不被動
- [ ] 對品質有自己的標準
- [ ] 對產品有興趣、有意見
- [ ] 願意 mentor 之後的新人

---

# 紅旗清單（要警覺的訊號）

任一出現都該認真討論：

## 紅旗 1：消失型

- 一週沒在 Slack 出聲
- standup / 1:1 缺席不打招呼
- PR 留言不回

## 紅旗 2：防衛型

- review 留言每條都辯
- 出 bug 第一反應怪別人 / 怪 framework / 怪 AI
- mentor 一講話就解釋自己沒錯

## 紅旗 3：撞牆型

- 同樣的 review 留言被講第三次以上
- 同樣的 bug 模式出現第三次以上
- 三個月仍不能獨立完成第一個月的任務

## 紅旗 4：AI 依賴型

- 任何問題第一動作問 AI（不先思考）
- 看不懂自己 commit 的 code
- 不會解釋為什麼這樣寫

## 紅旗 5：失溫型

- 對工作沒情緒、好壞都沒反應
- 從不主動分享 / 提問 / 提案
- 任務完成度可以但人不在現場

紅旗不代表不適合，代表需要對話。**早點講，比拖到試用期結束才講好很多。**

---

# 給 mentor 的提醒

- 不要靜默累積：每月用這份清單跟新人對一次
- 把 ⬜ 列出來不是責備，是「我們下個月一起改善」
- ✅ 多的地方要強化、給更多挑戰，否則他會覺得無聊離開
- 三個月評不過不是「他不適合」，常常是「我們沒帶好」

---

# 給新人的提醒

- 別把這份清單當考卷
- ⬜ 不丟臉，知道自己 ⬜ 在哪才是強項
- 三個月學不會的事，半年會
- 半年學不會的事，一年會
- 學不會但 **願意繼續學** 比 **裝懂** 強一萬倍
