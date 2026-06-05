# Bumblebee — Domain Glossary

## Palette

The five named color roles used by the theme. Each role maps to two concrete hex values: one for dark mode, one for light mode.

| Role | Dark | Light |
|------|------|-------|
| Background | `#0B0B0B` | `#FFFFFF` |
| Text | `#DEDEDE` | `#1A1A1A` |
| Heading | `#FFFFFF` | `#000000` |
| Accent | `#FFC14A` | `#B8860B` |
| Emphasis | `#FF5757` | `#C83030` |

## Accent

The amber hue (`#FFC14A` dark / `#B8860B` light) assigned exclusively to interactive and navigational elements: links, checkboxes, selection highlight, active icons, and the interactive-accent Obsidian token. Nothing non-interactive uses the Accent color.

## Emphasis

The red hue (`#FF5757` dark / `#C83030` light) assigned exclusively to bold/strong text (`<strong>`, `**…**`). Communicates "this word matters right now" — distinct from Accent so readers never confuse "important text" with "clickable element".

## Selection

Text selection uses the Accent color at full opacity (`alpha: 1`) with black foreground text. Matches the aihero.dev reference aesthetic.

## Syntax Palette

Six token roles used only inside fenced code blocks. Two reuse core palette hues; four are code-specific:

| Token | Dark | Light | Reuses core? |
|-------|------|-------|--------------|
| keyword | `#FF5757` | `#C83030` | ✓ Emphasis |
| function | `#FFC14A` | `#B8860B` | ✓ Accent |
| string | `#7EE787` | `#2A7A3B` | — |
| property | `#79C0FF` | `#1A5FAA` | — |
| value/number | `#D2A8FF` | `#7B4DAA` | — |
| comment | `#555555` | `#999999` | — |

## Dark Mode / Light Mode

The theme ships two color modes. Dark is the default and primary design target. Light mode uses darker variants of Accent and Emphasis to clear WCAG AA contrast requirements on a white background. Font choices are controlled by Obsidian's user settings, not by the theme.
