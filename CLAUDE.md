# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo purpose

This is a **documentation-only repository** — the onboarding roadmap for Stranity new-hire **Golang + React fullstack engineers** (backend-leaning). There is no application code, no build system, no tests, no package manager. Edits here are Markdown only.

Audience: junior fullstack, backend-to-fullstack converts, React Native-to-Web converts, interns/new grads. Spans **pre-onboarding → end of month 3**, with an additional 6-month outlook.

## Document structure

All content lives in `docs/`, numbered for reading order:

- `01_backend_learning.md` — Go fundamentals through testing (8 phases, junior-friendly with timing, resources, pitfalls, exercises)
- `02_frontend_learning.md` — React + new stack (Tailwind, shadcn/ui, RTK Query, RHF+Zod). Backend-leaning hires use AI to fill gaps
- `03_git_and_engineering.md` — Git, branch strategy, commit conventions, PR flow, code review, Linux, Docker
- `04_pre_onboarding_project.md` — **Strategy Management System** full spec (story, DB schema, API spec, UI pages, file structure, 2-week timeline, README template, Q&A)
- `05_first_month_goal.md` — week-by-week onboarding (setup, micro-PRs, first meaningful PR, standing up)
- `06_second_month_goal.md` — small feature ownership, design docs, PR review
- `07_third_month_goal.md` — executor → owner, probation evaluation, 6-month outlook
- `08_code_style_guide.md` — Go and React style with concrete bad/good examples, unified API response shape
- `09_debugging_guide.md` — scientific method, BE/FE toolboxes, common bug patterns, AI-assisted debug
- `10_evaluation_checklist.md` — three-tier (✅/🔶/⬜) mentor-newhire dialogue tool, red-flag catalog
- `11_ai_agent_workflow.md` — Claude Code / Cursor / Copilot use, prompting, ticket-to-PR workflow, common pitfalls

The roadmap is **read sequentially**. When adding or editing a doc, preserve its position in the learning arc — content meant for month 2 must not appear in month 1 material.

## Stranity frontend tech stack (important)

The **current** frontend stack (matching `react-paraninja`, `react-eltryn`) is:

- React 18/19 + Vite + TypeScript strict
- Tailwind CSS + shadcn/ui (Radix UI primitives)
- Redux Toolkit + RTK Query
- React Hook Form + Zod
- Vitest + React Testing Library + MSW
- openapi-typescript codegen

**Ant Design is legacy**. Older repos still use it, but **new repos and new features should not**. If you edit docs that mention frontend stack, do not re-introduce AntD as a required tool.

Stranity fullstack engineers are **backend-leaning**. Frontend depth in docs is deliberately lower than backend depth, and AI-agent assistance is explicitly encouraged for frontend work.

## Writing style conventions

The existing docs follow a deliberate house style. Match it:

- **Traditional Chinese (繁體中文) is the primary language.** Use English only for technical terms (`useEffect`, `JWT`, `transaction`), file paths, and code.
- **Short declarative sentences**, often one per line. Avoid paragraphs.
- **Heavy use of bulleted lists** for skills, requirements, and goals.
- **`---` horizontal rules** between every subsection (this is intentional pacing for readers, not decoration).
- Lead each doc with a "## 這份文件給誰看" / "## 目標" / "## 心法" or equivalent intent-stating section.
- Sections like "不需要追求" / "不該做的事" / "紅旗" are used to set scope ceilings — keep them when adding new material.
- Do not add emojis except as semantic markers used elsewhere in the same doc (✅/🔶/⬜ for checklist tiers; ❌/✅ for bad/good code comparisons).

## Commit and branch conventions (from CONTRIBUTING.md)

- Branch prefixes: `feature/`, `fix/`, `refactor/`, `chore/`, `docs/`
- Commit prefixes: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:` — never bare `update`, `fix bug`, `test`
- Recent commits in this repo all use `docs:` prefix with imperative subject

## Stranity product context

The roadmap targets engineers who will work on Stranity's four products. Companion repos live alongside this one under `~/stranity/`:

- **eltrade** — `eltrade-worker/`, `eltrade-watchdog/`, `eltrade-sdk-python/` (Go backend services + Python SDK)
- **gobe** — `gobe2/` (Go backend; has AGENTS.md)
- **paraninja** — `react-paraninja/` (frontend; has CLAUDE.md, AGENTS.md, API_CODEGEN.md). **Primary reference for new frontend stack**.
- **sikax** — `sikaz/`, `sikax_tool/` (Go services with CI scripts and CLAUDE.md)
- **eltryn** — `react-eltryn/`, `react-eltryn-admin/` (additional frontend references for new stack)

When the roadmap references "現有 codebase" or "existing code", these are the repos new hires will read. Cross-reference them when adding examples, but **do not embed code from those repos here** — link to concepts, not implementations, so the roadmap stays evergreen.

## When editing roadmap content

- A change to month N's expectations may break the difficulty progression in month N+1. Read adjacent month files before raising or lowering the bar.
- `10_evaluation_checklist.md` is the rubric mentors grade against — any new "should be able to" claim in months 1–3 must have a matching checklist item, and vice versa.
- `04_pre_onboarding_project.md`'s tech requirements (Gin/GORM/Postgres/Tailwind/shadcn/RTK Query) are the floor for everything that follows. Don't introduce new required tech in later docs without adding it to the pre-onboarding learning material first.
- The `examples/` and `.github/` directories referenced in the original README scaffold were never created and have been removed from the README. If a future task needs example artifacts (e.g., PR templates, API response samples), create the directory plus the file and reference it from README — don't leave dangling references.
