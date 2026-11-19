# Kibo UI 组件方法与接入

Date: 2026-11-19 (Asia/Shanghai)
Scope: official component directory/setup, registry source locations, license and real-use risks. Read-only; no CLI/install/business wiring.

## Current entry points and findings

- Directory: https://www.kibo-ui.com/components ; introduction: https://www.kibo-ui.com/docs ; setup: https://www.kibo-ui.com/docs/setup ; usage: https://www.kibo-ui.com/docs/usage . The current directory includes collaboration, project management, code, forms, images, finance and style groups.
- GitHub official repository: https://github.com/shadcnblocks/kibo ; repo describes custom registry built on shadcn/ui. Actual package paths inspected via Cindy GitHub plugin: `packages/gantt/index.tsx`, `packages/dropzone/index.tsx`, `packages/color-picker/index.tsx`, `packages/code-block/index.tsx`; each has package metadata, proving components are actual source rather than only screenshots.
- License: official docs and GitHub listing state MIT; repo has `license.md`. The repo path listing confirms that file, but its exact body was not fetched. Treat license claim as docs/repo metadata confirmation, not legal review.
- Setup explicitly requires React and initialized shadcn/ui, configured Tailwind and CSS Variables mode. `npx kibo-ui add <slug>` (or shadcn CLI) copies editable component code plus declared dependencies to configured components path. It is additive source; not a runtime library contract.

## Representative components and composition

1. **Gantt** — draggable/resizable timeline items, markers, today marker, grouping and multiple items per row. Docs preview uses hierarchical task timeline. Source `packages/gantt/index.tsx` (~40KB). Compose with project selector + range controls + grouped rows + task details editor. Real integration must map task IDs, start/end, dependencies, grouping and write operations; validate DST/time zones, date granularity, overlaps, drag constraints and mobile scrolling. No actual backend/date adapter wiring verified.
2. **Dropzone** — upload area with file selection/drop interaction. Source `packages/dropzone/index.tsx` (~5KB). Compose with file list, progress, retry/remove and server result state; validate MIME/size server-side, cancellation, duplicate files, rejected file reasons, presigned upload/auth, keyboard fallback and progress. Demo does not supply persistence.
3. **Color Picker** — Figma-like interactive picker, drag/drop, hue and alpha sliders, EyeDropper, HEX/RGB/CSS/HSL and live preview. Source `packages/color-picker/index.tsx` (~13KB). Compose with color swatch, format input, opacity, reset/apply. EyeDropper permission/browser support is conditional; validate color conversion round trips, contrast, alpha serialization, keyboard, dark mode and unsupported browser fallback.
4. **Code Block** — syntax-highlighted code presentation; client source `packages/code-block/index.tsx` (~15KB), separate `server.tsx` (~1.6KB), indicating server/client entry distinction. Compose with language label, copy/download, collapsible or diff context. Check highlighter dependency/bundle, SSR hydration, untrusted code rendering, long-line overflow, clipboard permission and language fallback.
5. **Kanban / Table / Calendar** — project management surfaces listed in official directory. They provide alternate status/calendar/table views for the same task set. Compose using one canonical task model and shared filters, with each view emitting stable task IDs/actions. Docs listing verified, but source/package metadata was not inspected for these three in this pass.

## Dependencies, API and source entry

- Official setup says `npx kibo-ui add gantt` (same convention for each slug); docs explain the CLI adds source and any required dependency. Kibo source is organized as per-component packages; checked package metadata exists for gantt, dropzone, color-picker, code-block. Component docs provide feature details/install command; usage page says component code remains in project and can be customized.
- shadcn must already be initialized. Tailwind/shadcn theme CSS variables are a hard setup constraint. Exact component dependencies need reading from its `package.json` or CLI diff for the selected slug. No installation was executed.

## Acceptance risks and status

Observed current docs, public repo directories/source-file existence and setup requirements; no source file body was executed/installed and no actual business connection/rendered preview was made. Gantt needs date/timezone, drag/drop, narrow-screen and keyboard acceptance. Dropzone needs server-side validation and persistence behavior. Color picker browser API permissions and color-space edge cases require fallback coverage. Code block needs SSR, XSS, long-line, clipboard and dependency checks. Confirm no conflict with current shadcn component versions and CSS variable theme before adding. License statement verified from official docs and repo listing; did not inspect individual third-party dependency licenses.

## Evidence URLs

- https://www.kibo-ui.com/docs
- https://www.kibo-ui.com/docs/setup
- https://www.kibo-ui.com/docs/usage
- https://www.kibo-ui.com/components/gantt
- https://www.kibo-ui.com/components/color-picker
- https://github.com/shadcnblocks/kibo (repo tree)
- https://github.com/shadcnblocks/kibo/tree/main/packages/gantt
- https://github.com/shadcnblocks/kibo/tree/main/packages/dropzone
- https://github.com/shadcnblocks/kibo/tree/main/packages/color-picker
- https://github.com/shadcnblocks/kibo/tree/main/packages/code-block

## Full registry/source read: Dropzone

Read complete `packages/dropzone/index.tsx` (5,044 bytes; blob `4633adc6ea60d0c624ac1147c028251e13e28e78`), full `packages/dropzone/package.json` (519 bytes; blob `837bf085a69854b9153759d296b5c432e1c3e81e`), and `license.md` (1,068 bytes; blob `e01f0f824a42e798cf03032f6f0f1e0023ae7f12`) from the official `shadcnblocks/kibo` GitHub repository via Cindy GitHub plugin. `license.md` contains the MIT grant, notice-retention condition and as-is/no-warranty clauses; official docs also label the repo MIT.

- Imports `useDropzone`/types from `react-dropzone`, `UploadIcon` from `lucide-react`, and local shadcn `Button`/`cn`. Exports `Dropzone`, `DropzoneContent`, `DropzoneEmptyState` and their prop types.
- `DropzoneProps` extends `Omit<DropzoneOptions, "onDrop">` and adds optional `src?: File[]`, `className`, `onDrop(acceptedFiles, fileRejections, event)`, children. It sets `maxFiles = 1` by default; forwards `accept`, size limits, max files, error/disabled and other dropzone options into `useDropzone`. If there are rejections it calls `onError` with the first rejection's first error then returns; accepted files are delivered through `onDrop`. `src` is used to render the existing-file content and keyed as JSON to reset internal widget state when it changes. Empty/content children can be customized.
- It is an input/acceptance widget, not an uploader: source contains no network call, progress, persistence, virus scan, retry, or server validation. Application must upload returned `File[]`, enforce authorization and server MIME/size constraints, then update `src` with persisted display state. UX acceptance must check rejection explanation (the component only forwards first failure to `onError`), keyboard activation, disabled state, multiple-file rules, and large file behavior.
- Package metadata says dependencies include `react-dropzone ^14.3.8`, `lucide-react ^0.545.0`, React/ReactDOM `^19.2.0`, and workspace shadcn UI. `@repo/*` aliases are monorepo-specific; Kibo CLI resolves into app source, so inspect the generated imports and package changes before claiming plug-and-play. The registry being component source means app's shadcn primitives/theme are still prerequisites.

Full source and package metadata were read; no installation, upload endpoint, browser interaction, or business integration was exercised. This clarifies prior notes: rendered docs call it an upload zone, but implementation hands files to consumer and does not perform upload.
