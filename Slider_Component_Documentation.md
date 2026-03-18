# Slider Component Documentation

**Component:** Slider
**Version:** 6.0 Beta
**Figma Node:** `2718-10436`
**Last Updated:** 2026-03-18
**Status:** Beta

---

## Table of Contents

1. [Overview](#overview)
2. [Component Variants](#component-variants)
3. [Anatomy](#anatomy)
4. [Size & Spacing Specifications](#size--spacing-specifications)
5. [Style Variants](#style-variants)
6. [State Specifications](#state-specifications)
7. [Props API](#props-api)
8. [Code Examples](#code-examples)
9. [Accessibility Specifications](#accessibility-specifications)
10. [Animation Specifications](#animation-specifications)
11. [Design Tokens](#design-tokens)
12. [Best Practices](#best-practices)
13. [Related Components](#related-components)
14. [Version History](#version-history)
15. [Testing Checklist](#testing-checklist)

---

## Overview

The **Slider** component allows users to select a single numeric value from a continuous or discrete range by dragging a thumb handle along a track. It is optimized for percentage-based or bounded numeric inputs where precise keyboard or touch control is essential.

### When to Use

- Adjusting a continuous value within a defined range (e.g., volume, brightness, zoom level, spacing)
- Providing a direct-manipulation input for values where approximate selection is acceptable
- Settings panels, filters, or configuration screens where a visual range representation aids comprehension

### When Not to Use

- When the user needs to enter a precise numeric value — use a Text Input with `type="number"` instead
- When the range has a very large number of discrete steps and exact step values must be communicated — use a Select or Stepper component
- When multiple simultaneous range selections are needed — use a Range Slider (two-thumb variant) if available

---

## Component Variants

The Slider ships as a single horizontal track variant in v6. Platform-specific implementations are detailed in the Props API and Code Examples sections.

| Variant | Platform | Description |
|---|---|---|
| Default (Continuous) | Web / React | Full continuous range, no discrete steps |
| Stepped | Web / React | Snaps to defined step increments |
| Default | Android (Compose) | Native `Slider` composable with `value` + `onValueChange` |
| Default | iOS (SwiftUI) | Native `Slider` with `value` binding + `in` range |

---

## Anatomy

```
┌──────────────────────────────────────────────────────────────┐
│                                                              │
│   ●━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━○──────────────────────   │
│   ↑                                ↑                         │
│  Min                             Thumb                       │
│  Value                        (at ~50%)                      │
│                                                              │
│   ◀─────── Filled Track ──────▶◀─── Unfilled Track ──────▶  │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Part Descriptions

| Part | Element | Description |
|---|---|---|
| **Track** | `<div role="none">` | Full-width horizontal rail spanning the component width |
| **Filled Track** | `<div>` (pseudo or child) | Colored portion from the minimum to the current thumb position |
| **Unfilled Track** | `<div>` (pseudo or child) | Neutral portion from the current thumb position to maximum |
| **Thumb** | `<div role="slider">` | Circular drag handle; receives focus and keyboard events |
| **Min Label** *(optional)* | `<span>` | Text label displayed below the left end of the track |
| **Max Label** *(optional)* | `<span>` | Text label displayed below the right end of the track |
| **Value Label** *(optional)* | `<div>` (tooltip) | Floating label above the thumb showing the current value |
| **Step Markers** *(stepped only)* | `<div>` repeated | Tick marks along the track at each step interval |

---

## Size & Spacing Specifications

### Track Dimensions

| Property | Value | Token |
|---|---|---|
| Track height | 4px | `--slider-track-height: 4px` |
| Track border-radius | 2px (pill) | `--slider-track-radius: 2px` |
| Track horizontal padding | 0px (full bleed to container) | — |
| Minimum track width | 120px | `--slider-min-width: 120px` |

### Thumb Dimensions

| Property | Value | Token |
|---|---|---|
| Thumb diameter | 20px | `--slider-thumb-size: 20px` |
| Thumb border-radius | 50% | — |
| Thumb border width | 0px (shadow only) | — |
| Thumb elevation shadow | Elevation / Level 1 (dual drop-shadow) | `--elevation-level-1` |
| Thumb focus ring width | 2px | `--focus-ring-width: 2px` |
| Thumb focus ring offset | 2px | `--focus-ring-offset: 2px` |

### Elevation / Level 1 Shadow (Thumb)

The thumb carries the design system's **Elevation Level 1** treatment — a dual drop-shadow:

```css
/* Elevation / Level 1 */
box-shadow:
  0px 1px 2px rgba(0, 0, 0, 0.12),
  0px 0px 1px rgba(0, 0, 0, 0.08);
```

### Touch Target

The thumb's interactive hit area is expanded beyond its visual 20px diameter to meet minimum touch target requirements:

| Property | Value |
|---|---|
| Minimum touch target | 44×44px |
| Implementation | `::before` pseudo-element with `content: ""`, `position: absolute`, `inset: -12px` |

### Vertical Rhythm & Label Spacing

| Property | Value |
|---|---|
| Track vertical centering within component | Component height = 44px (thumb touch target drives height) |
| Label → Track gap (min/max labels) | 8px |
| Value tooltip → Thumb gap | 6px |
| Label font size | 12px |
| Label font weight | Regular (400) |

---

## Style Variants

### Track Fill Colors

| State | Filled Track Color | Token |
|---|---|---|
| Default | Brand / Primary | `--color-brand-primary` |
| Disabled | Neutral / Muted | `--color-neutral-muted` |

### Thumb Colors

| State | Background | Border/Shadow |
|---|---|---|
| Rest | `--color-surface-default` (white) | Elevation Level 1 |
| Hover | `--color-surface-default` | Elevation Level 1 + subtle scale |
| Pressed/Active | `--color-surface-default` | Elevation Level 1 (compressed) |
| Focus | `--color-surface-default` | Elevation Level 1 + focus ring |
| Disabled | `--color-neutral-subtle` | No shadow |

### Unfilled Track Color

| State | Color | Token |
|---|---|---|
| Default | `--color-neutral-subtle` | Neutral track background |
| Disabled | `--color-neutral-subtle` (same, opacity reduced) | — |

---

## State Specifications

### Rest

- Thumb is positioned at the current value along the track
- Filled track extends from 0 to the thumb position
- Elevation Level 1 shadow visible on thumb
- No focus ring

### Hover

- Cursor changes to `grab`
- Thumb may scale up slightly (1.05×) to indicate interactivity
- Elevation Level 1 shadow remains

### Pressed / Active

- Cursor changes to `grabbing`
- Thumb scale returns to 1.0 (visual feedback of press)
- User can drag thumb left/right; value updates continuously (`onValueChange` fires on every move)

### Focused (Keyboard)

- 2px focus ring (`--color-focus-ring`, typically brand blue) appears around the thumb
- Focus ring offset: 2px
- Arrow key navigation is available (see Accessibility section)

### Disabled

- Track fill and thumb render in `--color-neutral-muted`
- `aria-disabled="true"` on the slider element
- `pointer-events: none` prevents interaction
- `cursor: not-allowed`
- Opacity: 0.4

---

## Props API

### Web / React

```typescript
interface SliderProps {
  /** Current value of the slider */
  value: number;

  /** Minimum value of the range (default: 0) */
  min?: number;

  /** Maximum value of the range (default: 100) */
  max?: number;

  /** Step increment for discrete sliders (default: 1; use 0 for fully continuous) */
  step?: number;

  /** Callback fired when the value changes during drag */
  onValueChange: (value: number) => void;

  /** Callback fired when the user commits a value (mouseup / touchend / keyup) */
  onValueCommit?: (value: number) => void;

  /** Whether the slider is disabled */
  disabled?: boolean;

  /** Accessible label for the slider (required if no visible label element) */
  'aria-label'?: string;

  /** ID of an element that labels the slider */
  'aria-labelledby'?: string;

  /** ID of an element that describes the slider */
  'aria-describedby'?: string;

  /** Human-readable value text for screen readers (e.g., "50%", "Medium") */
  'aria-valuetext'?: string;

  /** Show min/max text labels beneath the track ends */
  showLabels?: boolean;

  /** Custom formatter for min/max labels */
  formatLabel?: (value: number) => string;

  /** Show a floating value tooltip above the thumb */
  showTooltip?: boolean | 'hover' | 'always';

  /** Custom formatter for the value tooltip */
  formatTooltip?: (value: number) => string;

  /** Show tick marks at each step position */
  showStepMarkers?: boolean;

  /** Additional CSS class applied to the root element */
  className?: string;

  /** Inline style overrides for the root element */
  style?: React.CSSProperties;
}
```

### Android — Jetpack Compose

```kotlin
@Composable
fun Slider(
    value: Float,                           // Current value (0.0f–1.0f normalized, or within valueRange)
    onValueChange: (Float) -> Unit,         // Called during drag
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    valueRange: ClosedFloatingPointRange<Float> = 0f..1f,
    steps: Int = 0,                         // 0 = continuous; N = N-1 discrete intervals
    onValueChangeFinished: (() -> Unit)? = null, // Called on drag end
    colors: SliderColors = SliderDefaults.colors(),
    interactionSource: MutableInteractionSource = remember { MutableInteractionSource() }
)
```

**Basic Compose usage:**

```kotlin
var sliderPosition by remember { mutableStateOf(0.5f) }

Slider(
    value = sliderPosition,
    onValueChange = { sliderPosition = it }
)
```

### iOS — SwiftUI

```swift
Slider(
    value: Binding<Double>,             // Two-way binding to the current value
    in range: ClosedRange<Double>,      // e.g., 0...100
    step: Double.Stride = 1,           // Step granularity (optional)
    onEditingChanged: (Bool) -> Void   // Called when drag begins/ends
)
```

**Basic SwiftUI usage:**

```swift
@State private var sliderValue: Double = 50.0

Slider(value: $sliderValue, in: 0...100) { editing in
    print("Editing: \(editing), Value: \(sliderValue)")
}
```

---

## Code Examples

### 1. Basic Uncontrolled Slider

```tsx
import { Slider } from '@design-system/components';

export function BasicSlider() {
  return (
    <Slider
      value={50}
      min={0}
      max={100}
      onValueChange={(val) => console.log(val)}
      aria-label="Volume"
    />
  );
}
```

---

### 2. Controlled Slider with State

```tsx
import { useState } from 'react';
import { Slider } from '@design-system/components';

export function VolumeControl() {
  const [volume, setVolume] = useState(50);

  return (
    <div>
      <label id="volume-label">Volume: {volume}%</label>
      <Slider
        value={volume}
        min={0}
        max={100}
        step={1}
        onValueChange={setVolume}
        aria-labelledby="volume-label"
        aria-valuetext={`${volume} percent`}
      />
    </div>
  );
}
```

---

### 3. Slider with Min/Max Labels and Tooltip

```tsx
import { useState } from 'react';
import { Slider } from '@design-system/components';

export function SpacingSlider() {
  const [spacing, setSpacing] = useState(16);

  return (
    <Slider
      value={spacing}
      min={4}
      max={64}
      step={4}
      onValueChange={setSpacing}
      showLabels
      formatLabel={(val) => `${val}px`}
      showTooltip="hover"
      formatTooltip={(val) => `${val}px`}
      aria-label="Element spacing"
      aria-valuetext={`${spacing} pixels`}
    />
  );
}
```

---

### 4. Disabled Slider

```tsx
import { Slider } from '@design-system/components';

export function DisabledSlider() {
  return (
    <Slider
      value={30}
      min={0}
      max={100}
      onValueChange={() => {}}
      disabled
      aria-label="Brightness (unavailable)"
    />
  );
}
```

---

### 5. Stepped Slider with Tick Marks

```tsx
import { useState } from 'react';
import { Slider } from '@design-system/components';

export function QualitySlider() {
  const [quality, setQuality] = useState(2);
  const labels = ['Low', 'Medium', 'High', 'Ultra'];

  return (
    <div>
      <label id="quality-label">Quality: {labels[quality]}</label>
      <Slider
        value={quality}
        min={0}
        max={3}
        step={1}
        onValueChange={setQuality}
        showStepMarkers
        showLabels
        formatLabel={(val) => labels[val]}
        aria-labelledby="quality-label"
        aria-valuetext={labels[quality]}
      />
    </div>
  );
}
```

---

### 6. Slider in a Settings Form

```tsx
import { useState } from 'react';
import { Slider } from '@design-system/components';

export function SettingsPanel() {
  const [brightness, setBrightness] = useState(75);
  const [contrast, setContrast] = useState(50);

  return (
    <form aria-label="Display Settings">
      <fieldset>
        <legend>Display</legend>

        <div className="setting-row">
          <label id="brightness-label">Brightness</label>
          <Slider
            value={brightness}
            min={0}
            max={100}
            onValueChange={setBrightness}
            onValueCommit={(val) => applyBrightness(val)}
            aria-labelledby="brightness-label"
            aria-valuetext={`${brightness}%`}
          />
          <span aria-hidden="true">{brightness}%</span>
        </div>

        <div className="setting-row">
          <label id="contrast-label">Contrast</label>
          <Slider
            value={contrast}
            min={0}
            max={100}
            onValueChange={setContrast}
            onValueCommit={(val) => applyContrast(val)}
            aria-labelledby="contrast-label"
            aria-valuetext={`${contrast}%`}
          />
          <span aria-hidden="true">{contrast}%</span>
        </div>
      </fieldset>
    </form>
  );
}
```

---

### 7. React Hook Form Integration

```tsx
import { Controller, useForm } from 'react-hook-form';
import { Slider } from '@design-system/components';

interface FilterFormValues {
  priceMax: number;
}

export function PriceFilter() {
  const { control, handleSubmit } = useForm<FilterFormValues>({
    defaultValues: { priceMax: 500 },
  });

  return (
    <form onSubmit={handleSubmit(console.log)}>
      <label id="price-label">Maximum Price</label>
      <Controller
        name="priceMax"
        control={control}
        render={({ field }) => (
          <Slider
            value={field.value}
            min={0}
            max={1000}
            step={10}
            onValueChange={field.onChange}
            onValueCommit={field.onBlur}
            aria-labelledby="price-label"
            aria-valuetext={`$${field.value}`}
          />
        )}
      />
    </form>
  );
}
```

---

### 8. Compose — Stepped Slider with Range

```kotlin
@Composable
fun OpacityControl() {
    var opacity by remember { mutableStateOf(1f) }

    Column {
        Text("Opacity: ${(opacity * 100).roundToInt()}%")
        Slider(
            value = opacity,
            onValueChange = { opacity = it },
            valueRange = 0f..1f,
            steps = 9, // Creates 10 discrete positions: 0%, 10%, 20%...100%
            onValueChangeFinished = {
                // Commit the value (e.g., save to preferences)
            }
        )
    }
}
```

---

### 9. SwiftUI — Labeled Slider

```swift
struct SpeedControl: View {
    @State private var speed: Double = 1.0

    var body: some View {
        VStack(alignment: .leading) {
            Text("Playback Speed: \(speed, specifier: "%.1f")×")
                .font(.subheadline)
            Slider(value: $speed, in: 0.5...2.0, step: 0.25) { _ in }
                .accessibilityLabel("Playback Speed")
                .accessibilityValue("\(speed, specifier: "%.1f") times")
        }
        .padding()
    }
}
```

---

### 10. Custom Formatted Percentage Slider (with `aria-valuetext`)

```tsx
import { useState } from 'react';
import { Slider } from '@design-system/components';

function formatPercent(value: number): string {
  if (value === 0) return 'None';
  if (value === 100) return 'Full';
  return `${value}%`;
}

export function OpacitySlider() {
  const [opacity, setOpacity] = useState(75);

  return (
    <div>
      <label id="opacity-label">Opacity</label>
      <Slider
        value={opacity}
        min={0}
        max={100}
        step={5}
        onValueChange={setOpacity}
        aria-labelledby="opacity-label"
        aria-valuetext={formatPercent(opacity)}
        showTooltip="always"
        formatTooltip={formatPercent}
      />
    </div>
  );
}
```

---

## Accessibility Specifications

### ARIA Role

The slider thumb implements `role="slider"` with the full set of required and optional ARIA attributes:

```html
<div
  role="slider"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-valuenow="50"
  aria-valuetext="50 percent"
  aria-labelledby="slider-label"
  aria-orientation="horizontal"
  tabindex="0"
>
```

| ARIA Attribute | Required | Description |
|---|---|---|
| `role="slider"` | ✅ | Identifies the element as a slider widget |
| `aria-valuemin` | ✅ | The minimum allowed value |
| `aria-valuemax` | ✅ | The maximum allowed value |
| `aria-valuenow` | ✅ | The current numeric value |
| `aria-valuetext` | Recommended | Human-readable text representation of the current value (e.g., "50%", "Medium"). Always provide when the numeric value alone is not meaningful |
| `aria-orientation` | Recommended | Set to `"horizontal"` (default assumed, but explicit is better) |
| `aria-label` or `aria-labelledby` | ✅ | Associates the slider with a visible label or provides an accessible name |
| `aria-describedby` | Optional | Associates with a description element for additional context |
| `aria-disabled` | Conditional | Set to `"true"` when the slider is disabled |

### Keyboard Navigation

| Key | Action |
|---|---|
| `Tab` | Move focus to the slider thumb |
| `Shift + Tab` | Move focus away from the slider thumb |
| `ArrowRight` / `ArrowUp` | Increase value by one step |
| `ArrowLeft` / `ArrowDown` | Decrease value by one step |
| `Page Up` | Increase value by a large step (typically 10% of range) |
| `Page Down` | Decrease value by a large step |
| `Home` | Set value to minimum |
| `End` | Set value to maximum |

Large step size for Page Up/Down defaults to `Math.max(1, Math.round((max - min) / 10))`.

### Focus Management

- The thumb (`role="slider"`) is the sole focusable element within the component
- `tabindex="0"` is always present on the thumb
- When the slider is disabled, `tabindex="-1"` is set and `aria-disabled="true"` is applied
- Focus ring: 2px solid `--color-focus-ring` with 2px offset, visible at all times during keyboard navigation

### Screen Reader Behavior

- On value change (keyboard or drag), the new `aria-valuenow` and `aria-valuetext` values are announced automatically since they are live attributes on the focused element
- Do **not** add `aria-live` to the slider; the role itself handles announcements
- When using stepped sliders, provide meaningful `aria-valuetext` for each step (e.g., "Low", "Medium", "High") rather than relying on the numeric step index
- Always pair the slider with a visible label; never use `aria-label` as a substitute for a visible label unless space constraints are extreme

### Contrast Requirements

| Element | Minimum Contrast | Target |
|---|---|---|
| Filled track vs. background | 3:1 (WCAG 1.4.11 non-text) | ≥ 4.5:1 |
| Thumb vs. background | 3:1 (non-text) | ≥ 4.5:1 |
| Focus ring vs. adjacent colors | 3:1 | — |
| Label text vs. background | 4.5:1 (WCAG 1.4.3) | — |

### Touch Accessibility

- Thumb touch target is 44×44px minimum (expanded via `::before` pseudo-element)
- Drag interaction works with one finger; no complex gestures required
- On iOS VoiceOver: swipe left/right adjusts value; `accessibilityAdjustableComponent` protocol used natively
- On Android TalkBack: volume keys adjust the focused slider value

---

## Animation Specifications

### Thumb Drag

No transition animation during active drag — the thumb follows the pointer position in real time for zero latency:

```css
.slider-thumb {
  /* No transition during drag (applied via JS class removal) */
}
```

### Filled Track Width

The filled track width transitions smoothly only on **keyboard** interactions and **programmatic** value changes (not during drag):

```css
.slider-track-filled {
  transition: width 100ms ease-out;
}

/* Remove transition during pointer drag */
.slider-root[data-dragging] .slider-track-filled {
  transition: none;
}
```

### Thumb Scale on Hover / Press

```css
.slider-thumb {
  transition: transform 150ms ease, box-shadow 150ms ease;
}

.slider-thumb:hover {
  transform: scale(1.1);
}

.slider-thumb:active {
  transform: scale(1.0);
}
```

### Focus Ring

```css
.slider-thumb:focus-visible {
  outline: 2px solid var(--color-focus-ring);
  outline-offset: 2px;
  transition: outline-offset 100ms ease;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .slider-track-filled,
  .slider-thumb {
    transition: none;
  }
}
```

---

## Design Tokens

### Spacing & Sizing Tokens

| Token | Value | Usage |
|---|---|---|
| `--slider-track-height` | `4px` | Track thickness |
| `--slider-track-radius` | `2px` | Track border-radius (pill shape) |
| `--slider-thumb-size` | `20px` | Thumb diameter |
| `--slider-min-width` | `120px` | Minimum slider width |
| `--slider-touch-target` | `44px` | Touch target expansion |

### Color Tokens

| Token | Light Mode | Dark Mode | Usage |
|---|---|---|---|
| `--slider-track-fill-color` | `--color-brand-primary` | `--color-brand-primary` | Filled portion of track |
| `--slider-track-empty-color` | `--color-neutral-subtle` | `--color-neutral-subtle` | Unfilled portion of track |
| `--slider-thumb-bg` | `--color-surface-default` | `--color-surface-default` | Thumb background |
| `--slider-thumb-disabled-bg` | `--color-neutral-subtle` | `--color-neutral-subtle` | Thumb when disabled |
| `--slider-track-disabled-color` | `--color-neutral-muted` | `--color-neutral-muted` | Track when disabled |
| `--slider-focus-ring` | `--color-focus-ring` | `--color-focus-ring` | Keyboard focus ring |

### Elevation Token

| Token | Value | Usage |
|---|---|---|
| `--elevation-level-1` | `0px 1px 2px rgba(0,0,0,0.12), 0px 0px 1px rgba(0,0,0,0.08)` | Thumb shadow |

### Typography Tokens (Labels)

| Token | Value | Usage |
|---|---|---|
| `--slider-label-font-size` | `12px` | Min/max label text |
| `--slider-label-font-weight` | `400` (Regular) | Min/max label weight |
| `--slider-label-color` | `--color-text-muted` | Min/max label color |
| `--slider-tooltip-font-size` | `12px` | Value tooltip text |
| `--slider-tooltip-font-weight` | `500` (Medium) | Value tooltip weight |

---

## Best Practices

### Do

- **Always provide an accessible label.** Use a visible `<label>` with `aria-labelledby`, or at minimum `aria-label` on the slider thumb.
- **Provide `aria-valuetext`** whenever the numeric `aria-valuenow` alone is not self-explanatory (e.g., for stepped sliders where each value corresponds to a named option like "Low / Medium / High").
- **Use `onValueCommit`** for side effects (API calls, animations) and `onValueChange` for reactive UI updates only. This prevents excessive network requests during drag.
- **Display the current value** visually adjacent to the slider so all users can see it without relying solely on the tooltip.
- **Expand the thumb touch target** to at least 44×44px for touch devices.
- **Test with keyboard navigation** — confirm all arrow keys, Home, End, Page Up/Down work as expected.
- **Use stepped sliders** when values correspond to named options or when arbitrary precision is not useful.

### Don't

- **Don't use a slider for precise numeric input** — users cannot reliably target a specific pixel-level value. Pair the slider with a numeric input field if precision matters.
- **Don't omit `aria-valuemin` and `aria-valuemax`** — screen readers need these to compute percentages and announce relative position.
- **Don't skip `aria-valuetext` for stepped sliders** with named options — "2" is meaningless to a screen reader user if the options are "Low", "Medium", "High", "Ultra".
- **Don't make the slider the only means of input** in forms where a precise value might be needed — offer a text input alternative.
- **Don't fire expensive side effects in `onValueChange`** (e.g., API calls on every mouse move) — use `onValueCommit` instead.
- **Don't place multiple sliders in a group without individual labels** — each slider must have its own accessible name.
- **Don't use a slider for binary on/off states** — use a Switch or Toggle instead.

### Pairing with a Numeric Input

For precision-critical use cases, pair the slider with a number input:

```tsx
import { useState } from 'react';
import { Slider, Input } from '@design-system/components';

export function PreciseSlider() {
  const [value, setValue] = useState(50);

  const handleSliderChange = (val: number) => setValue(val);
  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const parsed = parseInt(e.target.value, 10);
    if (!isNaN(parsed)) setValue(Math.min(100, Math.max(0, parsed)));
  };

  return (
    <div style={{ display: 'flex', gap: 16, alignItems: 'center' }}>
      <label id="precise-label" style={{ flexShrink: 0 }}>Zoom</label>
      <Slider
        value={value}
        min={0}
        max={100}
        onValueChange={handleSliderChange}
        aria-labelledby="precise-label"
        aria-valuetext={`${value}%`}
        style={{ flex: 1 }}
      />
      <Input
        type="number"
        value={value}
        min={0}
        max={100}
        onChange={handleInputChange}
        aria-labelledby="precise-label"
        style={{ width: 64 }}
        suffix="%"
      />
    </div>
  );
}
```

---

## Related Components

| Component | Relationship |
|---|---|
| **Input (Number)** | Use instead when precise numeric entry is required |
| **Progress Bar** | Use when displaying read-only progress; not interactive |
| **Segmented Control** | Use for a small discrete set of labeled options instead of a stepped slider |
| **Switch / Toggle** | Use for binary (on/off) values |
| **Stepper** | Use for incrementing/decrementing numeric values by fixed amounts with visible controls |

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Horizontal track, single thumb, Elevation Level 1 on thumb, Compose + SwiftUI APIs |

---

## Testing Checklist

### Visual

- [ ] Track renders at 4px height with pill ends (border-radius: 2px)
- [ ] Filled track color matches `--color-brand-primary`
- [ ] Unfilled track color matches `--color-neutral-subtle`
- [ ] Thumb diameter is 20px with Elevation Level 1 shadow (dual drop-shadow)
- [ ] Thumb is vertically centered on the track
- [ ] Thumb position accurately reflects the current value proportionally
- [ ] Disabled state: muted fill color, reduced opacity, no shadow

### Interaction

- [ ] Drag: thumb follows pointer/touch position within track bounds
- [ ] Click anywhere on track: thumb jumps to clicked position
- [ ] `onValueChange` fires continuously during drag
- [ ] `onValueCommit` fires once on mouseup/touchend/keyup
- [ ] Value is clamped to [min, max] range
- [ ] Step snapping works correctly for stepped sliders
- [ ] Touch target is at least 44×44px (verified via DevTools)

### Keyboard

- [ ] `Tab` moves focus to slider thumb
- [ ] Focus ring is visible on thumb when focused
- [ ] `ArrowRight` / `ArrowUp` increases value by step
- [ ] `ArrowLeft` / `ArrowDown` decreases value by step
- [ ] `Page Up` increases value by large step (~10% of range)
- [ ] `Page Down` decreases value by large step
- [ ] `Home` sets value to minimum
- [ ] `End` sets value to maximum
- [ ] Disabled slider cannot receive keyboard focus

### Screen Reader

- [ ] `role="slider"` present on thumb element
- [ ] `aria-valuemin`, `aria-valuemax`, `aria-valuenow` present and correct
- [ ] `aria-valuetext` provides human-readable value description
- [ ] `aria-label` or `aria-labelledby` provides an accessible name
- [ ] Value changes announced automatically on keypress
- [ ] Disabled state announced (`aria-disabled="true"`)

### Animation

- [ ] No lag/transition during active drag
- [ ] Smooth transition on keyboard value changes (100ms)
- [ ] `prefers-reduced-motion: reduce` disables all transitions
- [ ] Thumb hover scale animation: 1.0 → 1.1 → 1.0 on press

### Platform (Native)

- [ ] **Android (Compose):** `Slider(value, onValueChange)` renders correctly; TalkBack announces value changes with volume keys
- [ ] **iOS (SwiftUI):** `Slider(value:in:)` renders correctly; VoiceOver swipe adjusts value; `accessibilityValue` reflects current value

### Accessibility Audit

- [ ] Filled track meets 3:1 contrast against page background (WCAG 1.4.11)
- [ ] Thumb meets 3:1 contrast against page background
- [ ] Label text meets 4.5:1 contrast
- [ ] Focus ring meets 3:1 contrast against adjacent colors
- [ ] Component passes axe-core with zero violations
- [ ] Tested in macOS VoiceOver + Safari
- [ ] Tested in NVDA + Chrome (Windows)
- [ ] Tested in TalkBack (Android)
