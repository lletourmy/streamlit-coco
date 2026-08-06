# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Versioning: date-based (`YYYY-MM-DD`) until v1.0.0 is cut, then semantic versioning.
Package version in `pyproject.toml` is `0.1.0` (first PyPI release).

Living plan (what’s next): [`doc/roadmap.md`](doc/roadmap.md).

---

## [Unreleased]

## [0.1.0] — 2026-08-06

First PyPI release of `streamlit-coco` (alpha). Published from temporary Trusted Publisher on [`lletourmy/streamlit-coco`](https://github.com/lletourmy/streamlit-coco).

### Changed
- **Temporary PyPI release repo** — `make sync-release` / Trusted Publisher gate target [`lletourmy/streamlit-coco`](https://github.com/lletourmy/streamlit-coco) until DevoteamSP is validated on PyPI ([`doc/deployment/publish.md`](doc/deployment/publish.md))
- **Dependabot** — bump transitive `cryptography` to `50.0.0` (and `pyopenssl` to `26.4.0`); `GitPython` already at `3.1.58`

### Added

#### Examples
- **Product Backlog Desk** — multipage demo (`examples/backlog_desk/`, `make backlog`): Board / Epic / Ticket / Release with a right-rail Copilot (skills + `panel()`) over local JSON/Markdown (no SQL); navigator-style theme; Edit/Write gated by approvals

### Changed
- **Compact tool cards** — transcript tools render as collapsed expanders (label: family · status · meta); auto-open on error; CCv2 `<details>` parity ([`doc/features/tools-display/SPEC.md`](doc/features/tools-display/SPEC.md))

#### Dual-repo publish
- Public release repo (temporary) [`lletourmy/streamlit-coco`](https://github.com/lletourmy/streamlit-coco); `make sync-release` / `scripts/sync_release.sh`; guide [`doc/deployment/publish.md`](doc/deployment/publish.md)
- Apache-2.0 `LICENSE`; PyPI Trusted Publisher gate in `release.yml` (`github.repository == lletourmy/streamlit-coco` only)

#### Phase 3 — HITL, headless, render flexibility
- **Headless multi-turn** — `CocoSession.stream()` and `await session.run(prompt)`; `execute_plan()` / `set_permission_mode()`; extended `examples/headless_pipeline.py`
- **Streamlit-free core imports** — lazy `__getattr__` for UI exports so headless scripts never load Streamlit; smoke test + example assert
- **Plan mode Execute CTA** — native `render_plan_banner()` in `panel()`; CCv2 banner **Execute plan** trigger
- **Edit/Write unified diff** — approval + transcript previews via `difflib` (`tool_extract.unified_diff`); Before/After fallback when empty
- **Pluggable text renderer** — `text_renderer=` on `panel()`, `render_transcript()`, `render_output_field()` (`markdown` / `write` / `text` / … or callable); feature docs under `doc/features/text-renderer/`
- **App-owned `request_input`** — form + optional multi-field `schema=` (AskUserQuestion remains the in-turn CoCo channel)
- Headless checklist re-signed (2026-07-27): `query()` + `run()` + `stream()` live path; no Streamlit import

#### Earlier unreleased (pre–Phase 3 on this branch)
- **Clear tool “running” captions when done** — parse SDK `UserMessage` / NDJSON `user` tool results; finalize leftover `running` tools on turn `result`
- **CCv2 skill hygiene** — JS cleanup via AbortController; pause `run_every` on pending approval; drop `provide_input`; `isolate_styles=True`; CSS via `--st-yellow-*` / `--st-red-*` / radius tokens
- **API reference** — [`doc/api.md`](doc/api.md)
- **Deployment docs (local)** — [`doc/deployment/local.md`](doc/deployment/local.md)
- **Typed error hierarchy** — `streamlit_coco.errors`; `require_environment()`
- **NDJSON fixture corpus** — `tests/fixtures/ndjson/` + `tests/test_ndjson_fixtures.py`
- Feature docs pack + checklist sign-offs (panel, approvals, tools-display, structured-output, chat-ccv2, headless)
- GitHub CI/CD (ci / security / release + optional PyPI publish on `v*` tags); `make publish`; hatch sdist excludes for agent/IDE dirs
- Smoke tests: CCv2 register-once; core import does not load Streamlit

### Changed
- [`doc/prd.md`](doc/prd.md) reconciled with shipped Phase 3; FR-S* / FR-ST* status; FR-S3 superseded by tools-display SPEC (not “collapse cards”)
- [`doc/roadmap.md`](doc/roadmap.md) — Later: FR-S2, FR-S7, FR-ST4 truncation, Vite/CCv2, e2e; Next: Docker/SPCS docs; PyPI `0.1.0` via temporary Trusted Publisher
- `doc-dev/` — development-only docs tree; excluded from `make sync-release` and sdist (never published to `streamlit-coco`)
- Package / README / identity URLs point at the public [`streamlit-coco`](https://github.com/lletourmy/streamlit-coco) repo; development continues on [`streamlit-coco-dev`](https://github.com/DevoteamSP/streamlit-coco-dev)
- GitHub repository renamed to [`DevoteamSP/streamlit-coco-dev`](https://github.com/DevoteamSP/streamlit-coco-dev) (package name remains `streamlit-coco`)
- Examples `structured_output.py` / `approval_gate.py`: `get_or_create_session` + eager `start()` for CCv2 transcript across reruns
- Chat demo sidebar: compact status badges; Settings popover; test prompts behind a toggle
- CCv2 `chat()` registration cached (`@lru_cache`) so `st.components.v2.component` runs once per process
- Headless example: separate event loops for `query()` vs `CocoSession` to avoid SDK cancel-scope teardown issues

### Fixed
- Grep / Glob completed cards: compact summary instead of dumping full result bodies
- AskUserQuestion: free-form / “Other…” options always last in radio / multiselect
- Security workflow: free Gitleaks CLI instead of `gitleaks-action@v2` (org license)

---

## [2026-07-24]

### Added
- **Tools display & user interactions** — full spec + implementation ([`doc/features/tools-display/SPEC.md`](doc/features/tools-display/SPEC.md))
  - Meaningful bordered tool cards (no default JSON expanders) for Glob, Grep, Read, Write, Edit, Bash, SQL / `sql_execute`, AskUserQuestion, ExitPlanMode, and generic / MCP tools
  - `streamlit_coco.tool_names`, `tool_extract`, `tool_cards` dispatch; CCv2 frontend parity
  - AskUserQuestion UI: radio / multiselect, **Other…** free-text, Submit / Cancel; always routed through `can_use_tool`
  - SQL card: query code block + dataframe / text results; SQL preview on approval
  - ExitPlanMode: Approve plan / Reject (with optional feedback); never “Always allow”
  - CoCo debug mode (`STREAMLIT_COCO_DEBUG` / `COCO_DEBUG` / `st.session_state["coco_debug"]`) for collapsed **Raw tool payload**
- Feature checklist + `display_*` test prompt pack ([`doc/features/tools-display/test-checklist.md`](doc/features/tools-display/test-checklist.md), [`examples/testdata/prompts.json`](examples/testdata/prompts.json) v2 — 50+ prompts)
- Chat demo: Plan mode toggle, debug checkbox, test-prompt runner by category

### Changed
- Approval button order (left → right): **Approve once** · **Always allow** · **Deny** (Deny rightmost); AskUser Submit · Cancel; plan Approve · Reject
- Tool approvals show family-specific previews (path, content, Before/After, command, SQL) instead of raw JSON by default

---

## [2026-07-23]

### Added
- `streamlit_coco.bootstrap` — `check_environment` / start gate helpers, `get_or_create_session`, `chat_input_bar`, `reset_session`, `stop_session`
- `streamlit_coco.diagnostics` — `CocoEnvironment` probe (CLI, SDK, Snowflake config) without starting an agent
- Session readiness lifecycle — `CONNECTING` → `READY` / `ERROR`, `ensure_ready()`, init metadata capture
- Soft status chrome in `panel()` — Starting / Thinking / tool activity / Needs approval without remount flicker
- DSP N1 feature test checklists under `doc/features/*/test-checklist.md`
- `doc/roadmap.md` — Now / Next / Soon / Later plan aligned with `doc/PRD.md`
- `Makefile` targets for install, test, lint, format, check, build, and example apps
- Cursor rule pinning Cortex Code Agent SDK docs as source of truth

### Changed
- Preferred app pattern documented as `panel()` + `chat_input_bar` / `st.chat_input` (legacy `chat()` retained)
- `doc/PRD.md` and `README.md` synced to the implemented `panel()`-first API and package layout
- Example `examples/chat_app.py` simplified around bootstrap helpers

### Fixed
- Avoid double-display of Snowflake connections TOML path in the environment / start gate UI
- `chat_input_bar` — graceful fallback when Streamlit lacks `submit_mode` (< 1.59)
- Session status skeleton — fallback placeholder when `st.skeleton` is unavailable (< 1.59)

---

## [2026-07-22] — Alpha baseline (shipped)

Core library and preferred Streamlit UX first landed:

- [x] Pip-installable package + `CocoOptions` / `CocoSession` / `query()`
- [x] Native UI: `panel()` + app-owned input (`chat_input_bar` / `send_prompt`)
- [x] Streaming transcript, tool cards, Stop, fragment polling
- [x] Human-in-the-loop approvals (`require_approval_for`, Deny / Approve once / Always)
- [x] Structured output (inline JSON or `on_structured_output`)
- [x] Legacy CCv2 `chat()` (still supported)
- [x] Examples: chat, approval gate, structured output, headless pipeline

### Added (detail)
- Initial `streamlit-coco` package (`0.1.0` alpha): Python API + Streamlit embedding for Snowflake CoCo
- Normalized `CocoEvent` model and unit tests (`tests/test_core.py`)
- Legacy CCv2 `chat()` component with static frontend assets under `streamlit_coco/frontend/`
- `doc/PRD.md` and `README.md`

---

<!-- Notes:
- Link PRs/issues when available: (#42) or (DevoteamSP/streamlit-coco-dev#42)
- One entry per user-visible change
- Security fixes always under "Security", never under "Fixed"
- Update [Unreleased] as you go; rename to a date (or semver after v1.0.0) on release
-->
