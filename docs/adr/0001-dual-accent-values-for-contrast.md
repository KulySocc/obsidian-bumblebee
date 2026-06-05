# ADR 0001 — Two accent values: one per mode

**Status:** Accepted  
**Date:** 2026-06-04

## Context

The Bumblebee palette defines amber (`#FFC14A`) as the single Accent hue. On a dark `#0B0B0B` background this hue passes WCAG AA comfortably. On a white `#FFFFFF` background it produces a contrast ratio of ~1.6:1 — far below the 4.5:1 requirement for body text and the 3:1 requirement for large/bold text.

The same issue applies to the Emphasis red: `#FF5757` achieves ~3.1:1 on white, also below the AA threshold for normal prose.

The Obsidian community theme guidelines and health-score tooling check accessibility, so shipping a light mode that fails contrast would penalise the release.

## Decision

Maintain the same hue identity in both modes but darken the values for light mode:

| Role | Dark | Light |
|------|------|-------|
| Accent | `#FFC14A` | `#B8860B` |
| Emphasis | `#FF5757` | `#C83030` |

`#B8860B` achieves ~4.7:1 on white (passes AA). `#C83030` achieves ~5.1:1 on white (passes AA).

## Alternatives considered

**Single value for both modes** — keep `#FFC14A` everywhere and accept the contrast failure on light backgrounds. Rejected: blocks a clean community release and is hard to retrofit once the theme ships.

**Different background** — use a warm off-white (e.g. `#FFF8EE`) to close the gap. Rejected: still requires darkening the accent, and introduces a tinted background that was not part of the original design intent.

## Consequences

- `theme.css` must define both `.theme-dark` and `.theme-light` blocks with separate accent/emphasis values.
- The visual "feel" of the amber brand is preserved in both modes — `#B8860B` reads as dark amber/goldenrod, not a different hue.
- A future reader seeing two different hex values for "the amber color" will find the explanation here.
