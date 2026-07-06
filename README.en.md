# Warm Paper Design System (WPDS) — Agent Guide

A **tool-agnostic** design system specification that any AI coding agent (Claude, GPT, Copilot, WorkBuddy, etc.) can read and follow to produce visually consistent UI. This directory holds the spec itself; when an agent builds, styles, or reviews UI for this product, it should load and follow these rules.

This guide can also be pasted directly as a **system prompt / design constraint** for any agent.

---

## What this is

- An independent design-system spec (not bound to any single assistant or framework).
- Two core files: this guide + [`references/wpds-spec.md`](references/wpds-spec.md) (the complete token tables, components, animations, breakpoints, a11y, and anti-patterns).
- The goal is to turn the "warm paper" visual language into deterministic instructions an agent can repeat — instead of re-describing it from scratch every time.

---

## Directory structure

```
wpds/
├── SKILL.md                 # (Optional) trigger metadata + rule summary for platforms like WorkBuddy
├── references/
│   └── wpds-spec.md         # Full design spec (authoritative reference, read on demand)
└── README.md                # This agent-agnostic guide
└── README.en.md             # English version of this guide
```

> `wpds-spec.md` is the authoritative reference. Read it when you need a precise value (exact hex, radius, shadow, font size, breakpoint, or component CSS) so it doesn't sit in context permanently.

---

## Design philosophy

Warm, focused, paper-like. A mood somewhere between a printed textbook and a modern digital tool.

- **Warm** — Warm sand backgrounds replace cold white to reduce long-session visual fatigue.
- **Quiet** — Restrained color use. Coral is the single active accent across the whole system; nothing else is used decoratively.
- **Paper** — Rounded panels, soft shadows, low-contrast dividers — evoking the physical feel of paper and cards.
- **Dark = re-lighting** — Dark mode is not a simple inversion. It is a soft-light remap onto a warm-brown base, keeping the warm tone throughout.

---

## When to apply

Load and apply this spec when any of the following are true:

- Building / styling / reviewing any frontend page, component, prototype, or design-token output for this product.
- Generating a Tailwind config, global CSS, or design tokens.
- The user mentions keywords like "warm paper, warm sand, coral accent, glass panel, bento card, aurora ambient light, paper grain, floating letters".

---

## Agent execution steps

1. **Read the spec** — On the scenarios above, first read [`references/wpds-spec.md`](references/wpds-spec.md) for exact token values.
2. **Apply the rules** — Every rule in "Enforced rules" below must be satisfied; none may be violated.
3. **Produce** — All HTML / CSS / components / config must use `warm-*` tokens, a single coral accent, warm shadows, and dark-mode re-lighting.
4. **Self-check** — Before delivering, walk the "Checklist" at the end line by line.

---

## Enforced rules (must be followed strictly)

1. **Single accent color** — Coral (`coral-500 #f97316` default / `coral-600 #ea580c` hover). Do not introduce new accent colors; teal `#06b6d4` is reserved only for aurora ambient light and gradient text.
2. **Warm palette only** — All neutrals use `warm-*`; `text-neutral-*` is a compatibility alias only — new code must use `warm-*`. **No** pure white `#ffffff` / pure black `#000000` as a background.
3. **Warm shadows** — Always `rgba(42,36,28, …)` (warm black); never the default cool-toned shadows.
4. **No scoped / CSS-in-JS** — All styling goes through global CSS + utility classes (e.g. Tailwind).
5. **Dark mode = re-lighting** — Toggle via `<html class="dark">`; map `warm-50↔warm-950`, `warm-900↔warm-50`, accent `coral-500→coral-400`; write dark styles right after the light ones using `html.dark .class`.
6. **Typography** — Geist for body + Geist Mono for monospace; mind the custom weight mapping (don't rely on framework-default `medium`/`semibold` semantics directly).
7. **Animation** — One system-wide easing curve: `cubic-bezier(0.16, 1, 0.3, 1)`.
8. **Accessibility** — skip-link, `aria-label` on icon buttons, `role="log"` + `aria-live="polite"` on log regions, `aria-hidden` on decorative elements, WCAG-AA contrast in both modes, and respect `prefers-reduced-motion`.

Full rules, token tables, and component CSS are in [`references/wpds-spec.md`](references/wpds-spec.md).

---

## Pasteable system-prompt snippet

> You are building frontend UI for this product. Strictly follow the Warm Paper Design System (WPDS):
> - Background uses the warm-sand palette `warm-*`; no pure white/black backgrounds.
> - The single accent color is coral `#f97316` (hover `#ea580c`); no other accent colors.
> - Shadows always use warm black `rgba(42,36,28,…)`.
> - Styles live in global CSS / utility classes; no component-scoped styles and no CSS-in-JS.
> - Dark mode re-lights via `<html class="dark">`, not inversion.
> - Animations use the easing `cubic-bezier(0.16, 1, 0.3, 1)`.
> - Meet WCAG-AA contrast; add `aria-hidden` to decorative elements.
> - Consult `references/wpds-spec.md` for exact values.

---

## Landing tokens in a project

Write the `warm-*` / `coral-*` palettes, warm shadows, `darkMode: 'class'`, spring easing, and aurora animations into the project's Tailwind config / design-token file, then reference them from global CSS / utility classes. Exact token values are in [`references/wpds-spec.md`](references/wpds-spec.md).

---

## Checklist (self-check before delivery)

- [ ] Background is `warm-50` / `warm-950`, no pure white/black
- [ ] Only coral as the accent color
- [ ] Shadows carry warm black `rgba(42,36,28,…)`
- [ ] Dark mode re-lights via `html.dark`, not inversion
- [ ] Animation easing is `cubic-bezier(0.16, 1, 0.3, 1)`
- [ ] Decorative elements are `aria-hidden`; interactive elements have `aria-label`

---

## Anti-patterns (explicitly forbidden)

1. Introducing a new accent color.
2. Using `text-neutral-*` in new code (compatibility alias only).
3. Component-scoped styles.
4. Introducing a new CSS-in-JS solution.
5. Using cool-toned default shadows.
6. Using pure white `#ffffff` / pure black `#000000` as a background.
7. Decorative blocks of text carrying semantics or missing `aria-hidden`.

---

## Related files

- [`references/wpds-spec.md`](references/wpds-spec.md) — Full design spec (authoritative reference)
- `SKILL.md` — (Optional) trigger metadata per platform; ignore or adapt as needed
- [`README.md`](README.md) — Chinese version of this guide
