# Dock Component Documentation

## Overview

The Dock component is a persistent, floating navigation bar that provides quick access to primary app destinations or actions. Inspired by macOS and mobile-style app docks, it is typically positioned at the bottom or side of the screen and remains visible as users scroll or navigate between views. Unlike a traditional bottom navigation bar — which is always full-width — a Dock is compact, often centered, and may feature magnification or hover-expansion effects. It suits applications that prioritize a clean canvas with minimal persistent chrome, while keeping key navigation actions a single click away.

---

## Component Variants

Dock supports five primary usage patterns:

### 1. **Icon Dock**
- **Purpose**: Compact dock displaying icon-only items with tooltips on hover
- **Use Cases**: Desktop applications, productivity tools, creative suites, sidebars
- **Behavior**: Hover reveals tooltip label; click navigates or triggers action
- **Visual**: Row of evenly-spaced icons in a floating pill-shaped container
- **Magnification**: Optional scale-up effect on hover (macOS dock style)

### 2. **Icon + Label Dock**
- **Purpose**: Dock displaying icon and label for each item simultaneously
- **Use Cases**: Mobile-style navigation on tablets, media apps, dashboard switchers
- **Behavior**: Labels always visible; no tooltip needed
- **Visual**: Slightly taller container; label below icon

### 3. **Segmented Dock**
- **Purpose**: Dock divided into logical groups separated by dividers
- **Use Cases**: Mixed navigation + action items, grouped tool palettes, hybrid nav/toolbar
- **Behavior**: Groups provide visual and semantic separation within the dock
- **Visual**: Dividers between item clusters; each group can have independent sizing

### 4. **Collapsible Dock**
- **Purpose**: Dock that can be minimized to a small trigger button when not in use
- **Use Cases**: Immersive UIs (video players, editors, games), focus modes, canvas tools
- **Behavior**: Collapses to a small pill or edge trigger; expands on hover or click
- **Visual**: Collapsed: small handle or icon; Expanded: full dock slides in

### 5. **Floating Action Dock**
- **Purpose**: Dock acting as a collection of related floating action buttons (FAB group)
- **Use Cases**: Creation flows, context-sensitive quick actions, tool switchers
- **Behavior**: May expand sub-items (speed-dial style) from a primary FAB anchor
- **Visual**: Elevated with shadow; primary FAB anchors the group

---

## Position Variants

| Position | Description | Best For |
|----------|-------------|---------|
| **Bottom Center** | Centered at bottom of viewport | Mobile-style nav, general purpose |
| **Bottom Left** | Anchored to bottom-left | macOS-style, tool docks |
| **Bottom Right** | Anchored to bottom-right | FAB-adjacent docks |
| **Left Center** | Vertical dock on left edge | Desktop sidebars, creative apps |
| **Right Center** | Vertical dock on right edge | Inspector panels, secondary tools |
| **Top Center** | Centered at top | Breadcrumb-style navigation |

---

## Size Variants

| Size | Item Size | Icon | Container Height | Use Case |
|------|-----------|------|-----------------|---------|
| **XS** | 32px | 14px | 44px | Ultra-compact, dense tool palettes |
| **SM** | 40px | 16px | 52px | Compact desktop docks |
| **MD** | 48px | 20px | 60px | **Default**, standard navigation dock |
| **LG** | 56px | 24px | 68px | Tablet and touch-primary UIs |
| **XL** | 64px | 28px | 80px | Large-format or accessibility-focused |

---

## State Specifications

### **Default Item State**
- Icon color: Gray 500 (#6B7280)
- Background: Transparent (within dock container)
- Border Radius: Rounded (8px) on individual items
- Cursor: pointer
- Opacity: 100%
- Scale: 1.0

### **Hover State**
- Icon color: Gray 900 (#111827) or brand color
- Background: Gray 100 (#F3F4F6) or soft tint
- Scale: 1.1–1.2 (magnification effect, optional)
- Tooltip: Appears after 300ms delay
- Duration: 150ms ease-out
- Adjacent items: Scale slightly (0.85–0.95) for macOS-style wave effect (optional)

### **Active / Selected State**
- Icon color: Brand color (#3B82F6) or high-contrast
- Background: Brand tint (soft fill)
- Indicator: Dot below icon, filled background, or underline
- Scale: Returns to 1.0 (no magnification when selected)
- Persistence: Stays active for current view/route

### **Pressed State**
- Scale: 0.92–0.95
- Background: Slightly darker
- Duration: 100ms ease-in

### **Focus State** (Keyboard)
- Outline: 2px solid brand color
- Outline Offset: 2px
- Ring: 0 0 0 4px rgba(59,130,246,0.2)
- Duration: 100ms ease-out

### **Disabled State**
- Icon color: Gray 300 (#D1D5DB)
- Background: Transparent
- Cursor: not-allowed
- Opacity: 40%
- Scale: 1.0 (no magnification)

### **Badge State**
- Badge: Dot or count overlaid top-right of icon
- Dot badge: 8px circle, no border
- Count badge: 16–20px pill
- Badge colors: Danger (#EF4444) for alerts, brand for info
- Scale: Badge does not scale during magnification

### **Tooltip**
- Appears: After 300ms hover (or immediately if dock was recently hovered)
- Position: Above item (bottom dock), or to the right (left dock)
- Content: Item label text
- Behavior: Disappears on mouseout or on click
- Style: Dark background (#1F2937), white text, 12px font, 6px padding, 4px radius

---

## Props API

### **Dock**

```typescript
interface DockProps {
  // Items
  children: React.ReactNode;                   // DockItem elements

  // Layout
  position?:
    | 'bottom-center' | 'bottom-left' | 'bottom-right'
    | 'left-center' | 'right-center' | 'top-center';
  orientation?: 'horizontal' | 'vertical';     // Inferred from position if not set
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';

  // Appearance
  variant?: 'floating' | 'fixed' | 'inline';  // Positioning behavior
  background?: 'light' | 'dark' | 'blur' | 'transparent';
  shadow?: boolean;                            // Elevation shadow
  border?: boolean;                            // Subtle border outline

  // Behavior
  magnification?: boolean;                     // Hover magnification effect
  magnificationScale?: number;                 // Max scale (default 1.25)
  magnificationRange?: number;                 // Items affected by magnification (default 3)
  collapsible?: boolean;                       // Enable collapse/expand
  defaultCollapsed?: boolean;
  onCollapsedChange?: (collapsed: boolean) => void;

  // Selection
  value?: string;                              // Active item value (controlled)
  defaultValue?: string;                       // Initial active item
  onChange?: (value: string) => void;          // Selection change handler

  // Safe Area
  safeAreaInset?: boolean;                     // Respect device safe areas

  // Accessibility
  aria-label?: string;                         // e.g., "Main navigation"
  role?: 'navigation' | 'toolbar';

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

### **DockItem**

```typescript
interface DockItemProps {
  // Identity
  value: string;                               // Unique identifier
  label: string;                               // Display label (tooltip or visible)

  // Content
  icon: React.ReactNode;                       // Icon element
  badge?: string | number;                     // Badge content
  badgeColor?: 'primary' | 'danger' | 'success' | 'warning';
  showBadge?: boolean;

  // Interaction
  onClick?: (event: React.MouseEvent) => void;
  href?: string;                               // For link-based navigation
  disabled?: boolean;

  // Appearance
  showLabel?: boolean;                         // Override dock-level label setting
  tooltip?: string;                            // Custom tooltip (defaults to label)
  active?: boolean;                            // Override active state

  // Accessibility
  aria-label?: string;
  aria-current?: 'page' | 'true' | 'false';

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

### **DockDivider**

```typescript
interface DockDividerProps {
  orientation?: 'vertical' | 'horizontal';     // Inferred from Dock orientation
  spacing?: 'xs' | 'sm' | 'md';
  className?: string;
}
```

---

## Code Examples

### **1. Basic Icon Dock (Bottom Navigation)**
```typescript
import { Dock, DockItem } from '@components/dock';
import { Home, Search, Bell, Settings, User } from 'lucide-react';
import { useState } from 'react';

export function AppDock() {
  const [active, setActive] = useState('home');

  return (
    <Dock
      position="bottom-center"
      value={active}
      onChange={setActive}
      aria-label="Main navigation"
    >
      <DockItem value="home" label="Home" icon={<Home size={20} />} />
      <DockItem value="search" label="Search" icon={<Search size={20} />} />
      <DockItem value="alerts" label="Alerts" icon={<Bell size={20} />} badge={3} badgeColor="danger" />
      <DockItem value="profile" label="Profile" icon={<User size={20} />} />
      <DockItem value="settings" label="Settings" icon={<Settings size={20} />} />
    </Dock>
  );
}
```

### **2. Magnification Dock (macOS Style)**
```typescript
import { Dock, DockItem } from '@components/dock';
import { FileText, Image, Mail, Music, Video, Code, Globe, Cog } from 'lucide-react';

export function MacStyleDock() {
  return (
    <Dock
      position="bottom-center"
      magnification
      magnificationScale={1.4}
      magnificationRange={3}
      background="blur"
      shadow
      aria-label="Application dock"
    >
      <DockItem value="docs" label="Documents" icon={<FileText size={24} />} />
      <DockItem value="photos" label="Photos" icon={<Image size={24} />} />
      <DockItem value="mail" label="Mail" icon={<Mail size={24} />} badge={12} />
      <DockItem value="music" label="Music" icon={<Music size={24} />} />
      <DockItem value="video" label="Video" icon={<Video size={24} />} />
      <DockItem value="code" label="Code" icon={<Code size={24} />} />
      <DockItem value="browser" label="Browser" icon={<Globe size={24} />} />
      <DockItem value="settings" label="Settings" icon={<Cog size={24} />} />
    </Dock>
  );
}
```

### **3. Segmented Dock (Nav + Actions)**
```typescript
import { Dock, DockItem, DockDivider } from '@components/dock';
import { Home, Layers, BarChart2, Plus, Share, Download } from 'lucide-react';
import { useState } from 'react';

export function SegmentedDock() {
  const [active, setActive] = useState('home');

  return (
    <Dock
      position="bottom-center"
      value={active}
      onChange={setActive}
      aria-label="Main navigation"
    >
      {/* Navigation group */}
      <DockItem value="home" label="Home" icon={<Home size={20} />} />
      <DockItem value="projects" label="Projects" icon={<Layers size={20} />} />
      <DockItem value="analytics" label="Analytics" icon={<BarChart2 size={20} />} />

      <DockDivider />

      {/* Action group */}
      <DockItem value="create" label="Create" icon={<Plus size={20} />} onClick={() => {}} />
      <DockItem value="share" label="Share" icon={<Share size={20} />} onClick={() => {}} />
      <DockItem value="export" label="Export" icon={<Download size={20} />} onClick={() => {}} />
    </Dock>
  );
}
```

### **4. Vertical Side Dock**
```typescript
import { Dock, DockItem } from '@components/dock';
import { Pen, MousePointer, Square, Circle, Type, Eraser } from 'lucide-react';
import { useState } from 'react';

export function ToolDock() {
  const [tool, setTool] = useState('select');

  return (
    <Dock
      position="left-center"
      orientation="vertical"
      value={tool}
      onChange={setTool}
      size="sm"
      aria-label="Drawing tools"
      role="toolbar"
    >
      <DockItem value="select" label="Select" icon={<MousePointer size={16} />} />
      <DockItem value="pen" label="Pen" icon={<Pen size={16} />} />
      <DockItem value="rectangle" label="Rectangle" icon={<Square size={16} />} />
      <DockItem value="circle" label="Circle" icon={<Circle size={16} />} />
      <DockItem value="text" label="Text" icon={<Type size={16} />} />
      <DockItem value="eraser" label="Eraser" icon={<Eraser size={16} />} />
    </Dock>
  );
}
```

### **5. Collapsible Dock**
```typescript
import { Dock, DockItem } from '@components/dock';
import { Home, Inbox, Calendar, Settings } from 'lucide-react';
import { useState } from 'react';

export function CollapsibleDock() {
  const [active, setActive] = useState('home');

  return (
    <Dock
      position="bottom-center"
      value={active}
      onChange={setActive}
      collapsible
      defaultCollapsed={false}
      background="blur"
      shadow
      aria-label="Main navigation"
    >
      <DockItem value="home" label="Home" icon={<Home size={20} />} />
      <DockItem value="inbox" label="Inbox" icon={<Inbox size={20} />} badge={5} />
      <DockItem value="calendar" label="Calendar" icon={<Calendar size={20} />} />
      <DockItem value="settings" label="Settings" icon={<Settings size={20} />} />
    </Dock>
  );
}
```

### **6. Dock with Router Integration**
```typescript
import { Dock, DockItem } from '@components/dock';
import { useNavigate, useLocation } from 'react-router-dom';
import { Home, Folder, Users, Settings } from 'lucide-react';

const navItems = [
  { value: '/', label: 'Home', icon: <Home size={20} /> },
  { value: '/projects', label: 'Projects', icon: <Folder size={20} /> },
  { value: '/team', label: 'Team', icon: <Users size={20} /> },
  { value: '/settings', label: 'Settings', icon: <Settings size={20} /> },
];

export function RouterDock() {
  const navigate = useNavigate();
  const location = useLocation();
  const active = navItems.find((i) => location.pathname.startsWith(i.value))?.value ?? '/';

  return (
    <Dock
      position="bottom-center"
      value={active}
      onChange={(val) => navigate(val)}
      aria-label="Main navigation"
      role="navigation"
    >
      {navItems.map((item) => (
        <DockItem
          key={item.value}
          value={item.value}
          label={item.label}
          icon={item.icon}
          href={item.value}
          aria-current={active === item.value ? 'page' : undefined}
        />
      ))}
    </Dock>
  );
}
```

---

## Accessibility Specifications

### **ARIA Roles**
- **`role="navigation"` + `aria-label`**: When dock is used for primary page navigation
- **`role="toolbar"`**: When dock is used for tool selection or actions (not navigation)
- **`role="tablist"`**: If dock controls visible content panels (tab-like behavior)
- Items use `<button>` or `<a>` elements — never `<div>` with onClick

### **ARIA Attributes on Items**
- **`aria-label`**: Required if label is not always visible (icon-only docks)
- **`aria-current="page"`**: On the currently active navigation item
- **`aria-pressed`**: For toolbar items that toggle a state
- **`aria-selected`**: For tablist-pattern items
- **`aria-disabled`**: On disabled items
- **`aria-describedby`**: Link to badge description if badge has semantic meaning

### **Keyboard Navigation**
- **Tab**: Moves focus to the dock (first item or last focused item)
- **Shift+Tab**: Moves focus out of dock
- **Arrow Keys** (within dock):
  - Left/Right (horizontal): Navigate between items
  - Up/Down (vertical): Navigate between items
  - Home: Jump to first item
  - End: Jump to last item
- **Enter / Space**: Activate focused item
- **Escape**: Close tooltip if open; collapse dock if collapsible

### **Focus Management**
- Focus ring: 2px solid, 2px offset on all items
- Focus order follows visual left-to-right (or top-to-bottom for vertical)
- When dock collapses, move focus to the collapse trigger button
- When dock expands, restore focus to previously focused item or first item

### **Tooltips and Screen Readers**
- Icon-only docks: `aria-label` on each item makes tooltips informational, not essential
- Tooltip shown visually: `role="tooltip"` with `aria-describedby` linkage
- Screen readers should read the item label directly from `aria-label`, not the tooltip

### **Touch Accessibility**
- Minimum touch target: 44×44px per item
- Adequate spacing between items (≥8px)
- Badge content announced: "Alerts, 3 notifications, button"
- Magnification effect purely visual — no functional consequence

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .dock-item { transition: none; transform: none !important; }
  .dock-tooltip { animation: none; }
}
```

---

## Animation Specifications

### **Hover Magnification**
- **Target**: `transform: scale()`
- **Duration**: 200ms
- **Easing**: cubic-bezier(0.34, 1.56, 0.64, 1) (slight spring overshoot)
- **Max Scale**: 1.25 (default); configurable
- **Adjacent Items**: Scale to 1.1 (nearest), 1.05 (next), 1.0 (beyond range)

### **Active Item Indicator**
- **Target**: Dot or background opacity + scale
- **Duration**: 200ms ease-out

### **Tooltip Entrance**
- **Target**: Opacity + translateY
- **Duration**: 150ms ease-out
- **Delay**: 300ms after hover start
  ```css
  @keyframes tooltipEnter {
    from { opacity: 0; transform: translateY(4px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  ```

### **Dock Collapse / Expand**
- **Target**: Width (horizontal) or height (vertical), opacity
- **Duration**: 250ms
- **Easing**: cubic-bezier(0.4, 0, 0.2, 1)
  ```css
  max-width: 0 → 400px; /* or max-height for vertical */
  opacity: 0 → 1;
  ```

### **Press**
- **Target**: `transform: scale(0.92)`
- **Duration**: 100ms ease-in
- **Return**: 150ms ease-out back to 1.0

---

## Design Tokens

### **Container**
| Property | Value |
|----------|-------|
| Background (light) | rgba(255,255,255,0.85) |
| Background (blur) | rgba(255,255,255,0.7) + `backdrop-filter: blur(20px)` |
| Background (dark) | rgba(17,24,39,0.85) |
| Border | 1px solid rgba(0,0,0,0.08) |
| Border Radius | 16px (pill) |
| Shadow | 0 8px 32px rgba(0,0,0,0.12), 0 2px 8px rgba(0,0,0,0.08) |
| Padding | 8px |
| Item Gap | 4px |

### **Items**
| State | Icon Color | Background |
|-------|-----------|------------|
| Default | #6B7280 | transparent |
| Hover | #111827 | #F3F4F6 |
| Active | #3B82F6 | #EFF6FF |
| Pressed | #1D4ED8 | #DBEAFE |
| Disabled | #D1D5DB | transparent |

### **Active Indicator**
| Property | Value |
|----------|-------|
| Dot diameter | 4px |
| Dot color | #3B82F6 |
| Dot position | Below icon (bottom dock), beside icon (side dock) |

### **Tooltip**
| Property | Value |
|----------|-------|
| Background | #1F2937 |
| Text | #F9FAFB |
| Font size | 12px |
| Padding | 4px 8px |
| Border radius | 4px |
| Offset from item | 8px |

---

## Best Practices

### **Item Count**
- Bottom dock: 3–7 items (5 is optimal); use segmented dock for 8+
- Side dock: Up to 10 items with appropriate sizing
- Fewer is better — dock items should be primary, high-frequency destinations
- Move secondary items to a settings/profile overflow menu

### **Icon Selection**
- Use filled icons for active state, outlined for inactive (reinforces selection)
- Keep icon style consistent across all dock items (all outlined or all filled)
- Avoid highly detailed icons at small sizes — simplify at 20px and below
- Test icon legibility at SM size (16px) if using compact docks

### **Badges**
- Use sparingly — badges on multiple items simultaneously create visual noise
- Use dot badge for "has updates", count badge only up to 99 (show "99+" beyond)
- Clear badges when user visits the destination

### **Magnification**
- Use only in desktop contexts; disable on touch devices
- Ensure magnification doesn't cause dock items to overlap other content
- Max magnification of 1.4× — beyond this feels disproportionate
- Always respect `prefers-reduced-motion`

### **Positioning**
- Bottom-center is the most universally familiar position
- Left/right side docks suit desktop-first productivity tools
- Avoid top-center (conflicts with browser/OS chrome)
- Always account for safe areas on mobile (notches, home indicator)

### **Responsive Behavior**
- Desktop (≥1024px): Standard floating dock with magnification
- Tablet (640–1023px): Dock without magnification; slightly larger items
- Mobile (<640px): Consider switching to full-width bottom navigation instead

---

## Related Components

- **Bottom Navigation**: Full-width mobile navigation bar; use instead of Dock at mobile widths
- **Sidebar**: Full-height navigation panel for complex, multi-level navigation
- **Toolbar**: Inline, non-floating tool strip; use when dock needs to be embedded
- **FAB**: Single floating action button; use Dock for multiple primary actions
- **Tabs**: Tab navigation for switching content panels within a view

---

## Version History

- **v1.0**: Basic horizontal dock, icon items, active indicator, tooltip
- **v1.1**: Vertical orientation, side positioning, badge support
- **v1.2**: Segmented dock with DockDivider, blur background variant
- **v2.0**: Magnification effect, collapsible mode, full keyboard nav, a11y audit
- **v2.1**: Router integration helpers, safe area support, reduced-motion compliance

---

## Testing Checklist

- [ ] Dock renders at correct position (bottom-center, left, right, etc.)
- [ ] Icon-only items show tooltip on hover after 300ms delay
- [ ] Active item shows selected indicator (dot, filled background)
- [ ] Hover state changes icon color and background
- [ ] Pressed state scales item down (0.92×)
- [ ] Magnification effect scales hovered item and adjacent items correctly
- [ ] Magnification disabled on touch devices
- [ ] Disabled items show no hover or active feedback
- [ ] Badge renders at top-right of icon with correct color
- [ ] Badge count truncates at "99+"
- [ ] Keyboard: Arrow keys navigate between items
- [ ] Keyboard: Home/End jumps to first/last item
- [ ] Keyboard: Enter/Space activates item
- [ ] Keyboard: Escape dismisses tooltip
- [ ] Focus ring visible on keyboard focus
- [ ] `role="navigation"` and `aria-label` set on dock container
- [ ] `aria-current="page"` set on active navigation item
- [ ] Icon-only items have `aria-label`
- [ ] Collapse/expand animation smooth at 60fps
- [ ] Dock respects device safe areas on mobile
- [ ] Reduced motion uses no-scale transitions
- [ ] Touch targets all ≥ 44×44px
- [ ] Blur background renders correctly in supported browsers
