# AGENTS.md — streamlit-coco

Guidance for coding agents working in this repository.

## Product

Streamlit library + optional CCv2 component for Snowflake CoCo (Cortex Code Agent SDK). Preferred UX: `panel()` + app-owned `chat_input_bar` / `st.chat_input`.

Canonical SDK docs: https://docs.snowflake.com/en/user-guide/cortex-code-agent-sdk/cortex-code-agent-sdk  
Pinned rule: `.cursor/rules/coco-sdk-docs.mdc`

## Layout

- `streamlit_coco/` — library (`ui`, `session`, `permissions`, `tool_*`, `bootstrap`, …)
- `examples/` — chat, approval, structured, headless demos
- `doc/prd.md`, `doc/api.md`, `doc/roadmap.md`, `doc/features/` — product + API + DSP N1 feature docs/checklists
- `doc-dev/` — **dev-only** docs (never synced to public `streamlit-coco`)
- `doc/deployment/publish.md` — dual-repo sync + tag → PyPI (`streamlit-coco`)
- `CHANGELOG.md` — shipped history (not the roadmap)

**Repos:** develop in `DevoteamSP/streamlit-coco-dev`; publish from `lletourmy/streamlit-coco` (`make sync-release`, then tag `v*`) until DevoteamSP is PyPI-validated.

## Commands

```bash
make install   # uv sync --extra dev
make check     # ruff + pytest
make audit     # pip-audit
make chat      # Streamlit demo
```

## Conventions

- Do not default-show raw JSON tool payloads; use meaningful compact tool expanders (`doc/features/tools-display/SPEC.md`).
- Approval buttons left→right: Approve once · Always allow · Deny.
- AskUserQuestion / ExitPlanMode always go through pending HITL; never “Always allow”.
- Update `CHANGELOG.md` `[Unreleased]` for user-visible changes; update feature checklists when UX changes.
- **Before cutting a release:** clean + update `CHANGELOG.md` **and** `doc/roadmap.md` (see `doc/deployment/publish.md`). Do not tag until both are current.
- Prefer small, focused diffs; no drive-by refactors.

## Testing

- Unit/smoke: `tests/`
- Manual UI: `doc/features/*/test-checklist.md` before release
