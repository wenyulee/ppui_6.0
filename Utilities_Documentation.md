# Utilities Documentation

**Section:** Utilities
**Version:** 6.0 Beta
**Figma File:** Components v6 – Beta (`qqH85RTAgalgEUUtJahjAB`)
**Figma Nodes:**
- Slot: `174-2160`
- Text: `272-6296`
- Slot Vertical: `543-11005`
- Slot Horizontal: `1529-10112`
- Chevron: `138-30598`
- Selected: `138-30600`
- Close: `192-2033`
- StateLayer / Base: `4580-2158`
- StateLayer / Emphasis: `4580-2162`

---

## Table of Contents

1. [Overview](#1-overview)
2. [Slot](#2-slot)
   - 2.1 [Single Slot](#21-single-slot)
   - 2.2 [Slot Vertical](#22-slot-vertical)
   - 2.3 [Slot Horizontal](#23-slot-horizontal)
3. [Text](#3-text)
4. [Chevron](#4-chevron)
5. [Selected](#5-selected)
6. [Close](#6-close)
7. [StateLayer](#7-statelayer)
   - 7.1 [StateLayer / Base](#71-statelayer--base)
   - 7.2 [StateLayer / Emphasis](#72-statelayer--emphasis)
8. [Design Tokens Reference](#8-design-tokens-reference)
9. [Accessibility Specifications](#9-accessibility-specifications)
10. [Best Practices](#10-best-practices)
11. [Related Components](#11-related-components)
12. [Version History](#12-version-history)

---

## 1. Overview

**Utilities** are a set of primitive, reusable building blocks used internally by higher-level components throughout the PayPal design system. They are not standalone UI patterns — instead, they are composed into components such as List rows, Cards, Modals, Settings pages, and interactive controls.

**Utility summary:**

| Utility | Node | Purpose |
|---------|------|---------|
| Slot | `174-2160` | Single generic content placeholder slot |
| Slot Vertical | `543-11005` | Stack of 3 Slots in a column layout |
| Slot Horizontal | `1529-10112` | Row of 3 Slots in a horizontal layout |
| Text | `272-6296` | Body/Medium text primitive |
| Chevron | `138-30598` | Right-pointing caret icon for trailing navigation affordance |
| Selected | `138-30600` | Check-circle indicator for selected state |
| Close | `192-2033` | Dismiss/close action button |
| StateLayer / Base | `4580-2158` | Interaction overlay for light/default surfaces |
| StateLayer / Emphasis | `4580-2162` | Interaction overlay for dark/emphasis surfaces |

---

## 2. Slot

A **Slot** is a generic flex container placeholder that accepts arbitrary content — icons, text, controls, or any composed element. Slots serve as the structural unit in list rows, settings items, and form controls.

### 2.1 Single Slot

**Figma Node:** `174-2160`

The base single-slot unit used in any layout context.

#### Specifications

| Property | Value | Token |
|----------|-------|-------|
| Typography | Label/Medium | `tokens.brand.text.fontfamily.label` |
| Font size | 14 px | `tokens.base.text.fontsize.ramp[14]` |
| Font weight | 500 (Medium) | `tokens.brand.text.fontweight.label` |
| Line height | 14 px | `tokens.base.text.lineheight.ramp[14]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |
| Layout | Flex, `align-items: stretch` | — |

#### iOS Code Connect

```swift
// PDSSwitch.swift
import PDSComponents

PDSSwitch(
    label: "Label",
    isOn: $isOn
)
```

#### Usage

```tsx
// Single slot — accepts any content child
<Slot>
  <Switch label="Notifications" />
</Slot>

<Slot>
  <Icon name="chevron-right" />
</Slot>
```

---

### 2.2 Slot Vertical

**Figma Node:** `543-11005`

Three Slot units stacked vertically with uniform spacing. Represents the typical settings section pattern (e.g., a list of toggle rows).

#### Specifications

| Property | Value | Token |
|----------|-------|-------|
| Layout | Column (flex-direction: column) | — |
| Gap between slots | 8 px | `tokens.base.spacing[8]` |
| Width | 248 px | — |
| Alignment | `align-items: flex-start` | — |
| Content alignment | `align-content: stretch` | — |

#### iOS Code Connect

```swift
// PDSSwitch.swift — three-slot vertical group
VStack(spacing: tokens.base.spacing[8]) {
    PDSSwitch(label: "Label", isOn: $isOn1)
    PDSSwitch(label: "Label", isOn: $isOn2)
    PDSSwitch(label: "Label", isOn: $isOn3)
}
```

#### Usage

```tsx
<SlotVertical>
  <Slot><Switch label="Wi-Fi" /></Slot>
  <Slot><Switch label="Bluetooth" /></Slot>
  <Slot><Switch label="Notifications" /></Slot>
</SlotVertical>
```

---

### 2.3 Slot Horizontal

**Figma Node:** `1529-10112`

Three Slot units arranged in a horizontal row. Represents leading action areas in top bars, toolbars, or segmented controls.

#### iOS Code Connect

```swift
// PDSSwitch.swift — three-slot horizontal group
HStack {
    PDSSwitch(label: "Label", isOn: $isOn1)
    PDSSwitch(label: "Label", isOn: $isOn2)
    PDSSwitch(label: "Label", isOn: $isOn3)
}
```

#### Android Code Connect

```kotlin
// TopBar.kt — horizontal slot group in leading position
TopBar(
    leadingContent = {
        Row {
            Slot { /* content */ }
            Slot { /* content */ }
            Slot { /* content */ }
        }
    }
)
```

#### Usage

```tsx
<SlotHorizontal>
  <Slot><IconButton icon="menu" /></Slot>
  <Slot><IconButton icon="search" /></Slot>
  <Slot><IconButton icon="more" /></Slot>
</SlotHorizontal>
```

---

## 3. Text

**Figma Node:** `272-6296`

The **Text** utility is the base typographic primitive for body content within composed components. It renders a plain string using the Body/Medium style.

### Typography Specification

| Property | Value | Token |
|----------|-------|-------|
| Font family | Plain Regular | `tokens.brand.text.fontfamily.body` |
| Font size | 14 px | `tokens.base.text.fontsize.ramp[14]` |
| Font weight | 400 (Regular) | `tokens.brand.text.fontweight.body` |
| Line height | 14 px | `tokens.base.text.lineheight.ramp[14]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |
| Color | `content.base` | `tokens.colorScheme.content.base` |

> **Note:** This is Body/Medium — the Plain Regular (400) style at 14 px — the standard inline text size for list descriptions, input hints, and row labels.

### Code Connect

```kotlin
// Compose (Android)
Text(
    text = "Label",
    style = PdsTheme.typography.bodyMedium
)
```

### Usage

```tsx
// As a body text wrapper
<Text variant="body-medium">Transaction complete</Text>

// Composed inside a list row
<ListRow
  trailing={<Chevron />}
  label={<Text>Settings</Text>}
/>
```

### CSS

```css
.pds-text--body-medium {
  font-family: var(--tokens-brand-text-fontfamily-body); /* Plain Regular */
  font-size: var(--tokens-base-text-fontsize-ramp-14);   /* 14px */
  font-weight: var(--tokens-brand-text-fontweight-body); /* 400 */
  line-height: var(--tokens-base-text-lineheight-ramp-14);
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-base);
}
```

---

## 4. Chevron

**Figma Node:** `138-30598`

The **Chevron** utility is a right-pointing caret icon used as a trailing affordance in list rows, navigation links, and disclosure elements. It communicates that the item is tappable and will navigate deeper or reveal more content.

### Specifications

| Property | Value |
|----------|-------|
| Icon name | `chevron-right` / `caret-right` |
| Rendered size | 16×16 px |
| Color | `content.base` (default) / `content.secondary` (muted) |
| Alignment | Vertically centered, trailing edge |

### Code Connect

```swift
// iOS — PDSIcon
import PDSComponents

PDSIcon(name: "caret-right", size: .small) // 16pt
```

```kotlin
// Android — Icon.ChevronRight from List.kt
import com.paypal.pds.components.List

Icon.ChevronRight(
    tint = PdsTheme.colorScheme.content.base
)
```

### Usage

```tsx
import { Icon } from '@paypal/design-system';

// As a standalone trailing chevron in a list row
<ListRow
  label="Manage cards"
  trailing={<Icon name="chevron-right" size={16} />}
/>

// Inside a navigation link
<NavigationItem
  label="Privacy settings"
  trailing={<Chevron />}
/>
```

### CSS

```css
.pds-chevron {
  width: 16px;
  height: 16px;
  color: var(--tokens-colorScheme-content-base);
  flex-shrink: 0;
}
```

### Accessibility

- The Chevron icon is always decorative (`aria-hidden="true"`) — the navigational affordance is communicated by the parent list row or link element.
- Do not assign a separate `aria-label` to the chevron; label the interactive parent instead.

---

## 5. Selected

**Figma Node:** `138-30600`

The **Selected** utility renders a filled check-circle icon to communicate that an item is in a selected or confirmed state. Used in selection lists, payment method pickers, and settings confirmations.

### Specifications

| Property | Value | Token |
|----------|-------|-------|
| Icon | `check-circle-fill` | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Color | Brand blue / `content.link` | `tokens.colorScheme.content.link` |
| Gap from container edge | 8 px | `tokens.base.spacing[8]` |
| Layout | Flex row, `align-items: center` | — |

> The icon fill color is the brand accent blue (`#0066f5`), consistent with `content.link` — signaling a positive, confirmed selection.

### Usage

```tsx
import { Icon } from '@paypal/design-system';

// Selected indicator in a list row
<ListRow
  label="Visa •••• 4242"
  trailing={isSelected ? <Icon name="check-circle-fill" size={24} color="content.link" /> : <Chevron />}
/>

// Payment method selector
<PaymentOption
  label="PayPal Balance"
  selected={true}
  indicator={<Selected />}
/>
```

### CSS

```css
.pds-selected {
  display: flex;
  align-items: center;
  gap: var(--tokens-base-spacing-8); /* 8px */
}

.pds-selected__icon {
  width: 24px;
  height: 24px;
  color: var(--tokens-colorScheme-content-link); /* #0066f5 */
  flex-shrink: 0;
}
```

### Accessibility

- The `Selected` icon should have `aria-label="Selected"` when used as a standalone indicator, or `aria-hidden="true"` when the selected state is also communicated via `aria-selected` on the parent list item.

```tsx
// When parent carries aria-selected
<li role="option" aria-selected={true}>
  <Icon name="check-circle-fill" aria-hidden="true" />
  Visa •••• 4242
</li>

// When icon is the sole indicator
<Icon name="check-circle-fill" aria-label="Selected" />
```

---

## 6. Close

**Figma Node:** `192-2033`

The **Close** utility is a pre-configured `IconButton` (small, tertiary variant) with an `×` (close/dismiss) icon. It is used in modals, sheets, toasts, banners, and any overlay that requires a dismiss affordance.

### Specifications

| Property | Value |
|----------|-------|
| Component | `IconButton` |
| Variant | `tertiary` |
| Size | `small` (32×32 px touch target) |
| Icon | `close` / `×` (16×16 px) |
| Background | Transparent (tertiary) |
| Hit area | 32×32 px minimum |

### Code Connect

```tsx
// React
import { IconButton } from '@paypal/design-system';

<IconButton
  size="small"
  variant="tertiary"
  aria-label="Close"
>
  <CloseIcon />
</IconButton>
```

### Usage

```tsx
// Modal close button
<Modal>
  <Modal.Header>
    <h2>Confirm payment</h2>
    <Close aria-label="Close modal" onClick={onClose} />
  </Modal.Header>
  <Modal.Body>...</Modal.Body>
</Modal>

// Banner dismiss
<Banner
  message="Your payment was successful."
  onDismiss={handleDismiss}
  dismissButton={<Close aria-label="Dismiss notification" />}
/>
```

### CSS

```css
/* Close inherits all IconButton/tertiary/small styles */
.pds-close {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: 999px;
  background: transparent;
  border: none;
  cursor: pointer;
  color: var(--tokens-colorScheme-content-base);
  padding: 0;
}

.pds-close:hover {
  background-color: var(--tokens-colorScheme-background-states-base-hover);
}

.pds-close:active {
  background-color: var(--tokens-colorScheme-background-states-base-pressed);
}

.pds-close:focus-visible {
  outline: 2px solid var(--tokens-colorScheme-border-focus);
  outline-offset: 2px;
}
```

### Accessibility

- Always provide a descriptive `aria-label` — the default is `"Close"` but it should be contextual: `"Close modal"`, `"Dismiss notification"`, `"Close menu"`.
- Must be keyboard-focusable and activatable via Enter and Space.
- Minimum touch target: 32×32 px.

---

## 7. StateLayer

The **StateLayer** is a transparent overlay div that communicates pointer/touch interaction states (hover, pressed) on interactive elements. It sits at the top of the z-stack inside any pressable component, covering the full hit area.

There are two StateLayer variants corresponding to the two surface types:

| Variant | Surface | Use case |
|---------|---------|---------|
| **Base** | Light / default backgrounds | Buttons, list rows, cards on white/light backgrounds |
| **Emphasis** | Dark / emphasis backgrounds | Controls on dark fills, brand-colored surfaces, inverted components |

The demo size in Figma is **40×40 px**, matching the standard icon button hit area, but StateLayer is applied at **100% width/height** of its parent in production.

---

### 7.1 StateLayer / Base

**Figma Node:** `4580-2158`

For use on **light / default** (white or near-white) surfaces.

#### State Tokens

| State | Background Color | Token |
|-------|----------------|-------|
| Enabled | `transparent` | — |
| Hover | `rgba(5, 55, 130, 0.04)` | `tokens.colorScheme.background.states.base.hover` |
| Pressed | `rgba(5, 55, 130, 0.08)` | `tokens.colorScheme.background.states.base.pressed` |

> The base state layer uses a blue-tinted dark overlay derived from the PayPal brand navy (`#05 37 82`). Hover is 4% opacity; pressed is 8% opacity.

#### CSS

```css
.pds-state-layer {
  position: absolute;
  inset: 0;
  border-radius: inherit;
  pointer-events: none;
  transition: background-color 100ms ease;
}

/* Base variant — light surfaces */
.pds-state-layer--base {
  background-color: transparent;
}

.pds-interactive:hover .pds-state-layer--base {
  background-color: var(
    --tokens-colorScheme-background-states-base-hover,
    rgba(5, 55, 130, 0.04)
  );
}

.pds-interactive:active .pds-state-layer--base {
  background-color: var(
    --tokens-colorScheme-background-states-base-pressed,
    rgba(5, 55, 130, 0.08)
  );
}
```

#### TypeScript Interface

```typescript
interface StateLayerBaseProps {
  state?: 'Enabled' | 'Hover' | 'Pressed';
  className?: string;
}
```

---

### 7.2 StateLayer / Emphasis

**Figma Node:** `4580-2162`

For use on **dark / emphasis** surfaces — black fills, brand-colored backgrounds, inverted or high-contrast components.

#### State Tokens

| State | Background Color | Token |
|-------|----------------|-------|
| Enabled | `transparent` | — |
| Hover | `rgba(255, 255, 255, 0.24)` | `tokens.colorScheme.background.states.emphasis.hover` |
| Pressed | `rgba(255, 255, 255, 0.31)` | `tokens.colorScheme.background.states.emphasis.pressed` |

> The emphasis state layer uses a white overlay to lighten dark surfaces on interaction. Hover is 24% white; pressed is 31% white.

#### CSS

```css
/* Emphasis variant — dark surfaces */
.pds-state-layer--emphasis {
  background-color: transparent;
}

.pds-interactive:hover .pds-state-layer--emphasis {
  background-color: var(
    --tokens-colorScheme-background-states-emphasis-hover,
    rgba(255, 255, 255, 0.24)
  );
}

.pds-interactive:active .pds-state-layer--emphasis {
  background-color: var(
    --tokens-colorScheme-background-states-emphasis-pressed,
    rgba(255, 255, 255, 0.31)
  );
}
```

#### TypeScript Interface

```typescript
interface StateLayerEmphasisProps {
  state?: 'Enabled' | 'Hover' | 'Pressed';
  className?: string;
}
```

---

### StateLayer Composition Pattern

StateLayers are always positioned **absolutely** inside a **relatively-positioned** interactive parent:

```tsx
// Generic pressable component with StateLayer
function PressableCard({ children, surface = 'base', onClick }: PressableCardProps) {
  return (
    <div
      className="pds-pressable-card relative cursor-pointer"
      onClick={onClick}
    >
      {/* Content renders below the state layer */}
      {children}

      {/* StateLayer sits above content, pointer-events: none */}
      <div
        className={`pds-state-layer pds-state-layer--${surface}`}
        aria-hidden="true"
      />
    </div>
  );
}

// On a light card
<PressableCard surface="base">
  <p>Pay $25.00</p>
</PressableCard>

// On a dark/brand-colored surface
<PressableCard surface="emphasis">
  <p>Continue</p>
</PressableCard>
```

### StateLayer Transition

```css
/* Consistent 100ms ease for both enter and exit */
.pds-state-layer {
  transition: background-color 100ms ease;
}
```

---

## 8. Design Tokens Reference

### Typography Tokens (Slot & Text)

| Token | Value | Usage |
|-------|-------|-------|
| `tokens.brand.text.fontfamily.label` | Plain Medium | Slot label text |
| `tokens.brand.text.fontfamily.body` | Plain Regular | Text utility |
| `tokens.base.text.fontsize.ramp[14]` | 14 px | Both Label/Medium and Body/Medium |
| `tokens.brand.text.fontweight.label` | 500 | Slot label weight |
| `tokens.brand.text.fontweight.body` | 400 | Text utility weight |
| `tokens.base.text.lineheight.ramp[14]` | — | Shared line height |
| `tokens.base.text.letterspacing[0]` | 0 | No letter spacing |

### Spacing Tokens (Slot & Selected)

| Token | Value | Usage |
|-------|-------|-------|
| `tokens.base.spacing[8]` | 8 px | Slot Vertical gap; Selected icon gap |

### Size Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `tokens.base.size[24]` | 24 px | Selected / Chevron icon size |
| `tokens.base.size[16]` | 16 px | Chevron icon size |

### Color Tokens

| Token | Value | Usage |
|-------|-------|-------|
| `tokens.colorScheme.content.base` | `#000000` (light) | Text, Chevron, Close icon color |
| `tokens.colorScheme.content.link` | `#0066f5` | Selected check-circle fill |
| `tokens.colorScheme.border.focus` | `#0066f5` | Close button focus ring |
| `tokens.colorScheme.background.states.base.hover` | `rgba(5,55,130,0.04)` | StateLayer Base hover |
| `tokens.colorScheme.background.states.base.pressed` | `rgba(5,55,130,0.08)` | StateLayer Base pressed |
| `tokens.colorScheme.background.states.emphasis.hover` | `rgba(255,255,255,0.24)` | StateLayer Emphasis hover |
| `tokens.colorScheme.background.states.emphasis.pressed` | `rgba(255,255,255,0.31)` | StateLayer Emphasis pressed |

### CSS Custom Properties

```css
:root {
  /* StateLayer — Base */
  --tokens-colorScheme-background-states-base-hover: rgba(5, 55, 130, 0.04);
  --tokens-colorScheme-background-states-base-pressed: rgba(5, 55, 130, 0.08);

  /* StateLayer — Emphasis */
  --tokens-colorScheme-background-states-emphasis-hover: rgba(255, 255, 255, 0.24);
  --tokens-colorScheme-background-states-emphasis-pressed: rgba(255, 255, 255, 0.31);

  /* Content */
  --tokens-colorScheme-content-link: #0066f5;
  --tokens-colorScheme-border-focus: #0066f5;

  /* Spacing */
  --tokens-base-spacing-8: 8px;

  /* Typography */
  --tokens-base-text-fontsize-ramp-14: 14px;
  --tokens-brand-text-fontweight-label: 500;
  --tokens-brand-text-fontweight-body: 400;
}
```

---

## 9. Accessibility Specifications

### Summary by Utility

| Utility | ARIA / A11y Notes |
|---------|------------------|
| Slot | No ARIA role; must be provided by its content child |
| Text | Rendered as `<span>` or `<p>`; inherits surrounding semantic context |
| Chevron | Always `aria-hidden="true"` — decorative trailing affordance |
| Selected | `aria-hidden="true"` when parent carries `aria-selected`; else `aria-label="Selected"` |
| Close | Must have `aria-label` (e.g., `"Close"`, `"Dismiss"`); keyboard: Enter / Space |
| StateLayer | Always `aria-hidden="true"`, `pointer-events: none` |

### Focus Management

- **Close**: Receives focus when triggered by keyboard. On activation, focus should return to the element that triggered the overlay (modal trigger, notification origin).
- **StateLayers**: Must never intercept keyboard focus or pointer events.

### WCAG Criteria

| Criterion | Notes |
|-----------|-------|
| 1.1.1 Non-text Content | Chevron and Selected icons carry `aria-hidden`; Close carries `aria-label` |
| 1.4.1 Use of Color | Selected state is not conveyed by color alone — check-circle fill provides shape distinction |
| 1.4.11 Non-text Contrast | StateLayer hover/pressed overlays do not affect underlying content contrast (translucent only) |
| 2.1.1 Keyboard | Close: Enter / Space activation. Slots: keyboard handled by composed content child |
| 2.4.3 Focus Order | Close button participates in natural document focus order within its parent overlay |
| 4.1.2 Name, Role, Value | Close has explicit `aria-label`. StateLayer is `aria-hidden` |

---

## 10. Best Practices

### Slot

- Use Slot as a structural container — always provide meaningful content within it; an empty Slot renders nothing visible.
- Slot Vertical is the correct layout for settings-style toggle lists; do not use `SlotHorizontal` for stacked content.
- Match the slot layout to the component context: `SlotHorizontal` for toolbar/top-bar leading actions; `SlotVertical` for settings sections.

### Text

- Use the Text utility only for body-weight (400) copy. For labels, headings, or links, use the appropriate semantic Typography component from Foundations.
- Do not set custom `font-weight` on the Text utility — weight is controlled by the `variant` prop.

### Chevron

- Always pair Chevron with an interactive parent (`<button>`, `<a>`, `<li role="option">`).
- Chevron direction should always be `right` for navigation (forward/deeper). Use `down` / `up` for disclosure/expand patterns via the Icon component directly.

### Selected

- Use `Selected` only for the confirmed selection state. Do not use it as a generic "success" indicator — use the Banner or Contextual Alert for that.
- Pair the visual indicator with `aria-selected` on the parent element.

### Close

- Always use a contextual `aria-label` — `"Close"` alone is insufficient when multiple closeable items coexist on screen.
- The Close button must be the **first or last** focusable element inside its container to maintain predictable tab order.

### StateLayer

- Always use **Base** StateLayer on white, light gray, or any `background.base` surface.
- Always use **Emphasis** StateLayer on `background.utility.emphasis` (black) or any dark/brand-colored fill.
- Never apply both StateLayer variants to the same element simultaneously.
- StateLayer transitions (`100ms ease`) should match the component's other interaction timing.
- Never set `pointer-events: auto` on a StateLayer — it must remain passthrough.

---

## 11. Related Components

| Component | Relationship |
|-----------|-------------|
| **List** | Composes Slot, Chevron, Selected, Text as row sub-elements |
| **IconButton** | Close is a pre-configured small/tertiary IconButton |
| **Switch / Toggle** | Primary content inside Slot Vertical (Code Connect: `PDSSwitch`) |
| **Modal** | Uses Close as its dismiss button |
| **Banner** | Uses Close as its dismiss button |
| **Button** | Uses StateLayer Base internally for hover/pressed states |
| **TopNavigation** | Uses Slot Horizontal for leading icon button group (Code Connect: `TopBar.kt`) |
| **Card** | Uses StateLayer Base as its interaction overlay |
| **Selection Card** | Uses Selected indicator + StateLayer |

---

## 12. Version History

| Version | Change |
|---------|--------|
| 6.0 Beta | Initial release of Utilities section with Slot, Text, Chevron, Selected, Close, and StateLayer |
| 6.0 Beta | StateLayer split into `base` and `emphasis` variants with distinct token sets |
| 6.0 Beta | Slot Vertical (column, 8px gap) and Slot Horizontal (row) layout variants introduced |
| 6.0 Beta | iOS Code Connect: `PDSSwitch` (Slot), `PDSIcon` (Chevron) |
| 6.0 Beta | Android Code Connect: `Icon.ChevronRight` from `List.kt` (Chevron), `TopBar.kt` (Slot Horizontal) |

---

*Documentation generated from Figma nodes `174-2160`, `272-6296`, `543-11005`, `1529-10112`, `138-30598`, `138-30600`, `192-2033`, `4580-2158`, `4580-2162` — Components v6 Beta (`qqH85RTAgalgEUUtJahjAB`)*
