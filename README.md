# Stranity New Hire Roadmap

這個 repository 是給 Stranity 新進工程師使用的 onboarding roadmap，特別適合 **Golang + React Fullstack Engineer**。

目標不是讓新人短時間內背完所有技術，而是透過清楚的階段目標，逐步建立：

- Production 級工程能力
- Fullstack 開發能力
- 團隊協作能力
- 系統思維
- Product Engineering mindset

---

## 適用對象

這份 roadmap 特別適合：

- Junior Fullstack Engineer
- Backend 轉 Fullstack 工程師
- React Native 轉 React Web 工程師
- 有 CS 基礎但缺少 production 經驗的新進工程師
- Internship / New Grad Engineer

---

## 技術棧

### Backend

- Golang
- Gin
- GORM
- PostgreSQL
- Redis
- JWT
- RESTful API
- WebSocket

### Frontend

- React
- Vite
- TypeScript
- Ant Design
- Zustand
- TanStack Query / React Query
- TailwindCSS

### Infra / Tooling

- Docker
- Docker Compose
- Linux
- Git / GitHub
- CI/CD
- Nginx

---

## Repo 結構

```txt
stranity-hire-roadmap/
├── README.md
├── CONTRIBUTING.md
├── docs/
│   ├── 01_backend_learning.md
│   ├── 02_frontend_learning.md
│   ├── 03_git_and_engineering.md
│   ├── 04_pre_onboarding_project.md
│   ├── 05_first_month_goal.md
│   ├── 06_second_month_goal.md
│   ├── 07_third_month_goal.md
│   ├── 08_code_style_guide.md
│   ├── 09_debugging_guide.md
│   └── 10_evaluation_checklist.md
├── examples/
│   ├── api_response_format.md
│   ├── commit_message_examples.md
│   └── project_structure_examples.md
└── .github/
    ├── PULL_REQUEST_TEMPLATE.md
    └── ISSUE_TEMPLATE/
        ├── onboarding_task.md
        └── learning_note.md
```

---

## 建議使用方式

### 入職前

請先閱讀：

1. [`docs/01_backend_learning.md`](docs/01_backend_learning.md)
2. [`docs/02_frontend_learning.md`](docs/02_frontend_learning.md)
3. [`docs/03_git_and_engineering.md`](docs/03_git_and_engineering.md)
4. [`docs/04_pre_onboarding_project.md`](docs/04_pre_onboarding_project.md)

並完成 pre-onboarding fullstack project。

---

### 入職後三個月

請依序完成：

1. [`docs/05_first_month_goal.md`](docs/05_first_month_goal.md)
2. [`docs/06_second_month_goal.md`](docs/06_second_month_goal.md)
3. [`docs/07_third_month_goal.md`](docs/07_third_month_goal.md)

每個階段都會包含：

- 技術目標
- 工作目標
- Deliverables
- 評估標準
- 常見風險

---

## 我們重視什麼

比起「寫得快」，我們更重視：

- 可維護性
- 清楚的思考
- Debug 能力
- 溝通能力
- 學習能力
- Ownership

---

## 新人應具備的心態

你不需要一開始什麼都會。

但希望你：

- 願意研究
- 願意問問題
- 願意改善
- 願意持續學習
- 願意對自己寫的功能負責

工程能力不是靠短期衝刺完成，而是靠每一次 feature、debug、review、refactor 慢慢累積。

---

## Pre-onboarding Project

入職前請完成一個小型 fullstack project：

**Strategy Management System**

詳細規格請看：

[`docs/04_pre_onboarding_project.md`](docs/04_pre_onboarding_project.md)

---

## Mentor 使用方式

Mentor 可以用這份 roadmap 來：

- 安排新人第一週任務
- 檢查 pre-onboarding project
- 做 code review
- 評估三個月試用期表現
- 設計下一階段成長目標

---

## 最終目標

三個月後，期待新人可以：

- 理解 Stranity 的基本產品與工程架構
- 獨立完成小型 fullstack feature
- 能閱讀與修改現有 codebase
- 能主動 debug 並提出解法
- 能寫出可維護、可 review 的 PR
- 開始具備 feature ownership
