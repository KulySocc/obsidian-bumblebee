# ADR 0002 — Bold text uses a distinct hue, not weight alone

**Status:** Accepted  
**Date:** 2026-06-04

## Context

In most Obsidian themes, `**bold text**` is rendered at the same color as body text with only `font-weight: 700` for differentiation. On a near-black background with `#DEDEDE` body text, the weight delta is subtle and easy to miss in long prose.

The design goal is that bolded words should be immediately noticeable — "I see it right away" — without looking clickable (which would conflict with the Accent amber).

## Decision

Bold/strong text (`<strong>`, `**…**`) is assigned a dedicated Emphasis color — vivid red (`#FF5757` dark / `#C83030` light) — via Obsidian's `--bold-color` token.

This red is categorically different from the amber Accent, so readers never misread a bold word as a link.

## Alternatives considered

**Pure white `#FFFFFF` for bold** — brighter than body text but same hue family. Rejected: on a dark background the brightness delta between `#DEDEDE` and `#FFFFFF` is imperceptible in running prose.

**Amber `#FFC14A` for bold** — reuses the Accent color. Rejected: visually indistinguishable from a link; breaks the "amber = interactive" semantic.

**Weight only, no color change** — the default Obsidian behavior. Rejected: insufficient visual pop on long-form notes.

## Consequences

- `--bold-color` is set in both `.theme-dark` and `.theme-light`.
- The Emphasis red is reserved strictly for bold text. It must not be reused for decorative purposes.
- In the syntax palette, keywords also use the Emphasis red — consistent: keywords are "structurally important" just as bold words are semantically important.
