# 入職第二個月：能獨立做事

## 這個月的核心目標

> 從「靠 mentor 餵任務」進化到「能自己拆解一個 feature」。

第一個月你在 **跟著做**，第二個月你開始 **自己做**。差別在：

- mentor 給你一個句子的需求，你能變出可動工的 task list
- 你會主動找 PM / 後端 / 設計 / SRE 對齊
- 不再等 review 才發現問題，自己 review 自己

---

# 任務範圍升級

第一個月你做的是 **點**（一個欄位、一個按鈕、一個 bug）。

第二個月你要做的是 **小型 feature**：跨層、跨檔、可能跨服務。

## 範例任務（依難度排序）

1. 後台一個列表頁加 export 功能（DB → API → 前端按鈕 → 下載）
2. 新增一個訊息通知設定頁（CRUD + 預覽 + 預設值）
3. 修一個 race condition：兩個請求同時跑會壞，找出 + 寫 test + 修
4. 把一個 endpoint 從同步改成非同步：用 worker / queue
5. 一個現有頁面的 RWD 改造 + 載入效能優化

預期：**每個任務 3 至 10 天**。

---

# Backend 能力目標

## 能設計簡單 API

- 從需求出發決定 endpoint：`POST /api/v1/...` vs `PATCH ...`
- 決定 request / response 結構
- 知道何時該回 4xx、何時 5xx
- 文件化：用 OpenAPI、Markdown、或 Postman collection 任一種

## 能寫複雜 query

- JOIN 多表
- aggregate（`COUNT` + `GROUP BY`）
- 知道哪些 query 該加 index
- 用 `EXPLAIN ANALYZE` 看 query plan

## Migration

- 寫一個可逆 migration（up / down）
- 知道 zero-downtime 的注意事項：
  - 加欄位先放 nullable
  - rename 分兩階段（先加新欄位、寫雙寫、移除舊欄位）
  - 不在尖峰跑大型 backfill

## Transaction & Concurrency

- 知道哪些操作必須在同個 transaction
- 知道 deadlock 怎麼發生、怎麼避免
- 用 `context` 處理超時
- 知道 worker pool / channel 的基本模式

---

# Frontend 能力目標

## Component 設計

- 把 5 個地方重複用到的 UI 抽成 reusable component
- 把 props 設計得有彈性但不複雜（避免 17 個 boolean prop）
- 知道何時用 children、何時用 render prop、何時用 slot pattern

## 表單

- 多步驟表單（wizard）
- 動態欄位（field array）
- 表單之間的依賴（A 改了 → B 變 enabled）
- Zod schema 寫到能複用在 backend validation

## 狀態

- Redux slice 設計：什麼放 slice、什麼放 RTK Query
- Optimistic update（mutation 成功前先改 UI）
- error retry 與重試上限

## 效能

- 知道何時 component 重 render（用 React DevTools Profiler）
- 知道 `useMemo` / `useCallback` 不該到處用
- list 大時用虛擬列表（`react-window`）
- 知道 bundle size 怎麼看（`pnpm build` 後看 dist size）

---

# 協作能力目標

## 設計討論

- 大一點的功能動手前主動寫一份 design doc（哪怕只是 5 段）：
  - 問題是什麼
  - 兩三個方案
  - 選哪個、為什麼
  - 風險與未知數
- 找 mentor / 後端 / 前端 dialog 30 分鐘

## Code Review（被 review）

- 主動找 reviewer，而不是等
- review 留言 24 小時內回應（不能立刻改至少回「收到，明天處理」）
- 同樣的 review 留言不要被講第二次

## Code Review（review 別人）

- 第二月開始，你也會被指派 review 別人的 PR
- 先看 PR 描述，理解意圖再看 code
- 講「為什麼」不只「怎麼改」
- 區分 must / should / nit

## Sprint / 任務管理

- 每個任務切到 1 天內可完成的子任務
- 卡關超過半天主動講
- 估時不準很正常，但要學會「估完之後追蹤」

---

# Stranity 產品理解目標

第二個月結束你應該能跟人解釋：

- Stranity 主要客戶是誰、買的是什麼
- eltrade / gobe / paraninja / sikax 各自是什麼產品
- 你負責的 repo 在整個系統的位置
- 你負責的 domain 主要的 entity 與 flow

不必當專家，但別人問「那個策略匯入是怎麼運作的」你能講出大致流程。

---

# AI 協作進化

第一個月你用 AI 寫 code，第二個月你要用 AI 做更多事：

- 用 AI 做 codebase 探勘（找出影響範圍）
- 用 AI 寫 design doc 草稿
- 用 AI review 自己的 PR（pre-review）
- 用 AI 生 test case
- 用 AI 補沒寫過的 boilerplate（migration、Dockerfile、CI config）

詳見 [`11_ai_agent_workflow.md`](11_ai_agent_workflow.md) 高階技巧段。

---

# 第二個月 Deliverables

- [ ] 完整獨立 ship 一個 feature 到 staging
- [ ] 完整獨立 ship 一個 feature 到 production（小型即可）
- [ ] 至少一次跨人協作（PM / 後端 / 前端 / SRE）
- [ ] 至少一份 design doc / RFC
- [ ] 至少 review 過 5 個別人 PR
- [ ] 開始能對自己負責的模組有發言權

---

# 第二個月常見地雷

- **接太多任務，每個都做一半**：寧可少做、做完
- **不敢說自己卡關**：第一週可以裝沒事，第二個月還這樣 mentor 會擔心
- **review 留言一直被講同一件事**：表示你沒在改習慣，要刻意練習
- **重複造輪子**：先搜 codebase，多半有現成 helper
- **不寫 daily note 了**：覺得自己會了所以懶得寫 — 通常是退步的開始
- **AI 用越多卻越不懂自己 code**：要警覺

---

# 月底 self check

- [ ] 我可以解釋我負責 domain 的 3 個主要 flow
- [ ] 我可以 review 別人 PR 並給出有價值的留言
- [ ] 我可以從 「需求」到 「production」 走完整個流程
- [ ] 我可以估自己任務的時程，誤差在 ±50% 內
- [ ] 我至少寫過 1 份 design doc 或 RFC
- [ ] 我至少修過 1 次 production bug

做不到沒關係，留到下個月。
