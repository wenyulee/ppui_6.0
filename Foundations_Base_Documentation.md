# Foundations / Base Documentation

**Section:** Foundations / Base
**Version:** 6.0 Beta
**Figma Nodes:** `7087-12067` (cover), `7087-12058` (Blur), `7087-12071` (Border Size), `7087-12522` (Border Radius), `7087-12386` (Elevation Shadow), `7087-12400` (Size), `7087-12455` (Spacing), `32048-12581` (Typography Ramp), `32048-12634` (Letter Spacing), `7087-12626` (Layout Grid), `27086-12771` (Motion)
**Last Updated:** 2026-03-18
**Status:** Beta

---

## Table of Contents

1. [Overview](#overview)
2. [Blur](#blur)
3. [Border — Size](#border--size)
4. [Border — Radius](#border--radius)
5. [Elevation / Shadow](#elevation--shadow)
6. [Size](#size)
7. [Spacing](#spacing)
8. [Typography — Ramp](#typography--ramp)
9. [Typography — Letter Spacing](#typography--letter-spacing)
10. [Layout — Grid](#layout--grid)
11. [Motion Tokens](#motion-tokens)
12. [Token Naming Convention](#token-naming-convention)
13. [Usage Guidelines](#usage-guidelines)

---

## Overview

The **Foundations / Base** layer defines the core design tokens that power every component in the design system. These are the lowest-level primitives — raw values for blur, borders, elevation, sizing, spacing, typography, layout, and motion — that semantic and component-level tokens reference upward.

All tokens follow the namespace pattern:

```
tokens.base.<category>.<key>
```

Responsive variants (for typography) follow:

```
tokens.responsive.text.<property>.<scale>.<size>
```

Color scheme–aware tokens follow:

```
tokens.colorScheme.<role>.<variant>
```

> **Important:** Always reference tokens by name, never by raw value. Raw values are subject to change; the token name is the stable contract.

---

## Blur

**Token namespace:** `tokens.base.blur[n]`

The blur token is used for backdrop-blur effects — glass morphism surfaces, frosted overlays, and modal backdrops.

### Blur Scale

| Token | CSS Property | Value | Usage |
|---|---|---|---|
| `tokens.base.blur[8]` | `backdrop-filter: blur(8px)` | 8px | Modal backdrops, frosted overlay surfaces |

### CSS Custom Property

```css
--blur-8: 8px;

/* Usage */
.frosted-surface {
  backdrop-filter: blur(var(--blur-8));
  background: rgba(255, 255, 255, 0.01); /* near-transparent to enable backdrop-filter */
}
```

### Usage Notes

- `blur-8` is the only blur step in the base scale for v6. Additional levels may be added in future versions.
- Always pair backdrop-blur with a translucent background color — a fully opaque background will obscure the blur effect.
- Test blur performance on low-powered devices; backdrop-filter is GPU-accelerated but can degrade on older hardware.

---

## Border — Size

**Token namespace:** `tokens.base.border.size[n]`

Border size tokens define the thickness of strokes and dividers. The scale is intentionally narrow to maintain visual consistency.

### Border Size Scale

| Token | Value | Visual | Usage |
|---|---|---|---|
| `tokens.base.border.size[1]` | 1px | Thin line | Default borders, dividers, card outlines, input borders at rest |
| `tokens.base.border.size[2]` | 2px | Medium line | Focus rings, selected states, active borders (e.g. selected card 1px → 2px) |
| `tokens.base.border.size[3]` | 3px | Thick line | High-emphasis borders, error state highlights |

### CSS Custom Properties

```css
--border-size-1: 1px;
--border-size-2: 2px;
--border-size-3: 3px;
```

### Usage by Component

| Use Case | Token |
|---|---|
| Card border (rest) | `size-1` |
| Input border (rest) | `size-1` |
| Input border (focus/typing) | `size-2` |
| Selection card (selected) | `size-2` |
| Focus ring | `size-2` |
| Dividers | `size-1` |
| Timeline connector | `size-2` (used as border-height) |

---

## Border — Radius

**Token namespace:** `tokens.base.border.radius[n]` / `tokens.base.border.radius.full`

Border radius tokens define corner rounding. The scale spans from sharp utility corners to full pill shapes.

### Border Radius Scale

| Token | Value | Example Shape | Usage |
|---|---|---|---|
| `tokens.base.border.radius[4]` | 4px | Small square → soft corner | Text shimmers, small chips, tags |
| `tokens.base.border.radius[8]` | 8px | Moderate rounding | Buttons (secondary/tertiary), input fields, tooltips, container shimmers |
| `tokens.base.border.radius[12]` | 12px | Rounded rectangle | Cards, menus, dropdowns, modals, tab bars |
| `tokens.base.border.radius[24]` | 24px | Large rounded rectangle | Overlays, bottom sheets, modals |
| `tokens.base.border.radius[32]` | 32px | Very round rectangle | Large cards, hero sections, full-bleed cards |
| `tokens.base.border.radius.full` | 999px | Full pill | Buttons (primary/pill), switches, badges, tags, pagination dots |

### CSS Custom Properties

```css
--radius-4:    4px;
--radius-8:    8px;
--radius-12:   12px;
--radius-24:   24px;
--radius-32:   32px;
--radius-full: 999px;
```

### Semantic Aliases (Recommended)

Rather than referencing raw radius tokens directly in components, use semantic aliases that map to the base scale:

```css
--radius-xs:   var(--radius-4);   /* 4px  — chips, tags */
--radius-sm:   var(--radius-8);   /* 8px  — inputs, buttons */
--radius-md:   var(--radius-12);  /* 12px — cards, modals */
--radius-lg:   var(--radius-24);  /* 24px — sheets */
--radius-xl:   var(--radius-32);  /* 32px — large cards */
--radius-pill: var(--radius-full);/* 999px — pills, switches */
```

### Usage by Component

| Component | Radius |
|---|---|
| Button (Primary / pill-shaped) | `radius-full` (999px) |
| Button (Secondary / rectangular) | `radius-8` |
| Input / Text Field | `radius-8` |
| Card | `radius-12` |
| Tab Bar container | `radius-12` |
| Tab Item (active pill) | `radius-full` |
| Switch track | `radius-full` |
| Badge / Tag | `radius-full` |
| Modal (Dialog) | `radius-24` |
| Bottom Sheet | `radius-24` (top corners only) |
| Tooltip / Coachtip | `radius-8` |
| Shimmer — Text variant | `radius-4` |
| Shimmer — Container variant | `radius-8` |

---

## Elevation / Shadow

**Token namespace:** `tokens.colorScheme.shadow.base` / `tokens.colorScheme.shadow.emphasis`

The elevation system uses layered drop-shadows using two shadow color tokens to create depth. All shadows use the brand's signature blue-tinted dark tones rather than pure grey.

### Shadow Color Tokens

| Token | Light Mode Value | Usage |
|---|---|---|
| `tokens.colorScheme.shadow.base` | `rgba(5, 55, 130, 0.04)` | Subtle base shadow layer |
| `tokens.colorScheme.shadow.emphasis` | `rgba(5, 55, 130, 0.08)` | Stronger emphasis shadow layer |

> The `rgb(5, 55, 130)` base color is a deep brand blue, giving shadows a warm, intentional hue rather than a flat grey.

### Elevation Levels

#### Level 1 — Subtle lift

Used for: Slider thumb, floating action elements, cards at rest that need minimal separation.

```
Effect 1: DROP_SHADOW  offset(0, 4)  radius 4  color: shadow.base     (rgba(5,55,130,0.04))
Effect 2: DROP_SHADOW  offset(0, 0)  radius 8  color: shadow.emphasis (rgba(5,55,130,0.08))
```

```css
--elevation-level-1:
  0px 4px 4px rgba(5, 55, 130, 0.04),
  0px 0px 8px rgba(5, 55, 130, 0.08);
```

#### Level 2 — Standard elevation

Used for: Dropdowns, menus, tooltips, popovers, date pickers.

```
Effect 1: DROP_SHADOW  offset(0, 2)  radius 8   color: shadow.base     (rgba(5,55,130,0.04))
Effect 2: DROP_SHADOW  offset(0, 4)  radius 4   color: shadow.base     (rgba(5,55,130,0.04))
Effect 3: DROP_SHADOW  offset(0, 0)  radius 12  color: shadow.emphasis (rgba(5,55,130,0.08))
```

```css
--elevation-level-2:
  0px 2px 8px  rgba(5, 55, 130, 0.04),
  0px 4px 4px  rgba(5, 55, 130, 0.04),
  0px 0px 12px rgba(5, 55, 130, 0.08);
```

#### Level 3 — High elevation

Used for: Modals, bottom sheets, dialogs, full-screen overlays.

```
Effect 1: DROP_SHADOW  offset(0, 2)  radius 8   color: shadow.base     (rgba(5,55,130,0.04))
Effect 2: DROP_SHADOW  offset(0, 4)  radius 4   color: shadow.base     (rgba(5,55,130,0.04))
Effect 3: DROP_SHADOW  offset(0, 4)  radius 20  color: shadow.emphasis (rgba(5,55,130,0.08))
```

```css
--elevation-level-3:
  0px 2px 8px  rgba(5, 55, 130, 0.04),
  0px 4px 4px  rgba(5, 55, 130, 0.04),
  0px 4px 20px rgba(5, 55, 130, 0.08);
```

### Elevation Usage Guide

| Level | Component Examples |
|---|---|
| Level 1 | Slider thumb, Payment card thumbnail, Floating button |
| Level 2 | Menu dropdown, Tooltip, Coachtip, Date picker popover |
| Level 3 | Modal dialog, Bottom sheet, Full-screen overlay |

### Elevation Principles

- Use elevation sparingly — only when spatial layering is semantically meaningful.
- Higher elevation = further from the base surface = larger/more diffuse shadow.
- Never apply elevation to elements that are laid flat (e.g., regular cards in a list that do not float above the content layer).
- In dark mode, reduce shadow opacity or replace with border-based separation, as shadows are less visible on dark backgrounds.

---

## Size

**Token namespace:** `tokens.base.size[n]`

The size scale defines fixed pixel dimensions used for element widths, heights, icon sizes, avatar sizes, and other structural dimensions. These are distinct from spacing tokens (which define gaps and padding) — size tokens define the intrinsic dimensions of elements.

### Size Scale

| Token | Value | Common Usage |
|---|---|---|
| `tokens.base.size[2]` | 2px | Divider thickness, very thin rule |
| `tokens.base.size[4]` | 4px | Micro indicator dot |
| `tokens.base.size[8]` | 8px | Tiny icon, badge indicator |
| `tokens.base.size[16]` | 16px | Small icon (default), step indicator |
| `tokens.base.size[20]` | 20px | Slider thumb, small avatar, icon (medium-small) |
| `tokens.base.size[24]` | 24px | Icon (standard), switch thumb area |
| `tokens.base.size[28]` | 28px | Switch thumb (27px rounds to this) |
| `tokens.base.size[32]` | 32px | Icon (large), tag/chip height |
| `tokens.base.size[40]` | 40px | Button height (default), tab item height, touch target minimum |
| `tokens.base.size[48]` | 48px | Touch target (min for accessibility), avatar (medium) |
| `tokens.base.size[56]` | 56px | Button height (large), list item height |
| `tokens.base.size[64]` | 64px | Avatar (large), icon (x-large), progress bar segment |
| `tokens.base.size[80]` | 80px | Avatar (x-large), illustration spot |

### CSS Custom Properties

```css
--size-2:   2px;
--size-4:   4px;
--size-8:   8px;
--size-16:  16px;
--size-20:  20px;
--size-24:  24px;
--size-28:  28px;
--size-32:  32px;
--size-40:  40px;
--size-48:  48px;
--size-56:  56px;
--size-64:  64px;
--size-80:  80px;
```

### Key Size Landmarks

| Dimension | Value | Significance |
|---|---|---|
| Minimum touch target | 48px | WCAG 2.5.5 (AAA) / Apple HIG / Material Design minimum |
| Default icon size | 16px | Small icon — system icons, inline icons |
| Standard icon size | 24px | Default standalone icon size |
| Default button height | 40px | Standard interactive element minimum height |

---

## Spacing

**Token namespace:** `tokens.base.spacing[n]`

The spacing scale defines all gaps, margins, padding, and layout distances. It is used for both internal component spacing (padding within a component) and external layout spacing (gaps between components).

### Spacing Scale

| Token | Value | Usage Category |
|---|---|---|
| `tokens.base.spacing[2]` | 2px | Micro: text-to-icon tight coupling, sub-element gap |
| `tokens.base.spacing[4]` | 4px | XS: icon internal padding, badge offset |
| `tokens.base.spacing[8]` | 8px | S: between label and description, between items in dense list |
| `tokens.base.spacing[12]` | 12px | S+: icon-to-label gap, form field internal gap |
| `tokens.base.spacing[16]` | 16px | M: standard component padding, list item padding |
| `tokens.base.spacing[20]` | 20px | M+: section internal padding |
| `tokens.base.spacing[24]` | 24px | L: between distinct elements, radio group item gap |
| `tokens.base.spacing[28]` | 28px | L+: section separator |
| `tokens.base.spacing[32]` | 32px | XL: content block padding, section top padding |
| `tokens.base.spacing[36]` | 36px | XL+: section gap |
| `tokens.base.spacing[40]` | 40px | 2XL: large section gap |
| `tokens.base.spacing[48]` | 48px | 2XL+: page section gap |
| `tokens.base.spacing[56]` | 56px | 3XL: screen-level vertical rhythm |
| `tokens.base.spacing[64]` | 64px | 3XL+: page outer padding (desktop) |
| `tokens.base.spacing[96]` | 96px | 4XL: large layout section gap |
| `tokens.base.spacing[128]` | 128px | 5XL: hero spacing, very large layout gap |

### CSS Custom Properties

```css
--space-2:   2px;
--space-4:   4px;
--space-8:   8px;
--space-12:  12px;
--space-16:  16px;
--space-20:  20px;
--space-24:  24px;
--space-28:  28px;
--space-32:  32px;
--space-36:  36px;
--space-40:  40px;
--space-48:  48px;
--space-56:  56px;
--space-64:  64px;
--space-96:  96px;
--space-128: 128px;
```

### Spacing Principles

- **Use the scale.** Never use arbitrary values like `13px` or `17px`. If a value falls between two steps, use the closer step.
- **Internal spacing** (padding within a component): typically `space-8` through `space-24`.
- **External spacing** (gaps between components): typically `space-16` through `space-64`.
- **Page-level padding**: `space-64` on desktop; `space-16` to `space-24` on mobile.
- **The 8px base unit** (`space-8`) is the fundamental rhythm unit. Most steps are multiples of 4 or 8.

### Common Component Spacing Reference

| Spacing | Value | Component Examples |
|---|---|---|
| `space-2` | 2px | Title → Description gap (Timeline), text sub-label gap |
| `space-4` | 4px | Connector–icon gap (Timeline), tab item gap |
| `space-8` | 8px | Text shimmer line gap, between label + description, group items |
| `space-12` | 12px | Switch → label gap, avatar → text gap |
| `space-16` | 16px | Card padding, modal section gap, list item padding, tab bar padding |
| `space-24` | 24px | Radio group item gap, section separator |
| `space-32` | 32px | Page content section gap |
| `space-64` | 64px | Desktop page outer padding |

---

## Typography — Ramp

**Token namespace:** `tokens.base.text.fontsize.ramp[n]` / `tokens.base.text.lineheight.ramp[n]`

The typography ramp defines the core font-size and line-height pairing table. These are the primitive values that semantic type styles (Display, Heading, Body, Label) map to.

### Font Size & Line Height Ramp

| Ramp Step | Font Size | Line Height | Ratio | Usage |
|---|---|---|---|---|
| `ramp[120]` | 120px | 120px (1.0) | 1.0 | Hero display — extreme display text |
| `ramp[96]` | 96px | 96px (1.0) | 1.0 | Display XXL |
| `ramp[76]` | 76px | 80px | ~1.05 | Display Large (responsive desktop) |
| `ramp[60]` | 60px | 64px | ~1.07 | Display Medium |
| `ramp[48]` | 48px | 48px (1.0) | 1.0 | Display Small |
| `ramp[40]` | 40px | 40px (1.0) | 1.0 | Heading XL |
| `ramp[32]` | 32px | 32px (1.0) | 1.0 | Heading Large |
| `ramp[24]` | 24px | 32px | 1.33 | Heading Medium |
| `ramp[20]` | 20px | 28px | 1.4 | Heading Small |
| `ramp[16]` | 16px | 24px | 1.5 | Body Large / Label Large |
| `ramp[14]` | 14px | 20px | 1.43 | Body Medium / Label Medium |
| `ramp[12]` | 12px | 16px | 1.33 | Body Small / Label Small |
| `ramp[10]` | 10px | 12px | 1.2 | Caption / legal / micro text |

### Semantic Type Style Mapping

The semantic type styles used in components map to ramp values as follows:

#### Display Styles (responsive — values shown at desktop breakpoint)

| Style Token | Font Size | Line Height | Weight | Letter Spacing |
|---|---|---|---|---|
| `Display/Large` | `ramp[76]` → 76px | 80px | Black (900) | `n3` (−3px) |
| `Display/Small` | `ramp[48]` → 48px | 48px | Black (900) | `n3` (−3px) |

#### Heading Styles (responsive)

| Style Token | Font Size | Line Height | Weight | Letter Spacing |
|---|---|---|---|---|
| `Heading/Medium` | `ramp[32]` → 32px | 32px | Black (900) | `n1` (−1px) |
| `Heading/Small` | `ramp[24]` → 24px | 32px | Black (900) | `n1` (−1px) |

#### Body Styles (fixed)

| Style Token | Font Size | Line Height | Weight | Letter Spacing |
|---|---|---|---|---|
| `Body/Large` | `ramp[16]` → 16px | 24px | Regular (400) | `0` |
| `Body/Medium` | `ramp[14]` → 14px | 20px | Regular (400) | `0` |
| `Body/Small` | `ramp[12]` → 12px | 16px | Regular (400) | `0` |

#### Label Styles (fixed)

| Style Token | Font Size | Line Height | Weight | Letter Spacing |
|---|---|---|---|---|
| `Label/Large` | `ramp[16]` → 16px | 24px | Medium (500) | `0` |
| `Label/Medium` | `ramp[14]` → 14px | 20px | Medium (500) | `0` |
| `Label/Small` | `ramp[12]` → 12px | 16px | Medium (500) | `0` |

### Font Families

| Category | Token | Font |
|---|---|---|
| Display | `tokens.brand.text.fontfamily.display` | `PayPal_Pro` (Black weight variant) |
| Heading | `tokens.brand.text.fontfamily.heading` | `PayPal_Pro` (Black weight variant) |
| Body | `tokens.brand.text.fontfamily.body` | Brand body font (Regular/Regular weight) |
| Label | `tokens.brand.text.fontfamily.label` | Brand label font (Medium weight) |
| Link | `tokens.brand.text.fontfamily.link` | Brand link font (Medium weight) |
| Code / Mono | `JetBrains Mono` (Medium) | Used in design system documentation only |

### Font Weight Tokens

| Token | Value | Usage |
|---|---|---|
| `tokens.brand.text.fontweight.display` | 900 (Black) | Display text |
| `tokens.brand.text.fontweight.heading` | 900 (Black) | Heading text |
| `tokens.brand.text.fontweight.body` | 400 (Regular) | Body text |
| `tokens.brand.text.fontweight.label` | 500 (Medium) | Label text |
| `tokens.brand.text.fontweight.link` | 500 (Medium) | Link text |

---

## Typography — Letter Spacing

**Token namespace:** `tokens.base.text.letterspacing.<step>`

Letter spacing tokens define the tracked spacing applied to text at various sizes. All values are negative (tightening) — larger text sizes use more tightening to maintain visual density.

### Letter Spacing Scale

| Token | Value | Applied at Size | Example |
|---|---|---|---|
| `tokens.base.text.letterspacing.n3` | −3px | `ramp[76]`+ (display large) | "Shop in stores" at 76px |
| `tokens.base.text.letterspacing.n2` | −2px | `ramp[60]` (display medium) | "Shop in stores" at 60px |
| `tokens.base.text.letterspacing.n1` | −1px | `ramp[40]` (heading XL) | "Shop in stores" at 40px |
| `tokens.base.text.letterspacing.n05` | −0.5px | `ramp[32]` (heading large) | "Shop in stores" at 32px |
| `tokens.base.text.letterspacing.n02` | −0.2px | `ramp[24]` (heading medium) | "Shop in stores" at 24px |
| `tokens.base.text.letterspacing[0]` | 0px | `ramp[16]` and below | All body/label/caption text |

### Responsive Letter Spacing Mapping

| Responsive Token | Value | Size Scale |
|---|---|---|
| `tokens.responsive.text.letterspacing.display.lg` | −3px | Display Large |
| `tokens.responsive.text.letterspacing.display.sm` | −3px | Display Small |
| `tokens.responsive.text.letterspacing.heading.md` | −1px | Heading Medium |
| `tokens.responsive.text.letterspacing.heading.sm` | −1px | Heading Small |

### Letter Spacing Principle

Letter spacing scales inversely with font size: the larger the text, the tighter the tracking. This maintains optical proportionality — large letterforms rendered at display sizes appear too loose without negative tracking.

---

## Layout — Grid

**Token namespace:** Layout grid (Figma Layout Guide)

The responsive grid system defines column counts, column widths, outer margins (gutters), and inner gaps across three breakpoints.

### Breakpoints

| Breakpoint | Range | Frame Width (Figma) |
|---|---|---|
| **Mobile** | 0px – 599px | 402px |
| **Tablet** | 600px – 1279px | 840px |
| **Desktop** | 1280px+ | 1440px |

### Grid Specifications

#### Mobile (0px – 599px)

| Property | Value |
|---|---|
| Columns | 2 |
| Column width | ~177px (flexible, fills available width) |
| Outer margin (left + right) | 15px each side |
| Inner gutter | ~18px (space between the 2 columns) |
| Content width | frame − 2 × margin = 402 − 30 = 372px |

```
|←15px→| col1 (177px) |←18px→| col2 (177px) |←15px→|
```

#### Tablet (600px – 1279px)

| Property | Value |
|---|---|
| Columns | 12 |
| Column width | 44px |
| Outer margin (left + right) | 24px each side |
| Inner gutter | 24px (space between each column) |
| Content width | 840 − 48 = 792px |

```
|←24px→| [44px] [24px] [44px] [24px] ... × 12 columns |←24px→|
```

#### Desktop (1280px+)

| Property | Value |
|---|---|
| Columns | 12 |
| Column width | 90px |
| Outer margin (left + right) | 48px each side |
| Inner gutter | 24px (space between each column) |
| Content width | 1440 − 96 = 1344px |

```
|←48px→| [90px] [24px] [90px] [24px] ... × 12 columns |←48px→|
```

### Grid Application in Figma

Use the **Layout Guide** in Figma's right-side panel to apply these grids to your frames. The grid is pre-configured per breakpoint.

> Banner note from the Figma file: "Use the 'Layout Guide' on the right side panel to apply grids to your frames."

### Common Column Spans

| Span | Mobile | Tablet | Desktop |
|---|---|---|---|
| Full width | 2/2 cols | 12/12 cols | 12/12 cols |
| Three-quarters | — | 9/12 cols | 9/12 cols |
| Half | 1/2 col | 6/12 cols | 6/12 cols |
| One-third | — | 4/12 cols | 4/12 cols |
| One-quarter | — | 3/12 cols | 3/12 cols |

---

## Motion Tokens

**Token namespace:** `tokens.motion.ease.*` / `tokens.motion.duration.*`

Motion tokens define the easing curves and timing durations used for all animations and transitions in the design system. They are organized into two categories: **Easing** (shape of the curve) and **Timing** (duration of the animation).

---

### Easing

#### Standard Easing

Standard easing tokens are for everyday UI transitions — appearing, disappearing, and state changes.

| Token | Value | Notes / When to Use |
|---|---|---|
| `ease.linear` | `cubic-bezier(0, 0, 1, 1)` | Progress indicators, loading bars, shimmer sweeps, mechanical/continuous motion |
| `ease.standard.in-out` | `cubic-bezier(0.8, 0, 0.2, 1)` | Simple, symmetrical transitions that both enter and exit (e.g., color changes, opacity fades) |
| `ease.standard.in` | `cubic-bezier(0, 0, 0, 1)` | Standard **entry** transitions — elements decelerating as they arrive (slide in, fade in) |
| `ease.standard.out` | `cubic-bezier(1, 0, 1, 1)` | Standard **exit** transitions — elements accelerating as they leave (slide out, fade out) |

#### Expressive Easing

Expressive easing tokens are for branded, high-impact moments — onboarding, success states, feature highlights.

| Token | Value | Notes / When to Use |
|---|---|---|
| `ease.expressive.in-out` | `cubic-bezier(0.95, 0, 0.05, 1)` | Branded expressive transitions; symmetrical — very strong ease-in and ease-out. High visual impact. |
| `ease.expressive.in` | `cubic-bezier(0, 0.45, 0.05, 1)` | Expressive **entrances** — sharp ramp-in with a smooth, gradual settle (decelerating in) |
| `ease.expressive.out` | `cubic-bezier(1, 0, 1, 0.55)` | Expressive **exits** — fast, confident release with an elegant, lingering finish (accelerating out) |
| `ease.expressive.loop` | `cubic-bezier(0.75, 0.4, 0.35, 0.6)` | Smooth, continuous looping animation with expressive ease on each cycle. For seamless non-linear loops (e.g., loading animations, Lottie loops) |

### CSS Custom Properties for Easing

```css
--ease-linear:            cubic-bezier(0, 0, 1, 1);
--ease-standard-in-out:   cubic-bezier(0.8, 0, 0.2, 1);
--ease-standard-in:       cubic-bezier(0, 0, 0, 1);
--ease-standard-out:      cubic-bezier(1, 0, 1, 1);
--ease-expressive-in-out: cubic-bezier(0.95, 0, 0.05, 1);
--ease-expressive-in:     cubic-bezier(0, 0.45, 0.05, 1);
--ease-expressive-out:    cubic-bezier(1, 0, 1, 0.55);
--ease-expressive-loop:   cubic-bezier(0.75, 0.4, 0.35, 0.6);
```

---

### Timing

Duration tokens define how long animations take. They are organized into three sub-categories: **Animation Timing** (for UI transitions), **Loop Timing** (for infinite animations), and **Display Timing** (for notification auto-dismiss).

#### Animation Timing

| Token | Value | Notes / When to Use |
|---|---|---|
| `duration.0` | 0ms | No animation. Use for `prefers-reduced-motion: reduce` fallbacks or programmatic state changes. |
| `duration.25` | 25ms | Subtle haptic-style micro-feedback. |
| `duration.50` | 50ms | Quick micro-feedback and binary state changes (e.g., checkbox check, toggle flick). |
| `duration.100` | 100ms | Hover effects, tap feedback, focus ring appearance. |
| `duration.150` | 150ms | Light interactions — icon swaps, color transitions on simple elements. |
| `duration.300` | 300ms | **Default duration.** Most UI transitions — dropdowns, tooltips, button state changes. |
| `duration.400` | 400ms | Larger surfaces — modals, side sheets, drawers. |
| `duration.600` | 600ms | Page-level transitions — full route changes, hero entrances. |

#### Loop Timing

| Token | Value | Notes / When to Use |
|---|---|---|
| `duration.loop` | `infinite` | Infinite / indeterminate animations — loaders, skeleton shimmers, progress bar indeterminate mode. |

#### Display Timing

| Token | Value | Notes / When to Use |
|---|---|---|
| `duration.5000` | 5000ms | Auto-dismiss timing for Toast / notification display. |

### CSS Custom Properties for Timing

```css
--duration-0:    0ms;
--duration-25:   25ms;
--duration-50:   50ms;
--duration-100:  100ms;
--duration-150:  150ms;
--duration-300:  300ms;
--duration-400:  400ms;
--duration-600:  600ms;
--duration-loop: infinite;
--duration-5000: 5000ms;
```

---

### Motion Usage Guide

#### Entry vs. Exit Pairing

Always pair an `in` easing with an `out` easing when an element both enters and exits:

```css
/* Element entering the screen */
.modal-enter {
  animation: slide-up var(--duration-300) var(--ease-standard-in);
}

/* Element leaving the screen */
.modal-exit {
  animation: slide-down var(--duration-300) var(--ease-standard-out);
}
```

#### Easing Selection Decision Tree

```
Is the animation branded/expressive (onboarding, success, hero)?
  → Yes: Use ease.expressive.*
  → No:
    Is the animation looping continuously?
      → Yes: Use ease.linear (progress) or ease.expressive.loop (branded loop)
      → No:
        Is the element entering, exiting, or doing both?
          → Both:  ease.standard.in-out
          → Enter: ease.standard.in
          → Exit:  ease.standard.out
```

#### Duration Selection Guide

| Animation Type | Recommended Duration |
|---|---|
| Hover state change | `duration.100` |
| Toggle / switch | `duration.150` |
| Tooltip appearance | `duration.150` |
| Dropdown menu | `duration.300` |
| Tab / segment transition | `duration.250` (between 150 and 300) |
| Modal entrance | `duration.300` – `duration.400` |
| Bottom sheet entrance | `duration.400` |
| Page-level transition | `duration.600` |
| Shimmer loop | `duration.loop` (+ 1.6s cycle via CSS) |
| Toast auto-dismiss | `duration.5000` |

### Reduced Motion

All animated elements must respect `prefers-reduced-motion`:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: var(--duration-0) !important;
    transition-duration: var(--duration-0) !important;
  }
}
```

For infinite animations (loaders, shimmers), replace with a static visual state — do not merely slow them down.

---

## Token Naming Convention

All base tokens follow a consistent naming pattern:

```
tokens.<tier>.<category>.<scale-key>
```

| Tier | Description | Example |
|---|---|---|
| `base` | Raw, fixed-value primitive tokens | `tokens.base.spacing[16]` |
| `brand` | Brand-specific overrides (font, weight) | `tokens.brand.text.fontfamily.display` |
| `responsive` | Values that change across breakpoints | `tokens.responsive.text.fontsize.display.lg` |
| `colorScheme` | Semantic color tokens (light/dark aware) | `tokens.colorScheme.shadow.emphasis` |

### Tier Hierarchy

```
colorScheme tokens       ← Semantic, theme-aware (use in components)
       ↑
brand tokens             ← Brand identity overrides
       ↑
base tokens              ← Raw primitives (spacing, size, radius, blur, motion)
```

---

## Usage Guidelines

### Dos

- **Always use tokens.** Never hard-code raw values (`padding: 13px`) — use the nearest token step.
- **Use semantic tokens in components.** Prefer `--color-brand-primary` over `tokens.base.color.blue.500` — semantic tokens are theme-aware.
- **Match radius to component scale.** Small components (badges, chips) use `radius-4` or `radius-8`; larger surfaces (modals, cards) use `radius-12` through `radius-32`.
- **Pair elevation with functional purpose.** Only use shadows to communicate genuine spatial layering, not decoration.
- **Follow the motion hierarchy.** Use `ease.standard.*` by default; reserve `ease.expressive.*` for brand moments.
- **Respect `prefers-reduced-motion`.** All transitions and animations must have a `duration.0` fallback.

### Don'ts

- **Don't skip steps in the scale.** If `space-16` feels too small and `space-24` too large, use `space-20` — don't invent `space-18`.
- **Don't mix easing families.** Pairing `ease.expressive.in` with `ease.standard.out` creates visual inconsistency.
- **Don't apply elevation purely for aesthetics.** Elevation communicates z-depth — overusing it flattens its meaning.
- **Don't use display font weight on body text.** Black (900) is reserved for display and heading sizes only.
- **Don't use `duration.600` for small UI transitions.** Long durations on small elements feel sluggish — reserve `duration.600` for full-page transitions.

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Blur (blur-8), Border Size (1–3px), Border Radius (4–999px, 6 steps), Elevation (3 levels, blue-tinted shadow tokens), Size ramp (2–80px, 13 steps), Spacing ramp (2–128px, 16 steps), Typography ramp (10–120px, 13 steps), Letter Spacing (0 to −3px, 6 steps), Layout Grid (Mobile 2col / Tablet 12col / Desktop 12col), Motion Tokens (8 easing curves, 11 duration steps) |
