# Frontend Learning Guide (React)

## 這份文件給誰看

你是即將加入 Stranity 的新進工程師，會碰前端：

- 沒寫過 React，但寫過其他 UI（Vue、React Native、Flutter）
- 寫過 React，但都是 class component 或舊版（CRA、Redux saga）
- 寫過練習專案，但沒做過 production 前端

這份文件不是 React 教科書，而是 **入職前你應該長出哪些前端能力** 的清單。

---

## 學習總時程建議

純新手（會 HTML/CSS/JS 但沒寫過 React）：

- 全力學：**3 至 5 週**
- 平日下班學：**6 至 10 週**

有其他框架經驗（Vue、Angular、RN）：

- 全力學：**1.5 至 2 週**
- 平日下班學：**3 至 5 週**

目標一樣：入職前能完成 pre-onboarding project 的前端部分。

---

## 環境準備

入職前請先在本機備好：

- **Node.js**：20 LTS 以上（建議用 `nvm` 或 `fnm` 管理版本）
- **pnpm**：團隊主力 package manager（`npm i -g pnpm`）
- **VS Code** + 以下 extension：
  - ESLint
  - Prettier
  - Tailwind CSS IntelliSense
- **Chrome / Edge** + 以下 devtools extension：
  - React Developer Tools
  - Redux DevTools（雖然我們用 Zustand，但很多舊 reference 會用到）

確認你能跑：

```bash
node --version    # >= v20
pnpm --version
```

---

# 第一階段：TypeScript 基礎（3 至 5 天）

## 為什麼重要

Stranity 前端全部用 TypeScript。寫純 JavaScript 在 review 會直接被打回票。

## 必須會

- 基本型別：`string`、`number`、`boolean`、`null`、`undefined`、`any`、`unknown`、`never`
- 物件型別：`interface` vs `type`，怎麼選？通常 `interface` 給物件、`type` 給 union / mapped
- union 與 intersection：`string | number`、`A & B`
- 字面值型別：`type Status = 'active' | 'inactive'`
- 泛型：`function identity<T>(x: T): T`、`Array<T>`、`Promise<T>`
- utility type：`Partial<T>`、`Pick<T,K>`、`Omit<T,K>`、`Record<K,V>`、`ReturnType<T>`
- type guard：`typeof`、`instanceof`、自訂 `(x: unknown): x is User =>`
- enum vs union literal：**團隊偏好 union literal**
- `as const`：`const colors = ['red', 'blue'] as const`
- non-null assertion `!`：盡量避免，必要時加註解

## 容易踩的坑

- 到處用 `any`：等於放棄 TS。沒辦法時用 `unknown` 並做 type guard
- `as Type` 強轉：跟 `any` 一樣是逃避，會在 runtime 炸
- 把 nullable 當 non-null：API 回來的欄位可能是 `null`，要正確標型

## 練習題

寫一個 `parseUser(input: unknown): User | null`：

- input 是 `unknown`
- 用 type guard 一一檢查欄位（id 是 number、name 是 non-empty string、email 包含 `@`）
- 失敗回 null，成功回 typed `User`

---

# 第二階段：React Fundamentals（1 週）

## 學習資源

- 官方：[react.dev](https://react.dev/learn) — 一定要全看完，特別是 "Thinking in React"
- 中文：[React 官方文件繁體中文翻譯](https://zh-hant.react.dev/)

## 必須會

- **JSX**：和 HTML 的差異（`className`、`htmlFor`、`{}` 嵌入表達式）
- **Component**：function component（不必學 class）
- **Props**：傳值、children、destructure、`PropsWithChildren<T>`
- **State**：`useState`、為什麼不能直接 mutate
- **Effect**：`useEffect`、dependency array、cleanup function
- **Ref**：`useRef`、DOM ref、值的容器（不觸發 rerender）
- **Memo**：`useMemo`、`useCallback`、`React.memo`，但 **不要過度使用**
- **Custom hook**：抽 logic 出去的標準作法
- **Context**：適合「整個 app 都會用」的全域值（theme、currentUser），不適合所有狀態
- **Suspense**、**Error Boundary**：知道存在即可，後面再深入

## 容易踩的坑

- **直接改 state**：`state.push(x)` 是錯的，要 `setState([...state, x])`
- **effect 依賴漏掉**：lint 會警告，認真聽
- **無窮迴圈**：在 effect 內 `setState` 又沒寫 dependency
- **過度 `useEffect`**：很多事情該在 event handler 或 derived state 處理，不該開 effect
- **key 用 index**：list 排序或刪除會出 bug，要用穩定 id

## 練習題

寫一個 Todo App：

- 加 / 刪 / 編輯 / 標記完成
- 用 `useState` + 自訂 hook `useTodos()`
- 篩選器：全部 / 已完成 / 未完成
- 把篩選後的 list 用 `useMemo`
- 完成後刻意把 state 寫成 mutation 形式（`todos.push(...)`），看 bug 怎麼出現，再修正

---

# 第三階段：Vite 與專案 setup（1 至 2 天）

## 為什麼重要

Stranity 前端全部用 Vite，不用 CRA。

## 必須會

- `pnpm create vite@latest my-app -- --template react-ts`
- 設定檔：`vite.config.ts` 怎麼讀
- alias：`'@/components/Button'` 比 `'../../components/Button'` 好
- 環境變數：`import.meta.env.VITE_API_URL`，**檔名要 `.env.local`**，不要 commit
- proxy：dev 環境怎麼把 `/api` 轉到後端，避開 CORS
- build：`pnpm build` 出來的 `dist/` 是什麼

## 練習題

幫你的 Todo App：

- 改用 Vite + TS
- 設定 `@/` alias
- 加一個假環境變數 `VITE_APP_NAME`，在頁面標題顯示
- `pnpm build && pnpm preview` 跑起來

---

# 第四階段：Ant Design（3 至 5 天）

## 為什麼重要

Stranity 後台與部分產品 UI 用 Ant Design 5。新人最常做的就是 CRUD 後台，幾乎都用 AntD。

## 必須會的 component

- Layout：`Layout`、`Menu`、`Breadcrumb`
- Form：`Form`、`Form.Item`、`Input`、`Select`、`DatePicker`、`Switch`
- Form 驗證：`rules`、`validateTrigger`
- Table：`Table` + `columns` 設定、分頁、排序、`rowSelection`
- Modal：`Modal`、`Modal.confirm`
- Notification / Message：`notification.success`、`message.error`
- Button、Tag、Tooltip、Spin、Skeleton

## 必須會的觀念

- **controlled vs uncontrolled form**：AntD `Form` 是 controlled
- `Form.useForm()` 拿 form instance，外部觸發 `form.submit()`、`form.resetFields()`
- `getFieldsValue` / `setFieldsValue`：手動讀寫表單值
- `Table` 的 `pagination` prop：受控分頁的標準寫法
- AntD theme override：用 `ConfigProvider` 改主題色

## 容易踩的坑

- 在 Form 內混用 controlled state（`value={x} onChange={...}`）會打架，要用 `Form.Item` 包
- Table 的 `dataSource` 必須是 `Array`，傳 `undefined` 會炸
- 全域用 `message.error` 之前要 call `App.useApp()` 或包 `<App>`（AntD 5 的新規範）

## 練習題

把 Todo App 改成 AntD 版本：

- Layout 包左側 Menu
- 用 Table 列出 todo（標題、狀態、建立時間、操作）
- 用 Modal + Form 做新增 / 編輯
- 刪除用 `Modal.confirm`
- 操作完成用 `notification.success`

---

# 第五階段：State 管理（Zustand）（2 至 3 天）

## 為什麼重要

簡單的 state 用 `useState` 就好。跨 component 共用的 state 用 Zustand。

**為什麼不用 Redux**：太多 boilerplate。Zustand 是團隊偏好的輕量方案。

## 必須會

- 建 store：`create<State>()((set, get) => ({ ... }))`
- 在 component 用：`const count = useStore((s) => s.count)`
- selector 只訂閱需要的 slice，避免不必要 rerender
- action：直接在 store 內定義
- 持久化：`persist` middleware（搭配 localStorage）
- devtools：`devtools` middleware

## 何時用 Zustand vs 何時用 React Query

- **Zustand**：本地 / client-side state（登入狀態、UI 狀態、theme、表單草稿）
- **TanStack Query**：server state（從 API 拿來的資料，會 cache、會 refetch）

兩者不衝突，一個 app 兩個都用很正常。

## 練習題

把 Todo App 的 todo list 從 `useState` 改成 Zustand store：

- 抽出 `useTodoStore`：state 與 actions
- 多頁面共用（首頁 + 統計頁）
- 加 persist middleware 重整不掉資料

---

# 第六階段：TanStack Query（API 整合）（3 至 5 天）

## 為什麼重要

從後端拿資料、cache、refetch、loading / error 狀態管理。**入職第一個月你會大量用這個套件。**

## 必須會

- `useQuery({ queryKey, queryFn })`：基本讀取
- `queryKey` 設計：陣列形式，包含所有變數（`['todos', { page, keyword }]`）
- `useMutation({ mutationFn, onSuccess })`：寫入操作
- `queryClient.invalidateQueries`：mutation 後刷新 cache
- `enabled`、`staleTime`、`gcTime` 各自意義
- error handling：`isError`、`error`、`useErrorBoundary`
- optimistic update：先改 UI，再等 server 回來

## 必須會：搭配 axios

- 建一個共用 axios instance（base URL、auth header interceptor、error normalizer）
- 統一處理 401（自動刷 refresh token 或踢回 login）

## 容易踩的坑

- **queryKey 沒帶變數**：篩選條件改了卻看到舊資料，因為 cache key 沒變
- **mutation 後沒 invalidate**：UI 沒更新
- **把 query data 放進 useState**：違反 single source of truth
- **每個 component 都 build 自己的 axios instance**：errror handling、auth 會散落

## 練習題

把 Todo App 的儲存從 localStorage 換成你自己寫的後端：

- 共用 axios instance（從 `VITE_API_URL` 讀 base）
- `useTodosQuery({ page, keyword })`
- `useCreateTodoMutation()`、`useUpdateTodoMutation()`、`useDeleteTodoMutation()`
- mutation 成功後 `invalidateQueries(['todos'])`
- loading 用 AntD `Skeleton`、error 用 `notification.error`

---

# 第七階段：TailwindCSS（2 天）

## 為什麼重要

Stranity 部分新專案用 Tailwind。AntD 與 Tailwind 可以共存（AntD 給元件、Tailwind 給排版與微調）。

## 必須會

- utility 思維：不寫 CSS，組合 class
- responsive：`md:`、`lg:` 前綴
- spacing scale：`p-4`、`m-2`、`gap-3`
- flex / grid：`flex`、`grid`、`grid-cols-3`、`items-center`、`justify-between`
- 顏色與字級：`text-gray-500`、`text-sm`
- `@apply` 與 `cn()` helper（`clsx` + `tailwind-merge`）

## 容易踩的坑

- 一堆 class 排成一行 → 用 `cn()` + 條件分組
- 重複 class → 包成 component，不要 `@apply` 一大坨

## 練習題

把 Todo App 的版面用 Tailwind 排：

- header / sidebar / content 三欄
- 響應式：手機版收成單欄
- 主色 / 邊框 / spacing 都用 Tailwind class

---

# 第八階段：Routing 與 Form（2 天）

## React Router

- `createBrowserRouter` 或 `<BrowserRouter>` + `<Routes>`
- 動態參數：`/users/:id`，用 `useParams`
- query string：用 `useSearchParams`
- nested route + `<Outlet />`
- 保護路由：自訂 `<RequireAuth>` 包 layout

## React Hook Form（選讀）

AntD `Form` 已經夠用，但複雜表單（巢狀、動態欄位、跨 step）建議學 RHF：

- `useForm`、`register`、`handleSubmit`、`watch`
- 與 zod 搭配做 schema validation

---

# 第九階段：閱讀 Stranity 前端 codebase（入職第一週）

入職前不必做，但入職第一週請開：

- `~/stranity/react-paraninja/` — 主前端專案，看 component 組織
- `~/stranity/react-eltryn/`、`~/stranity/react-eltryn-admin/` — 另兩個前端參考
- `~/stranity/react-panel-customer/`、`~/stranity/react-panelweb/` — Panel 系列前端

觀察：

- component 與 page 怎麼分目錄
- API 層怎麼包（axios? 自己包？）
- 全域 state 用什麼方式管
- AntD theme 怎麼設

**Stranity 內部慣例優先於本文件**。

---

# 入職前 self check

入職前一週，請確認自己能：

- [ ] 不查資料就用 `useState` + `useEffect` 寫一個 component
- [ ] 解釋 `useEffect` 的 cleanup function 何時跑
- [ ] 寫一個 AntD Form 含驗證、loading 狀態、submit 後 reset
- [ ] 用 `useQuery` + `useMutation` 串接一個 CRUD
- [ ] 解釋為什麼 `useState` 不能直接 mutate 陣列
- [ ] 設定 Vite proxy 讓 `/api` 打到本機後端
- [ ] 用 React DevTools 看自己寫的 component tree
- [ ] 完成 [`04_pre_onboarding_project.md`](04_pre_onboarding_project.md) 的前端

做不到沒關係，記下做不到的項目，第一天告訴你的 mentor。
