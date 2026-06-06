# ADR 0005 — Callouts use a tinted title row over an outlined transparent body

**Status:** Accepted  
**Date:** 2026-06-06

## Context

Obsidian's default callout — and the upstream GitHub Theme scaffolding Bumblebee
inherits (see [ADR-0004](0004-bumblebee-is-an-attributed-recolor.md)) — paints the
entire callout in a single tinted wash. Stacked on a long note, those filled blocks
fight the body type for attention and clash with Bumblebee's restraint principle
(see [ADR-0002](0002-emphasis-uses-hue-not-weight.md)): only Accent and Emphasis
are meant to carry visual weight; everything else stays quiet.

A round of prototyping (`.scratch/callout-redesign/`) compared five structurally
different treatments. Ghost-outline ("B") won for being the quietest while still
type-coded. A second pass tested three flavours of B against the two states that
break thin-bordered designs — **collapsed** (body hidden) and **title-only** (no
body at all). Variant **B3 — Tinted title row** read best in those edge cases:
collapsed/title-only callouts become clean banner-style status chips, while
expanded ones gain a visible header zone without filling the whole block.

## Decision

A callout is composed of two stacked zones inside one 1px tinted outline:

- **Title row:** the type's RGB tint at `0.12` alpha as background, with the
  matching tint at full saturation as the title text colour.
- **Body:** transparent, separated from the title by a hairline divider at `0.22`
  tint alpha.

The divider is implemented as a `border-top` on `.callout-content`, so it
**vanishes for free** when Obsidian omits `.callout-content` (title-only) or
hides it (collapsed). No state-specific selectors required.

The treatment is the theme's only callout style — no opt-out toggle.

## Alternatives considered

**A — Bar + tinted header band:** clearest two-zone block, but the heavy bar +
filled header read too "docs-portal" against Bumblebee's restraint.

**B1 — Even outline / B2 — Weighted left rail:** quieter than B3, but the
collapsed/title-only states reduce to a single thin-outlined line of coloured
text. B2 carries colour better than B1, but neither produced the banner-chip
read that fits status-only callouts.

**C — Chip header / D — Filled card + disc:** strongest visual identity, but
both pull attention away from body type. C's black-on-tint chip echoed the
Selection aesthetic well but felt too declarative for inline notes.

**E — Typographic rule:** lightest of all, no box at all — judged too minimal;
the colour-coded identity didn't carry without an enclosing shape.

## Consequences

- The `body.callout-on` "minimal callout style" toggle and its `@settings` entry
  inherited from the upstream theme are **removed**. The Bumblebee callout is no
  longer configurable; this is the look. If alternates are wanted later, that's
  a new toggle, not a revival of the old one.
- The `--callout-*` variables in `:root` are tuned to neutralise Obsidian's
  filled-block defaults (border-width `1px`, blend-mode `normal`, padding `0`).
  Anything that reads those variables — third-party plugins, future Bumblebee
  toggles — will see the new values, not Obsidian's stock defaults.
- This is a small but real deviation from the upstream GitHub Theme `@settings`
  block that [ADR-0004](0004-bumblebee-is-an-attributed-recolor.md) flagged as
  derived work. The deviation is intentional and does not change the attribution
  decision; the `callout` setting ID is simply no longer used.
- Identical alpha values work in both dark and light mode (verified during
  prototyping). No per-mode overrides for the callout block.
