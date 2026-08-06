# Roadmap — streamlit-coco

Living plan. Product detail: [`doc/prd.md`](prd.md). Shipped history: [`CHANGELOG.md`](../CHANGELOG.md).

**Status:** Alpha `0.1.0` published on PyPI (temporary Trusted Publisher: [`lletourmy/streamlit-coco`](https://github.com/lletourmy/streamlit-coco)).  
**Last updated:** 2026-08-06

---

## Now

| Priority | Item | Ref |
| --- | --- | --- |
| P0 | Post-release: switch publisher back to DevoteamSP when org is PyPI-validated | [publish.md](deployment/publish.md) |

Manual checklists: [`doc/features/README.md`](features/README.md) — re-run before release.

---

## Next — v0.2.0

| Item | Ref |
| --- | --- |
| Remote CoCo APIs (no CLI dependency) | [#4](https://github.com/DevoteamSP/streamlit-coco-dev/issues/4) |
| Approval audit log persistence (SEC-01) | — |
| Least-privilege role documentation (SEC-02) | — |
| Deployment docs (Docker, SPCS) | [deployment/](deployment/README.md); PRD §8 / G6 |

---

## Later

### Streaming & transcript UX
- [ ] Auto-scroll to bottom while running; pause if user scrolls up (FR-S2)
- [ ] Richer markdown / SQL highlighting (FR-S6)
- [ ] Copy-to-clipboard on assistant messages and tool results (FR-S7)
- [ ] Optional `max_messages` truncation + “load earlier” (FR-ST4)
- [ ] Theming / a11y pass (native `panel()` transcript; keyboard traps, ARIA)

### Platform & packaging
- [ ] Packaged CCv2 via Vite / `asset_dir` (PRD §14.7; optional if still needed)
- [ ] Community sample apps (≥ 3)
- [ ] SBOM generation in release workflow (SEC-04)
- [ ] Automated UI / e2e tests (promote checklists beyond manual N1)

---

## Out of scope (for now)

- Full IDE workspace
- MCP *product* servers (MCP passthrough via `mcp_servers` works today)
- File upload into `cwd`
- Sub-agents
- Slack integration
- Remote agent proxy / browser-side CoCo (see issue #4 for remote APIs instead)

---

## Success checks (90 days post-launch)

| Metric | Target |
| --- | --- |
| PyPI downloads | 500+ |
| GitHub stars | 50+ |
| Time-to-first-working-app | < 30 min via quickstart |
| Open P0 "streaming broken on rerun" | 0 for 30 days |
| Community examples | ≥ 3 |
