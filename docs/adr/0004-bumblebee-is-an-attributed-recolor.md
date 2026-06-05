# ADR 0004 — Bumblebee ships as an attributed recolor of GitHub Theme

**Status:** Accepted
**Date:** 2026-06-05

## Context

While preparing Bumblebee for the Obsidian community themes directory, a
structural diff against `krios2146/obsidian-theme-github` ("GitHub Theme",
MIT, © 2023 Vladimir Kidyaev) showed that **~82% of Bumblebee's distinct
non-trivial CSS lines are byte-identical** to that theme, including its entire
`@settings` block (same setting IDs: `callout`, `headers-underline`, `kanban`)
and component scaffolding. A meaningful share of the overlap is unavoidable
Obsidian boilerplate (the `--anim-duration-*` / `--background-modifier-*`
defaults that every theme copies from the app's own stylesheet), but the
`@settings` structure and feature set are the upstream author's creative work.

Bumblebee's genuine original contribution is the **palette** — the dark/light
colour roles and syntax tokens documented in `CONTEXT.md` and ADRs 0001–0002.
The scaffolding it sits on is derived, not independently authored.

This was surfaced because the publication goal had two hard constraints: **no
false data** in anything published, and a **clean community health review**.
Shipping a LICENSE that named only the Bumblebee author would have violated
both — MIT requires the upstream copyright notice to travel with substantial
portions of the software, and Obsidian reviewers reject uncredited near-copies.

## Decision

Publish Bumblebee as an **honest recolor with full attribution** rather than
rewriting it to originality or abandoning the submission.

- The `LICENSE` retains `Copyright (c) 2023 Vladimir Kidyaev` (required by MIT)
  and adds `Copyright (c) 2026 KulySocc` covering the recolor.
- The `README` states plainly that Bumblebee is a recolor of GitHub Theme by
  @krios2146, with a link to the source.
- The published author identity remains the GitHub handle **KulySocc** only —
  attribution of the upstream author is a third party's required credit, not
  the Bumblebee author's personal information, so it does not conflict with the
  privacy constraint.

## Consequences

- Bumblebee is presented truthfully as derivative work; no claim of sole
  authorship is made.
- The upstream `@settings` IDs and structure are kept as-is, so future edits
  should stay compatible with that scaffolding unless a deliberate rewrite is
  undertaken.
- If the theme is ever rewritten to be genuinely independent (its own
  `@settings` block and component CSS), this ADR should be superseded and the
  Vladimir Kidyaev copyright line re-evaluated.
