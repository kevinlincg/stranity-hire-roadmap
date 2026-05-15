# Pre-Onboarding Fullstack Project

## 專案名稱

**Strategy Management System**（策略管理系統）

---

# 1. 為什麼要做這個專案

## 1.1 我們想評估什麼

入職前我們不會考你 LeetCode，但會請你做一個小型 fullstack 專案。原因是：

- **CV 寫得再好都不如看 code**
- 想看你怎麼組織專案、命名、處理錯誤
- 想看你 git commit 的習慣
- 想看你怎麼處理「需求沒寫清楚」的情況（會故意留模糊地帶）
- 想看你怎麼寫 README、怎麼讓別人能跑起來

## 1.2 這不是什麼

- **不是** 要你做大型系統
- **不是** 要你用最新最潮的技術
- **不是** 要你功能做最多
- **不是** 要你寫最快

「清楚、可維護、能跑、有思考」遠勝「多功能、酷炫、看不懂」。

## 1.3 預期時程

| 程度 | 全力做 | 下班做 |
|------|-------|-------|
| 第一次寫 Go + React fullstack | 10 至 14 天 | 3 至 4 週 |
| 有後端經驗、第一次寫前端（或反之） | 7 至 10 天 | 2 至 3 週 |
| 兩邊都熟 | 4 至 5 天 | 1 至 2 週 |

**做不完不要硬撐。** 寧可功能少但乾淨，不要塞滿但一團亂。完不成的部分老實寫在 README，反而會加分。

---

# 2. 故事背景

> 你被一家量化交易公司聘為後台工程師。
>
> 交易員（user）會在系統裡管理自己的「交易策略」（strategy），每個策略對應一個股票標的（symbol）與一個啟用狀態（status）。
>
> 你要做的是「策略管理後台」：交易員可以登入、新增 / 修改 / 刪除 / 查詢自己的策略。
>
> 系統會被多個交易員同時使用，所以每個策略屬於某個 user，不能被別人看到或改到。

這個故事是為了讓你做設計決策時有依據（例如：策略是不是每個 user 獨立？答案是「對」）。

---

# 3. 技術要求

## 3.1 必須使用

### Backend

- **Go**：1.22 以上
- **Gin**：HTTP framework
- **GORM**：ORM
- **PostgreSQL**：16 以上
- **JWT**：認證（推薦 `github.com/golang-jwt/jwt/v5`）

### Frontend

- **React**：18 以上
- **Vite**：build tool
- **TypeScript**：嚴格模式（`strict: true`）
- **Ant Design**：5.x
- API 串接套件自選：`@tanstack/react-query` + `axios` 是推薦組合

### Infra

- **Docker** + **Docker Compose**：一個 `docker compose up` 就要能跑全部

## 3.2 不必使用（但會加分）

- TanStack Query / SWR
- Zustand（單頁 app 用 `useState` 也夠）
- migration 工具（`golang-migrate`、`goose`、`atlas` 擇一）
- ESLint + Prettier + golangci-lint
- Makefile / Taskfile
- unit test

---

# 4. 功能規格

## 4.1 認證

### 需求

- 使用者用 email + 密碼登入
- 登入成功回 access token（JWT）
- 後續所有 API 都要帶 token
- 沒帶 token 或 token 過期回 401

### Endpoint

| Method | Path | 說明 |
|--------|------|-----|
| POST | `/api/v1/auth/register` | 註冊（**只需做一個內建帳號 seed 也可以，但建議實作 register**） |
| POST | `/api/v1/auth/login` | 登入，回 token |
| GET | `/api/v1/auth/me` | 拿當前 user info（需 token） |

### Token 規範

- access token TTL：1 小時
- payload 至少包含：`user_id`、`exp`
- 不要把密碼或其他敏感資料放進 payload
- secret 從環境變數讀

### 加分項目

- refresh token（rotation）
- 登出 endpoint（token 黑名單，redis）

---

## 4.2 Strategy CRUD

每個 strategy 屬於登入的 user。**user A 看不到 / 改不到 user B 的 strategy**。

### Endpoint

| Method | Path | 說明 |
|--------|------|-----|
| GET | `/api/v1/strategies` | 列表（含分頁、篩選、排序） |
| GET | `/api/v1/strategies/:id` | 單筆 |
| POST | `/api/v1/strategies` | 新增 |
| PUT | `/api/v1/strategies/:id` | 全量更新 |
| PATCH | `/api/v1/strategies/:id/status` | 只改狀態（加分項，可選） |
| DELETE | `/api/v1/strategies/:id` | 刪除（soft delete） |

### Strategy 欄位

| 欄位 | 型別 | 說明 |
|------|------|------|
| `id` | uint64 | primary key |
| `user_id` | uint64 | 屬於哪個 user，FK |
| `name` | string | 策略名稱，非空、長度 1 至 100 |
| `symbol` | string | 股票代號，例如 `2330`、`AAPL`、`BTCUSDT`；非空、長度 1 至 20 |
| `status` | string | `active` / `inactive`，預設 `inactive` |
| `description` | string | 可選，最長 500 |
| `created_at` | timestamp | 建立時間 |
| `updated_at` | timestamp | 更新時間 |
| `deleted_at` | timestamp / null | soft delete 時間，預設 null |

---

## 4.3 分頁、篩選、排序

### Query string 規範

```
GET /api/v1/strategies?page=1&page_size=20&keyword=AAPL&status=active&sort=-created_at
```

| 參數 | 說明 | 預設值 |
|------|------|-------|
| `page` | 第幾頁，從 1 開始 | 1 |
| `page_size` | 每頁筆數，1 至 100 | 20 |
| `keyword` | `name` 或 `symbol` 模糊比對 | 無 |
| `status` | `active` / `inactive` 過濾 | 無（全部） |
| `sort` | 排序欄位，前綴 `-` 為倒序，例如 `-created_at` | `-created_at` |

### Response 格式

見 §5 統一格式。

---

## 4.4 統一 Response 格式

請設計一個全 app 一致的 response shape。建議：

### 成功（單筆）

```json
{
  "data": {
    "id": 1,
    "name": "AAPL Long",
    "symbol": "AAPL",
    "status": "active",
    "description": "...",
    "created_at": "2026-05-16T08:00:00Z",
    "updated_at": "2026-05-16T08:00:00Z"
  }
}
```

### 成功（列表）

```json
{
  "data": [
    { "...": "..." }
  ],
  "pagination": {
    "page": 1,
    "page_size": 20,
    "total": 137
  }
}
```

### 失敗

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "name is required",
    "details": [
      { "field": "name", "rule": "required" }
    ]
  }
}
```

### Error code 建議

| code | HTTP status | 用途 |
|------|-------------|------|
| `VALIDATION_ERROR` | 400 | 欄位驗證失敗 |
| `UNAUTHORIZED` | 401 | 未登入或 token 失效 |
| `FORBIDDEN` | 403 | 沒權限（例：改別人的 strategy） |
| `NOT_FOUND` | 404 | 資源不存在 |
| `CONFLICT` | 409 | 衝突（例：name 重複） |
| `INTERNAL_ERROR` | 500 | 伺服器錯誤 |

**任何 4xx 都不該回 stack trace，500 也要 log 後吞掉 detail。**

---

## 4.5 前端頁面

至少要有：

### 頁面 1：Login

- email + 密碼欄
- 「登入」按鈕
- 錯誤訊息（密碼錯、帳號不存在）
- 登入後跳到 strategy list

### 頁面 2：Strategy List

- 上方：keyword 搜尋、status 篩選、「新增」按鈕
- 中間：Table（id、name、symbol、status、created_at、操作）
- 下方：分頁器
- 操作欄：編輯、刪除（刪除前要確認）
- Table loading 狀態
- 空資料狀態

### 頁面 3：Strategy 新增 / 編輯（Modal）

- 用 Modal 包 Form
- 欄位：name、symbol、status（Select）、description（TextArea）
- 驗證：required、長度
- 成功後關閉 Modal、refresh list、show 成功 notification
- 失敗 show error notification

### 頁面 4：Header / Layout

- 顯示當前 user email
- 登出按鈕
- 公司 logo 或 app 名稱

---

## 4.6 Docker Compose

`docker compose up` 後要能：

- backend 在 `http://localhost:8080`
- frontend 在 `http://localhost:5173`
- postgres 在 `localhost:5432`
- frontend 能打到 backend（dev proxy 或 CORS）
- postgres 資料用 named volume 保存（`docker compose down` 不會掉，`down -v` 才會掉）

範例骨架：

```yaml
services:
  postgres:
    image: postgres:16
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app
      POSTGRES_DB: strategy_db
    ports:
      - "5432:5432"
    volumes:
      - pg_data:/var/lib/postgresql/data

  backend:
    build: ./backend
    depends_on: [postgres]
    environment:
      DB_DSN: postgres://app:app@postgres:5432/strategy_db?sslmode=disable
      JWT_SECRET: change_me
    ports:
      - "8080:8080"

  frontend:
    build: ./frontend
    depends_on: [backend]
    ports:
      - "5173:5173"

volumes:
  pg_data:
```

---

# 5. 建議檔案結構

## 5.1 整體

```
strategy-management/
├── README.md
├── docker-compose.yml
├── .env.example
├── backend/
└── frontend/
```

## 5.2 Backend

```
backend/
├── cmd/
│   └── server/
│       └── main.go
├── internal/
│   ├── api/
│   │   ├── router.go
│   │   ├── auth_handler.go
│   │   ├── strategy_handler.go
│   │   └── middleware/
│   │       ├── auth.go
│   │       ├── cors.go
│   │       └── logger.go
│   ├── service/
│   │   ├── auth_service.go
│   │   └── strategy_service.go
│   ├── repository/
│   │   ├── user_repository.go
│   │   └── strategy_repository.go
│   ├── model/
│   │   ├── user.go
│   │   └── strategy.go
│   ├── config/
│   │   └── config.go
│   └── pkg/
│       ├── jwt/
│       ├── hash/
│       └── response/
├── migrations/
│   ├── 001_create_users.sql
│   └── 002_create_strategies.sql
├── Dockerfile
├── go.mod
└── go.sum
```

## 5.3 Frontend

```
frontend/
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── api/
│   │   ├── client.ts            # axios instance
│   │   ├── auth.ts
│   │   └── strategy.ts
│   ├── pages/
│   │   ├── Login.tsx
│   │   └── StrategyList.tsx
│   ├── components/
│   │   ├── Layout.tsx
│   │   └── StrategyFormModal.tsx
│   ├── hooks/
│   │   └── useAuth.ts
│   ├── store/                   # 若用 zustand
│   │   └── authStore.ts
│   ├── types/
│   │   └── strategy.ts
│   └── utils/
├── public/
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
└── Dockerfile
```

---

# 6. 兩週時程建議

下表假設你每天能投入 2 至 4 小時。

## Day 1 至 2：建環境與骨架

- 開 GitHub repo（private 也可）
- 寫好 README 第一版（即使先寫「TBD」）
- 起 `docker compose up postgres` 跑起 DB
- backend：起一個能回 `GET /healthz → 200` 的 Gin server
- frontend：起一個 Vite + AntD hello world，能打到 `/healthz` 拿到 200

**Day 2 結束時，前後端能 docker-compose 起來互通。**

## Day 3 至 4：認證

- 設計 `users` 表 + migration
- 寫 `register`、`login`、`me` 三個 endpoint
- JWT middleware
- 前端：Login 頁、登入流程、protected route

**Day 4 結束時，能在前端登入、看到 me 資訊、重整不掉登入。**

## Day 5 至 8：Strategy CRUD

- 設計 `strategies` 表 + migration
- backend：CRUD + 分頁 + 篩選 + 排序 + 權限（只能改自己的）
- 前端：Strategy List 頁 + 新增 / 編輯 Modal + 刪除確認

**Day 8 結束時，完整 CRUD 在前端可用。**

## Day 9 至 10：打磨

- 統一 response 格式
- 錯誤訊息 i18n（中英文擇一即可，一致就好）
- loading、空資料、錯誤狀態的 UI
- 前後端的 README

## Day 11 至 12：加分項目（時間夠才做）

- 至少一個 backend unit test
- 至少一個 frontend component test 或 hook test
- refresh token
- dark mode

## Day 13 至 14：交付前複盤

- 用乾淨環境 clone 自己的 repo，照 README 跑一次（必做）
- 修 README 中不清楚的地方
- 整理 commit history（讓人看得懂）
- 自己跑一次完整流程：登入 → 新增 → 編輯 → 刪除 → 篩選 → 翻頁

---

# 7. README 規範

請在專案根目錄寫一份 README，至少包含：

```markdown
# Strategy Management System

簡短介紹（2 至 3 句）。

## 技術棧
- Backend: Go 1.22 + Gin + GORM + PostgreSQL
- Frontend: React 18 + Vite + TypeScript + Ant Design
- Infra: Docker Compose

## 如何啟動

\`\`\`bash
cp .env.example .env
docker compose up -d
\`\`\`

打開 http://localhost:5173

預設帳號（如果有 seed）：
- email: demo@example.com
- password: demo1234

## 專案結構

（簡述）

## API

簡述 endpoint，或附 Postman collection / curl 範例。

## 已知限制 / 沒做完的部分

- refresh token 未實作
- ...

## 我會怎麼改進（若有更多時間）

- ...
```

「已知限制」與「我會怎麼改進」是 **大加分項**。代表你知道自己沒做什麼、知道更好的做法。

---

# 8. 加分項目（依重要性排序）

1. **DB migration 工具**（不是 GORM AutoMigrate）：`golang-migrate` 或 `goose`
2. **golangci-lint** + **ESLint / Prettier** 並通過
3. **至少一個 backend unit test 與一個 frontend test**
4. **統一 logger**（zerolog / zap），含 request id
5. **refresh token**
6. **role / permission**（admin 看得到所有人的 strategy）
7. **dark mode**
8. **可重用 Table component**（包好分頁、排序、loading）
9. **API spec**（OpenAPI / Swagger）
10. **CI**（GitHub Actions：lint + test + build）

---

# 9. 評估標準

我們會從這 8 個面向看：

| 項目 | 重點 |
|------|------|
| **Project structure** | 分層清楚、檔名有規律、不亂塞 |
| **Naming** | 變數 / function / file / branch 名都看得懂 |
| **API design** | RESTful、status code 正確、response 一致 |
| **Code readability** | 不需大量註解就能讀懂 |
| **Git commit quality** | 訊息清楚、一個 commit 一件事 |
| **Frontend architecture** | component 切分合理、state 不亂、API 層集中 |
| **Error handling** | 不吞錯誤、UI 有對應狀態、邊界條件有考慮 |
| **Debugging ability** | README 寫得讓 reviewer 能重現、bug 解釋得清楚 |

---

# 10. 不需要做的事

- **microservice** — 一個 backend 即可
- **超複雜的權限系統** — 只有「自己的策略只有自己能改」就夠
- **fancy animation** — UI 乾淨即可
- **自己手刻 JWT** — 用套件
- **自己手刻 ORM** — 用 GORM
- **K8s / Terraform / CI/CD pipeline** — Docker Compose 就夠

過度工程是扣分項，不是加分項。

---

# 11. 常見 Q&A

**Q: 一定要做 register 嗎？只 seed 一個帳號可以嗎？**
A: 可以，但 README 要寫明預設帳號。做了 register 是小加分。

**Q: 要做 admin / 多角色嗎？**
A: 不必。所有 user 都是同一個角色就好。多角色是加分項。

**Q: status 一定要是 active / inactive 嗎？可以多加狀態嗎？**
A: 可以加，但 README 要說明每個狀態的意義。

**Q: 一定要 soft delete 嗎？**
A: 是。`deleted_at` 欄位是規格的一部分。

**Q: 前端可以用 Next.js 嗎？**
A: 不行，請用純 React + Vite。Stranity 內部用 SPA + 獨立 backend。

**Q: 可以用 SQLite 嗎？**
A: 不可以。請用 Postgres。SQLite 跟 Postgres 行為差異很大（型別、index、index 排序、JSON）。

**Q: 不會 Docker，可以不寫 Dockerfile 嗎？**
A: 可以省略 Dockerfile，但 docker-compose.yml 一定要有（至少要起 postgres）。Dockerfile 是加分項。

**Q: 我做不完怎麼辦？**
A: 老實在 README 寫「沒做的部分 + 為什麼 + 我會怎麼補」。**誠實比裝完整好太多。**

**Q: 不確定某個需求怎麼解？**
A: 寫信問 mentor。把你的兩個方案、各自優缺、你選哪個、為什麼，一次說清楚。看你怎麼思考比看答案重要。

**Q: 要附 demo 影片或截圖嗎？**
A: 不必，但前端有截圖貼在 README 是大加分。

---

# 12. 交付方式

1. 推到 GitHub（public 或 invite collaborator）
2. 確認 `git clone` 後照 README 能跑（**親自在乾淨機器或新資料夾驗證一次**）
3. 寄信給 mentor：
   - repo 連結
   - 你花了多久
   - 你最得意的設計決定
   - 你沒做完的部分
   - 你想討論的問題

---

# 13. 心態

這個專案不是入學考。**沒過就不錄取** 不是制度。

它是你和 mentor 第一次合作的引子。我們想看到：

- 你怎麼面對「需求沒寫死」的情況
- 你怎麼面對「不會」的時候
- 你怎麼面對「做不完」的時候
- 你怎麼把一件事做到「可以給人看」的程度

慢慢來，誠實面對自己。
