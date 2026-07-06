### **Warm Paper Design System (WPDS) — Full Specification **

Below is the complete, generalized English version of the design system, incorporating the structural refinements and abstractions applied earlier. It is written as a standalone specification (`SKILL.md` equivalent) suitable for any product surface — not just a specific assistant — and can be dropped directly into a design-token pipeline or component library documentation.

## Design Philosophy

Warm, focused, paper-like. The overall atmosphere sits between "paper textbook" and "modern digital tool."

- **Warm**: A warm-sand base replaces cold white, reducing visual fatigue during long sessions.
- **Calm**: Color is used sparingly. The primary accent (coral/orange) is the only active accent color; nothing else is used decoratively.
- **Paper-like**: Rounded panels, subtle shadows, and low-contrast dividers simulate the physical texture of paper and cards.
- **Dark mode as re-lighting, not inversion**: Dark mode is not a simple color flip — it is a soft glow on a warm-brown base that preserves the warm tone throughout.

---

## Design Tokens

### Color System

#### Warm Palette (Base)

```
warm-25:  #fefcf9   Lightest surface (cards, panels)
warm-50:  #fdf8f0   Body background
warm-75:  #faf1e4   Hover / secondary background
warm-100: #f5ead8   Light borders / dividers
warm-200: #ede0cc   Light text / secondary borders
warm-300: #d9cbaf   Borders / placeholder text
warm-400: #b8a58a   Muted secondary text
warm-500: #9a876c   Mid-tone text
warm-600: #7a6a54   Body copy / assistant text
warm-700: #5c4f3e   Dark body text
warm-800: #3f362a   Headings
warm-900: #2a241c   Primary text (light mode)
warm-950: #1a1611   Body background (dark mode)
```

#### Accent Color (Coral — Orange)

The single active accent color, used for buttons, links, focus states, selection states, and icon hover states.

```
coral-50:  #fff7ed
coral-100: #ffedd5
coral-200: #fed7aa
coral-300: #fdba74
coral-400: #fb923c
coral-500: #f97316   Primary
coral-600: #ea580c   Hover
coral-700: #c2410c   Pressed
```

#### Secondary Accent (Teal — Blue-Cyan)

Reserved for aurora ambient lighting and gradient-text accents only; rarely used standalone.

```
teal-500: #06b6d4
```

#### Neutral (Warm-Compatible Alias)

Functionally identical to the warm palette; exists purely for Tailwind `text-neutral-*` / `bg-neutral-*` compatibility. **New code should always prefer `warm-*`.**

#### Dark Mode Color Mapping

- Background inversion: `warm-50` ↔ `warm-950`
- Foreground inversion: `warm-900` ↔ `warm-50`
- Border contrast preserved between `warm-700` ↔ `warm-300`
- Accent brightened by one step: `coral-500` → `coral-400`
- Shadows become darker and more diffused in dark mode

If a rule requires no meaningful change between modes (e.g., a solid-fill button/icon remains legible in both), omit the redundant `dark:` variant rather than duplicating identical values.

---

### Typography

#### Primary Font Stack

```css
font-family: 'Geist', 'figmaSans Fallback', 'SF Pro Display', system-ui, sans-serif;
```

- **Geist** (Vercel's variable font): default weight `400`, headings `540`, body text `330`–`480`.
- The fallback chain prioritizes `system-ui` for fast native rendering.
- CJK glyphs are covered by `Noto Sans SC` (loaded via Google Fonts) or the equivalent regional font for the target locale.

#### Monospace Font Stack

```css
font-family: 'Geist Mono', 'figmaMono Fallback', 'SF Mono', ui-monospace, monospace;
```

Used for: section labels, code blocks, timestamps, numeric statistics, and tab labels.

#### Font Weights

```
thin:        320
extralight:  330
light:       340
book:        450
medium:      480
semibold:    540
bold:        700
```

Tailwind's default `font-medium` (500) maps to `book` (450), and `font-semibold` (600) maps to `medium` (480). Do not rely on Tailwind's default weight semantics — always reference this mapping table.

#### Font Size & Line Height Scale

| Usage | Size | Line Height | Letter Spacing | Weight |
|---|---|---|---|---|
| Hero title | `86px` | `1.0` | `-1.72px` | `400` |
| Section title | `64px` | `1.1` | `-0.96px` | `400` |
| Page title (3xl) | `2rem` | — | `-0.96px` | `book (450)` |
| Body (base) | `1rem` | `1.6–1.75` | `-0.14px` | `400` |
| Section label | `0.75rem` | — | `0.6px` uppercase | `400` mono |
| Utility text | `0.875rem` | — | `-0.14px` | `medium (480)` |
| Hint text | `11px–12px` | — | — | `400` |

#### Global Letter Spacing

```css
letter-spacing: -0.14px;  /* body-level default */
```

All body-level text carries a slight negative tracking — the only globally applied `letter-spacing` value in the system. Large headings use a proportionally larger negative value.

---

### Spacing

Based entirely on Tailwind's default spacing scale (`px` / `py` / `gap` / `space-y`); no custom spacing values are introduced.

Common spacing patterns:
- `.px-4 sm:px-6` — horizontal padding for panels/containers
- `.py-4 sm:py-5` — vertical padding for message bubbles/list items
- `.gap-1 sm:gap-2` — spacing between inline elements
- `.mb-4` / `.space-y-6` — spacing between block-level elements

---

### Borders & Radius

| Token | Value | Usage |
|---|---|---|
| `rounded-pill` | `50px` | Buttons, tags, pill tabs, nav links |
| `rounded-2xl` | `1rem` | Panels, cards, surfaces |
| `rounded-xl` | `0.75rem` | Buttons, icon containers |
| `rounded-2xl` | `16px` | Code blocks, chat composer |
| `rounded-full` | `9999px` | Avatars, small icon containers |

**Border color**: Light: `warm-300` / Dark: `warm-700`

---

### Shadows

```
warm-sm:   0 1px 3px rgba(42,36,28,0.06)
warm:      0 2px 8px rgba(42,36,28,0.06), 0 1px 2px rgba(42,36,28,0.04)
warm-lg:   0 8px 24px rgba(42,36,28,0.07), 0 2px 4px rgba(42,36,28,0.04)
warm-card: 0 20px 40px -15px rgba(42,36,28,0.06)
warm-xl:   0 25px 50px -12px rgba(42,36,28,0.1)
```

Shadows always use warm-black `rgba(42,36,28, ...)` rather than pure black `rgba(0,0,0,...)`, preserving the warm material feel.

---

### Gradients & Ambient Lighting

#### Primary Button Gradient

```css
background: linear-gradient(135deg, #f97316, #ea580c);
hover:      linear-gradient(135deg, #fb923c, #f97316);
```

#### Gradient Text (disabled states or special emphasis)

```css
background: linear-gradient(135deg, #f97316, #fb923c, #06b6d4, #f97316);
background-size: 200% 200%;
animation: shimmer 3s ease-in-out infinite;
```

#### Aurora Ambient Light (background decoration)

Three floating, gaussian-blurred color blobs, used for large empty areas such as landing pages:

```css
.aurora-a { background: rgba(249,115,22,0.08); }   /* orange glow */
.aurora-b { background: rgba(6,182,212,0.06); }     /* teal glow */
.aurora-c { background: rgba(251,146,60,0.05); }    /* warm-orange glow */
/* animation: 18–26s slow drift */
```

In dark mode, opacity increases (0.12 / 0.10 / 0.08).

#### Warm Ambient Glow (static radial glow)

```css
.warm-ambient-a: radial-gradient(circle, rgba(249,115,22,0.10) 0%, transparent 70%)
.warm-ambient-b: radial-gradient(circle, rgba(6,182,212,0.08) 0%, transparent 70%)
```

#### Grain (paper texture overlay)

A fixed, full-screen SVG noise filter, opacity 0.025 (light) / 0.04 (dark), simulating paper texture.

```css
.app-grain { opacity: 0.025; /* SVG feTurbulence fractalNoise */ }
html.dark .app-grain { opacity: 0.04; }
```

#### Floating Letters (landing page ambient decoration)

Letters that gently float near the bottom of the viewport, rendered in the mono font at extremely low opacity `rgba(42,36,28,0.06)`, with randomized drift.

---

### Component Patterns

#### Glass Panel

Frosted-glass navigation bar / dropdown menu:

```css
background: rgba(253,248,240,0.72);
backdrop-filter: blur(24px);
border: 1px solid rgba(218,203,175,0.35);
border-radius: 50px;
box-shadow: inset 0 1px 0 rgba(255,252,249,0.5), 0 8px 32px -8px rgba(42,36,28,0.08);
```

Dark mode: background `rgba(26,22,17,0.72)`, border `rgba(92,79,62,0.35)`.

#### Surface Panel / Card / Bento Tile

Three near-identical lightweight card styles, differing in hover behavior and gap:

- **surface-panel**: base white surface, no interaction
- **list-panel**: child elements separated by dividers (`border-top`)
- **bento-tile**: hover lifts the element by 2px

```css
background: #fefcf9;
border: 1px solid rgba(218,203,175,0.2);
border-radius: 20px;
box-shadow: 0 20px 40px -15px rgba(42,36,28,0.05);
```

#### Buttons

| Type | Style | Hover | Pressed |
|---|---|---|---|
| `.btn-primary` | Orange gradient fill, white text | brighter gradient step, lifts 1px | scale(0.98) |
| `.btn-secondary` | Transparent + border | warm fill + darker border, lifts 1px | scale(0.98) |
| `.btn-ghost` | Fully transparent, gray text | 6% black overlay | — |

**Disabled state**: opacity 0.35, cursor not-allowed, no shadow/no movement.

#### Pill Tab

```css
.pill-tab-active:   coral-500 fill + white text
.pill-tab-inactive: transparent + warm-300 border + warm-600 text
hover inactive:      6% coral overlay + warm-900 text
```

#### Upload Zone

```css
border: 2px dashed #d9cbaf;
border-radius: 20px;
/* on drag-enter */
border-color: #f97316;
background: rgba(249,115,22,0.05);
```

#### Chat Composer / Input Field

```css
background: #fefcf9;
border: 1px solid #d9cbaf;
border-radius: 16px;
/* focus-within */
border-color: #f97316;
/* send/action button */
width: 40px; height: 40px; border-radius: 12px;
background: #f97316; color: white;
```

#### Toast

A fixed, top-centered notification bar; background `neutral-900` (success) / `red-600` (error), using scale-based transition animation.

---

### Animation & Transitions

#### Easing Function

The entire system uses a single easing curve consistently: `cubic-bezier(0.16, 1, 0.3, 1)` — an Apple-style "spring-like" ease.

#### Preset Animations

| Class | Effect | Duration |
|---|---|---|
| `.stagger-item` | List items fade in and slide up sequentially | 0.45s, 60ms stagger |
| `.hero-line-1/2` | Hero section lines scale and pop in sequentially | 0.8s, 150ms stagger |
| `fade-enter/leave` | Fade in/out | 0.3s |
| `slide-up-enter/leave` | Slide up 12px + fade in | 0.4s |
| `scale-enter/leave` | Scale from 0.96 + fade in | 0.3s |
| `.animate-pulse` | Loading indicator (three bouncing dots) | Tailwind default |

#### Transition Types

```
Hover interaction:      transition-all duration-300 ease (default)
Button/card hover:      add translateY(-1px)
Click feedback:         scale(0.98)
Nav bar/background:     transition-colors duration-300
```

---

### Dark Mode

Toggled via `<html class="dark">`. The system's approach to dark mode is not "inversion" but "re-lighting":

- Background: `warm-50` → `warm-950`
- Panel surface: `warm-25` → `warm-900`
- Primary text: `warm-900` → `warm-50`
- Border: `warm-300` → `warm-700`
- Accent: `coral-500` → `coral-400`
- Shadows become more diffused and more transparent in dark mode
- Aurora ambient lighting becomes brighter in dark mode

**CSS organization**: Dark-mode styles are placed immediately after their light-mode counterparts using the `html.dark .class-name` selector, rather than a separate `@media` block or duplicated root variables.

---

### Responsive Breakpoints

```
xs:  480px   (extra small screens)
sm:  640px   (mobile landscape)
md:  768px   (tablet)
lg:  1024px  (narrow desktop) — nav collapse / layout switch point
xl:  1280px
2xl: 1440px
3xl: 1920px  (large-screen optimization)
```

Key breakpoints:
- **lg (1024px)**: mobile nav ↔ desktop nav; sidebar visibility; chat grid switches 1 → 4 columns
- **sm (640px)**: mobile component radius/font-size contraction

The body background color only ever toggles between `warm-50`/`warm-950`; no intermediate background tiers are used.

---

### Markdown Rendering

#### Reading Mode (`.markdown-reading`)

```
font-size: 15px
line-height: 1.75
letter-spacing: -0.14px
```

- `h1`: 1.5rem / weight 540
- `h2`: 1.25rem / weight 540
- `h3`: 1.125rem / weight 480
- `p`: color warm-600 (dark: warm-400)
- `pre`: background warm-900 (#2a241c), radius 16px (dark: warm-950 #1a1611 + border warm-800)
- `code`: inline warm-orange background `rgba(249,115,22,0.08)`, radius 0.5rem
- `blockquote`: 3px left border warm-300, italic
- `table`: border warm-100; th background warm-75

#### General Content (`.markdown-content`)

Nearly identical to reading mode, but `h1`/`h2` use larger margins; `p` color remains warm-600; inline `code` style is the same.

---

### Iconography

Heroicons `outline` variant is used sitewide (`stroke-width="2"` / `1.75` / `1.5`).

Base rules:
- Use `stroke="currentColor"` so icons follow text color.
- Standard sizes: `w-4 h-4` (16px) / `w-5 h-5` (20px) / `w-3.5 h-3.5` (14px).
- Icon containers maintain a 1:1 aspect ratio, using `flex items-center justify-center`.

---

### HTML / Accessibility Conventions

- Nav bars include a `skip-link` (a hidden link to the main content that appears on focus).
- Icon buttons must have an `aria-label`.
- Buttons use a `touch-target` class to ensure a mobile tap area ≥ 2.75rem.
- Message/log regions use `role="log"` + `aria-live="polite"`.
- Purely decorative elements are marked `aria-hidden="true"`.
- Color contrast: body text must satisfy WCAG AA (verified for both light and dark modes).
- On browsers that don't support `prefers-reduced-motion`, `.floating-letter` elements are hidden by default.

---

### Content Security Policy

- UI font weights are not loaded from a CDN (KaTeX CSS is bundled locally).
- All Markdown content rendered via `v-html` is sanitized through DOMPurify (allowed tags/attributes are defined in a dedicated `useMarkdownRender` composable/utility).
- The DOMPurify configuration permits only a restricted tag subset (h1–h6, p, pre, code, table, a, img, span, div, etc.).
- Link protocol allowlist: only http/https are permitted; `javascript:` / `vbscript:` / `data:` / `file:` are rejected.

---

### File Organization

```
src/
  style.css          — global styles, CSS components, animations
  tailwind.config.js — design tokens (colors, fonts, shadows, animations, etc.)
  components/        — shared UI components
    chat/            — chat-related component group
    settings/        — settings panel components
  composables/       — reusable framework-agnostic logic (or hooks, depending on stack)
  stores/            — global state management
  views/             — routed pages
```

---

### Component Authoring Conventions

All components follow these conventions:

- **Props**: declared with typed prop definitions; props without defaults use the framework's "with defaults" pattern.
- **Events/Emits**: declared with typed emit definitions, using kebab-case for external event names.
- **Template/markup order**: markup precedes script logic; components do not use scoped styles — the entire system relies on global CSS class conventions + Tailwind utilities.
- **Component grouping**: non-generic, feature-specific components live under their relevant business directory (e.g., `components/chat/`).

---

### Anti-Patterns (Do Not)

The following should be challenged or prohibited outright:

1. **Do not introduce a new accent color** — coral/orange is the only active accent color in the system.
2. **`text-neutral-*` is a compatibility alias only** — new code should always use `warm-*`.
3. **Do not use component-level scoped styles** — all styling lives in the global stylesheet, via Tailwind utilities or global CSS layers.
4. **Do not introduce new CSS-in-JS solutions** — Tailwind utilities + global CSS layer overrides cover every styling need.
5. **Shadows must use the `warm-*` series**, never Tailwind's default `shadow-*` utilities (which lean cold/blue-gray).
6. **Never use pure white `#ffffff` or pure black `#000000`** as a background color.
7. **Large blocks of decorative text (e.g., floating letters) must carry no semantic meaning** and must be marked `aria-hidden="true"`.

---

### Development Commands (Reference Template)

```bash
npm run dev        # start local dev server
npm run build      # production build (type-check + bundler build)
npm run test       # run unit tests
npm run lint       # run linter
```

