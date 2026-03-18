# Component Documentation

## Table of Contents
1. [Accordion / Disclosure](#accordion--disclosure)
2. [Select / Dropdown](#select--dropdown)

---

# Accordion / Disclosure

## Description

The Accordion component is a collapsible container that expands and collapses to show or hide additional content. It's used to organize large amounts of content into manageable sections, reducing cognitive load and improving page layout efficiency. Accordions work well for FAQs, detailed settings, navigation menus, and content hierarchies where users need selective access to information.

**When to use:**
- Organizing related content into collapsible sections
- FAQs and help documentation
- Multi-step forms where users complete one section at a time
- Settings or configuration panels with many options
- Hierarchical navigation structures
- Reducing initial page height with optional content

**When NOT to use:**
- Content that users need to see all at once
- Critical information that should never be hidden
- Simple yes/no content that doesn't need grouping
- On mobile where collapsed content might be forgotten

---

## Component Structure

```
Accordion
├── AccordionItem
│   ├── AccordionTrigger (Label + Chevron Icon)
│   └── AccordionContent (Slot area with expandable content)
```

---

## Variants

| Variant | Use When | Example |
|---------|----------|---------|
| **Single Expand** | Only one item can be open at a time | Step-by-step wizard, Process flow |
| **Multi-Expand** | Multiple items can be open simultaneously | FAQ page, Settings panel |
| **Bordered** | Visual separation between items | Formal settings, Documentation |
| **Flat** | Minimal styling, subtle dividers | Inline help, Collapsible sections |

---

## States

### Accordion Item States

| State | Visual Indicator | Chevron Icon | Interactive |
|-------|------------------|--------------|-------------|
| **Collapsed** | Item contracted, content hidden | Chevron pointing down (▼) | Clickable, pointer cursor |
| **Hover (Collapsed)** | Subtle background change or shadow lift | Chevron down (▼) | Highlighted background |
| **Active/Focused (Collapsed)** | Focus ring around trigger | Chevron down (▼) | Keyboard focused |
| **Expanded** | Item expanded, content visible | Chevron pointing up (▲) | Clickable, pointer cursor |
| **Hover (Expanded)** | Subtle background change | Chevron up (▲) | Highlighted background |
| **Active/Focused (Expanded)** | Focus ring around trigger | Chevron up (▲) | Keyboard focused |
| **Disabled** | Grayed out, no interaction | Chevron (grayed) | Not clickable, cursor: not-allowed |
| **Transitioning** | Smooth animation during open/close | Chevron animates | No interaction during transition |

---

## Props / Properties

### AccordionContainer Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `type` | `'single' \| 'multiple'` | `'single'` | Whether only one or multiple items can be open |
| `disabled` | `boolean` | `false` | Disables all accordion items |
| `collapsible` | `boolean` | `false` | Allows closing the currently open item (only for single type) |
| `defaultValue` | `string \| string[]` | `undefined` | Initially expanded item(s) by value |
| `value` | `string \| string[]` | — | Controlled expanded item(s) |
| `onValueChange` | `(value) => void` | — | Callback when expanded item(s) change |
| `className` | `string` | — | CSS class for custom styling |

### AccordionItem Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `value` | `string` | — | Unique identifier for this item (required) |
| `disabled` | `boolean` | `false` | Disables this specific item |
| `className` | `string` | — | CSS class for custom styling |

### AccordionTrigger Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Label text or content |
| `icon` | `ReactNode` | ChevronDown | Custom icon component |
| `className` | `string` | — | CSS class for custom styling |

### AccordionContent Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Content to show when expanded (Slot) |
| `className` | `string` | — | CSS class for custom styling |

---

## Accessibility

### ARIA Attributes

```jsx
// Trigger button
<button
  role="button"
  aria-expanded={isOpen}        // "true" when expanded, "false" when collapsed
  aria-controls="panel-id"      // References the content region
  id="trigger-id"               // Referenced by aria-labelledby in content
/>

// Content region
<div
  role="region"
  id="panel-id"                 // Matches aria-controls
  aria-labelledby="trigger-id"  // References the trigger label
/>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Enter** | Toggle expand/collapse |
| **Space** | Toggle expand/collapse |
| **Tab** | Move to next accordion trigger |
| **Shift + Tab** | Move to previous accordion trigger |
| **Home** | (Optional) Jump to first accordion item |
| **End** | (Optional) Jump to last accordion item |
| **Arrow Down** | (Optional) Move to next item |
| **Arrow Up** | (Optional) Move to previous item |

### Screen Reader Announcement

- Trigger announced as: `"[Label], button, collapsed"` or `"[Label], button, expanded"`
- Content announced as: `"[Label] panel"` (when expanded)
- Chevron icon hidden from screen readers (`aria-hidden="true"`)

---

## Code Examples

### Basic Usage (Single Expand)

```jsx
import { Accordion, AccordionItem, AccordionTrigger, AccordionContent } from '@/components/Accordion'

export function FAQAccordion() {
  return (
    <Accordion type="single" collapsible>
      <AccordionItem value="faq-1">
        <AccordionTrigger>What is your return policy?</AccordionTrigger>
        <AccordionContent>
          We offer a 30-day money-back guarantee on all products. No questions asked.
        </AccordionContent>
      </AccordionItem>

      <AccordionItem value="faq-2">
        <AccordionTrigger>How long does shipping take?</AccordionTrigger>
        <AccordionContent>
          Standard shipping takes 5-7 business days. Express shipping available for 2-3 day delivery.
        </AccordionContent>
      </AccordionItem>

      <AccordionItem value="faq-3">
        <AccordionTrigger>Do you ship internationally?</AccordionTrigger>
        <AccordionContent>
          Yes, we ship to over 150 countries. Shipping costs vary by location.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
  )
}
```

### Multiple Expand

```jsx
<Accordion type="multiple">
  <AccordionItem value="settings-1">
    <AccordionTrigger>Account Settings</AccordionTrigger>
    <AccordionContent>
      {/* Form fields for account settings */}
    </AccordionContent>
  </AccordionItem>

  <AccordionItem value="settings-2">
    <AccordionTrigger>Privacy Settings</AccordionTrigger>
    <AccordionContent>
      {/* Privacy options */}
    </AccordionContent>
  </AccordionItem>

  <AccordionItem value="settings-3">
    <AccordionTrigger>Notification Preferences</AccordionTrigger>
    <AccordionContent>
      {/* Notification settings */}
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

### Controlled Component

```jsx
import { useState } from 'react'

export function ControlledAccordion() {
  const [openItems, setOpenItems] = useState(['section-1'])

  return (
    <Accordion
      type="multiple"
      value={openItems}
      onValueChange={setOpenItems}
    >
      {/* AccordionItems */}
    </Accordion>
  )
}
```

### With Disabled Items

```jsx
<Accordion type="single">
  <AccordionItem value="item-1">
    <AccordionTrigger>Available Section</AccordionTrigger>
    <AccordionContent>This section is available.</AccordionContent>
  </AccordionItem>

  <AccordionItem value="item-2" disabled>
    <AccordionTrigger>Locked Section (Disabled)</AccordionTrigger>
    <AccordionContent>This section is currently disabled.</AccordionContent>
  </AccordionItem>
</Accordion>
```

---

## Do's and Don'ts

### ✅ Do

- Use accordions for organizing lengthy content into logical sections
- Provide clear, descriptive labels that indicate what's inside
- Keep content within each section concise and focused
- Consider the mobile experience—accordions work especially well on small screens
- Ensure the first item is sometimes expanded by default to show content exists
- Use consistent animation timing for expand/collapse (200-300ms recommended)
- Make trigger buttons large enough for touch targets (min 44×44px on mobile)
- Disable items that aren't yet available rather than hiding them completely

### ❌ Don't

- Don't hide critical information inside accordions—users might not discover it
- Don't use accordions for content that's usually viewed together
- Don't nest accordions too deeply (max 2-3 levels)
- Don't use unexpressive labels like "More" or "Details"—be specific
- Don't forget to announce state with aria-expanded for screen readers
- Don't make accordions load asynchronously without indicating loading state
- Don't use accordions for navigation—use a proper navigation component
- Don't override the default keyboard behavior users expect

---

## Animation Specifications

### Expand Animation
- **Duration:** 200-250ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1) or ease-out
- **Properties Animated:**
  - Height: 0 → full content height
  - Opacity: 0 → 1 (optional, for smoother effect)
  - Chevron: rotate(0deg) → rotate(180deg)

### Collapse Animation
- **Duration:** 150-200ms
- **Easing:** cubic-bezier(0.7, 0, 0.84, 0) or ease-in
- **Properties Animated:**
  - Height: full content height → 0
  - Opacity: 1 → 0 (optional)
  - Chevron: rotate(180deg) → rotate(0deg)

---

## Design Tokens Used

- **Colors:**
  - Background: `surface.default`
  - Trigger hover: `surface.hover`
  - Border: `border.subtle`
  - Text: `text.primary`
  - Icon: `text.secondary`

- **Typography:**
  - Trigger text: `typography.button.medium`
  - Content text: `typography.body.regular`

- **Spacing:**
  - Trigger padding: `spacing.md` (12px vertical, 16px horizontal)
  - Content padding: `spacing.lg` (16px)
  - Item gap: `spacing.sm` (8px)

- **Borders:**
  - Radius: `border.radius.sm` (4px)
  - Width: 1px

---

---

# Select / Dropdown

## Description

The Select component is a dropdown menu that allows users to choose one option from a list. It's used when space is limited and you need to present multiple options without cluttering the interface. Selects are essential for forms, filters, and settings where users must make a choice from predefined options.

**When to use:**
- Form fields with 3+ options
- Choosing from a list of categories or values
- Applying filters or sorting
- Selecting configuration options
- Any scenario with limited screen space

**When NOT to use:**
- Only 2 options (use Radio buttons or Toggle)
- List is very long (100+ items) without search—use Autocomplete/Combobox
- Users need to see all options at once—use Radio buttons
- Users can select multiple options—use Checkboxes or Multi-select
- Content is hierarchical—use Nested Select or different component

---

## Component Structure

```
Select
├── SelectTrigger (Visible closed state)
├── SelectValue (Selected value display)
├── SelectIcon (Chevron or dropdown indicator)
├── SelectContent (Dropdown menu - opens on trigger click)
│   ├── SelectItem
│   ├── SelectItem
│   └── SelectItem
└── SelectViewport (Scrollable area for items)
```

---

## Variants

| Variant | Use When | Example |
|---------|----------|---------|
| **Default** | Standard select field | Form input, Filter dropdown |
| **Compact** | Space-constrained UI | Mobile, Dense data tables |
| **Full-Width** | Primary form field | Modal dialogs, Main forms |
| **With Icon** | Visual categorization | Enum-like selections (currency, language) |
| **With Description** | Detailed options needed | Selecting plans, complex choices |

---

## States

| State | Visual Indicator | Interaction |
|-------|------------------|-------------|
| **Closed - Default** | Shows selected value, chevron down (▼) | Clickable |
| **Closed - Hover** | Subtle background highlight or border change | Hover effect, pointer cursor |
| **Closed - Focus** | Focus ring around trigger | Keyboard focused |
| **Open** | Dropdown displayed, chevron up (▲) | Menu items visible and interactive |
| **Open - Item Hover** | Item highlighted with background color | Hover on individual items |
| **Open - Item Focus** | Item has focus indicator (ring or highlight) | Keyboard navigation |
| **Selected** | Checkmark or highlight on chosen item | Visual confirmation |
| **Disabled** | Grayed out, no interaction | cursor: not-allowed |
| **Empty State** | Placeholder text shown | No selection made |
| **Loading** | Spinner or skeleton in dropdown | Async content loading |

---

## Props / Properties

### Select Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `value` | `string` | — | Currently selected value |
| `defaultValue` | `string` | — | Initial selected value |
| `onValueChange` | `(value: string) => void` | — | Callback when selection changes |
| `disabled` | `boolean` | `false` | Disables the entire select |
| `placeholder` | `string` | "Select an option..." | Text shown when nothing selected |
| `name` | `string` | — | Form field name (for form submission) |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Size variant |
| `className` | `string` | — | CSS class for custom styling |

### SelectTrigger Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Content inside button |
| `icon` | `ReactNode` | ChevronDown | Custom icon |
| `className` | `string` | — | CSS class |

### SelectContent Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | SelectItem elements |
| `side` | `'top' \| 'bottom'` | `'bottom'` | Dropdown position |
| `align` | `'start' \| 'center' \| 'end'` | `'start'` | Horizontal alignment |
| `maxHeight` | `number` | `300` | Max height in pixels (enables scroll) |
| `className` | `string` | — | CSS class |

### SelectItem Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `value` | `string` | — | Value for this option (required) |
| `disabled` | `boolean` | `false` | Disables this option |
| `children` | `ReactNode` | — | Display label |
| `className` | `string` | — | CSS class |

---

## Accessibility

### ARIA Attributes

```jsx
// Trigger button
<button
  role="combobox"
  aria-expanded={isOpen}           // "true" when open, "false" when closed
  aria-haspopup="listbox"          // Indicates a listbox will appear
  aria-controls="select-content"   // References the dropdown
  id="select-trigger"
/>

// Dropdown menu
<div
  role="listbox"
  id="select-content"               // Matches aria-controls
  aria-label="Options"
>
  <div
    role="option"
    aria-selected={isSelected}     // "true" for selected item
    id="option-1"
  >
    Option Text
  </div>
</div>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Enter / Space** | Open dropdown or select focused item |
| **Escape** | Close dropdown |
| **Tab** | Move focus out of select (closes dropdown) |
| **Arrow Down** | Highlight next item (opens dropdown if closed) |
| **Arrow Up** | Highlight previous item (opens dropdown if closed) |
| **Home** | Jump to first item |
| **End** | Jump to last item |
| **Type Letters** | Jump to first item starting with that letter |

### Screen Reader Announcement

- Trigger announced as: `"[Label], combobox, [current value], [collapsed/expanded]"`
- Item announced as: `"[Option text], option, [position X of Y], [selected/not selected]"`

---

## Code Examples

### Basic Usage

```jsx
import { Select, SelectTrigger, SelectContent, SelectItem, SelectValue } from '@/components/Select'

export function CountrySelect() {
  const [country, setCountry] = useState('us')

  return (
    <Select value={country} onValueChange={setCountry}>
      <SelectTrigger>
        <SelectValue />
      </SelectTrigger>
      <SelectContent>
        <SelectItem value="us">United States</SelectItem>
        <SelectItem value="ca">Canada</SelectItem>
        <SelectItem value="mx">Mexico</SelectItem>
        <SelectItem value="uk">United Kingdom</SelectItem>
        <SelectItem value="au">Australia</SelectItem>
      </SelectContent>
    </Select>
  )
}
```

### With Placeholder and Default Value

```jsx
<Select defaultValue="" onValueChange={handleChange}>
  <SelectTrigger>
    <SelectValue placeholder="Choose a status..." />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="active">Active</SelectItem>
    <SelectItem value="inactive">Inactive</SelectItem>
    <SelectItem value="pending">Pending Review</SelectItem>
  </SelectContent>
</Select>
```

### With Disabled Items

```jsx
<Select>
  <SelectTrigger>
    <SelectValue />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="starter">Starter Plan</SelectItem>
    <SelectItem value="pro">Pro Plan</SelectItem>
    <SelectItem value="enterprise" disabled>Enterprise (Contact Sales)</SelectItem>
  </SelectContent>
</Select>
```

### With Size Variant

```jsx
<div className="space-y-4">
  <Select size="sm">
    <SelectTrigger><SelectValue /></SelectTrigger>
    <SelectContent>{/* items */}</SelectContent>
  </Select>

  <Select size="md">
    <SelectTrigger><SelectValue /></SelectTrigger>
    <SelectContent>{/* items */}</SelectContent>
  </Select>

  <Select size="lg">
    <SelectTrigger><SelectValue /></SelectTrigger>
    <SelectContent>{/* items */}</SelectContent>
  </Select>
</div>
```

### In a Form

```jsx
import { useForm } from 'react-hook-form'

export function SettingsForm() {
  const { register, handleSubmit, watch } = useForm()
  const selectedTheme = watch('theme')

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label htmlFor="theme">Theme</label>
      <Select value={selectedTheme} onValueChange={(val) => register('theme').onChange(val)}>
        <SelectTrigger id="theme">
          <SelectValue />
        </SelectTrigger>
        <SelectContent>
          <SelectItem value="light">Light Mode</SelectItem>
          <SelectItem value="dark">Dark Mode</SelectItem>
          <SelectItem value="auto">Auto (System)</SelectItem>
        </SelectContent>
      </Select>
      <button type="submit">Save Settings</button>
    </form>
  )
}
```

---

## Do's and Don'ts

### ✅ Do

- Use clear, concise labels for each option
- Sort options logically (alphabetically, by frequency, or by importance)
- Show the current selection when dropdown is closed
- Provide a default selection when possible
- Use disabled state for unavailable options (don't remove them)
- Keep option text brief—add descriptions in a separate field if needed
- Test keyboard navigation thoroughly
- Use consistent spacing and typography
- Make the trigger button large enough for touch (min 44×44px)

### ❌ Don't

- Don't use select for only 2 options—use Toggle or Radio buttons
- Don't nest selects within selects
- Don't use placeholder as the first option—use actual options
- Don't remove disabled options from the list
- Don't use very long lists without search/filter capability (100+ items)
- Don't change the dropdown width on open/close
- Don't forget to show the selected value prominently
- Don't use abbreviations without context ("EST" vs "Eastern Standard Time")

---

## Animation Specifications

### Open Animation
- **Duration:** 150-200ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Properties:**
  - Content: scale-y 0 → 1
  - Opacity: 0 → 1
  - Chevron: rotate(0deg) → rotate(180deg)

### Highlight Animation
- **Duration:** 50-100ms
- **Easing:** ease-out
- **Properties:**
  - Item background: fade to highlight color

---

## Design Tokens Used

- **Colors:**
  - Background: `surface.default`
  - Trigger hover: `surface.hover`
  - Item hover: `surface.selected`
  - Border: `border.default`
  - Text: `text.primary`
  - Icon: `text.secondary`

- **Typography:**
  - Trigger text: `typography.body.medium`
  - Item text: `typography.body.regular`

- **Spacing:**
  - Trigger padding: `spacing.sm` (8px vertical, 12px horizontal)
  - Content padding: `spacing.sm` (8px)
  - Item padding: `spacing.md` (12px horizontal, 10px vertical)
  - Item gap: 0

- **Borders:**
  - Radius: `border.radius.sm` (4px)
  - Width: 1px

---

## Related Components

- **Autocomplete / Combobox** — Use when users need to search/filter long lists
- **Multi-Select** — Use when users can choose multiple options
- **Radio Button Group** — Use for 2-3 options where all must be visible
- **Checkbox Group** — Use when multiple selections are needed
- **Tabs** — Use for switching between content sections (not a dropdown)

---

## Resources

- [ARIA: listbox role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/listbox_role)
- [ARIA: combobox pattern](https://www.w3.org/WAI/ARIA/apg/patterns/combobox/)
- [Radix UI Select](https://www.radix-ui.com/docs/primitives/components/select)
- [Material Design Select](https://m3.material.io/components/menus/overview)

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
