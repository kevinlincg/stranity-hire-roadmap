# Debug 指南

## 心法

> Debug 是 **科學方法的應用**，不是運氣或天賦。

流程：

1. 觀察現象
2. 形成假設
3. 設計實驗驗證
4. 看結果（不是看你以為的結果）
5. 縮小範圍
6. 修

**不該做的事：**

- 用直覺猜、改了再說
- 同時改 5 個地方
- 看到 error 訊息就 google + 貼第一個解法
- 「重啟試試看」當解法（重啟是 workaround，不是 fix）

---

# 通用 Debug 步驟

## Step 1：穩定重現

最重要的一步。沒法重現的 bug 不能修。

- 寫下完整的「我做了什麼 → 看到什麼」
- 在本機重現
- 不能本機重現的話，先想差別在哪：資料？權限？版本？並發？

## Step 2：縮小範圍

二分法：

- 是前端錯還是後端錯？打開 Network tab 看 request / response
- 後端是 handler 錯還是 service 錯？看 log、加 log
- 是哪段 commit 開始壞？`git bisect`
- 是哪個 user / 資料壞？挑乾淨資料試

## Step 3：形成假設

**假設要可驗證**：

❌ 「應該是 timeout 吧」
✅ 「我推測是因為 DB connection pool 滿了。如果我把 max_open_conns 從 10 改成 100，問題會消失」

## Step 4：實驗

一次只動一個變因。改了之後重現一次，看結果有沒有變。

## Step 5：找根因

「拿掉某行就好了」不是根因。**為什麼那行影響了現象？**

5 個為什麼（5 Whys）：

> 現象：頁面 loading 卡住。
> 為什麼？前端等 API 沒等到。
> 為什麼？API 回 504 timeout。
> 為什麼？DB query 跑超過 30 秒。
> 為什麼？沒有 index，scan 1000 萬筆。
> 為什麼？migration 加欄位時沒一起加 index。
> 根因：migration 遺漏 index。

---

# Backend Debug 工具箱

## Log

### 看 log 的習慣

- 從錯誤訊息往前看 5 至 10 行
- 找 request id 跨服務追
- 看 timestamp，注意是否並發
- 在 staging：直接 `kubectl logs` / `docker logs` / 公司 log 工具
- 在 production：用 log 平台（Kibana、Datadog、公司用的）

### 寫 log 的習慣

```go
log.Info().
    Str("user_id", uid).
    Str("strategy_id", sid).
    Msg("strategy created")
```

- 結構化（zerolog / zap），不要 `fmt.Println`
- 帶有用的 context（user_id、strategy_id、request_id）
- 不要 log 密碼、token、PII
- 同個事件不要重複 log（呼叫鏈每層都 log = 噪音）

## 工具

| 工具 | 用途 |
|------|------|
| `curl` / `httpie` | 直接打 API |
| Postman / Insomnia | 圖形化 API 測試、收 collection |
| `psql` / `dbeaver` / `tableplus` | 連 DB 看資料 |
| `EXPLAIN ANALYZE` | 看 query plan |
| `pprof` | Go profiler，找 CPU / memory hot spot |
| `go test -race` | 抓 race condition |
| `delve` (`dlv`) | Go debugger，下中斷點 |
| `lsof -i :8080` | 看哪個 process 佔 port |
| `tcpdump` / Wireshark | 抓封包（罕見場景） |

## 常見 Backend 問題模式

### 1. nil pointer dereference

```
panic: runtime error: invalid memory address or nil pointer dereference
```

- 看 stack trace 哪行
- 八成是某個 `db.First(&x, id)` 沒檢查 err 就用 x

### 2. 「我新增了但拿不到」

- 沒包 transaction，A 寫完 commit 前 B 已經查
- AutoMigrate 沒跑
- 連到不一樣的 DB（環境變數 / connection string）

### 3. N+1 query

- 列表很慢
- DB log 看到 1 + 1000 個 query
- 修法：`Preload`、`JOIN`

### 4. Goroutine leak

- 服務記憶體一直漲、不釋放
- `pprof goroutine` 看 goroutine 數
- 找 「起了卻沒辦法結束」 的 goroutine

### 5. Race condition

- 「偶爾」會發生的 bug
- 跑 test 加 `-race`
- 共享 state 沒鎖、map 並發寫入

### 6. JWT 過期 / 失效

- 401 但帳號明明剛 login
- 看 token expiry vs 服務器時鐘
- secret 換了沒？

### 7. CORS

- 前端打 API 看到 `blocked by CORS`
- middleware 設了但 `Allowed-Origin` 不對
- preflight (OPTIONS) 沒被 router 接到

---

# Frontend Debug 工具箱

## DevTools

### Elements

- 看 DOM 結構是否正確
- 看 class 是否套用
- 改 CSS 即時試（不會存）

### Console

- `console.log` 是常用工具，但別留在 commit
- `console.table(array)` 看 array of object 比 log 漂亮
- 看 React 警告：key 重複、effect dependency、hydration mismatch

### Network

- 看 request URL、method、payload
- 看 response status、body
- 看 timing：DNS / TLS / TTFB / Content
- 篩 XHR / Fetch / WS 分類看

### Application

- LocalStorage / SessionStorage 看 token 存了沒
- Cookie 看 server 設了沒、過期沒、SameSite 對不對

### Performance

- 錄一段 interaction
- 看 main thread 哪段忙
- 看 long task

### React DevTools

- Components：看每個 component 的 props / state / hooks
- Profiler：錄一段 interaction，看哪個 component 重 render

### Redux DevTools

- action 序列
- state 變化
- 可以 time travel

## 常見 Frontend 問題模式

### 1. 「資料來了但 UI 不更新」

- 直接 mutate state（`state.push`）
- queryKey 沒帶變數，cache 沒重 fetch
- mutation 成功沒 `invalidateQueries`
- selector 取的是新物件參考但內容沒變（用 `shallowEqual` 或 `useShallow`）

### 2. effect 跑兩次

- 多半是 `<StrictMode>` 故意的，dev 才有
- production build 不會
- 真正的 bug 是「兩次跑出不同結果」

### 3. 「按了沒反應」

- onClick 漏掉
- 事件被 `preventDefault` / `stopPropagation` 吃掉
- 元素被另一個透明 div 蓋住
- form 內 button 沒設 `type="button"`，預設 submit

### 4. 表單送出整頁 reload

- form 內 button 是預設 submit
- onSubmit 沒 `event.preventDefault()`
- 用 RHF 的話用 `handleSubmit` 包

### 5. CORS（前端視角）

- 看 Console 紅字 `blocked by CORS`
- 找後端工程師處理（不是前端問題）
- 開發環境可用 Vite proxy 繞過

### 6. 環境變數讀不到

- 名稱沒 `VITE_` 前綴
- 改了 `.env` 沒重啟 dev server
- build 後才改 `.env`，build artifact 是 build 時刻的值

---

# AI 協助 Debug

AI 在 debug 上是 **嫌疑人列表機**：給它現象、log、code，它能列出可能原因。但驗證還是你的事。

### 有用的 prompt

```
我的 API 偶爾回 504。狀況：
- 大約 1% 機率
- 集中在台北早上 9-10 點
- 後端服務本身負載沒高
- 上游 nginx 沒 error
- 後端 log 看到 query timeout

可能的原因有哪些？請按發生機率排序。
```

```
我這段 Go code 跑 `go test -race` 報 race。請指出哪兩行衝突，
並建議修法（不要直接寫，先解釋）。

（貼 code）
```

### 不該做的

- 不貼任何 log / code 就問「為什麼壞」
- 把 AI 給的修法直接 commit 不驗證
- 用 AI 修但自己看不懂改了什麼

---

# Debug 練習

對著 pre-onboarding project 自己塞 bug：

1. 把 `db.Where("id = ?", id)` 改成 `db.Where("id = ?", 0)` — 看你怎麼找
2. 把 `useEffect(() => fetch(...), [])` 的依賴拿掉 — 看你怎麼發現會 infinite loop
3. 在 service 寫個跨表寫入但不包 transaction — 用程式試到資料不一致

故意製造 bug 再修，比看 textbook 學十次有效。

---

# 心態

最後也是最重要的：

- 不要 panic
- 不要怪資料、怪框架、怪 mentor、怪 AI — 90% 是自己 code 寫錯
- 修一個 bug 之後問自己：「我學到什麼，下次怎麼避免」
- 不會修的 bug 不丟臉，**裝懂亂改才丟臉**
- 修好之後寫 **post-mortem**（公司有的話用公司格式）：
  - 現象
  - 影響
  - 根因
  - 修法
  - 學到的事

Debug 累積成你的本事，不是消耗你的時間。
