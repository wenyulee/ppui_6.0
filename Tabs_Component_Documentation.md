# Tabs Component Documentation

**Component:** Tabs
**Version:** 6.0 Beta
**Figma Nodes:** `1442-784` (Tabs bar), `9075-30197` (Parts)
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

The **Tabs** component is a horizontally scrollable navigation bar that allows users to switch between related content views. The active tab is indicated by a **filled dark pill** that slides to the selected item; inactive tabs render as plain muted-text labels against the bar's neutral background. This pill-indicator style (as opposed to an underline indicator) is consistent across both mobile and desktop contexts.

### When to Use

- Switching between sibling content sections at the same level of hierarchy (e.g., "Overview", "Activity", "Settings")
- Filtering a content list into named categories (e.g., "All", "Pending", "Completed")
- Top-of-screen or section-level navigation on mobile where a familiar chip-style tab pattern is expected
- Contexts with 2–8 tab items; beyond 8 items, consider a different navigation pattern

### When Not to Use

- As primary app navigation (use a Bottom Navigation Bar or Navigation Drawer instead)
- When tab content represents meaningfully different destinations with their own URL — use a router-backed navigation pattern
- When only two mutually exclusive options exist — use a Segmented Control
- When tabs would nest more than one level deep — flatten the hierarchy instead

### Tabs vs. Segmented Control

| Attribute | Tabs | Segmented Control |
|---|---|---|
| Item count | 2–8+ (scrollable) | 2–4 (fixed, all visible) |
| Overflow handling | Horizontal scroll | No overflow; all items always visible |
| Pill animation | Slides to selected tab | Slides to selected item |
| ARIA role | `tablist` / `tab` | `tablist` / `tab` OR `radiogroup` / `radio` |
| Content panels | Yes — associated `tabpanel` regions | No — switches views inline |

---

## Component Variants

The Tabs component has a single layout variant: a horizontally scrollable pill-tab bar. Tab item state is the primary dimension of variation.

| Sub-component | Figma Node | Description |
|---|---|---|
| `Tabs` (bar) | `1442-784` | The full scrollable tab bar container |
| `Tabs.Item` (Active=True) | `1442-922` | Individual tab item in the selected/active state |
| `Tabs.Item` (Active=False) | `1442-925` | Individual tab item in the unselected/inactive state |

---

## Anatomy

```
┌──────────────────────────────────────────────────────────────────────┐
│  ╔══════════╗   Label    Label    Label    Label    Label  →scroll   │
│  ║  Label   ║                                                        │
│  ╚══════════╝                                                        │
│      ↑                                                               │
│  Active pill                                                         │
│  (dark fill)                                                         │
└──────────────────────────────────────────────────────────────────────┘
                              Tab Bar
```

### Part Descriptions

| Part | Element | Description |
|---|---|---|
| **Tab Bar** | `<div role="tablist">` | Horizontally scrollable container holding all tab items; neutral grey background with rounded corners |
| **Tab Item (Active)** | `<button role="tab" aria-selected="true">` | Filled dark pill; white label text; indicates the currently selected tab |
| **Tab Item (Inactive)** | `<button role="tab" aria-selected="false">` | Transparent background; muted label text |
| **Scroll Container** | `<div>` (overflow-x: auto, scrollbar hidden) | Wraps the tab list to enable horizontal scrolling when items overflow |
| **Tab Panel** | `<div role="tabpanel">` | Content region associated with each tab; only the active panel is visible |

---

## Size & Spacing Specifications

### Tab Bar Container

| Property | Value | Token |
|---|---|---|
| Bar height | 48px | `--tabs-bar-height: 48px` |
| Bar background | `--color-neutral-subtle` (light grey) | `--tabs-bar-bg` |
| Bar border-radius | `--radius-xl` (12px) | `--tabs-bar-radius` |
| Bar horizontal padding | 4px | `--tabs-bar-padding-x: 4px` |
| Bar vertical padding | 4px | `--tabs-bar-padding-y: 4px` |
| Gap between tab items | 2px | `--tabs-item-gap: 2px` |

### Tab Item

| Property | Value | Token |
|---|---|---|
| Item height | 40px | `--tabs-item-height: 40px` |
| Item min-width | auto (content-sized) | — |
| Item horizontal padding | 16px | `--tabs-item-padding-x: 16px` |
| Item vertical padding | 8px | `--tabs-item-padding-y: 8px` |
| Item border-radius | 9999px (full pill) | `--tabs-item-radius: 9999px` |
| Example item width (Figma) | 69px | Varies with label length |

### Typography

| Property | Value | Token |
|---|---|---|
| Font style | Label / Medium | `--font-label-medium` |
| Font size | 14px | `tokens.base.text.fontsize.ramp[14]` |
| Font weight | 500 (Medium) | `tokens.brand.text.fontweight.label` |
| Line height | Matches 14px ramp | `tokens.base.text.lineheight.ramp[14]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |
| Font family | Brand label font family | `tokens.brand.text.fontfamily.label` |

### Active Pill Shadow

The active tab pill does not use Elevation; it is distinguished purely by fill color and text color contrast.

---

## Style Variants

### Tab Item States — Visual Summary

| State | Background | Text Color | Token |
|---|---|---|---|
| Active (selected) | `--color-neutral-900` (near-black) | `--color-text-on-dark` (white) | `--tabs-item-active-bg` |
| Inactive (unselected) | Transparent | `--color-text-muted` | `--tabs-item-inactive-bg` |
| Inactive Hover | `--color-neutral-100` (very light grey) | `--color-text-default` | `--tabs-item-hover-bg` |
| Focused (keyboard) | Transparent / active bg | — | Focus ring applied |
| Disabled | Transparent | `--color-text-disabled` | `--tabs-item-disabled-color` |

### Tab Bar Background

The tab bar sits on a `--color-neutral-subtle` surface, creating a subtle container that groups the tabs visually. The active pill's dark fill creates strong contrast against this light background.

---

## State Specifications

### Active / Selected

- Background: `--color-neutral-900` (dark filled pill)
- Label: white (`--color-text-on-dark`)
- `aria-selected="true"`
- `tabindex="0"` (receives focus in keyboard navigation)

### Inactive / Unselected

- Background: transparent
- Label: muted grey (`--color-text-muted`)
- `aria-selected="false"`
- `tabindex="-1"` (excluded from Tab key focus; reached via arrow keys)

### Hover (Inactive)

- Background: `--color-neutral-100` (subtle fill)
- Label: `--color-text-default` (standard dark)
- Cursor: `pointer`

### Focused (Keyboard)

- 2px focus ring on the currently focused tab item
- Focus ring color: `--color-focus-ring` (brand blue)
- Focus ring offset: 2px
- Focus ring border-radius matches pill shape (9999px)
- Only the active tab item has `tabindex="0"` at rest; arrow keys move focus within the tablist (roving tabindex)

### Disabled

- Background: transparent
- Label: `--color-text-disabled`
- `aria-disabled="true"`
- `tabindex="-1"`; skipped by both Tab and arrow key navigation

---

## Props API

### Web / React

#### `Tabs` (container)

```typescript
interface TabsProps {
  /**
   * The value of the currently active tab.
   * Must match the `value` prop of one of the child `Tabs.Item` components.
   */
  value: string;

  /**
   * Callback fired when a tab is selected.
   * Receives the value of the newly selected tab.
   */
  onValueChange: (value: string) => void;

  /** Tab items. Should be an array of `Tabs.Item` elements. */
  children: React.ReactNode;

  /** Accessible label for the tab list (required if no visible heading labels the group). */
  'aria-label'?: string;

  /** ID of a visible heading element that labels this tab group. */
  'aria-labelledby'?: string;

  /** Additional CSS class for the tab bar container. */
  className?: string;

  /** Inline style overrides. */
  style?: React.CSSProperties;
}
```

#### `Tabs.Item` (individual tab)

```typescript
interface TabsItemProps {
  /**
   * Unique value identifying this tab.
   * Used to match against `Tabs.value` to determine active state.
   */
  value: string;

  /** The label text displayed in the tab. */
  label: string;

  /** Whether this tab item is disabled. */
  disabled?: boolean;

  /** Optional icon displayed before the label. */
  icon?: React.ReactNode;

  /** Optional badge count or indicator displayed after the label. */
  badge?: number | string;

  /** Additional CSS class. */
  className?: string;
}
```

### Android — Jetpack Compose

```kotlin
@Composable
fun Tabs(
    selectedTabIndex: Int,
    modifier: Modifier = Modifier,
    tabs: @Composable () -> Unit
)

// Individual tab data item
data class TabData(
    val text: String,
    val icon: ImageVector? = null,
    val badge: String? = null,
    val enabled: Boolean = true
)
```

**Usage:**

```kotlin
Tabs {
    TabData(text = "Label")
    TabData(text = "Label")
    TabData(text = "Label")
    TabData(text = "Label")
    TabData(text = "Label")
    TabData(text = "Label")
    TabData(text = "Label")
}
```

---

## Code Examples

### 1. Basic Tabs

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';

export function BasicTabs() {
  const [active, setActive] = useState('overview');

  return (
    <>
      <Tabs
        value={active}
        onValueChange={setActive}
        aria-label="Content sections"
      >
        <Tabs.Item value="overview" label="Overview" />
        <Tabs.Item value="activity" label="Activity" />
        <Tabs.Item value="settings" label="Settings" />
      </Tabs>

      <div>
        {active === 'overview' && <OverviewPanel />}
        {active === 'activity' && <ActivityPanel />}
        {active === 'settings' && <SettingsPanel />}
      </div>
    </>
  );
}
```

---

### 2. Tabs with Associated Tab Panels (Full ARIA Pattern)

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';

const TAB_ITEMS = [
  { value: 'all',       label: 'All' },
  { value: 'pending',   label: 'Pending' },
  { value: 'completed', label: 'Completed' },
  { value: 'archived',  label: 'Archived' },
];

export function FilteredList() {
  const [active, setActive] = useState('all');

  return (
    <div>
      <Tabs
        value={active}
        onValueChange={setActive}
        aria-label="Order status filter"
      >
        {TAB_ITEMS.map((tab) => (
          <Tabs.Item key={tab.value} value={tab.value} label={tab.label} />
        ))}
      </Tabs>

      {TAB_ITEMS.map((tab) => (
        <div
          key={tab.value}
          role="tabpanel"
          id={`panel-${tab.value}`}
          aria-labelledby={`tab-${tab.value}`}
          hidden={active !== tab.value}
          tabIndex={0}
        >
          <OrderList status={tab.value} />
        </div>
      ))}
    </div>
  );
}
```

---

### 3. Tabs with Badge Counts

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';

export function InboxTabs() {
  const [active, setActive] = useState('inbox');

  return (
    <Tabs value={active} onValueChange={setActive} aria-label="Inbox categories">
      <Tabs.Item value="inbox"    label="Inbox"   badge={12} />
      <Tabs.Item value="sent"     label="Sent" />
      <Tabs.Item value="drafts"   label="Drafts"  badge={3} />
      <Tabs.Item value="archived" label="Archived" />
    </Tabs>
  );
}
```

---

### 4. Scrollable Tabs (Many Items)

The tab bar scrolls automatically when items overflow the container width. No configuration is needed — the scroll behaviour is built into the component.

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';

const CATEGORIES = [
  'All', 'Electronics', 'Clothing', 'Home', 'Sports',
  'Books', 'Toys', 'Automotive', 'Garden',
];

export function CategoryTabs() {
  const [active, setActive] = useState('All');

  return (
    <Tabs
      value={active}
      onValueChange={setActive}
      aria-label="Product categories"
    >
      {CATEGORIES.map((cat) => (
        <Tabs.Item key={cat} value={cat} label={cat} />
      ))}
    </Tabs>
  );
}
```

---

### 5. Tabs with Icons

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';
import { HomeIcon, BellIcon, SettingsIcon } from '@design-system/icons';

export function IconTabs() {
  const [active, setActive] = useState('home');

  return (
    <Tabs value={active} onValueChange={setActive} aria-label="Main sections">
      <Tabs.Item value="home"          label="Home"          icon={<HomeIcon />} />
      <Tabs.Item value="notifications" label="Notifications" icon={<BellIcon />} badge={5} />
      <Tabs.Item value="settings"      label="Settings"      icon={<SettingsIcon />} />
    </Tabs>
  );
}
```

---

### 6. Tabs with a Disabled Item

```tsx
import { useState } from 'react';
import { Tabs } from '@design-system/components';

export function TabsWithDisabled() {
  const [active, setActive] = useState('active');

  return (
    <Tabs value={active} onValueChange={setActive} aria-label="Account sections">
      <Tabs.Item value="active"  label="Active" />
      <Tabs.Item value="history" label="History" />
      <Tabs.Item value="premium" label="Premium" disabled />
    </Tabs>
  );
}
```

---

### 7. URL-Synced Tabs (React Router)

```tsx
import { useSearchParams } from 'react-router-dom';
import { Tabs } from '@design-system/components';

const TABS = ['overview', 'transactions', 'statements', 'settings'];

export function AccountTabs() {
  const [searchParams, setSearchParams] = useSearchParams();
  const active = searchParams.get('tab') ?? 'overview';

  const handleChange = (value: string) => {
    setSearchParams({ tab: value }, { replace: true });
  };

  return (
    <Tabs value={active} onValueChange={handleChange} aria-label="Account navigation">
      {TABS.map((tab) => (
        <Tabs.Item
          key={tab}
          value={tab}
          label={tab.charAt(0).toUpperCase() + tab.slice(1)}
        />
      ))}
    </Tabs>
  );
}
```

---

### 8. Lazy-Loaded Tab Panels

Tab panels are only mounted when first selected, then kept in the DOM but hidden to preserve scroll state:

```tsx
import { useState, useRef } from 'react';
import { Tabs } from '@design-system/components';

export function LazyTabs() {
  const [active, setActive] = useState('tab1');
  const mountedRef = useRef<Set<string>>(new Set(['tab1']));

  const handleChange = (value: string) => {
    mountedRef.current.add(value);
    setActive(value);
  };

  const TABS = [
    { value: 'tab1', label: 'Overview', Panel: OverviewPanel },
    { value: 'tab2', label: 'Analytics', Panel: AnalyticsPanel },
    { value: 'tab3', label: 'Reports', Panel: ReportsPanel },
  ];

  return (
    <div>
      <Tabs value={active} onValueChange={handleChange} aria-label="Dashboard">
        {TABS.map(({ value, label }) => (
          <Tabs.Item key={value} value={value} label={label} />
        ))}
      </Tabs>

      {TABS.map(({ value, Panel }) =>
        mountedRef.current.has(value) ? (
          <div
            key={value}
            role="tabpanel"
            aria-labelledby={`tab-${value}`}
            hidden={active !== value}
            tabIndex={0}
          >
            <Panel />
          </div>
        ) : null
      )}
    </div>
  );
}
```

---

### 9. Compose — Tabs with State

```kotlin
@Composable
fun ContentTabs() {
    var selectedIndex by remember { mutableStateOf(0) }

    val tabs = listOf(
        TabData(text = "Overview"),
        TabData(text = "Activity"),
        TabData(text = "Settings"),
        TabData(text = "Help")
    )

    Column {
        Tabs(selectedTabIndex = selectedIndex) {
            tabs.forEachIndexed { index, tab ->
                Tab(
                    selected = selectedIndex == index,
                    onClick = { selectedIndex = index },
                    text = { Text(tab.text) }
                )
            }
        }

        when (selectedIndex) {
            0 -> OverviewContent()
            1 -> ActivityContent()
            2 -> SettingsContent()
            3 -> HelpContent()
        }
    }
}
```

---

### 10. Tabs Controlled by External State (e.g., Deep Link)

```tsx
import { useEffect, useState } from 'react';
import { Tabs } from '@design-system/components';

export function DeepLinkedTabs({ initialTab = 'summary' }: { initialTab?: string }) {
  const validTabs = ['summary', 'details', 'history'];
  const [active, setActive] = useState(
    validTabs.includes(initialTab) ? initialTab : 'summary'
  );

  // Sync from external prop (e.g. URL param passed from parent router)
  useEffect(() => {
    if (validTabs.includes(initialTab)) setActive(initialTab);
  }, [initialTab]);

  return (
    <Tabs value={active} onValueChange={setActive} aria-label="Transaction details">
      <Tabs.Item value="summary" label="Summary" />
      <Tabs.Item value="details" label="Details" />
      <Tabs.Item value="history" label="History" />
    </Tabs>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern

Tabs implement the [ARIA Tabs Pattern](https://www.w3.org/WAI/ARIA/apg/patterns/tabs/):

```html
<!-- Tab bar -->
<div role="tablist" aria-label="Content sections">

  <!-- Active tab -->
  <button
    role="tab"
    id="tab-overview"
    aria-selected="true"
    aria-controls="panel-overview"
    tabindex="0"
  >
    Overview
  </button>

  <!-- Inactive tabs -->
  <button
    role="tab"
    id="tab-activity"
    aria-selected="false"
    aria-controls="panel-activity"
    tabindex="-1"
  >
    Activity
  </button>

  <button
    role="tab"
    id="tab-settings"
    aria-selected="false"
    aria-controls="panel-settings"
    tabindex="-1"
    aria-disabled="true"
  >
    Settings
  </button>

</div>

<!-- Tab panels (only the active one is visible) -->
<div
  role="tabpanel"
  id="panel-overview"
  aria-labelledby="tab-overview"
  tabindex="0"
>
  <!-- Overview content -->
</div>

<div
  role="tabpanel"
  id="panel-activity"
  aria-labelledby="tab-activity"
  tabindex="0"
  hidden
>
  <!-- Activity content -->
</div>
```

### Required ARIA Attributes

| Attribute | Element | Value | Purpose |
|---|---|---|---|
| `role="tablist"` | Tab bar container | — | Identifies the container as a tab list |
| `aria-label` or `aria-labelledby` | Tab bar container | — | Names the tab group for screen readers |
| `role="tab"` | Each tab button | — | Identifies each button as a tab |
| `aria-selected` | Each tab button | `"true"` / `"false"` | Indicates active state |
| `aria-controls` | Each tab button | `id` of associated panel | Associates tab with its panel |
| `tabindex` | Tab buttons | `0` (active) / `-1` (inactive) | Roving tabindex pattern |
| `role="tabpanel"` | Each content panel | — | Identifies the content region |
| `aria-labelledby` | Each panel | `id` of its tab | Associates panel with its tab |
| `tabindex="0"` | Each panel | — | Makes panel content reachable by keyboard |

### Keyboard Navigation (Roving Tabindex)

| Key | Action |
|---|---|
| `Tab` | Move focus into the tab list (to the active tab); move focus out of the tab list into the tab panel |
| `ArrowRight` | Move focus to the next tab; wraps from last to first |
| `ArrowLeft` | Move focus to the previous tab; wraps from first to last |
| `Home` | Move focus to the first tab |
| `End` | Move focus to the last tab |
| `Space` / `Enter` | Activate the focused tab (if using manual activation) |

> **Automatic vs. Manual Activation:** This design system uses **automatic activation** — focus and selection move together. When the user presses ArrowRight/ArrowLeft, both focus AND `aria-selected` update immediately. There is no separate activation step.

Disabled tabs are **skipped** by arrow key navigation.

### Tab Panel Focus

After a tab is activated (by click or keyboard), focus should move to the associated `role="tabpanel"` element. The panel must have `tabindex="0"` to be focusable. Content within the panel is then navigable with subsequent Tab presses.

```tsx
// Move focus to panel on tab change
const handleChange = (value: string) => {
  setActive(value);
  // Focus the new panel after state update
  requestAnimationFrame(() => {
    document.getElementById(`panel-${value}`)?.focus();
  });
};
```

### Labeling the Tab Group

The `role="tablist"` element must have an accessible name. Use `aria-label` for a concise string or `aria-labelledby` when a visible heading labels the group:

```html
<!-- With aria-label -->
<div role="tablist" aria-label="Account sections">

<!-- With aria-labelledby -->
<h2 id="account-heading">Account</h2>
<div role="tablist" aria-labelledby="account-heading">
```

### Badge Accessibility

If a badge count is displayed within a tab item, ensure it is included in the accessible name or announced via `aria-label`:

```html
<button
  role="tab"
  aria-selected="false"
  aria-label="Inbox, 12 unread"
>
  Inbox
  <span aria-hidden="true" class="badge">12</span>
</button>
```

### Contrast Requirements

| Element | Minimum Contrast | Requirement |
|---|---|---|
| Active label (white on dark) | ≥ 4.5:1 (WCAG 1.4.3) | White text on `--color-neutral-900` |
| Inactive label (muted on bar bg) | ≥ 4.5:1 | Muted text on `--color-neutral-subtle` |
| Active pill vs. bar background | ≥ 3:1 (non-text, WCAG 1.4.11) | Dark fill against light grey bar |
| Focus ring vs. adjacent color | ≥ 3:1 | Focus ring on inactive/active item |

---

## Animation Specifications

### Active Pill Slide

The dark pill indicator slides from the previously active tab to the newly active tab when a new selection is made:

```css
/* The active pill is a background element that translates */
.tabs-active-indicator {
  position: absolute;
  background: var(--tabs-item-active-bg);
  border-radius: 9999px;
  transition:
    transform 250ms cubic-bezier(0.4, 0, 0.2, 1),
    width 250ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

Alternatively, the active background can be applied directly to the tab button with a `background-color` transition:

```css
.tab-item {
  background-color: transparent;
  transition: background-color 200ms ease;
}

.tab-item[aria-selected="true"] {
  background-color: var(--tabs-item-active-bg);
}
```

The sliding indicator approach (first method) is preferred for a more fluid feel when switching between non-adjacent tabs.

| Property | Value |
|---|---|
| Duration | 250ms |
| Easing | `cubic-bezier(0.4, 0, 0.2, 1)` (standard) |
| Properties | `transform` (translateX) + `width` |

### Label Color Transition

The text color of departing (becoming inactive) and arriving (becoming active) tabs transitions simultaneously:

```css
.tab-item-label {
  transition: color 200ms ease;
}
```

### Scroll to Active Tab

When a tab is activated that is not fully visible within the scroll container, the bar smoothly scrolls to bring the active tab into view:

```typescript
function scrollTabIntoView(tabElement: HTMLElement) {
  tabElement.scrollIntoView({
    behavior: 'smooth',
    block: 'nearest',
    inline: 'center',
  });
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .tabs-active-indicator,
  .tab-item,
  .tab-item-label {
    transition: none;
  }
}
```

---

## Design Tokens

### Sizing Tokens

| Token | Value | Usage |
|---|---|---|
| `--tabs-bar-height` | `48px` | Overall tab bar height |
| `--tabs-bar-padding-x` | `4px` | Horizontal padding inside bar |
| `--tabs-bar-padding-y` | `4px` | Vertical padding inside bar |
| `--tabs-bar-radius` | `12px` | Tab bar container border-radius |
| `--tabs-item-height` | `40px` | Individual tab item height |
| `--tabs-item-padding-x` | `16px` | Horizontal padding within each tab item |
| `--tabs-item-padding-y` | `8px` | Vertical padding within each tab item |
| `--tabs-item-radius` | `9999px` | Tab item border-radius (full pill) |
| `--tabs-item-gap` | `2px` | Gap between adjacent tab items |

### Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--tabs-bar-bg` | `--color-neutral-subtle` | Tab bar container background |
| `--tabs-item-active-bg` | `--color-neutral-900` | Active tab pill fill |
| `--tabs-item-active-label` | `--color-text-on-dark` (white) | Active tab label color |
| `--tabs-item-inactive-label` | `--color-text-muted` | Inactive tab label color |
| `--tabs-item-hover-bg` | `--color-neutral-100` | Inactive tab hover fill |
| `--tabs-item-hover-label` | `--color-text-default` | Inactive tab hover label color |
| `--tabs-item-disabled-label` | `--color-text-disabled` | Disabled tab label color |
| `--tabs-focus-ring` | `--color-focus-ring` | Keyboard focus ring color |

### Typography Tokens

| Token | Value | Usage |
|---|---|---|
| `--tabs-label-font-family` | `tokens.brand.text.fontfamily.label` | Tab label font |
| `--tabs-label-font-size` | `14px` | Tab label size |
| `--tabs-label-font-weight` | `500` (Medium) | Tab label weight |
| `--tabs-label-line-height` | `tokens.base.text.lineheight.ramp[14]` | Tab label line height |
| `--tabs-label-letter-spacing` | `0` | Tab label letter spacing |

### Animation Tokens

| Token | Value | Usage |
|---|---|---|
| `--tabs-transition-duration` | `250ms` | Pill slide duration |
| `--tabs-transition-easing` | `cubic-bezier(0.4, 0, 0.2, 1)` | Pill slide easing |
| `--tabs-label-transition-duration` | `200ms` | Label color transition duration |

---

## Best Practices

### Do

- **Always provide an accessible label on the `tablist`** via `aria-label` or `aria-labelledby` so screen reader users understand what the tabs navigate.
- **Associate each tab with its panel** using `aria-controls` on the tab and `aria-labelledby` on the panel.
- **Use automatic activation** (move selection with focus) for predictability on mobile and screen readers.
- **Scroll the active tab into view** when it is activated programmatically (deep linking, external state changes).
- **Keep tab labels concise** — one or two words. Long labels reduce the number of visible tabs and create an overcrowded bar.
- **Use badge counts sparingly** — include the count in the accessible name (`aria-label`) so screen reader users receive the information.
- **Make sure the currently active tab is visually distinct without relying only on color** — the pill fill, white label text, and position change provide redundant cues.

### Don't

- **Don't nest tabs within tabs** — this creates confusing hierarchy. Flatten or use a different navigation pattern.
- **Don't use Tabs for binary choices** — use Segmented Control when there are only 2 options that should all be visible simultaneously.
- **Don't put more than ~8 tabs in a bar** without reconsidering the information architecture. Many tabs signal the content structure may need rethinking.
- **Don't use `role="tabpanel"` without `tabindex="0"`** — the panel needs to be focusable for keyboard users navigating out of the tab list.
- **Don't load all tab panel content upfront** if panels contain heavy components — use lazy loading to keep initial render fast.
- **Don't remove inactive panels from the DOM immediately** if the user may have scrolled within them — keeping them hidden (via `hidden` attribute) preserves scroll state.
- **Don't skip the `aria-controls` / `aria-labelledby` linkage** — these are required for the ARIA tabs pattern to function correctly with assistive technology.

### Tab Count Guidelines

| Tab Count | Recommendation |
|---|---|
| 2 | Consider Segmented Control instead |
| 3–5 | Ideal; all or most items visible at once |
| 6–8 | Acceptable; horizontal scroll activates on small screens |
| 9+ | Re-evaluate architecture; consider a filter menu or Select |

---

## Related Components

| Component | Relationship |
|---|---|
| **Segmented Control** | Use for 2–4 options that must all be visible simultaneously. Tabs support overflow scrolling; Segmented Control does not. |
| **Menu** | Use for navigation with many items where a persistent tab bar is inappropriate |
| **Selection Card** | Use for visual card-based category selection |
| **Pagination** | Dot-based step/carousel indicator — not a content navigation pattern |
| **Radio / RadioGroup** | Use in forms for selecting one option from a group when the tab-panel pattern is not appropriate |

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Pill-indicator tab bar; `Active=True` / `Active=False` item states; Compose `TabData` API; `role="tablist"` / `role="tab"` / `role="tabpanel"` ARIA pattern; roving tabindex keyboard navigation; 250ms pill slide animation |

---

## Testing Checklist

### Visual

- [ ] Tab bar renders at 48px height with `--radius-xl` (12px) corners and `--color-neutral-subtle` background
- [ ] Tab items are 40px tall with full-pill (9999px) border-radius
- [ ] Active tab: dark (`--color-neutral-900`) fill, white label text
- [ ] Inactive tab: transparent background, muted label text
- [ ] Active tab pill accurately covers only the selected item
- [ ] Label typography: 14px, 500 weight, brand label font family
- [ ] Tab bar scrolls horizontally when items overflow the container width
- [ ] Active tab scrolls into view when selected

### Interaction

- [ ] Clicking/tapping an inactive tab activates it and updates the displayed panel
- [ ] `onValueChange` fires with the new tab value on selection
- [ ] Disabled tab does not respond to click or tap
- [ ] Horizontal scroll works on touch devices with a single finger
- [ ] Tab bar does not show a visible scrollbar (hidden via CSS)

### Keyboard

- [ ] `Tab` moves focus into the tab bar (onto the active tab item)
- [ ] `Tab` again moves focus from the tab bar into the tab panel
- [ ] `ArrowRight` moves focus to the next tab and activates it
- [ ] `ArrowLeft` moves focus to the previous tab and activates it
- [ ] `ArrowRight` wraps from last tab to first
- [ ] `ArrowLeft` wraps from first tab to last
- [ ] `Home` moves focus and selection to the first tab
- [ ] `End` moves focus and selection to the last tab
- [ ] Disabled tabs are skipped by arrow key navigation
- [ ] Focus ring visible (2px, brand blue, pill-shaped) on focused tab item

### Screen Reader

- [ ] `role="tablist"` present on the container; `aria-label` or `aria-labelledby` present
- [ ] Each tab button has `role="tab"`
- [ ] Active tab has `aria-selected="true"`; inactive tabs have `aria-selected="false"`
- [ ] Each tab has `aria-controls` pointing to its panel's `id`
- [ ] Each panel has `role="tabpanel"`, `tabindex="0"`, and `aria-labelledby` pointing to its tab's `id`
- [ ] Inactive panels have `hidden` attribute
- [ ] Disabled tabs have `aria-disabled="true"`
- [ ] Tabs with badges include badge count in the accessible name

### Animation

- [ ] Active pill slides smoothly (250ms) to newly selected tab
- [ ] Label color transitions simultaneously with pill movement
- [ ] `prefers-reduced-motion: reduce` disables all transitions (instant state change)
- [ ] No animation artifacts when switching rapidly between tabs

### Platform (Native)

- [ ] **Android (Compose):** `Tabs { TabData(text = "Label") ... }` renders correct pill-tab bar; selected tab shows filled pill
- [ ] **Android:** TalkBack announces tab label and selected/unselected state on focus
- [ ] **Android:** Arrow gesture or swipe between panels updates the active tab indicator
