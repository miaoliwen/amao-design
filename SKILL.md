---
name: amao-design
description: |
  Apply the "Warm Paper Design System" (WPDS) — the canonical UI/visual
  specification for the amao product surfaces. This skill should be used when
  building, styling, or reviewing any frontend UI, web page, component,
  prototype, or design-token output for amao, or whenever the user references
  the warm-paper aesthetic, the warm-sand palette, the coral/orange accent,
  glass panels, bento tiles, aurora ambient lighting, or "paper-like" styling.
  It enforces tokens, typography, components, animations, dark-mode re-lighting,
  and the system's anti-patterns (e.g. no extra accent colors, no pure
  white/black backgrounds, no scoped/CSS-in-JS styling).
agent_created: true
---

# Amao Design — Warm Paper Design System (WPDS)

## Overview

This skill is the authoritative, standalone specification for the Warm Paper
Design System used across amao product surfaces. It converts a design-token
pipeline or component library into consistent, "paper-like" UI: warm-sand
bases, a single coral/orange accent, rounded panels, soft warm shadows, and a
dark mode that re-lights rather than inverts. Load it whenever producing or
reviewing amao UI so the output matches the established visual language.

## When to Use

- Building or styling any amao frontend page, component, or prototype.
- Generating Tailwind config, CSS, or design tokens for amao.
- Reviewing UI against the system's rules (colors, typography, spacing,
  borders, shadows, gradients, components, animation, dark mode, a11y).
- The user mentions: warm paper, warm-sand, coral accent, glass panel, bento
  tile, aurora, floating letters, paper grain, or "paper-like" UI.

## How to Use

When a task needs visual decisions, read the full specification from
`references/wpds-spec.md` (this skill's bundled reference). Key enforcement
rules to keep in mind without re-reading:

1. **Single accent color** — coral/orange (`coral-500 #f97316` primary,
   `coral-600 #ea580c` hover). Never introduce a new accent color; teal
   (`#06b6d4`) is reserved for aurora/gradient-text only.
2. **Warm palette only** — all neutrals use `warm-*`. `text-neutral-*` is a
   compatibility alias; new code uses `warm-*`. Never use pure `#ffffff` or
   `#000000` for backgrounds.
3. **Warm shadows** — always `rgba(42,36,28, …)` warm-black, never Tailwind's
   default cold `shadow-*`.
4. **No scoped / CSS-in-JS** — all styling via global CSS + Tailwind utilities.
5. **Dark mode = re-lighting** — toggle via the `html.dark` class; map
   `warm-50 ↔ warm-950`, `warm-900 ↔ warm-50`, accent `coral-500 → coral-400`;
   place dark styles right after light using `html.dark .class`.
6. **Typography** — Geist primary, Geist Mono for labels/code; use the
   custom weight mapping (do not trust Tailwind's default `font-medium`/
   `font-semibold` semantics).
7. **Animation** — single easing `cubic-bezier(0.16, 1, 0.3, 1)`.
8. **A11y** — skip-link, `aria-label` on icon buttons, `role="log"` +
   `aria-live="polite"` for logs, decorative elements `aria-hidden`,
   WCAG-AA contrast in both modes; honor `prefers-reduced-motion`.

### Reference

- `references/wpds-spec.md` — complete token tables, component patterns,
  animation presets, responsive breakpoints, markdown rendering, iconography,
  CSP, file organization, authoring conventions, and the full anti-patterns
  list. Load this for any concrete value (exact hex, radius, shadow, font
  size, breakpoint, or component CSS).

### Optional Assets

If an amao project needs a starter, generate a Tailwind config + global CSS
from the tokens in `references/wpds-spec.md` rather than copying defaults.
