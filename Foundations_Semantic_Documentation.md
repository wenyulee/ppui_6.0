# Foundations / Semantic — Design Token Documentation

**File:** Components v6 Beta
**Figma Nodes:** `32048-12694`, `7087-12088`, `7087-12551`, `7087-12564`, `7087-12577`, `7087-12600`, `7087-12587`, `7087-12613`
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

Semantic tokens are the **second layer** of the design token system, sitting between base/primitive tokens and components. While base tokens define raw values (e.g., `spacing[16]` = 16px), semantic tokens assign **meaning** and **intent** to those values (e.g., `content.muted` = text that is visually de-emphasized).

Semantic tokens cover two primary domains documented in this file:

1. **Color / Semantic** — A structured vocabulary of color roles across three categories: Content (text & icons), Background, and Border. Each category is further organized into base, muted, brand, utility, role (by status), elevated, container, overlay, and shimmer sub-categories.

2. **Typography / Semantic** — Six named text styles (Display, Heading, Title, Label, Body, Link), each with size variants (Large / Medium / Small), mapped to specific font families, weights, sizes, line-heights, and letter-spacings.

All semantic tokens reference base tokens under the hood. They are consumed via CSS custom properties in the format `--tokens.colorScheme.<category>` and `--tokens.brand.text.<property>` / `--tokens.responsive.text.<property>`.

---

## 1. Color Semantic Tokens

### 1.1 Token Hierarchy

```
tokens.colorScheme
├── content          ← text & icon colors
│   ├── base / muted / faint / link
│   ├── brand.primary / secondary / tertiary
│   ├── utility.inverse / selected / unselected
│   └── role.[base|emphasis].[info|negative|neutral|positive|special|warning]
├── background       ← surface fill colors
│   ├── base / muted
│   ├── brand.primary / secondary / tertiary
│   ├── utility.emphasis / selected / unselected
│   ├── container.filled / outlined
│   ├── elevated.modal / popover / scrim
│   ├── overlay.card / progressbar / ui
│   ├── shimmer.start / end
│   └── role.[base|emphasis].[info|negative|neutral|positive|special|warning]
└── border           ← stroke & outline colors
    ├── base / muted / focus
    ├── utility.emphasis / glow
    ├── container.filled / outlined
    └── role.[base|emphasis].[info|negative|neutral|positive|special|warning]
```

### 1.2 Content Tokens

Content tokens are used for **text and iconography**. They define how content should appear at different levels of visual emphasis.

| Token | CSS Variable | Light Mode Value | Usage |
|---|---|---|---|
| `content.base` | `--tokens.colorScheme.content.base` | `#000000` | Primary body text, high-emphasis labels |
| `content.muted` | `--tokens.colorScheme.content.muted` | `rgba(0,0,0,0.63)` | Secondary text, placeholders, section labels |
| `content.faint` | `--tokens.colorScheme.content.faint` | `rgba(0,0,0,0.48)` | Hint text, disabled labels, captions |
| `content.link` | `--tokens.colorScheme.content.link` | `#0066f5` | Hyperlinks and text-style CTAs |
| `content.utility.inverse` | `--tokens.colorScheme.content.utility.inverse` | `#ffffff` | Text on dark/filled backgrounds |
| `content.utility.selected` | `--tokens.colorScheme.content.utility.selected` | `#000000` | Active/selected tab, toggle label |
| `content.utility.unselected` | `--tokens.colorScheme.content.utility.unselected` | `rgba(0,0,0,0.63)` | Inactive tab or toggle label |
| `content.brand.primary` | `--tokens.colorScheme.content.brand.primary` | `#000000` | Text on brand-primary background (#60cdff) |
| `content.brand.secondary` | `--tokens.colorScheme.content.brand.secondary` | `#ffffff` | Text on brand-secondary background (#0066f5) |
| `content.brand.tertiary` | `--tokens.colorScheme.content.brand.tertiary` | `#ffffff` | Text on brand-tertiary background (#002991) |

**Role-based content tokens** (semantic status colors):

| Token | Base Value | Emphasis Value | Usage |
|---|---|---|---|
| `content.role.*.info` | `#0066f5` | `#ffffff` | Informational state text/icon |
| `content.role.*.negative` | `#c31526` | `#ffffff` | Error / danger state text/icon |
| `content.role.*.neutral` | `#000000` | `#ffffff` | Neutral badge / tag text |
| `content.role.*.positive` | `#008053` | `#ffffff` | Success state text/icon |
| `content.role.*.special` | `#4422bf` | `#ffffff` | Special / promotional text/icon |
| `content.role.*.warning` | `#785000` | `#ffffff` | Warning state text/icon |

> **Note:** `content.role.emphasis.*` tokens are always `#ffffff` — they are designed for use on the filled/emphasis role backgrounds (e.g., a white label on a red `background.role.emphasis.negative` chip).

---

### 1.3 Background Tokens

Background tokens define **surface fills** for containers, overlays, semantic states, and brand expressions.

#### Base Surfaces

| Token | CSS Variable | Value | Usage |
|---|---|---|---|
| `background.base` | `--tokens.colorScheme.background.base` | `#ffffff` | Primary page / card background |
| `background.muted` | `--tokens.colorScheme.background.muted` | `#f5f7fa` | Subtle section fill, alternate rows |
| `background.utility.emphasis` | `--tokens.colorScheme.background.utility.emphasis` | `#000000` | Filled high-emphasis elements (active toggle track, etc.) |
| `background.utility.selected` | `--tokens.colorScheme.background.utility.selected` | `#000000` | Active selection highlight (e.g., active tab pill) |
| `background.utility.unselected` | `--tokens.colorScheme.background.utility.unselected` | `#999999` | Inactive toggle track |

#### Brand Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.brand.primary` | `#60cdff` | Hero sections, brand callouts (sky blue) |
| `background.brand.secondary` | `#0066f5` | Primary CTA fills, brand buttons (PayPal blue) |
| `background.brand.tertiary` | `#002991` | Deep brand backgrounds, high-contrast brand (navy) |

#### Container Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.container.filled` | `rgba(5,55,130,0.04)` | Tinted fill for cards with subtle depth |
| `background.container.outlined` | `#ffffff` | White background for outlined containers |

#### Elevated Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.elevated.modal` | `#ffffff` | Modal / dialog surface |
| `background.elevated.popover` | `#ffffff` | Tooltip, dropdown, menu surface |
| `background.elevated.scrim` | `rgba(0,0,0,0.63)` | Dark overlay behind modals (scrim) |

#### Overlay Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.overlay.card` | `rgba(255,255,255,0.63)` | Frosted card overlay on dark imagery |
| `background.overlay.progressbar` | `rgba(0,0,0,0.24)` | Progress bar track background |
| `background.overlay.ui` | `rgba(0,0,0,0.63)` | UI overlay (e.g., image overlay gradient) |

#### Shimmer Backgrounds

| Token | Value | Usage |
|---|---|---|
| `background.shimmer.start` | `rgba(255,255,255,0.24)` | Shimmer highlight (bright sweep start) |
| `background.shimmer.end` | `rgba(5,55,130,0.04)` | Shimmer base (subtle blue-tint end) |

#### Role-based Backgrounds

Each semantic role has two variants: **base** (tinted, for subtle states) and **emphasis** (filled, for strong states).

| Role | Base Value | Emphasis Value |
|---|---|---|
| `background.role.*.info` | `#e3f7ff` | `#0038ba` |
| `background.role.*.negative` | `#fbeaeb` | `#9f111f` |
| `background.role.*.neutral` | `rgba(5,55,130,0.08)` | `#000000` |
| `background.role.*.positive` | `#e6faee` | `#006b51` |
| `background.role.*.special` | `#efebff` | `#3514ae` |
| `background.role.*.warning` | `#fff5e1` | `#785000` |

---

### 1.4 Border Tokens

Border tokens define **stroke colors** for dividers, outlines, focus rings, and semantic state borders.

#### Base Borders

| Token | CSS Variable | Value | Usage |
|---|---|---|---|
| `border.base` | `--tokens.colorScheme.border.base` | `rgba(0,0,0,0.16)` | Default dividers, input outlines, card borders |
| `border.muted` | `--tokens.colorScheme.border.muted` | `rgba(5,55,130,0.08)` | Subtle dividers, section separators |
| `border.focus` | `--tokens.colorScheme.border.focus` | `#0066f5` | Focus rings on interactive elements (WCAG AA) |
| `border.utility.emphasis` | `--tokens.colorScheme.border.utility.emphasis` | `#000000` | High-contrast borders, active outlines |
| `border.utility.glow` | `--tokens.colorScheme.border.utility.glow` | `rgba(0,0,0,0)` | Transparent placeholder (for animated glow states) |

#### Container Borders

| Token | Value | Usage |
|---|---|---|
| `border.container.filled` | `rgba(255,255,255,0)` | Transparent border on filled containers |
| `border.container.outlined` | `rgba(0,0,0,0.16)` | Outlined container border (cards, inputs) |

#### Role-based Borders

| Role | Base Value | Emphasis Value |
|---|---|---|
| `border.role.*.info` | `#60cdff` | `#0038ba` |
| `border.role.*.negative` | `#f09c9f` | `#9f111f` |
| `border.role.*.neutral` | `rgba(0,0,0,0.16)` | `#000000` |
| `border.role.*.positive` | `#73e6ab` | `#006b51` |
| `border.role.*.special` | `#b5a0ff` | `#1d0088` |
| `border.role.*.warning` | `#ffe9be` | `#aa7100` |

---

## 2. Typography Semantic Tokens

The semantic typography system defines **six named text roles**, each with discrete size steps. Every role maps to a specific font family (brand font), weight, and uses the base text ramp or responsive text ramp for sizing.

### 2.1 Font Families

| Role | CSS Token | Typeface | Weight |
|---|---|---|---|
| Display | `--tokens.brand.text.fontfamily.display` | PayPal Pro (Black) | 900 |
| Heading | `--tokens.brand.text.fontfamily.heading` | PayPal Pro (Black) | 900 |
| Title | `--tokens.brand.text.fontfamily.title` | Plain (Medium) | 500 |
| Label | `--tokens.brand.text.fontfamily.label` | Plain (Medium) | 500 |
| Body | `--tokens.brand.text.fontfamily.body` | Plain (Regular) | 400 |
| Link | `--tokens.brand.text.fontfamily.link` | Plain (Medium) | 500 |

**Notes:**
- **PayPal Pro** is a proprietary brand typeface used for large-scale marketing text (Display) and structural headings.
- **Plain** is a neutral, geometric sans-serif used for UI text at reading sizes.
- Display and Heading share the same font family but use different responsive sizing tokens (`display.*` vs `heading.*`).
- Title, Label, Body, and Link all use `Plain` but differ in weight (Medium vs Regular) and decoration (Link adds `text-decoration: underline`).

---

### 2.2 Display Typography

Display type is for **hero headlines and marketing copy**. Sizes are responsive (mobile values shown; desktop values are larger — see Foundations Base responsive type ramp).

Font family: `tokens.brand.text.fontfamily.display` (PayPal Pro, Black / weight 900)
Feature settings: `'liga' 0` (ligatures disabled)

| Variant | Token Path | Font Size (Mobile) | Line Height (Mobile) | Letter Spacing |
|---|---|---|---|---|
| Display Large | `tokens.responsive.text.fontsize.display.lg` | **76px** | 80px (`display.lg`) | −3px |
| Display Medium | `tokens.responsive.text.fontsize.display.md` | **60px** | 64px (`display.md`) | −3px |
| Display Small | `tokens.responsive.text.fontsize.display.sm` | **48px** | 48px (`display.sm`) | −3px |

CSS usage:
```css
.text-display-lg {
  font-family: var(--tokens.brand.text.fontfamily.display, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.display.lg, 76px);
  line-height: var(--tokens.responsive.text.lineheight.display.lg, 80px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.display.lg, -3px);
  font-feature-settings: 'liga' 0;
}

.text-display-md {
  font-family: var(--tokens.brand.text.fontfamily.display, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.display.md, 60px);
  line-height: var(--tokens.responsive.text.lineheight.display.md, 64px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.display.md, -3px);
  font-feature-settings: 'liga' 0;
}

.text-display-sm {
  font-family: var(--tokens.brand.text.fontfamily.display, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.display.sm, 48px);
  line-height: var(--tokens.responsive.text.lineheight.display.sm, 48px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.display.sm, -3px);
  font-feature-settings: 'liga' 0;
}
```

---

### 2.3 Heading Typography

Heading type is for **section titles and content hierarchy within UI screens**. Also uses the responsive token system.

Font family: `tokens.brand.text.fontfamily.heading` (PayPal Pro, Black / weight 900)
Feature settings: `'liga' 0`

| Variant | Token Path | Font Size (Mobile) | Line Height (Mobile) | Letter Spacing |
|---|---|---|---|---|
| Heading Large | `tokens.responsive.text.fontsize.heading.lg` | **40px** | 40px (`heading.lg`) | −1px |
| Heading Medium | `tokens.responsive.text.fontsize.heading.md` | **32px** | 32px (`heading.md`) | −1px |
| Heading Small | `tokens.responsive.text.fontsize.heading.sm` | **24px** | 32px (`heading.sm`) | −1px |

CSS usage:
```css
.text-heading-lg {
  font-family: var(--tokens.brand.text.fontfamily.heading, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.heading.lg, 40px);
  line-height: var(--tokens.responsive.text.lineheight.heading.lg, 40px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.heading.lg, -1px);
  font-feature-settings: 'liga' 0;
}

.text-heading-md {
  font-family: var(--tokens.brand.text.fontfamily.heading, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.heading.md, 32px);
  line-height: var(--tokens.responsive.text.lineheight.heading.md, 32px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.heading.md, -1px);
  font-feature-settings: 'liga' 0;
}

.text-heading-sm {
  font-family: var(--tokens.brand.text.fontfamily.heading, 'PayPal_Pro', sans-serif);
  font-size: var(--tokens.responsive.text.fontsize.heading.sm, 24px);
  line-height: var(--tokens.responsive.text.lineheight.heading.sm, 32px);
  letter-spacing: var(--tokens.responsive.text.letterspacing.heading.sm, -1px);
  font-feature-settings: 'liga' 0;
}
```

---

### 2.4 Title Typography

Title type is for **card titles, modal headers, and prominent UI section labels**. Uses the base text ramp (non-responsive, fixed sizes).

Font family: `tokens.brand.text.fontfamily.title` (Plain, Medium / weight 500)
Letter spacing: 0px (all sizes)

| Variant | Font Size Token | Size Value | Line Height Token | Line Height |
|---|---|---|---|---|
| Title Large | `tokens.base.text.fontsize.ramp[20]` | **20px** | `tokens.base.text.lineheight.ramp[24]` | 32px |
| Title Medium | `tokens.base.text.fontsize.ramp[16]` | **16px** | `tokens.base.text.lineheight.ramp[16]` | 24px |

CSS usage:
```css
.text-title-lg {
  font-family: var(--tokens.brand.text.fontfamily.title, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[20\], 20px);
  line-height: var(--tokens.base.text.lineheight.ramp\[24\], 32px);
  letter-spacing: 0px;
}

.text-title-md {
  font-family: var(--tokens.brand.text.fontfamily.title, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[16\], 16px);
  line-height: var(--tokens.base.text.lineheight.ramp\[16\], 24px);
  letter-spacing: 0px;
}
```

---

### 2.5 Label Typography

Label type is for **form labels, button text, tab labels, badges, and short UI strings** that need medium-weight legibility.

Font family: `tokens.brand.text.fontfamily.label` (Plain, Medium / weight 500)
Letter spacing: 0px (all sizes)

| Variant | Font Size Token | Size Value | Line Height Token | Line Height |
|---|---|---|---|---|
| Label Large | `tokens.base.text.fontsize.ramp[16]` | **16px** | `tokens.base.text.lineheight.ramp[16]` | 24px |
| Label Medium | `tokens.base.text.fontsize.ramp[14]` | **14px** | `tokens.base.text.lineheight.ramp[14]` | 20px |
| Label Small | `tokens.base.text.fontsize.ramp[12]` | **12px** | `tokens.base.text.lineheight.ramp[12]` | 16px |

CSS usage:
```css
.text-label-lg {
  font-family: var(--tokens.brand.text.fontfamily.label, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[16\], 16px);
  line-height: var(--tokens.base.text.lineheight.ramp\[16\], 24px);
  letter-spacing: 0px;
}

.text-label-md {
  font-family: var(--tokens.brand.text.fontfamily.label, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[14\], 14px);
  line-height: var(--tokens.base.text.lineheight.ramp\[14\], 20px);
  letter-spacing: 0px;
}

.text-label-sm {
  font-family: var(--tokens.brand.text.fontfamily.label, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[12\], 12px);
  line-height: var(--tokens.base.text.lineheight.ramp\[12\], 16px);
  letter-spacing: 0px;
}
```

---

### 2.6 Body Typography

Body type is for **paragraph text, descriptions, list items, and long-form reading content**. Uses Regular weight for readability at small sizes.

Font family: `tokens.brand.text.fontfamily.body` (Plain, Regular / weight 400)
Letter spacing: 0px (all sizes)

| Variant | Font Size Token | Size Value | Line Height Token | Line Height |
|---|---|---|---|---|
| Body Large | `tokens.base.text.fontsize.ramp[16]` | **16px** | `tokens.base.text.lineheight.ramp[16]` | 24px |
| Body Medium | `tokens.base.text.fontsize.ramp[14]` | **14px** | `tokens.base.text.lineheight.ramp[14]` | 20px |
| Body Small | `tokens.base.text.fontsize.ramp[12]` | **12px** | `tokens.base.text.lineheight.ramp[12]` | 16px |

CSS usage:
```css
.text-body-lg {
  font-family: var(--tokens.brand.text.fontfamily.body, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[16\], 16px);
  line-height: var(--tokens.base.text.lineheight.ramp\[16\], 24px);
  letter-spacing: 0px;
}

.text-body-md {
  font-family: var(--tokens.brand.text.fontfamily.body, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[14\], 14px);
  line-height: var(--tokens.base.text.lineheight.ramp\[14\], 20px);
  letter-spacing: 0px;
}

.text-body-sm {
  font-family: var(--tokens.brand.text.fontfamily.body, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[12\], 12px);
  line-height: var(--tokens.base.text.lineheight.ramp\[12\], 16px);
  letter-spacing: 0px;
}
```

---

### 2.7 Link Typography

Link type matches Body sizing steps but uses **Medium weight** (Plain Medium) and includes **underline text decoration** to signal interactivity.

Font family: `tokens.brand.text.fontfamily.link` (Plain, Medium / weight 500)
Text decoration: `underline` (solid)
Letter spacing: 0px (all sizes)
Color: `--tokens.colorScheme.content.link` = `#0066f5`

| Variant | Font Size Token | Size Value | Line Height Token | Line Height |
|---|---|---|---|---|
| Link Large | `tokens.base.text.fontsize.ramp[16]` | **16px** | `tokens.base.text.lineheight.ramp[16]` | 24px |
| Link Medium | `tokens.base.text.fontsize.ramp[14]` | **14px** | `tokens.base.text.lineheight.ramp[14]` | 20px |
| Link Small | `tokens.base.text.fontsize.ramp[12]` | **12px** | `tokens.base.text.lineheight.ramp[12]` | 16px |

CSS usage:
```css
.text-link-lg {
  font-family: var(--tokens.brand.text.fontfamily.link, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[16\], 16px);
  line-height: var(--tokens.base.text.lineheight.ramp\[16\], 24px);
  letter-spacing: 0px;
  color: var(--tokens.colorScheme.content.link, #0066f5);
  text-decoration: underline;
  text-decoration-style: solid;
}

.text-link-md {
  font-family: var(--tokens.brand.text.fontfamily.link, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[14\], 14px);
  line-height: var(--tokens.base.text.lineheight.ramp\[14\], 20px);
  letter-spacing: 0px;
  color: var(--tokens.colorScheme.content.link, #0066f5);
  text-decoration: underline;
  text-decoration-style: solid;
}

.text-link-sm {
  font-family: var(--tokens.brand.text.fontfamily.link, 'Plain', sans-serif);
  font-size: var(--tokens.base.text.fontsize.ramp\[12\], 12px);
  line-height: var(--tokens.base.text.lineheight.ramp\[12\], 16px);
  letter-spacing: 0px;
  color: var(--tokens.colorScheme.content.link, #0066f5);
  text-decoration: underline;
  text-decoration-style: solid;
}
```

---

## 3. Typography Scale Summary

The full semantic typography scale, organized by role and size:

| Role | Variant | Size | Line Height | Letter Spacing | Font | Weight |
|---|---|---|---|---|---|---|
| Display | Large | 76px (mobile) | 80px | −3px | PayPal Pro | 900 |
| Display | Medium | 60px (mobile) | 64px | −3px | PayPal Pro | 900 |
| Display | Small | 48px (mobile) | 48px | −3px | PayPal Pro | 900 |
| Heading | Large | 40px (mobile) | 40px | −1px | PayPal Pro | 900 |
| Heading | Medium | 32px (mobile) | 32px | −1px | PayPal Pro | 900 |
| Heading | Small | 24px (mobile) | 32px | −1px | PayPal Pro | 900 |
| Title | Large | 20px | 32px | 0px | Plain | 500 |
| Title | Medium | 16px | 24px | 0px | Plain | 500 |
| Label | Large | 16px | 24px | 0px | Plain | 500 |
| Label | Medium | 14px | 20px | 0px | Plain | 500 |
| Label | Small | 12px | 16px | 0px | Plain | 500 |
| Body | Large | 16px | 24px | 0px | Plain | 400 |
| Body | Medium | 14px | 20px | 0px | Plain | 400 |
| Body | Small | 12px | 16px | 0px | Plain | 400 |
| Link | Large | 16px | 24px | 0px | Plain | 500 |
| Link | Medium | 14px | 20px | 0px | Plain | 500 |
| Link | Small | 12px | 16px | 0px | Plain | 500 |

> **Responsive sizing note:** Display and Heading use `tokens.responsive.*` tokens. These resolve to different pixel values across breakpoints (mobile, tablet, desktop). The values shown above are the **mobile** resolved values. At desktop breakpoints, Display Large scales up to ~120px and Heading Large to ~56px. Title, Label, Body, and Link use `tokens.base.text.*` tokens which are **fixed** (non-responsive).

---

## 4. TypeScript Types

### Color Token Paths

```typescript
// Content color tokens
type ContentToken =
  | 'content.base'
  | 'content.muted'
  | 'content.faint'
  | 'content.link'
  | 'content.utility.inverse'
  | 'content.utility.selected'
  | 'content.utility.unselected'
  | 'content.brand.primary'
  | 'content.brand.secondary'
  | 'content.brand.tertiary'
  | `content.role.base.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`
  | `content.role.emphasis.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`;

// Background color tokens
type BackgroundToken =
  | 'background.base'
  | 'background.muted'
  | 'background.utility.emphasis'
  | 'background.utility.selected'
  | 'background.utility.unselected'
  | 'background.brand.primary'
  | 'background.brand.secondary'
  | 'background.brand.tertiary'
  | 'background.container.filled'
  | 'background.container.outlined'
  | 'background.elevated.modal'
  | 'background.elevated.popover'
  | 'background.elevated.scrim'
  | 'background.overlay.card'
  | 'background.overlay.progressbar'
  | 'background.overlay.ui'
  | 'background.shimmer.start'
  | 'background.shimmer.end'
  | `background.role.base.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`
  | `background.role.emphasis.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`;

// Border color tokens
type BorderToken =
  | 'border.base'
  | 'border.muted'
  | 'border.focus'
  | 'border.utility.emphasis'
  | 'border.utility.glow'
  | 'border.container.filled'
  | 'border.container.outlined'
  | `border.role.base.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`
  | `border.role.emphasis.${'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning'}`;

// Semantic role type
type SemanticRole = 'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning';
type SemanticEmphasis = 'base' | 'emphasis';

// Typography scale
type TypographyRole = 'display' | 'heading' | 'title' | 'label' | 'body' | 'link';
type TypographySize = 'lg' | 'md' | 'sm';

interface TypographyStyle {
  fontFamily: string;
  fontSize: string; // CSS custom property reference
  lineHeight: string; // CSS custom property reference
  letterSpacing: string;
  fontWeight: 400 | 500 | 900;
  textDecoration?: 'underline';
}
```

### React Typography Component

```typescript
import React from 'react';

type TypographyRole = 'display' | 'heading' | 'title' | 'label' | 'body' | 'link';
type TypographySize = 'lg' | 'md' | 'sm';
type SemanticColor =
  | 'base' | 'muted' | 'faint' | 'link' | 'inverse'
  | 'info' | 'negative' | 'neutral' | 'positive' | 'special' | 'warning';

interface TypographyProps {
  role: TypographyRole;
  size?: TypographySize;
  color?: SemanticColor;
  as?: keyof JSX.IntrinsicElements;
  children: React.ReactNode;
  className?: string;
}

const colorMap: Record<SemanticColor, string> = {
  base: 'var(--tokens.colorScheme.content.base, #000)',
  muted: 'var(--tokens.colorScheme.content.muted, rgba(0,0,0,0.63))',
  faint: 'var(--tokens.colorScheme.content.faint, rgba(0,0,0,0.48))',
  link: 'var(--tokens.colorScheme.content.link, #0066f5)',
  inverse: 'var(--tokens.colorScheme.content.utility.inverse, #fff)',
  info: 'var(--tokens.colorScheme.content.role.base.info, #0066f5)',
  negative: 'var(--tokens.colorScheme.content.role.base.negative, #c31526)',
  neutral: 'var(--tokens.colorScheme.content.role.base.neutral, #000)',
  positive: 'var(--tokens.colorScheme.content.role.base.positive, #008053)',
  special: 'var(--tokens.colorScheme.content.role.base.special, #4422bf)',
  warning: 'var(--tokens.colorScheme.content.role.base.warning, #785000)',
};

export const Typography: React.FC<TypographyProps> = ({
  role,
  size = 'md',
  color = 'base',
  as,
  children,
  className,
}) => {
  // Determine semantic HTML element
  const defaultElement: Record<TypographyRole, keyof JSX.IntrinsicElements> = {
    display: 'h1',
    heading: 'h2',
    title: 'h3',
    label: 'span',
    body: 'p',
    link: 'a',
  };
  const Tag = as ?? defaultElement[role];

  return (
    <Tag
      className={`text-${role}-${size} ${className ?? ''}`}
      style={{ color: colorMap[color] }}
    >
      {children}
    </Tag>
  );
};
```

---

## 5. Semantic Color Usage Patterns

### 5.1 Status / Role Color Pattern

The role color system uses a consistent **base / emphasis** split across content, background, and border:

```tsx
// Base variant — tinted background with colored text/border
// Use for: inline badges, alert banners, info callouts
<div style={{
  background: 'var(--tokens.colorScheme.background.role.base.negative)',
  border: '1px solid var(--tokens.colorScheme.border.role.base.negative)',
  color: 'var(--tokens.colorScheme.content.role.base.negative)',
}}>
  Payment failed
</div>

// Emphasis variant — filled dark background with white text
// Use for: filled chips, status tags, prominent notifications
<div style={{
  background: 'var(--tokens.colorScheme.background.role.emphasis.positive)',
  color: 'var(--tokens.colorScheme.content.role.emphasis.positive)', // = white
}}>
  Verified
</div>
```

### 5.2 Focus Ring Pattern

Always use `border.focus` for keyboard focus indicators to ensure WCAG AA contrast compliance:

```css
/* Standard focus ring */
:focus-visible {
  outline: 2px solid var(--tokens.colorScheme.border.focus, #0066f5);
  outline-offset: 2px;
}
```

### 5.3 Muted/Faint Hierarchy

Use the content hierarchy tokens to establish visual weight without changing font size:

```tsx
<div>
  <p style={{ color: 'var(--tokens.colorScheme.content.base)' }}>
    Transaction amount    {/* primary — full black */}
  </p>
  <p style={{ color: 'var(--tokens.colorScheme.content.muted)' }}>
    Sent to John Doe      {/* secondary — 63% opacity */}
  </p>
  <p style={{ color: 'var(--tokens.colorScheme.content.faint)' }}>
    Completed 3 days ago  {/* tertiary — 48% opacity */}
  </p>
</div>
```

### 5.4 Typography Role Selection Guide

| Use Case | Recommended Role | Recommended Size |
|---|---|---|
| Page hero / landing headline | Display | lg or md |
| App section title (full screen) | Display | sm |
| Modal title, sheet header | Heading | lg |
| Card title, section header | Heading | md or sm |
| List item title, sidebar nav | Title | lg |
| Form field group label | Title | md |
| Button text | Label | lg or md |
| Tab label, badge text | Label | md |
| Tag / chip text | Label | sm |
| Long-form paragraph | Body | lg or md |
| Caption, helper text | Body | sm |
| Inline hyperlink | Link | (match surrounding Body size) |

---

## 6. Accessibility Specifications

### Color Contrast Requirements

The semantic color system is designed to meet **WCAG 2.1 AA** minimum contrast ratios. Key pairings:

| Foreground Token | Background Token | Contrast Ratio | Passes AA (4.5:1 text / 3:1 UI) |
|---|---|---|---|
| `content.base` (#000) | `background.base` (#fff) | 21:1 | ✅ |
| `content.muted` (rgba 0,0,0,0.63 = ~#5E5E5E) | `background.base` (#fff) | ~5.7:1 | ✅ |
| `content.faint` (rgba 0,0,0,0.48 = ~#848484) | `background.base` (#fff) | ~3.9:1 | ✅ small text |
| `content.link` (#0066f5) | `background.base` (#fff) | ~4.6:1 | ✅ |
| `content.role.base.negative` (#c31526) | `background.base` (#fff) | ~5.6:1 | ✅ |
| `content.utility.inverse` (#fff) | `background.utility.emphasis` (#000) | 21:1 | ✅ |
| `border.focus` (#0066f5) | `background.base` (#fff) | ~4.6:1 | ✅ (3:1 required for UI) |

### Typography Accessibility

All typography roles at their minimum sizes meet WCAG AA for normal text (at least 4.5:1 against standard backgrounds when used with `content.base` on `background.base`):

- **Minimum readable size:** Body Small / Label Small at 12px — use only for captions or supplementary content, not primary instructions.
- **Display and Heading** sizes (24px+) fall under WCAG "large text" rules (3:1 contrast threshold).
- **Link** text must maintain `content.link` color (#0066f5) and `text-decoration: underline` to remain distinguishable for color-blind users without relying on color alone.
- **Responsive Display/Heading** types automatically scale larger on desktop, improving legibility — never set these as static pixel values.

---

## 7. Design Token CSS Custom Properties — Complete Reference

### Content Tokens

```css
:root {
  /* Base content */
  --tokens-colorScheme-content-base: #000000;
  --tokens-colorScheme-content-muted: rgba(0, 0, 0, 0.63);
  --tokens-colorScheme-content-faint: rgba(0, 0, 0, 0.48);
  --tokens-colorScheme-content-link: #0066f5;

  /* Utility */
  --tokens-colorScheme-content-utility-inverse: #ffffff;
  --tokens-colorScheme-content-utility-selected: #000000;
  --tokens-colorScheme-content-utility-unselected: rgba(0, 0, 0, 0.63);

  /* Brand */
  --tokens-colorScheme-content-brand-primary: #000000;
  --tokens-colorScheme-content-brand-secondary: #ffffff;
  --tokens-colorScheme-content-brand-tertiary: #ffffff;

  /* Role — base */
  --tokens-colorScheme-content-role-base-info: #0066f5;
  --tokens-colorScheme-content-role-base-negative: #c31526;
  --tokens-colorScheme-content-role-base-neutral: #000000;
  --tokens-colorScheme-content-role-base-positive: #008053;
  --tokens-colorScheme-content-role-base-special: #4422bf;
  --tokens-colorScheme-content-role-base-warning: #785000;

  /* Role — emphasis (always white) */
  --tokens-colorScheme-content-role-emphasis-info: #ffffff;
  --tokens-colorScheme-content-role-emphasis-negative: #ffffff;
  --tokens-colorScheme-content-role-emphasis-neutral: #ffffff;
  --tokens-colorScheme-content-role-emphasis-positive: #ffffff;
  --tokens-colorScheme-content-role-emphasis-special: #ffffff;
  --tokens-colorScheme-content-role-emphasis-warning: #ffffff;
}
```

### Background Tokens

```css
:root {
  /* Surfaces */
  --tokens-colorScheme-background-base: #ffffff;
  --tokens-colorScheme-background-muted: #f5f7fa;

  /* Utility */
  --tokens-colorScheme-background-utility-emphasis: #000000;
  --tokens-colorScheme-background-utility-selected: #000000;
  --tokens-colorScheme-background-utility-unselected: #999999;

  /* Brand */
  --tokens-colorScheme-background-brand-primary: #60cdff;
  --tokens-colorScheme-background-brand-secondary: #0066f5;
  --tokens-colorScheme-background-brand-tertiary: #002991;

  /* Container */
  --tokens-colorScheme-background-container-filled: rgba(5, 55, 130, 0.04);
  --tokens-colorScheme-background-container-outlined: #ffffff;

  /* Elevated */
  --tokens-colorScheme-background-elevated-modal: #ffffff;
  --tokens-colorScheme-background-elevated-popover: #ffffff;
  --tokens-colorScheme-background-elevated-scrim: rgba(0, 0, 0, 0.63);

  /* Overlay */
  --tokens-colorScheme-background-overlay-card: rgba(255, 255, 255, 0.63);
  --tokens-colorScheme-background-overlay-progressbar: rgba(0, 0, 0, 0.24);
  --tokens-colorScheme-background-overlay-ui: rgba(0, 0, 0, 0.63);

  /* Shimmer */
  --tokens-colorScheme-background-shimmer-start: rgba(255, 255, 255, 0.24);
  --tokens-colorScheme-background-shimmer-end: rgba(5, 55, 130, 0.04);

  /* Role — base */
  --tokens-colorScheme-background-role-base-info: #e3f7ff;
  --tokens-colorScheme-background-role-base-negative: #fbeaeb;
  --tokens-colorScheme-background-role-base-neutral: rgba(5, 55, 130, 0.08);
  --tokens-colorScheme-background-role-base-positive: #e6faee;
  --tokens-colorScheme-background-role-base-special: #efebff;
  --tokens-colorScheme-background-role-base-warning: #fff5e1;

  /* Role — emphasis */
  --tokens-colorScheme-background-role-emphasis-info: #0038ba;
  --tokens-colorScheme-background-role-emphasis-negative: #9f111f;
  --tokens-colorScheme-background-role-emphasis-neutral: #000000;
  --tokens-colorScheme-background-role-emphasis-positive: #006b51;
  --tokens-colorScheme-background-role-emphasis-special: #3514ae;
  --tokens-colorScheme-background-role-emphasis-warning: #785000;
}
```

### Border Tokens

```css
:root {
  /* Base borders */
  --tokens-colorScheme-border-base: rgba(0, 0, 0, 0.16);
  --tokens-colorScheme-border-muted: rgba(5, 55, 130, 0.08);
  --tokens-colorScheme-border-focus: #0066f5;

  /* Utility */
  --tokens-colorScheme-border-utility-emphasis: #000000;
  --tokens-colorScheme-border-utility-glow: rgba(0, 0, 0, 0);

  /* Container */
  --tokens-colorScheme-border-container-filled: rgba(255, 255, 255, 0);
  --tokens-colorScheme-border-container-outlined: rgba(0, 0, 0, 0.16);

  /* Role — base */
  --tokens-colorScheme-border-role-base-info: #60cdff;
  --tokens-colorScheme-border-role-base-negative: #f09c9f;
  --tokens-colorScheme-border-role-base-neutral: rgba(0, 0, 0, 0.16);
  --tokens-colorScheme-border-role-base-positive: #73e6ab;
  --tokens-colorScheme-border-role-base-special: #b5a0ff;
  --tokens-colorScheme-border-role-base-warning: #ffe9be;

  /* Role — emphasis */
  --tokens-colorScheme-border-role-emphasis-info: #0038ba;
  --tokens-colorScheme-border-role-emphasis-negative: #9f111f;
  --tokens-colorScheme-border-role-emphasis-neutral: #000000;
  --tokens-colorScheme-border-role-emphasis-positive: #006b51;
  --tokens-colorScheme-border-role-emphasis-special: #1d0088;
  --tokens-colorScheme-border-role-emphasis-warning: #aa7100;
}
```

### Typography Tokens

```css
:root {
  /* Font families */
  --tokens-brand-text-fontfamily-display: 'PayPal_Pro', sans-serif;
  --tokens-brand-text-fontfamily-heading: 'PayPal_Pro', sans-serif;
  --tokens-brand-text-fontfamily-title: 'Plain', sans-serif;
  --tokens-brand-text-fontfamily-label: 'Plain', sans-serif;
  --tokens-brand-text-fontfamily-body: 'Plain', sans-serif;
  --tokens-brand-text-fontfamily-link: 'Plain', sans-serif;

  /* Font weights */
  --tokens-brand-text-fontweight-display: 900;
  --tokens-brand-text-fontweight-heading: 900;
  --tokens-brand-text-fontweight-title: 500;
  --tokens-brand-text-fontweight-label: 500;
  --tokens-brand-text-fontweight-body: 400;
  --tokens-brand-text-fontweight-link: 500;

  /* Display (mobile) */
  --tokens-responsive-text-fontsize-display-lg: 76px;
  --tokens-responsive-text-lineheight-display-lg: 80px;
  --tokens-responsive-text-letterspacing-display-lg: -3px;
  --tokens-responsive-text-fontsize-display-md: 60px;
  --tokens-responsive-text-lineheight-display-md: 64px;
  --tokens-responsive-text-letterspacing-display-md: -3px;
  --tokens-responsive-text-fontsize-display-sm: 48px;
  --tokens-responsive-text-lineheight-display-sm: 48px;
  --tokens-responsive-text-letterspacing-display-sm: -3px;

  /* Heading (mobile) */
  --tokens-responsive-text-fontsize-heading-lg: 40px;
  --tokens-responsive-text-lineheight-heading-lg: 40px;
  --tokens-responsive-text-letterspacing-heading-lg: -1px;
  --tokens-responsive-text-fontsize-heading-md: 32px;
  --tokens-responsive-text-lineheight-heading-md: 32px;
  --tokens-responsive-text-letterspacing-heading-md: -1px;
  --tokens-responsive-text-fontsize-heading-sm: 24px;
  --tokens-responsive-text-lineheight-heading-sm: 32px;
  --tokens-responsive-text-letterspacing-heading-sm: -1px;

  /* Fixed scale (title / label / body / link) */
  --tokens-base-text-fontsize-ramp-20: 20px;
  --tokens-base-text-fontsize-ramp-16: 16px;
  --tokens-base-text-fontsize-ramp-14: 14px;
  --tokens-base-text-fontsize-ramp-12: 12px;
  --tokens-base-text-lineheight-ramp-24: 32px;
  --tokens-base-text-lineheight-ramp-16: 24px;
  --tokens-base-text-lineheight-ramp-14: 20px;
  --tokens-base-text-lineheight-ramp-12: 16px;
}
```

---

## 8. Platform APIs

### Android — Jetpack Compose

```kotlin
// Color tokens via MaterialTheme
object SemanticColors {
    val ContentBase = Color.Black
    val ContentMuted = Color(0xFF000000).copy(alpha = 0.63f)
    val ContentFaint = Color(0xFF000000).copy(alpha = 0.48f)
    val ContentLink = Color(0xFF0066F5)
    val ContentInverse = Color.White

    // Role colors
    val ContentInfo = Color(0xFF0066F5)
    val ContentNegative = Color(0xFFC31526)
    val ContentPositive = Color(0xFF008053)
    val ContentWarning = Color(0xFF785000)
    val ContentSpecial = Color(0xFF4422BF)

    val BackgroundBase = Color.White
    val BackgroundMuted = Color(0xFFF5F7FA)
    val BackgroundBrandPrimary = Color(0xFF60CDFF)
    val BackgroundBrandSecondary = Color(0xFF0066F5)
    val BackgroundBrandTertiary = Color(0xFF002991)

    val BorderBase = Color(0xFF000000).copy(alpha = 0.16f)
    val BorderFocus = Color(0xFF0066F5)
}

// Typography
val AppTypography = Typography(
    displayLarge = TextStyle(
        fontFamily = PayPalProFont,
        fontWeight = FontWeight.Black,
        fontSize = 76.sp,
        lineHeight = 80.sp,
        letterSpacing = (-3).sp
    ),
    headlineLarge = TextStyle(
        fontFamily = PayPalProFont,
        fontWeight = FontWeight.Black,
        fontSize = 40.sp,
        lineHeight = 40.sp,
        letterSpacing = (-1).sp
    ),
    titleLarge = TextStyle(
        fontFamily = PlainFont,
        fontWeight = FontWeight.Medium,
        fontSize = 20.sp,
        lineHeight = 32.sp,
        letterSpacing = 0.sp
    ),
    labelLarge = TextStyle(
        fontFamily = PlainFont,
        fontWeight = FontWeight.Medium,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.sp
    ),
    bodyLarge = TextStyle(
        fontFamily = PlainFont,
        fontWeight = FontWeight.Normal,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.sp
    )
)
```

### iOS — SwiftUI

```swift
// Color tokens
extension Color {
    // Content
    static let contentBase = Color.black
    static let contentMuted = Color.black.opacity(0.63)
    static let contentFaint = Color.black.opacity(0.48)
    static let contentLink = Color(red: 0, green: 0.40, blue: 0.96)
    static let contentInverse = Color.white

    // Role colors
    static let contentInfo = Color(red: 0, green: 0.40, blue: 0.96)
    static let contentNegative = Color(red: 0.76, green: 0.08, blue: 0.15)
    static let contentPositive = Color(red: 0, green: 0.50, blue: 0.33)
    static let contentWarning = Color(red: 0.47, green: 0.31, blue: 0)
    static let contentSpecial = Color(red: 0.27, green: 0.13, blue: 0.75)

    // Background
    static let backgroundBase = Color.white
    static let backgroundMuted = Color(red: 0.96, green: 0.97, blue: 0.98)
    static let backgroundBrandPrimary = Color(red: 0.38, green: 0.80, blue: 1.0)
    static let backgroundBrandSecondary = Color(red: 0, green: 0.40, blue: 0.96)

    // Border
    static let borderBase = Color.black.opacity(0.16)
    static let borderFocus = Color(red: 0, green: 0.40, blue: 0.96)
}

// Typography
extension Font {
    static let displayLg = Font.custom("PayPalPro-Black", size: 76)
    static let displayMd = Font.custom("PayPalPro-Black", size: 60)
    static let displaySm = Font.custom("PayPalPro-Black", size: 48)
    static let headingLg = Font.custom("PayPalPro-Black", size: 40)
    static let headingMd = Font.custom("PayPalPro-Black", size: 32)
    static let headingSm = Font.custom("PayPalPro-Black", size: 24)
    static let titleLg = Font.custom("Plain-Medium", size: 20)
    static let titleMd = Font.custom("Plain-Medium", size: 16)
    static let labelLg = Font.custom("Plain-Medium", size: 16)
    static let labelMd = Font.custom("Plain-Medium", size: 14)
    static let labelSm = Font.custom("Plain-Medium", size: 12)
    static let bodyLg = Font.custom("Plain-Regular", size: 16)
    static let bodyMd = Font.custom("Plain-Regular", size: 14)
    static let bodySm = Font.custom("Plain-Regular", size: 12)
    static let linkLg = Font.custom("Plain-Medium", size: 16)
    static let linkMd = Font.custom("Plain-Medium", size: 14)
    static let linkSm = Font.custom("Plain-Medium", size: 12)
}
```

---

## 9. Best Practices

### Color Token Best Practices

**Do use semantic tokens, not base primitives, for UI elements.** The semantic layer provides meaning and supports theming (dark mode, brand variants). Using a raw hex like `#000000` directly bypasses the token system and breaks in theme switches.

**Match content, background, and border tokens from the same role for semantic states.** For example, an error state should pair `content.role.base.negative` + `background.role.base.negative` + `border.role.base.negative` together — not mix roles.

**Use `content.muted` and `content.faint` for hierarchy, not different font sizes.** When body copy needs to be de-emphasized (e.g., metadata, helper text), apply the lighter content tokens rather than shrinking the font size below the minimum readable threshold.

**Reserve `background.utility.emphasis` (black) for high-contrast filled interactive elements** like the active Tabs pill or Switch track. Do not use it for decorative backgrounds.

**Never use `border.utility.glow` (transparent) visually** — it exists to enable animated glow-to-color border transitions via CSS `transition: border-color`.

### Typography Best Practices

**Never use Display or Heading styles at their smallest sizes for body content.** Display Small at 48px and Heading Small at 24px are structural titles, not reading text. Drop to Title Large (20px) for the largest non-headline body context.

**Match typography role to semantic HTML element.** `display` → `<h1>`, `heading` → `<h2>`/`<h3>`, `title` → `<h3>`/`<h4>`, `body` → `<p>`, `label` → `<span>`/`<label>`, `link` → `<a>`.

**Do not override the font feature settings `'liga' 0` on Display and Heading.** PayPal Pro's ligature behavior can affect layout at large sizes.

**For responsive Display and Heading text, always use the `tokens.responsive.*` token paths** rather than hard-coding mobile pixel values. The responsive tokens automatically resolve to the correct value at each breakpoint.

**Use Label for interactive controls; use Body for readable content.** While Label Large and Body Large share the same size (16px) and line-height (24px), they differ in weight (500 vs 400). Label's medium weight improves legibility in noisy UI contexts (buttons, tabs); Body's regular weight optimizes for extended reading.

---

## 10. Related Foundations

- **Foundations / Base** — Primitive tokens: spacing ramp, size ramp, border-radius, elevation shadows, motion tokens, base typography ramp, layout grid
- **Foundations / Semantic (this document)** — Semantic color + typography token layer
- **Color** — Brand palette and full color system (coming soon)
- **Dark Mode / Themes** — How `colorScheme` tokens resolve in alternate themes

---

## 11. Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial semantic token documentation from Components v6 Beta Figma file |

---

## 12. Testing Checklist

- [ ] All semantic content color tokens render correct hex/rgba values in browser dev tools
- [ ] `content.muted` and `content.faint` pass 4.5:1 contrast against `background.base`
- [ ] `border.focus` ring is visible at 2px width against all standard backgrounds
- [ ] All semantic role state pairings (base + background + border) render cohesively
- [ ] Display and Heading typography scale responsively across breakpoints (mobile → tablet → desktop)
- [ ] Title / Label / Body / Link remain at fixed sizes across all breakpoints
- [ ] Link styles include both `color: content.link` and `text-decoration: underline`
- [ ] Body and Label at size Small (12px) meet minimum 4.5:1 contrast ratio
- [ ] PayPal Pro font loaded correctly with `'liga' 0` feature setting applied to Display/Heading
- [ ] Plain font loaded correctly in Regular (400) and Medium (500) weights
- [ ] Dark mode / theme switch resolves `colorScheme` tokens to correct dark-mode values
- [ ] `background.shimmer.start` and `.end` tokens produce correct gradient sweep in Shimmer component
