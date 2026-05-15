# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo purpose

This is a **public, documentation-only repository** — Stranity's open onboarding roadmap for **Golang + React fullstack engineers** (backend-leaning). It is read by candidates who have an offer, new hires before their start date, and external readers evaluating Stranity's engineering culture.

Because the repo is public:

- **Do not** add company-internal details (specific repo names, file paths, tool names, internal flows, mentor scripts, evaluation rubrics that candidates could game).
- Stranity-internal versions of the same material belong in a private repo.

There is no application code, build system, tests, or package manager here — edits are Markdown only.

## Document structure

All content lives in `docs/`, numbered for reading order:

- `01_backend_learning.md` — Go fundamentals through testing
- `02_frontend_learning.md` — React + new stack, deliberately lighter depth since hires are backend-leaning
- `03_git_and_engineering.md` — Git, branching, commits, PR flow, code review, Linux, Docker
- `04_pre_onboarding_project.md` — Strategy Management System full spec
- `05_first_month_goal.md` — week-by-week onboarding plan
- `06_second_month_goal.md` — independent feature ownership
- `07_third_month_goal.md` — executor → owner, probation evaluation
- `08_code_style_guide.md` — Go and React style with examples
- `09_debugging_guide.md` — scientific method, BE/FE tooling
- `10_evaluation_checklist.md` — three-tier (✅/🔶/⬜) self-check
- `11_ai_agent_workflow.md` — Claude Code / Cursor / Copilot use

The roadmap is **read sequentially**. When editing, preserve a doc's position in the learning arc — content meant for month 2 must not appear in month 1 material.

## Tech stack the docs assume

Backend: Go 1.22+, Gin, GORM, Postgres, Redis, JWT.

Frontend: React 18/19 + Vite + TypeScript strict, Redux Toolkit + RTK Query, Tailwind + shadcn/ui (Radix), React Hook Form + Zod, Vitest + RTL + MSW.

Ant Design is treated as legacy — don't re-introduce it as required.

## Writing style

- **Traditional Chinese (繁體中文) is the primary language.** Use English only for technical terms, file paths, and code.
- **Short declarative sentences**, often one per line. Avoid paragraphs.
- **Heavy use of bulleted lists.**
- **`---` horizontal rules** between subsections — intentional pacing, not decoration.
- Each doc opens with a "## 這份文件給誰看" / "## 目標" / "## 心法" or equivalent intent-stating section.
- Sections like "不需要追求" / "不該做的事" / "紅旗" set scope ceilings — keep them when adding new material.
- No emojis except semantic markers already in use (✅/🔶/⬜ for checklist tiers; ❌/✅ for bad/good comparisons).

## Commit and branch conventions (from CONTRIBUTING.md)

- Branch prefixes: `feature/`, `fix/`, `refactor/`, `chore/`, `docs/`
- Commit prefixes: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:` — never bare `update`, `fix bug`, `test`

## Cross-doc consistency rules

- A change to month N's expectations may break the difficulty progression in month N+1. Read adjacent month files before raising or lowering the bar.
- `10_evaluation_checklist.md` is the rubric — any new "should be able to" claim in months 1–3 needs a matching checklist item, and vice versa.
- `04_pre_onboarding_project.md`'s tech requirements are the floor for everything that follows. Don't introduce new required tech in later docs without adding it to the pre-onboarding learning material first.

## Public vs private boundary

If you're tempted to add any of the below to a doc here, it belongs in a private repo instead:

- Specific company repo names, paths, or file references
- Internal tool names (issue tracker, CI platform, log platform, Slack channel structure)
- Mentor-facing scripts, onboarding day-by-day playbooks
- Real past-hire incidents, red-flag examples with details
- Probation evaluation rubrics that candidates could rehearse against
- Anything that wouldn't be appropriate in a job-description-attached document
