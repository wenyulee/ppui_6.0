# Tooltip Component Documentation

**Component:** Tooltip
**Figma Node:** `260-10897`
**File:** Components v6 Beta
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

The Tooltip is a **compact, non-interactive informational overlay** that appears on hover (pointer devices) or focus (keyboard) to provide supplementary context for a UI element. It is the simplest floating surface in the design system: a solid-black rounded rectangle displaying white Label/Small text.

**Design philosophy:** Maximum contrast, minimum surface area. The full-black fill (`background.utility.emphasis`) with inverse-white text achieves a 21:1 contrast ratio and reads clearly over any page content. No shadow or elevation is applied — the dark background provides sufficient visual separation from the page.

**Key characteristics:**
- Black fill, white text — no elevation shadow
- Label/Small typography (12px, Medium weight)
- 4px border-radius (not a pill — a small rounded rect)
- 8px padding on all sides
- Single-line, `white-space: nowrap`
- Appears after a short hover delay to prevent accidental triggers
- Four placement variants: top, bottom, left, right
- Arrow/caret pointing toward the trigger element

---

## Component Variants

The Tooltip has a single visual style. Variants are positional:

| Variant | Description |
|---|---|
| **Top** (default) | Tooltip floats above the trigger, arrow points down |
| **Bottom** | Tooltip floats below the trigger, arrow points up |
| **Left** | Tooltip floats left of the trigger, arrow points right |
| **Right** | Tooltip floats right of the trigger, arrow points left |

Placement auto-flips when the preferred position would overflow the viewport.

---

## Anatomy

```
                    ┌─────────────┐
                    │   Label     │   ← Tooltip surface
                    └──────┬──────┘
                           │  ← Arrow / caret
                         [trigger]
```

- **Surface** — Black rounded rectangle container
- **Label text** — Label/Small white text, single line
- **Arrow** — 6×6px rotated square (CSS clip or border-trick triangle) in the same black fill, pointing toward the trigger element

---

## Size & Spacing Specifications

### Surface

| Property | Token | Value |
|---|---|---|
| Padding (all sides) | `tokens.base.spacing[8]` | 8px |
| Border radius | `tokens.base.border.radius[4]` | 4px |
| Min width | — | 24px (2 × 8px padding + 1 char) |
| Max width | — | 256px (wraps to two lines beyond this; prefer single-line) |
| White-space | — | `nowrap` (default; do not force-wrap) |

### Arrow / Caret

| Property | Value |
|---|---|
| Width | 8px |
| Height | 8px |
| Shape | 45° rotated square clipped to triangle, matching container fill |
| Gap to trigger | 4px (from arrow tip to trigger edge) |
| Offset | Centered on trigger, or on the tooltip surface when trigger is narrower |

### Offset from Trigger

Total visual gap between trigger and tooltip (including arrow): **8px**

```
[trigger]
    │
  4px gap
    ▲  ← 8px arrow height × sin(45°) ≈ 5.66px, clipped to 4px visible tip
    │
┌───────────┐
│  Tooltip  │
└───────────┘
```

---

## Style Specifications

### Container

| Property | Token | Resolved Value |
|---|---|---|
| Background | `tokens.colorScheme.background.utility.emphasis` | `#000000` |
| Border radius | `tokens.base.border.radius[4]` | `4px` |
| Box shadow | — | none |
| Display | — | `inline-flex` |
| Flex direction | — | `column` |
| Align items | — | `center` |

### Label Text

| Property | Token | Resolved Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.label` | Plain (Medium) |
| Font size | `tokens.base.text.fontsize.ramp[12]` | `12px` |
| Line height | `tokens.base.text.lineheight.ramp[12]` | `16px` |
| Font weight | `tokens.brand.text.fontweight.label` | `500` |
| Letter spacing | `tokens.base.text.letterspacing[0]` | `0px` |
| Color | `tokens.colorScheme.content.utility.inverse` | `#ffffff` |
| White-space | — | `nowrap` |

### Arrow

| Property | Value |
|---|---|
| Fill | `tokens.colorScheme.background.utility.emphasis` = `#000000` |
| Size | 8×8px square, rotated 45° |
| Clipping | Half-clipped to produce a filled triangle pointing at the trigger |

---

## Placement & Positioning

The Tooltip is portal-rendered at the document root (outside normal DOM flow) and positioned absolutely relative to the trigger's bounding rect.

### Placement Logic

```
Preferred:    top
Auto-flip:    if top overflows viewport → use bottom
              if left overflows viewport → shift right (keep same axis)
              if right overflows viewport → shift left (keep same axis)
              if both top and bottom overflow → use the side with more space
```

### Offset Calculations

| Placement | Tooltip X | Tooltip Y | Arrow position |
|---|---|---|---|
| **top** | trigger.centerX − tooltip.width/2 | trigger.top − tooltip.height − 8px | Bottom center of tooltip |
| **bottom** | trigger.centerX − tooltip.width/2 | trigger.bottom + 8px | Top center of tooltip |
| **left** | trigger.left − tooltip.width − 8px | trigger.centerY − tooltip.height/2 | Right center of tooltip |
| **right** | trigger.right + 8px | trigger.centerY − tooltip.height/2 | Left center of tooltip |

---

## Behavior & Timing

### Show Delay

A **150ms delay** is applied before showing the tooltip on hover. This prevents the tooltip from flashing during fast cursor movement across the screen.

| Event | Delay | Action |
|---|---|---|
| `mouseenter` | 150ms | Show tooltip |
| `mouseleave` | 0ms | Hide immediately |
| `focus` (keyboard) | 0ms | Show immediately (no delay for keyboard navigation) |
| `blur` (keyboard) | 0ms | Hide immediately |
| `Escape` key | 0ms | Hide immediately |
| Scroll | 0ms | Hide immediately |

### Persistence

- Tooltips **do not persist on click** — they are informational only, never interactive
- The tooltip stays visible while the cursor remains on the trigger or the trigger retains focus
- Hovering over the tooltip itself (if the cursor passes over it) does not dismiss it

---

## Animation Specifications

### Entrance

| Property | Token | Value |
|---|---|---|
| Duration | `tokens.base.motion.duration[100]` | 100ms |
| Easing | `tokens.base.motion.ease.standard.out` | `cubic-bezier(1, 0, 1, 1)` |
| Opacity | `0` → `1` |
| Transform (top/bottom) | `translateY(±4px)` → `translateY(0)` |
| Transform (left/right) | `translateX(±4px)` → `translateX(0)` |

### Exit

| Property | Token | Value |
|---|---|---|
| Duration | `tokens.base.motion.duration[50]` | 50ms |
| Easing | `tokens.base.motion.ease.standard.in` | `cubic-bezier(0, 0, 0, 1)` |
| Opacity | `1` → `0` |
| Transform | None (exit is fade-only) |

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .tooltip {
    animation: none !important;
    transition: opacity 50ms linear;
  }
}
```

---

## Props API

### TypeScript Interface

```typescript
interface TooltipProps {
  /**
   * The text label to display inside the tooltip.
   * Keep under ~40 characters; longer strings will wrap or exceed max-width.
   */
  label: string;

  /**
   * The trigger element. Must be a focusable element (button, input, link, or
   * element with tabIndex) to be keyboard-accessible.
   */
  children: React.ReactElement;

  /**
   * Preferred placement relative to the trigger.
   * The tooltip auto-flips if the preferred placement would overflow the viewport.
   * @default 'top'
   */
  placement?: 'top' | 'bottom' | 'left' | 'right';

  /**
   * Delay in ms before showing the tooltip on mouse hover.
   * Keyboard focus always shows immediately (0ms delay).
   * @default 150
   */
  showDelay?: number;

  /**
   * Additional pixel offset between arrow tip and trigger edge.
   * @default 4
   */
  offset?: number;

  /**
   * Disable the tooltip without removing it from the DOM.
   * Useful for conditional tooltips where the trigger state changes.
   * @default false
   */
  disabled?: boolean;

  /**
   * ID override for the tooltip element.
   * Auto-generated if omitted. Used to wire aria-describedby on the trigger.
   */
  id?: string;
}
```

---

## Code Examples

### Basic Usage

```tsx
import { Tooltip } from '@/components/Tooltip';

// Text trigger — wraps the trigger child
<Tooltip label="More information">
  <button type="button" aria-label="Info">
    <InfoIcon size={16} aria-hidden="true" />
  </button>
</Tooltip>

// Input with helper tooltip
<Tooltip label="Must be 8–20 characters" placement="right">
  <input type="password" aria-label="Password" />
</Tooltip>
```

### Conditional Tooltip

```tsx
// Show tooltip only when content is truncated
<Tooltip label={fullText} disabled={!isTruncated}>
  <span className="truncate">{fullText}</span>
</Tooltip>
```

### Component Implementation

```tsx
import React, {
  useState, useRef, useId, useEffect, useCallback
} from 'react';
import { createPortal } from 'react-dom';
import { useFloating, offset, flip, shift, arrow } from '@floating-ui/react';

export const Tooltip: React.FC<TooltipProps> = ({
  label,
  children,
  placement = 'top',
  showDelay = 150,
  offset: offsetProp = 4,
  disabled = false,
  id: idProp,
}) => {
  const [visible, setVisible] = useState(false);
  const autoId = useId();
  const tooltipId = idProp ?? `tooltip-${autoId}`;
  const showTimer = useRef<ReturnType<typeof setTimeout> | null>(null);
  const arrowRef = useRef<HTMLDivElement>(null);

  const { refs, floatingStyles, context, middlewareData } = useFloating({
    placement,
    open: visible,
    middleware: [
      offset(offsetProp + 4), // 4px arrow height + desired offset
      flip(),
      shift({ padding: 8 }),
      arrow({ element: arrowRef }),
    ],
  });

  const show = useCallback((immediate = false) => {
    if (disabled) return;
    if (immediate) {
      setVisible(true);
    } else {
      showTimer.current = setTimeout(() => setVisible(true), showDelay);
    }
  }, [disabled, showDelay]);

  const hide = useCallback(() => {
    if (showTimer.current) clearTimeout(showTimer.current);
    setVisible(false);
  }, []);

  // Hide on scroll
  useEffect(() => {
    if (!visible) return;
    const handleScroll = () => hide();
    window.addEventListener('scroll', handleScroll, { capture: true, passive: true });
    return () => window.removeEventListener('scroll', handleScroll, { capture: true });
  }, [visible, hide]);

  // Clone trigger child to inject ARIA + event handlers
  const trigger = React.cloneElement(children, {
    ref: refs.setReference,
    'aria-describedby': visible ? tooltipId : undefined,
    onMouseEnter: (e: React.MouseEvent) => {
      children.props.onMouseEnter?.(e);
      show(false);
    },
    onMouseLeave: (e: React.MouseEvent) => {
      children.props.onMouseLeave?.(e);
      hide();
    },
    onFocus: (e: React.FocusEvent) => {
      children.props.onFocus?.(e);
      show(true); // immediate for keyboard
    },
    onBlur: (e: React.FocusEvent) => {
      children.props.onBlur?.(e);
      hide();
    },
    onKeyDown: (e: React.KeyboardEvent) => {
      children.props.onKeyDown?.(e);
      if (e.key === 'Escape') hide();
    },
  });

  // Arrow position
  const arrowX = middlewareData.arrow?.x;
  const arrowY = middlewareData.arrow?.y;
  const staticSide = {
    top: 'bottom',
    right: 'left',
    bottom: 'top',
    left: 'right',
  }[context.placement.split('-')[0]] ?? 'bottom';

  return (
    <>
      {trigger}
      {visible && createPortal(
        <div
          id={tooltipId}
          role="tooltip"
          ref={refs.setFloating}
          style={floatingStyles}
          className="tooltip"
        >
          <span className="tooltip__label">{label}</span>
          <div
            ref={arrowRef}
            className="tooltip__arrow"
            style={{
              left: arrowX != null ? `${arrowX}px` : '',
              top: arrowY != null ? `${arrowY}px` : '',
              [staticSide]: '-4px',
            }}
          />
        </div>,
        document.body,
      )}
    </>
  );
};
```

### CSS Styles

```css
.tooltip {
  /* Layout */
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  padding: var(--tokens-base-spacing-8, 8px);
  max-width: 256px;
  pointer-events: none; /* tooltip is non-interactive */
  z-index: 10000;

  /* Visuals */
  background: var(--tokens-colorScheme-background-utility-emphasis, #000);
  border-radius: var(--tokens-base-border-radius-4, 4px);

  /* Animation */
  animation: tooltip-in 100ms cubic-bezier(1, 0, 1, 1) forwards;
}

.tooltip__label {
  font-family: var(--tokens-brand-text-fontfamily-label, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-12, 12px);
  line-height: var(--tokens-base-text-lineheight-ramp-12, 16px);
  font-weight: 500;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-utility-inverse, #fff);
  white-space: nowrap;
}

.tooltip__arrow {
  position: absolute;
  width: 8px;
  height: 8px;
  background: var(--tokens-colorScheme-background-utility-emphasis, #000);
  transform: rotate(45deg);
  border-radius: 1px;
  pointer-events: none;
}

/* Entrance — translate from direction of origin */
@keyframes tooltip-in {
  from {
    opacity: 0;
    translate: var(--tooltip-enter-offset, 0 4px);
  }
  to {
    opacity: 1;
    translate: 0 0;
  }
}

/* Placement-aware entrance offset */
[data-placement^='top']    { --tooltip-enter-offset: 0 4px; }
[data-placement^='bottom'] { --tooltip-enter-offset: 0 -4px; }
[data-placement^='left']   { --tooltip-enter-offset: 4px 0; }
[data-placement^='right']  { --tooltip-enter-offset: -4px 0; }

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  .tooltip {
    animation: none;
    opacity: 1;
  }
}
```

---

## Accessibility Specifications

### ARIA Pattern

The Tooltip implements the **WAI-ARIA Tooltip pattern**.

| Element | ARIA Attribute | Value | Purpose |
|---|---|---|---|
| Tooltip container | `role="tooltip"` | `"tooltip"` | Declares the element as a tooltip |
| Tooltip container | `id` | auto-generated | Used to reference from trigger |
| Trigger element | `aria-describedby` | `{tooltip-id}` (when visible) | Associates tooltip text with the trigger |

**Important:** `aria-describedby` is set only while the tooltip is **visible**. Removing it when hidden prevents stale associations.

### Keyboard Navigation

| Key / Event | Action |
|---|---|
| `Tab` | Focus moves to trigger element; tooltip appears immediately |
| `Shift+Tab` | Focus leaves trigger; tooltip hides |
| `Escape` | Tooltip hides; focus stays on trigger |
| Any key while tooltip visible | Tooltip hides if `Escape`; other keys do not dismiss |

**Tooltip content is never focusable** — the tooltip container has `pointer-events: none` and is excluded from the tab order entirely. Users read the tooltip content via `aria-describedby` association, not by navigating into it.

### Trigger Requirements

The trigger element **must be a focusable element** for the tooltip to be keyboard-accessible:

```tsx
// ✅ Correct — button is natively focusable
<Tooltip label="Delete item">
  <button type="button" aria-label="Delete">
    <TrashIcon aria-hidden="true" />
  </button>
</Tooltip>

// ✅ Correct — div with role and tabIndex
<Tooltip label="Additional info">
  <div role="button" tabIndex={0} onClick={handleClick}>
    Info
  </div>
</Tooltip>

// ❌ Wrong — non-focusable div; keyboard users cannot access the tooltip
<Tooltip label="Cannot be accessed via keyboard">
  <div>Hover me</div>
</Tooltip>
```

### Screen Reader Behavior

When a keyboard user focuses the trigger:
1. The screen reader announces the trigger's `aria-label` / text content
2. After a short pause, it announces the associated tooltip text (via `aria-describedby`)

Example announcement: *"Delete button. Permanently removes this item from your account."*

### Color Contrast

| Element | Foreground | Background | Ratio |
|---|---|---|---|
| Label text | `#ffffff` | `#000000` | **21:1** ✅ |

---

## Platform APIs

### Android — Jetpack Compose

```kotlin
import androidx.compose.material3.TooltipBox
import androidx.compose.material3.TooltipDefaults
import androidx.compose.material3.rememberTooltipState
import androidx.compose.material3.PlainTooltip

@Composable
fun TooltipExample() {
    val tooltipState = rememberTooltipState()

    TooltipBox(
        positionProvider = TooltipDefaults.rememberPlainTooltipPositionProvider(
            spacingBetweenTooltipAndAnchor = 8.dp
        ),
        tooltip = {
            PlainTooltip(
                containerColor = Color.Black,
                contentColor = Color.White,
                shape = RoundedCornerShape(4.dp),
            ) {
                Text(
                    text = "Label",
                    style = TextStyle(
                        fontFamily = PlainFont,
                        fontWeight = FontWeight.Medium,
                        fontSize = 12.sp,
                        lineHeight = 16.sp,
                        letterSpacing = 0.sp
                    )
                )
            }
        },
        state = tooltipState
    ) {
        IconButton(onClick = {}) {
            Icon(Icons.Default.Info, contentDescription = "More info")
        }
    }
}
```

### iOS — SwiftUI

```swift
import SwiftUI

// Native iOS 17+ tooltip support
struct ContentView: View {
    var body: some View {
        Button {
            // action
        } label: {
            Image(systemName: "info.circle")
        }
        // iOS 17+: native .help() tooltip
        .help("Label")
    }
}

// Custom tooltip implementation for iOS 15/16
struct TooltipView: View {
    let label: String

    var body: some View {
        Text(label)
            .font(.custom("Plain-Medium", size: 12))
            .foregroundColor(.white)
            .lineLimit(1)
            .padding(8)
            .background(
                RoundedRectangle(cornerRadius: 4)
                    .fill(Color.black)
            )
    }
}

struct TooltipModifier: ViewModifier {
    let label: String
    @State private var isVisible = false

    func body(content: Content) -> some View {
        content
            .overlay(alignment: .top) {
                if isVisible {
                    TooltipView(label: label)
                        .offset(y: -40)
                        .transition(.opacity.combined(with: .offset(y: 4)))
                        .zIndex(999)
                }
            }
            .onHover { hovering in
                withAnimation(.easeOut(duration: 0.1)) {
                    isVisible = hovering
                }
            }
            .accessibilityHint(label)
    }
}

extension View {
    func tooltip(_ label: String) -> some View {
        modifier(TooltipModifier(label: label))
    }
}
```

---

## Design Tokens Reference

| Token | Resolved Value | Usage in Tooltip |
|---|---|---|
| `tokens.colorScheme.background.utility.emphasis` | `#000000` | Container background |
| `tokens.colorScheme.content.utility.inverse` | `#ffffff` | Label text color |
| `tokens.base.border.radius[4]` | `4px` | Container corner radius |
| `tokens.base.spacing[8]` | `8px` | Container padding (all sides) |
| `tokens.brand.text.fontfamily.label` | Plain (Medium) | Label font family |
| `tokens.base.text.fontsize.ramp[12]` | `12px` | Label font size |
| `tokens.base.text.lineheight.ramp[12]` | `16px` | Label line height |
| `tokens.brand.text.fontweight.label` | `500` | Label font weight |
| `tokens.base.text.letterspacing[0]` | `0px` | Label letter spacing |
| `tokens.base.motion.duration[100]` | `100ms` | Entrance animation |
| `tokens.base.motion.duration[50]` | `50ms` | Exit animation |
| `tokens.base.motion.ease.standard.out` | `cubic-bezier(1,0,1,1)` | Entrance easing |
| `tokens.base.motion.ease.standard.in` | `cubic-bezier(0,0,0,1)` | Exit easing |

---

## Best Practices

**Write tooltip labels as short noun phrases or brief sentences.** The tooltip container is `nowrap` by default — labels over ~40 characters risk overflow on narrow viewports. Good examples: "Copy link", "Send to bank account", "Required field". Avoid full sentences with punctuation.

**Do not put critical information in a tooltip.** Because tooltips are hidden by default and unavailable on touch devices without additional implementation, any content that is essential for task completion must be visible at all times (as body text or an inline label). Tooltips are supplementary only.

**Never put interactive content inside a tooltip.** Links, buttons, and inputs must not be placed inside the tooltip container. The tooltip has `pointer-events: none` and is excluded from the tab order. Use a Popover component for interactive floating content.

**Icon-only buttons must always have a tooltip.** When a button contains only an icon (no visible label text), a tooltip is required to communicate its purpose to pointer users — `aria-label` handles screen readers, but sighted users who are unfamiliar with the icon benefit from the tooltip label.

**Do not use tooltips for form validation errors.** Error messages must be always-visible inline text, not tooltips. Tooltips may supplement an input (e.g., "Must be 8–20 characters" as a hint), but they must not be the only way to communicate an error state.

**Respect the hover delay.** The 150ms delay is intentional — removing it causes tooltips to flash on every cursor movement across the page. The delay should be `0ms` only for keyboard focus (where there is no accidental hover risk).

---

## Related Components

- **Toast** — Transient notification that appears in a fixed position; Toast uses `content.base` (black text on white), Tooltip uses `content.utility.inverse` (white text on black)
- **Popover** — Interactive floating container for menus, pickers, and other controls; unlike Tooltip, Popover is focusable and persists on click
- **Badge** — Inline status indicator with similar Label/Small text but rendered as inline content, not a floating overlay

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial Tooltip documentation from Components v6 Beta Figma file |

---

## Testing Checklist

- [ ] Container renders with `background.utility.emphasis` (#000000) fill
- [ ] No box-shadow / elevation applied
- [ ] Border-radius is 4px (not a pill; not 8px)
- [ ] Label text is Label/Small: 12px / 16px line-height / 500 weight / Plain / white
- [ ] `white-space: nowrap` prevents text wrap on single-line labels
- [ ] Padding is 8px all sides
- [ ] Tooltip appears after 150ms hover delay on mouse enter
- [ ] Tooltip appears immediately (0ms) on keyboard focus
- [ ] Tooltip hides immediately on mouse leave
- [ ] Tooltip hides immediately on blur
- [ ] `Escape` key hides visible tooltip; focus remains on trigger
- [ ] `role="tooltip"` on container
- [ ] `aria-describedby` on trigger references tooltip `id` while visible
- [ ] `aria-describedby` removed from trigger when tooltip is hidden
- [ ] Screen reader announces tooltip text after trigger label on focus
- [ ] Tooltip is `pointer-events: none` (cursor passes through)
- [ ] Tooltip is excluded from tab order (cannot be focused)
- [ ] Arrow/caret visible, fills black, points toward trigger
- [ ] Placement `top` (default): tooltip above trigger, arrow below tooltip
- [ ] Placement `bottom`: tooltip below trigger, arrow above tooltip
- [ ] Placement `left` / `right`: tooltip to side, arrow on appropriate edge
- [ ] Auto-flip triggers when preferred placement overflows viewport
- [ ] Entrance animation: 100ms fade + 4px slide from correct direction
- [ ] `prefers-reduced-motion`: slide suppressed; opacity-only transition retained
- [ ] Scroll event hides visible tooltip
- [ ] Touch devices: tooltip not shown on tap (touch has no hover state)
