# Bottom Navigation Component Documentation

## Description

The Bottom Navigation component is a navigation bar positioned at the bottom of the screen, typically used in mobile applications to provide quick access to primary destinations or sections. It displays a row of navigation items (usually 3-5) with optional icons and labels. Bottom navigation is ideal for mobile-first designs where thumb accessibility is important, and works well as a persistent navigation pattern that remains visible throughout the user's journey.

**When to use:**
- Mobile apps with 3-5 primary destinations
- Quick access to main sections (Home, Search, Profile, etc.)
- Apps where users frequently switch between sections
- Bottom-positioned navigation for better thumb reach
- Fixed/persistent navigation that should always be visible
- Touch-friendly interfaces where bottom placement is ergonomic
- Apps with clear primary navigation hierarchy

**When NOT to use:**
- Desktop applications (use top navigation instead)
- More than 5 primary sections (use drawer or tabs)
- Temporary or contextual navigation
- Apps where bottom space is needed for content
- Navigation requiring nested menus or dropdowns
- Apps with very long or variable-length labels
- Accessibility-critical apps where bottom placement reduces reach

---

## Component Structure

```
BottomNavigation
├── BottomNavigationBar (Container)
│   ├── BottomNavigationItem
│   │   ├── BottomNavigationIcon (Leading icon)
│   │   ├── BottomNavigationLabel (Text label)
│   │   └── BottomNavigationBadge (Optional notification badge)
│   ├── BottomNavigationItem
│   └── BottomNavigationItem
└── BottomNavigationBar (Full width, fixed bottom)
```

---

## Navigation Types & Variants

### Type 1: Icon + Label Navigation
**Usage:** Standard navigation with both icons and text labels

```
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ 🏠 Home │ 🔍 Explore │ ❤️ Saved │ 📬 Messages │ 👤 Profile │
└─────────┴─────────┴─────────┴─────────┴─────────┘
```

- **Layout:** Icon above, label below
- **Icon size:** 24px default
- **Label size:** 12px (caption)
- **Active state:** Icon tinted with color
- **Padding:** ~12px vertical per item

### Type 2: Icon-Only Navigation
**Usage:** Compact navigation when space is limited

```
┌────┬────┬────┬────┬────┐
│ 🏠  │ 🔍  │ ❤️  │ 📬  │ 👤  │
└────┴────┴────┴────┴────┘
```

- **Icon size:** 28px
- **No text labels** (rely on tooltips or context)
- **More compact** than icon + label
- **Requires clear, universally recognizable icons**
- **Use case:** Experienced users or space constraints

### Type 3: Scrollable Navigation
**Usage:** When more than 5 items need to be included

```
┌──────────────────────────────────────────────────┐
│ 🏠 Home │ 🔍 Explore │ ❤️ Saved │ 📬 Messages │ 👤 Profile │ 💬 Community → │
└──────────────────────────────────────────────────┘
         (horizontally scrollable)
```

- **Horizontal scroll:** Items scroll left/right
- **Leading indicator:** Shows more items available
- **Active indicator:** Shows current position
- **Usually 6+ items**
- **Smooth scrolling behavior**

### Type 4: Floating Action Button Integration
**Usage:** Bottom nav combined with a floating action button

```
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ 🏠 Home │ 🔍 Explore │ ✚ Add │ 📬 Messages │ 👤 Profile │
└─────────┴─────────┴─────────┴─────────┴─────────┘
                    (centered, elevated)
```

- **FAB positioned:** Center or right side
- **FAB size:** 56px diameter (elevated above nav)
- **Primary action:** Create, compose, add, etc.
- **Navigation items:** Usually 4 instead of 5
- **Visual hierarchy:** FAB is most prominent

### Type 5: Shifted Navigation
**Usage:** Shows/hides labels on selection

```
Active: 🏠           Inactive: 🔍 Explore
(label hidden)        (icon + label)
```

- **Active item:** Icon only, label hidden
- **Inactive items:** Icon + label visible
- **More compact** when active
- **Reduces visual clutter**
- **Mobile optimization**

---

## Size Variants

| Size | Height | Use Case | Icon | Label |
|------|--------|----------|------|-------|
| **Compact** | 48px | Dense mobile UI | 20px | 10px |
| **Default** | 56px | Standard mobile | 24px | 12px |
| **Large** | 64px | Tablet/large touch | 28px | 14px |
| **Extra Large** | 72px | Accessibility focused | 32px | 16px |

---

## Item Count Variants

| Count | Configuration | Use Case |
|-------|---------------|----------|
| **3 items** | Minimal set | Basic apps, utility apps |
| **4 items** | Most common | Standard apps (with FAB) |
| **5 items** | Full width | Comprehensive apps |
| **6+ items** | Scrollable | Complex apps (with scroll) |

---

## Visual States

| State | Visual | Interaction |
|-------|--------|-------------|
| **Default** | Item not selected | Inactive color (gray) |
| **Active/Selected** | Item selected | Brand color, bold or heavier |
| **Hover** | Subtle highlight (desktop/tablet) | Background tint |
| **Pressed** | Visual feedback on tap | Darker tint or animation |
| **Disabled** | Grayed out, 50% opacity | No interaction, cursor not-allowed |
| **With Badge** | Badge indicator on icon | Shows count or notification |
| **Focused** | Keyboard focus indicator | Ring or highlight |
| **Loading** | Spinner or skeleton | Awaiting navigation |

---

## Props / Properties

### BottomNavigation Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `BottomNavigationItem[]` | — | Navigation items (required) |
| `value` | `string` | — | Currently active item value |
| `onChange` | `(value: string) => void` | — | Callback when item selected |
| `size` | `'compact' \| 'default' \| 'large' \| 'xl'` | `'default'` | Navigation size |
| `variant` | `'icon-label' \| 'icon-only' \| 'shifted' \| 'scrollable'` | `'icon-label'` | Display variant |
| `showLabels` | `boolean` | `true` | Show item labels |
| `sticky` | `boolean` | `true` | Fixed positioning at bottom |
| `elevation` | `boolean` | `true` | Show shadow elevation |
| `fullWidth` | `boolean` | `true` | Stretch to full width |
| `maxItems` | `number` | `5` | Max items before scrolling |
| `className` | `string` | — | Custom CSS class |

### BottomNavigationItem Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `value` | `string` | — | Unique identifier (required) |
| `label` | `string` | — | Item text label |
| `icon` | `ReactNode` | — | Item icon (required) |
| `href` | `string` | — | Link destination (optional) |
| `onClick` | `() => void` | — | Click handler |
| `badge` | `number \| string \| boolean` | — | Badge indicator |
| `badgeColor` | `'error' \| 'warning' \| 'success' \| 'info'` | `'error'` | Badge color |
| `disabled` | `boolean` | `false` | Disable interaction |
| `showBadge` | `boolean` | `true` | Show badge if present |
| `className` | `string` | — | Custom CSS class |

---

## Accessibility

### ARIA Attributes

```jsx
// Bottom Navigation Container
<nav
  role="navigation"
  aria-label="Main navigation"
>
  {/* items */}
</nav>

// Individual Items
<button
  role="tab"
  aria-selected={isSelected}         // "true" for active
  aria-controls="content-panel"      // References associated content
  id="nav-item-home"                 // For aria-labelledby
>
  <span aria-hidden="true">🏠</span> {/* Icon hidden from SR */}
  <span>Home</span>
</button>

// With Badge
<button aria-label="Messages, 5 unread">
  <span aria-hidden="true">📬</span>
  <span aria-label="5">5</span>       {/* Badge count */}
</button>

// With Content Panel
<div
  id="content-panel"
  role="tabpanel"
  aria-labelledby="nav-item-home"    {/* References tab */}
>
  {/* Content for selected tab */}
</div>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Focus next navigation item |
| **Shift + Tab** | Focus previous navigation item |
| **Enter / Space** | Activate focused item |
| **Home** | Focus first item |
| **End** | Focus last item |
| **Arrow Left** | Previous item (optional) |
| **Arrow Right** | Next item (optional) |

### Screen Reader Announcement

- Navigation container: `"Navigation, main"`
- Active item: `"Home, tab, selected, 1 of 5"`
- Inactive item: `"Explore, tab, not selected, 2 of 5"`
- With badge: `"Messages, tab, 5 unread, not selected"`
- Content panel: `"Home panel"` (when switching)

---

## Code Examples

### Basic Icon + Label Navigation

```jsx
import { BottomNavigation, BottomNavigationItem } from '@/components/BottomNavigation'
import { HomeIcon, SearchIcon, HeartIcon, MessageIcon, UserIcon } from '@/icons'
import { useState } from 'react'

export function MainNavigation() {
  const [activeTab, setActiveTab] = useState('home')

  return (
    <BottomNavigation value={activeTab} onChange={setActiveTab}>
      <BottomNavigationItem
        value="home"
        icon={<HomeIcon />}
        label="Home"
      />
      <BottomNavigationItem
        value="explore"
        icon={<SearchIcon />}
        label="Explore"
      />
      <BottomNavigationItem
        value="saved"
        icon={<HeartIcon />}
        label="Saved"
      />
      <BottomNavigationItem
        value="messages"
        icon={<MessageIcon />}
        label="Messages"
        badge={5}
      />
      <BottomNavigationItem
        value="profile"
        icon={<UserIcon />}
        label="Profile"
      />
    </BottomNavigation>
  )
}
```

### Icon-Only Navigation

```jsx
<BottomNavigation
  variant="icon-only"
  size="default"
  value={activeTab}
  onChange={setActiveTab}
>
  <BottomNavigationItem value="home" icon={<HomeIcon />} label="Home" />
  <BottomNavigationItem value="search" icon={<SearchIcon />} label="Search" />
  <BottomNavigationItem value="add" icon={<PlusIcon />} label="Add" />
  <BottomNavigationItem value="messages" icon={<MessageIcon />} label="Messages" />
  <BottomNavigationItem value="profile" icon={<UserIcon />} label="Profile" />
</BottomNavigation>
```

### Shifted Navigation (Labels Hide When Active)

```jsx
<BottomNavigation
  variant="shifted"
  value={activeTab}
  onChange={setActiveTab}
>
  <BottomNavigationItem
    value="home"
    icon={<HomeIcon />}
    label="Home"
  />
  <BottomNavigationItem
    value="explore"
    icon={<SearchIcon />}
    label="Explore"
  />
  <BottomNavigationItem
    value="saved"
    icon={<HeartIcon />}
    label="Saved"
  />
  <BottomNavigationItem
    value="messages"
    icon={<MessageIcon />}
    label="Messages"
  />
  <BottomNavigationItem
    value="profile"
    icon={<UserIcon />}
    label="Profile"
  />
</BottomNavigation>
```

### With Badges/Notifications

```jsx
<BottomNavigation value={activeTab} onChange={setActiveTab}>
  <BottomNavigationItem
    value="home"
    icon={<HomeIcon />}
    label="Home"
  />
  <BottomNavigationItem
    value="search"
    icon={<SearchIcon />}
    label="Explore"
  />
  <BottomNavigationItem
    value="messages"
    icon={<MessageIcon />}
    label="Messages"
    badge={3}
    badgeColor="error"
  />
  <BottomNavigationItem
    value="notifications"
    icon={<BellIcon />}
    label="Alerts"
    badge={true}  // Just shows dot
  />
  <BottomNavigationItem
    value="profile"
    icon={<UserIcon />}
    label="Profile"
  />
</BottomNavigation>
```

### Scrollable Navigation (6+ Items)

```jsx
<BottomNavigation
  variant="scrollable"
  value={activeTab}
  onChange={setActiveTab}
  maxItems={5}
>
  <BottomNavigationItem value="home" icon={<HomeIcon />} label="Home" />
  <BottomNavigationItem value="explore" icon={<SearchIcon />} label="Explore" />
  <BottomNavigationItem value="saved" icon={<HeartIcon />} label="Saved" />
  <BottomNavigationItem value="messages" icon={<MessageIcon />} label="Messages" />
  <BottomNavigationItem value="community" icon={<PeopleIcon />} label="Community" />
  <BottomNavigationItem value="profile" icon={<UserIcon />} label="Profile" />
</BottomNavigation>
```

### With Navigation Links (Router Integration)

```jsx
import { useLocation } from 'react-router-dom'

export function RouterNavigation() {
  const location = useLocation()

  return (
    <BottomNavigation value={location.pathname} onChange={(path) => navigate(path)}>
      <BottomNavigationItem value="/" icon={<HomeIcon />} label="Home" href="/" />
      <BottomNavigationItem value="/explore" icon={<SearchIcon />} label="Explore" href="/explore" />
      <BottomNavigationItem value="/saved" icon={<HeartIcon />} label="Saved" href="/saved" />
      <BottomNavigationItem value="/messages" icon={<MessageIcon />} label="Messages" href="/messages" />
      <BottomNavigationItem value="/profile" icon={<UserIcon />} label="Profile" href="/profile" />
    </BottomNavigation>
  )
}
```

### With Floating Action Button

```jsx
export function NavigationWithFAB() {
  const [activeTab, setActiveTab] = useState('home')

  return (
    <div className="pb-20">
      {/* Page content */}

      <BottomNavigation value={activeTab} onChange={setActiveTab}>
        <BottomNavigationItem value="home" icon={<HomeIcon />} label="Home" />
        <BottomNavigationItem value="explore" icon={<SearchIcon />} label="Explore" />
        <BottomNavigationItem value="add" icon={<PlusIcon />} label="Create" />  {/* Placeholder */}
        <BottomNavigationItem value="messages" icon={<MessageIcon />} label="Messages" />
        <BottomNavigationItem value="profile" icon={<UserIcon />} label="Profile" />
      </BottomNavigation>

      {/* Floating Action Button */}
      <button className="fixed bottom-24 right-4 w-14 h-14 rounded-full bg-primary text-white shadow-lg">
        <PlusIcon size={28} />
      </button>
    </div>
  )
}
```

### Responsive Behavior (Mobile-First)

```jsx
export function ResponsiveNavigation() {
  const isMobile = useMediaQuery('(max-width: 768px)')

  return (
    <>
      {/* Mobile: Bottom Navigation */}
      {isMobile && (
        <BottomNavigation value={activeTab} onChange={setActiveTab}>
          {/* items */}
        </BottomNavigation>
      )}

      {/* Desktop: Top Navigation */}
      {!isMobile && (
        <TopNavigation value={activeTab} onChange={setActiveTab}>
          {/* items */}
        </TopNavigation>
      )}
    </>
  )
}
```

---

## Do's and Don'ts

### ✅ Do

- Use 3-5 primary navigation items (5 maximum)
- Provide clear, concise labels paired with icons
- Make icons easily recognizable and consistent
- Use bottom navigation for mobile-first designs
- Keep labels brief (1 word or short phrase ideal)
- Use badges for notification counts and alerts
- Ensure active state is clearly visible with color
- Make touch targets at least 48×48px
- Test keyboard navigation thoroughly
- Use semantic HTML (nav element, roles)

### ❌ Don't

- Don't use more than 5 items (use drawer or tabs instead)
- Don't hide icons (pair with labels or use tooltips)
- Don't use abstract or unclear icons
- Don't use bottom navigation on desktop without alternative
- Don't use for secondary or tertiary navigation
- Don't make labels too long or truncate them
- Don't forget to implement aria-label for icon-only navigation
- Don't use with nested menus or dropdowns
- Don't place critical content behind bottom nav overlap
- Don't use inconsistent iconography across items

---

## Animation Specifications

### Item Selection Animation
- **Duration:** 200ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Animation:**
  - Label opacity: 0.6 → 1
  - Icon color: gray → brand color
  - Icon scale: 1 → 1.1 → 1

### Badge Entrance
- **Duration:** 300ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Animation:**
  - Scale: 0 → 1
  - Opacity: 0 → 1

### Scroll Animation (Scrollable Variant)
- **Duration:** 250ms
- **Easing:** ease-out
- **Animation:** Smooth horizontal scroll with momentum

### Ripple Effect (Optional)
- **Duration:** 400ms
- **Easing:** ease-out
- **Animation:** Radial expand from tap point, fade out

---

## Design Tokens Used

- **Colors:**
  - Active item: Brand color (`#3B82F6`)
  - Inactive item: `#6B7280` (gray)
  - Background: `#FFFFFF`
  - Border: `#E5E7EB`
  - Badge: Error red (`#EF4444`)

- **Typography:**
  - Label: `typography.caption` (12px)
  - Font weight: 500 (medium)

- **Spacing:**
  - Vertical padding: 8-12px (per item)
  - Horizontal padding: 16px (container)
  - Icon size: 24px default
  - Icon to label gap: 4px
  - Item gap: 0 (equal distribution)

- **Borders:**
  - Top border: 1px separator line
  - Radius: 0 (full width)
  - Shadow: Subtle elevation shadow

- **Sizing:**
  - Height: 56px default
  - Min height (touch): 48px
  - Icon container: 32px
  - Badge size: 16-20px

---

## Mobile Considerations

- **Thumb Zone:** Bottom navigation is optimal for thumb reach
- **Safe Area:** Account for notches and home indicators (iOS)
- **Orientation:** Handle landscape (usually hide or compress)
- **Scrolling:** Items don't scroll with content (sticky/fixed)
- **Gestures:** Tap to navigate, swipe to scroll (if scrollable variant)
- **Layout Shift:** No layout shift on navigation change
- **Content Gap:** Content area accounts for nav height (pb-16 or similar)

---

## Related Components

- **Navigation Drawer** — For more navigation items or nested menus
- **Top Navigation** — Desktop-first alternative
- **Tabs** — Horizontal tab navigation
- **Breadcrumb** — Secondary navigation
- **Floating Action Button** — Primary action complement
- **Badge** — Notification indicators

---

## Use Case Examples

### Social Media App
- Home (feed)
- Explore/Discover
- Create Post (FAB)
- Messages
- Profile

### E-Commerce App
- Home
- Categories
- Search
- Cart (with badge count)
- Account

### Music Streaming App
- Home
- Search
- Library
- Downloads
- Profile

### Fitness App
- Dashboard
- Workouts
- Add Workout (FAB)
- Progress
- Settings

### Weather App
- Today
- Forecast
- Saved Locations
- Alerts
- Settings

---

## Browser Support

- iOS Safari 12+
- Chrome Mobile 80+
- Firefox Mobile 68+
- Samsung Internet 10+
- All modern mobile browsers

**Desktop Fallback:** Consider showing as top navigation or collapsing into hamburger menu

---

## Performance Considerations

- **Rendering:** Minimal re-renders on item change
- **Touch Response:** <100ms tap feedback
- **Animation:** GPU-accelerated transforms
- **Mobile:** Optimize for low-end devices
- **Accessibility:** No performance penalty for a11y features

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
**Navigation Types Documented:** 5 variants (Icon+Label, Icon-Only, Scrollable, Shifted, FAB Integration)
