# List Component Documentation

## Overview

The List component is a flexible, structured display element for presenting collections of related items in a scannable, consistent format. Lists are one of the most fundamental UI patterns — they surface content ranging from navigation menus and settings options to contact directories, file explorers, and activity feeds. The List component abstracts the semantic HTML list elements (`<ul>`, `<ol>`, `<dl>`) into a composable system of ListItem, ListItemAvatar, ListItemIcon, ListItemText, ListItemAction, and ListItemMeta subcomponents. It handles dividers, selection states, density, interactive items, nested expansion, and drag-and-drop consistently across every list context in the application.

---

## Component Variants

List supports six primary usage patterns:

### 1. **Basic List**
- **Purpose**: Vertically stacked display of text-only items
- **Use Cases**: Simple menu options, text item collections, option lists
- **Features**: Consistent spacing, optional dividers, optional ordered numbering
- **Behavior**: Static display; no interaction by default
- **HTML**: `<ul>` (unordered) or `<ol>` (ordered) with `<li>` items

### 2. **Icon List**
- **Purpose**: List items with a leading icon to reinforce category or type
- **Use Cases**: Feature lists, settings menus, navigation drawers, file type lists
- **Features**: Leading icon (16–20px), label, optional secondary text, optional trailing action
- **Behavior**: Icon is decorative by default; may carry semantic meaning for status icons

### 3. **Avatar List**
- **Purpose**: List items with a leading avatar representing a person or entity
- **Use Cases**: Contact lists, team member lists, comment feeds, conversation lists, assignee pickers
- **Features**: Avatar (32–40px), primary label, secondary descriptor (role, email, status), trailing action
- **Behavior**: Entire row clickable; optional selection; optional online status indicator

### 4. **Interactive / Selectable List**
- **Purpose**: List where items can be clicked to navigate, trigger an action, or be selected
- **Use Cases**: Settings navigation, file browser, email inbox, product list with selection
- **Features**: Hover state, active/selected indicator, keyboard navigation, focus management
- **Behavior**: Single-select or multi-select; selected items highlighted; ARIA selection attributes

### 5. **Nested / Expandable List**
- **Purpose**: Hierarchical list with expandable parent items revealing child items
- **Use Cases**: File explorer, category navigation, grouped settings, accordion-style item trees
- **Features**: Expand/collapse chevron, indented children, expand-all option, animated reveal
- **Behavior**: Click parent to toggle children; supports multiple levels of nesting

### 6. **Data List (Key-Value)**
- **Purpose**: Displays structured data as label-value pairs
- **Use Cases**: Profile details, order summary, entity metadata, specification sheets
- **HTML**: `<dl>` with `<dt>` (term) and `<dd>` (description)
- **Features**: Horizontal or vertical layout, inline or stacked pairs
- **Behavior**: Static (read-only) by default; optional copy-to-clipboard action per row

---

## Size / Density Variants

| Density | Item Height | Padding V | Avatar | Icon | Use Case |
|---------|-------------|-----------|--------|------|---------|
| **Compact** | 32–36px | 4px | 24px | 14px | Dense tables, sidebar menus, mobile |
| **Default** | 48–52px | 8px | 32px | 16px | **Default**, standard lists, settings |
| **Comfortable** | 56–64px | 12px | 40px | 20px | Avatar lists, contact directories |
| **Spacious** | 72–80px | 16px | 48px | 24px | Feature lists, card-like rows |

---

## Style Variants

### **Plain** (Default)
- No border around list container
- Optional hairline dividers between items
- Items flush with container edges
- Best For: Inline lists within cards, panels, sidebars

### **Outlined**
- 1px border around entire list container
- Corner radius: 8px
- Items may or may not have internal dividers
- Best For: Standalone list components, settings panels, standalone menus

### **Card**
- Each item has individual card styling (shadow, border-radius)
- Gap between items (not touching)
- Best For: Spaced list layouts, activity feeds, notification lists

### **Inset**
- Dividers are inset (don't reach the left edge), aligned to content column
- Best For: Avatar lists, icon lists where content starts after the leading element

### **Flush**
- Items go edge-to-edge, no padding on container
- Best For: Full-width lists inside cards or panels

---

## State Specifications

### **Default State**
- Background: Transparent or White
- Text: Gray 900 (#111827)
- Secondary text: Gray 500 (#6B7280)
- Divider: Gray 100 (#F3F4F6) or Gray 200
- Cursor: default (non-interactive) or pointer (interactive)

### **Hover State** (Interactive Items)
- Background: Gray 50 (#F9FAFB)
- Duration: 100ms ease-out
- Cursor: pointer
- Text: Unchanged

### **Active / Selected State**
- Background: Brand tint (#EFF6FF)
- Text: Brand color (#3B82F6) or Gray 900
- Left indicator: 3px brand-colored bar on left edge (optional)
- Checkbox: Checked (multi-select mode)
- Duration: 100ms ease-out

### **Focus State** (Keyboard)
- Outline: 2px solid brand, 2px inset (inside the item)
- Background: Same as hover
- Duration: 100ms ease-out

### **Expanded State** (Nested Items)
- Chevron rotates 90° (▶ → ▼)
- Children revealed with slide-down + fade animation
- Parent background: Slightly tinted to show it is expanded

### **Disabled State**
- Text: Gray 400 (#9CA3AF)
- Background: No hover change
- Cursor: not-allowed
- Pointer Events: none
- Opacity: 60%

### **Loading State**
- Skeleton rows replace content
- Pulse animation (1.5s ease-in-out)
- Matches density of loaded list (same row heights)

### **Drag State** (Drag-and-Drop)
- Dragged item: Elevated shadow (Elevation 8), slightly transparent (80% opacity)
- Drop target: Blue top-border indicator or background highlight
- Other items: Shift to create gap at drop position

---

## Anatomy

```
ListContainer
├── ListSubheader (optional, sticky)
│
├── ListItem
│   ├── ListItemLeading (optional)
│   │   ├── Checkbox (multi-select)
│   │   ├── Avatar or Icon
│   │   └── DragHandle
│   ├── ListItemContent
│   │   ├── ListItemText (primary)
│   │   ├── ListItemSecondaryText (secondary)
│   │   └── ListItemTertiaryText (tertiary, compact)
│   ├── ListItemMeta (optional)
│   │   ├── Badge / Chip
│   │   ├── Status dot
│   │   └── Timestamp
│   └── ListItemTrailing (optional)
│       ├── Action button / icon button
│       ├── Chevron (expandable)
│       └── Trailing text
│
├── Divider (inset or full-width)
│
└── ListItem (×N)
```

---

## Props API

### **List (Container)**

```typescript
interface ListProps {
  // Semantic
  as?: 'ul' | 'ol' | 'dl' | 'div';           // HTML element (default: 'ul')
  ordered?: boolean;                           // Renders as <ol> with numbers

  // Appearance
  variant?: 'plain' | 'outlined' | 'card' | 'inset' | 'flush';
  density?: 'compact' | 'default' | 'comfortable' | 'spacious';
  dividers?: boolean | 'inset';               // Show dividers between items

  // Selection
  selectable?: boolean;                        // Enable item selection
  selectionMode?: 'single' | 'multiple';
  value?: string | string[];                   // Controlled selected item(s)
  defaultValue?: string | string[];
  onChange?: (value: string | string[]) => void;

  // Subheaders
  subheader?: string | React.ReactNode;        // Sticky list subheader

  // Loading
  loading?: boolean;                           // Show skeleton rows
  loadingRows?: number;                        // Number of skeleton rows (default: 3)

  // Drag and Drop
  draggable?: boolean;                         // Enable drag-and-drop reordering
  onReorder?: (newOrder: string[]) => void;    // Called after drag reorder

  // Accessibility
  'aria-label'?: string;
  'aria-labelledby'?: string;
  role?: string;                               // Override inferred role

  // Styling
  className?: string;
  style?: React.CSSProperties;
  children: React.ReactNode;
}
```

### **ListItem**

```typescript
interface ListItemProps {
  // Identity
  value?: string;                              // Unique ID for selection

  // Content
  primary: string | React.ReactNode;          // Primary label
  secondary?: string | React.ReactNode;       // Secondary label
  tertiary?: string | React.ReactNode;        // Tertiary line (compact)

  // Leading Elements
  icon?: React.ReactNode;                      // Leading icon
  avatar?: React.ReactNode;                    // Leading avatar
  checkbox?: boolean;                          // Leading checkbox (multi-select)
  dragHandle?: boolean;                        // Show drag handle

  // Trailing Elements
  trailing?: React.ReactNode;                  // Trailing content (custom)
  trailingText?: string;                       // Trailing text (timestamp, meta)
  action?: React.ReactNode;                    // Action button/icon button
  badge?: string | number;                     // Trailing badge
  chevron?: boolean;                           // Trailing chevron (navigation)

  // Nested
  expandable?: boolean;                        // Has child items
  defaultExpanded?: boolean;
  expanded?: boolean;                          // Controlled expand state
  onExpandChange?: (expanded: boolean) => void;
  children?: React.ReactNode;                  // Nested list items

  // States
  selected?: boolean;                          // Override selection state
  disabled?: boolean;
  active?: boolean;                            // Active/current item (nav)
  loading?: boolean;

  // Interaction
  onClick?: (event: React.MouseEvent) => void;
  href?: string;                               // Makes item a link
  as?: 'li' | 'div' | 'a' | 'button';

  // Accessibility
  'aria-label'?: string;
  'aria-current'?: 'page' | 'true';
  'aria-selected'?: boolean;
  'aria-expanded'?: boolean;

  // Styling
  className?: string;
  style?: React.CSSProperties;
  indent?: number;                             // Indentation level for nested items
}
```

### **ListSubheader**

```typescript
interface ListSubheaderProps {
  children: React.ReactNode;
  sticky?: boolean;                            // Stick to top on scroll (default: true)
  divider?: boolean;                           // Divider below subheader
  inset?: boolean;                             // Align with inset content
  className?: string;
}
```

---

## Code Examples

### **1. Basic Unordered List**
```typescript
import { List, ListItem } from '@components/list';

export function FeatureList() {
  return (
    <List aria-label="Product features">
      <ListItem primary="Unlimited projects" />
      <ListItem primary="Real-time collaboration" />
      <ListItem primary="Advanced analytics dashboard" />
      <ListItem primary="Priority customer support" />
      <ListItem primary="99.9% uptime SLA" />
    </List>
  );
}
```

### **2. Icon List with Secondary Text**
```typescript
import { List, ListItem } from '@components/list';
import { Bell, Mail, MessageSquare, Globe } from 'lucide-react';

export function NotificationSettings() {
  return (
    <List variant="outlined" dividers aria-label="Notification channels">
      <ListItem
        icon={<Bell size={18} />}
        primary="Push notifications"
        secondary="Receive alerts directly on your device"
        action={<Toggle />}
      />
      <ListItem
        icon={<Mail size={18} />}
        primary="Email digest"
        secondary="Daily summary of activity sent to your inbox"
        action={<Toggle />}
      />
      <ListItem
        icon={<MessageSquare size={18} />}
        primary="In-app notifications"
        secondary="See updates in the notification centre"
        action={<Toggle />}
      />
      <ListItem
        icon={<Globe size={18} />}
        primary="Browser notifications"
        secondary="Requires browser permission"
        action={<Toggle />}
      />
    </List>
  );
}
```

### **3. Avatar Contact List**
```typescript
import { List, ListItem } from '@components/list';
import { Avatar } from '@components/avatar';
import { Button } from '@components/button';
import { MessageCircle } from 'lucide-react';

const contacts = [
  { id: '1', name: 'Sarah Johnson', role: 'Product Designer', status: 'online', src: '/avatars/sarah.jpg' },
  { id: '2', name: 'Alex Chen', role: 'Frontend Engineer', status: 'away', src: '/avatars/alex.jpg' },
  { id: '3', name: 'Maria Lopez', role: 'Product Manager', status: 'offline', src: '/avatars/maria.jpg' },
];

export function ContactList() {
  return (
    <List
      density="comfortable"
      dividers="inset"
      aria-label="Team contacts"
    >
      {contacts.map((contact) => (
        <ListItem
          key={contact.id}
          value={contact.id}
          avatar={
            <Avatar
              src={contact.src}
              alt={contact.name}
              size="md"
              status={contact.status as any}
            />
          }
          primary={contact.name}
          secondary={contact.role}
          action={
            <Button
              size="sm"
              variant="ghost"
              icon={<MessageCircle size={16} />}
              aria-label={`Message ${contact.name}`}
            />
          }
        />
      ))}
    </List>
  );
}
```

### **4. Single-Select Interactive List**
```typescript
import { List, ListItem } from '@components/list';
import { useState } from 'react';
import { CreditCard, Building, Smartphone } from 'lucide-react';

const paymentMethods = [
  { id: 'card', label: 'Credit / Debit Card', sub: 'Visa, Mastercard, Amex', icon: <CreditCard size={18} /> },
  { id: 'bank', label: 'Bank Transfer', sub: 'ACH / SEPA direct debit', icon: <Building size={18} /> },
  { id: 'mobile', label: 'Mobile Pay', sub: 'Apple Pay, Google Pay', icon: <Smartphone size={18} /> },
];

export function PaymentMethodSelector() {
  const [selected, setSelected] = useState('card');

  return (
    <List
      selectable
      selectionMode="single"
      value={selected}
      onChange={(val) => setSelected(val as string)}
      variant="outlined"
      dividers
      aria-label="Select payment method"
    >
      {paymentMethods.map((method) => (
        <ListItem
          key={method.id}
          value={method.id}
          icon={method.icon}
          primary={method.label}
          secondary={method.sub}
          chevron={false}
        />
      ))}
    </List>
  );
}
```

### **5. Multi-Select List with Checkboxes**
```typescript
import { List, ListItem } from '@components/list';
import { useState } from 'react';

const permissions = [
  { id: 'read', label: 'Read', description: 'View content and data' },
  { id: 'write', label: 'Write', description: 'Create and edit content' },
  { id: 'delete', label: 'Delete', description: 'Remove content permanently' },
  { id: 'admin', label: 'Admin', description: 'Manage team and settings' },
];

export function PermissionList() {
  const [selected, setSelected] = useState<string[]>(['read']);

  return (
    <List
      selectable
      selectionMode="multiple"
      value={selected}
      onChange={(vals) => setSelected(vals as string[])}
      variant="outlined"
      dividers
      aria-label="Assign permissions"
    >
      {permissions.map((perm) => (
        <ListItem
          key={perm.id}
          value={perm.id}
          checkbox
          primary={perm.label}
          secondary={perm.description}
        />
      ))}
    </List>
  );
}
```

### **6. Expandable Nested List**
```typescript
import { List, ListItem } from '@components/list';
import { Folder, File } from 'lucide-react';

export function FileExplorer() {
  return (
    <List variant="plain" aria-label="File explorer">
      <ListItem
        icon={<Folder size={16} />}
        primary="src"
        expandable
        defaultExpanded
      >
        <List>
          <ListItem
            icon={<Folder size={16} />}
            primary="components"
            expandable
            indent={1}
          >
            <List>
              <ListItem icon={<File size={14} />} primary="Button.tsx" indent={2} />
              <ListItem icon={<File size={14} />} primary="Input.tsx" indent={2} />
              <ListItem icon={<File size={14} />} primary="Modal.tsx" indent={2} />
            </List>
          </ListItem>
          <ListItem icon={<File size={16} />} primary="App.tsx" indent={1} />
          <ListItem icon={<File size={16} />} primary="index.ts" indent={1} />
        </List>
      </ListItem>
      <ListItem
        icon={<Folder size={16} />}
        primary="public"
        expandable
        indent={0}
      >
        <List>
          <ListItem icon={<File size={16} />} primary="favicon.ico" indent={1} />
          <ListItem icon={<File size={16} />} primary="index.html" indent={1} />
        </List>
      </ListItem>
    </List>
  );
}
```

### **7. Data List (Key-Value Pairs)**
```typescript
import { List, ListItem } from '@components/list';

const orderDetails = [
  { term: 'Order number', value: '#ORD-2026-0042' },
  { term: 'Placed on', value: 'March 18, 2026' },
  { term: 'Status', value: 'Shipped' },
  { term: 'Delivery address', value: '123 Main St, San Francisco, CA 94105' },
  { term: 'Payment', value: 'Visa ending 4242' },
  { term: 'Total', value: '$129.00 USD' },
];

export function OrderSummary() {
  return (
    <List
      as="dl"
      variant="outlined"
      dividers
      density="compact"
      aria-label="Order details"
    >
      {orderDetails.map(({ term, value }) => (
        <ListItem
          key={term}
          primary={term}
          trailing={<span className="text-gray-900 font-medium">{value}</span>}
        />
      ))}
    </List>
  );
}
```

### **8. List with Subheaders**
```typescript
import { List, ListItem, ListSubheader } from '@components/list';
import { Avatar } from '@components/avatar';

const grouped = {
  Online: [
    { id: '1', name: 'Sarah Johnson', role: 'Designer' },
    { id: '2', name: 'Alex Chen', role: 'Engineer' },
  ],
  Away: [
    { id: '3', name: 'Maria Lopez', role: 'PM' },
  ],
  Offline: [
    { id: '4', name: 'Tom Wright', role: 'Marketing' },
    { id: '5', name: 'Priya Patel', role: 'Sales' },
  ],
};

export function GroupedTeamList() {
  return (
    <List dividers="inset" density="comfortable" aria-label="Team members by status">
      {Object.entries(grouped).map(([status, members]) => (
        <>
          <ListSubheader key={status} sticky>
            {status} · {members.length}
          </ListSubheader>
          {members.map((member) => (
            <ListItem
              key={member.id}
              avatar={<Avatar initials={member.name[0]} size="sm" />}
              primary={member.name}
              secondary={member.role}
            />
          ))}
        </>
      ))}
    </List>
  );
}
```

### **9. Loading Skeleton List**
```typescript
import { List } from '@components/list';

export function LoadingList() {
  return (
    <List
      loading
      loadingRows={5}
      density="comfortable"
      dividers
      aria-label="Loading contacts"
      aria-busy="true"
    />
  );
}
```

### **10. Draggable Reorderable List**
```typescript
import { List, ListItem } from '@components/list';
import { useState } from 'react';

export function ReorderableList() {
  const [items, setItems] = useState([
    { id: '1', label: 'First priority task' },
    { id: '2', label: 'Second priority task' },
    { id: '3', label: 'Third priority task' },
    { id: '4', label: 'Fourth priority task' },
  ]);

  const handleReorder = (newOrder: string[]) => {
    setItems((prev) =>
      newOrder.map((id) => prev.find((item) => item.id === id)!)
    );
  };

  return (
    <List
      draggable
      onReorder={handleReorder}
      dividers
      aria-label="Task priority list"
    >
      {items.map((item) => (
        <ListItem
          key={item.id}
          value={item.id}
          dragHandle
          primary={item.label}
        />
      ))}
    </List>
  );
}
```

---

## Accessibility Specifications

### **Semantic HTML**
- **`<ul>` + `<li>`**: Default for unordered lists; implicit `role="list"` / `role="listitem"`
- **`<ol>` + `<li>`**: For ordered/numbered lists; use when sequence matters
- **`<dl>` + `<dt>` + `<dd>`**: For key-value data lists; `<dt>` is the term, `<dd>` is the value
- **`<nav>` + `<ul>`**: For navigation lists; add `aria-label` to `<nav>`
- **`role="listbox"` + `role="option"`**: For selectable lists (single or multi-select)
- **`role="tree"` + `role="treeitem"`**: For expandable nested/hierarchical lists
- **`role="menu"` + `role="menuitem"`**: For action/command menus (not navigation)

### **When to Use Which Role**
| Use Case | HTML / ARIA |
|----------|-------------|
| Static text list | `<ul>` + `<li>` |
| Ordered/numbered | `<ol>` + `<li>` |
| Key-value data | `<dl>` + `<dt>` + `<dd>` |
| Navigation menu | `<nav><ul>` + `<li>` |
| Selectable options (single/multi) | `role="listbox"` + `role="option"` |
| File explorer / tree | `role="tree"` + `role="treeitem"` |
| Action commands | `role="menu"` + `role="menuitem"` |

### **Selection Accessibility**
```tsx
// Single-select list
<ul role="listbox" aria-label="Payment method" aria-activedescendant="option-card">
  <li role="option" id="option-card" aria-selected="true">Credit Card</li>
  <li role="option" id="option-bank" aria-selected="false">Bank Transfer</li>
</ul>

// Multi-select list
<ul role="listbox" aria-multiselectable="true" aria-label="Permissions">
  <li role="option" aria-selected="true">Read</li>
  <li role="option" aria-selected="false">Write</li>
</ul>
```

### **Expandable / Tree Accessibility**
```tsx
<ul role="tree" aria-label="File explorer">
  <li role="treeitem" aria-expanded="true" aria-level="1">
    <button aria-expanded="true">src</button>
    <ul role="group">
      <li role="treeitem" aria-level="2">App.tsx</li>
    </ul>
  </li>
</ul>
```

### **Keyboard Navigation**

**Standard List (non-interactive)**
- **Tab**: Moves focus through interactive elements (actions, links) within items
- No arrow key navigation required

**Selectable List (`role="listbox"`)**
- **Tab**: Move focus into listbox
- **Up/Down Arrow**: Move between options
- **Space**: Toggle selection (multi-select) or select (single-select)
- **Home**: Jump to first option
- **End**: Jump to last option
- **Ctrl+A**: Select all (multi-select)

**Tree List (`role="tree"`)**
- **Up/Down Arrow**: Move between visible treeitems
- **Right Arrow**: Expand collapsed item or move to first child
- **Left Arrow**: Collapse expanded item or move to parent
- **Home/End**: Jump to first/last visible treeitem
- **Enter**: Activate (open/navigate to) focused treeitem

**Drag-and-Drop List**
- Keyboard alternative: Up/Down arrows to reorder when drag handle focused
- `aria-grabbed` / `aria-dropeffect` (deprecated — use custom announcements via `aria-live` region)
- Announce position changes: "Task moved from position 2 to position 4"

### **Loading State**
- `aria-busy="true"` on the list container during loading
- `aria-live="polite"` on a status element: "Loading contacts..."
- After load: "5 contacts loaded" or remove the announcement

### **Subheaders**
- `role="presentation"` on `<li>` containing the subheader (it's not an option/item)
- Or use `<li aria-hidden="true">` for visual-only separators
- Better: Use CSS `::before` pseudo-element for purely visual section dividers

### **Screen Reader Announcements**
- Non-interactive list: "Feature list, 5 items. Unlimited projects. Real-time collaboration." (list and count announced)
- Selectable: "Payment method, listbox. Credit Card, 1 of 3, selected. Bank Transfer, 2 of 3, not selected."
- Expanded tree: "src, expanded, level 1. components, collapsed, level 2."
- After expand: "src, expanded. 3 items."
- After drag reorder: "Task moved to position 3 of 4."

### **Touch Accessibility**
- Minimum touch target per item: 44px height
- Action buttons within items: 44×44px
- Drag handles: 44×44px for comfortable touch reordering
- Swipe-to-reveal actions (mobile): Accessible via visible button fallback too

---

## Animation Specifications

### **Hover Background**
- **Target**: background-color
- **Duration**: 100ms ease-out

### **Selection Fill**
- **Target**: background-color, border-color
- **Duration**: 150ms ease-out

### **Expand / Collapse (Nested)**
- **Target**: max-height + opacity
- **Duration**: 200ms ease-out (expand), 150ms ease-in (collapse)
- **Overflow**: hidden on children container
  ```css
  .list-children {
    overflow: hidden;
    transition: max-height 200ms ease-out, opacity 200ms ease-out;
  }
  .list-children.collapsed { max-height: 0; opacity: 0; }
  .list-children.expanded  { max-height: 2000px; opacity: 1; }
  ```

### **Chevron Rotation**
- **Target**: transform rotate
- **Duration**: 200ms ease-out
  ```css
  .chevron { transition: transform 200ms ease-out; }
  .expanded .chevron { transform: rotate(90deg); }
  ```

### **Skeleton Pulse**
- **Target**: background-color
- **Duration**: 1.5s ease-in-out infinite
  ```css
  @keyframes skeletonPulse {
    0%, 100% { background: #E5E7EB; }
    50%       { background: #F3F4F6; }
  }
  ```

### **Drag Lift**
- **Target**: box-shadow + opacity
- **Duration**: 150ms ease-out
  ```css
  .dragging {
    box-shadow: 0 8px 24px rgba(0,0,0,0.15);
    opacity: 0.85;
  }
  ```

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .list-children, .chevron, .list-item { transition: none; animation: none; }
}
```

---

## Design Tokens

### **Colors**
| State | Background | Text | Secondary Text | Divider |
|-------|------------|------|----------------|---------|
| Default | transparent | #111827 | #6B7280 | #F3F4F6 |
| Hover | #F9FAFB | #111827 | #6B7280 | — |
| Selected | #EFF6FF | #1D4ED8 | #3B82F6 | — |
| Disabled | transparent | #9CA3AF | #D1D5DB | — |
| Subheader | #F9FAFB | #6B7280 | — | — |

### **Spacing (Default Density)**
| Property | Value |
|----------|-------|
| Item padding vertical | 8px |
| Item padding horizontal | 16px |
| Icon → text gap | 12px |
| Avatar → text gap | 12px |
| Primary → secondary text gap | 2px |
| Action button inset | 8px from right edge |
| Selection indicator width | 3px |
| Indent per level | 20px |

### **Typography**
| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Primary label | 14px | 500 | #111827 |
| Secondary label | 13px | 400 | #6B7280 |
| Tertiary label | 12px | 400 | #9CA3AF |
| Subheader | 11px | 600 (uppercase) | #9CA3AF |
| Trailing text | 12px | 400 | #9CA3AF |

---

## Best Practices

### **Choosing the Right Variant**
- Plain: For lists embedded in cards, panels, or other containers with their own framing
- Outlined: For standalone list components that need a visual boundary
- Card: When items benefit from individual separation (activity feed, notification list)
- Inset dividers: Always for avatar or icon lists — aligns divider with content, not container

### **Content Hierarchy**
- Primary text: The item's name or title — most visually prominent
- Secondary text: Supporting detail — role, description, date
- Meta / trailing: Timestamps, badges, status — least prominent
- Don't put critical information only in secondary or trailing text

### **Interaction Design**
- Make the entire row clickable — not just an icon or small button — for primary navigation items
- Reserve trailing action buttons for secondary actions (e.g., message button on a contact row)
- Clearly distinguish active/selected state from hover (different background + optional indicator bar)
- For destructive actions (delete), use a confirmation step — don't trigger from a list row swipe alone

### **Performance**
- Virtualize long lists (50+ items) using react-window or react-virtual
- Avoid rendering all items at once in infinite scroll — paginate or virtualize
- Use `loading` prop skeleton instead of spinners — preserves layout and reduces CLS
- Memoize list items with `React.memo` if items are complex and list is large

### **Mobile**
- Use `density="comfortable"` or `density="spacious"` on mobile for touch targets
- Swipe gestures (reveal actions) must have a visible button fallback
- Subheaders should be sticky for long grouped lists so users know their context
- Consider bottom sheet for action menus instead of inline trailing actions on mobile

### **Empty State**
- Never render a list container with zero items and no message
- Show the EmptyState component inside the list container when empty
- Preserve list container dimensions during loading to prevent layout shift

---

## Related Components

- **Menu**: Popup list for actions/commands; semantically a `role="menu"` — similar visuals, different purpose
- **Select**: Dropdown option list; uses listbox role internally
- **Table**: Tabular data with columns; use for 3+ attributes per item
- **Accordion**: Expandable content sections; use for richer content than ListItem supports
- **Combobox**: Searchable option list; shares avatar/icon list item styling
- **Navigation**: Header and sidebar nav; uses List internally
- **Empty State**: Shown when a list has no items to display

---

## Version History

- **v1.0**: Basic `<ul>/<ol>` wrapper, plain/outlined variants, dividers, density
- **v1.1**: Icon and avatar list items, trailing actions, single-select mode
- **v1.2**: Multi-select with checkboxes, ListSubheader, inset dividers
- **v2.0**: Expandable nested list (tree), data list (`<dl>`), drag-and-drop reorder
- **v2.1**: Loading skeleton, card variant, full ARIA audit (listbox/tree/menu roles)
- **v2.2**: Virtualization hints, reduced motion, mobile density improvements

---

## Testing Checklist

- [ ] All density variants render correct item heights
- [ ] Plain, outlined, card, inset, and flush variants render correctly
- [ ] Full-width and inset dividers render in correct positions
- [ ] Icon and avatar render at correct size for each density
- [ ] Hover state appears on interactive items
- [ ] Selected state renders correctly (background + optional indicator)
- [ ] Multi-select: checkbox appears and toggles independently
- [ ] Single-select: selecting one item deselects the previous
- [ ] Expandable: chevron rotates and children reveal/hide with animation
- [ ] Nested items indent correctly per level
- [ ] Subheader is sticky on scroll within a bounded container
- [ ] Loading skeleton matches list density and shows pulse animation
- [ ] Disabled items: no hover, cursor not-allowed, correct opacity
- [ ] `<ul>` used by default; `<ol>` when ordered; `<dl>` for data list
- [ ] `role="listbox"` + `role="option"` applied for selectable lists
- [ ] `role="tree"` + `role="treeitem"` applied for expandable nested lists
- [ ] `aria-selected` reflects selection state
- [ ] `aria-expanded` reflects expand state on tree items
- [ ] `aria-busy="true"` on container during loading
- [ ] Arrow key navigation works in listbox mode
- [ ] Arrow + expand/collapse key nav works in tree mode
- [ ] Tab moves through interactive elements (action buttons, links)
- [ ] Drag handle: keyboard reorder works via arrow keys
- [ ] Screen reader announces list count and item positions
- [ ] Touch targets ≥ 44px height per item
- [ ] All animations disabled under `prefers-reduced-motion`
- [ ] Empty state visible when list has no items
