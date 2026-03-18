# Switch Component Documentation

**Component:** Switch
**Version:** 6.0 Beta
**Figma Node:** `70-14`
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

The **Switch** component is a binary toggle control that allows users to turn a setting or feature on or off. It represents an immediate, in-place state change with no confirmation step required. The Switch communicates its state visually through track color, thumb position, and — when checked — a checkmark icon inside the thumb.

### When to Use

- Toggling application settings on or off (e.g., notifications, dark mode, location access)
- Enabling or disabling individual features within a configuration panel
- Any binary on/off decision that takes effect immediately without requiring form submission
- Mobile settings screens where a familiar toggle control is expected by users

### When Not to Use

- When the change requires a confirmation step before taking effect — use a Checkbox inside a form with a submit action instead
- When selecting one option from a group of options — use Radio buttons or a Segmented Control
- When the choice is part of a multi-item form that is submitted as a unit — use Checkbox
- When three or more states are needed — use a Select or Segmented Control

### Switch vs. Checkbox

| Attribute | Switch | Checkbox |
|---|---|---|
| Semantics | On / Off | Selected / Deselected |
| Effect | Immediate (no submit) | Deferred (requires submit) |
| Context | Settings, feature toggles | Forms, multi-select lists |
| ARIA role | `switch` | `checkbox` |

---

## Component Variants

| Variant | Figma Node | `checked` | Description |
|---|---|---|---|
| Checked (ON) | `70:15` | `true` | Dark track + white thumb with checkmark icon |
| Unchecked (OFF) | `70:17` | `false` | Grey track + white thumb, no checkmark |

---

## Anatomy

### Checked (ON) State

```
┌────────────────────────┐
│  ██████████████ (●✓)  │
│       Track      Thumb │
└────────────────────────┘
```

### Unchecked (OFF) State

```
┌────────────────────────┐
│  (●) ░░░░░░░░░░░░░░░  │
│  Thumb      Track      │
└────────────────────────┘
```

### Part Descriptions

| Part | Element | Description |
|---|---|---|
| **Track** | `<button role="switch">` | Pill-shaped container that changes color based on checked state |
| **Thumb** | `<span>` (child of track) | White circular handle that slides between left (OFF) and right (ON) positions |
| **Checkmark Icon** | `<Icon name="checkmark">` (inside thumb) | Displayed inside the thumb only when `checked=true`; hidden when `checked=false` |

---

## Size & Spacing Specifications

### Track Dimensions

| Property | Value | Token |
|---|---|---|
| Track width | 51px | `--switch-track-width: 51px` |
| Track height | 31px | `--switch-track-height: 31px` |
| Track border-radius | 9999px (full pill) | `--switch-track-radius: 9999px` |
| Track border | None | — |

### Thumb Dimensions

| Property | Value | Token |
|---|---|---|
| Thumb diameter | 27px | `--switch-thumb-size: 27px` |
| Thumb border-radius | 50% | — |
| Thumb inset (padding within track) | 2px | `--switch-thumb-inset: 2px` |
| Thumb shadow | Elevation / Level 1 (dual drop-shadow) | `--elevation-level-1` |

### Thumb Position

| State | Thumb X Position | Notes |
|---|---|---|
| OFF (unchecked) | `left: 2px` | Thumb sits at the left end of the track |
| ON (checked) | `left: calc(100% - thumb-size - 2px)` = `right: 2px` | Thumb sits at the right end of the track |

### Checkmark Icon

| Property | Value |
|---|---|
| Icon size | 16px × 16px |
| Icon name | `checkmark` (from PDSIcon / system icon set) |
| Icon color | `--color-brand-on-primary` (white) |
| Visibility | Present only when `checked=true` |

### Touch Target

| Property | Value |
|---|---|
| Minimum touch target | 44×44px |
| Implementation | `min-height: 44px` on the root or `::before` expansion on the track |

### Label Spacing (when paired with a label)

| Property | Value |
|---|---|
| Switch → Label gap | 12px |
| Label font size | 16px (Body / Default) |
| Label font weight | 400 (Regular) |
| Description text font size | 14px (Body / Small) |
| Description text color | `--color-text-muted` |
| Label → Description gap | 2px |

---

## Style Variants

### Track Colors

| State | Track Background | Token |
|---|---|---|
| Checked (ON) | `--color-neutral-900` (near-black / brand dark) | `--switch-track-on-color` |
| Unchecked (OFF) | `--color-neutral-300` (mid grey) | `--switch-track-off-color` |
| Checked + Disabled | `--color-neutral-400` (muted dark) | `--switch-track-on-disabled-color` |
| Unchecked + Disabled | `--color-neutral-200` (light grey) | `--switch-track-off-disabled-color` |

### Thumb Colors

| State | Thumb Background | Token |
|---|---|---|
| All (enabled) | `--color-surface-default` (white) | `--switch-thumb-color` |
| Disabled | `--color-surface-default` (white, opacity reduced) | `--switch-thumb-disabled-color` |

### Elevation / Level 1 Shadow (Thumb)

```css
/* Elevation / Level 1 */
box-shadow:
  0px 1px 2px rgba(0, 0, 0, 0.12),
  0px 0px 1px rgba(0, 0, 0, 0.08);
```

---

## State Specifications

### Unchecked / OFF (Default)

- Track: grey (`--color-neutral-300`)
- Thumb: white, positioned at `left: 2px`
- No checkmark icon inside thumb
- `aria-checked="false"`

### Checked / ON

- Track: dark (`--color-neutral-900`)
- Thumb: white, positioned at `right: 2px`
- Checkmark icon visible inside thumb
- `aria-checked="true"`

### Hover (Enabled)

- Cursor: `pointer`
- Track may apply a subtle brightness/opacity overlay to indicate interactivity
- No dimension changes

### Focus (Keyboard)

- 2px focus ring around the track (`--color-focus-ring`, brand blue)
- Focus ring offset: 2px
- Focus ring border-radius matches track radius (pill)

### Pressed / Active

- Thumb may elongate slightly (pill stretch) during press — a fluid animation feedback
- Returns to circular shape on release

### Disabled

- Track: muted color (on or off variant)
- Thumb: white with reduced opacity on the overall component (`opacity: 0.4`)
- `cursor: not-allowed`
- `aria-disabled="true"` (or `disabled` attribute on native button)
- Not interactive; no hover/focus states

---

## Props API

### Web / React

```typescript
interface SwitchProps {
  /**
   * Whether the switch is in the ON (checked) state.
   * @default false
   */
  checked: boolean;

  /**
   * Callback fired when the user toggles the switch.
   * Receives the new checked state as the argument.
   */
  onCheckedChange: (checked: boolean) => void;

  /** Whether the switch is disabled and non-interactive. */
  disabled?: boolean;

  /**
   * Accessible label for the switch.
   * Required if no visible label element is associated via `aria-labelledby`.
   */
  'aria-label'?: string;

  /** ID of a visible label element that names this switch. */
  'aria-labelledby'?: string;

  /** ID of a description element for additional context. */
  'aria-describedby'?: string;

  /** HTML id attribute for the switch element. */
  id?: string;

  /** Additional CSS class applied to the root element. */
  className?: string;

  /** Inline style overrides. */
  style?: React.CSSProperties;
}
```

### Android — Jetpack Compose

```kotlin
@Composable
fun Switch(
    checked: Boolean,
    onCheckedChange: ((Boolean) -> Unit)?,
    modifier: Modifier = Modifier,
    thumbContent: (@Composable () -> Unit)? = null,
    enabled: Boolean = true,
    colors: SwitchColors = SwitchDefaults.colors(),
    interactionSource: MutableInteractionSource = remember { MutableInteractionSource() }
)
```

**Checked (with checkmark icon):**

```kotlin
Switch(
    checked = true,
    onCheckedChange = { /* handle change */ },
    thumbContent = {
        Icon(
            imageVector = Icons.Filled.Check,
            contentDescription = null,
            modifier = Modifier.size(SwitchDefaults.IconSize)
        )
    }
)
```

**Unchecked (no thumb content):**

```kotlin
Switch(
    checked = false,
    onCheckedChange = { /* handle change */ }
)
```

### iOS — SwiftUI

The Switch maps to the native `Toggle` in SwiftUI, styled to match the design system:

```swift
Toggle(isOn: $isEnabled) {
    Text("Enable Notifications")
}
.toggleStyle(PDSSwitchStyle()) // Design system custom style
```

---

## Code Examples

### 1. Basic Controlled Switch

```tsx
import { useState } from 'react';
import { Switch } from '@design-system/components';

export function NotificationsToggle() {
  const [enabled, setEnabled] = useState(false);

  return (
    <Switch
      checked={enabled}
      onCheckedChange={setEnabled}
      aria-label="Enable notifications"
    />
  );
}
```

---

### 2. Switch with Visible Label

```tsx
import { useState } from 'react';
import { Switch } from '@design-system/components';

export function LabeledSwitch() {
  const [enabled, setEnabled] = useState(false);

  return (
    <div style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
      <label htmlFor="dark-mode-switch" style={{ fontSize: 16 }}>
        Dark Mode
      </label>
      <Switch
        id="dark-mode-switch"
        checked={enabled}
        onCheckedChange={setEnabled}
      />
    </div>
  );
}
```

---

### 3. Switch with Label and Description (Settings Row)

```tsx
import { useState } from 'react';
import { Switch } from '@design-system/components';

export function SettingRow() {
  const [locationEnabled, setLocationEnabled] = useState(true);

  return (
    <div
      style={{
        display: 'flex',
        justifyContent: 'space-between',
        alignItems: 'center',
        padding: '12px 0',
      }}
    >
      <div style={{ display: 'flex', flexDirection: 'column', gap: 2 }}>
        <label
          htmlFor="location-switch"
          style={{ fontSize: 16, fontWeight: 400 }}
        >
          Location Services
        </label>
        <span
          id="location-desc"
          style={{ fontSize: 14, color: 'var(--color-text-muted)' }}
        >
          Allow app to access your location while in use
        </span>
      </div>
      <Switch
        id="location-switch"
        checked={locationEnabled}
        onCheckedChange={setLocationEnabled}
        aria-describedby="location-desc"
      />
    </div>
  );
}
```

---

### 4. Disabled Switch

```tsx
import { Switch } from '@design-system/components';

export function DisabledSwitch() {
  return (
    <div style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
      <label htmlFor="premium-switch" style={{ color: 'var(--color-text-disabled)' }}>
        Premium Feature (Upgrade to enable)
      </label>
      <Switch
        id="premium-switch"
        checked={false}
        onCheckedChange={() => {}}
        disabled
      />
    </div>
  );
}
```

---

### 5. Settings Panel with Multiple Switches

```tsx
import { useState } from 'react';
import { Switch } from '@design-system/components';

interface NotificationSettings {
  email: boolean;
  push: boolean;
  sms: boolean;
}

export function NotificationSettings() {
  const [settings, setSettings] = useState<NotificationSettings>({
    email: true,
    push: false,
    sms: true,
  });

  const toggle = (key: keyof NotificationSettings) => {
    setSettings((prev) => ({ ...prev, [key]: !prev[key] }));
  };

  const rows: { key: keyof NotificationSettings; label: string; description: string }[] = [
    { key: 'email', label: 'Email Notifications', description: 'Receive updates via email' },
    { key: 'push', label: 'Push Notifications', description: 'Alerts on your device' },
    { key: 'sms', label: 'SMS Notifications', description: 'Text messages for critical alerts' },
  ];

  return (
    <section aria-labelledby="notifications-heading">
      <h2 id="notifications-heading">Notifications</h2>
      <ul role="list" style={{ listStyle: 'none', padding: 0, margin: 0 }}>
        {rows.map(({ key, label, description }) => (
          <li
            key={key}
            style={{
              display: 'flex',
              justifyContent: 'space-between',
              alignItems: 'center',
              padding: '14px 0',
              borderBottom: '1px solid var(--color-border-subtle)',
            }}
          >
            <div style={{ display: 'flex', flexDirection: 'column', gap: 2 }}>
              <label htmlFor={`${key}-switch`} style={{ fontSize: 16 }}>{label}</label>
              <span
                id={`${key}-desc`}
                style={{ fontSize: 14, color: 'var(--color-text-muted)' }}
              >
                {description}
              </span>
            </div>
            <Switch
              id={`${key}-switch`}
              checked={settings[key]}
              onCheckedChange={() => toggle(key)}
              aria-describedby={`${key}-desc`}
            />
          </li>
        ))}
      </ul>
    </section>
  );
}
```

---

### 6. Controlled Switch with Async Side Effect

Use `onCheckedChange` to immediately optimistically update, then revert on error:

```tsx
import { useState } from 'react';
import { Switch } from '@design-system/components';

export function AsyncToggle({ featureId }: { featureId: string }) {
  const [enabled, setEnabled] = useState(false);
  const [updating, setUpdating] = useState(false);

  const handleToggle = async (newValue: boolean) => {
    setEnabled(newValue);    // Optimistic update
    setUpdating(true);
    try {
      await updateFeatureFlag(featureId, newValue);
    } catch {
      setEnabled(!newValue); // Revert on failure
    } finally {
      setUpdating(false);
    }
  };

  return (
    <Switch
      checked={enabled}
      onCheckedChange={handleToggle}
      disabled={updating}
      aria-label="Enable beta feature"
    />
  );
}
```

---

### 7. React Hook Form Integration

```tsx
import { Controller, useForm } from 'react-hook-form';
import { Switch } from '@design-system/components';

interface PrivacySettings {
  shareData: boolean;
  marketingEmails: boolean;
}

export function PrivacyForm() {
  const { control, handleSubmit } = useForm<PrivacySettings>({
    defaultValues: { shareData: false, marketingEmails: false },
  });

  return (
    <form onSubmit={handleSubmit(console.log)}>
      <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 16 }}>
        <label htmlFor="share-data">Share usage data</label>
        <Controller
          name="shareData"
          control={control}
          render={({ field }) => (
            <Switch
              id="share-data"
              checked={field.value}
              onCheckedChange={field.onChange}
            />
          )}
        />
      </div>

      <div style={{ display: 'flex', justifyContent: 'space-between', marginBottom: 24 }}>
        <label htmlFor="marketing-emails">Marketing emails</label>
        <Controller
          name="marketingEmails"
          control={control}
          render={({ field }) => (
            <Switch
              id="marketing-emails"
              checked={field.value}
              onCheckedChange={field.onChange}
            />
          )}
        />
      </div>

      <button type="submit">Save preferences</button>
    </form>
  );
}
```

---

### 8. Compose — Switch with Checkmark (ON) and Plain (OFF)

```kotlin
@Composable
fun FeatureToggle(
    label: String,
    checked: Boolean,
    onCheckedChange: (Boolean) -> Unit
) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 12.dp),
        horizontalArrangement = Arrangement.SpaceBetween,
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(text = label, style = MaterialTheme.typography.bodyLarge)

        Switch(
            checked = checked,
            onCheckedChange = onCheckedChange,
            thumbContent = if (checked) {
                {
                    Icon(
                        imageVector = Icons.Filled.Check,
                        contentDescription = null,
                        modifier = Modifier.size(SwitchDefaults.IconSize)
                    )
                }
            } else null
        )
    }
}
```

---

### 9. SwiftUI — Settings List Row

```swift
struct SettingsToggleRow: View {
    let title: String
    let description: String
    @Binding var isOn: Bool

    var body: some View {
        HStack {
            VStack(alignment: .leading, spacing: 2) {
                Text(title)
                    .font(.body)
                Text(description)
                    .font(.caption)
                    .foregroundColor(.secondary)
            }
            Spacer()
            Toggle("", isOn: $isOn)
                .labelsHidden()
                .toggleStyle(PDSSwitchStyle())
        }
        .padding(.vertical, 12)
    }
}
```

---

### 10. Fully Accessible Standalone Switch (Custom HTML)

For contexts where the design system component is not available, the correct semantic HTML implementation:

```tsx
export function AccessibleSwitch({
  checked,
  onChange,
  label,
  description,
  id,
}: {
  checked: boolean;
  onChange: (val: boolean) => void;
  label: string;
  description?: string;
  id: string;
}) {
  const descId = description ? `${id}-desc` : undefined;

  return (
    <div style={{ display: 'flex', justifyContent: 'space-between', alignItems: 'center' }}>
      <div>
        <label htmlFor={id}>{label}</label>
        {description && (
          <p id={descId} style={{ fontSize: 14, color: 'var(--color-text-muted)', margin: '2px 0 0' }}>
            {description}
          </p>
        )}
      </div>

      <button
        id={id}
        type="button"
        role="switch"
        aria-checked={checked}
        aria-describedby={descId}
        onClick={() => onChange(!checked)}
        style={{
          width: 51,
          height: 31,
          borderRadius: 9999,
          border: 'none',
          cursor: 'pointer',
          backgroundColor: checked ? 'var(--switch-track-on-color)' : 'var(--switch-track-off-color)',
          position: 'relative',
          transition: 'background-color 200ms ease',
          flexShrink: 0,
        }}
      >
        <span
          style={{
            position: 'absolute',
            top: 2,
            left: checked ? 'calc(100% - 29px)' : 2,
            width: 27,
            height: 27,
            borderRadius: '50%',
            backgroundColor: 'white',
            transition: 'left 200ms ease',
            display: 'flex',
            alignItems: 'center',
            justifyContent: 'center',
            boxShadow: '0px 1px 2px rgba(0,0,0,0.12), 0px 0px 1px rgba(0,0,0,0.08)',
          }}
          aria-hidden="true"
        >
          {checked && (
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
              <path
                d="M3.5 8L6.5 11L12.5 5"
                stroke="var(--color-brand-on-primary)"
                strokeWidth="1.5"
                strokeLinecap="round"
                strokeLinejoin="round"
              />
            </svg>
          )}
        </span>
        <span className="sr-only">{checked ? 'On' : 'Off'}</span>
      </button>
    </div>
  );
}
```

---

## Accessibility Specifications

### ARIA Role

The Switch uses `role="switch"` — a specialization of `role="checkbox"` that specifically represents a binary on/off control:

```html
<button
  type="button"
  role="switch"
  aria-checked="true"
  aria-labelledby="switch-label"
  aria-describedby="switch-desc"
>
  <!-- thumb + checkmark -->
</button>
```

| ARIA Attribute | Required | Value | Description |
|---|---|---|---|
| `role="switch"` | ✅ | — | Identifies the element as an on/off toggle |
| `aria-checked` | ✅ | `"true"` / `"false"` | Reflects the current on/off state |
| `aria-label` or `aria-labelledby` | ✅ | — | Associates the switch with its human-readable name |
| `aria-describedby` | Optional | — | Points to a description element for additional context |
| `aria-disabled` | Conditional | `"true"` | Applied when the switch is disabled |

> **Note on `aria-checked`:** Unlike `role="checkbox"` which supports a `"mixed"` (indeterminate) state, `role="switch"` only supports `"true"` and `"false"`. Never set `aria-checked="mixed"` on a Switch.

### Keyboard Navigation

| Key | Action |
|---|---|
| `Tab` | Move focus to the switch |
| `Shift + Tab` | Move focus away |
| `Space` | Toggle the switch (ON → OFF or OFF → ON) |
| `Enter` | Toggle the switch (same as Space; both are standard for `role="switch"`) |

### Labeling Requirements

Every Switch must have an accessible name. The preferred approach (in priority order):

1. **Visible label with `htmlFor` / `aria-labelledby`** — best for discoverability
2. **`aria-label`** — only when no visible label exists (e.g., icon-only contexts)
3. **Never unlabeled** — a switch without an accessible name is a WCAG 4.1.2 violation

```html
<!-- Preferred: visible label -->
<label id="lbl-dark">Dark Mode</label>
<button role="switch" aria-checked="true" aria-labelledby="lbl-dark">…</button>

<!-- Acceptable: aria-label only -->
<button role="switch" aria-checked="false" aria-label="Enable dark mode">…</button>
```

### State Announcement

Screen readers will announce the Switch state using patterns such as:
- **NVDA + Chrome:** "Dark Mode, switch, on" / "Dark Mode, switch, off"
- **VoiceOver + Safari:** "Dark Mode, on, switch" / "Dark Mode, off, switch"
- **TalkBack (Android):** "Dark Mode, toggle button, on" / "… off"

The visual "On" / "Off" state text should **not** be announced separately — the `aria-checked` attribute handles this. Do not add visible "On"/"Off" text labels unless the design explicitly calls for them, and if you do, mark them `aria-hidden="true"` to avoid double announcement.

### Checkmark Icon

The checkmark icon inside the thumb is purely decorative and must carry `aria-hidden="true"`. It provides visual reinforcement but must not add redundant screen reader output.

```html
<span aria-hidden="true" class="switch-checkmark">
  <!-- checkmark SVG or icon -->
</span>
```

### Focus Management

- Focus ring: 2px solid `--color-focus-ring` applied to the track element, `outline-offset: 2px`
- The focus ring border-radius must match the track's pill shape (9999px) so it wraps the full component
- Only the track element receives focus — the thumb is not separately focusable
- When disabled, `tabindex="-1"` is set and the switch is skipped during Tab navigation

### Contrast Requirements

| Element | Minimum Contrast | Requirement |
|---|---|---|
| ON track vs. background | 3:1 (non-text, WCAG 1.4.11) | Track must be distinguishable |
| OFF track vs. background | 3:1 | — |
| Checkmark icon vs. thumb background | 3:1 | Icon on white thumb |
| Focus ring vs. adjacent colors | 3:1 | — |
| Label text vs. background | 4.5:1 (text, WCAG 1.4.3) | — |

---

## Animation Specifications

### Thumb Slide

The thumb translates horizontally between OFF (left) and ON (right) positions on toggle:

```css
.switch-thumb {
  transition: left 200ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

| Property | Value |
|---|---|
| Duration | 200ms |
| Easing | `cubic-bezier(0.4, 0, 0.2, 1)` (Material standard easing) |
| Property | `left` (or `transform: translateX`) |

### Track Color Transition

The track background color transitions simultaneously with the thumb slide:

```css
.switch-track {
  transition: background-color 200ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Checkmark Icon Fade

The checkmark inside the thumb fades in when transitioning to ON and fades out when transitioning to OFF:

```css
.switch-checkmark {
  opacity: 0;
  transition: opacity 150ms ease 50ms; /* slight delay so thumb arrives before icon appears */
}

.switch-track[aria-checked="true"] .switch-checkmark {
  opacity: 1;
}
```

### Thumb Press Elongation (Optional)

On press, the thumb stretches into a slightly wider pill shape to provide tactile feedback — then snaps back on release:

```css
.switch-thumb {
  transition: left 200ms ease, width 100ms ease;
}

.switch-track:active .switch-thumb {
  width: 31px; /* slightly wider than rest 27px */
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .switch-track,
  .switch-thumb,
  .switch-checkmark {
    transition: none;
  }
}
```

---

## Design Tokens

### Sizing Tokens

| Token | Value | Usage |
|---|---|---|
| `--switch-track-width` | `51px` | Track horizontal dimension |
| `--switch-track-height` | `31px` | Track vertical dimension |
| `--switch-track-radius` | `9999px` | Track border-radius (full pill) |
| `--switch-thumb-size` | `27px` | Thumb diameter |
| `--switch-thumb-inset` | `2px` | Thumb padding within track |
| `--switch-icon-size` | `16px` | Checkmark icon size |

### Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--switch-track-on-color` | `--color-neutral-900` | Track fill when ON |
| `--switch-track-off-color` | `--color-neutral-300` | Track fill when OFF |
| `--switch-track-on-disabled-color` | `--color-neutral-400` | Track fill when ON + disabled |
| `--switch-track-off-disabled-color` | `--color-neutral-200` | Track fill when OFF + disabled |
| `--switch-thumb-color` | `--color-surface-default` (white) | Thumb background |
| `--switch-icon-color` | `--color-brand-on-primary` (white) | Checkmark icon color |

### Elevation Token

| Token | Value | Usage |
|---|---|---|
| `--elevation-level-1` | `0px 1px 2px rgba(0,0,0,0.12), 0px 0px 1px rgba(0,0,0,0.08)` | Thumb drop shadow |

### Animation Tokens

| Token | Value | Usage |
|---|---|---|
| `--switch-transition-duration` | `200ms` | Thumb slide + track color duration |
| `--switch-transition-easing` | `cubic-bezier(0.4, 0, 0.2, 1)` | Standard easing |
| `--switch-icon-fade-duration` | `150ms` | Checkmark fade duration |
| `--switch-icon-fade-delay` | `50ms` | Delay before checkmark fades in |

---

## Best Practices

### Do

- **Always provide an accessible label.** Use a visible `<label>` with `htmlFor` or `aria-labelledby`. Every Switch must have an accessible name.
- **Use Switch for immediate, in-place changes.** The user should not need to press a "Save" button for the switch to take effect.
- **Position the Switch to the right of its label** in settings rows — this is the established pattern on both iOS and Android and matches user expectations.
- **Show a loading/disabled state for async operations.** If toggling triggers a network request, disable the Switch while the request is in flight and show a Loader nearby.
- **Use `aria-describedby`** to associate supplementary description text with the Switch, enabling screen reader users to understand context without reading the surrounding paragraph.
- **Test with keyboard navigation** — confirm Space and Enter both toggle the switch.

### Don't

- **Don't use Switch for multi-select or form submission contexts** — use Checkbox instead.
- **Don't mark the checkmark icon as meaningful** to screen readers — it is decorative (`aria-hidden="true"`).
- **Don't use `aria-checked="mixed"`** — Switch only supports true/false, not an indeterminate state.
- **Don't place switches inside `<form>` elements that submit** — Switch is for immediate toggles. If the value must be submitted, use a Checkbox.
- **Don't create switch groups without a `<fieldset>/<legend>`** if the switches are logically related (e.g., a group of notification preference toggles under one heading).
- **Don't rely solely on color** to communicate the on/off state — the thumb position and checkmark icon provide redundant non-color cues.
- **Don't apply transitions longer than ~250ms** — overly slow animations make the UI feel sluggish for a simple toggle.

### Settings Row Layout Pattern

The canonical settings row pattern pairs the Switch with a label (and optional description) on the left, Switch on the right:

```
┌──────────────────────────────────────────────────┐
│  Dark Mode                             ● ████   │
│  Switch the interface to dark theme    ON        │
└──────────────────────────────────────────────────┘
```

- Label: 16px / Regular
- Description: 14px / Regular / Muted
- Switch: right-aligned
- Row height: minimum 48px; typically 56–64px with description text

---

## Related Components

| Component | Relationship |
|---|---|
| **Checkbox** | Use for form submission contexts and multi-select. Switch is for immediate toggles. |
| **Radio / RadioGroup** | Use when selecting one option from a group (not binary on/off) |
| **Segmented Control** | Use for choosing between 2–4 named options; Switch is strictly binary |
| **Slider** | Use when a value exists on a continuous spectrum rather than a binary state |
| **Selection Card** | Use for rich binary selections that need visual card presentation |

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Two states (Checked/Unchecked); checkmark icon in ON thumb; Compose `thumbContent` API; `role="switch"` ARIA pattern; 200ms slide + color transition |

---

## Testing Checklist

### Visual

- [ ] Track dimensions: 51×31px, full-pill border-radius (9999px)
- [ ] Thumb diameter: 27px, 2px inset from track edge
- [ ] ON state: dark track (`--color-neutral-900`), white thumb at right end
- [ ] OFF state: grey track (`--color-neutral-300`), white thumb at left end
- [ ] Checkmark icon (16px) visible inside thumb when ON; absent when OFF
- [ ] Thumb carries Elevation Level 1 shadow in both states
- [ ] Disabled: muted track color, reduced opacity on component
- [ ] Label positioned to the left of Switch in settings row layout

### Interaction

- [ ] Clicking/tapping anywhere on the track toggles the switch
- [ ] `onCheckedChange` fires with the new boolean value on each toggle
- [ ] Disabled switch does not respond to click or tap
- [ ] Touch target is at least 44×44px

### Keyboard

- [ ] `Tab` moves focus to the switch track
- [ ] Focus ring visible (2px, brand blue) with pill border-radius
- [ ] `Space` toggles the switch
- [ ] `Enter` toggles the switch
- [ ] Disabled switch is not reachable via Tab

### Screen Reader

- [ ] `role="switch"` present on the interactive element
- [ ] `aria-checked="true"` when ON; `aria-checked="false"` when OFF
- [ ] Accessible name present (via `aria-label` or `aria-labelledby`)
- [ ] Checkmark icon has `aria-hidden="true"`
- [ ] State change announced on toggle (VoiceOver / NVDA)
- [ ] `aria-disabled="true"` present on disabled switch
- [ ] Disabled switch is skipped by screen reader tab navigation

### Animation

- [ ] Thumb slides 200ms (`cubic-bezier(0.4, 0, 0.2, 1)`) on toggle
- [ ] Track color transitions simultaneously with thumb slide
- [ ] Checkmark fades in with 150ms delay on transition to ON
- [ ] Checkmark fades out immediately on transition to OFF
- [ ] `prefers-reduced-motion: reduce` disables all transitions (static state change)

### Platform (Native)

- [ ] **Android (Compose):** `Switch(checked = true, thumbContent = { Icon(Check) })` renders checkmark in thumb; `Switch(checked = false)` renders without thumb content
- [ ] **Android:** TalkBack announces "on" / "off" on toggle
- [ ] **iOS (SwiftUI):** `Toggle(isOn:)` with `PDSSwitchStyle` matches visual spec
- [ ] **iOS:** VoiceOver announces "on" / "off" on toggle; double-tap to activate
