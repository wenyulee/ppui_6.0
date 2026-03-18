# Selection Card Component Documentation

## Overview

The Selection Card component (formerly "Selection Card Group") presents a collection of tappable cards arranged in a **2-column grid** or a **horizontal scroll row**, allowing users to pick one or more options from a visually rich set. Each card shows a label and optional description in a rounded-rectangle tile; a selected card gains a highlighted border to communicate its chosen state.

Unlike a Radio Group (vertical text list) or Segmented Control (compact horizontal bar), the Selection Card is designed for scenarios where each option benefits from a larger touch target, may carry a supporting description or custom content, and where scanning multiple options side-by-side helps the user decide.

> **Formerly called:** Selection Card Group. The component wraps both the grid/scroll container (`SelectionCard`) and the individual tile (`SelectionCard.Item`).

**Component name:** `SelectionCard` / `SelectionCard.Item` / `SelectionCardModel`
**Figma nodes:** `893-14647` (Container), `5586-18590` (Parts — `SelectionCard.Item`)
**Platform:** React / TypeScript (Web), SwiftUI (iOS), Jetpack Compose (Android)
**Category:** Form / Selection / Input

---

## Component Variants

### By Layout (`SelectionCard` container)

| Layout | Node | Description | Typical Use |
|--------|------|-------------|-------------|
| **Grid** | `893:14626` | 2-column equal-width grid; items wrap to new rows | 4–8 options that users need to compare; payment categories, shipping types |
| **H-Scroll** | `893:14718` | Single horizontal scrolling row; items are fixed width | 4–8+ options where vertical space is limited; tags, quick actions, category pickers |

### By Item Content (`SelectionCard.Item`)

| Variant | Node | Height | Description |
|---------|------|--------|-------------|
| **Standard, Unselected** | `893:9260` | 72 px | Label + Description; default unselected state |
| **Standard, Selected** | `893:9265` | 72 px | Label + Description; selected state with highlight border |
| **Custom, Unselected** | `7209:15746` | 84 px | Arbitrary content slot; unselected |
| **Custom, Selected** | `7209:16308` | 84 px | Arbitrary content slot; selected |

### By Selection Mode

| Mode | Description |
|------|-------------|
| **Single select** | Exactly one card can be selected at a time (radio-like) |
| **Multi-select** | One or more cards can be selected simultaneously (checkbox-like) |

---

## Anatomy

### Grid Layout (2 columns)

```
┌────────────────────────────────────────┐
│  ┌──────────────┐  ┌──────────────┐   │
│  │ Label        │  │ Label        │   │  ← Row 1
│  │ Description  │  │ Description  │   │
│  └──────────────┘  └──────────────┘   │
│  ┌──────────────┐  ┌──────────────┐   │
│  │ Label        │  │ Label        │   │  ← Row 2 (selected card has highlight)
│  │ Description  │  │ Description  │   │
│  └──────────────┘  └──────────────┘   │
│     ···                               │
└────────────────────────────────────────┘
```

### H-Scroll Layout (single row, scrollable)

```
┌──────────────────────────────────────────────── →  scroll
│  ┌──────────────┐  ┌──────────────┐  ┌────────
│  │ Label        │  │ Label        │  │ Label
│  │ Description  │  │ Description  │  │ Descr…
│  └──────────────┘  └──────────────┘  └────────
└────────────────────────────────────────────────
```

### SelectionCard.Item Anatomy

```
┌──────────────────────────────┐  ← Card (222px wide, 72/84px tall)
│ Label           (Medium, 14) │  ← Label/Medium
│ Description     (Regular,14) │  ← Body/Medium (optional)
│ [Custom content slot]        │  ← Custom variant only (84px height)
└──────────────────────────────┘
```

---

## Size & Spacing Specifications

### Container

| Property | Grid | H-Scroll | Token |
|----------|------|----------|-------|
| Width | 100% (fluid) | 100% (scrollable) | — |
| Columns | 2 | 1 row | — |
| Column gap | ~8–12 px | ~8–12 px | `tokens.base.spacing[8]` |
| Row gap (Grid) | ~8–12 px | N/A | `tokens.base.spacing[8]` |
| Padding | 0 px | 0 px | `tokens.base.spacing[0]` |
| Overflow (H-Scroll) | `scroll-x` | — | — |

### SelectionCard.Item

| Property | Standard | Custom | Token |
|----------|----------|--------|-------|
| Width | 222 px (base) / fluid in grid | 222 px (base) | — |
| Height | 72 px | 84 px | — |
| Border radius | ~16 px | ~16 px | `tokens.base.border.radius[16]` |
| Horizontal padding | 16 px | 16 px | `tokens.base.spacing[16]` |
| Vertical padding | 12 px | 12 px | `tokens.base.spacing[12]` |
| Gap (label → description) | 2 px | 2 px | `tokens.base.spacing[2]` |

### Typography

| Element | Style | Size | Weight | Token |
|---------|-------|------|--------|-------|
| Label | Label/Medium | 14 px | Medium | `tokens.brand.text.fontfamily.label` / `tokens.brand.text.fontweight.label` |
| Description | Body/Medium | 14 px | Regular | `tokens.brand.text.fontfamily.body` / `tokens.brand.text.fontweight.body` |

Both share: `tokens.base.text.fontsize.ramp[14]`, `tokens.base.text.lineheight.ramp[14]`, `tokens.base.text.letterspacing[0]`

---

## Style Specifications

### Item — Unselected

| Property | Value | Token |
|----------|-------|-------|
| Background | White | `tokens.colorScheme.background.default` |
| Border width | 1 px | `tokens.base.border.size[1]` |
| Border color | `tokens.colorScheme.border.muted` | rgba(5,55,130,0.08) |
| Label color | `tokens.colorScheme.content.base` | Black |
| Description color | `tokens.colorScheme.content.muted` | rgba(0,0,0,0.63) |

### Item — Selected

| Property | Value | Token |
|----------|-------|-------|
| Background | White | `tokens.colorScheme.background.default` |
| Border width | 2 px | `tokens.base.border.size[2]` |
| Border color | `tokens.colorScheme.border.selected` | Brand blue / black |
| Label color | `tokens.colorScheme.content.base` | Black |
| Description color | `tokens.colorScheme.content.muted` | rgba(0,0,0,0.63) |

### Item — Hover (Desktop)

| Property | Value |
|----------|-------|
| Background | `tokens.colorScheme.background.hover` |
| Border | Unchanged from current state |

### Item — Disabled

| Property | Value |
|----------|-------|
| Background | `tokens.colorScheme.background.disabled` |
| Border | `tokens.colorScheme.border.muted` (1 px) |
| Label + Description | `tokens.colorScheme.content.disabled` |
| Cursor | `not-allowed` |

---

## State Specifications

### Unselected (Default)
- White card surface, 1 px muted border
- Label in base color, description in muted color

### Selected
- Border upgrades from 1 px muted → 2 px selected (brand/primary)
- Background remains white; selection is communicated purely through the border weight and color change
- In single-select mode, selecting a new card deselects the previous one
- In multi-select mode, each card toggles independently

### Hover (Desktop)
- Background tints to `tokens.colorScheme.background.hover`
- Border unchanged

### Pressed / Active
- Background tints to `tokens.colorScheme.background.pressed`
- Slight scale reduction (`transform: scale(0.98)`) for tactile feedback

### Focus
- Focus ring drawn around the card using `outline` at `tokens.colorScheme.border.focus`
- `outline-offset: 2px`

### Disabled
- Muted background and text; `pointer-events: none`
- `aria-disabled="true"` on the card element

---

## Props API

### TypeScript Interface

```typescript
import { ReactNode } from 'react';

// ─── SelectionCardModel ───────────────────────────────────────────────────────

export interface SelectionCardModel {
  /**
   * Unique identifier for this card item.
   */
  id: string;

  /**
   * Primary label text displayed in Label/Medium style.
   */
  label: string;

  /**
   * Optional supporting description in Body/Medium style.
   */
  description?: string;

  /**
   * Custom content rendered inside the card.
   * When provided, the card uses the 84 px (Custom) height variant.
   * Overrides label and description when set.
   */
  customContent?: ReactNode;

  /**
   * Leading icon or image rendered before the text.
   */
  leadingContent?: ReactNode;

  /**
   * When true, this card cannot be selected.
   * @default false
   */
  disabled?: boolean;
}

// ─── SelectionCard.Item ───────────────────────────────────────────────────────

export interface SelectionCardItemProps {
  /**
   * The data model for this card.
   */
  item: SelectionCardModel;

  /**
   * Whether this card is currently selected.
   * @default false
   */
  selected?: boolean;

  /**
   * Callback fired when the card is clicked or activated via keyboard.
   */
  onSelect?: (id: string) => void;

  /**
   * Additional class name(s) applied to the card element.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── SelectionCard (Container) ───────────────────────────────────────────────

export type SelectionCardLayout = 'grid' | 'h-scroll';
export type SelectionCardMode   = 'single' | 'multi';

export interface SelectionCardProps {
  /**
   * The array of card data models to render.
   */
  items: SelectionCardModel[];

  /**
   * Layout mode for the card container.
   * 'grid'     → 2-column wrapping grid
   * 'h-scroll' → single horizontal scrolling row
   * @default 'grid'
   */
  layout?: SelectionCardLayout;

  /**
   * Selection mode.
   * 'single' → only one card selected at a time
   * 'multi'  → multiple cards can be selected simultaneously
   * @default 'single'
   */
  mode?: SelectionCardMode;

  /**
   * Currently selected card ID(s) — controlled mode.
   * Pass a string for single mode; string[] for multi mode.
   */
  value?: string | string[];

  /**
   * Default selected ID(s) — uncontrolled mode.
   */
  defaultValue?: string | string[];

  /**
   * Callback fired when the selection changes.
   * Returns the new selected ID (single) or array of IDs (multi).
   */
  onChange?: (value: string | string[]) => void;

  /**
   * Accessible label for the card group.
   * Example: "Payment category", "Shipping option"
   */
  'aria-label'?: string;

  /**
   * ID of a visible element that labels the group.
   */
  'aria-labelledby'?: string;

  /**
   * When true, all cards in the group are disabled.
   * @default false
   */
  disabled?: boolean;

  /**
   * When true, at least one card must be selected.
   * @default false
   */
  required?: boolean;

  /**
   * Additional class name(s) applied to the container.
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

### 1. Basic Grid — Single Select

```tsx
import { SelectionCard } from '@ds/components';

const PAYMENT_CATEGORIES = [
  { id: 'shopping',    label: 'Shopping',    description: 'Online & in-store retail' },
  { id: 'food',        label: 'Food',        description: 'Restaurants & delivery'   },
  { id: 'travel',      label: 'Travel',      description: 'Flights, hotels & cars'   },
  { id: 'bills',       label: 'Bills',       description: 'Utilities & subscriptions'},
  { id: 'health',      label: 'Health',      description: 'Medical & wellness'       },
  { id: 'other',       label: 'Other',       description: 'Everything else'          },
];

export function CategoryPicker({ onSelect }) {
  return (
    <SelectionCard
      items={PAYMENT_CATEGORIES}
      layout="grid"
      mode="single"
      defaultValue="shopping"
      onChange={id => onSelect(id as string)}
      aria-label="Spending category"
    />
  );
}
```

### 2. Controlled Single-Select

```tsx
import { useState } from 'react';
import { SelectionCard } from '@ds/components';

const SHIPPING_OPTIONS = [
  { id: 'standard',  label: 'Standard',  description: '3–5 business days' },
  { id: 'express',   label: 'Express',   description: '1–2 business days' },
  { id: 'overnight', label: 'Overnight', description: 'Next business day' },
  { id: 'pickup',    label: 'Pick up',   description: 'Ready in 2 hours'  },
];

export function ShippingSelector() {
  const [method, setMethod] = useState('standard');

  return (
    <div>
      <h2 id="shipping-label">Choose a shipping method</h2>
      <SelectionCard
        items={SHIPPING_OPTIONS}
        layout="grid"
        mode="single"
        value={method}
        onChange={v => setMethod(v as string)}
        aria-labelledby="shipping-label"
        required
      />
      <p className="mt-2 text-sm text-muted">
        Selected: {SHIPPING_OPTIONS.find(o => o.id === method)?.label}
      </p>
    </div>
  );
}
```

### 3. Multi-Select Grid

```tsx
import { useState } from 'react';
import { SelectionCard, Button } from '@ds/components';

const INTERESTS = [
  { id: 'tech',     label: 'Technology',  description: 'Gadgets & software' },
  { id: 'finance',  label: 'Finance',     description: 'Investing & banking' },
  { id: 'travel',   label: 'Travel',      description: 'Destinations & tips' },
  { id: 'health',   label: 'Health',      description: 'Fitness & wellness'  },
  { id: 'food',     label: 'Food',        description: 'Recipes & dining'    },
  { id: 'sports',   label: 'Sports',      description: 'News & highlights'   },
];

export function InterestPicker({ onContinue }) {
  const [selected, setSelected] = useState<string[]>([]);

  return (
    <div>
      <h2 id="interests-label">What are you interested in?</h2>
      <p className="text-muted mb-4">Select all that apply</p>

      <SelectionCard
        items={INTERESTS}
        layout="grid"
        mode="multi"
        value={selected}
        onChange={v => setSelected(v as string[])}
        aria-labelledby="interests-label"
      />

      <Button
        size="large"
        variant="primary"
        disabled={selected.length === 0}
        onClick={() => onContinue(selected)}
        className="mt-6 w-full"
      >
        Continue ({selected.length} selected)
      </Button>
    </div>
  );
}
```

### 4. H-Scroll Layout

```tsx
import { SelectionCard } from '@ds/components';

const QUICK_AMOUNTS = [
  { id: '10',   label: '$10',  description: 'Quick send' },
  { id: '25',   label: '$25',  description: 'Popular'    },
  { id: '50',   label: '$50',  description: 'Popular'    },
  { id: '100',  label: '$100', description: 'Common'     },
  { id: '250',  label: '$250', description: ''           },
  { id: '500',  label: '$500', description: ''           },
];

export function QuickAmountPicker({ onSelect }) {
  return (
    <SelectionCard
      items={QUICK_AMOUNTS}
      layout="h-scroll"
      mode="single"
      onChange={v => onSelect(v as string)}
      aria-label="Quick amount"
    />
  );
}
```

### 5. Custom Content Cards (84 px variant)

```tsx
import { SelectionCard } from '@ds/components';
import { VisaIcon, McIcon, AmexIcon } from '@ds/icons';

const PAYMENT_METHODS = [
  {
    id: 'paypal-balance',
    label: 'PayPal Balance',
    description: '$230.00 available',
    customContent: null, // uses standard label+description
  },
  {
    id: 'visa-4242',
    label: 'Visa',
    description: '••••4242',
    leadingContent: <VisaIcon width={32} height={20} />,
  },
  {
    id: 'mc-8888',
    label: 'Mastercard',
    description: '••••8888',
    leadingContent: <McIcon width={32} height={20} />,
  },
  {
    id: 'amex-1234',
    label: 'Amex',
    description: '••••1234',
    leadingContent: <AmexIcon width={32} height={20} />,
    disabled: true,
  },
];

export function PaymentMethodCards({ onSelect }) {
  return (
    <SelectionCard
      items={PAYMENT_METHODS}
      layout="grid"
      mode="single"
      onChange={v => onSelect(v as string)}
      aria-label="Payment method"
    />
  );
}
```

### 6. With Heading and Validation

```tsx
import { useState } from 'react';
import { SelectionCard, Button } from '@ds/components';

const ACCOUNT_TYPES = [
  { id: 'personal',  label: 'Personal',  description: 'For individuals' },
  { id: 'business',  label: 'Business',  description: 'For companies'   },
  { id: 'nonprofit', label: 'Non-profit', description: 'For charities'  },
];

export function AccountTypeStep({ onNext }) {
  const [type, setType]     = useState('');
  const [touched, setTouched] = useState(false);
  const hasError = touched && !type;

  return (
    <form onSubmit={e => { e.preventDefault(); setTouched(true); if (type) onNext(type); }}>
      <h2 id="account-type-heading">What type of account do you need?</h2>

      <SelectionCard
        items={ACCOUNT_TYPES}
        layout="grid"
        mode="single"
        value={type}
        onChange={v => { setType(v as string); setTouched(true); }}
        aria-labelledby="account-type-heading"
        required
      />

      {hasError && (
        <p role="alert" className="text-negative text-sm mt-2">
          Please select an account type to continue.
        </p>
      )}

      <Button type="submit" variant="primary" size="large" className="mt-6 w-full">
        Continue
      </Button>
    </form>
  );
}
```

### 7. Loading Skeleton State

```tsx
import { Skeleton } from '@ds/components';

export function SelectionCardSkeleton({ count = 6 }: { count?: number }) {
  return (
    <div
      className="grid grid-cols-2 gap-2"
      aria-busy="true"
      aria-label="Loading options"
    >
      {Array.from({ length: count }).map((_, i) => (
        <Skeleton
          key={i}
          height={72}
          style={{ borderRadius: 16 }}
        />
      ))}
    </div>
  );
}
```

### 8. Dynamic Items from API

```tsx
import { useState, useEffect } from 'react';
import { SelectionCard } from '@ds/components';
import { SelectionCardSkeleton } from './SelectionCardSkeleton';

export function DynamicCategoryPicker({ onSelect }) {
  const [categories, setCategories] = useState([]);
  const [isLoading, setLoading]     = useState(true);

  useEffect(() => {
    fetchCategories()
      .then(setCategories)
      .finally(() => setLoading(false));
  }, []);

  if (isLoading) return <SelectionCardSkeleton count={6} />;

  return (
    <SelectionCard
      items={categories}
      layout="grid"
      mode="single"
      onChange={v => onSelect(v as string)}
      aria-label="Category"
    />
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern

Use `role="listbox"` / `role="option"` for single-select and `role="group"` with individual checkbox-like items for multi-select.

#### Single-Select (listbox pattern)

```html
<div
  role="listbox"
  aria-label="Shipping method"
  aria-required="true"
>
  <div
    role="option"
    aria-selected="false"
    tabindex="-1"
    id="opt-standard"
  >
    <span class="label">Standard</span>
    <span class="description">3–5 business days</span>
  </div>

  <div
    role="option"
    aria-selected="true"
    tabindex="0"
    id="opt-express"
  >
    <span class="label">Express</span>
    <span class="description">1–2 business days</span>
  </div>

  <div
    role="option"
    aria-selected="false"
    aria-disabled="true"
    tabindex="-1"
    id="opt-overnight"
  >
    <span class="label">Overnight</span>
    <span class="description">Next business day (unavailable)</span>
  </div>
</div>
```

#### Multi-Select (group + checkbox pattern)

```html
<div
  role="group"
  aria-label="Interests"
>
  <div
    role="checkbox"
    aria-checked="true"
    tabindex="0"
  >
    <span class="label">Technology</span>
    <span class="description">Gadgets & software</span>
  </div>

  <div
    role="checkbox"
    aria-checked="false"
    tabindex="0"
  >
    <span class="label">Finance</span>
    <span class="description">Investing & banking</span>
  </div>
</div>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Container (single) | `"listbox"` |
| `role` | Container (multi) | `"group"` |
| `role` | Item (single) | `"option"` |
| `role` | Item (multi) | `"checkbox"` |
| `aria-selected` | Each option (single) | `"true"` / `"false"` |
| `aria-checked` | Each checkbox (multi) | `"true"` / `"false"` |
| `aria-label` | Container | Descriptive group name |
| `aria-labelledby` | Container | ID of external heading |
| `aria-required` | Container | `"true"` when selection is mandatory |
| `aria-disabled` | Disabled item | `"true"` |
| `tabindex` | Selected/focused item (single) | `"0"` |
| `tabindex` | All others (single, listbox) | `"-1"` |
| `tabindex` | Each item (multi, checkboxes) | `"0"` |

### Keyboard Interaction — Single Select (Listbox)

| Key | Behavior |
|-----|----------|
| `Tab` | Moves focus into the listbox (to selected option or first); Tab exits |
| `ArrowDown` / `ArrowRight` | Moves focus and selection to the next option; wraps |
| `ArrowUp` / `ArrowLeft` | Moves focus and selection to previous option; wraps |
| `Home` | Moves focus and selection to first option |
| `End` | Moves focus and selection to last option |
| `Enter` / `Space` | Selects the focused option |

### Keyboard Interaction — Multi-Select (Checkboxes)

| Key | Behavior |
|-----|----------|
| `Tab` | Moves focus sequentially through all cards |
| `Space` | Toggles the focused card's selected state |
| `Enter` | Same as Space — toggles selection |

### Screen Reader Announcements

- On entering group: _"Shipping method, list"_ or _"Interests, group"_
- On selecting option: _"Express, 1–2 business days, selected, option 2 of 4"_
- On toggling checkbox: _"Technology, Gadgets & software, checked, 1 of 6"_
- Disabled item: _"Overnight, Next business day, dimmed, option 3 of 4"_

### Touch Targets

Cards are 72–84 px tall × fluid width — well above the 44 × 44 px minimum touch target requirement.

### Color Contrast

| Element | Minimum Ratio | WCAG |
|---------|---------------|------|
| Label text on white card | 4.5:1 | 1.4.3 |
| Description text on white card | 4.5:1 | 1.4.3 |
| Selected border vs. white card | 3:1 | 1.4.11 |
| Unselected border vs. white background | 3:1 | 1.4.11 |
| Disabled text | — | Informational; exempt per WCAG 1.4.3 |
| Focus ring | 3:1 | 1.4.11 |

---

## Animation Specifications

### Selection Transition (Border + Background)

```css
.selection-card-item {
  transition:
    border-color 150ms ease,
    border-width 150ms ease,
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.selection-card-item--selected {
  border-width: 2px;
  border-color: var(--tokens.colorScheme.border.selected);
}
```

### Press / Active Scale

```css
.selection-card-item:active {
  transform: scale(0.98);
  transition: transform 80ms ease;
}
```

### Focus Ring

```css
.selection-card-item:focus-visible {
  outline: 2px solid var(--tokens.colorScheme.border.focus);
  outline-offset: 2px;
}
```

### H-Scroll Snap (Optional)

```css
.selection-card-hscroll {
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
}

.selection-card-item {
  scroll-snap-align: start;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .selection-card-item {
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Container ──────────────────────────────────────────────────────── */
--selection-card-gap:              var(--tokens.base.spacing[8]);            /* 8px */
--selection-card-columns:          2;
--selection-card-overflow-hscroll: scroll;

/* ─── Item ────────────────────────────────────────────────────────────── */
--selection-card-item-width:       222px;
--selection-card-item-height:      72px;
--selection-card-item-height-custom: 84px;
--selection-card-item-radius:      var(--tokens.base.border.radius[16]);     /* 16px */
--selection-card-item-padding-x:   var(--tokens.base.spacing[16]);           /* 16px */
--selection-card-item-padding-y:   var(--tokens.base.spacing[12]);           /* 12px */
--selection-card-item-gap:         var(--tokens.base.spacing[2]);            /* 2px (label → description) */

/* ─── Colors — Unselected ─────────────────────────────────────────────── */
--selection-card-bg:               var(--tokens.colorScheme.background.default);
--selection-card-border:           var(--tokens.colorScheme.border.muted);
--selection-card-border-width:     var(--tokens.base.border.size[1]);        /* 1px */

/* ─── Colors — Selected ───────────────────────────────────────────────── */
--selection-card-border-selected:  var(--tokens.colorScheme.border.selected);
--selection-card-border-width-sel: var(--tokens.base.border.size[2]);        /* 2px */

/* ─── Colors — Hover / Pressed ────────────────────────────────────────── */
--selection-card-bg-hover:         var(--tokens.colorScheme.background.hover);
--selection-card-bg-pressed:       var(--tokens.colorScheme.background.pressed);

/* ─── Colors — Disabled ───────────────────────────────────────────────── */
--selection-card-bg-disabled:      var(--tokens.colorScheme.background.disabled);
--selection-card-border-disabled:  var(--tokens.colorScheme.border.muted);

/* ─── Typography ──────────────────────────────────────────────────────── */
--selection-card-label-family:     var(--tokens.brand.text.fontfamily.label);
--selection-card-label-size:       var(--tokens.base.text.fontsize.ramp[14]);  /* 14px */
--selection-card-label-weight:     var(--tokens.brand.text.fontweight.label);  /* Medium */
--selection-card-label-line-height:var(--tokens.base.text.lineheight.ramp[14]);/* 20px */
--selection-card-label-color:      var(--tokens.colorScheme.content.base);

--selection-card-desc-family:      var(--tokens.brand.text.fontfamily.body);
--selection-card-desc-size:        var(--tokens.base.text.fontsize.ramp[14]);  /* 14px */
--selection-card-desc-weight:      var(--tokens.brand.text.fontweight.body);   /* Regular */
--selection-card-desc-color:       var(--tokens.colorScheme.content.muted);
```

---

## Best Practices

### Do

- **Use for 4–8 options that benefit from a larger touch target** — Selection Cards are best when options need more visual space than a radio group's text rows but fewer than a full Modal.
- **Keep labels short and parallel in structure** — All labels in a group should be the same part of speech (all nouns, all verbs). "Shopping", "Travel", "Health" — not "Shopping", "Travel globally", "Health care".
- **Provide descriptions when the label alone is ambiguous** — "Express" means little without "1–2 business days" to anchor it.
- **Pre-select the most common option** (single-select) — Reduces required taps and reduces abandonment, especially in checkout flows.
- **Use H-Scroll for time or amount pickers** — Horizontal scroll cards are ideal for values in a series (amounts, dates, time slots) where the user scans left-to-right.
- **Use Grid for categorical choices** — Two-column grids work well for categories, shipping methods, and account types where options are conceptually parallel.
- **Use multi-select mode with a summary counter** — Show how many items are selected ("Continue (3 selected)") so users know their choices are registered.

### Don't

- **Don't use more than 8 cards in a grid** — Beyond 8 items the grid becomes a long scroll. Use a List with selectable rows or a Menu for large option sets.
- **Don't mix label-only and description cards in the same group** — Inconsistent card heights create visual imbalance. Either all cards have descriptions or none do.
- **Don't rely on color alone to communicate selection** — The border color change must be paired with a border width change (1 px → 2 px) and ideally a check icon, so the selection state is clear without relying solely on color.
- **Don't use the Selection Card as a navigation component** — Cards that navigate to a new page should use a standard Card with interactive behavior, not a Selection Card.
- **Don't nest selection interactions** — A `SelectionCard.Item` should not contain its own interactive elements (buttons, links) that would conflict with the card's own tap/select behavior.
- **Don't pre-select a "None" option in multi-select** — In multi-select mode, start with zero items selected; the empty state communicates "nothing yet chosen".

---

## Selection Card vs. Related Components

| Component | When to Use |
|-----------|------------|
| **Selection Card** | Visual, card-based single or multi-selection; options benefit from 2D layout; 4–8 items |
| **Radio Group** | Single selection; vertical text list; options can have descriptions; 2–7 items |
| **Checkbox Group** | Multi-selection; vertical text list; 2–7 items |
| **Segmented Control** | Single selection; 2–5 very short labels; compact horizontal bar |
| **List (Selectable)** | Single or multi-select from long lists (8+ items); items have rich metadata |
| **Menu** | Contextual action/option selection in an overlay; not a form field |
| **Chips (Filter)** | Multi-select filter tags; compact, persistent; combined with other UI |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Radio Group** | Functional alternative for single-select with vertical text layout |
| **Checkbox Group** | Functional alternative for multi-select with vertical text layout |
| **Segmented Control** | Alternative for very compact, 2–5 option switching |
| **List (Selectable)** | Alternative for long option lists (8+) with richer metadata |
| **Card** | Visual parent pattern; Selection Card is a specialised interactive Card |
| **Skeleton** | Loading placeholder for unloaded Selection Card items |
| **Button** | Adjacent CTA to proceed after selection is made |
| **Pagination / Progress Bar** | Sometimes accompany Selection Cards in multi-step onboarding flows |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Renamed from "Selection Card Group" to "Selection Card"; extracted `SelectionCard.Item` as a named sub-component with `Custom` and `Selected` variants; standardised Grid and H-Scroll layout props; added `mode` prop for single vs. multi-select |
| 5.1 | Added `leadingContent` slot to items; added H-Scroll layout |
| 5.0 | Added `customContent` slot (84 px item height variant) |
| 4.0 | Initial release — Grid layout, single-select, standard items only |

---

## Testing Checklist

### Visual
- [ ] Grid layout renders 2 equal-width columns with 8 px gap
- [ ] H-Scroll layout renders items in a single horizontal row with scroll
- [ ] Standard items are 72 px tall; Custom items are 84 px tall
- [ ] Item border radius is ~16 px
- [ ] Unselected cards have 1 px `border.muted` border, white background
- [ ] Selected cards have 2 px `border.selected` border, white background
- [ ] Label renders in Label/Medium (14 px, Medium weight)
- [ ] Description renders in Body/Medium (14 px, Regular weight)
- [ ] Disabled cards render in muted/disabled color scheme
- [ ] Leading content (icon/image) renders correctly in item leading slot
- [ ] Custom content renders in the 84 px variant correctly

### Behavior
- [ ] Single-select: selecting a card deselects the previous selection
- [ ] Multi-select: each card toggles independently
- [ ] `onChange` fires with the correct value (string for single, string[] for multi)
- [ ] Controlled mode: `value` prop drives selection; `onChange` required to update
- [ ] Uncontrolled mode: `defaultValue` sets initial selection
- [ ] Disabled card cannot be clicked or selected
- [ ] H-Scroll is horizontally scrollable; all items accessible via scroll

### Keyboard — Single Select (Listbox)
- [ ] Tab enters the container (focused on selected or first item)
- [ ] Tab exits the container
- [ ] ArrowDown / ArrowRight moves selection to next item
- [ ] ArrowUp / ArrowLeft moves selection to previous item
- [ ] Arrow keys wrap at boundaries
- [ ] Home moves to first; End moves to last
- [ ] Enter / Space selects the focused item

### Keyboard — Multi-Select
- [ ] Tab cycles through all cards
- [ ] Space toggles the focused card's selection
- [ ] Disabled cards are skipped in tab order

### Accessibility
- [ ] Container has `role="listbox"` (single) or `role="group"` (multi)
- [ ] Container has `aria-label` or `aria-labelledby`
- [ ] Each item has `role="option"` (single) or `role="checkbox"` (multi)
- [ ] `aria-selected` / `aria-checked` reflects current state
- [ ] Disabled items have `aria-disabled="true"`
- [ ] Focus ring is visible on keyboard focus
- [ ] Screen reader announces item label, description, and selected state
- [ ] Card text meets 4.5:1 contrast on white background
- [ ] Selected border meets 3:1 contrast vs. white card background
