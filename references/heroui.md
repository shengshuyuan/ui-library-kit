# HeroUI 组件方法与接入

Date: 2026-11-19 (Asia/Shanghai)
Scope: v3 React documentation, official source/license, representative components and risks. Read-only; no install or business wiring.

## Current entry points and findings

- Official v3 React docs: https://heroui.com/en/docs/react/getting-started ; all components: https://heroui.com/en/docs/react/components ; index: https://heroui.com/llms.txt . Official site describes HeroUI v3 as Tailwind CSS v4 + React Aria Components, React 19+, with compound components and CSS-first theme tokens.
- GitHub: https://github.com/heroui-inc/heroui ; source branch `v3`, components under `packages/react/src/components/`. Cindy GitHub plugin confirmed source file listings for Button, Table, Calendar. The Table implementation is 468 lines/14.5KB; Calendar implementation is ~16KB. This verifies actual implementation location, not runtime behavior.
- License: GitHub v3 `LICENSE` fetched and read; Apache License 2.0. Official docs also state Apache-2.0. Do not confuse React Native product line with React web package.
- Docs show current latest release as v3.2.7 (Oct 28 2026). v2 is separately documented; do not mix v2 API/examples/provider/style wiring with v3.
- Installation packages in official v3 release docs: `@heroui/styles` and `@heroui/react`; exact peer dependencies should be verified from the chosen release's package metadata before installation. React Aria and Tailwind v4 are architectural dependencies.

## 5 representative components and composition

1. **Table** — composed table primitives with sortable columns, row selection, column resizing, async loading, custom cells and virtualization for large datasets (docs/release notes). Source: `packages/react/src/components/table/table.tsx`; public usage from `@heroui/react`. Compose with toolbar/search/filter, selection action bar, pagination, and empty/loading/error presentations. Wire stable row IDs and server sort/filter/page state; don't confuse client sorting with backend results.
2. **DatePicker / DateRangePicker + Calendar/RangeCalendar** — locale-aware date selection, calendar navigation, internationalized date values and unavailable-date constraints. Calendar is compound (`Calendar.Header`, `Heading`, `NavButton`, `Grid`, `GridHeader`, `GridBody`, `Cell`, etc.); v3 differs substantially from v2's monolithic Calendar. Source path: `packages/react/src/components/calendar/calendar.tsx`; public docs list each API. Business usage: date filters, reservations, reporting windows. Define timezone boundary (date-only vs instant), locale/calendar rules, min/max and unavailable dates at the application boundary.
3. **Select / ComboBox** — accessible collection selection; ComboBox adds searchable/filterable and async/large-list patterns (Virtualizer example noted in release docs). Compose `Select` with `Label`, `ListBox`, option items and descriptions; use async search for large entities. Acceptance includes keyboard navigation, option loading, empty/no-match, focus restoration and selected values not shown as labels only.
4. **TextField + FieldError/Form** — labels, descriptions, required/invalid state and validation integration are shared form patterns. Use field composition with native form submission and server validation; preserve errors on re-render. Verify browser autofill, required labels, error association and disabled/read-only distinctions.
5. **Drawer / Modal / Toast** — drawer has placements and drag-to-dismiss; Modal/AlertDialog for decision flows; Toast stack for notifications and promise state. Compose modal for destructive confirmation, drawer for contextual inspector, toast for successful background action. Acceptance: focus trap/restore, escape/backdrop rules, reduced motion, mobile gestures and avoiding toast as sole error channel.

## API/source/dependencies

- Component guide and page examples are API truth; GitHub source uses `packages/react/src/components/<name>/`. Verified listings (via GitHub plugin) for `button.tsx`, `table.tsx`, `calendar.tsx`, plus barrel indexes. Table/calendar page contains anatomy and examples.
- `@heroui/react` imports and compound parts must be taken from current v3 docs. v2-to-v3 migration page documents changed/removed components, changed compound APIs and renamed DateInput → DateField, Autocomplete → ComboBox, Divider → Separator.
- Use current package metadata for exact peer versions. No package install or version compatibility check was run.

## Acceptance risks and status

Observed official docs/release notes, source file listings and license; no installed build, preview, assistive technology run or business data connection was performed. Risks: v2/v3 API/provider/CSS mismatch; Tailwind v4 token integration; date-only/timezone conversions; virtualized table focus/selection semantics; async result race; mobile drawer gestures; validation and focus behavior. Acceptance should test real query states, RTL/locale, keyboard/screen reader, light/dark, narrow viewport, error/loading/empty, and React/peer constraints for pinned version.

## Evidence URLs

- https://heroui.com/llms.txt
- https://heroui.com/en/docs/react/getting-started
- https://heroui.com/en/docs/react/components
- https://heroui.com/en/docs/react/releases
- https://heroui.com/en/docs/react/migration
- https://github.com/heroui-inc/heroui/tree/v3/packages/react/src/components (plugin confirmed actual directories/files)
- https://github.com/heroui-inc/heroui/blob/v3/LICENSE (Apache-2.0 read via plugin)

## Full source read: Table and form composition

Read complete `packages/react/src/components/table/table.tsx` from branch `v3`, blob `1106e90baeb1c37d12248c8888c13eea84475cb6` (14,852 bytes), and complete `textfield/textfield.tsx` (`3b2f45e6426c7713f4225c5f5deaa84cdca942d3`) plus `form/form.tsx` (`d856664d2d137a88408604911defb5265a3b9c3e`). These were fetched using the Cindy GitHub plugin and base64-decoded for inspection.

- Table source imports React Aria Table, Header, Column, Body, Row, Cell, Collection, ColumnResizer, ResizableTableContainer, TableLoadMoreItem, HeroUI style variants, and shared `dom`/class composition helpers. It defines contextual slot styling, then exports compound parts `TableRoot`, `TableScrollContainer`, `TableContent`, `TableHeader`, `TableColumn`, `TableColumnResizer`, `TableBody`, `TableRow`, `TableCell`, `TableFooter`, `TableCollection`, `TableLoadMoreItem`, `TableLoadMoreContent`, `TableResizableContainer`, and `TableSortableColumnHeader`. `Table.Content` defaults keyboard navigation to `tab`; sortable header receives `sortDirection`, optional custom indicator and `showIndicator`. `Column` and descendants pass React Aria props through. This substantiates component API; sorting/model state still belongs to consumer/React Aria collection props, and server data loading must be wired by app.
- TextField is not one monolith: `TextFieldRoot` wraps React Aria `TextField`, applies variant/fullWidth style and provides variant context. `FormRoot` is a thin wrapper over React Aria `Form`. Compose field pieces from the neighboring package components (`Label`, `Description`, `Input`, `FieldError`, `Fieldset`) according to docs; `Form` does not own business submission or validation rules. This explicitly corrects the earlier vague “TextField + FieldError/Form” description: API is compound at the field pieces, but Form is only a native React Aria adapter.
- Risks: live validation and submission handlers remain app responsibilities; use `isInvalid`/description/error association and test server rejections, reset behavior, native browser validation/autofill, keyboard and focus. Ensure the pinned v3 exports agree with example imports; source is v3, not v2.

Full source was read, not built, mounted, or tested against a business form.
