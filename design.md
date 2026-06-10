# Pulse — Design System

> Design documentation for **Pulse · Care OS**, a healthcare provider dashboard.
> This document describes the system **as it is implemented** in `pulse-healthcare-dashboard.html`
> (Tailwind CSS via CDN + Lucide icons). All tokens, type, and component patterns below are
> extracted directly from that file so the docs and the UI stay in sync.

---

## 1. Foundations

### 1.1 Tech stack

| Concern        | Choice                                                            |
| -------------- | ----------------------------------------------------------------- |
| Styling        | Tailwind CSS (CDN) with an inline `tailwind.config` theme extend   |
| Icons          | [Lucide](https://lucide.dev) via UMD bundle, `lucide.createIcons()`|
| Fonts          | Google Fonts — Inter + Plus Jakarta Sans                          |
| Color model    | Semantic token names mapped to hex values                          |
| Theme          | Light only (no dark mode currently defined)                        |

Tokens are defined twice in the source: once in `tailwind.config.theme.extend` (so utility
classes like `bg-primary` work) and again as **inline `style="…"` fallbacks** on key elements
(so the design survives even if the CDN/JIT hasn't resolved a class). When editing, keep both in
sync.

---

## 2. Color

### 2.1 Semantic palette

| Token                     | Hex        | Role / usage                                            |
| ------------------------- | ---------- | ------------------------------------------------------- |
| `background`              | `#f6f8f9`  | App canvas / page background                            |
| `foreground`              | `#1a2332`  | Primary text                                            |
| `card`                    | `#ffffff`  | Card & surface background                               |
| `primary`                 | `#0d9488`  | Brand teal — CTAs, active nav, links, logo              |
| `primary-foreground`      | `#f0fdf9`  | Text/icon on primary surfaces                           |
| `secondary`               | `#eef6f8`  | Muted brand surface — message avatars                   |
| `secondary-foreground`    | `#2d3a4d`  | Text on secondary surfaces                              |
| `muted`                   | `#f1f5f7`  | Subtle fills — hover states, chips, table header        |
| `muted-foreground`        | `#64748b`  | Secondary/supporting text, inactive nav, metadata       |
| `accent`                  | `#ccfbf1`  | Light teal — avatar tiles, schedule icons               |
| `accent-foreground`       | `#0f3d3e`  | Text/icon on accent surfaces                            |
| `destructive`             | `#dc2626`  | Errors, high risk, alert dots                           |
| `destructive-foreground`  | `#ffffff`  | Text on destructive surfaces                            |
| `success`                 | `#22c55e`  | Positive deltas, low risk, "checked in"                 |
| `success-foreground`      | `#ffffff`  | Text on solid success surfaces                          |
| `warning`                 | `#f59e0b`  | Pending/attention, moderate risk, "waiting"             |
| `warning-foreground`      | `#451a03`  | Text on warning tints (dark brown for contrast)         |
| `info`                    | `#3b82f6`  | Informational accents, appointment metric               |
| `info-foreground`         | `#ffffff`  | Text on solid info surfaces                             |
| `border`                  | `#e2e8f0`  | All hairline borders & dividers (global `*` default)    |
| `ring`                    | `#0d9488`  | Focus ring (matches primary)                            |

### 2.2 Tint convention (status colors)

Status chips and icon tiles use a **translucent fill of the base color** rather than the solid
token, paired with a saturated text/icon color. The opacity is tuned per hue so each reads at a
similar lightness:

| Hue        | Fill                          | Foreground            |
| ---------- | ----------------------------- | --------------------- |
| Primary    | `rgba(13,148,136,0.10)`       | `#0d9488`             |
| Info       | `rgba(59,130,246,0.15)`       | `#3b82f6`             |
| Success    | `rgba(34,197,94,0.15)`        | `#22c55e`             |
| Warning    | `rgba(245,158,11,0.20)`       | `#451a03`             |
| Destructive| `rgba(220,38,38,0.15)`        | `#dc2626`             |

> **Contrast note:** success/info tints use the saturated hue as text on a light tint — acceptable
> for non-essential status labels at chip sizes, but avoid for body copy. Warning deliberately uses
> the dark-brown `warning-foreground` for stronger contrast. If WCAG AA on chips is required, darken
> the success/info foregrounds (e.g. `#15803d` / `#1d4ed8`).

### 2.3 Gradients

| Name                | Definition                                  | Usage                          |
| ------------------- | ------------------------------------------- | ------------------------------ |
| Brand gradient      | `linear-gradient(135deg, #0d9488, #0f766e)` | "Clinical performance" panel   |

Metric tiles on the gradient use `bg-white/10` + `backdrop-blur` for a frosted-glass effect.

---

## 3. Typography

### 3.1 Font families

| Role     | Family               | Tailwind          | Fallback stack                          |
| -------- | -------------------- | ----------------- | --------------------------------------- |
| Body     | **Inter**            | `font-sans`       | `ui-sans-serif, system-ui, sans-serif`  |
| Display  | **Plus Jakarta Sans**| `font-display`    | `ui-sans-serif, system-ui, sans-serif`  |

- **Body (Inter):** weights 400, 500, 600, 700. Enabled OpenType features: `"cv11", "ss01"`.
- **Display (Plus Jakarta Sans):** weights 500, 600, 700, 800. Used for all headings, big numbers,
  avatar initials, and time labels. Headings get `letter-spacing: -0.02em`.

`h1`–`h5` automatically use the display family via the base stylesheet.

### 3.2 Type scale (as used)

| Element                  | Classes                                   | Size / weight              |
| ------------------------ | ----------------------------------------- | -------------------------- |
| Page title (`h1`)        | `text-xl font-semibold`                   | 20px / 600, display        |
| Card title (`h2`)        | `font-display text-lg font-semibold`      | 18px / 600                 |
| Stat number             | `font-display text-3xl font-semibold`     | 30px / 600                 |
| Metric number (gradient)| `font-display text-2xl font-semibold`     | 24px / 600                 |
| Body / table cell        | `text-sm`                                 | 14px / 400–500             |
| Secondary text           | `text-xs text-muted-foreground`           | 12px                       |
| Micro labels / timestamps| `text-[11px]` / `text-[10px]`             | 11px / 10px                |
| Eyebrow labels           | `text-xs font-medium uppercase tracking-wide` | 12px, uppercase        |

---

## 4. Spacing, radius, elevation

### 4.1 Spacing scale

Standard Tailwind 4px scale. Conventions in this UI:

| Context                     | Value                       |
| --------------------------- | --------------------------- |
| Page padding                | `p-6` (24px)                |
| Card padding                | `p-5` (20px)                |
| Section vertical rhythm     | `space-y-6` (24px)          |
| Grid gaps                   | `gap-4` (16px) / `gap-6` (24px) |
| List row padding            | `py-3` (12px)               |
| Inline icon/text gaps       | `gap-2` / `gap-3`           |

### 4.2 Border radius

| Token        | Value       | Usage                                      |
| ------------ | ----------- | ------------------------------------------ |
| `rounded-2xl`| `1rem`      | Cards / major surfaces                     |
| `rounded-xl` | `0.875rem`  | Icon tiles, logo mark, inner panels        |
| `rounded-lg` | `0.75rem`   | Buttons, inputs, nav items                 |
| `rounded-md` | default     | Small chips ("Today", "Open")              |
| `rounded-full`| —          | Avatars, status pills, badges, dots        |

### 4.3 Elevation

| Level     | Class             | Usage                          |
| --------- | ----------------- | ------------------------------ |
| Resting   | `shadow-sm`       | All cards                      |
| Hover     | `hover:shadow-md` | Stat cards (with `transition-shadow`) |

### 4.4 Glassmorphism

Sticky chrome uses translucent backgrounds + blur:

- **Sidebar:** `background: rgba(255,255,255,0.6)` + `backdrop-filter: blur(8px)`
- **Topbar:** `background: rgba(246,248,249,0.8)` + `backdrop-filter: blur(8px)`

---

## 5. Iconography

- **Library:** Lucide, rendered from `data-lucide="<name>"` attributes, initialized once via
  `lucide.createIcons()` at the end of `<body>`.
- **Sizes:** `h-4 w-4` (16px) default in nav/buttons/lists; `h-5 w-5` (20px) for the logo mark.
- **Icons in use:** `heart` (logo), `layout-dashboard`, `users`, `calendar-days`, `file-text`,
  `message-square`, `pill`, `trending-up`, `life-buoy`, `settings`, `search`, `bell`, `plus`,
  `chevron-right`, `stethoscope`, `video`, `activity`, `clipboard-list`.
- **Color:** icons inherit text color, or are tinted to match their status hue inside chip tiles.

---

## 6. Layout

```
┌──────────┬───────────────────────────────────────────────┐
│          │  Topbar (sticky, glass)                        │
│ Sidebar  ├───────────────────────────────────────────────┤
│ (w-64,   │  Stats row        — grid 1 / sm:2 / xl:4       │
│  sticky, │  Schedule (2col) + Messages (1col) — xl:3      │
│  glass,  │  Patients table (2col) + Tasks/Perf (1col)     │
│  lg:flex)│                                                │
└──────────┴───────────────────────────────────────────────┘
```

- **Shell:** `flex` with a fixed-width sidebar (`w-64`, hidden below `lg`) and a fluid `main`.
- **Sidebar & topbar** are `sticky top-0` with full viewport height / blur.
- **Content grids** are responsive: stats `grid-cols-1 sm:grid-cols-2 xl:grid-cols-4`; content
  rows `grid-cols-1 xl:grid-cols-3` with a `xl:col-span-2` primary column.
- **Breakpoints:** Tailwind defaults — `sm` 640, `md` 768, `lg` 1024, `xl` 1280.

---

## 7. Components

### 7.1 Card (surface)
```html
<div class="rounded-2xl border border-border bg-card p-5 shadow-sm">…</div>
```
Base container for every panel. Add `hover:shadow-md transition-shadow` for interactive stat cards.

### 7.2 Stat card
- Header row: eyebrow label (`text-xs uppercase tracking-wide muted`) + tinted icon tile
  (`h-8 w-8 rounded-lg`, `rgba(hue,…)` fill).
- Value row: `font-display text-3xl font-semibold` number + small colored delta (e.g. `+12%`,
  `−4m`, `2 urgent`).

### 7.3 Sidebar nav item
```html
<!-- active -->
<button class="… rounded-lg px-3 py-2 text-sm bg-primary/10 text-primary">…</button>
<!-- inactive -->
<button class="… rounded-lg px-3 py-2 text-sm text-muted-foreground hover:bg-muted hover:text-foreground">…</button>
```
- Active = `primary/10` fill + `primary` text. Inactive = muted text, hover fills `muted`.
- Optional trailing count badge: `rounded-full bg-primary px-1.5 py-0.5 text-[10px] text-primary-foreground`.

### 7.4 Buttons

| Variant   | Pattern                                                                                  |
| --------- | ---------------------------------------------------------------------------------------- |
| Primary   | `h-10 rounded-lg bg-primary px-4 text-sm font-medium text-primary-foreground shadow-sm hover:opacity-95` |
| Icon      | `h-10 w-10 rounded-lg border border-border bg-card hover:bg-muted`                        |
| Outline   | `rounded-lg border border-border bg-background py-2 text-sm font-medium hover:bg-muted`   |
| Ghost/small | `rounded-md border border-border px-2.5 py-1.5 text-xs font-medium hover:bg-muted` ("Open") |
| Link      | `text-sm font-medium hover:underline` in `primary`, usually + `chevron-right`            |

### 7.5 Input (search)
```html
<input class="h-10 w-80 rounded-lg border border-border bg-card pl-9 pr-3 text-sm
              outline-none focus:ring-2"
       style="--tw-ring-color:rgba(13,148,136,0.3);">
```
Leading icon absolutely positioned at `left-3`; focus shows a 2px teal ring at 30% opacity.

### 7.6 Avatar
- Circle `rounded-full`, initials in `font-display text-xs font-semibold`.
- Two fills in use: `accent` (`#ccfbf1`/`#0f3d3e`) for patients, `secondary` (`#eef6f8`/`#2d3a4d`)
  for message senders.
- **Online dot:** `h-2.5 w-2.5 rounded-full` primary, with a 2px card-colored border ring,
  positioned `-right-0.5 -top-0.5`.

### 7.7 Status pill / badge
```html
<span class="rounded-full px-2.5 py-0.5 text-[11px] font-medium"
      style="background:rgba(34,197,94,0.15);color:#22c55e;">Checked in</span>
```
| Status      | Tint                 |
| ----------- | -------------------- |
| Checked in  | success              |
| Waiting     | warning              |
| Upcoming    | `bg-muted text-muted-foreground` |
| New / count | primary tint or solid|

Risk badges add a leading `h-1.5 w-1.5 rounded-full bg-current` dot: **Low** = success,
**Moderate** = warning, **High** = destructive.

### 7.8 List row (schedule)
Time (`w-14` right-aligned, display) → icon tile (`h-10 w-10 rounded-xl` accent) → name + subtitle
(truncating) → status pill → optional "Open" button. Rows separated by `divide-y divide-border`.

### 7.9 Message item
Avatar (+ online dot) → name + right-aligned timestamp (`text-[11px] muted`) → truncated preview.
Whole row is `rounded-lg p-2 hover:bg-muted`.

### 7.10 Table
```html
<div class="overflow-hidden rounded-xl border border-border">
  <table class="w-full text-sm">
    <thead class="text-xs uppercase tracking-wide text-muted-foreground"
           style="background:rgba(241,245,247,0.6);"> … </thead>
    <tbody class="divide-y divide-border"> … </tbody>
  </table>
</div>
```
- Header: tinted muted background, uppercase tracked labels, `px-4 py-2.5`.
- Cells: `px-4 py-3`; supporting cells use `text-muted-foreground`.
- Row hover: `hover:bg-muted/40`.

### 7.11 Task item
Checkbox `h-4 w-4 rounded border-border` with `accent-color:#0d9488` → label (`flex-1 text-sm`)
→ due chip (`rounded-md bg-muted px-2 py-0.5 text-[11px] muted`).

### 7.12 Gradient metric panel
`rounded-2xl` card with brand gradient + white text. Inner metric tiles: `rounded-xl bg-white/10
p-3 backdrop-blur`, each a `font-display text-2xl` number + `text-[11px] opacity-80` label, in a
`grid-cols-2 gap-4`.

---

## 8. Interaction & state

| State    | Treatment                                                            |
| -------- | -------------------------------------------------------------------- |
| Hover    | Surfaces → `bg-muted` / `bg-muted/40`; stat cards → `shadow-md`; links → underline; primary button → `opacity-95` |
| Focus    | `focus:ring-2` with teal ring (`ring` token / `rgba(13,148,136,0.3)`)|
| Active   | Nav active = `bg-primary/10 text-primary`                            |
| Transitions | `transition-shadow` / `transition-colors`                         |

> **Gaps (not yet defined):** disabled, loading/skeleton, empty, and error states; explicit
> keyboard-focus styling on every interactive element; and a dark theme. Add these when extending.

---

## 9. Voice & content

- **Tone:** warm, clinical, concise. First-person greeting ("Good morning, Dr. Hart").
- **Trust signals:** surface security/compliance inline ("HIPAA-encrypted", "Secure messages").
- **Numbers first:** lead cards with the metric, then a short qualifying delta.
- **Truncate, don't wrap:** names and previews use `truncate` to protect layout.

---

## 10. Quick reference (copy-paste tokens)

```js
// tailwind.config.theme.extend
colors: {
  background:'#f6f8f9', foreground:'#1a2332', card:'#ffffff',
  primary:'#0d9488', 'primary-foreground':'#f0fdf9',
  secondary:'#eef6f8', 'secondary-foreground':'#2d3a4d',
  muted:'#f1f5f7', 'muted-foreground':'#64748b',
  accent:'#ccfbf1', 'accent-foreground':'#0f3d3e',
  destructive:'#dc2626', 'destructive-foreground':'#ffffff',
  success:'#22c55e', 'success-foreground':'#ffffff',
  warning:'#f59e0b', 'warning-foreground':'#451a03',
  info:'#3b82f6', 'info-foreground':'#ffffff',
  border:'#e2e8f0', ring:'#0d9488',
},
fontFamily: {
  sans:['Inter','ui-sans-serif','system-ui','sans-serif'],
  display:['Plus Jakarta Sans','ui-sans-serif','system-ui','sans-serif'],
},
borderRadius: { '2xl':'1rem', xl:'0.875rem', lg:'0.75rem' },
```
