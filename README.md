# Stranity New Hire Roadmap

這份 repository 是給 Stranity 新進工程師使用的 onboarding roadmap，主要對象是 **Golang + React 全端工程師**。

目標不是讓新人短時間內背完所有技術，而是透過清楚的階段目標，逐步建立：

- Production 級工程能力
- Fullstack 開發能力（**後端為主軸，前端會大量靠 AI agent 協助**）
- 團隊協作能力
- 系統思維
- Product Engineering mindset

---

## 適用對象

- Junior Fullstack Engineer
- Backend 轉 Fullstack 工程師
- React Native 轉 React Web 工程師
- 有 CS 基礎但缺少 production 經驗的新進工程師
- Internship / New Grad Engineer

---

## 技術棧

### Backend

- Golang 1.22+
- Gin、GORM
- PostgreSQL、Redis
- JWT、RESTful、WebSocket

### Frontend

- React 18 / 19 + Vite + TypeScript（strict）
- React Router
- Redux Toolkit + RTK Query
- Tailwind CSS + shadcn/ui (Radix UI)
- React Hook Form + Zod
- Vitest + React Testing Library + MSW

### Infra / Tooling

- Docker、Docker Compose
- Linux command line
- Git / GitHub、CI / CD
- AI agent：Claude Code / Cursor / Copilot

---

## 文件結構

```
docs/
├── 01_backend_learning.md         Go 學習路徑
├── 02_frontend_learning.md        React + 新 stack 學習路徑
├── 03_git_and_engineering.md      Git、PR、Linux、Docker 基礎
├── 04_pre_onboarding_project.md   入職前 fullstack project 完整規格
├── 05_first_month_goal.md         第一個月目標與週進度
├── 06_second_month_goal.md        第二個月：能獨立做事
├── 07_third_month_goal.md         第三個月：成為可靠戰力
├── 08_code_style_guide.md         Go / React 風格指南
├── 09_debugging_guide.md          Debug 方法與工具
├── 10_evaluation_checklist.md     雙向評估清單
└── 11_ai_agent_workflow.md        AI agent 協作技巧
```

---

## 建議使用方式

### 入職前

依序閱讀：

1. [`docs/01_backend_learning.md`](docs/01_backend_learning.md)
2. [`docs/02_frontend_learning.md`](docs/02_frontend_learning.md)
3. [`docs/03_git_and_engineering.md`](docs/03_git_and_engineering.md)
4. [`docs/11_ai_agent_workflow.md`](docs/11_ai_agent_workflow.md)
5. [`docs/04_pre_onboarding_project.md`](docs/04_pre_onboarding_project.md)

並完成 pre-onboarding fullstack project。

### 入職後三個月

依序對齊：

1. [`docs/05_first_month_goal.md`](docs/05_first_month_goal.md)
2. [`docs/06_second_month_goal.md`](docs/06_second_month_goal.md)
3. [`docs/07_third_month_goal.md`](docs/07_third_month_goal.md)

並用 [`docs/10_evaluation_checklist.md`](docs/10_evaluation_checklist.md) 每月對齊一次。

### 日常隨時查

- 程式風格：[`docs/08_code_style_guide.md`](docs/08_code_style_guide.md)
- 卡 bug：[`docs/09_debugging_guide.md`](docs/09_debugging_guide.md)
- 用 AI 卡關：[`docs/11_ai_agent_workflow.md`](docs/11_ai_agent_workflow.md)

---

## 我們重視什麼

比起「寫得快」，更重視：

- 可維護性
- 清楚的思考
- Debug 能力
- 溝通能力
- 學習能力
- Ownership
- **會用 AI，但 code 自己負責**

---

## 新人應具備的心態

你不需要一開始什麼都會。

但希望你：

- 願意研究
- 願意問問題
- 願意改善
- 願意持續學習
- 願意對自己寫的功能負責
- 用 AI 但不被 AI 牽著走

工程能力不是靠短期衝刺完成，而是靠每一次 feature、debug、review、refactor 慢慢累積。

---

## Pre-onboarding Project

入職前請完成一個小型 fullstack project。完整規格見 [`docs/04_pre_onboarding_project.md`](docs/04_pre_onboarding_project.md)。

---

## 最終目標

三個月後，期待新人可以：

- 理解公司產品與工程架構
- 獨立完成小型 fullstack feature
- 能閱讀與修改現有 codebase
- 能主動 debug 並提出解法
- 能寫出可維護、可 review 的 PR
- 能用 AI agent 顯著加速但 code 品質自己把關
- 開始具備 feature ownership
