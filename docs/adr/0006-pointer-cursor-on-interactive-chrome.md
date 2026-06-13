# ADR 0006 — Interactive chrome shows the pointer cursor

**Status:** Accepted
**Date:** 2026-06-13

## Context

Obsidian assigns `cursor: var(--cursor)` (= `default`, the arrow) to almost all
of its chrome — file tree, ribbon, `.clickable-icon`, view actions — reserving
`pointer` (`--cursor-link`) for hyperlinks. This is a deliberate native-desktop
feel: on macOS/Windows, clickable list rows and toolbar buttons don't show a
hand. Bumblebee passed those defaults through unchanged, so the chrome showed an
arrow while the odd element showed a hand — read as inconsistent/broken.

IDEs (Cursor, VS Code) use the opposite convention: every interactive element
shows the pointer. That is the desired feel.

Measured live (ADR-0003 method, inject + re-measure): setting `--cursor: pointer`
propagates to tree, folder titles, `.clickable-icon`, view actions, ribbon,
nav-action buttons and status-bar clickables, with **no collateral** — resize
handles stay `col-resize`, text inputs `text`, editor content `auto`, because
those set their own cursor rather than reading `var(--cursor)`. A few genuinely
interactive elements don't use `var(--cursor)` and stay an arrow: workspace
**tabs**, the **new-tab button**, and the **task checkbox**.

## Decision

Make interactive chrome show the pointer, hardcoded (no Style Settings toggle):

- Flip `--cursor: pointer`.
- Add targeted `cursor: pointer` rules for the elements the variable misses:
  workspace tabs, the new-tab button, and the task checkbox.

## Consequences

- IDE-feel: everything clickable shows a hand. A deliberate deviation from
  Obsidian's native-desktop default.
- The targeted selectors are maintenance surface. If Obsidian renames them or
  starts using `var(--cursor)` natively, the rules may look inert — re-measure
  before deleting (cf. ADR 0003).
- Property keys (`.metadata-property-key`) were deliberately left out: weak
  payoff for another fragile selector.

## Alternatives considered

**Keep Obsidian's native default** — rejected; the IDE-feel is preferred and the
mixed arrow/hand state read as broken.

**Style Settings toggle** — rejected for the same reason ADR 0003 declined one:
keep the theme lean and intentionally opinionated.
