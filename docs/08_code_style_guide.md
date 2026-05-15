# 程式碼風格指南

## 大原則

> 寫給未來的自己看，不是寫給編譯器看。

排序：

1. **正確** — 能做到需求
2. **清楚** — 別人 / 半年後的你看得懂
3. **可維護** — 改起來不會打到別處
4. **效能** — 證實是瓶頸再優化

「先讓它能跑、再讓它對、再讓它快」這個 mantra 是 baseline，不是藉口。

---

# 通用原則

## 命名

### 好命名

- `userService.CreateUser(req)`
- `strategyRepository.ListByUserID(userID)`
- `parseSymbolFromCSV(line)`
- `isOrderExecutable(order)`
- `MAX_RETRY_COUNT`

### 壞命名

- `data`、`info`、`result`、`tmp`、`obj`、`x`、`foo`、`bar`
- `doIt()`、`handle()`、`process()`（處理什麼？）
- `flag`、`status`（什麼狀態？）
- `i`、`j`、`k`（只能用在很短的 loop counter）

### 規則

- 變數名長度跟使用範圍成正比：local 用短的、跨檔案用完整名
- boolean 用 `is`、`has`、`can`、`should` 開頭
- function 用動詞開頭：`get`、`list`、`create`、`update`、`delete`、`find`、`is`
- 縮寫只在團隊都懂時才用（`API`、`URL`、`JSON` 可以；`usrRepoSvc` 不行）

## Function 設計

- **單一職責**：一個 function 做一件事
- **參數少**：超過 4 個參數就考慮包成 struct / object
- **早 return**：先處理錯誤、邊界，主邏輯不縮排
- **長度警戒**：超過 50 行去想能不能拆
- **副作用顯式**：function name 看不出有副作用就改名

### 範例

❌ 巢狀地獄：

```go
func ProcessOrder(o Order) error {
    if o.User != nil {
        if o.Status == "pending" {
            if o.Symbol != "" {
                // ... 主邏輯
            } else {
                return errors.New("symbol required")
            }
        } else {
            return errors.New("invalid status")
        }
    } else {
        return errors.New("user required")
    }
}
```

✅ 早 return：

```go
func ProcessOrder(o Order) error {
    if o.User == nil {
        return errors.New("user required")
    }
    if o.Status != "pending" {
        return errors.New("invalid status")
    }
    if o.Symbol == "" {
        return errors.New("symbol required")
    }
    // ... 主邏輯
}
```

---

# Backend (Go) 風格

## 套件結構

- 每個套件職責清楚（見 [`01_backend_learning.md`](01_backend_learning.md) 第六階段）
- 避免循環引用
- internal package 用 `internal/` 隔離
- pkg / 子目錄都用小寫單數：`user`、`strategy`、`order`

## 命名慣例

- 套件名小寫、無底線、無連字號：`userservice` 而非 `user-service`
- exported 名大寫開頭：`UserService`、`CreateUser`
- private 名小寫開頭：`hashPassword`、`buildQuery`
- interface 不必加 `I` 前綴（不是 Java）
- struct method receiver 用 1 至 2 個字母縮寫：`func (u *User) ...`

## Error Handling

- 永遠處理 error，不要 `_ = doSomething()`
- 包 context：`fmt.Errorf("create user: %w", err)`
- sentinel error：`var ErrNotFound = errors.New("not found")`，呼叫端用 `errors.Is`
- typed error：實作 `Error() string` 與自訂方法
- handler 層轉成 HTTP error，service 層只 return error
- log 一個 error 一次就好（呼叫鏈每層都 log 是噪音）

❌ 吞錯誤：

```go
result, _ := repo.Get(id)
// result 可能是 nil，下面就會 panic
```

✅ 處理：

```go
result, err := repo.Get(id)
if err != nil {
    return fmt.Errorf("get strategy %d: %w", id, err)
}
```

## 結構分層

handler → service → repository → DB

- **handler**：HTTP 細節（解 request、回 response），不寫商業邏輯
- **service**：商業邏輯主體，呼叫 repository
- **repository**：純 DB 存取，不知道 HTTP / context 細節

❌ handler 寫 SQL：

```go
func GetStrategy(c *gin.Context) {
    var s Strategy
    db.Where("id = ?", c.Param("id")).First(&s)  // 不該在這
    c.JSON(200, s)
}
```

✅ 分層：

```go
func (h *StrategyHandler) GetStrategy(c *gin.Context) {
    id, _ := strconv.ParseUint(c.Param("id"), 10, 64)
    strategy, err := h.service.GetByID(c.Request.Context(), id)
    if err != nil {
        respondError(c, err)
        return
    }
    c.JSON(http.StatusOK, gin.H{"data": strategy})
}
```

## Context

- 所有跨 IO 的 function 第一個參數是 `ctx context.Context`
- 不要把 context 存進 struct
- 不要把 context 用來傳「resource」(用參數)；只放 deadline、cancel、request-scoped value

## Goroutine

- 起 goroutine 一定要想：怎麼結束？
- 用 `errgroup` 或 channel + `select` 整理多 goroutine
- 共享狀態要鎖：`sync.Mutex`，或設計成不共享

## Comment 規則

- public function / struct / interface 加 doc comment（`// FuncName ...`）
- 註解寫 **為什麼**，不寫 **做什麼**
- 看 code 就知道做什麼的別寫註解
- 過時註解比沒註解糟糕

---

# Frontend (React / TS) 風格

## 檔名

- component / page：`PascalCase.tsx`（`StrategyList.tsx`、`LoginPage.tsx`）
- hook：`useXxx.ts`（`useAuth.ts`、`useDebounce.ts`）
- 其他：`camelCase.ts`（`apiClient.ts`、`formatCurrency.ts`）
- 測試：`Xxx.test.tsx` 或 `__tests__/Xxx.test.tsx`

## Component 寫法

- 用 `function`，不用 `const Component = () => {}`（個人習慣可選，但 repo 內統一）
- 一個檔案一個主 component；小 helper component 可放同檔案
- props 用 `type` 定義：

```tsx
type StrategyCardProps = {
  strategy: Strategy;
  onEdit?: (s: Strategy) => void;
  className?: string;
};

export function StrategyCard({ strategy, onEdit, className }: StrategyCardProps) {
  // ...
}
```

- 用 `cn(...)` 合併 class：

```tsx
<div className={cn('rounded border p-4', isActive && 'border-blue-500', className)}>
```

## State 規則

- 簡單共用就 `useState` + props drilling
- 全 app 共用就 Redux slice
- server data 永遠用 RTK Query / TanStack Query 之類，**不要存 useState**
- 不要把 props 直接 copy 進 useState（會 stale）

## Hook 規則

- 自訂 hook 名一定 `useXxx` 開頭
- 不在條件 / loop 內呼叫 hook
- `useEffect` 的依賴陣列誠實寫，lint 報黃線認真聽
- 「為什麼跑兩次」多半是 `<StrictMode>`，不是 bug

## Form

- 用 RHF + Zod
- Zod schema 抽到 `src/lib/schemas/`
- TS 型別從 Zod 推導：`type FormValues = z.infer<typeof schema>`

## API 串接

- 用 RTK Query 或共用 axios instance
- 不要每個 component 自己 `axios.get`
- error 全域處理（401 → 登出、5xx → toast）

## TS 規範

- `any` 是禁字（極例外加註解說明）
- `as Foo` 強轉前先想能不能用 type guard
- nullable 老實標：`string | null`
- 不要為了「能編譯」加一堆 `!`

## Comment 規則

- component 上方不必廢話註解
- 複雜邏輯說 **為什麼**：「這個 effect 重複跑是因為 StrictMode，已驗證」

---

# API Response 格式

## 成功

單筆：

```json
{ "data": { "id": 1, "name": "..." } }
```

列表：

```json
{
  "data": [...],
  "pagination": { "page": 1, "page_size": 20, "total": 100 }
}
```

## 錯誤

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "name is required",
    "details": [{ "field": "name", "rule": "required" }]
  }
}
```

## HTTP status

| status | 用途 |
|--------|------|
| 200 | 成功 |
| 201 | 建立成功 |
| 204 | 成功但無內容 |
| 400 | 客戶端錯誤（驗證失敗、格式錯） |
| 401 | 未認證 |
| 403 | 已認證但無權限 |
| 404 | 資源不存在 |
| 409 | 衝突（重複、版本不對） |
| 422 | 業務規則拒絕（語法 OK 但邏輯不通） |
| 500 | 伺服器錯 |
| 503 | 服務暫不可用 |

不要 200 OK 但 body 寫 `{ "success": false }`。HTTP status 就是給機器讀的。

---

# Commit / PR 風格

請看 [`03_git_and_engineering.md`](03_git_and_engineering.md) 第三、四部分。

---

# 工具與設定

- Go：`gofmt`、`goimports`、`golangci-lint`（設定見各 repo）
- TypeScript：`prettier` + `eslint`（設定見各 repo）
- 跑 CI 前本機跑一次 lint，不要等 CI 紅燈

---

# 不要做的事（總表）

- ❌ 一個 PR 改 30 個 file 且彼此無關
- ❌ commit message 寫 `update`
- ❌ catch 後不處理
- ❌ 用 `any` 通過 TS 編譯
- ❌ 在 handler 寫 SQL
- ❌ 在 service 寫 `c.JSON`
- ❌ 直接改 state（`state.push(...)`）
- ❌ 沒包 transaction 的多步驟寫入
- ❌ commit `.env`、密鑰
- ❌ 為了「DRY」過度抽象（兩處重複不該抽 helper）
- ❌ AI 寫完不看就 commit
