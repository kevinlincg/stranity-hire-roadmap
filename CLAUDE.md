# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repo purpose

This is a **documentation-only repository** — the onboarding roadmap for Stranity new-hire **Golang + React fullstack engineers**. There is no application code, no build system, no tests, and no package manager. Edits here are Markdown only.

Audience: junior fullstack, backend-to-fullstack converts, React Native-to-Web converts, interns/new grads. Spans **pre-onboarding → end of month 3**.

## Document structure

All content lives in `docs/`, numbered for reading order:

- `01_backend_learning.md`, `02_frontend_learning.md` — pre-onboarding self-study (Go/Gin/GORM/Postgres; React/Vite/TS/AntD/Zustand/TanStack Query/Tailwind)
- `04_pre_onboarding_project.md` — the **Strategy Management System** spec (login + JWT + CRUD + pagination + Docker Compose). This is the artifact mentors evaluate before day 1.
- `05_first_month_goal.md`, `06_second_month_goal.md`, `07_third_month_goal.md` — progressive milestones for the 3-month probation
- `08_code_style_guide.md`, `09_debugging_guide.md`, `10_evaluation_checklist.md` — cross-cutting references mentors use during reviews

The roadmap is **read sequentially**. When adding or editing a doc, preserve its position in the learning arc — content meant for month 2 must not appear in month 1 material.

## Known drift in README.md

`README.md` advertises content that does not exist on disk. If you touch the README or add files, reconcile these:

- `docs/03_git_and_engineering.md` — referenced but missing
- `examples/` directory (`api_response_format.md`, `commit_message_examples.md`, `project_structure_examples.md`) — referenced but missing
- `.github/PULL_REQUEST_TEMPLATE.md` and `.github/ISSUE_TEMPLATE/` — referenced but missing

Either create the file or update the README — don't leave dangling references.

## Writing style conventions

The existing docs follow a deliberate house style. Match it:

- **Short declarative sentences**, often one per line. Avoid paragraphs.
- **Heavy use of bulleted lists** for skills, requirements, and goals.
- **`---` horizontal rules** between every subsection (this is intentional pacing for readers, not decoration).
- **Mixed Traditional Chinese + English** is expected. Docs 01–05 lean Chinese; docs 06–10 lean English. Don't force a single language — match the file you're editing.
- Lead each doc with a `## 目標` / `## Goal` section stating intent.
- Sections like "不需要追求" / "Avoid" are used to set scope ceilings — keep them when adding new material.
- Do not add emojis, badges, or decorative formatting.

## Commit and branch conventions (from CONTRIBUTING.md)

- Branch prefixes: `feature/`, `fix/`, `refactor/` (e.g. `feature/login-api`, `fix/strategy-pagination`)
- Commit prefixes: `feat:`, `fix:`, `refactor:`, `docs:` — never bare `update`, `fix bug`, or `test`
- Recent commits in this repo follow `docs:` / verb-first style (`add evaluation checklist`, `add debugging guide`)

## Stranity product context

The roadmap targets engineers who will work on Stranity's four products. Companion repos live alongside this one under `~/stranity/`:

- **eltrade** — `eltrade-worker/`, `eltrade-watchdog/`, `eltrade-sdk-python/` (Go backend services + Python SDK)
- **gobe** — `gobe2/` (Go backend; has its own AGENTS.md)
- **paraninja** — `react-paraninja/` (React frontend; has CLAUDE.md, AGENTS.md, API_CODEGEN.md)
- **sikax** — `sikaz/`, `sikax_tool/` (Go services with CI scripts and CLAUDE.md)

When the roadmap references "現有 codebase" or "existing code", these are the repos new hires will read. Cross-reference them when adding examples, but **do not embed code from those repos here** — link to concepts, not implementations, so the roadmap stays evergreen.

## When editing roadmap content

- A change to month N's expectations may break the difficulty progression in month N+1. Read adjacent month files before raising or lowering the bar.
- `10_evaluation_checklist.md` is the rubric mentors grade against — any new "should be able to" claim in months 1–3 must have a matching checklist item, and vice versa.
- `04_pre_onboarding_project.md`'s tech requirements (Gin/GORM/Postgres/AntD/Docker Compose) are the floor for everything that follows. Don't introduce new required tech in later docs without adding it to the pre-onboarding learning material first.
