# Backend Learning Guide (Golang)

## 學習目標

這份文件的目標是幫助你在入職前建立 production backend 的基礎能力。

你不需要一開始就會大型系統設計。

但希望你至少能：

- 理解 Go project structure
- 能獨立撰寫 CRUD API
- 能閱讀與修改現有 code
- 能 debug backend issue
- 能理解 API request flow

---

# 第一階段：Go Fundamentals

## 必學內容

### Go Syntax

請熟悉：

- struct
- interface
- pointer
- slice
- map
- package
- error handling
- defer

---

### Goroutine & Channel

理解：

- goroutine
- channel
- select
- mutex
- waitgroup

目標：

理解 concurrent programming 基本概念。

---

### Context

請理解：

- context.Background
- context.WithTimeout
- cancellation

Production backend 大量依賴 context。

---

# 第二階段：Web Backend

## Gin

請熟悉：

- routing
- middleware
- request binding
- validation
- response format

---

## GORM

請熟悉：

- CRUD
- association
- transaction
- migration
- pagination

---

## PostgreSQL

請理解：

- index
- foreign key
- query optimization
- transaction

---

# 第三階段：Production Engineering

## API Design

請學習：

- RESTful API
- consistent response format
- pagination
- filtering
- sorting

---

## Error Handling

請避免：

- panic everywhere
- unclear error message

請學習：

- wrapped error
- centralized error handling

---

## Logging

請理解：

- request log
- structured logging
- trace request flow

---

# 入職前最低要求

你至少應該能：

- 使用 Gin 建立 API
- 使用 GORM 操作 PostgreSQL
- 實作 JWT login
- 理解 middleware
- 使用 Docker compose 啟動服務

---

# 推薦資源

## Golang

- A Tour of Go
- Effective Go

## Gin

- https://gin-gonic.com/

## GORM

- https://gorm.io/

## PostgreSQL

- https://www.postgresql.org/docs/
