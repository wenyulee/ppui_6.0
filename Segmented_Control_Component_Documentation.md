# Segmented Control Component Documentation

## Overview

The Segmented Control is a horizontal set of mutually exclusive options rendered inside a single pill-shaped container. It presents 2–5 short text labels (or icons) as equal-width segments; the active segment is elevated with a white pill and subtle shadow, while inactive segments share the container's grey background. It is used for switching between views, modes, or filters where all options are visible simultaneously and only one can be selected at a time.

The Segmented Control is visually and semantically distinct from a Tab Bar (which drives full page navigation) and a Radio Group (which uses vertically stacked, independent inputs). It communicates a compact, iOS-native-feeling choice control and is ideal for 2–4 options with short labels.

**Component name:** `SegmentedControl` / `SegmentedControlItem`
**Figma nodes:** `1442-908` (Full component), `9075-29889` (Parts — `SegmentedControl.Item`)
**Platform:** React / TypeScript (Web), SwiftUI (iOS), Jetpack Compose (Android)
**Category:** Navigation / Form / Selection

---

## Component Variants

### By Item Count

| Items | Recommended Use |
|-------|----------------|
| **2** | Binary mode toggle (e.g., List / Grid, Sent / Received) |
| **3** | Three-way view/period selector (e.g., Day / Week / Month) |
| **4** | Four-option filter (e.g., All / Active / Pending / Completed) |
| **5** | Maximum; beyond 5, labels become too compressed to read |

### By Content Type

| Type | Description | Example |
|------|-------------|---------|
| **Label only** (default) | Text-only segments | "Week" / "Month" / "Year" |
| **Icon only** | Icon without label; requires tooltip or `aria-label` | 📋 / 🔲 / ▤ |
| **Icon + Label** | Icon paired with short text | 🔍 Search / 📂 Browse |

---

## Anatomy

```
┌──────────────────────────────────────────────────┐  ← Container (grey pill)
│  ┌──────────────┐                                │
│  │  Label       │     Label          Label       │  ← Active item: white pill
│  └──────────────┘                                │    Inactive items: transparent
└──────────────────────────────────────────────────┘
```

### Sub-Component: `SegmentedControl.Item` (node `1442:861`)

| State | Node | Size | Background |
|-------|------|------|-----------|
| `Active=True` | `1442:860` | 77 × 40 px | White, with shadow (elevated) |
| `Active=False` | `1442:859` | 77 × 40 px | Transparent (shows container grey) |

---

## Size & Spacing Specifications

### Container

| Property | Value | Token |
|----------|-------|-------|
| Height | 40 px | Fixed (from item height: 77 × 40 px) |
| Width | 100% (fluid) / 370 px | Fills parent |
| Border radius | `tokens.base.border.radius[12]` | ~12 px |
| Background | Light grey | `tokens.colorScheme.background.utility.unselected` |
| Padding (inner) | 2 px | Inset for the active pill to float |

### Segment Item

| Property | Value | Token |
|----------|-------|-------|
| Height | 40 px | Fixed |
| Width | Equal fraction of container (100% / itemCount) | Fluid |
| Border radius | `tokens.base.border.radius[10]` | ~10 px (slightly smaller than container) |
| Horizontal padding | 8 px | `tokens.base.spacing[8]` |
| Min-width | Enough to fit label at Label/Medium (14 px) | — |

### Active Item Elevation

```
shadow-1: x=0, y=1, blur=4, spread=0, color=rgba(0,0,0,0.10)
shadow-2: x=0, y=0, blur=1, spread=0, color=rgba(0,0,0,0.06)
```

### Typography

| Property | Value | Token |
|----------|-------|-------|
| Font family | Label | `tokens.brand.text.fontfamily.label` |
| Font weight | Medium | `tokens.brand.text.fontweight.label` |
| Font size | 14 px | `tokens.base.text.fontsize.ramp[14]` |
| Line height | 20 px | `tokens.base.text.lineheight.ramp[14]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |

---

## Style Specifications

### Colors

| Element | State | Token | Default Value |
|---------|-------|-------|---------------|
| Container background | All | `tokens.colorScheme.background.utility.unselected` | Light grey |
| Active item background | Active | `tokens.colorScheme.background.default` | White |
| Active item shadow | Active | See elevation above | — |
| Inactive item background | Inactive | Transparent | — |
| Active label color | Active | `tokens.colorScheme.content.base` | Black |
| Inactive label color | Inactive | `tokens.colorScheme.content.muted` | rgba(0,0,0,0.63) |
| Disabled item label | Disabled | `tokens.colorScheme.content.disabled` | — |
| Focus ring | Focused item | `tokens.colorScheme.border.focus` | Blue |

---

## State Specifications

### Default (First Item Active)
- First `SegmentedControlItem` is active by default unless `defaultValue` is provided
- Active item: white pill with shadow, Label/Medium text in `content.base`
- Inactive items: grey background, Label/Medium text in `content.muted`

### Active Transition
- When the user selects a new segment, the white pill slides to the new position (animated)
- Text color transitions from muted → base (incoming) and base → muted (outgoing)

### Hover (Inactive Item)
- Subtle background highlight or opacity change on the hovered inactive segment

### Focus
- Focus ring drawn around the focused segment item
- Tab moves focus into the control; arrow keys navigate between items

### Disabled (Individual Item)
- The item's label is rendered in `content.disabled`
- `aria-disabled="true"` on the item; excluded from keyboard navigation
- Cannot be selected by click or keyboard

### Disabled (Entire Control)
- All items rendered in disabled color
- Container opacity reduced
- No interaction possible

---

## Props API

### TypeScript Interface

```typescript
import { ReactNode } from 'react';

// ─── SegmentedControlItem ─────────────────────────────────────────────────────

export interface SegmentedControlItemProps {
  /**
   * The unique value representing this segment.
   * Used to match against the SegmentedControl's value prop.
   */
  value: string;

  /**
   * Label text displayed inside the segment.
   * Keep to 1–3 words for best results.
   */
  label?: string;

  /**
   * Leading icon rendered before (or instead of) the label.
   */
  icon?: ReactNode;

  /**
   * When true, the item cannot be selected.
   * @default false
   */
  disabled?: boolean;

  /**
   * Accessible label for icon-only segments.
   * Required when no visible text label is provided.
   */
  'aria-label'?: string;

  /**
   * Additional class name(s) applied to the segment item.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── SegmentedControl ─────────────────────────────────────────────────────────

export interface SegmentedControlProps {
  /**
   * The SegmentedControlItem children.
   */
  children: ReactNode;

  /**
   * The currently selected value (controlled mode).
   */
  value?: string;

  /**
   * The default selected value (uncontrolled mode).
   * Defaults to the value of the first item if not provided.
   */
  defaultValue?: string;

  /**
   * Callback fired when the selected value changes.
   */
  onChange?: (value: string) => void;

  /**
   * When true, disables all segment items.
   * @default false
   */
  disabled?: boolean;

  /**
   * Accessible label for the segmented control group.
   * Describes the purpose of the selection to assistive technology.
   * Example: "View mode", "Time period"
   */
  'aria-label'?: string;

  /**
   * ID of an element that labels the segmented control.
   */
  'aria-labelledby'?: string;

  /**
   * Additional class name(s) applied to the container element.
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

### 1. Basic Three-Segment Control (Uncontrolled)

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function TimePeriodSelector() {
  return (
    <SegmentedControl
      defaultValue="week"
      aria-label="Time period"
    >
      <SegmentedControlItem value="day"   label="Day"   />
      <SegmentedControlItem value="week"  label="Week"  />
      <SegmentedControlItem value="month" label="Month" />
    </SegmentedControl>
  );
}
```

### 2. Controlled Segmented Control

```tsx
import { useState } from 'react';
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function ViewToggle({ onViewChange }) {
  const [view, setView] = useState<'list' | 'grid'>('list');

  const handleChange = (value: string) => {
    setView(value as 'list' | 'grid');
    onViewChange(value);
  };

  return (
    <SegmentedControl
      value={view}
      onChange={handleChange}
      aria-label="View mode"
    >
      <SegmentedControlItem value="list" label="List" />
      <SegmentedControlItem value="grid" label="Grid" />
    </SegmentedControl>
  );
}
```

### 3. Four-Segment Filter

```tsx
import { useState } from 'react';
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

type TransactionFilter = 'all' | 'sent' | 'received' | 'pending';

export function TransactionFilter() {
  const [filter, setFilter] = useState<TransactionFilter>('all');

  return (
    <>
      <SegmentedControl
        value={filter}
        onChange={v => setFilter(v as TransactionFilter)}
        aria-label="Transaction filter"
        aria-controls="transaction-list"
      >
        <SegmentedControlItem value="all"      label="All"      />
        <SegmentedControlItem value="sent"     label="Sent"     />
        <SegmentedControlItem value="received" label="Received" />
        <SegmentedControlItem value="pending"  label="Pending"  />
      </SegmentedControl>

      <TransactionList
        id="transaction-list"
        filter={filter}
        aria-live="polite"
      />
    </>
  );
}
```

### 4. Icon-Only Segments

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';
import { ListIcon, GridIcon, MapIcon } from '@ds/icons';

export function LayoutSelector() {
  return (
    <SegmentedControl defaultValue="list" aria-label="Layout style">
      <SegmentedControlItem
        value="list"
        icon={<ListIcon />}
        aria-label="List view"
      />
      <SegmentedControlItem
        value="grid"
        icon={<GridIcon />}
        aria-label="Grid view"
      />
      <SegmentedControlItem
        value="map"
        icon={<MapIcon />}
        aria-label="Map view"
      />
    </SegmentedControl>
  );
}
```

### 5. Icon + Label Segments

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';
import { SearchIcon, BrowseIcon } from '@ds/icons';

export function ExploreMode() {
  return (
    <SegmentedControl defaultValue="search" aria-label="Explore mode">
      <SegmentedControlItem
        value="search"
        icon={<SearchIcon />}
        label="Search"
      />
      <SegmentedControlItem
        value="browse"
        icon={<BrowseIcon />}
        label="Browse"
      />
    </SegmentedControl>
  );
}
```

### 6. With Disabled Item

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function PlanSelector({ userTier }: { userTier: 'free' | 'paid' }) {
  return (
    <SegmentedControl defaultValue="monthly" aria-label="Billing period">
      <SegmentedControlItem value="monthly" label="Monthly" />
      <SegmentedControlItem value="annual"  label="Annual"  />
      <SegmentedControlItem
        value="custom"
        label="Custom"
        disabled={userTier === 'free'}
        aria-label={userTier === 'free' ? 'Custom (upgrade required)' : 'Custom'}
      />
    </SegmentedControl>
  );
}
```

### 7. Driving Content Panels (Tab-Like Pattern)

```tsx
import { useState } from 'react';
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

const PANELS = {
  overview:      <OverviewPanel />,
  transactions:  <TransactionsPanel />,
  analytics:     <AnalyticsPanel />,
};

export function AccountDashboard() {
  const [panel, setPanel] = useState<keyof typeof PANELS>('overview');

  return (
    <section>
      <SegmentedControl
        value={panel}
        onChange={v => setPanel(v as keyof typeof PANELS)}
        aria-label="Dashboard section"
        aria-controls="dashboard-panel"
      >
        <SegmentedControlItem value="overview"     label="Overview"     />
        <SegmentedControlItem value="transactions" label="Transactions" />
        <SegmentedControlItem value="analytics"    label="Analytics"    />
      </SegmentedControl>

      <div
        id="dashboard-panel"
        role="region"
        aria-live="polite"
        aria-label={`${panel} content`}
      >
        {PANELS[panel]}
      </div>
    </section>
  );
}
```

### 8. URL-Synced Segmented Control (React Router)

```tsx
import { useSearchParams } from 'react-router-dom';
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function SyncedSegmentedControl() {
  const [searchParams, setSearchParams] = useSearchParams();
  const view = searchParams.get('view') || 'list';

  const handleChange = (value: string) => {
    setSearchParams(prev => {
      prev.set('view', value);
      return prev;
    });
  };

  return (
    <SegmentedControl
      value={view}
      onChange={handleChange}
      aria-label="View mode"
    >
      <SegmentedControlItem value="list"  label="List"  />
      <SegmentedControlItem value="grid"  label="Grid"  />
      <SegmentedControlItem value="chart" label="Chart" />
    </SegmentedControl>
  );
}
```

### 9. Entire Control Disabled

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function LockedViewSelector({ isLocked }: { isLocked: boolean }) {
  return (
    <SegmentedControl
      defaultValue="standard"
      disabled={isLocked}
      aria-label="Report view"
    >
      <SegmentedControlItem value="standard" label="Standard" />
      <SegmentedControlItem value="detailed" label="Detailed" />
      <SegmentedControlItem value="summary"  label="Summary"  />
    </SegmentedControl>
  );
}
```

### 10. Segmented Control with Adjacent Label

```tsx
import { SegmentedControl, SegmentedControlItem } from '@ds/components';

export function PeriodFilter() {
  return (
    <div className="flex items-center gap-3">
      <span id="period-label" className="text-sm font-medium text-muted">
        Period
      </span>
      <SegmentedControl
        defaultValue="30d"
        aria-labelledby="period-label"
      >
        <SegmentedControlItem value="7d"  label="7d"  />
        <SegmentedControlItem value="30d" label="30d" />
        <SegmentedControlItem value="90d" label="90d" />
        <SegmentedControlItem value="1y"  label="1y"  />
      </SegmentedControl>
    </div>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern

The Segmented Control should follow the **`tablist` / `tab` ARIA pattern** when it controls the display of content panels, or the **`radiogroup` / `radio` pattern** when it represents a mutually exclusive form value selection.

#### Tablist Pattern (content panels)

```html
<div
  role="tablist"
  aria-label="Time period"
>
  <button
    role="tab"
    id="tab-day"
    aria-selected="false"
    aria-controls="panel-day"
    tabindex="-1"
  >
    Day
  </button>
  <button
    role="tab"
    id="tab-week"
    aria-selected="true"
    aria-controls="panel-week"
    tabindex="0"
  >
    Week
  </button>
  <button
    role="tab"
    id="tab-month"
    aria-selected="false"
    aria-controls="panel-month"
    tabindex="-1"
  >
    Month
  </button>
</div>

<div role="tabpanel" id="panel-week" aria-labelledby="tab-week">
  <!-- Week content -->
</div>
```

#### Radiogroup Pattern (form selection)

```html
<div
  role="radiogroup"
  aria-label="View mode"
>
  <button role="radio" aria-checked="true"  tabindex="0">List</button>
  <button role="radio" aria-checked="false" tabindex="-1">Grid</button>
  <button role="radio" aria-checked="false" tabindex="-1">Map</button>
</div>
```

> **Which pattern to use:**
> - Use `tablist`/`tab` when each segment reveals a panel of associated content (like tabs)
> - Use `radiogroup`/`radio` when the selection is a filter or mode with no directly associated panel markup

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Container | `"tablist"` or `"radiogroup"` |
| `aria-label` | Container | Descriptive name (e.g., `"Time period"`) |
| `aria-labelledby` | Container | ID of external visible label |
| `role` | Each segment | `"tab"` or `"radio"` |
| `aria-selected` | Each tab | `"true"` / `"false"` |
| `aria-checked` | Each radio | `"true"` / `"false"` |
| `aria-controls` | Each tab | ID of the associated panel |
| `aria-disabled` | Disabled segment | `"true"` |
| `tabindex` | Active/focused segment | `"0"` |
| `tabindex` | All other segments | `"-1"` |
| `aria-label` | Icon-only segment | Descriptive text (e.g., `"Grid view"`) |

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| `Tab` | Moves focus into the control (to the active/selected segment); Tab again exits |
| `ArrowRight` / `ArrowDown` | Moves focus and selection to the next segment; wraps from last to first |
| `ArrowLeft` / `ArrowUp` | Moves focus and selection to the previous segment; wraps from first to last |
| `Home` | Moves focus and selection to the first segment |
| `End` | Moves focus and selection to the last segment |
| `Enter` / `Space` | Selects the focused segment (if not already selected) |

> **Important:** In a Segmented Control, arrow keys **both move focus and immediately select**. This differs from a standard radio group where selection requires an explicit `Space`. This follows the ARIA `tablist` keyboard contract.

### Focus Management

Only one segment is in the tab order at a time (roving `tabindex`):
- Selected segment: `tabindex="0"`
- All others: `tabindex="-1"`

When focus leaves and re-enters the control, focus goes to the currently selected segment.

### Screen Reader Announcements

- On entering the group: _"[Group label], tab group"_ (tablist) or _"[Group label], group"_ (radiogroup)
- On selecting a segment: _"[Label], tab, selected, [n] of [total]"_
- For icon-only segments: the `aria-label` value is read instead of any visual text

### Content Updates (Live Regions)

When selecting a segment drives a content change, the updated region should use `aria-live="polite"`:

```html
<div
  id="content-panel"
  role="region"
  aria-live="polite"
  aria-label="Transaction list"
>
  <!-- Content updates here when segment changes -->
</div>
```

### Color Contrast

| Element | Minimum Ratio | WCAG |
|---------|---------------|------|
| Active label on white | 4.5:1 | 1.4.3 |
| Inactive label on grey | 4.5:1 | 1.4.3 |
| Focus ring on container | 3:1 | 1.4.11 |
| Active segment vs. container | 3:1 | 1.4.11 Non-text Contrast |

---

## Animation Specifications

### Active Pill Slide Transition

The white active pill slides from one segment to the next rather than abruptly jumping:

```css
.segmented-control-active-pill {
  position: absolute;
  top: 2px;
  bottom: 2px;
  border-radius: var(--segmented-item-radius);
  background: var(--segmented-item-bg-active);
  box-shadow: var(--segmented-item-shadow);
  transition: transform 200ms cubic-bezier(0.4, 0, 0.2, 1),
              width 200ms cubic-bezier(0.4, 0, 0.2, 1);
  /* Transform-based animation for performance */
}
```

### Text Color Transition

```css
.segmented-control-label {
  transition: color 150ms ease;
}
```

### Focus Ring

```css
.segmented-control-item:focus-visible {
  outline: 2px solid var(--tokens.colorScheme.border.focus);
  outline-offset: -2px;
  border-radius: var(--segmented-item-radius);
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .segmented-control-active-pill,
  .segmented-control-label {
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Container ──────────────────────────────────────────────────────── */
--segmented-height:             40px;
--segmented-bg:                 var(--tokens.colorScheme.background.utility.unselected);
--segmented-radius:             var(--tokens.base.border.radius[12]);        /* ~12px */
--segmented-padding:            2px;

/* ─── Segment Item ────────────────────────────────────────────────────── */
--segmented-item-radius:        var(--tokens.base.border.radius[10]);        /* ~10px */
--segmented-item-padding-x:     var(--tokens.base.spacing[8]);               /* 8px */
--segmented-item-bg-active:     var(--tokens.colorScheme.background.default); /* white */
--segmented-item-bg-inactive:   transparent;
--segmented-item-shadow:        0px 1px 4px 0px rgba(0,0,0,0.10),
                                0px 0px 1px 0px rgba(0,0,0,0.06);

/* ─── Label Colors ────────────────────────────────────────────────────── */
--segmented-label-active:       var(--tokens.colorScheme.content.base);      /* black */
--segmented-label-inactive:     var(--tokens.colorScheme.content.muted);     /* rgba(0,0,0,0.63) */
--segmented-label-disabled:     var(--tokens.colorScheme.content.disabled);

/* ─── Typography ──────────────────────────────────────────────────────── */
--segmented-font-family:        var(--tokens.brand.text.fontfamily.label);
--segmented-font-size:          var(--tokens.base.text.fontsize.ramp[14]);   /* 14px */
--segmented-font-weight:        var(--tokens.brand.text.fontweight.label);   /* Medium */
--segmented-line-height:        var(--tokens.base.text.lineheight.ramp[14]); /* 20px */
--segmented-letter-spacing:     var(--tokens.base.text.letterspacing[0]);    /* 0 */

/* ─── Animation ──────────────────────────────────────────────────────── */
--segmented-transition-duration: 200ms;
--segmented-transition-easing:   cubic-bezier(0.4, 0, 0.2, 1);
```

---

## Best Practices

### Do

- **Use for 2–4 options with short, scannable labels** — Each segment should ideally be 1–2 words. If labels are long, consider a Select or Radio Group instead.
- **Animate the active pill slide** — The sliding transition is a key affordance that communicates the relationship between segments and makes the interaction feel tactile.
- **Pair with `aria-label` describing the purpose** — "View mode", "Time period", "Filter" — screen reader users need context for what the control is selecting.
- **Use `aria-controls` when driving content panels** — Link each segment to the panel it reveals so screen reader users can jump directly to the content.
- **Use `aria-live` on the content panel** — When the content area updates after segment selection, announce the change politely to AT users.
- **Ensure all segment labels are equal-weight visually** — Segments share equal width; longer labels should be trimmed or shortened to fit without wrapping.
- **Prefer 3 items for time-period controls** — Day / Week / Month, or Week / Month / Year — these are the most natural triples for this pattern.

### Don't

- **Don't use more than 5 segments** — Beyond 5, the labels become too small to read on mobile and the control loses legibility. Use a Select or horizontal scroll for more options.
- **Don't use Segmented Control for primary page navigation** — Use a Tab Bar (Bottom Navigation) or Header tabs for top-level navigation. Segmented Control is for content-level switching within a section.
- **Don't use it in a form as a replacement for Radio Group** — While semantically similar, the Segmented Control's visual style implies view switching, not form input. For submittable forms, use Radio Group.
- **Don't allow text wrapping inside segments** — Enforce `white-space: nowrap` and `text-overflow: ellipsis` on labels; use short labels to avoid wrapping at all.
- **Don't pre-select "All" unless it is truly the default** — If you have an "All" option, it should be the natural default and placed first (left) in the sequence.
- **Don't nest Segmented Controls** — Two layers of segmented controls for related selections is confusing. Combine dimensions into one control or use a different pattern.
- **Don't use icon-only segments without accessible labels** — Always provide `aria-label` on icon-only `SegmentedControlItem` elements.

---

## Segmented Control vs. Related Components

| Component | When to Use |
|-----------|------------|
| **Segmented Control** | 2–5 mutually exclusive options; all options visible; compact horizontal layout |
| **Radio Group** | 2–7 options in a form; options need descriptions; vertical layout preferred |
| **Button Group (Toggle)** | Similar visual; use when each item has an independent toggle state (multi-select) |
| **Tabs (Bottom Navigation)** | Top-level page navigation with associated route changes |
| **Select / Dropdown** | 5+ options; space-constrained; options don't need simultaneous visibility |
| **Chips (Filter)** | Multi-select filtering; options can be combined |
| **Pagination** | Position in a linear sequence; not mode/view selection |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Button Group** | Visual sibling; Button Group items are independent buttons, not a single-selection group |
| **Tabs / Bottom Navigation** | Semantic cousin for page-level navigation |
| **Radio Group** | Functional equivalent for form inputs; vertical layout |
| **Select** | Alternative for 5+ options or space-constrained contexts |
| **Icon** | Used inside `SegmentedControlItem` for icon or icon+label variants |
| **Chips** | Alternative when multi-select filtering is needed |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Standardised `SegmentedControl` + `SegmentedControlItem` API; extracted `SegmentedControl.Item` as a named sub-component with `Active=True` / `Active=False` states; defined sliding pill animation spec |
| 5.0 | Added icon support on `SegmentedControlItem` |
| 4.0 | Initial release |

---

## Testing Checklist

### Visual
- [ ] Container renders as pill shape with grey background
- [ ] Active segment renders with white background and shadow (Elevation/Level 1 equivalent)
- [ ] Inactive segments have no background (transparent over grey container)
- [ ] All segments are equal width (fill container equally)
- [ ] Container height is 40 px
- [ ] Labels use Label/Medium (14 px, Medium weight)
- [ ] Active label color is `content.base` (black)
- [ ] Inactive label color is `content.muted` (rgba(0,0,0,0.63))
- [ ] Disabled segments render with `content.disabled` color
- [ ] Entire-control disabled renders all items in disabled color with reduced opacity

### Animation
- [ ] Active pill slides smoothly to newly selected segment (200 ms ease)
- [ ] Text color transitions from muted → base on activation (150 ms)
- [ ] No animation under `prefers-reduced-motion: reduce`

### Behavior
- [ ] Clicking an inactive segment selects it and deselects the previous
- [ ] Only one segment can be active at a time
- [ ] Controlled mode: `value` + `onChange` required to update selection
- [ ] Uncontrolled mode: selection managed internally; `defaultValue` sets initial selection
- [ ] Clicking a disabled segment does nothing
- [ ] `onChange` fires with the string `value` of the newly selected segment

### Keyboard Navigation
- [ ] Tab enters the control (focus on active/selected segment)
- [ ] Tab exits the control to the next focusable element
- [ ] ArrowRight / ArrowDown moves selection to next segment
- [ ] ArrowLeft / ArrowUp moves selection to previous segment
- [ ] Arrow key navigation wraps (last → first, first → last)
- [ ] Home moves to and selects first segment
- [ ] End moves to and selects last segment
- [ ] Enter / Space selects the focused segment
- [ ] Disabled segments are skipped during arrow key navigation

### Accessibility
- [ ] Container has `role="tablist"` or `role="radiogroup"` (context-appropriate)
- [ ] Container has `aria-label` or `aria-labelledby`
- [ ] Each segment has `role="tab"` or `role="radio"`
- [ ] Active segment has `aria-selected="true"` or `aria-checked="true"`
- [ ] All inactive segments have `aria-selected="false"` or `aria-checked="false"`
- [ ] Only active segment has `tabindex="0"`; all others have `tabindex="-1"`
- [ ] Disabled segments have `aria-disabled="true"`
- [ ] Icon-only segments have `aria-label` on each item
- [ ] Screen reader announces group label and selected item on focus
- [ ] Content panel (if present) has `aria-controls` reference and `aria-live="polite"`
- [ ] Active label meets 4.5:1 contrast on white
- [ ] Inactive label meets 4.5:1 contrast on grey container background
- [ ] Focus ring is visible and meets 3:1 contrast
