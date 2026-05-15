# Backend Learning Guide (Golang)

## 這份文件給誰看

你是即將加入 Stranity 的新進工程師，主要負責後端或 fullstack 工作。

你可能：

- 沒寫過 Go，但寫過其他語言
- 寫過 Go 的玩具專案，但沒做過 production
- 有後端基礎，但缺少 API 設計與 debug 經驗

這份文件不是 Go 教科書，而是 **入職前你應該長出哪些能力** 的清單，以及為什麼這些能力重要。

---

## 學習總時程建議

如果你是純新手，從零開始學 Go：

- 全力學：**4 至 6 週**
- 平日下班學：**8 至 10 週**

如果你已有其他語言（Python、Java、Node）的後端經驗：

- 全力學：**2 至 3 週**
- 平日下班學：**4 至 6 週**

不需要追求「全部學完才入職」，重點是入職前能完成 [`04_pre_onboarding_project.md`](04_pre_onboarding_project.md) 的 pre-onboarding project。

---

## 環境準備

入職前請先在本機備好：

- **Go**：1.22 以上版本（[golang.org/dl](https://golang.org/dl/)）
- **PostgreSQL**：本機跑 Docker 即可，不必裝在系統上
- **Redis**：同上，用 Docker
- **Git**：command line 熟練
- **IDE**：建議 VS Code + Go extension，或 GoLand
- **API 測試工具**：Postman、Insomnia、或 `curl` 加 `httpie`
- **DB 工具**：DBeaver、TablePlus、或 `psql` command line

確認你能跑：

```bash
go version
docker --version
docker compose version
psql --version   # 用 docker 跑 psql 也可
```

---

# 第一階段：Go 語法基礎（1 至 2 週）

## 學習資源

- 官方：[A Tour of Go](https://go.dev/tour/) — 一定要全部跑完
- 官方：[Go by Example](https://gobyexample.com/) — 當字典查
- 書（選一本）：《The Go Programming Language》或 《Go 語言聖經》中譯版

## 必須會的語法

- 變數宣告：`var`、`:=`、零值
- 基本型別：`int`、`float64`、`string`、`bool`
- 複合型別：`struct`、`array`、`slice`、`map`、`pointer`
- 函式：多重回傳、命名回傳值、可變參數
- 控制流：`if`、`for`（Go 只有 `for`）、`switch`、`type switch`
- `interface`：定義、實作、空介面 `any`
- 套件：`package`、`import`、`export`（大寫即 public）
- 錯誤處理：`error` 型別、`errors.New`、`fmt.Errorf`、`errors.Is`、`errors.As`
- `defer`：執行順序、常見用途（關檔、unlock、recover）

## 容易踩的坑

- **slice 不是值傳遞**：傳進函式內 append，外面可能看不到變更（因為底層 array 被換掉）
- **map 不可並發寫入**：要嘛加鎖，要嘛用 `sync.Map`
- **for range 變數重用**：在 Go 1.22 前，`for i, v := range s` 的 `v` 每輪是同一個記憶體位置；1.22 起改正了，但讀舊 code 要小心
- **interface nil 不等於 nil**：把 `nil pointer` 塞進 interface 後，比較 `== nil` 會是 false

## 練習題

寫一個 CLI 程式，讀取 CSV 檔（欄位：id, name, score），輸出：

- 平均分數
- 分數最高的人
- 分數低於 60 的人數

用 `encoding/csv`、`os.Open`、`bufio`。重點不是邏輯，而是練 error handling 與 struct 使用。

---

# 第二階段：Concurrency（3 至 5 天）

## 為什麼重要

Stranity 的後端服務同時處理大量 API 與 WebSocket 連線，幾乎每個 service 都會用到 goroutine 與 channel。你不需要寫得多花俏，但必須讀得懂並避開 bug。

## 必須會

- **goroutine**：`go funcName()`，理解它是輕量 thread
- **channel**：buffered vs unbuffered、close、`for range`、`select`
- **sync 套件**：`sync.Mutex`、`sync.RWMutex`、`sync.WaitGroup`、`sync.Once`
- **context**：`context.Context`、`WithTimeout`、`WithCancel`、傳遞取消訊號
- **race detector**：跑 test 與 build 時加 `-race`

## 容易踩的坑

- **goroutine leak**：起了 goroutine 卻沒辦法結束（沒有 ctx 或 channel 通知）
- **channel deadlock**：寫了沒人讀的 unbuffered channel
- **拿值不拿指標**：對 struct 的 mutex 操作要拿指標，否則複製出去後鎖不到

## 練習題

寫一個 worker pool：

- 主程式產生 100 個任務（隨機 sleep 100ms 至 1s 後印出編號）
- 啟動 N 個 worker goroutine 並行處理
- 用 `context.WithTimeout(3s)` 取消尚未完成的任務
- 結束時印出實際完成數量

---

# 第三階段：HTTP 與 Gin（1 週）

## 為什麼重要

Stranity 多數服務都是 REST API + WebSocket。Gin 是團隊主力 framework。

## 必須會

- `net/http`：handler、`http.ResponseWriter`、`http.Request`、`http.ServeMux` 基本概念
- **Gin**：
  - router：`GET`、`POST`、`PUT`、`DELETE`、route group
  - middleware：`Use()`、自訂 middleware、`c.Next()`
  - 參數：`c.Param`、`c.Query`、`c.ShouldBindJSON`、`c.ShouldBindQuery`
  - 回傳：`c.JSON`、`c.AbortWithStatusJSON`
  - context 傳遞：`c.Set` / `c.Get`
- HTTP status code：200、201、204、400、401、403、404、409、422、500 各自的使用情境
- request validation：`go-playground/validator` 的 tag 用法
- CORS、logging、recovery middleware

## 練習題

實作一個 `/api/v1/users` CRUD，全部用記憶體 map 儲存：

- `POST /users`：建立，回傳 201
- `GET /users?keyword=&page=1&page_size=10`：列表，支援 keyword 模糊比對與分頁
- `GET /users/:id`：單筆
- `PUT /users/:id`：更新
- `DELETE /users/:id`：刪除，回傳 204
- 加一個 logging middleware 印出每個 request 的 method、path、耗時

---

# 第四階段：PostgreSQL 與 GORM（1 週）

## 為什麼重要

幾乎每個 feature 最後都要存資料。Stranity 用 Postgres + GORM。你不需要是 DBA，但要會寫正確、不會炸 production 的 query。

## 必須會：SQL 基礎

- `CREATE TABLE`、`ALTER TABLE`、index
- `SELECT` + `JOIN`（INNER、LEFT）
- `WHERE`、`ORDER BY`、`LIMIT`、`OFFSET`
- aggregate：`COUNT`、`SUM`、`AVG`、`GROUP BY`、`HAVING`
- transaction：`BEGIN`、`COMMIT`、`ROLLBACK`、隔離級別概念
- 索引：什麼欄位該加 index、複合索引順序、`EXPLAIN ANALYZE` 看計畫

## 必須會：GORM

- 模型定義：`gorm:"primaryKey"`、`gorm:"index"`、`gorm:"size:100"`
- 自動 migration vs 手寫 migration（production 用後者）
- CRUD：`Create`、`First`、`Find`、`Updates`、`Delete`
- 條件查詢：`Where`、`Or`、`Not`、`Order`、`Limit`、`Offset`
- 關聯：`Has One`、`Has Many`、`Belongs To`、`Many To Many`、`Preload`
- transaction：`db.Transaction(func(tx *gorm.DB) error { ... })`
- raw SQL：何時該繞過 ORM
- soft delete：`gorm.DeletedAt` 的行為

## 容易踩的坑

- **N+1 query**：跑 list 時對每個 row 又查一次關聯，會慢爆。用 `Preload` 解
- **`Updates` 預設略過 zero value**：想把欄位更新成 `false` 或 `0` 要用 `Select` 指定，或改用 `Save`
- **沒包 transaction**：跨表寫入沒包 tx，中間失敗就資料不一致

## 練習題

接續第三階段的 user CRUD：

- 換成 Postgres + GORM
- 加一個 `posts` 表，欄位：id、user_id、title、content、created_at
- 實作 `GET /users/:id/posts` 列出該 user 的 posts
- 加一個 `POST /users/:id/posts/batch`，一次新增多筆 post，要包在 transaction 裡

---

# 第五階段：認證與 JWT（3 至 5 天）

## 為什麼重要

幾乎所有 API 都要登入。JWT 是 Stranity 主要用法。

## 必須會

- 密碼雜湊：用 `bcrypt`，不要自己發明
- JWT 結構：header、payload、signature
- 套件：`github.com/golang-jwt/jwt/v5`
- 流程：
  1. login API 驗證密碼
  2. 簽 JWT，設 expiry（access token 短、refresh token 長）
  3. client 把 token 放在 `Authorization: Bearer <token>` header
  4. middleware 解析、驗證、把 user info 塞進 context
  5. handler 從 context 拿 user info
- refresh token：用途、儲存方式、rotation 概念

## 安全性必懂

- 密碼絕對不存明碼
- JWT secret 不能進 git，要從環境變數讀
- token 過期時間：access 15 分鐘至 1 小時，refresh 7 至 30 天
- 別把敏感資料塞進 JWT payload，它只是 base64 而已

## 練習題

接續上面的 user 系統：

- 加 `password_hash` 欄位
- `POST /auth/login`：驗證帳密、回傳 access token 與 refresh token
- `POST /auth/refresh`：用 refresh token 換新的 access token
- middleware 保護 `/api/v1/users/*`，未登入回 401
- `GET /api/v1/me`：回傳當前登入 user 資訊

---

# 第六階段：Project Structure（3 天）

## 為什麼重要

亂寫一個資料夾的 code，三週後自己也讀不懂。

## 建議結構（中型 service）

```
my-service/
├── cmd/
│   └── server/
│       └── main.go              入口點
├── internal/
│   ├── api/                     HTTP handler
│   │   ├── router.go
│   │   ├── user_handler.go
│   │   └── middleware/
│   ├── service/                 商業邏輯
│   │   └── user_service.go
│   ├── repository/              DB 存取
│   │   └── user_repository.go
│   ├── model/                   資料結構
│   │   └── user.go
│   └── config/                  設定
├── migrations/                  SQL migration
├── pkg/                         可被外部使用的 package（很少用）
├── docker-compose.yml
├── Dockerfile
├── go.mod
├── go.sum
└── README.md
```

## 為什麼分層

- **handler**：只負責 HTTP 細節（解 request、回 response），不寫商業邏輯
- **service**：商業邏輯主體，handler 與 repository 之間的橋
- **repository**：只負責 DB 操作，service 不該直接寫 SQL
- **model**：跨層共用的資料結構

這樣換 DB、換 HTTP framework、寫測試都簡單。

## 不要做的事

- 把所有 code 放在 `main.go`
- 在 handler 直接 `db.Where(...)`
- 在 service 裡 `c.JSON(...)`
- 用全域變數放 db connection、logger、config

---

# 第七階段：測試（3 天）

## 為什麼重要

入職後改 code，沒測試你會怕。寫測試是工程師的基本功，不是加分項。

## 必須會

- `go test ./...` 跑全部測試
- `go test -run TestName` 跑單一測試
- `go test -cover` 看覆蓋率
- `go test -race` 跑 race detector
- `*testing.T`、`t.Errorf`、`t.Fatal`
- table-driven test 寫法
- `testify/assert`、`testify/require`、`testify/mock` 套件
- 用 `httptest` 測 handler
- 用 `sqlmock` 或實際 dockerized Postgres 測 repository

## Pre-onboarding 不必達到

- 80% 覆蓋率
- 完整 integration test
- mock 全部依賴

但希望你至少寫一兩個 unit test，知道流程怎麼跑。

---

# 第八階段：閱讀 Stranity codebase（入職第一週）

入職前不必做，但入職第一週請開：

- `~/stranity/gobe2/` — 主後端服務
- `~/stranity/eltrade-worker/` — Go worker service 範例
- `~/stranity/sikaz/` — 另一個 Go service

觀察：

- 他們怎麼分目錄
- 他們怎麼寫 handler、service、repository
- 他們怎麼處理 error
- 他們的 config 怎麼讀

**Stranity 內部慣例優先於本文件**。本文件描述的是 baseline 能力，實際專案有自己的 style。

---

# 入職前 self check

入職前一週，請確認自己能：

- [ ] 不查資料就寫出一個 Gin route + handler
- [ ] 寫一個 GORM model 並 query 出資料
- [ ] 解釋 JWT 為什麼會過期、refresh token 為什麼存在
- [ ] 解釋 goroutine leak 是什麼、怎麼避免
- [ ] 解釋 transaction 用途、什麼時候必須包
- [ ] 用 `docker compose up` 跑起一個含 postgres 的本機環境
- [ ] 用 `curl` 或 Postman 打你自己寫的 API
- [ ] 完成 [`04_pre_onboarding_project.md`](04_pre_onboarding_project.md)

做不到沒關係，記下做不到的項目，第一天告訴你的 mentor。**誠實比裝懂重要 100 倍。**
