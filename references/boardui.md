# BoardUI 组件方法与接入

Date: 2026-11-19 (Asia/Shanghai)
Scope: official catalog, source repository, license, representative composition and acceptance risks. Read-only 组件方法与接入; nothing installed or wired into a business app.

## Current entry points and findings

- Official catalog: https://www.boardui.com/components (the rendered index currently labels 107 items; it distinguishes Base, Blocks, Charts and Templates, and marks Pro items).
- Installation and agent guidance: https://www.boardui.com/getting-started ; CLI examples use `npx boardui@latest add <slug>`. It copies source files and internal dependencies into the project.
- GitHub source: https://github.com/BoardUI/boardui . The repository tree exposes `components/application`, `components/base`, and `components/foundations`; README catalog calls out the free tier and says repo is generated/no PRs.
- License: repo `LICENSE` was read via Cindy GitHub plugin; it is MIT (Copyright 2026). Current official licensing page https://www.boardui.com/license says free source listed publicly is MIT, while PRO source requires a paid license; PRO license restricts redistribution/competing UI kit use. Check each catalog badge before copying. This differs from the free repo-wide MIT grant only in that paid sources are excluded.
- Design/runtime: React 19.2 + Tailwind v4 per homepage; React Aria primitives, semantic tokens, direction-aware RTL; source not runtime package. MCP is optional discovery/install route. README says chat starter app includes API route using a user-supplied key; never treat demo server setup as a required component dependency.

## Representative blocks (catalog + source/tree evidence)

1. **Agent Chat / Agent Thinking / Agent Log** — AI product UI. Chat combines sidebar/history rail, streaming assistant messages, thinking indicator, composer with stop control, attachment affordances and setup notice. Agent Thinking offers four visual variants with shimmering label and elapsed timer; Agent Log supplies reveal/blur/curved tree primitives underlying Task List and Web Search. Compose as a state flow: stream starts → thinking/timer → event/task log updates → response → composer enabled. CLI slugs include `agent-chat`, `agent-thinking`, `agent-log`. Official page observed: https://www.boardui.com/components/agent-thinking . Source directories confirmed under `components/application/agent-chat`, `agent-thinking`, `agent-log` in repository. Do not represent thinking/log blocks as actual model reasoning; wire only safe user-visible status events.
2. **Data Table** — TanStack Table powered filtering, sorting, selection, row status/actions, pagination. Page shows filter selects + search + table + result count; public install slug `data-table`; source path family `components/application/data-table`. Use server queries for durable business records; own query params, authorization, empty/error/loading states and mutations. Demo data is not business connection.
3. **Data Grid** — editable virtualized spreadsheet, range selection, filters, resizable columns, undo/redo, summaries, CSV export; slug `data-grid`, source under application blocks. Wire save semantics, permission checks, validation and CSV export privacy before accepting edits; virtualized cells require keyboard/selection and screen-reader checks.
4. **Notification Center** — grouped activity inbox, unread state, tabs, avatars, status icons and inline actions; slug `notification-center`. Connect event source and idempotent read/action endpoints; verify tab/filter persistence, unread count convergence and permission boundaries.
5. **Stat Cards / Revenue Chart Card** — KPI summary with delta chips; revenue card composes monthly area series against prior year, count-up headline, delta and hover month readout. Source docs and readme describe these as free `stat-cards`/`revenue-chart-card` blocks; many chart variants in catalog are PRO. Must map correct units, currency, interval, missing-data behavior and accessible textual summary.

## API/source/dependencies

- Registry/CLI is the real entry point: `npx boardui@latest add <slug>`; page Installation has manual code alternative. Catalog pages expose copyable code. The root app is not the component API.
- GitHub source tree and README were inspected, including root LICENSE and `components/application`/`components/base`; no package was installed, no component file copied locally, no business API wired.
- React Aria for interactions/accessibility; Tailwind v4 and BoardUI token/foundation files are important shared dependencies. Exact transitive package list varies by item: let CLI resolve, inspect diff/package changes first. Do not bring a block's global token CSS over a different design system wholesale.

## Acceptance risks and status

Observed docs/catalog/source only. No rendered local preview, browser QA, app integration, API contract or runtime tests were performed. Acceptance should cover 320px/narrow viewport, dark mode, keyboard and screen-reader semantics, loading/empty/error states, real server pagination and sorting semantics, stale/conflicting row updates, timezone/currency formatting, and bundle changes. For Agent Chat verify stream cancellation/error/retry and server-side key handling. License verification was performed at current docs and repository LICENSE; inspect item license badge at install time.

## Evidence URLs

- https://www.boardui.com/components
- https://www.boardui.com/components/data-table
- https://www.boardui.com/components/agent-thinking
- https://www.boardui.com/getting-started
- https://www.boardui.com/license
- https://github.com/BoardUI/boardui (root tree, README and LICENSE read using Cindy GitHub plugin)

## Source body read (2026-11-19)

Read the complete 8,616-byte `components/application/agent-thinking/agent-thinking.tsx` from GitHub (`main`, blob `f11a3784bdb1324be6f5d252c7ce96e4c1b3c558`).

- Props are exactly `variant?: "wave" | "spin" | "stars" | "infinity"`, `label`, `tone`, `shimmer`, `showTimer`, `className`; default wave/Thinking/shimmer+timer.
- Module composes `DotsIndicator`, `StarsIndicator`, `InfinityIndicator`, `ElapsedTimer`. It uses CSS variables/semantic classes, `cx`, `useTemplateCopy`; wave/spin updates dot opacities with an interval, while reduced-motion detection stops that interval. Timer uses performance.now and updates every 100ms. Root has `role="status"`.
- Business connection is intentionally absent: this is a presentational status primitive. Mount it from the app's explicit request pending state, unmount/replace when terminal/error/cancel state arrives. Pass a concise status label; do not expose private chain-of-thought. Check template-copy dependency and CSS animations/global tokens when using this source alone.

This is full source read, not a local run or render acceptance.
