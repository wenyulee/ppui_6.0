# Button Component Documentation

## Description

The Button component is a fundamental interactive element that triggers actions or navigates to destinations. Buttons are the most commonly used call-to-action elements in interfaces and can be customized with different styles, sizes, and states to indicate their importance and function. Proper button design is critical for usability and should clearly communicate their purpose to users.

**When to use:**
- Triggering primary actions (Submit, Save, Publish)
- Confirming decisions or destructive actions
- Navigating to different pages or sections
- Opening modals, menus, or other UI components
- Toggling features or states on/off
- Submitting forms
- Resetting or canceling operations
- Any interactive action that requires user initiation

**When NOT to use:**
- Navigation to a different page (use links instead)
- Wrapping large content areas (use proper layout)
- Disabled state permanently (remove button or explain why)
- Multiple buttons that perform conflicting actions
- As a label or purely informational element

---

## Component Structure

```
Button
├── ButtonIcon (Optional leading icon)
├── ButtonLabel (Text content)
└── ButtonTrailingIcon (Optional trailing icon)
```

---

## Button Types & Variants

### Type 1: Solid/Filled Button
**Usage:** Primary actions with high emphasis

```
┌──────────────────┐
│  Primary Button  │  Solid color background
└──────────────────┘
```

- **Appearance:** Solid background color, white/contrasting text
- **Primary variant:** Brand color background
- **Secondary variant:** Gray background
- **Emphasis:** Highest, use sparingly (1 per section)
- **Use cases:** Submit, Save, Publish, Create, Confirm

### Type 2: Outlined Button
**Usage:** Secondary actions with medium emphasis

```
┌──────────────────┐
│ Secondary Button │  Colored border, transparent background
└──────────────────┘
```

- **Appearance:** Colored border, transparent background, colored text
- **Hover state:** Subtle background tint
- **Emphasis:** Medium, can use multiple per section
- **Use cases:** Cancel, Secondary actions, Optional selections

### Type 3: Ghost/Text Button
**Usage:** Tertiary actions with low emphasis

```
Ghost Button  ← Minimal styling, text only
```

- **Appearance:** No border, transparent background, colored text
- **Hover state:** Text color darker or subtle background
- **Emphasis:** Lowest, maximum flexibility
- **Use cases:** Help, Skip, More options, Learn more

### Type 4: Danger Button
**Usage:** Destructive actions requiring caution

```
┌──────────────────┐
│   Delete Item    │  Red/destructive color, solid
└──────────────────┘
```

- **Color:** Red/destructive color (#EF4444)
- **Appearance:** Similar to solid, but warning color
- **Confirmation:** Often paired with modal confirmation
- **Use cases:** Delete, Remove, Clear all, Destructive actions

### Type 5: Icon Button
**Usage:** Actions represented by icons only

```
┌────┐
│ 🔍  │  Icon inside button, no text
└────┘
```

- **Size:** Usually compact (32-48px)
- **Icon size:** 20-24px
- **Shape:** Circle or square
- **Use cases:** Close, Search, Menu, Share, Settings

---

## Size Variants

| Size | Height | Padding | Text | Icon | Use Case |
|------|--------|---------|------|------|----------|
| **XS** | 24px | 4px 8px | 12px | 16px | Compact UI, inline |
| **SM** | 32px | 6px 12px | 13px | 18px | Small contexts |
| **MD** | 40px | 8px 16px | 14px | 20px | Standard, default |
| **LG** | 48px | 12px 20px | 16px | 24px | Prominent, mobile |
| **XL** | 56px | 16px 24px | 18px | 28px | Hero, large CTAs |

---

## Style Variants

| Style | Background | Text Color | Border | Use Case |
|-------|-----------|-----------|--------|----------|
| **Solid** | Brand color | White | None | Primary actions |
| **Outlined** | Transparent | Brand color | Brand color | Secondary actions |
| **Ghost** | Transparent | Brand color | None | Tertiary actions |
| **Soft** | Brand 10% opacity | Brand color | None | Tertiary with subtle hint |
| **Gradient** | Gradient fill | White | None | Premium, prominent |

---

## Color Variants

| Color | Primary Use | Secondary | Tertiary |
|-------|------------|-----------|----------|
| **Primary** | Primary actions | Secondary actions | Ghost actions |
| **Secondary** | Alternative primary | Less emphasized | Info/Help |
| **Success** | Confirmation, completion | Approvals | Positive actions |
| **Warning** | Caution, alerts | Attention needed | Non-critical warnings |
| **Danger** | Destructive actions | Delete, remove | Destructive confirmations |
| **Info** | Informational CTA | Learn more | Help, support |

---

## States

| State | Visual | Interaction |
|-------|--------|-------------|
| **Default** | Normal appearance | Clickable, pointer cursor |
| **Hover** | Subtle enhancement | Slight shadow or color shift |
| **Active/Pressed** | Darker or inset effect | Immediate visual feedback |
| **Focus** | Focus ring visible | Keyboard navigation |
| **Disabled** | Grayed out, 50% opacity | No interaction, cursor not-allowed |
| **Loading** | Spinner or skeleton | No click, waiting state |
| **Error** | Error color, optional message | Shows validation error |

---

## Props / Properties

### Button Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Button content (text or elements) |
| `variant` | `'solid' \| 'outlined' \| 'ghost' \| 'soft'` | `'solid'` | Button style |
| `color` | `'primary' \| 'secondary' \| 'success' \| 'warning' \| 'danger' \| 'info'` | `'primary'` | Color semantic |
| `size` | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | `'md'` | Button size |
| `shape` | `'rounded' \| 'square' \| 'circle'` | `'rounded'` | Border radius |
| `fullWidth` | `boolean` | `false` | Stretch to parent width |
| `disabled` | `boolean` | `false` | Disable interaction |
| `loading` | `boolean` | `false` | Show loading state |
| `onClick` | `() => void` | — | Click handler |
| `type` | `'button' \| 'submit' \| 'reset'` | `'button'` | HTML button type |
| `icon` | `ReactNode` | — | Leading icon |
| `iconPosition` | `'left' \| 'right'` | `'left'` | Icon placement |
| `trailingIcon` | `ReactNode` | — | Trailing icon |
| `aria-label` | `string` | — | Accessible label (for icon buttons) |
| `badge` | `number \| string` | — | Badge indicator |
| `className` | `string` | — | Custom CSS class |

---

## Accessibility

### ARIA Attributes

```jsx
// Basic Button
<button
  type="button"
  aria-label="Submit form"  // Descriptive label
>
  Submit
</button>

// Icon-Only Button
<button
  type="button"
  aria-label="Close dialog"  // REQUIRED for icon buttons
  aria-describedby="close-help"
>
  ✕
</button>

// Loading State
<button
  type="submit"
  disabled
  aria-busy="true"           // Indicates loading state
  aria-label="Submitting form, please wait"
>
  <span aria-hidden="true">⏳</span> Submitting...
</button>

// Danger Action
<button
  type="button"
  aria-label="Delete this item permanently. Cannot be undone."
  className="btn-danger"
>
  Delete
</button>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Focus on button |
| **Shift + Tab** | Focus previous element |
| **Enter** | Activate button |
| **Space** | Activate button (alternative) |

### Screen Reader Announcement

- Standard button: `"[Text], button"`
- Icon button: `"[aria-label], button"`
- Loading button: `"[Text], button, loading"`
- Disabled button: `"[Text], button, disabled"`
- With badge: `"[Text], button, 5 items"`

---

## Code Examples

### Basic Solid Button

```jsx
import { Button } from '@/components/Button'

export function BasicButton() {
  const handleClick = () => {
    console.log('Button clicked!')
  }

  return (
    <Button onClick={handleClick}>
      Click Me
    </Button>
  )
}
```

### Color Variants

```jsx
<div className="space-y-2">
  <Button color="primary">Primary</Button>
  <Button color="secondary">Secondary</Button>
  <Button color="success">Success</Button>
  <Button color="warning">Warning</Button>
  <Button color="danger">Danger</Button>
  <Button color="info">Info</Button>
</div>
```

### Style Variants

```jsx
<div className="space-y-2">
  <Button variant="solid">Solid Button</Button>
  <Button variant="outlined">Outlined Button</Button>
  <Button variant="ghost">Ghost Button</Button>
  <Button variant="soft">Soft Button</Button>
</div>
```

### Size Variants

```jsx
<div className="flex gap-2 items-center">
  <Button size="xs">XS</Button>
  <Button size="sm">Small</Button>
  <Button size="md">Medium</Button>
  <Button size="lg">Large</Button>
  <Button size="xl">XL</Button>
</div>
```

### Icon Button

```jsx
import { SearchIcon, CloseIcon, SettingsIcon } from '@/icons'

export function IconButtons() {
  return (
    <div className="flex gap-2">
      <Button size="md" icon={<SearchIcon />} aria-label="Search" />
      <Button size="md" icon={<SettingsIcon />} aria-label="Settings" />
      <Button size="md" icon={<CloseIcon />} aria-label="Close" />
    </div>
  )
}
```

### Button with Icon + Text

```jsx
import { SaveIcon, TrashIcon, EditIcon } from '@/icons'

export function ButtonsWithIcons() {
  return (
    <div className="flex gap-2">
      <Button icon={<SaveIcon />}>Save Changes</Button>
      <Button icon={<EditIcon />} variant="outlined">Edit</Button>
      <Button icon={<TrashIcon />} color="danger" variant="ghost">Delete</Button>
    </div>
  )
}
```

### Loading State

```jsx
import { Button } from '@/components/Button'
import { useState } from 'react'

export function LoadingButton() {
  const [isLoading, setIsLoading] = useState(false)

  const handleSubmit = async () => {
    setIsLoading(true)
    try {
      await submitForm()
    } finally {
      setIsLoading(false)
    }
  }

  return (
    <Button
      type="submit"
      loading={isLoading}
      disabled={isLoading}
      onClick={handleSubmit}
    >
      {isLoading ? 'Submitting...' : 'Submit'}
    </Button>
  )
}
```

### Danger/Destructive Action

```jsx
import { Trash2Icon } from '@/icons'

export function DeleteButton() {
  const [showConfirm, setShowConfirm] = useState(false)

  const handleDelete = () => {
    deleteItem()
    setShowConfirm(false)
  }

  return (
    <>
      <Button
        color="danger"
        icon={<Trash2Icon />}
        onClick={() => setShowConfirm(true)}
      >
        Delete Item
      </Button>

      {showConfirm && (
        <Modal>
          <p>Are you sure? This cannot be undone.</p>
          <Button color="danger" onClick={handleDelete}>Delete</Button>
          <Button variant="outlined" onClick={() => setShowConfirm(false)}>Cancel</Button>
        </Modal>
      )}
    </>
  )
}
```

### Full Width Button

```jsx
<Button fullWidth color="primary" size="lg">
  Continue to Next Step
</Button>
```

### Button Group/Form Actions

```jsx
export function FormActions() {
  return (
    <div className="flex gap-2 justify-end">
      <Button variant="outlined">Cancel</Button>
      <Button color="primary">Save Changes</Button>
      <Button color="danger" variant="ghost">Reset</Button>
    </div>
  )
}
```

### Disabled States

```jsx
<div className="space-y-2">
  <Button disabled>Disabled Button</Button>
  <Button color="danger" disabled>Disabled Danger</Button>
  <Button variant="outlined" disabled>Disabled Outlined</Button>
  <Button variant="ghost" disabled>Disabled Ghost</Button>
</div>
```

---

## Do's and Don'ts

### ✅ Do

- Use clear, action-oriented button text (verbs: Save, Delete, Submit)
- Limit primary buttons to one per section
- Use color semantically (danger = red, success = green)
- Make buttons at least 44×44px for touch targets
- Provide aria-label for icon-only buttons
- Show loading state during async operations
- Use disabled state instead of hiding buttons
- Ensure sufficient color contrast (4.5:1 minimum)
- Align related buttons in groups
- Test keyboard navigation

### ❌ Don't

- Don't use button text that's vague ("Click Here", "Go", "Submit")
- Don't mix multiple button styles on same screen unnecessarily
- Don't use buttons for navigation (use links)
- Don't disable buttons without explanation
- Don't make buttons too small for touch interaction
- Don't use only color to indicate button state
- Don't place primary and danger buttons next to each other
- Don't forget disabled state in design
- Don't use too many buttons per section
- Don't make buttons wider than necessary

---

## Animation Specifications

### Click/Press Animation
- **Duration:** 150ms
- **Easing:** cubic-bezier(0.4, 0, 0.2, 1)
- **Animation:**
  - Scale: 1 → 0.98 → 1
  - Opacity (shadow): 0.2 → 0.4 → 0.3

### Hover Animation (Non-touch)
- **Duration:** 200ms
- **Easing:** ease-out
- **Animation:**
  - Shadow: subtle → elevated
  - Background: slight darken/lighten
  - Transform: none (no scale on hover)

### Loading Animation
- **Duration:** Continuous
- **Easing:** linear
- **Animation:**
  - Spinner rotation: 0deg → 360deg
  - Optional: subtle pulse or shimmer

### Focus Ring Animation
- **Duration:** 100ms
- **Easing:** ease-out
- **Animation:**
  - Ring scale: 0 → 1
  - Ring opacity: 0 → 1

---

## Design Tokens Used

- **Colors:**
  - Primary: Brand color (`#3B82F6`)
  - Secondary: Gray (`#6B7280`)
  - Success: Green (`#10B981`)
  - Warning: Amber (`#F59E0B`)
  - Danger: Red (`#EF4444`)
  - Info: Blue (`#3B82F6`)

- **Typography:**
  - Small: 12px, 500 weight
  - Medium: 14px, 500 weight (default)
  - Large: 16px, 500 weight
  - XL: 18px, 600 weight

- **Spacing:**
  - XS: 4px vertical, 8px horizontal
  - SM: 6px vertical, 12px horizontal
  - MD: 8px vertical, 16px horizontal
  - LG: 12px vertical, 20px horizontal
  - XL: 16px vertical, 24px horizontal
  - Icon gap: 8px

- **Borders:**
  - Radius (rounded): 6px
  - Radius (square): 0px
  - Radius (circle): 50%
  - Border width: 1px (outlined)

- **Shadows:**
  - None: Default (solid)
  - Hover: Subtle elevation
  - Active: Inset or reduced shadow

---

## Related Components

- **Link** — Text-based navigation
- **IconButton** — Icon-only actions
- **ButtonGroup** — Multiple buttons together
- **Dropdown** — Button with menu
- **Toggle** — On/off button state
- **Badge** — Notification indicator on button

---

## Best Practices

1. **Hierarchy:** Use visual weight to indicate importance
2. **Consistency:** Maintain consistent button styling across app
3. **Feedback:** Always provide visual feedback on interaction
4. **Accessibility:** Every button must be keyboard accessible
5. **Clarity:** Text must clearly indicate action
6. **Spacing:** Adequate padding and gaps between buttons
7. **Loading:** Show state during async operations
8. **Confirmation:** Confirm destructive actions
9. **Mobile:** Ensure touch-friendly sizes on mobile
10. **Context:** Match button prominence to content importance

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
**Button Types Documented:** 5 variants (Solid, Outlined, Ghost, Danger, Icon)
