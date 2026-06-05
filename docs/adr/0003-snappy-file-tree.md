# ADR 0003 — File navigator snaps instead of animating

**Status:** Accepted
**Date:** 2026-06-05

## Context

The file tree felt sluggish, and folders flickered on collapse. We measured
the live behaviour against a real vault (6038 files, 200 folders) via the
Obsidian CLI rather than guessing:

- **The theme adds no custom file-tree CSS.** The navigator is styled entirely
  through Obsidian's `--nav-item-*` design tokens, so the theme was not the
  direct cause.
- **Obsidian 1.13 animates expand/collapse with a ~100ms CSS transition** that
  it applies inline to `.tree-item-children` (`transition: …100ms cubic-bezier`).
  It is a CSS transition, not a Web Animations / JS animation
  (`element.getAnimations()` returned `[]`), so it is overridable from a theme
  stylesheet — a rule with `!important` beats Obsidian's non-important inline
  transition.
- **The actual compute cost of expanding a normal folder is ~0.3ms.** The list
  is virtualised (`vChildren`): only visible rows exist in the DOM. The felt
  latency was almost entirely the animation duration, not real work.
- **The flicker on collapse is the virtualisation reconciliation.** After a
  folder's height snaps to 0, Obsidian shuffles row nodes in and out of the
  collapsed container (~11ms later) and rewrites the virtual scroll spacer's
  `min-height` several times, nudging the rows below. This is most visible on
  medium folders (~50 direct children) because they cross the scrollable
  threshold; tiny folders don't, and the one pathological folder
  (`99 Meta/_Assets`, 4858 direct children) is a separate freeze that no CSS
  can fix.

Candidate fixes were tested empirically by injecting CSS into the live app:

- `scrollbar-gutter: stable` — **rejected.** macOS overlay scrollbars steal no
  width, so the flicker was unaffected.
- `transition: none` alone — fixed the sluggishness but not the collapse
  flicker.
- `transition: none` **plus** hiding collapsed children — removed both.

## Decision

Make the file navigator snap, as a hardcoded theme default (no Style Settings
toggle), via two rules:

```css
.nav-files-container .tree-item-children {
    transition: none !important;
}
.nav-files-container .tree-item.is-collapsed > .tree-item-children {
    display: none !important;
}
```

The first removes the ~100ms slide so folders open/close instantly. The second
takes the collapsed children container out of layout, so Obsidian's
post-collapse row reconciliation never paints — killing the flicker. Expansion
correctness was verified: children render, height and scroll height compute
normally.

## Consequences

- Folders open and close instantly. Performance-first, matching the theme's UX
  priority.
- Users who prefer Obsidian's default slide animation cannot restore it without
  editing the theme. Accepted: the theme is intentionally opinionated, and a
  toggle was declined to keep it lean.
- This works around Obsidian-core behaviour observed in 1.13. A future Obsidian
  version could change the expand/collapse mechanism; if these rules ever look
  inert or wrong, re-measure before deleting them — the reasoning above is the
  expensive part to reconstruct.

## Alternatives considered

**Keep the animation but shorten it (~60ms)** — still leaves the collapse
flicker, since that comes from virtualisation, not the transition duration.

**`scrollbar-gutter: stable`** — rejected after live testing; irrelevant under
macOS overlay scrollbars.

**Restructure the vault / exclude `_Assets` from the explorer** — addresses the
separate 4858-child freeze, not the everyday sluggishness. Out of scope for the
theme.
