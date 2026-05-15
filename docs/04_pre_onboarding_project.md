# Pre-Onboarding Fullstack Project

## 專案名稱

Strategy Management System

---

# 專案目的

這個專案的目的不是要你做出大型系統。

而是讓你：

- 熟悉 fullstack workflow
- 熟悉 Golang + React 技術棧
- 練習 production 級開發流程
- 展示你的 code organization 與工程能力

---

# 技術要求

## Backend

- Golang
- Gin
- GORM
- PostgreSQL

## Frontend

- React
- Vite
- Ant Design

## Infra

- Docker Compose

---

# 功能需求

## 1. Authentication

### Backend

需要：

- login API
- JWT authentication
- auth middleware

### Frontend

需要：

- login page
- protected route
- token storage

---

## 2. Strategy CRUD

### 欄位

- id
- name
- symbol
- status
- created_at

---

### Backend

需要：

- create strategy
- update strategy
- delete strategy
- list strategy

---

### Frontend

需要：

- strategy table
- create modal
- edit modal
- delete confirmation

---

## 3. Pagination & Filtering

需要：

- page
- page size
- keyword filter

---

## 4. Error Handling

請實作：

- backend response format
- frontend notification handling

---

## 5. Docker

請提供：

- docker-compose.yml

包含：

- frontend
- backend
- postgres

---

## 6. README

請撰寫：

- 如何啟動
- project structure
- API description

---

# 加分項目

以下非必要，但會加分：

- migration
- refresh token
- role permission
- dark mode
- reusable table component
- unit test

---

# 評估重點

我們會觀察：

- project structure
- naming
- API design
- code readability
- git commit quality
- frontend architecture
- error handling
- debugging ability

---

# 不需要追求

不需要：

- microservice
- over-engineering
- fancy animation

重點是：

- 清楚
- 可維護
- 有思考

---

# Git 要求

請使用：

- feature branch
- clear commit message

避免：

- update
- fix bug
- test

請使用：

- feat: add login api
- fix: resolve token validation issue
- refactor: simplify strategy service
