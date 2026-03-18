# Logo Component Documentation

**Component:** Logo (PayPal.Monogram + PayPal.Logo)
**Figma Nodes:**
- `2840-12487` — PayPal.Monogram (the "P" mark)
- `2840-12500` — PayPal.Logo (the "PayPal" wordmark)
**iOS Code Connect:** `PDSButton.swift` (pds_core)
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

The Logo component library contains two distinct PayPal brand marks used throughout product experiences:

| Component | Description | Figma Node |
|---|---|---|
| **PayPal.Monogram** | The standalone "P" mark — the PayPal symbol without text | `2840-12487` |
| **PayPal.Logo** (Wordmark) | The "PayPal" text logotype | `2840-12500` |

Both assets are sourced from the **PayPal Assets CDN** (`paypalobjects.com`) and must never be recreated or modified. They are provided as optimized SVGs with precise sizing specifications for each usage context.

---

## 1. PayPal.Monogram (Mark)

The monogram is the standalone **"P" symbol** — used in compact contexts where the full wordmark would not fit, or where brand recognition is sufficient without the text (e.g., app icon backgrounds, avatar slots, loading screens).

### Color Variants

| Variant | Code Name | CDN Asset | Usage Context |
|---|---|---|---|
| `default` | `paypal-mark-color` | [`paypal-mark-color.svg`](https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-color.svg) | Light backgrounds; default brand expression |
| `inverse` | `paypal-mark-monotone` | [`paypal-mark-monotone.svg`](https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-monotone.svg) | Dark or brand-filled backgrounds; white/monotone rendering |

**Visual description:**
- `default` — The full-color PayPal "P" in a gradient of dark navy blue and sky blue/teal
- `inverse` — A flat white monotone "P" for rendering on dark or colored surfaces

### Monogram Sizing

The monogram is a square-proportioned symbol. Recommended sizes by context:

| Context | Recommended Height | Recommended Width |
|---|---|---|
| App icon / launch screen | 48px | 48px |
| Avatar / profile slot | 32px | 32px |
| Navigation context (inline) | 28px | 28px |
| Favicon / small badge | 16px | 16px |

> The monogram is not constrained to a fixed width in the same way as the wordmark — use proportional scaling at any size. Minimum rendered size is **16px** to preserve legibility of the "P" form.

---

## 2. PayPal.Logo (Wordmark)

The wordmark is the **"PayPal" text logotype** — the primary brand expression used in app headers, navigation bars, marketing surfaces, and any context where the full brand name should be visible.

### Color Variants

| Variant | Figma Name | Code Name | CDN Asset | Usage Context |
|---|---|---|---|---|
| `default` | Color=default | `paypal-wordmark-color` | [`paypal-wordmark-color.svg`](https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg) | White and light-grey backgrounds; standard product use |
| `alt` | Color=alt | *(RGB color)* | [`paypal-wordmark-color.svg`](https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg) | Brand-blue or tinted backgrounds; PayPal blue (#60cdff) surfaces |
| `inverse` | Color=inverse | `paypal-wordmark-monotone` | [`paypal-wordmark-monotone.svg`](https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-monotone.svg) | Dark backgrounds; navy, black, or dark-brand surfaces |

**Visual description:**
- `default` — "PayPal" in solid **black** (`#000000`); for white/light backgrounds
- `alt` — "PayPal" in **PayPal brand blue** (the signature two-tone gradient); for branded contexts
- `inverse` — "PayPal" in **white** (`#ffffff`); for dark or brand-fill backgrounds

### Wordmark Sizing

The wordmark has a fixed recommended height for each context. Width is always proportional (intrinsic, based on the SVG aspect ratio of approximately **2.82:1**).

| Context | Height | Width (approx.) | Token |
|---|---|---|---|
| **Navigation bar / header** (primary use) | **28px** | **~79px** | `tokens.base.size[28]` |
| Marketing / hero (large) | 40px | ~113px | `tokens.base.size[40]` |
| Footer / legal lock-up | 20px | ~56px | `tokens.base.size[20]` |
| Minimum (any context) | 16px | ~45px | `tokens.base.size[16]` |

> **Official sizing requirement (from Figma component description):** *"Set to 28px height when used in a nav or header in product experience."* The 28px height is the canonical nav/header size and must not be changed for in-product TopNav use.

### Wordmark Container Layout

In the Figma implementation, the wordmark SVG is inset within its container by a small margin on all sides:

```
Container: 28px tall × 79px wide
Image inset: top 4.17%, right 1.79%, bottom 4.17%, left 1.48%

Pixel values at 28px height:
  top inset:    ~1.2px
  bottom inset: ~1.2px
  left inset:   ~1.2px
  right inset:  ~1.4px
```

This inset is built into the SVG source files themselves — do not add additional padding around the logo.

---

## 3. Asset Reference

All logo assets are served from the official PayPal Assets CDN. These URLs are permanent and publicly accessible.

### PayPal.Monogram (Mark)

| Variant | SVG URL |
|---|---|
| Color (default) | `https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-color.svg` |
| Monotone / Inverse | `https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-monotone.svg` |

### PayPal.Logo (Wordmark)

| Variant | SVG URL |
|---|---|
| Color (default black) | `https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg` |
| Monotone / Inverse (white) | `https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-monotone.svg` |

> **Note:** There is no separate "alt" (blue) CDN file distinguished in the design system. The `alt` color variant may reference the same `paypal-wordmark-color.svg` source with a CSS `filter` override, or an alternate asset managed by the Brand team. Confirm the correct blue-variant asset with Brand/Design before implementing.

---

## 4. Props API

### PayPal.Monogram Props

```typescript
interface PayPalMonogramProps {
  /**
   * Color variant of the monogram.
   * - 'default': Full-color gradient P mark (for light backgrounds)
   * - 'inverse': Monotone white P mark (for dark backgrounds)
   * @default 'default'
   */
  color?: 'default' | 'inverse';

  /**
   * Height of the rendered mark in pixels.
   * Width scales proportionally.
   * Minimum recommended: 16px.
   * @default 28
   */
  size?: number;

  /**
   * Accessible alt text. Defaults to "PayPal".
   * Pass empty string ("") when the mark is decorative and a sibling
   * element already identifies PayPal (e.g., visible wordmark nearby).
   * @default 'PayPal'
   */
  alt?: string;

  className?: string;
}
```

### PayPal.Logo (Wordmark) Props

```typescript
interface PayPalLogoProps {
  /**
   * Color variant of the wordmark.
   * - 'default': Black wordmark (for white/light backgrounds)
   * - 'alt': Color/blue wordmark (for brand-tinted backgrounds)
   * - 'inverse': White wordmark (for dark backgrounds)
   * @default 'default'
   */
  color?: 'default' | 'alt' | 'inverse';

  /**
   * Height of the wordmark in pixels. Width scales proportionally (~2.82:1 ratio).
   * Use 28px for nav/header. Minimum: 16px.
   * @default 28
   */
  height?: number;

  /**
   * Accessible alt text. Defaults to "PayPal".
   * Pass empty string ("") only when a visible sibling already reads "PayPal"
   * and the image is purely decorative.
   * @default 'PayPal'
   */
  alt?: string;

  /**
   * Optional href to wrap the logo in a link (e.g., to navigate home).
   * When provided, renders as <a href={href}> with accessible label.
   */
  href?: string;

  className?: string;
}
```

---

## 5. Code Examples

### PayPal.Monogram

```tsx
import { PayPalMonogram } from '@/components/Logo';

// Default color (light backgrounds)
<PayPalMonogram color="default" size={28} alt="PayPal" />

// Inverse (dark backgrounds)
<PayPalMonogram color="inverse" size={28} alt="PayPal" />

// Decorative (wordmark is nearby — no need to repeat the alt text)
<PayPalMonogram color="default" size={28} alt="" aria-hidden="true" />
```

### PayPal.Logo (Wordmark)

```tsx
import { PayPalLogo } from '@/components/Logo';

// Default black — for white/light backgrounds (nav bar use)
<PayPalLogo color="default" height={28} alt="PayPal" />

// Alt color — for brand-tinted surfaces
<PayPalLogo color="alt" height={28} alt="PayPal" />

// Inverse white — for dark/navy backgrounds
<PayPalLogo color="inverse" height={28} alt="PayPal" />

// Linked wordmark (home navigation)
<PayPalLogo color="default" height={28} alt="PayPal" href="/" />
```

### Component Implementation

```tsx
import React from 'react';

const MONOGRAM_URLS = {
  default: 'https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-color.svg',
  inverse: 'https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-mark-monotone.svg',
} as const;

const WORDMARK_URLS = {
  default: 'https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg',
  alt:     'https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg',
  inverse: 'https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-monotone.svg',
} as const;

// PayPal.Monogram
export const PayPalMonogram: React.FC<PayPalMonogramProps> = ({
  color = 'default',
  size = 28,
  alt = 'PayPal',
  className,
}) => (
  <img
    src={MONOGRAM_URLS[color]}
    alt={alt}
    height={size}
    width={size}
    className={className}
    style={{ display: 'block', flexShrink: 0 }}
    draggable={false}
  />
);

// PayPal.Logo (Wordmark)
export const PayPalLogo: React.FC<PayPalLogoProps> = ({
  color = 'default',
  height = 28,
  alt = 'PayPal',
  href,
  className,
}) => {
  // Wordmark aspect ratio: ~79:28 ≈ 2.821:1
  const width = Math.round(height * (79 / 28));

  const img = (
    <img
      src={WORDMARK_URLS[color]}
      alt={href ? '' : alt}  // alt is on the wrapping <a> when linked
      height={height}
      width={width}
      className={className}
      style={{ display: 'block', height: `${height}px`, width: 'auto', flexShrink: 0 }}
      draggable={false}
    />
  );

  if (href) {
    return (
      <a href={href} aria-label={alt} style={{ display: 'inline-flex', alignItems: 'center' }}>
        {img}
      </a>
    );
  }

  return img;
};
```

### CSS (sizing helpers)

```css
/* Standard nav/header wordmark */
.paypal-logo--nav {
  height: var(--tokens-base-size-28, 28px);
  width: auto; /* intrinsic ~79px */
  display: block;
  flex-shrink: 0;
}

/* Marketing / hero wordmark */
.paypal-logo--hero {
  height: var(--tokens-base-size-40, 40px);
  width: auto; /* intrinsic ~113px */
  display: block;
}

/* Footer wordmark */
.paypal-logo--footer {
  height: var(--tokens-base-size-20, 20px);
  width: auto; /* intrinsic ~56px */
  display: block;
}
```

---

## 6. Color Variant Selection Guide

### Wordmark Variant by Background

| Background color | Wordmark variant | Monogram variant |
|---|---|---|
| `background.base` — white (`#fff`) | `default` (black) | `default` (color) |
| `background.muted` — off-white (`#f5f7fa`) | `default` (black) | `default` (color) |
| `background.brand.primary` — sky blue (`#60cdff`) | `alt` (brand blue) | `default` (color) |
| `background.brand.secondary` — PayPal blue (`#0066f5`) | `inverse` (white) | `inverse` (white) |
| `background.brand.tertiary` — navy (`#002991`) | `inverse` (white) | `inverse` (white) |
| `background.utility.emphasis` — black (`#000`) | `inverse` (white) | `inverse` (white) |
| Dark photography / dark backgrounds | `inverse` (white) | `inverse` (white) |

### Decision Flowchart

```
Is the background dark (navy, black, dark brand)?
  └─ YES → use inverse (white) variant
  └─ NO: Is the background a brand-blue (#60cdff / #0066f5)?
       └─ #0066f5 or darker → use inverse (white)
       └─ #60cdff (sky blue) → use alt (brand color) or default (black)
       └─ NO (white, grey, light) → use default (black)
```

---

## 7. Accessibility Specifications

### Alt Text Rules

The logo images must always have meaningful alt text unless they are purely decorative:

```tsx
// ✅ Logo is the only identifier for PayPal on this surface
<PayPalLogo alt="PayPal" />

// ✅ Linked logo — alt text on the <a>, not the <img>
<a href="/" aria-label="PayPal — Go to home">
  <img src={wordmarkUrl} alt="" />
</a>

// ✅ Logo appears alongside the text "PayPal" — mark is decorative
<img src={monogramUrl} alt="" aria-hidden="true" />
<span>PayPal</span>

// ❌ Missing alt — screen reader reads filename
<img src={wordmarkUrl} />

// ❌ Redundant alt when text is already visible
<img src={wordmarkUrl} alt="PayPal logo" />
<span>PayPal</span>
```

### Linked Logo

When the logo is a home/navigation link:

```tsx
// Recommended: aria-label on the <a>, empty alt on <img>
<a href="/" aria-label="PayPal">
  <img
    src="https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg"
    alt=""
    height="28"
    width="79"
  />
</a>
```

### Color Contrast

| Variant | Logo color | Minimum background | Contrast |
|---|---|---|---|
| `default` (black) | `#000000` | `#ffffff` | 21:1 ✅ |
| `inverse` (white) | `#ffffff` | `#002991` (navy) | ~12.6:1 ✅ |
| `alt` (blue) | `#0066f5` (approx) | `#ffffff` | ~4.6:1 ✅ |

### Focus Ring on Linked Logo

When the logo is rendered as a link, it must show a visible focus indicator:

```css
.paypal-logo-link:focus-visible {
  outline: 2px solid var(--tokens-colorScheme-border-focus, #0066f5);
  outline-offset: 4px;
  border-radius: 4px;
}
```

---

## 8. Platform APIs

### Android — Jetpack Compose

```kotlin
import androidx.compose.foundation.Image
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.semantics.contentDescription
import androidx.compose.ui.semantics.semantics

// Wordmark (default — for light backgrounds)
@Composable
fun PayPalWordmark(
    colorVariant: WordmarkColor = WordmarkColor.Default,
    heightDp: Dp = 28.dp,
    modifier: Modifier = Modifier
) {
    val painter = when (colorVariant) {
        WordmarkColor.Default -> painterResource(R.drawable.paypal_logo_rgb_black)
        WordmarkColor.Alt     -> painterResource(R.drawable.paypal_logo_rgb)
        WordmarkColor.Inverse -> painterResource(R.drawable.paypal_logo_rgb_white)
    }

    // Aspect ratio: 79:28
    val widthDp = heightDp * (79f / 28f)

    Image(
        painter = painter,
        contentDescription = "PayPal",
        modifier = modifier
            .height(heightDp)
            .width(widthDp)
    )
}

enum class WordmarkColor { Default, Alt, Inverse }

// Monogram
@Composable
fun PayPalMonogram(
    colorVariant: MonogramColor = MonogramColor.Default,
    sizeDp: Dp = 28.dp,
    modifier: Modifier = Modifier
) {
    val painter = when (colorVariant) {
        MonogramColor.Default -> painterResource(R.drawable.paypal_mark_color)
        MonogramColor.Inverse -> painterResource(R.drawable.paypal_mark_monotone)
    }

    Image(
        painter = painter,
        contentDescription = "PayPal",
        modifier = modifier.size(sizeDp)
    )
}

enum class MonogramColor { Default, Inverse }
```

### iOS — SwiftUI

The Figma Code Connect points to `PDSButton.swift`, indicating these logos are exposed as branded asset components in the `pds_core` module:

```swift
import SwiftUI

// Wordmark
struct PayPalWordmark: View {
    enum ColorVariant { case `default`, alt, inverse }

    var colorVariant: ColorVariant = .default
    var height: CGFloat = 28

    private var imageName: String {
        switch colorVariant {
        case .default: return "paypal-wordmark-color"    // black
        case .alt:     return "paypal-wordmark-rgb"      // brand blue
        case .inverse: return "paypal-wordmark-monotone" // white
        }
    }

    // Aspect ratio: ~79:28 ≈ 2.821
    private var width: CGFloat { height * (79.0 / 28.0) }

    var body: some View {
        Image(imageName)
            .resizable()
            .scaledToFit()
            .frame(height: height, width: width)
            .accessibilityLabel("PayPal")
            .accessibilityAddTraits(.isImage)
    }
}

// Monogram (Mark)
struct PayPalMonogram: View {
    enum ColorVariant { case `default`, inverse }

    var colorVariant: ColorVariant = .default
    var size: CGFloat = 28

    private var imageName: String {
        switch colorVariant {
        case .default: return "paypal-mark-color"
        case .inverse: return "paypal-mark-monotone"
        }
    }

    var body: some View {
        Image(imageName)
            .resizable()
            .scaledToFit()
            .frame(width: size, height: size)
            .accessibilityLabel("PayPal")
    }
}

// Usage examples
struct ExamplesView: View {
    var body: some View {
        VStack {
            // Nav bar wordmark
            PayPalWordmark(colorVariant: .default, height: 28)

            // On dark background
            PayPalWordmark(colorVariant: .inverse, height: 28)
                .padding()
                .background(Color(red: 0, green: 0.16, blue: 0.57))

            // Monogram
            PayPalMonogram(colorVariant: .default, size: 32)
        }
    }
}
```

---

## 9. Design Tokens Reference

| Token | Value | Usage |
|---|---|---|
| `tokens.base.size[28]` | `28px` | **Standard nav/header wordmark height** |
| `tokens.base.size[40]` | `40px` | Marketing / hero wordmark height |
| `tokens.base.size[20]` | `20px` | Footer wordmark height |
| `tokens.base.size[16]` | `16px` | Minimum wordmark/mark height |
| `tokens.colorScheme.background.base` | `#ffffff` | Background requiring `default` (black) variant |
| `tokens.colorScheme.background.brand.tertiary` | `#002991` | Background requiring `inverse` (white) variant |
| `tokens.colorScheme.content.base` | `#000000` | Color of `default` wordmark text |
| `tokens.colorScheme.content.utility.inverse` | `#ffffff` | Color of `inverse` wordmark text |

---

## 10. Best Practices

**Always use the CDN-hosted SVG files.** The PayPal logo assets are served from `paypalobjects.com` with appropriate caching, versioning, and format optimization. Never inline the SVG paths, rasterize the logo to PNG for digital products, or recreate the letterforms manually.

**Never modify the logo.** The aspect ratio must be preserved at all times. Do not stretch, squash, rotate, recolor, add shadows, apply opacity, crop, or otherwise alter the mark or wordmark. The only permitted variation is selecting the appropriate color variant (`default`, `alt`, or `inverse`) for the background.

**Use the `inverse` variant on any background that reduces contrast below 3:1.** If a background color makes the black or blue wordmark hard to read, switch to the white `inverse` variant. This applies to brand blues (`#0066f5`, `#002991`), dark images, and any background darker than approximately `#767676`.

**Always set an explicit `height` and let `width` be `auto`.** SVGs scale to their intrinsic aspect ratio when only one dimension is constrained. Setting both `height` and `width` explicitly can cause letterform distortion if the aspect ratio drifts. Use `height: 28px; width: auto` for nav bar usage.

**The monogram is not a replacement for the wordmark.** The "P" mark should only appear alone when space constraints make the full wordmark impractical (e.g., app icons, favicons, small avatar slots). On any surface where there is room, prefer the full wordmark to reinforce brand recognition.

**Add 16px of clear space around the logo on all sides.** The brand identity guidelines require a minimum clear space equal to the height of the "P" letterform (approximately half the wordmark height). At 28px, this means ~14–16px on all sides. Do not crowd the logo with text, other logos, or interface elements.

**Do not use the logo as a button label.** The wordmark or monogram must not replace button text (e.g., a "Pay with PayPal" button should use the wordmark as a prefix but retain the full text label). Using a logo image as the sole interactive label is inaccessible to screen readers and violates brand guidelines.

---

## 11. Related Components

- **Top Navigation** — Primary consumer of `PayPal.Logo` (28px wordmark, `color="default"`) in the left slot
- **WebView.TopNav** — The same `TopNavWordmark` sub-component (79px × 28px, left-aligned at 16px from edge)
- **Button** — PayPal-branded payment buttons may include the monogram alongside action text
- **Foundations / Semantic** — `background.brand.*` tokens define surfaces requiring `alt` and `inverse` logo variants

---

## 12. Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial Logo documentation; Monogram (2 variants) + Wordmark (3 variants) |

---

## 13. Testing Checklist

**PayPal.Monogram**
- [ ] `color="default"` renders `paypal-mark-color.svg` (gradient blue "P")
- [ ] `color="inverse"` renders `paypal-mark-monotone.svg` (white "P")
- [ ] Size prop controls `height` and `width` equally (square proportions)
- [ ] `alt="PayPal"` is set by default
- [ ] `alt=""` with `aria-hidden="true"` when used alongside a visible wordmark
- [ ] Minimum rendered size 16×16px — mark remains recognizable
- [ ] No CSS filter or recolor applied to the SVG asset

**PayPal.Logo (Wordmark)**
- [ ] `color="default"` renders black wordmark (`paypal-wordmark-color.svg`)
- [ ] `color="alt"` renders brand-color (blue) wordmark
- [ ] `color="inverse"` renders white wordmark (`paypal-wordmark-monotone.svg`)
- [ ] Default height is 28px per Figma spec ("Set to 28px in nav or header")
- [ ] Width scales intrinsically (`width: auto`) — not hard-coded
- [ ] Aspect ratio preserved at all rendered sizes (~2.82:1)
- [ ] `alt="PayPal"` on standalone image
- [ ] When `href` provided: `<a aria-label="PayPal">` wraps `<img alt="">`
- [ ] Focus ring visible on linked wordmark: 2px `border.focus` (#0066f5), 4px offset
- [ ] Logo is not draggable (`draggable="false"`)
- [ ] SVG served from `paypalobjects.com` CDN (not bundled inline)
- [ ] Clear space of ~16px maintained around logo in all compositions
- [ ] Wordmark not used as sole interactive label on a button
