# Frontend Learning Guide (React)

## 這份文件給誰看

你是即將加入 Stranity 的 **Fullstack Engineer**，但 **後端權重比較高**。

實際工作上：

- 多數新功能你會從後端 API 一路做到前端 UI
- 前端不要求做到「設計師等級的精緻度」，但要寫得乾淨、能跑、能被同事接手
- 前端細節（樣式微調、佈局、無障礙、動畫）**鼓勵大量用 AI agent 協助**

## 心態定位

> 後端要扎實，前端能讀懂、改得動、配合 AI 寫得出來就好。
>
> 但你不能完全不懂前端而只 copy AI 的輸出 — 不懂的話 review 不出 bug，AI 寫錯你也看不出來。

換句話說：

- **不需要** 把 React 內部運作機制讀到底
- **不需要** 把 Tailwind 每個 utility 背起來
- **需要** 看得懂 component / props / state / effect 在幹嘛
- **需要** 知道專案結構、知道某個功能該改哪裡
- **需要** 知道怎麼問 AI agent 才能得到對的答案、怎麼 review AI 寫的東西

更多 AI 協作技巧見 [`11_ai_agent_workflow.md`](11_ai_agent_workflow.md)。

---

## 學習總時程建議

純新手（會 HTML/CSS/JS 但沒寫過 React）：

- 全力學：**2 至 3 週**
- 平日下班學：**4 至 6 週**

有其他框架經驗（Vue、Angular、RN）：

- 全力學：**4 至 6 天**
- 平日下班學：**1.5 至 2 週**

目標：入職前能看懂使用此 stack 的 component，並完成 pre-onboarding project 的前端部分（可以借助 AI）。

---

## 環境準備

- **Node.js**：20 LTS 以上（用 `nvm` 或 `fnm`）
- **pnpm** 或 **npm**：團隊兩者都用
- **VS Code** + 以下 extension：
  - ESLint
  - Prettier
  - Tailwind CSS IntelliSense
  - ES7+ React/Redux/React-Native snippets
- **Chrome / Edge** + React Developer Tools

確認你能跑：

```bash
node --version    # >= v20
pnpm --version
```

---

# Stranity 實際前端技術棧

兩個現役主力 repo 的 stack（**新 repo 已不用 Ant Design**）：

| 類別 | 套件 | 用途 |
|------|------|------|
| Framework | **React 18 / 19 + Vite + TypeScript (strict)** | 主框架 |
| Routing | **React Router 6 / 7** | 路由 |
| State | **Redux Toolkit + RTK Query** | 全域 state + API |
| UI | **Tailwind CSS + shadcn/ui (Radix UI)** | 樣式 + 元件 |
| Form | **React Hook Form + Zod** | 表單 + 驗證 |
| HTTP | **axios** | API client |
| Test | **Vitest + React Testing Library + MSW** | 測試 |
| API 型別 | **openapi-typescript** | 從 OpenAPI 產 TS 型別 |
| Chart | recharts / lightweight-charts / echarts | 圖表（看 repo） |
| 動畫 | framer-motion | 動畫（看 repo） |

舊 repo 的 stack：

- 部分舊 admin / 後台仍用 Ant Design
- 入職時可能會碰到，但 **新功能優先用新 stack**

「會 AntD 是加分項，但不是必備」。

---

# 第一階段：TypeScript（3 至 5 天）

## 為什麼重要

Stranity 前端全部 TypeScript strict mode。AI 寫出 `any` 你要看得出來並要求改。

## 必須會

- 基本：`string`、`number`、`boolean`、`null`、`undefined`、`any`、`unknown`、`never`
- 物件：`interface` 與 `type` 的差異（用法上幾乎可換，團隊偏 `type`）
- union / intersection：`A | B`、`A & B`
- 字面值型別：`type Status = 'active' | 'inactive'`
- 泛型：`function pick<T, K extends keyof T>(o: T, k: K[]): Pick<T, K>`
- utility type：`Partial<T>`、`Pick<T,K>`、`Omit<T,K>`、`Record<K,V>`、`ReturnType<T>`、`Awaited<T>`
- type narrowing：`typeof`、`in`、自訂 type predicate（`x is User`）
- `as const`、`satisfies`
- non-null assertion `!`：盡量不用

## AI 協作

寫 TS 時鼓勵讓 AI 把型別補齊，但你要會：

- 看出 AI 用 `any` 偷懶
- 知道 `as Foo` 強轉是逃避型別系統，不是解決
- 把 API response 對到後端實際型別（最好用 codegen，見下方）

---

# 第二階段：React 基礎（5 至 7 天）

## 學習資源

- 官方：[react.dev](https://react.dev/learn) — 必看 "Thinking in React"、"You Might Not Need an Effect"
- 中文：[React 官方文件繁體中文翻譯](https://zh-hant.react.dev/)

## 必須會

- **JSX**：和 HTML 差異（`className`、`htmlFor`、`{}` 表達式）
- **Function component**：別學 class component
- **Props**：傳值、children、`PropsWithChildren<T>`、`ComponentProps<'button'>`
- **State**：`useState`、為何不能 mutate
- **Effect**：`useEffect`、dependency array、cleanup
- **Ref**：`useRef`、DOM ref vs 容器
- **Memo（看狀況用）**：`useMemo`、`useCallback`、`React.memo` — **不要過度用**
- **Custom hook**：把 logic 抽出去
- **Context**：用於整 app 都需要的東西（auth user、theme）

## 你 **不需要** 馬上深入

- Suspense / streaming SSR
- React Server Components
- 自寫 reconciler
- concurrent features 細節

懂概念就好，需要時 AI 會教你。

## 容易踩的坑

- 直接改 state：`state.push(x)` 錯，要 `setState([...state, x])`
- effect dependency 漏掉 — lint 警告認真聽
- 過度 `useEffect` — 很多事該在 event handler 處理
- list 用 `index` 當 key — 改順序會出 bug

---

# 第三階段：Vite 與專案 setup（1 天）

## 必須會

- `pnpm create vite@latest my-app -- --template react-ts`
- `vite.config.ts` 大致看得懂、加 plugin、設 alias
- alias：`@/...` 比 `../../../` 好
- 環境變數：`import.meta.env.VITE_API_URL`（檔名 `.env.local` 不 commit）
- dev proxy：`server.proxy` 把 `/api` 轉到後端避 CORS
- `pnpm build`、`pnpm preview` 流程

入職後直接看公司 production repo 的 `vite.config.ts` 模仿即可。

---

# 第四階段：Tailwind CSS（2 至 3 天）

## 為什麼是 Tailwind 不是 AntD

新 repo 不用 AntD，所有樣式都靠 Tailwind。配 Radix / shadcn 後元件也是自己組。

## 必須會

- utility 思維：不寫 CSS，組合 class
- 常用：spacing（`p-`、`m-`、`gap-`）、flex / grid、text、color、border、rounded
- responsive：`md:`、`lg:`
- state variants：`hover:`、`focus:`、`disabled:`、`dark:`
- `cn()` helper（`clsx` + `tailwind-merge`）：條件式 class 合併
- 在 component 內怎麼接收外部 `className` 與本地 class 合併

## 不必背 utility

開發時用 Tailwind IntelliSense + AI 補就好。**重點是看得懂別人寫的 Tailwind**。

---

# 第五階段：shadcn/ui 與 Radix UI（2 至 3 天）

## 為什麼用這個

shadcn/ui 不是傳統 npm 套件，而是「複製進 src 自己改」的元件集合，底層是 Radix UI（無樣式、有互動邏輯）。

優點：

- 樣式完全自己控制（Tailwind）
- 不會被套件版本綁死
- AI 改起來比 AntD 容易（檔案就在 repo 裡，AI 直接讀）

## 必須會

- 在 shadcn 的設計裡，UI 元件住在 `src/components/ui/`
- 怎麼用 CLI 加新元件（`npx shadcn add button`）
- 看得懂 Radix primitive：Dialog、DropdownMenu、Popover、Select、Tabs、Tooltip 的 trigger / content 結構
- `cva` (class-variance-authority)：variant + size 怎麼定義
- 表單元件怎麼跟 React Hook Form 結合

## 練習

到 shadcn/ui 官網（[ui.shadcn.com](https://ui.shadcn.com/)）讀過幾個元件原始碼，例如 `button`、`dialog`、`select`。

看不懂的部分問 AI。入職後再看公司 repo 怎麼擴充與調整這些元件。

---

# 第六階段：React Hook Form + Zod（2 天）

## 為什麼

Stranity 表單統一用 RHF + Zod。表單驗證寫 schema 就好，型別自動產出。

## 必須會

- `useForm({ resolver: zodResolver(schema) })`
- `register` vs `Controller`：原生 input 用 `register`，包過的元件（Select、DatePicker）用 `Controller`
- `handleSubmit(onSubmit)`
- `watch`、`setValue`、`reset`
- Zod schema：`z.object({ name: z.string().min(1), ... })`
- 從 Zod schema 推導 TS 型別：`type FormValues = z.infer<typeof schema>`

## 練習

寫一個 login form：

- email（required + 格式）
- password（required + 至少 8 字）
- submit 按鈕：loading 中 disabled

---

# 第七階段：Redux Toolkit + RTK Query（3 至 5 天）

## 為什麼是 Redux 不是 Zustand

Stranity 主 repo 用 Redux Toolkit。Toolkit 已經把舊版 Redux 的 boilerplate 砍掉九成，搭配 RTK Query 把「打 API」這件事一併處理。

## Redux Toolkit 基礎

- `configureStore`
- `createSlice`：state、reducer、actions 一起定義
- typed hook：`useAppDispatch`、`useAppSelector`
- store 切 slice 的原則：每個 domain 一個 slice

## RTK Query 基礎

- `createApi({ baseQuery, tagTypes, endpoints })`
- `endpoint`：`query` 與 `mutation`
- 自動產生 hook：`useGetStrategiesQuery`、`useCreateStrategyMutation`
- cache 邏輯：`providesTags` / `invalidatesTags`
- loading / error 狀態都在 hook 回傳值裡

## 練習

在 pre-onboarding project 用 RTK Query 串接 strategy CRUD：

- `strategyApi`：`listStrategies`、`getStrategy`、`createStrategy`、`updateStrategy`、`deleteStrategy`
- mutation 後用 `invalidatesTags(['Strategy'])` 重整 list

---

# 第八階段：React Router 與保護路由（1 天）

## 必須會

- `createBrowserRouter` 或 `<BrowserRouter>` + `<Routes>`
- 動態參數：`/strategies/:id`，`useParams`
- query string：`useSearchParams`
- nested route + `<Outlet />`
- 保護路由：自訂 `<ProtectedRoute>`，未登入導去 `/login`
- programmatic navigation：`useNavigate`

## 參考實作

入職後參考公司前端 repo 的 `ProtectedRoute` 元件實作。

---

# 第九階段：OpenAPI codegen（1 天，選做）

## 為什麼

後端如果有 OpenAPI spec（Stranity 多數新 API 都有），前端可以從 spec 自動產 TS 型別，**API 改了型別也跟著改，無需手動同步**。

## 典型流程

```bash
npm run codegen:download   # 下載 openapi.json
npm run codegen:generate   # 產生 src/types/api.ts
npm run codegen:paths      # 產生 path 常數
```

入職後第一週可能用不到，但要知道有這條路。

---

# 第十階段：測試（2 天，視時間）

## 必須會

- Vitest：`pnpm test`
- React Testing Library：用 user 視角測 UI
- `render`、`screen.getByRole`、`userEvent`
- MSW：mock API，避免真的打網路
- 何時寫測試：複雜 hook、複雜邏輯、bug fix 之後

不必追求高覆蓋率。**「重要的有測就好」**。

---

# 第十一階段：閱讀公司前端 codebase（入職第一週）

入職前不必做。入職第一週 mentor 會帶你看公司主力前端 repo。請花時間觀察：

- `features/` 怎麼按 domain 分
- `store/api/` 的 RTK Query 怎麼定義 endpoint
- `lib/schemas/` 怎麼存 Zod schema
- `components/ui/` 的 shadcn 元件結構

**Stranity 內部慣例優先於本文件**。

---

# 怎麼用 AI 補足前端能力

詳見 [`11_ai_agent_workflow.md`](11_ai_agent_workflow.md)。簡述：

## 推薦的 AI 工具

- **Claude Code** — terminal 直接讀寫 repo，適合「實作整個 feature」
- **Cursor** — IDE 風格、適合一邊看一邊改
- **GitHub Copilot** — 行內自動完成
- **ChatGPT / Claude.ai 網頁版** — 純諮詢、做 design 討論

## 你應該用 AI 做什麼

- 把後端的 OpenAPI / handler 描述貼進去，請 AI 列出對應的前端型別、API 層、UI 草稿
- 把現有 component 貼進去，請 AI 加 dark mode、a11y、loading 狀態
- 卡在 Tailwind class、Radix prop、RHF 用法 — 直接問
- 「為什麼這個 useEffect 跑兩次？」這種 React 機制問題

## 你 **不應該** 用 AI 做什麼

- 不看就 commit AI 寫的 code
- 把 API key、生產資料、客戶資料貼進去
- 問「我整個系統該怎麼架」這種沒上下文的大哉問 — 結果一定很爛

---

# 入職前 self check

- [ ] 不查資料就用 `useState` + `useEffect` 寫一個 component
- [ ] 解釋 `useEffect` 的 cleanup function 何時跑
- [ ] 看得懂 `cn()`、`cva`、Tailwind class merge
- [ ] 用 RHF + Zod 寫一個有驗證的表單
- [ ] 用 RTK Query 寫一個 query + mutation
- [ ] 看得懂 shadcn 的 `button.tsx` 與 `dialog.tsx`
- [ ] 設定 Vite proxy 讓 `/api` 打到本機後端
- [ ] 至少用過一次 Claude Code 或 Cursor 完成一段 feature
- [ ] 完成 [`04_pre_onboarding_project.md`](04_pre_onboarding_project.md) 前端

做不到沒關係，記下做不到的項目，第一天告訴你的 mentor。
