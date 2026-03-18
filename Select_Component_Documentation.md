# Select / Dropdown Component Documentation

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
