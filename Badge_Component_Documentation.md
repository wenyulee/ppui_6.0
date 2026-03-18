# Badge Component Documentation

## Description

The Badge component is a small label that displays status, category, count, or other supplementary information. Badges are used to highlight, categorize, or draw attention to specific items without taking up significant space. They're commonly found on avatars, navigation items, product listings, and throughout interfaces to provide at-a-glance context.

**When to use:**
- Displaying notification counts or unread messages
- Showing status indicators (active, inactive, pending, approved, rejected)
- Categorizing items with tags or labels
- Highlighting important information or alerts
- Marking new, updated, or featured items
- Indicating user roles or permissions
- Displaying metric counts or progress indicators
- Showing system or environmental states

**When NOT to use:**
- Critical information that must be prominent (use Alert instead)
- Information requiring detailed explanation (use Tooltip or dedicated section)
- Large amounts of text (badges are compact)
- As the primary call-to-action (use Button instead)
- When you need interactive selection (use Checkbox or Tag)

---

## Component Structure

```
Badge
├── BadgeIcon (Optional leading icon)
├── BadgeText (Label or count text)
├── BadgeDot (Optional status indicator dot)
└── BadgeRemove (Optional close/dismiss button)
```

---

## Badge Types & Variants

### Type 1: Status Badge
**Usage:** Displaying status states with semantic colors

```
┌────────────┐
│ ● Active   │  Colored dot + status text
└────────────┘
```

- **Status types:**
  - **Active (Green):** #10B981 — Item is active/online
  - **Pending (Yellow):** #F59E0B — Awaiting action or review
  - **Inactive (Gray):** #6B7280 — Item is inactive/offline
  - **Error (Red):** #EF4444 — Error or critical state
  - **Info (Blue):** #3B82F6 — Informational status

- **Variants:**
  - Filled: Solid color background with white text
  - Outlined: Border with colored text
  - Dot only: Just the indicator dot (12px diameter)

### Type 2: Count Badge
**Usage:** Displaying notification counts or numbers

```
┌─────┐
│  12 │  White text on colored background
└─────┘
```

- **Placement:** Top-right corner of icons/avatars
- **Styles:** Filled background, typically red or blue
- **Overflow:** Shows "99+" for numbers over 99
- **Semantics:**
  - Red: Urgent/error count
  - Blue: Informational count
  - Gray: Neutral count

### Type 3: Category/Tag Badge
**Usage:** Categorizing or tagging items

```
┌──────────────┐
│ 🏷️ Feature   │  Icon + category label
└──────────────┘
```

- **Content:** Category name or topic
- **Icon:** Optional leading icon for visual hierarchy
- **Dismissible variant:** Include "x" button for removal
- **Colors:** Semantic or category-specific colors

### Type 4: Label Badge
**Usage:** Simple text labels for classification

```
┌─────────┐
│ Approved │  Text only, subtle styling
└─────────┘
```

- **Styles:**
  - Filled: Solid background color
  - Outlined: Border only with colored text
  - Ghost: Minimal (text color only)
  - Subtle: Low contrast background

### Type 5: Dismissible Badge
**Usage:** Removable tags or temporary labels

```
┌──────────────┐
│ Feature ╳    │  Text + close button
└──────────────┘
```

- **Close button:** Small × or icon button
- **Hover state:** Button becomes prominent
- **Animation:** Slide out on dismiss
- **Callback:** Triggers `onDismiss` event

---

## Size Variants

| Size | Px | Use Case | Text Size | Padding |
|------|-----|----------|-----------|---------|
| **XS** | 16-20 | Compact UI, dense lists | 10px | 2px 6px |
| **SM** | 20-24 | Default, most common | 12px | 4px 8px |
| **MD** | 24-32 | Prominent, highlighted | 14px | 6px 12px |
| **LG** | 32-40 | Large displays, headers | 16px | 8px 16px |

---

## Shape Variants

| Shape | Use Case | Border Radius |
|-------|----------|---------------|
| **Rounded Pill** | Standard, most common | 9999px / 50% |
| **Rounded Square** | Modern, balanced | 6-8px |
| **Square** | Compact, minimal | 0px |
| **Circle** | Count badges, indicators | 50% |

---

## Color Variants

### Semantic Colors

| Semantic | Color | Use Case |
|----------|-------|----------|
| **Success** | #10B981 (Green) | Approved, active, available |
| **Warning** | #F59E0B (Amber) | Pending, caution, attention |
| **Error** | #EF4444 (Red) | Error, rejected, unavailable |
| **Info** | #3B82F6 (Blue) | Information, neutral, default |
| **Primary** | Brand color | Product feature, highlight |
| **Secondary** | Gray/Neutral | Generic label, category |

### Style Variants

| Style | Background | Text Color | Border | Use Case |
|-------|-----------|-----------|--------|----------|
| **Filled** | Semantic color | White | None | High emphasis, primary |
| **Outlined** | Transparent | Semantic color | Semantic color | Medium emphasis |
| **Ghost** | Transparent | Semantic color | None | Low emphasis, subtle |
| **Subtle** | Tinted (10% opacity) | Semantic color | None | Very low emphasis |

---

## States

| State | Visual | Interaction |
|-------|--------|-------------|
| **Default** | Static badge | Informational |
| **Hover** | Subtle shadow or opacity change | Pointer cursor if dismissible |
| **Active** | Highlighted or selected | Semantic emphasis |
| **Disabled** | Grayed out, 50% opacity | No interaction |
| **Loading** | Spinner or pulse animation | Awaiting action |
| **Dismissing** | Fade out animation | Slide/fade away |
| **Count Update** | Pulse or scale animation | Number increased/decreased |

---

## Props / Properties

### Badge Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Badge content (text, number, or components) |
| `variant` | `'filled' \| 'outlined' \| 'ghost' \| 'subtle'` | `'filled'` | Visual style |
| `color` | `'success' \| 'warning' \| 'error' \| 'info' \| 'primary' \| 'secondary'` | `'info'` | Color semantic |
| `size` | `'xs' \| 'sm' \| 'md' \| 'lg'` | `'sm'` | Badge size |
| `shape` | `'pill' \| 'rounded' \| 'square' \| 'circle'` | `'pill'` | Border radius style |
| `icon` | `ReactNode` | — | Leading icon component |
| `dot` | `boolean` | `false` | Show status dot indicator |
| `dismissible` | `boolean` | `false` | Show close/remove button |
| `onDismiss` | `() => void` | — | Callback when dismissed |
| `disabled` | `boolean` | `false` | Disable interactions |
| `className` | `string` | — | Custom CSS class |

### StatusBadge Props (Type 1)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `status` | `'active' \| 'pending' \| 'inactive' \| 'error'` | — | Status type (required) |
| `label` | `string` | — | Status label text |
| `showDot` | `boolean` | `true` | Show indicator dot |
| `animate` | `boolean` | `false` | Pulse animation (for active) |

### CountBadge Props (Type 2)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `count` | `number` | — | Number to display (required) |
| `max` | `number` | `99` | Max number before showing "+X" |
| `showZero` | `boolean` | `false` | Show badge when count is 0 |
| `color` | `'error' \| 'info' \| 'secondary'` | `'error'` | Badge color |

### DismissibleBadge Props (Type 5)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `onDismiss` | `() => void` | — | Callback on close (required) |
| `dismissIcon` | `ReactNode` | × | Custom close icon |
| `dismissAriaLabel` | `string` | "Remove" | Aria label for close button |

---

## Accessibility

### ARIA Attributes

```jsx
// Status badge
<span
  role="status"
  aria-label="Active"  // Current status
>
  ● Active
</span>

// Count badge (notification)
<span
  role="status"
  aria-live="polite"           // Announce changes
  aria-label="12 new messages" // Full context
>
  12
</span>

// Dismissible badge
<div
  role="group"
  aria-label="Feature tag"
>
  <span>Feature</span>
  <button
    aria-label="Remove Feature tag"
    onClick={onDismiss}
  >
    ×
  </button>
</div>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Focus on dismissible badges |
| **Enter / Space** | Dismiss badge (if dismissible) |
| **Escape** | Dismiss badge (if dismissible) |

### Screen Reader Announcement

- Status badge: `"Active, status"` or `"Pending approval"`
- Count badge: `"12 new messages, status"`
- Dismissible badge: `"Feature tag, button, remove tag"`
- Updated count: `"Badge updated, 5 new items"` (with aria-live)

---

## Code Examples

### Basic Status Badge

```jsx
import { Badge } from '@/components/Badge'

export function StatusBadges() {
  return (
    <div className="flex gap-2">
      <Badge color="success" dot>Active</Badge>
      <Badge color="warning" dot>Pending</Badge>
      <Badge color="error" dot>Inactive</Badge>
      <Badge color="info" dot>Processing</Badge>
    </div>
  )
}
```

### Count Badge

```jsx
import { Badge } from '@/components/Badge'

export function NotificationBadge() {
  const [unreadCount, setUnreadCount] = useState(12)

  return (
    <div className="relative">
      <button className="relative">
        <MailIcon />
        {unreadCount > 0 && (
          <Badge
            variant="filled"
            color="error"
            shape="circle"
            size="sm"
            className="absolute -top-2 -right-2"
          >
            {unreadCount > 99 ? '99+' : unreadCount}
          </Badge>
        )}
      </button>
    </div>
  )
}
```

### Dismissible Tag Badge

```jsx
import { Badge } from '@/components/Badge'

export function TagList() {
  const [tags, setTags] = useState(['React', 'UI', 'Components'])

  const removeTag = (tagToRemove) => {
    setTags(tags.filter(tag => tag !== tagToRemove))
  }

  return (
    <div className="flex flex-wrap gap-2">
      {tags.map(tag => (
        <Badge
          key={tag}
          variant="outlined"
          color="primary"
          dismissible
          onDismiss={() => removeTag(tag)}
        >
          {tag}
        </Badge>
      ))}
    </div>
  )
}
```

### Color Variants

```jsx
<div className="space-y-4">
  {/* Filled variants */}
  <div className="flex gap-2">
    <Badge variant="filled" color="success">Success</Badge>
    <Badge variant="filled" color="warning">Warning</Badge>
    <Badge variant="filled" color="error">Error</Badge>
    <Badge variant="filled" color="info">Info</Badge>
  </div>

  {/* Outlined variants */}
  <div className="flex gap-2">
    <Badge variant="outlined" color="success">Success</Badge>
    <Badge variant="outlined" color="warning">Warning</Badge>
    <Badge variant="outlined" color="error">Error</Badge>
    <Badge variant="outlined" color="info">Info</Badge>
  </div>

  {/* Ghost variants */}
  <div className="flex gap-2">
    <Badge variant="ghost" color="success">Success</Badge>
    <Badge variant="ghost" color="warning">Warning</Badge>
    <Badge variant="ghost" color="error">Error</Badge>
    <Badge variant="ghost" color="info">Info</Badge>
  </div>
</div>
```

### Size Variants

```jsx
<div className="flex items-center gap-2">
  <Badge size="xs">Extra Small</Badge>
  <Badge size="sm">Small</Badge>
  <Badge size="md">Medium</Badge>
  <Badge size="lg">Large</Badge>
</div>
```

### With Icon

```jsx
import { Badge } from '@/components/Badge'
import { StarIcon, CheckIcon } from '@/icons'

export function IconBadges() {
  return (
    <div className="flex gap-2">
      <Badge icon={<StarIcon />} color="warning">Featured</Badge>
      <Badge icon={<CheckIcon />} color="success">Verified</Badge>
      <Badge icon={<AlertIcon />} color="error">Alert</Badge>
    </div>
  )
}
```

### In Avatar Context

```jsx
import { Avatar, AvatarImage, AvatarFallback } from '@/components/Avatar'
import { Badge } from '@/components/Badge'

export function UserWithStatus() {
  return (
    <div className="relative">
      <Avatar size="lg">
        <AvatarImage src="user.jpg" alt="Jane Doe" />
        <AvatarFallback>JD</AvatarFallback>
      </Avatar>
      <Badge
        color="success"
        dot
        shape="circle"
        className="absolute -bottom-1 -right-1"
      >
        Online
      </Badge>
    </div>
  )
}
```

### In Product Card

```jsx
export function ProductCard() {
  return (
    <div className="relative">
      <img src="product.jpg" alt="Product" />

      <div className="absolute top-3 right-3 space-y-2">
        <Badge color="error" variant="filled">Sale</Badge>
        <Badge color="warning" variant="filled" icon={<StarIcon />}>
          New
        </Badge>
      </div>
    </div>
  )
}
```

### Live Count Update

```jsx
import { Badge } from '@/components/Badge'
import { useState, useEffect } from 'react'

export function LiveNotifications() {
  const [count, setCount] = useState(0)

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(prev => prev + 1)
    }, 5000)
    return () => clearInterval(interval)
  }, [])

  return (
    <Badge
      color="error"
      role="status"
      aria-live="polite"
      aria-label={`${count} new notifications`}
    >
      {count > 99 ? '99+' : count}
    </Badge>
  )
}
```

---

## Do's and Don'ts

### ✅ Do

- Use semantic colors to convey meaning (green = success, red = error)
- Keep badge text brief and scannable (1-3 words max)
- Use consistent sizing within the same context
- Position badges consistently (e.g., always top-right on avatars)
- Provide context via aria-label for screen readers
- Use badges to supplement, not replace, primary information
- Ensure color isn't the only way to convey status (add text or icon)
- Update count badges with aria-live for real-time changes
- Make dismissible badges keyboard accessible

### ❌ Don't

- Don't use badges for primary actions or critical information
- Don't hide important details inside a collapsed badge
- Don't use more than 2-3 badges per item (visual clutter)
- Don't use only color for status (include text or icon)
- Don't nest badges within badges
- Don't use ambiguous labels ("Info", "Status", "Item")
- Don't forget to update count badges when content changes
- Don't make dismissible badges hard to close on mobile
- Don't use overly long text that wraps

---

## Animation Specifications

### Count Update Animation
- **Duration:** 300ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Animation:**
  - Scale: 1 → 1.2 → 1
  - Opacity: 1 (constant, highlight on scale)

### Dismiss Animation
- **Duration:** 200ms
- **Easing:** ease-out
- **Animation:**
  - Slide: x 0 → -20px
  - Opacity: 1 → 0
  - Scale: 1 → 0.8

### Active State Pulse (Optional)
- **Duration:** 2 seconds (infinite)
- **Animation:**
  - Scale: 1 → 1.1 → 1
  - Opacity: 1 → 0.8 → 1

### Hover Effect (Dismissible)
- **Duration:** 150ms
- **Easing:** ease-out
- **Close button:** Fade in/scale 0.8 → 1

---

## Design Tokens Used

- **Colors:**
  - Success: `#10B981`
  - Warning: `#F59E0B`
  - Error: `#EF4444`
  - Info: `#3B82F6`
  - Primary: Brand color
  - Secondary: `#6B7280`

- **Typography:**
  - Small: `typography.caption`
  - Default: `typography.button.sm`
  - Large: `typography.body.sm`
  - Font weight: 600 (semi-bold)

- **Spacing:**
  - XS padding: 2px 6px
  - SM padding: 4px 8px
  - MD padding: 6px 12px
  - LG padding: 8px 16px
  - Gap between icon/text: 4px

- **Borders:**
  - Radius (pill): 9999px
  - Radius (rounded): 6px
  - Border width: 1px
  - Dot size: 8px diameter

---

## Related Components

- **Avatar** — User profiles (badges on top for status)
- **Button** — For primary actions (don't use badges)
- **Tag** — For interactive filtering
- **Tooltip** — For additional badge context
- **Alert** — For critical information
- **Notification** — For message counts

---

## Use Case Examples

### E-Commerce
- "Sale", "New", "Limited Stock" badges on products
- Count badges for cart items and wishlists
- Status badges for order states (Processing, Shipped, Delivered)

### Social Media
- Online/offline status badges on profiles
- Verification badges for verified accounts
- Badge counts for likes, comments, shares

### Project Management
- Task status badges (To Do, In Progress, Done)
- Priority badges (High, Medium, Low)
- Team member role badges (Manager, Contributor, Reviewer)

### Communication Apps
- Unread message count badges
- Online/away/offline status badges
- User presence indicators

### Admin Dashboards
- System status badges (Healthy, Warning, Critical)
- Permission badges (Admin, Moderator, User)
- Deployment badges (Staging, Production)

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
**Badge Types Documented:** 5 variants (Status, Count, Category/Tag, Label, Dismissible)
