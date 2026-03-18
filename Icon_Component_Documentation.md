# Icon Component Documentation

## Overview

The Icon component provides a consistent, accessible, and scalable interface for rendering SVG icons throughout the design system. Icons are visual shorthand — they communicate meaning faster than words when used appropriately, reduce cognitive load by associating familiar symbols with actions or states, and add visual rhythm to interfaces. The Icon component standardizes sizing, color inheritance, accessibility attributes, and rendering behavior across all icon usage in the application. It wraps the underlying SVG with a predictable API and enforces design token alignment, ensuring icons are always legible, properly scaled, and never accidentally lose their semantic meaning.

---

## Icon Library

The design system ships with a curated icon set organized into semantic categories. Icons follow a consistent visual style — uniform stroke width, optical sizing, and a standardized grid.

### **Icon Style Principles**
- **Grid**: 24×24px base grid (scalable)
- **Stroke width**: 1.5px (default), 2px (bold variant)
- **Corner radius**: Consistent rounded corners throughout
- **Visual style**: Outlined (default), Filled (active/selected states)
- **Optical balance**: Icons are optically adjusted within the grid for perceived equal size
- **Pixel-hinting**: Crisp at 16px, 20px, 24px (the three most common sizes)

### **Icon Categories**

**Navigation**
- `home`, `arrow-left`, `arrow-right`, `arrow-up`, `arrow-down`
- `chevron-left`, `chevron-right`, `chevron-up`, `chevron-down`
- `menu`, `sidebar`, `panel-left`, `panel-right`, `grid`, `list`

**Actions**
- `plus`, `minus`, `x`, `check`, `copy`, `edit`, `trash`, `move`
- `upload`, `download`, `share`, `send`, `refresh`, `rotate`
- `search`, `filter`, `sort-asc`, `sort-desc`, `expand`, `collapse`

**Communication**
- `bell`, `bell-off`, `mail`, `mail-open`, `message`, `message-circle`
- `phone`, `video`, `mic`, `mic-off`, `volume`, `volume-off`

**Media**
- `play`, `pause`, `stop`, `skip-forward`, `skip-back`
- `image`, `film`, `music`, `file`, `folder`, `folder-open`

**Status & Feedback**
- `check-circle`, `x-circle`, `alert-triangle`, `alert-circle`, `info`
- `clock`, `calendar`, `flag`, `bookmark`, `star`, `heart`

**Users & People**
- `user`, `users`, `user-plus`, `user-minus`, `user-check`
- `shield`, `lock`, `unlock`, `key`, `eye`, `eye-off`

**Data & Analytics**
- `bar-chart`, `bar-chart-2`, `pie-chart`, `trending-up`, `trending-down`
- `database`, `server`, `cloud`, `cloud-off`, `wifi`, `wifi-off`

**Commerce**
- `shopping-cart`, `shopping-bag`, `credit-card`, `dollar-sign`, `tag`, `gift`

**Settings & Tools**
- `settings`, `sliders`, `tool`, `code`, `terminal`, `cpu`
- `layers`, `layout`, `maximize`, `minimize`, `link`, `link-2`, `external-link`

**Misc**
- `globe`, `map`, `map-pin`, `compass`, `zap`, `sun`, `moon`, `thermometer`

---

## Size Variants

| Size | px Value | Use Case |
|------|----------|---------|
| **2XS** | 12px | Metadata labels, dense table indicators |
| **XS** | 14px | Caption-level icons, tight inline contexts |
| **SM** | 16px | Form inputs, buttons, badges, list items |
| **MD** | 20px | **Default**, nav items, general UI icons |
| **LG** | 24px | Page headers, feature icons, callouts |
| **XL** | 32px | Section headers, empty state icons |
| **2XL** | 40px | Card illustrations, feature highlights |
| **3XL** | 48px | Hero icons, illustration anchors |
| **4XL** | 64px | Full empty states, onboarding spots |

---

## Style Variants

### **Outlined** (Default)
- SVG rendered as stroked paths; no fill
- Stroke width: 1.5px
- Best For: General UI use, nav items, inactive states, secondary information
- Visual weight: Light to medium

### **Filled**
- SVG rendered with filled paths
- Best For: Active/selected states, primary actions, featured icons, mobile navigation active items
- Visual weight: Medium to heavy
- Common usage: Active nav items, selected state indicators, CTA icons

### **Bold**
- Outlined variant with increased stroke weight (2–2.5px)
- Best For: High-contrast scenarios, small sizes where thin strokes are hard to read, emphasis
- Visual weight: Heavy

### **Duotone**
- Two-tone icon: Primary color on main shape + lighter tint on secondary shape
- Best For: Feature illustrations, empty state icons, decorative usage, marketing
- Visual weight: Medium; adds visual depth
- Color: Primary color (foreground) + 20–30% opacity tint (background element)

### **Gradient**
- SVG with a gradient fill applied across paths
- Best For: Premium features, marketing, brand-forward contexts
- Visual weight: Decorative

---

## Color Behavior

### **Inheriting Color (Default)**
By default, Icon inherits `currentColor` from its parent element. This means:
- Icon inside a red button → icon is red
- Icon inside a disabled field → icon is gray
- Icon next to blue text → set parent `color: blue` and icon follows

```css
/* SVG uses currentColor */
svg { color: inherit; }
path { stroke: currentColor; } /* outlined */
path { fill: currentColor; }   /* filled */
```

### **Explicit Color**
Color can be set directly via the `color` prop using:
- Design token names: `"primary"`, `"success"`, `"danger"`, `"warning"`, `"info"`, `"muted"`
- Hex values: `"#3B82F6"`
- CSS variables: `"var(--color-primary)"`
- Tailwind classes: via `className`

### **Semantic Color Mapping**
| Token | Value | Use Case |
|-------|-------|---------|
| `primary` | #3B82F6 | Brand actions, active states |
| `secondary` | #6B7280 | Secondary/muted icons |
| `success` | #10B981 | Positive status icons |
| `warning` | #F59E0B | Caution icons |
| `danger` | #EF4444 | Error, delete, critical icons |
| `info` | #06B6D4 | Informational icons |
| `muted` | #9CA3AF | Placeholder, disabled |
| `inverse` | #FFFFFF | Icons on dark backgrounds |

---

## Props API

```typescript
interface IconProps {
  // Icon Selection
  name: string;                                // Icon name from the icon library
  // OR pass SVG directly:
  children?: React.ReactNode;                  // Custom SVG content (advanced use)

  // Sizing
  size?: '2xs' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl' | '3xl' | '4xl' | number;
  width?: number | string;                     // Override width independently
  height?: number | string;                    // Override height independently

  // Appearance
  variant?: 'outlined' | 'filled' | 'bold' | 'duotone' | 'gradient';
  color?: string;                              // Color token, hex, or CSS var
  strokeWidth?: number;                        // Override stroke width

  // Accessibility
  'aria-label'?: string;                       // Makes icon meaningful to screen readers
  'aria-hidden'?: boolean | 'true' | 'false'; // Default: true (decorative)
  title?: string;                              // SVG <title> for tooltip / SR fallback
  role?: 'img' | 'presentation';              // SVG role attribute

  // Animation
  spin?: boolean;                              // Continuous rotation (loading spinners)
  pulse?: boolean;                             // Pulsing opacity (loading state)
  bounce?: boolean;                            // Bounce animation
  animationDuration?: string;                  // CSS duration string (default: '1s')

  // Styling
  className?: string;
  style?: React.CSSProperties;

  // Events
  onClick?: (event: React.MouseEvent<SVGSVGElement>) => void;
}
```

---

## Code Examples

### **1. Basic Icon Usage**
```typescript
import { Icon } from '@components/icon';

export function BasicIcons() {
  return (
    <div className="flex items-center gap-4">
      {/* Decorative icon (most common) */}
      <Icon name="home" size="md" aria-hidden="true" />

      {/* Semantic icon with label */}
      <Icon name="alert-triangle" size="md" color="warning" aria-label="Warning" />

      {/* Custom size */}
      <Icon name="star" size={20} aria-hidden="true" />
    </div>
  );
}
```

### **2. Icon with Button (Paired)**
```typescript
import { Icon } from '@components/icon';
import { Button } from '@components/button';

export function IconButton() {
  return (
    <div className="flex gap-2">
      {/* Icon paired with visible text — icon is decorative */}
      <Button color="primary">
        <Icon name="plus" size="sm" aria-hidden="true" />
        Add item
      </Button>

      {/* Icon-only button — icon needs label on the button */}
      <button aria-label="Delete item">
        <Icon name="trash" size="md" color="danger" aria-hidden="true" />
      </button>
    </div>
  );
}
```

### **3. Semantic Status Icons**
```typescript
import { Icon } from '@components/icon';

type Status = 'success' | 'warning' | 'error' | 'info';

const statusConfig: Record<Status, { name: string; color: string; label: string }> = {
  success: { name: 'check-circle', color: 'success', label: 'Success' },
  warning: { name: 'alert-triangle', color: 'warning', label: 'Warning' },
  error:   { name: 'x-circle',       color: 'danger',  label: 'Error'   },
  info:    { name: 'info',            color: 'info',    label: 'Info'    },
};

export function StatusIcon({ status }: { status: Status }) {
  const { name, color, label } = statusConfig[status];
  return (
    <Icon
      name={name}
      size="md"
      color={color}
      aria-label={label}
      role="img"
    />
  );
}
```

### **4. Filled vs Outlined (Active State Toggle)**
```typescript
import { Icon } from '@components/icon';
import { useState } from 'react';

export function BookmarkToggle() {
  const [saved, setSaved] = useState(false);

  return (
    <button
      onClick={() => setSaved(!saved)}
      aria-label={saved ? 'Remove bookmark' : 'Add bookmark'}
      aria-pressed={saved}
    >
      <Icon
        name="bookmark"
        size="md"
        variant={saved ? 'filled' : 'outlined'}
        color={saved ? 'primary' : 'secondary'}
        aria-hidden="true"
      />
    </button>
  );
}
```

### **5. Spinning Loader Icon**
```typescript
import { Icon } from '@components/icon';

export function LoadingSpinner({ size = 'md' }: { size?: string }) {
  return (
    <Icon
      name="refresh"
      size={size}
      spin
      animationDuration="0.8s"
      color="primary"
      aria-label="Loading"
      role="img"
    />
  );
}
```

### **6. Icon with Background Container**
```typescript
import { Icon } from '@components/icon';

const featureIcons = [
  { name: 'zap', color: 'warning', bg: '#FFFBEB', label: 'Fast performance' },
  { name: 'shield', color: 'success', bg: '#ECFDF5', label: 'Secure by default' },
  { name: 'globe', color: 'info', bg: '#EFF6FF', label: 'Available anywhere' },
];

export function FeatureIcons() {
  return (
    <div className="flex gap-6">
      {featureIcons.map((feat) => (
        <div key={feat.name} className="flex flex-col items-center gap-2">
          <div
            className="flex items-center justify-center w-12 h-12 rounded-xl"
            style={{ backgroundColor: feat.bg }}
          >
            <Icon
              name={feat.name}
              size="lg"
              color={feat.color}
              aria-hidden="true"
            />
          </div>
          <span className="text-sm">{feat.label}</span>
        </div>
      ))}
    </div>
  );
}
```

### **7. Duotone Icon for Empty State**
```typescript
import { Icon } from '@components/icon';

export function EmptyInbox() {
  return (
    <div className="flex flex-col items-center gap-3">
      <Icon
        name="inbox"
        size="4xl"
        variant="duotone"
        color="primary"
        aria-hidden="true"
      />
      <h3 className="text-lg font-semibold">Your inbox is empty</h3>
      <p className="text-sm text-gray-500">Messages will appear here.</p>
    </div>
  );
}
```

### **8. Icon in Form Field**
```typescript
import { Icon } from '@components/icon';

export function SearchInput() {
  return (
    <div className="relative">
      <div className="absolute inset-y-0 left-3 flex items-center pointer-events-none">
        <Icon name="search" size="sm" color="muted" aria-hidden="true" />
      </div>
      <input
        type="search"
        placeholder="Search…"
        className="pl-9 pr-4 py-2 border rounded-lg w-full"
        aria-label="Search"
      />
    </div>
  );
}
```

### **9. Icon Size Scale Reference**
```typescript
import { Icon } from '@components/icon';

const sizes = ['2xs', 'xs', 'sm', 'md', 'lg', 'xl', '2xl', '3xl', '4xl'] as const;

export function SizeReference() {
  return (
    <div className="flex items-end gap-4">
      {sizes.map((size) => (
        <div key={size} className="flex flex-col items-center gap-1">
          <Icon name="star" size={size} aria-hidden="true" />
          <span className="text-xs text-gray-400">{size}</span>
        </div>
      ))}
    </div>
  );
}
```

### **10. Custom SVG via Children**
```typescript
import { Icon } from '@components/icon';

// For bespoke or brand icons not in the standard library
export function CustomBrandIcon() {
  return (
    <Icon size="md" aria-label="Brand Logo">
      <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path
          d="M12 2L2 7l10 5 10-5-10-5zM2 17l10 5 10-5M2 12l10 5 10-5"
          stroke="currentColor"
          strokeWidth="1.5"
          strokeLinecap="round"
          strokeLinejoin="round"
        />
      </svg>
    </Icon>
  );
}
```

---

## Accessibility Specifications

### **The Core Rule: Decorative vs. Semantic**
Every icon falls into one of two categories — and the right ARIA treatment depends entirely on which it is:

| Type | Description | ARIA Treatment |
|------|-------------|---------------|
| **Decorative** | Supports adjacent text; the text already conveys the meaning | `aria-hidden="true"` |
| **Semantic** | Conveys meaning on its own, without adjacent text | `aria-label="[meaning]"` + `role="img"` |

**Default behavior**: `aria-hidden="true"` — icons are decorative by default. You must explicitly opt into semantic treatment.

### **Decorative Icons (Most Common)**
```tsx
// ✅ Correct: icon supports visible text, so it's decorative
<button>
  <Icon name="download" size="sm" aria-hidden="true" />
  Download
</button>

// ✅ Correct: icon in nav with visible label
<nav>
  <a href="/settings">
    <Icon name="settings" aria-hidden="true" />
    Settings
  </a>
</nav>
```

### **Semantic Icons (Standalone Meaning)**
```tsx
// ✅ Correct: icon-only button — meaning must be on the button, not the icon
<button aria-label="Close dialog">
  <Icon name="x" aria-hidden="true" />
</button>

// ✅ Correct: status icon conveying state — no adjacent text
<Icon name="check-circle" color="success" aria-label="Verified" role="img" />

// ✅ Correct: standalone warning icon in a table cell
<Icon name="alert-triangle" color="warning" aria-label="Attention required" role="img" />
```

### **SVG Title Element**
For icons that may appear in non-React contexts or need browser tooltip behavior:
```tsx
// Adds <title> inside the SVG — read by some screen readers as fallback
<Icon name="info" title="More information" />
// Renders: <svg><title>More information</title>...</svg>
```

### **Interactive Icons**
- Never make an SVG the interactive element — wrap in `<button>` or `<a>`
- The button/link carries the `aria-label`, not the icon
- `aria-pressed` for toggle icons (bookmark, favorite, mute)
- `aria-expanded` on the trigger when icon controls a collapsible

### **Color Contrast for Icons**
- UI icons (interactive or informational): 3:1 minimum contrast against background (WCAG 1.4.11)
- Text-adjacent icons: Should match or exceed the text contrast (4.5:1 recommended)
- Never use color alone to convey state — pair with label text or shape change

### **Animated Icons**
- Spinning loaders: Provide `aria-label="Loading"` or `role="status"` with live region
- `prefers-reduced-motion`: Disable spin, pulse, and bounce animations
  ```css
  @media (prefers-reduced-motion: reduce) {
    .icon-spin, .icon-pulse, .icon-bounce { animation: none; }
  }
  ```

### **Screen Reader Behavior by Pattern**
| Pattern | Screen Reader Output |
|---------|---------------------|
| `<Icon aria-hidden="true" />` | (silent) |
| `<Icon aria-label="Delete" role="img" />` | "Delete, image" |
| `<button aria-label="Delete"><Icon aria-hidden /></button>` | "Delete, button" |
| `<Icon name="check-circle" aria-label="Success" />` | "Success, image" |
| Spinning loader with `aria-label` | "Loading, image" |

---

## Animation Specifications

### **Spin** (`spin` prop)
- **Use case**: Loading states, refresh indicators
- **Duration**: 0.8–1s (configurable via `animationDuration`)
- **Easing**: linear
- **Repeat**: Infinite
  ```css
  @keyframes iconSpin {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
  }
  .icon-spin { animation: iconSpin 1s linear infinite; }
  ```

### **Pulse** (`pulse` prop)
- **Use case**: Soft loading or attention indicators
- **Duration**: 1.5s
- **Easing**: ease-in-out
- **Repeat**: Infinite
  ```css
  @keyframes iconPulse {
    0%, 100% { opacity: 1; }
    50%       { opacity: 0.4; }
  }
  .icon-pulse { animation: iconPulse 1.5s ease-in-out infinite; }
  ```

### **Bounce** (`bounce` prop)
- **Use case**: Notification draw attention, celebratory
- **Duration**: 0.6s
- **Easing**: cubic-bezier(0.36, 0.07, 0.19, 0.97)
- **Repeat**: 3 times (or infinite)
  ```css
  @keyframes iconBounce {
    0%, 100% { transform: translateY(0); }
    40%       { transform: translateY(-6px); }
    60%       { transform: translateY(-3px); }
  }
  .icon-bounce { animation: iconBounce 0.6s cubic-bezier(0.36, 0.07, 0.19, 0.97) 3; }
  ```

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  [class*="icon-spin"],
  [class*="icon-pulse"],
  [class*="icon-bounce"] {
    animation: none !important;
  }
}
```

---

## Design Tokens

### **Size Scale**
| Token | px |
|-------|----|
| `--icon-size-2xs` | 12px |
| `--icon-size-xs` | 14px |
| `--icon-size-sm` | 16px |
| `--icon-size-md` | 20px |
| `--icon-size-lg` | 24px |
| `--icon-size-xl` | 32px |
| `--icon-size-2xl` | 40px |
| `--icon-size-3xl` | 48px |
| `--icon-size-4xl` | 64px |

### **Stroke Width**
| Variant | Value |
|---------|-------|
| Outlined (default) | 1.5px |
| Bold | 2px |
| Thin | 1px |

### **Color Tokens**
| Token | Value |
|-------|-------|
| `--icon-color-primary` | #3B82F6 |
| `--icon-color-secondary` | #6B7280 |
| `--icon-color-success` | #10B981 |
| `--icon-color-warning` | #F59E0B |
| `--icon-color-danger` | #EF4444 |
| `--icon-color-info` | #06B6D4 |
| `--icon-color-muted` | #9CA3AF |
| `--icon-color-inverse` | #FFFFFF |

---

## Usage Guidelines

### **Do**
- Use icons to reinforce adjacent text labels, not replace them
- Choose an icon that universally and instantly communicates its meaning
- Match icon visual weight (outlined vs filled) to surrounding typography weight
- Use `aria-hidden="true"` for decorative icons (the vast majority)
- Provide `aria-label` on the interactive parent when the icon is the only affordance
- Use the semantic status icons (`check-circle`, `x-circle`, `alert-triangle`, `info`) consistently throughout the app
- Use `filled` variant for active/selected states, `outlined` for default
- Scale icons proportionally to adjacent text (icon at 1em or matched point size)

### **Don't**
- Don't use icons without labels in critical actions (delete, submit) unless space is extremely constrained and tooltips are provided
- Don't mix icon styles (some outlined, some filled) at the same hierarchy level
- Don't rely on color alone to differentiate icon meaning — pair with a label or shape change
- Don't place `aria-label` directly on the `<Icon>` when it is inside a labelled `<button>` — this creates duplicate announcements
- Don't use low-contrast icons (< 3:1 ratio against background) for meaningful UI elements
- Don't animate icons that convey static status — animation should only signal change or process
- Don't create icon-only buttons without tooltips or visible labels

### **Icon Selection Principles**
- Prefer universally recognized symbols: magnifying glass for search, × for close, + for add
- Avoid ambiguous icons: a house icon is universally understood; a lightning bolt can mean speed, power, or error
- When in doubt, use text — it is always more explicit than an icon
- Use the same icon consistently for the same action throughout the entire application
- Never use more than one icon for the same concept in the same product

---

## Icon Implementation Notes

### **SVG Optimization**
All icons in the library are pre-optimized:
- Unnecessary metadata and comments stripped via SVGO
- Paths simplified and de-duplicated
- ViewBox normalized to `0 0 24 24`
- No hardcoded fill or stroke colors (all use `currentColor`)
- No embedded fonts or raster images

### **Tree Shaking**
Import individual icons to minimize bundle size:
```typescript
// ✅ Tree-shakeable named import
import { HomeIcon, SearchIcon } from '@components/icons';

// ❌ Barrel import (imports entire library)
import * as Icons from '@components/icons';
```

### **Sprite Sheet (Optional)**
For performance-critical applications, icons can be delivered as an SVG sprite:
```html
<!-- Sprite referenced by ID -->
<svg aria-hidden="true">
  <use href="/icons/sprite.svg#home" />
</svg>
```

### **Rendering at Non-Standard Sizes**
The icon SVG grid is `24×24`. When rendering at non-standard sizes:
- Multiples of the grid (12, 16, 20, 24, 32, 48, 64) render crisp
- Odd sizes (15, 17, 22) may have sub-pixel rendering artifacts on some screens
- Prefer the predefined size tokens over arbitrary values

---

## Related Components

- **Button**: Accepts `icon` prop for left or right icon placement
- **Badge**: Uses small icon variants for dot/status indicators
- **Avatar**: Uses icon fallback when no image or initials are available
- **Input**: Accepts leading/trailing icon slot
- **Chip**: Leading icon slot for compact filter chips
- **Toast / Alert**: Uses semantic status icons (check, warning, error, info)
- **Empty State**: Uses large `xl`–`4xl` icons as illustration anchors
- **Navigation**: Header, Sidebar, Bottom Nav all use `md`–`lg` icons

---

## Version History

- **v1.0**: Base icon set (200 icons), size scale, color inheritance
- **v1.1**: Filled variant, semantic color tokens, `aria-hidden` default
- **v1.2**: Duotone variant, animation props (spin, pulse, bounce)
- **v2.0**: Bold variant, gradient variant, complete design token system, SVGO optimization
- **v2.1**: Children prop for custom SVGs, sprite sheet support, full a11y audit
- **v2.2**: Gradient variant, reduced motion compliance, tree-shaking documentation

---

## Testing Checklist

- [ ] All size variants render at correct pixel dimensions
- [ ] Outlined variant uses `stroke: currentColor`, no hardcoded colors
- [ ] Filled variant uses `fill: currentColor`, no hardcoded colors
- [ ] Bold variant renders with increased stroke width
- [ ] Duotone variant renders two-tone correctly
- [ ] Color prop overrides `currentColor` correctly
- [ ] Color inherits from parent `color` CSS property by default
- [ ] `aria-hidden="true"` is default behavior (icons are decorative by default)
- [ ] `aria-label` + `role="img"` creates a meaningful accessible image
- [ ] `aria-label` on parent button is preferred over icon `aria-label` when interactive
- [ ] `spin` prop applies continuous rotation animation
- [ ] `pulse` prop applies opacity pulse animation
- [ ] `bounce` prop applies bounce animation
- [ ] All animations disabled under `prefers-reduced-motion: reduce`
- [ ] Icons render crisp at 16px, 20px, 24px (pixel-hint sizes)
- [ ] Custom SVG via `children` prop renders and inherits color
- [ ] Tree-shakeable imports work correctly (no full-library bundle)
- [ ] Icon contrast ≥ 3:1 for all semantic/informational use cases
- [ ] Screen reader skips decorative icons (aria-hidden)
- [ ] Screen reader announces semantic icons with role="img"
- [ ] ViewBox is `0 0 24 24` for all icons in library
- [ ] No hardcoded fill/stroke colors in any icon SVG
