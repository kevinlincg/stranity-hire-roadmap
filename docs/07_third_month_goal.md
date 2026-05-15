# 入職第三個月：成為可靠戰力

## 這個月的核心目標

> 從「能做事」進化到「能被信任」。

第二個月你 ship 一個 feature；第三個月 mentor 開始相信你能 **持續 ship**、可以把一塊長期負責的工作交給你。

試用期結束會評估：要不要正式錄用、薪資範圍、長期 ownership 範圍。

---

# 核心轉變：從「執行者」到「擁有者」

## 執行者思維

- 等任務派發
- 完成被交代的事就好
- 出問題等別人指出
- 不知道時等別人說

## 擁有者思維

- 主動找有價值的問題
- 完成「能用」之餘想到「能用很久」
- 出問題自己先發現、自己擔
- 不知道時主動研究、主動問

第三個月你要開始長出第二種思維。

---

# 技術深化

## Backend

### 複雜業務邏輯

- 能處理跨模組的工作流（A 完成 → 觸發 B → B 失敗 retry → 通知 C）
- 知道何時用 event-driven、何時用 RPC、何時用同步
- 知道 idempotency 是什麼、為什麼重要

### Async & 背景任務

- 寫一個背景 worker：consume queue / cron / event
- 知道 retry / backoff / dead-letter queue 的常見 pattern
- 知道 worker 怎麼測試（spin up worker、塞 message、看效果）

### Debug 進化

- 能看 production log（grep、jq、kibana 或公司用的 log 工具）
- 能讀 query plan 並修慢 query
- 能用 profiler / pprof 找 CPU / memory hot spot
- 能用 metric / trace 找跨服務問題

### 安全意識

- 知道 OWASP top 10 至少前 5 個
- input validation、SQL injection、XSS 在你的 code 怎麼處理
- secret rotation、env var 管理、JWT 安全細節

## Frontend

### UI 品質意識

- 對齊、間距、字級你會在意
- loading / error / empty 三狀態都有處理
- 邊界（資料 0 筆、9999 筆、極長字串、極短字串）都試過
- 在 mobile / tablet 至少看過一次

### State 管理進階

- 知道何時該把 state 放 server（RTK Query）vs client（slice）
- 知道 normalize state 是什麼、何時用 entity adapter
- form state、URL state、server state 三者邊界清楚

### Performance

- 大 list 用虛擬列表
- bundle 分析（`rollup-plugin-visualizer`）
- code splitting：lazy route、heavy component
- 圖片 lazy load、適當尺寸

---

# 團隊技能

## Review 別人 PR

- 一週至少 review 3 至 5 個別人 PR
- 留言有質量：抓 bug、提替代寫法、讚美寫得好的地方
- 不再只是「LGTM」

## 任務拆解與估時

- 接到大需求能切成可估時的子任務
- 估時誤差控制在 ±30%
- 估完之後追蹤：今天該做完的有做完嗎？沒做完為什麼？

## 溝通

- standup / sync 講話有重點：「做了什麼」「卡在哪」「需要誰幫忙」
- 寫文件不再是負擔，是預防之後重做的方法
- 跨團隊（PM、設計、後端、前端）有人脈、有信任

## Mentor 新人（如果有）

第三個月如果公司有更新的人進來，你會：

- 帶他 setup
- review 他的 PR
- 接他的問題

被 mentor 跟去 mentor，差別只有「會不會把自己當鏡子」。

---

# Feature Ownership

第三個月開始你會有 **長期負責的模組**。例如：

- 「策略管理」這塊都是你
- 「通知系統」這塊都是你

Ownership 不是「你寫的」，而是：

- 別人改你模組時你會被請來 review
- 出 bug 大家先找你
- 加新 feature 你能 / 應該主導 design
- 模組的長期演進方向是你提的

這是長期工程師的核心價值，不是 IC 等級提升靠多 ship feature，而是靠 **被信任承擔越來越大的範圍**。

---

# 第三個月評估重點

## 技術可靠度

- 寫的 code 經得起 review、上 production 不爆
- 估時 / 估範圍越來越準
- bug 多半是 corner case，不是低級錯誤

## 自主性

- 不必等指示就有事可做
- 卡關自己會研究 + 求救（不只其中之一）
- 學新東西速度上來了

## 影響範圍

- 別人會引用你的 design / 寫法
- review 留言被認真採納
- 模組品質因為你進步

## 心智模型

- 能解釋系統為什麼這樣設計（不只「現在這樣」）
- 能預測：「如果這樣改，未來 X 月後可能會有 Y 問題」
- 知道什麼該深挖、什麼該放掉

---

# 第三個月 Deliverables

- [ ] 至少 1 個中型 feature 從 design 到 production 主導
- [ ] 至少 1 個 production bug 根因分析 + 修復
- [ ] 至少 1 份 RFC / design doc 被採納
- [ ] Review 至少 15 個別人 PR
- [ ] 在 standup / weekly 能講「未來兩週我打算 ...」
- [ ] 開始有一塊「被信任的領域」

---

# 試用期評估 checklist

mentor 與主管會看：

### 技術

- [ ] CRUD API 寫得乾淨
- [ ] 能 debug production 問題
- [ ] 能處理併發、async、transaction
- [ ] 能讀 / 改現有 codebase 不破壞既有結構
- [ ] 能用 AI 但不依賴 AI

### 工程習慣

- [ ] git commit / branch / PR 規矩
- [ ] code review 來去都健康
- [ ] 文件能寫
- [ ] 測試有寫（重點處）

### 軟實力

- [ ] 溝通清楚、不防衛
- [ ] 提問有 context
- [ ] 接 feedback 有改變
- [ ] 看得到團隊不只自己

### 文化

- [ ] 主動、不被動
- [ ] 對品質有自己的標準
- [ ] 對產品有興趣、有意見
- [ ] 願意 mentor 之後的新人

---

# 第三個月以後

到第 4 至 6 個月，期望：

- 你能獨立 own 一個小型 epic（多個 feature 組成）
- 你開始參與技術選型討論
- 你能跨服務 debug
- 你會問「為什麼這個產品要這樣設計」而不只「這個 ticket 怎麼做」

到第 6 個月，你應該不再是「新人」。
