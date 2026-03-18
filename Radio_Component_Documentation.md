# Radio Component Documentation

## Overview

The Radio component is a single-selection form control that allows users to choose one option from a mutually exclusive set. It renders as a circular toggle — an outer ring with an inner dot appearing in the selected state — and is almost always used within a `RadioGroup`, which manages the shared `name` binding, keyboard navigation, and group-level validation messaging.

The individual `Radio` supports optional label text. The `RadioGroup` stacks multiple Radio items vertically with consistent spacing and optionally renders a validation message (error or helper text with an icon) below the list.

**Component name:** `Radio` / `RadioGroup`
**Figma nodes:** `604-6332` (Radio individual), `4474-77674` (RadioGroup)
**Platform:** React / TypeScript (Web), SwiftUI (iOS), Jetpack Compose (Android)
**Category:** Form / Input

---

## Component Variants

### Radio (Individual)

| Variant | Prop | Description |
|---------|------|-------------|
| **Unchecked + Label** | `checked=false`, `label=true` | Default unselected state with visible text label |
| **Checked + Label** | `checked=true`, `label=true` | Selected state with visible text label |
| **Unchecked, No Label** | `checked=false`, `label=false` | Standalone circle with no text; label provided externally |
| **Checked, No Label** | `checked=true`, `label=false` | Selected circle with no text; label provided externally |

### RadioGroup

| Variant | Prop | Description |
|---------|------|-------------|
| **Default** | `validation=false` | Stacked list of Radio items, no validation message |
| **With Validation** | `validation=true` | Adds a Contextual Alert (icon + text) below the list |

---

## Anatomy

### Radio (Individual)

```
  ○  Label text          ← Unchecked: empty ring
  ◉  Label text          ← Checked: ring + filled center dot
  ○                      ← Unchecked without label
  ◉                      ← Checked without label
```

### RadioGroup

```
  ○  Option A            ← Radio item (24px gap between items)
  ◉  Option B            ← Radio item (selected)
  ○  Option C
  ○  Option D
  ○  Option E
                         ← 8px gap between list and validation
  ⚠  Validation message  ← Contextual Alert (icon + label text, 12px)
```

---

## Size & Spacing Specifications

### Radio Control (Circle)

| Property | Value | Token |
|----------|-------|-------|
| Outer diameter | 20 × 20 px | Fixed |
| Border width | 2 px | `tokens.base.border.size[2]` |
| Border color (unchecked) | Black | `tokens.colorScheme.content.base` |
| Fill (unchecked) | White / transparent | `tokens.colorScheme.background.default` |
| Inner dot diameter (checked) | 8 px | — |
| Inner dot color (checked) | Black | `tokens.colorScheme.content.base` |

### Label Typography

| Property | Value | Token |
|----------|-------|-------|
| Font family | Body | `tokens.brand.text.fontfamily.body` |
| Font weight | Regular | `tokens.brand.text.fontweight.body` |
| Font size | 16 px | `tokens.base.text.fontsize.ramp[16]` |
| Line height | 24 px | `tokens.base.text.lineheight.ramp[16]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |
| Gap (circle → label) | 8 px | `tokens.base.spacing[8]` |

### RadioGroup Container

| Property | Value | Token |
|----------|-------|-------|
| Width | 370 px (fluid) | — |
| Padding (vertical) | 0 px | `tokens.base.spacing[0]` |
| Gap (items) | 24 px | `tokens.base.spacing[24]` |
| Gap (list → validation) | 8 px | `tokens.base.spacing[8]` |
| Alignment | `flex-start` | — |

### Validation Message

| Property | Value | Token |
|----------|-------|-------|
| Icon size | 16 × 16 px | — |
| Gap (icon → text) | 4 px | `tokens.base.spacing[4]` |
| Font size | 12 px | `tokens.base.text.fontsize.ramp[12]` |
| Line height | 16 px | `tokens.base.text.lineheight.ramp[12]` |
| Font color | rgba(0,0,0,0.63) | `tokens.colorScheme.content.muted` |

---

## State Specifications

### Unchecked (Default)
- Outer ring visible, no inner dot
- Border: `tokens.colorScheme.content.base` (black)
- Fill: white / transparent background

### Checked (Selected)
- Outer ring + filled inner dot (8 px)
- Both ring and dot: `tokens.colorScheme.content.base` (black)
- Only one Radio in a group can be checked at a time

### Hover
- Outer ring border brightens or cursor changes to pointer
- Optional: subtle background highlight on the label+control row

### Focus
- Focus ring drawn around the outer circle using `outline` or `box-shadow`
- Focus is visible when navigating with Tab or arrow keys
- Style: `tokens.colorScheme.border.focus` ring, 2 px offset

### Disabled
- Both ring and label rendered in `tokens.colorScheme.content.disabled`
- Reduced opacity (~40%)
- `pointer-events: none`; `cursor: not-allowed`
- `aria-disabled="true"` on the input; excluded from keyboard navigation flow

### Error
- Ring border switches to `tokens.colorScheme.status.negative` (red)
- Validation message appears below the RadioGroup (set by `validation` + `errorMessage` props)
- `aria-describedby` on the group pointing to the error message element
- `aria-invalid="true"` is not standard for radio groups; instead use `aria-errormessage` on the `<fieldset>`

### Read-Only
- Visually identical to the selected/unselected state
- `readonly` attribute (or `aria-readonly="true"` on the group container)
- No hover or focus interaction

---

## Props API

### TypeScript Interface

```typescript
import { ChangeEvent, ReactNode, InputHTMLAttributes } from 'react';

// ─── Radio (Individual) ───────────────────────────────────────────────────────

export interface RadioProps
  extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type' | 'size'> {
  /**
   * Text label displayed next to the radio circle.
   * When omitted, `aria-label` or `aria-labelledby` must be provided.
   */
  label?: string;

  /**
   * Supporting description text rendered below the label.
   */
  description?: string;

  /**
   * The value submitted with the form when this radio is selected.
   */
  value: string;

  /**
   * Whether the radio is currently selected.
   * Use in controlled mode; pair with `onChange`.
   */
  checked?: boolean;

  /**
   * Default checked state for uncontrolled mode.
   * @default false
   */
  defaultChecked?: boolean;

  /**
   * Shared name binding that groups radio buttons together.
   * All radios in a group must share the same `name`.
   */
  name?: string;

  /**
   * Callback fired when the radio is selected.
   */
  onChange?: (event: ChangeEvent<HTMLInputElement>) => void;

  /**
   * When true, the radio cannot be interacted with.
   * @default false
   */
  disabled?: boolean;

  /**
   * When true, the radio appears in an error state.
   * Typically controlled by the parent RadioGroup.
   * @default false
   */
  error?: boolean;

  /**
   * Additional class name(s) applied to the root label element.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── RadioGroup ───────────────────────────────────────────────────────────────

export interface RadioGroupProps {
  /**
   * The Radio items within this group.
   */
  children: ReactNode;

  /**
   * The name attribute shared by all radios in this group.
   * Passed automatically to child Radio components via context.
   */
  name: string;

  /**
   * The currently selected value (controlled mode).
   */
  value?: string;

  /**
   * Default selected value (uncontrolled mode).
   */
  defaultValue?: string;

  /**
   * Callback fired when the selected value changes.
   */
  onChange?: (value: string) => void;

  /**
   * Group legend / label describing the set of options.
   * Rendered as a <legend> inside a <fieldset> for accessibility.
   */
  legend: string;

  /**
   * Whether to visually hide the legend (it remains accessible to screen readers).
   * @default false
   */
  hideLegend?: boolean;

  /**
   * When true, shows the validation message below the group.
   * @default false
   */
  validation?: boolean;

  /**
   * Validation message text displayed when `validation` is true.
   */
  validationMessage?: string;

  /**
   * Semantic type of the validation message.
   * @default 'error'
   */
  validationType?: 'error' | 'warning' | 'info' | 'success';

  /**
   * When true, all radios in the group are disabled.
   * @default false
   */
  disabled?: boolean;

  /**
   * When true, all radios in the group are required.
   * @default false
   */
  required?: boolean;

  /**
   * Additional class name(s) applied to the root fieldset element.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}
```

---

## Code Examples

### 1. Basic RadioGroup (Uncontrolled)

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function ShippingOptions() {
  return (
    <RadioGroup
      name="shipping"
      legend="Shipping method"
      defaultValue="standard"
    >
      <Radio value="standard"  label="Standard (3–5 business days)" />
      <Radio value="express"   label="Express (1–2 business days)" />
      <Radio value="overnight" label="Overnight (next business day)" />
    </RadioGroup>
  );
}
```

### 2. Controlled RadioGroup

```tsx
import { useState } from 'react';
import { RadioGroup, Radio } from '@ds/components';

export function PaymentFrequency() {
  const [frequency, setFrequency] = useState('monthly');

  return (
    <RadioGroup
      name="payment-frequency"
      legend="Billing frequency"
      value={frequency}
      onChange={setFrequency}
    >
      <Radio value="monthly"   label="Monthly" />
      <Radio value="quarterly" label="Quarterly (save 5%)" />
      <Radio value="annual"    label="Annual (save 20%)" />
    </RadioGroup>
  );
}
```

### 3. RadioGroup with Validation

```tsx
import { useState } from 'react';
import { RadioGroup, Radio, Button } from '@ds/components';

export function AccountTypeForm() {
  const [accountType, setAccountType] = useState('');
  const [submitted, setSubmitted] = useState(false);
  const hasError = submitted && !accountType;

  return (
    <form onSubmit={e => { e.preventDefault(); setSubmitted(true); }}>
      <RadioGroup
        name="account-type"
        legend="Account type"
        value={accountType}
        onChange={setAccountType}
        validation={hasError}
        validationType="error"
        validationMessage="Please select an account type to continue."
        required
      >
        <Radio value="personal"  label="Personal" />
        <Radio value="business"  label="Business" />
        <Radio value="nonprofit" label="Non-profit" />
      </RadioGroup>

      <Button type="submit" variant="primary" size="large" className="mt-6">
        Continue
      </Button>
    </form>
  );
}
```

### 4. RadioGroup with Descriptions

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function NotificationPreference() {
  return (
    <RadioGroup
      name="notifications"
      legend="Notification preference"
      defaultValue="important"
    >
      <Radio
        value="all"
        label="All activity"
        description="Receive notifications for every transaction and update"
      />
      <Radio
        value="important"
        label="Important only"
        description="Receive notifications for payments, disputes, and security alerts"
      />
      <Radio
        value="none"
        label="None"
        description="No notifications; check the app manually for updates"
      />
    </RadioGroup>
  );
}
```

### 5. RadioGroup with Disabled Options

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function CurrencySelector({ isVerified }: { isVerified: boolean }) {
  return (
    <RadioGroup
      name="currency"
      legend="Default currency"
      defaultValue="USD"
    >
      <Radio value="USD" label="US Dollar (USD)" />
      <Radio value="EUR" label="Euro (EUR)" />
      <Radio value="GBP" label="British Pound (GBP)" />
      <Radio
        value="crypto"
        label="Cryptocurrency"
        disabled={!isVerified}
        description={!isVerified ? 'Identity verification required' : undefined}
      />
    </RadioGroup>
  );
}
```

### 6. Entire Group Disabled

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function LockedPlan({ currentPlan }: { currentPlan: string }) {
  return (
    <RadioGroup
      name="plan"
      legend="Subscription plan"
      value={currentPlan}
      disabled
    >
      <Radio value="free"    label="Free" />
      <Radio value="pro"     label="Pro" />
      <Radio value="premium" label="Premium" />
    </RadioGroup>
  );
}
```

### 7. RadioGroup without Visible Legend (Hidden Legend)

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function GenderSelect() {
  return (
    <div>
      <label className="font-medium mb-2 block">
        Gender <span aria-hidden>*</span>
      </label>
      <RadioGroup
        name="gender"
        legend="Gender"
        hideLegend  // Legend in DOM for screen readers; external visible label
        required
      >
        <Radio value="man"         label="Man" />
        <Radio value="woman"       label="Woman" />
        <Radio value="nonbinary"   label="Non-binary" />
        <Radio value="self-describe" label="I use a different term" />
        <Radio value="prefer-not"  label="Prefer not to say" />
      </RadioGroup>
    </div>
  );
}
```

### 8. Radio without Label (Icon-Only Row)

```tsx
import { RadioGroup, Radio } from '@ds/components';

const COLORS = ['red', 'blue', 'green', 'black'];

export function ColorPicker() {
  return (
    <RadioGroup
      name="color"
      legend="Choose a color"
      hideLegend={false}
    >
      <div className="flex gap-4">
        {COLORS.map(color => (
          <div key={color} className="flex flex-col items-center gap-1">
            <div
              style={{ width: 32, height: 32, background: color, borderRadius: 4 }}
              aria-hidden="true"
            />
            {/* Radio with no label prop — aria-label conveys the meaning */}
            <Radio
              value={color}
              aria-label={color}
            />
          </div>
        ))}
      </div>
    </RadioGroup>
  );
}
```

### 9. Form Integration with React Hook Form

```tsx
import { useForm, Controller } from 'react-hook-form';
import { RadioGroup, Radio, Button } from '@ds/components';

interface FormValues {
  contactMethod: string;
}

export function ContactPreferenceForm() {
  const { control, handleSubmit, formState: { errors } } = useForm<FormValues>();

  return (
    <form onSubmit={handleSubmit(data => console.log(data))}>
      <Controller
        name="contactMethod"
        control={control}
        rules={{ required: 'Please select a contact method' }}
        render={({ field, fieldState }) => (
          <RadioGroup
            name={field.name}
            legend="Preferred contact method"
            value={field.value}
            onChange={field.onChange}
            validation={!!fieldState.error}
            validationType="error"
            validationMessage={fieldState.error?.message}
            required
          >
            <Radio value="email"   label="Email" />
            <Radio value="phone"   label="Phone" />
            <Radio value="sms"     label="Text message" />
          </RadioGroup>
        )}
      />
      <Button type="submit" variant="primary" size="large" className="mt-6">
        Save preferences
      </Button>
    </form>
  );
}
```

### 10. RadioGroup with Contextual Descriptions and Warning

```tsx
import { RadioGroup, Radio } from '@ds/components';

export function DataRetentionPolicy() {
  const [retention, setRetention] = useState('90-days');
  const isShort = retention === '30-days';

  return (
    <RadioGroup
      name="data-retention"
      legend="Transaction history retention"
      value={retention}
      onChange={setRetention}
      validation={isShort}
      validationType="warning"
      validationMessage="Short retention may limit dispute resolution options."
    >
      <Radio value="30-days"  label="30 days"  description="Minimal storage" />
      <Radio value="90-days"  label="90 days"  description="Recommended for most users" />
      <Radio value="1-year"   label="1 year"   description="Required for business accounts" />
      <Radio value="forever"  label="Keep all" description="Full transaction history" />
    </RadioGroup>
  );
}
```

---

## Accessibility Specifications

### Semantic HTML

Always use a native `<input type="radio">` inside a `<label>`, grouped within a `<fieldset>` and `<legend>`:

```html
<fieldset>
  <legend>Shipping method</legend>

  <label>
    <input type="radio" name="shipping" value="standard" />
    Standard (3–5 business days)
  </label>

  <label>
    <input type="radio" name="shipping" value="express" checked />
    Express (1–2 business days)
  </label>

  <label>
    <input type="radio" name="shipping" value="overnight" />
    Overnight (next business day)
  </label>

  <!-- Validation message (error state) -->
  <div id="shipping-error" role="alert" aria-live="polite">
    <svg aria-hidden="true"><!-- alert icon --></svg>
    Please select a shipping method to continue.
  </div>
</fieldset>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | `<fieldset>` | Implicit `group` role via native fieldset |
| `aria-required` | `<fieldset>` | `"true"` when group is required |
| `aria-describedby` | `<fieldset>` | ID of the validation message element |
| `aria-invalid` | `<fieldset>` | Not standard; use `aria-errormessage` instead |
| `aria-errormessage` | `<fieldset>` | ID of the error message element |
| `aria-disabled` | `<fieldset>` | `"true"` for a fully disabled group |
| `aria-disabled` | `<input>` | `"true"` for individually disabled items |
| `aria-label` | `<input>` (no label) | Text label when no visible `<label>` is present |
| `aria-describedby` | `<input>` | ID of an optional description paragraph |

> **Note:** Do not use the HTML `disabled` attribute on individual inputs inside a `<fieldset>` that has its own `disabled`; instead, use `aria-disabled` and handle it in JS to preserve discoverability.

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| `Tab` | Moves focus into the radio group (to the first or previously selected radio) |
| `Tab` (within group) | Moves focus **out** of the group to the next focusable element |
| `ArrowDown` / `ArrowRight` | Moves selection to the next radio in the group; wraps to first |
| `ArrowUp` / `ArrowLeft` | Moves selection to the previous radio; wraps to last |
| `Space` | Selects the focused radio (if not already selected) |

> **Important:** Within a radio group, only **one** radio receives `tabindex="0"` at a time (roving tabindex). All others have `tabindex="-1"`. Tab moves in/out of the group; arrow keys move within it.

### Focus Management

```tsx
// Roving tabindex implementation
const [focusedIndex, setFocusedIndex] = useState(
  options.findIndex(o => o.value === value) || 0
);

const handleKeyDown = (e: KeyboardEvent, index: number) => {
  let next = index;
  if (e.key === 'ArrowDown' || e.key === 'ArrowRight') {
    next = (index + 1) % options.length;
  } else if (e.key === 'ArrowUp' || e.key === 'ArrowLeft') {
    next = (index - 1 + options.length) % options.length;
  } else {
    return;
  }
  e.preventDefault();
  setFocusedIndex(next);
  onChange(options[next].value);
  radioRefs[next].current?.focus();
};
```

### Screen Reader Announcements

When a radio is selected, screen readers announce: _"[Label], radio button, selected, [n] of [total]"_

When focus moves to an unselected radio (via arrow key), it announces: _"[Label], radio button, not selected"_

The group legend is read when entering the group: _"[Legend], group"_

Validation errors are announced via `role="alert"` or `aria-live="polite"`:

```html
<div id="group-error" role="alert">
  Please select an account type to continue.
</div>
```

### Touch Targets

The minimum touch target for the radio+label row must be at least 44 × 44 px (WCAG 2.5.5). Achieve this by applying `min-height: 44px` to each `<label>` row and vertical padding:

```css
.radio-label {
  display: flex;
  align-items: center;
  gap: 8px;
  min-height: 44px;
  cursor: pointer;
}
```

### Color Contrast

| Element | Ratio | WCAG |
|---------|-------|------|
| Radio border on white background | 3:1 | 1.4.11 Non-text Contrast |
| Inner dot (checked) on white | 3:1 | 1.4.11 Non-text Contrast |
| Label text | 4.5:1 | 1.4.3 Contrast (Minimum) |
| Focus ring | 3:1 | 1.4.11 Non-text Contrast |
| Error ring color on white | 3:1 | 1.4.11 Non-text Contrast |
| Validation message text | 4.5:1 | 1.4.3 Contrast (Minimum) |

---

## Animation Specifications

### Selection Transition (Dot Appear)

```css
@keyframes radio-dot-in {
  from {
    transform: scale(0);
    opacity: 0;
  }
  to {
    transform: scale(1);
    opacity: 1;
  }
}

.radio-inner-dot {
  transform-origin: center;
  animation: radio-dot-in 150ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Deselection Transition (Dot Disappear)

```css
@keyframes radio-dot-out {
  from { transform: scale(1); opacity: 1; }
  to   { transform: scale(0); opacity: 0; }
}

.radio-inner-dot--leaving {
  animation: radio-dot-out 100ms ease-in forwards;
}
```

### Focus Ring

```css
.radio-input:focus-visible + .radio-circle {
  outline: 2px solid var(--tokens.colorScheme.border.focus);
  outline-offset: 2px;
}
```

### Hover

```css
.radio-label:hover .radio-circle {
  background: var(--tokens.colorScheme.background.hover);
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .radio-inner-dot,
  .radio-inner-dot--leaving {
    animation: none;
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Control Circle ──────────────────────────────────────────────────── */
--radio-size:                  20px;
--radio-border-width:          2px;
--radio-border-color:          var(--tokens.colorScheme.content.base);         /* black */
--radio-border-color-error:    var(--tokens.colorScheme.status.negative);      /* red */
--radio-border-color-disabled: var(--tokens.colorScheme.content.disabled);
--radio-bg:                    var(--tokens.colorScheme.background.default);   /* white */
--radio-bg-hover:              var(--tokens.colorScheme.background.hover);
--radio-dot-size:              8px;
--radio-dot-color:             var(--tokens.colorScheme.content.base);         /* black */
--radio-dot-color-disabled:    var(--tokens.colorScheme.content.disabled);

/* ─── Label ───────────────────────────────────────────────────────────── */
--radio-label-family:          var(--tokens.brand.text.fontfamily.body);
--radio-label-size:            var(--tokens.base.text.fontsize.ramp[16]);      /* 16px */
--radio-label-weight:          var(--tokens.brand.text.fontweight.body);       /* Regular */
--radio-label-line-height:     var(--tokens.base.text.lineheight.ramp[16]);    /* 24px */
--radio-label-color:           var(--tokens.colorScheme.content.base);
--radio-label-color-disabled:  var(--tokens.colorScheme.content.disabled);
--radio-label-gap:             var(--tokens.base.spacing[8]);                  /* 8px */

/* ─── RadioGroup ──────────────────────────────────────────────────────── */
--radio-group-width:           370px;
--radio-group-item-gap:        var(--tokens.base.spacing[24]);                 /* 24px */
--radio-group-validation-gap:  var(--tokens.base.spacing[8]);                  /* 8px */

/* ─── Validation Message ──────────────────────────────────────────────── */
--radio-validation-icon-size:  16px;
--radio-validation-icon-gap:   var(--tokens.base.spacing[4]);                  /* 4px */
--radio-validation-font-size:  var(--tokens.base.text.fontsize.ramp[12]);      /* 12px */
--radio-validation-line-height:var(--tokens.base.text.lineheight.ramp[12]);    /* 16px */
--radio-validation-color:      var(--tokens.colorScheme.content.muted);        /* rgba(0,0,0,0.63) */
```

---

## Best Practices

### Do

- **Always wrap Radio buttons in a `RadioGroup` with a `legend`** — Without a `<fieldset>/<legend>`, screen readers cannot convey that the options are part of a related set. Even if the legend is visually hidden, it must be in the DOM.
- **Use Radio for mutually exclusive choices** — Radio buttons are for "pick exactly one" scenarios. For "pick one or more", use Checkboxes instead.
- **Pre-select a default option when there is a clear sensible default** — Leaving all radios unchecked forces the user to make a decision they may not have opinions on. Pre-select the most common or recommended option.
- **Order options logically** — Use a natural ordering: most-to-least common, alphabetical, or logical sequence (e.g., chronological for date ranges). Don't bury the most popular option at the bottom.
- **Keep labels short and distinct** — Each label should be scannable at a glance and unambiguous without reading the others.
- **Show the validation message inline below the group** — Not in a toast or modal. The user needs to see the error and the field together.
- **Provide descriptions for complex options** — When a label alone is insufficient, add a `description` below each Radio item for additional context.

### Don't

- **Don't use Radio for binary yes/no decisions** — Use a Switch or a single Checkbox for on/off toggles.
- **Don't disable an option without explaining why** — A disabled Radio gives no indication of why it's unavailable. Add a `description` explaining the condition, or remove the option and explain it elsewhere.
- **Don't use more than ~7 options in a radio group** — Beyond 7, a Select dropdown is easier to scan and use. Consider splitting the group or using a different control.
- **Don't pre-select a "None" or blank option just to have a default** — If no option is appropriate as a default, leave the group unselected and add proper required-field validation.
- **Don't use color as the only error indicator** — The radio ring turning red must be paired with a visible, descriptive validation message. Rely on text, not only color.
- **Don't implement Tab navigation between radios within a group** — Arrow keys move between radios; Tab exits the group. Implementing Tab-within-group breaks the native radio keyboard contract and confuses screen reader users.
- **Don't nest RadioGroups** — Radio groups should be flat. Nested groups create deeply confusing `<fieldset>` semantics.

---

## Radio vs. Related Components

| Component | When to Use |
|-----------|------------|
| **Radio / RadioGroup** | Mutually exclusive selection from 2–7 options; all options are always visible |
| **Checkbox** | One or more selections from a set; each item is independently toggleable |
| **Switch** | Single binary on/off toggle; not part of a selection group |
| **Select (Dropdown)** | Mutually exclusive selection from 8+ options; space-constrained; options don't need to be compared |
| **Button Group (Segmented Control)** | Mutually exclusive mode/view switching; options are very short (1–3 words); space is limited |
| **List (Selectable)** | Mutually exclusive selection where each option has rich content (icon, description, metadata) |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Checkbox** | Sibling form control for multi-selection; shares similar visual and API patterns |
| **Switch** | Binary toggle; not a radio but often used in the same form contexts |
| **Select** | Alternative for large option sets or space-constrained layouts |
| **Button Group** | Alternative for short, visually prominent option switching |
| **Contextual Alert** | The validation message sub-pattern used below the RadioGroup |
| **Input** | Sibling form control; RadioGroup often appears in the same form alongside Input fields |
| **Legal Consent** | Often uses Radio or Checkbox to capture consent choices |
| **Form / Fieldset** | Semantic container that RadioGroup renders within |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Extracted `RadioGroup` as a first-class component with `validation`, `validationMessage`, `validationType` props; standardised item gap to 24 px; added `description` prop on individual Radio items |
| 5.1 | Added `disabled` prop on individual Radio items (previously only group-level) |
| 5.0 | Added `validation` prop and Contextual Alert sub-pattern for error messages |
| 4.0 | Initial release |

---

## Testing Checklist

### Visual
- [ ] Unchecked Radio renders outer ring only (no inner dot)
- [ ] Checked Radio renders outer ring + 8 px inner dot
- [ ] Ring diameter is 20 × 20 px; inner dot is 8 × 8 px
- [ ] Label renders in 16 px Body/Regular with 8 px gap from circle
- [ ] RadioGroup items are spaced 24 px apart vertically
- [ ] Validation message renders below list with 8 px gap (icon + 4 px gap + 12 px text)
- [ ] Disabled Radio renders in muted/reduced-opacity color
- [ ] Error Radio renders with red (`status.negative`) ring border
- [ ] Dot appear/disappear animation plays on selection change

### Behavior
- [ ] Only one Radio in a group can be selected at a time
- [ ] Selecting a new Radio deselects the previous one
- [ ] Uncontrolled mode manages selection internally
- [ ] Controlled mode requires `value` + `onChange` to update
- [ ] Disabled Radio cannot be clicked or keyboard-selected
- [ ] Disabled group prevents interaction on all child radios
- [ ] `validation={true}` reveals the validation message
- [ ] `validationType` changes the icon and text color of the message

### Keyboard Navigation
- [ ] Tab enters the group (to selected radio or first if none selected)
- [ ] Tab exits the group to the next focusable element
- [ ] ArrowDown / ArrowRight moves to and selects the next radio
- [ ] ArrowUp / ArrowLeft moves to and selects the previous radio
- [ ] Arrow key wraps from last to first and first to last
- [ ] Space selects the focused radio
- [ ] Disabled radios are skipped by arrow key navigation

### Accessibility
- [ ] `<fieldset>` and `<legend>` structure is present in rendered HTML
- [ ] Hidden legend (`hideLegend`) is visually hidden but in the DOM
- [ ] `aria-required="true"` on `<fieldset>` when `required` is set
- [ ] `aria-describedby` on `<fieldset>` references the validation message element
- [ ] Validation message uses `role="alert"` for error; `aria-live="polite"` for others
- [ ] Each `<input>` has an associated `<label>` (via wrapping or `for`/`id`)
- [ ] Radio without label has `aria-label` set
- [ ] `aria-disabled="true"` on disabled radios (not just HTML `disabled`)
- [ ] Focus ring is visible on keyboard focus (`:focus-visible`)
- [ ] Screen reader announces group legend when entering group
- [ ] Selection change is announced by screen reader
- [ ] Touch targets meet 44 × 44 px minimum

### Form Integration
- [ ] React Hook Form `Controller` wraps RadioGroup correctly
- [ ] `name` prop is shared between RadioGroup and all child Radio inputs
- [ ] Form submit includes the selected radio value
- [ ] `required` validation fires when no option is selected on submit
