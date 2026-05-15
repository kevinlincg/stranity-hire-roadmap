# 與 AI Agent 協作的方式

## 為什麼需要這份文件

2024 年之後，工程師「會不會用 AI」的差距已經比「會不會某個框架」還大。

Stranity 的全端工程師偏後端，前端會大量依賴 AI agent。但 **AI 不是魔法**，用不好寫出來的 code 又長又錯，review 跟 debug 都比自己寫還累。

這份文件幫你建立 **「會用 AI、但 code 你負責」** 的工作習慣。

---

# 工具光譜

不同情境用不同工具：

| 工具 | 適合的情境 | 不適合 |
|------|----------|-------|
| **Claude Code** | 跨多檔讀寫、整個 feature、refactor、bug 調查 | 純 chat、無檔案的 brainstorm |
| **Cursor** | 一邊看 code 一邊改、行內 edit、autocomplete | 大型重構（context window 較小） |
| **GitHub Copilot** | 行內補全、寫 boilerplate、寫 test | 整體設計問題 |
| **Claude.ai / ChatGPT 網頁版** | 設計討論、寫文件、不需要實際碰 repo | 直接動 production code |

不熟的話，**從 Claude Code 開始**：

```bash
# 安裝
npm install -g @anthropic-ai/claude-code

# 在專案根目錄啟動
cd your-repo
claude
```

---

# Stranity 內的 AI 慣例

公司主力 repo 多半都有：

- `CLAUDE.md` — 給 Claude Code 的專案指南
- `AGENTS.md` — 給所有 AI agent 的指南（OpenAI Codex CLI、Cursor 等）

入職第一週請讀完你會碰到的 repo 的 `CLAUDE.md` / `AGENTS.md`。**這些文件已經告訴 AI 該注意什麼**，你就不必每次都重貼一遍。

入職前可以先看公開的 [Anthropic CLAUDE.md 範例](https://docs.claude.com/en/docs/claude-code/memory) 與各家 OSS repo 的 `AGENTS.md` 認識這個格式。

---

# 寫 prompt 的基本心法

## 1. 給 AI 與你同等的 context

你不會丟「幫我寫一個功能」給人類同事就走人，AI 也一樣。

**爛 prompt：**

> 幫我寫一個 strategy list

**好 prompt：**

> 在 `src/features/strategy/` 加 `StrategyListPage.tsx`。
> 用 RTK Query 的 `useListStrategiesQuery({ page, keyword, status })`。
> Table 用 `<table>` + Tailwind 自刻，欄位：name、symbol、status、created_at、操作。
> 操作欄包含 edit（開 dialog）與 delete（用 `AlertDialog` 確認）。
> Loading 狀態用 skeleton（參考 `src/components/ui/skeleton.tsx`）。
> 空資料顯示 "尚無策略"。
> 排版參考 `src/features/backtest/BacktestListPage.tsx`。

第二種 prompt 給 AI 三個有用的東西：

- **位置**：要放哪
- **資料來源**：要用哪個 hook
- **範例**：要模仿哪個現有檔案

## 2. 講清楚要什麼，更要講清楚不要什麼

**好 prompt 通常會講：**

- 「不要新增套件」
- 「不要動 store 設定」
- 「保留現有 `useAuth` 的呼叫方式」
- 「不要寫註解」

否則 AI 容易亂改、亂裝套件、過度註解。

## 3. 一次一件事

別丟「幫我做 login、register、reset password、OAuth、2FA」。

每個任務做完看一次 diff、確認沒問題、再下一個。

## 4. 要求 AI 先列計畫再動工

對大一點的改動：

> 在動 code 之前，先列出你打算改哪些檔案、各檔的變動摘要、有什麼風險。等我確認再開始。

這招在 Claude Code / Cursor 都很有效，可以省下「寫一堆才發現方向錯」的時間。

---

# 工作流：從接到任務到提交 PR

## Step 1：理解需求

- AI 不能取代「你跟 PM / mentor 確認需求」這一步
- 你看不懂的需求，AI 看了也猜
- 先把需求自己用一兩段話寫下來、貼給 mentor 確認

## Step 2：請 AI 探勘 codebase

```
請你看一下 src/features/auth/ 與 src/store/api/，
告訴我新增「忘記密碼」流程要碰哪些檔、現有 pattern 是什麼。
不要動 code，只回報。
```

這一步省掉你自己讀整個 repo 的時間，但 AI 可能漏看，記得自己也掃過一次。

## Step 3：請 AI 提案

```
基於上面的調查，請列出兩個實作方向，
各自的 file change、優缺點、與現有 pattern 的契合度。
```

選一個方向。如果都不對勁，跟 AI 說「兩個都不對，我比較希望 X」。

## Step 4：請 AI 實作（分段）

不要一次叫 AI 寫完。分成幾段：

1. 加好型別 / Zod schema / API endpoint
2. 加好 UI 元件骨架
3. 接好資料流
4. 補 loading / error / empty state

每段做完看 diff、跑起來、再進下一段。

## Step 5：自己 review

AI 寫的 code 必須通過你自己的 review 才能 commit。常見要抓的問題：

- TS 是否用了 `any`、`as Foo` 強轉
- effect dependency 是否漏掉
- error 是否被 swallow（`catch {}`）
- API path / payload 是否真的跟後端對
- naming 是否符合 repo 風格
- 有沒有多裝沒必要的套件
- 有沒有改到你沒要改的檔案

## Step 6：跑 lint / test

```bash
pnpm lint
pnpm test
pnpm build
```

跑不過的話讓 AI 改；改不掉的話自己接手。

## Step 7：寫 commit message

commit message **自己寫**。AI 寫的常常是「Add login feature」這種廢話。

按 [`03_git_and_engineering.md`](03_git_and_engineering.md) 的格式：

```
feat: add forgot password flow

User can request a reset link via /api/v1/auth/forgot-password.
Backend implementation pending; this PR wires up the UI and form
validation only. See ticket STN-128.
```

---

# 常見地雷

## 地雷 1：AI 幻覺套件

AI 會說「裝 `@stranity/ui` 就好」— 結果這個套件根本不存在。

**對策**：

- 看到 import 不認識，先 `cat package.json | grep <pkg>` 確認
- AI 建議裝新套件時問「為什麼？現有套件不行嗎？」
- 不要無腦 `pnpm add`

## 地雷 2：AI 改到不該改的檔案

AI 為了「順便」會改一堆檔案。`git diff` 一看：哇 30 個檔案。

**對策**：

- 在 prompt 寫「只改 X、Y 兩個檔案」
- 每段做完用 `git status` 看範圍
- 超出範圍的變動先 `git restore`

## 地雷 3：AI 把錯誤吞掉

```typescript
try {
  await doSomething();
} catch (e) {
  console.log(e);
}
```

**對策**：

- review 時搜 `catch` 看每個有沒有正確處理（rethrow、show toast、log + 設 state）
- AI 有時為了「通過編譯」會塞 catch 把問題壓下去

## 地雷 4：AI 把舊 pattern 換成它喜歡的新 pattern

你只想改一行，結果 AI 順手把整個檔案改成「比較好的寫法」。

**對策**：

- prompt 明確說「最小修改、保留現有風格」
- review diff 時注意：與這次任務無關的變動全刪

## 地雷 5：你開始相信 AI 多過相信自己

AI 跟你說「這樣可以」，你沒驗證就 commit；跑起來才發現是錯的。

**對策**：

- 重要邏輯一定自己跑過、看過、想過
- 「AI 說可以」不是 reviewer 接受的理由
- 你拿薪水，不是 AI

---

# 高階技巧

## 1. 把 spec / OpenAPI 直接餵給 AI

如果後端有 OpenAPI，整份 `openapi.json` 貼給 AI：「請對應這份 spec 產生 TS 型別與 RTK Query endpoint」。

不過 Stranity 多半已經有 codegen pipeline（見 [`02_frontend_learning.md`](02_frontend_learning.md) 第九階段），那條更穩。

## 2. 用 AI 寫 / 補 test

```
請看 src/lib/utils.ts 的 cn() 與 formatCurrency()，
用 Vitest 補 unit test，邊界條件要 cover：
- 空字串
- 負數
- 不同 locale
```

test 是 AI 最有發揮的場域之一，因為輸入輸出明確。

## 3. 用 AI 做 code review

開 PR 前先讓 AI 自己看一遍：

```
這是我這次的 diff。請以一個資深工程師的角度找出：
- 潛在 bug
- 命名問題
- 可以拆出去的重複邏輯
- 沒處理的邊界
請列具體 file:line。
```

當作 mentor 前的暖身。**不能取代真人 review**。

## 4. 用 AI 寫 commit message 草稿

```
這是我這次的 git diff，請依下面的 commit convention 草擬訊息：
- type: feat / fix / refactor / docs / chore
- subject 不超過 60 字
- body 描述為什麼，不是做了什麼
```

草稿讓你改一改就能用，比從零寫快。

## 5. 用 AI 整理你的學習筆記

讀完 RTK Query 文件，回頭問 AI：

```
我打算下午寫一個 strategyApi，請按下面的需求生成骨架，
然後幫我列我可能不知道的概念（providesTags / invalidatesTags 的細節）。
```

學中做、做中學。

---

# 不該交給 AI 的事

## 設計決策

「我們的 strategy schema 該加 `version` 嗎？」這種跨產品決策必須自己做（或跟 mentor / PM 討論）。AI 給的答案常常太教科書、沒考慮商業 context。

## 跟人類溝通

PM 提的需求要回問、Mentor review 的留言要回應、團隊在 Slack 討論 — **全部你自己來**。

別把 AI 寫的長回覆直接貼給 mentor。一聽就知道。

## 安全敏感操作

- 改 production env / 改 IAM / 改 DNS
- 處理用戶資料
- 寫密鑰、JWT secret、API key

這些事情你自己也要小心，AI 更不該主導。

---

# 自我培養：每月一次 AI 工作回顧

每個月花 30 分鐘問自己：

- 這個月有哪些任務「AI 幫了大忙」？
- 有哪些任務「AI 反而拖慢我」？
- 有哪些任務 AI 寫出來的 code 被 review 打回票？為什麼？
- 我有哪些 prompt 寫得特別好，可以變成範本？

寫進 [`Learning Notes`](../docs/) 或自己的個人筆記。三個月後回頭看，你的工作方式會變很多。

---

# 入職前 self check

- [ ] 安裝過 Claude Code 或 Cursor，跑過至少 3 次「給 prompt → 看 diff → 接受 / 拒絕」
- [ ] 看過 Stranity 一個主力 repo 的 `CLAUDE.md` 或 `AGENTS.md`
- [ ] 知道為什麼 AI 寫的 code 不能直接 commit
- [ ] 知道怎麼讓 AI「先列計畫，再動工」
- [ ] 知道遇到 AI 幻覺套件怎麼處理
- [ ] 至少用 AI 完成一個 pre-onboarding project 裡的 feature

做不到沒關係，記下做不到的項目，第一天告訴你的 mentor。
