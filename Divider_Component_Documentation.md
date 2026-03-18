# Divider Component Documentation

## Overview

The Divider component is a visual separator used to create clear distinctions between sections, groups of content, or individual items. It establishes hierarchy, improves scannability, and communicates logical grouping within a layout. Dividers can be purely decorative or carry semantic meaning — separating navigation sections, form field groups, list items, or content panels. They are available in horizontal and vertical orientations, support optional inline labels, and can be customized in weight, style, and color to match the visual hierarchy of any surface.

---

## Component Variants

Divider supports five primary usage patterns:

### 1. **Horizontal Divider**
- **Purpose**: Separates vertically stacked content sections
- **Use Cases**: Between form sections, under headings, between list items, below page headers, inside cards
- **Behavior**: Static; occupies full width of its container by default
- **HTML Element**: `<hr>` (semantic) or `<div role="separator">`

### 2. **Vertical Divider**
- **Purpose**: Separates side-by-side content or inline elements
- **Use Cases**: Between toolbar actions, inside navigation bars, between stat columns, inline text separators
- **Behavior**: Occupies full height of its flex/grid container parent
- **HTML Element**: `<div role="separator" aria-orientation="vertical">`

### 3. **Divider with Label**
- **Purpose**: Horizontal divider with centred (or aligned) text label breaking the line
- **Use Cases**: "Or continue with", section headings inside forms, "Today" / date separators in feeds, "More options" collapser
- **Behavior**: Line breaks around the label; label is informational, not interactive by default
- **Visual**: Line — Label — Line (label can be left-, center-, or right-aligned)

### 4. **Divider with Icon**
- **Purpose**: Horizontal divider with a centered icon instead of text
- **Use Cases**: Decorative separators in marketing pages, section break in rich content editors, stylistic separations
- **Behavior**: Icon replaces the label slot; non-interactive by default
- **Visual**: Line — Icon — Line

### 5. **List Divider (Inset)**
- **Purpose**: Divider inside a list that respects inset padding to align with list item content rather than the container edge
- **Use Cases**: Between list items in menus, settings lists, contact lists, conversation threads
- **Behavior**: Left (or right) edge is inset to align past avatar/icon columns
- **Visual**: Partial-width line starting at a specified offset from the left

---

## Orientation

| Orientation | Direction | Container Requirement | Typical Usage |
|-------------|-----------|----------------------|---------------|
| **Horizontal** | Left → Right | Any block container | Between stacked sections |
| **Vertical** | Top → Bottom | Flex row or inline context | Between side-by-side elements |

---

## Style Variants

### **Solid** (Default)
- Line style: Solid, continuous
- Best For: Clear structural separations, form sections, card dividers
- Visual weight: Medium

### **Dashed**
- Line style: Dashed segments with equal gaps
- Best For: Soft separations, optional sections, subtle grouping
- Visual weight: Light–Medium

### **Dotted**
- Line style: Dot series
- Best For: Decorative separators, table row separators, lighter content grouping
- Visual weight: Light

### **Gradient**
- Line style: Solid, fading to transparent at one or both ends
- Best For: Elegant section breaks, marketing pages, hero sections
- Visual weight: Decorative

### **Double**
- Line style: Two parallel thin lines with a small gap between
- Best For: Strong section breaks, table headers, formal documents
- Visual weight: Heavy

---

## Weight Variants

| Weight | Thickness | Use Case |
|--------|-----------|----------|
| **Hairline** | 0.5px | Subtle table row separators, very dense UIs |
| **Thin** | 1px | **Default**, most general-purpose separations |
| **Medium** | 2px | Stronger section breaks, navigation separators |
| **Thick** | 4px | Major section separations, accent dividers |
| **Bold** | 8px | Decorative, marketing, or branded separators |

---

## Color Variants

| Color | Value | Use Case |
|-------|-------|----------|
| **Default** | #E5E7EB (Gray 200) | Standard divider on white/light backgrounds |
| **Subtle** | #F3F4F6 (Gray 100) | Low-contrast; dense list separators |
| **Strong** | #D1D5DB (Gray 300) | Higher contrast; primary section dividers |
| **Dark** | #374151 (Gray 700) | Dividers on dark surfaces |
| **Primary** | #3B82F6 (Blue) | Accent or branded dividers |
| **Success** | #10B981 (Green) | Semantic section grouping |
| **Warning** | #F59E0B (Amber) | Caution zone separators |
| **Danger** | #EF4444 (Red) | Critical separation or alert boundary |
| **Custom** | any | via `color` or `className` prop |

---

## Spacing (Margin)

Dividers support controlled vertical (horizontal divider) or horizontal (vertical divider) spacing:

| Spacing | Value | Use Case |
|---------|-------|----------|
| **None** | 0px | Tight separations; spacing managed by parent |
| **XS** | 4px | Dense list items |
| **SM** | 8px | Compact form sections |
| **MD** | 16px | **Default**, standard section breaks |
| **LG** | 24px | Generous section separation |
| **XL** | 32px | Major content blocks |
| **2XL** | 48px | Hero or marketing sections |

---

## Label Alignment (Divider with Label)

| Alignment | Description | Use Case |
|-----------|-------------|----------|
| **Center** | Label centered; equal lines on both sides | "Or", date separators |
| **Left** | Label flush left; full line on right | Section headings in sidebars |
| **Right** | Label flush right; full line on left | Right-aligned category labels |

---

## Props API

```typescript
interface DividerProps {
  // Orientation
  orientation?: 'horizontal' | 'vertical';    // Default: 'horizontal'

  // Appearance
  variant?: 'solid' | 'dashed' | 'dotted' | 'gradient' | 'double';
  weight?: 'hairline' | 'thin' | 'medium' | 'thick' | 'bold';
  color?: string;                              // CSS color value or token

  // Label / Icon
  label?: string | React.ReactNode;           // Text label or node
  icon?: React.ReactNode;                      // Icon in label slot
  labelAlign?: 'left' | 'center' | 'right';   // Label alignment
  labelSpacing?: number;                       // Gap between label and lines (px)

  // Spacing
  spacing?: 'none' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl';
  spacingTop?: 'none' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl';
  spacingBottom?: 'none' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl';

  // Inset (List Divider)
  inset?: number | string;                     // Left inset in px or CSS value
  insetRight?: number | string;                // Right inset

  // Accessibility
  role?: 'separator' | 'presentation' | 'none';
  aria-orientation?: 'horizontal' | 'vertical';
  aria-label?: string;                         // For semantic separators

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

---

## Code Examples

### **1. Basic Horizontal Divider**
```typescript
import { Divider } from '@components/divider';

export function SectionBreak() {
  return (
    <div>
      <section>
        <h2>Personal Information</h2>
        {/* form fields */}
      </section>

      <Divider spacing="lg" />

      <section>
        <h2>Account Settings</h2>
        {/* form fields */}
      </section>
    </div>
  );
}
```

### **2. Divider with Label ("Or")**
```typescript
import { Divider } from '@components/divider';
import { Button } from '@components/button';

export function AuthDivider() {
  return (
    <div className="space-y-4">
      <Button fullWidth variant="outlined" label="Continue with Google" />

      <Divider label="Or" labelAlign="center" spacing="sm" />

      <Button fullWidth color="primary" label="Sign in with email" />
    </div>
  );
}
```

### **3. Vertical Divider in Toolbar**
```typescript
import { Divider } from '@components/divider';
import { Bold, Italic, Underline, AlignLeft, AlignCenter, AlignRight } from 'lucide-react';

export function Toolbar() {
  return (
    <div className="flex items-center gap-1 p-1 border rounded-lg">
      <button aria-label="Bold"><Bold size={16} /></button>
      <button aria-label="Italic"><Italic size={16} /></button>
      <button aria-label="Underline"><Underline size={16} /></button>

      <Divider orientation="vertical" spacing="xs" />

      <button aria-label="Align left"><AlignLeft size={16} /></button>
      <button aria-label="Align center"><AlignCenter size={16} /></button>
      <button aria-label="Align right"><AlignRight size={16} /></button>
    </div>
  );
}
```

### **4. Inset List Divider**
```typescript
import { Divider } from '@components/divider';
import { Avatar } from '@components/avatar';

const contacts = [
  { id: 1, name: 'Sarah Johnson', role: 'Designer' },
  { id: 2, name: 'Alex Chen', role: 'Engineer' },
  { id: 3, name: 'Maria Lopez', role: 'PM' },
];

export function ContactList() {
  return (
    <ul>
      {contacts.map((contact, index) => (
        <>
          <li key={contact.id} className="flex items-center gap-3 py-3 px-4">
            <Avatar initials={contact.name[0]} size="md" />
            <div>
              <p className="font-medium">{contact.name}</p>
              <p className="text-sm text-gray-500">{contact.role}</p>
            </div>
          </li>
          {index < contacts.length - 1 && (
            <Divider
              key={`divider-${contact.id}`}
              inset={64}
              spacing="none"
              color="subtle"
            />
          )}
        </>
      ))}
    </ul>
  );
}
```

### **5. Dashed Divider for Optional Sections**
```typescript
import { Divider } from '@components/divider';

export function OptionalSection() {
  return (
    <div>
      <h3>Required Details</h3>
      {/* required fields */}

      <Divider variant="dashed" label="Optional" labelAlign="left" spacing="md" />

      <h3 className="text-gray-500">Additional Notes</h3>
      {/* optional fields */}
    </div>
  );
}
```

### **6. Divider with Icon**
```typescript
import { Divider } from '@components/divider';
import { Sparkles } from 'lucide-react';

export function DecorativeDivider() {
  return (
    <Divider
      icon={<Sparkles size={16} className="text-amber-400" />}
      variant="solid"
      color="#F59E0B"
      weight="thin"
      spacing="xl"
    />
  );
}
```

### **7. Date Separator in Feed**
```typescript
import { Divider } from '@components/divider';

export function MessageFeed() {
  return (
    <div className="space-y-4">
      <div className="message">Yesterday's message content here</div>

      <Divider
        label="Today"
        labelAlign="center"
        variant="solid"
        color="#D1D5DB"
        spacing="sm"
      />

      <div className="message">Today's message content here</div>
    </div>
  );
}
```

### **8. Thick Accent Divider**
```typescript
import { Divider } from '@components/divider';

export function SectionHeader() {
  return (
    <div>
      <h1 className="text-3xl font-bold">Our Story</h1>
      <Divider
        weight="thick"
        color="primary"
        spacing="md"
        style={{ width: '48px' }}
      />
      <p>We started with a simple idea…</p>
    </div>
  );
}
```

### **9. Gradient Fade Divider**
```typescript
import { Divider } from '@components/divider';

export function HeroSection() {
  return (
    <section>
      <h2>Featured Content</h2>
      {/* content */}
      <Divider
        variant="gradient"
        weight="medium"
        spacing="xl"
        style={{
          background: 'linear-gradient(to right, transparent, #E5E7EB, transparent)',
        }}
      />
    </section>
  );
}
```

---

## Accessibility Specifications

### **HTML Semantics**
- **`<hr>`**: Use the native `<hr>` element for meaningful horizontal thematic breaks — it carries `role="separator"` implicitly
- **`<div role="separator">`**: Use for vertical dividers or styled dividers where `<hr>` can't be used
- **`aria-orientation="vertical"`**: Always set on vertical dividers
- **`role="presentation"` or `role="none"`**: For purely decorative dividers that should be invisible to screen readers
- **`role="separator"` with `aria-label`**: For dividers that carry semantic meaning (e.g., "Between required and optional fields")

### **When to Use Which Role**
| Scenario | Element / Role |
|----------|---------------|
| Thematic break between content sections | `<hr>` |
| Vertical separator between toolbar buttons | `<div role="separator" aria-orientation="vertical">` |
| Purely decorative line | `<div role="presentation">` or `aria-hidden="true"` |
| Named section boundary ("Today") | `<hr aria-label="Today">` or labeled `<div role="separator">` |
| List item separator | `<li role="separator">` inside `<ul role="listbox">` |

### **Label Accessibility**
- Text labels inside Divider with Label are readable by screen readers
- Icon-only dividers should be `aria-hidden="true"` (decorative) or have an `aria-label` if semantic
- Label text should be meaningful: "Or sign in with email" not just "Or"

### **Keyboard Navigation**
- Dividers themselves are not focusable (they are not interactive)
- Tab order skips over dividers entirely
- Exception: If a divider contains an interactive element (e.g., a "Show more" button), that element is focusable

### **Screen Reader Behavior**
- `<hr>` / `role="separator"`: Some screen readers (NVDA, JAWS) announce "separator" when focus passes near it during reading
- `role="presentation"`: Completely skipped by screen readers
- Named separator: Announced as "Today, separator" when `aria-label="Today"` is set

### **Color Contrast**
- Divider line: 3:1 minimum contrast against adjacent background (WCAG 1.4.11 Non-Text Contrast)
- Default Gray 200 (#E5E7EB) on White (#FFFFFF) = 1.6:1 — marginally below threshold; use Gray 300 (#D1D5DB) for AA compliance
- Label text on divider background: 4.5:1 minimum
- Recommended: Use `color="strong"` (#D1D5DB) or darker for full WCAG AA compliance

### **Best Practices**
- Prefer `<hr>` for horizontal dividers wherever possible (native semantics)
- Use `role="presentation"` for decorative-only dividers to reduce screen reader noise
- Do not use dividers as the sole method of grouping — supplement with headings, regions, or explicit labels
- Never use a divider as a substitute for a heading (`<h2>`, `<h3>`)
- Space consistently: equal margins above and below convey equal visual weight to adjacent sections

---

## Design Tokens

### **Colors**
| Token | Value | Usage |
|-------|-------|-------|
| `--divider-color-subtle` | #F3F4F6 | Hairline list separators |
| `--divider-color-default` | #E5E7EB | Standard separators |
| `--divider-color-strong` | #D1D5DB | High-contrast separators |
| `--divider-color-dark` | #374151 | Dark surface separators |
| `--divider-label-color` | #6B7280 | Label text color |
| `--divider-label-bg` | #FFFFFF | Label background (to break line) |

### **Weight**
| Token | Value |
|-------|-------|
| `--divider-weight-hairline` | 0.5px |
| `--divider-weight-thin` | 1px |
| `--divider-weight-medium` | 2px |
| `--divider-weight-thick` | 4px |
| `--divider-weight-bold` | 8px |

### **Spacing**
| Token | Value |
|-------|-------|
| `--divider-spacing-xs` | 4px |
| `--divider-spacing-sm` | 8px |
| `--divider-spacing-md` | 16px |
| `--divider-spacing-lg` | 24px |
| `--divider-spacing-xl` | 32px |
| `--divider-spacing-2xl` | 48px |

### **Label**
| Property | Value |
|----------|-------|
| Font size | 12px |
| Font weight | 500 (medium) |
| Letter spacing | 0.5px |
| Label horizontal padding | 12px (gap from line) |
| Text transform | none (sentence case) |

---

## Best Practices

### **When to Use a Divider**
- Between logically distinct sections of a form or page
- Between items in a list where spacing alone isn't enough
- After a page header before the main content area
- Between stat/metric columns in a data row
- Inside cards to separate header from body or body from footer

### **When Not to Use a Divider**
- As a substitute for appropriate white space and margin (prefer spacing first)
- Between every list item regardless of density (over-dividing reduces hierarchy)
- To replace a heading or section label
- Inside very dense components where it adds visual clutter
- Purely to "fill space" — always have a functional reason

### **Visual Hierarchy**
- Use heavier dividers (`medium`, `thick`) for major section breaks
- Use lighter dividers (`hairline`, `thin`) for sub-item or list separations
- Use colored/accent dividers sparingly — reserve for branded or focal separators
- Maintain consistent divider weight across the same layout surface

### **Spacing Consistency**
- Give equal spacing above and below unless intentionally asymmetric (e.g., tighter below a heading)
- In forms: 24px above and below section dividers (`spacing="lg"`)
- In lists: 0px spacing (`spacing="none"`) with padding on list items
- In cards: Use the built-in card divider via `CardFooter divider` prop rather than a standalone Divider

### **Label Usage**
- Keep labels short (1–3 words)
- Use sentence case: "Or continue with" not "OR CONTINUE WITH"
- Use muted text color (`--divider-label-color`) to de-emphasize
- Avoid interactive elements inside divider labels (use a full row with a button instead)

### **Inset Dividers**
- Align the inset offset with the content column, not the container edge
- Standard avatar inset: 56px (40px avatar + 16px gap)
- Standard icon inset: 40px (24px icon + 16px gap)
- Keep inset consistent across an entire list

---

## Common Patterns

### **Form Section Divider**
```
[Section A fields]
─────────────────────  ← Divider, spacing="lg"
[Section B fields]
```

### **Auth "Or" Divider**
```
[Social login button]
────────── Or ──────────  ← Divider with label="Or"
[Email login form]
```

### **Toolbar Vertical Divider**
```
[B] [I] [U]  |  [≡] [≡] [≡]
              ↑
        Vertical divider
```

### **List Inset Divider**
```
[Avatar]  Name          ← List item
             ────────── ← Inset divider (left edge at content column)
[Avatar]  Name          ← List item
```

### **Feed Date Separator**
```
[Message from yesterday]
────────── Today ──────────  ← Divider with label="Today"
[Message from today]
```

---

## Related Components

- **CardFooter**: Uses internal divider via `divider` prop — prefer this over standalone Divider inside cards
- **List**: Can render inset dividers between items automatically via `divider` prop
- **Menu**: Uses hairline dividers between menu item groups
- **Tabs**: Uses a bottom border (a specialized divider) beneath the tab list
- **Spacer**: For spacing-only gaps without any visual line

---

## Version History

- **v1.0**: Horizontal solid divider, color and weight variants
- **v1.1**: Vertical orientation, spacing tokens
- **v1.2**: Label support (text and icon), label alignment
- **v2.0**: Inset prop, dashed/dotted/double/gradient variants, full token set
- **v2.1**: Improved `<hr>` semantics by default, `role` prop, accessibility audit, reduced-motion compliance

---

## Testing Checklist

- [ ] Horizontal divider renders full width of container
- [ ] Vertical divider renders full height of flex container
- [ ] All style variants render correctly (solid, dashed, dotted, gradient, double)
- [ ] All weight variants show correct thickness
- [ ] Color prop applies custom color correctly
- [ ] Spacing variants add correct margin above/below
- [ ] Label renders centred between two lines by default
- [ ] Label alignment (left, center, right) works correctly
- [ ] Icon renders in label slot in place of text
- [ ] Inset prop offsets left edge correctly
- [ ] `<hr>` used by default for horizontal dividers
- [ ] `role="separator"` applied to vertical dividers
- [ ] `aria-orientation="vertical"` set on vertical dividers
- [ ] Decorative dividers have `role="presentation"` or `aria-hidden="true"`
- [ ] Named dividers have `aria-label` set correctly
- [ ] Dividers are not keyboard-focusable
- [ ] Label text is readable by screen readers
- [ ] Color contrast of divider line ≥ 3:1 against background
- [ ] Label text contrast ≥ 4.5:1
- [ ] Renders correctly in RTL layouts
