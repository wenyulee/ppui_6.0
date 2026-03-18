# Header Component Documentation

## Overview

The Header component is the primary navigation and branding container at the top of an application or page. It anchors the user's orientation within the product, provides access to global navigation, search, account controls, and contextual actions. A well-structured header communicates hierarchy at a glance — users immediately know where they are, what they can do, and how to get elsewhere. Headers range from minimal single-line bars to complex multi-zone layouts with mega-menus, breadcrumbs, and persistent toolbars. They must be responsive, accessible as a landmark region, and performant since they render on every page.

---

## Component Variants

Header supports six primary usage patterns:

### 1. **App Header** (Default)
- **Purpose**: Primary navigation bar for web applications; persists across all views
- **Use Cases**: SaaS dashboards, admin panels, productivity tools, internal tools
- **Zones**: Logo/brand left | Navigation center or left | Search + Actions + Avatar right
- **Behavior**: Fixed or sticky; collapses to hamburger menu on mobile
- **Height**: 56–64px (default)

### 2. **Marketing / Site Header**
- **Purpose**: Top navigation for marketing sites, landing pages, and content platforms
- **Use Cases**: Homepage, pricing page, blog, documentation site
- **Zones**: Logo left | Navigation center | CTA buttons right
- **Behavior**: Often transparent over hero; becomes opaque on scroll; mega-menu support
- **Height**: 64–80px

### 3. **Page Header**
- **Purpose**: Section-level header within an app page — not the global nav, but a title bar for the current view
- **Use Cases**: Dashboard page title, settings page header, list view header
- **Zones**: Breadcrumb top | Page title + badge left | Action buttons right
- **Behavior**: Stays in document flow (not fixed); may have sticky option
- **Height**: 48–72px

### 4. **Contextual / Section Header**
- **Purpose**: Sub-header within a page section, card, or panel
- **Use Cases**: Card header, drawer header, modal header, sidebar section title
- **Zones**: Icon + title left | Action (menu, close) right
- **Behavior**: Stays in document flow; no navigation
- **Height**: 40–56px

### 5. **Minimal Header**
- **Purpose**: Stripped-back header for focused/immersive contexts
- **Use Cases**: Editor mode, full-screen apps, onboarding flows, checkout
- **Zones**: Logo only, or logo + one action (exit/close)
- **Behavior**: Fixed; minimal chrome to reduce distraction
- **Height**: 48–56px

### 6. **Mobile Header**
- **Purpose**: Optimized header for small viewports with touch-friendly controls
- **Use Cases**: Mobile web, PWA, responsive breakpoint below 640px
- **Zones**: Hamburger left | Title center | Actions right
- **Behavior**: Triggers slide-out drawer or sheet navigation
- **Height**: 52–60px

---

## Anatomy

A complete App Header is composed of distinct zones:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  [Logo]   [Nav Item] [Nav Item ▾] [Nav Item]     [Search] [Notif] [Avatar]  │
│   Left              Center / Left Navigation               Right Actions     │
└──────────────────────────────────────────────────────────────────────────────┘
```

### **Zone Breakdown**

**Start Zone (Left)**
- Brand logo (links to home/dashboard)
- Optional product name or wordmark
- Hamburger menu trigger (mobile only)

**Navigation Zone (Center or Left-adjacent)**
- Primary nav items (links, dropdowns, mega-menus)
- Max 5–7 items for usability
- Active state indicates current section

**End Zone (Right)**
- Search trigger or inline search bar
- Notification bell (with badge)
- Help / Documentation link
- Account avatar / user menu

**Sub-Header Zone** (Optional, below main bar)
- Breadcrumbs
- Secondary navigation tabs
- Contextual toolbar (filters, view toggles, bulk actions)
- Progress indicator

---

## Size Variants

| Size | Height | Logo Size | Nav Font | Avatar | Use Case |
|------|--------|-----------|----------|--------|---------|
| **SM** | 48px | 24px | 13px | 28px | Compact / dense applications |
| **MD** | 56px | 28px | 14px | 32px | **Default**, standard app header |
| **LG** | 64px | 32px | 15px | 36px | Marketing headers, generous layouts |
| **XL** | 80px | 40px | 16px | 40px | Hero-adjacent, prominent branding |

---

## State Specifications

### **Default State**
- Background: White (#FFFFFF) or brand color
- Border-bottom: 1px solid #E5E7EB
- Shadow: None or Elevation 1 (0 1px 3px rgba(0,0,0,0.08))
- Position: Fixed top or sticky
- z-index: 100+

### **Scrolled State** (Marketing headers)
- Background: Transitions from transparent to opaque white
- Shadow: Elevation 2 appears (0 2px 8px rgba(0,0,0,0.1))
- Duration: 200ms ease-out on scroll threshold (~50px)
- Backdrop filter: Optional blur on scroll

### **Nav Item — Default**
- Color: Gray 600 (#4B5563)
- Background: Transparent
- Font weight: 500
- Cursor: pointer

### **Nav Item — Hover**
- Color: Gray 900 (#111827)
- Background: Gray 50 (#F9FAFB)
- Duration: 150ms ease-out

### **Nav Item — Active / Current**
- Color: Brand color (#3B82F6)
- Background: Brand tint (#EFF6FF)
- Indicator: Bottom border (2px brand) or filled background
- Font weight: 600

### **Nav Item — Focus**
- Outline: 2px solid brand, 2px offset
- Duration: 100ms ease-out

### **Nav Item — Disabled**
- Color: Gray 300 (#D1D5DB)
- Cursor: not-allowed
- No hover or active feedback

### **Dropdown — Open**
- Shadow: Elevation 4
- Border: 1px #E5E7EB
- Border radius: 8px
- Animation: 150ms scale + fade from top-left origin
- z-index: 200

### **Mobile Menu — Open**
- Drawer slides in from left (or sheet from bottom)
- Backdrop: rgba(0,0,0,0.4)
- Duration: 250ms ease-out

---

## Props API

### **Header**

```typescript
interface HeaderProps {
  // Branding
  logo?: React.ReactNode;                      // Logo component or image
  logoHref?: string;                           // Logo link destination (default: '/')
  logoAriaLabel?: string;                      // e.g., "Go to homepage"
  productName?: string;                        // Product/app name beside logo

  // Navigation
  navigation?: NavItem[];                      // Navigation item definitions
  activeNavItem?: string;                      // Value of currently active item
  onNavChange?: (value: string) => void;       // Navigation change handler

  // Actions (Right Zone)
  actions?: React.ReactNode;                   // Custom right-side elements
  showSearch?: boolean;                        // Show search trigger
  onSearchClick?: () => void;                  // Search trigger handler
  searchPlaceholder?: string;

  // User / Account
  user?: {
    name: string;
    email?: string;
    avatarSrc?: string;
    avatarInitials?: string;
  };
  userMenuItems?: UserMenuItem[];              // Account dropdown items
  onUserMenuAction?: (action: string) => void;

  // Notifications
  notificationCount?: number;                 // Badge count on bell icon
  onNotificationClick?: () => void;

  // Appearance
  variant?: 'app' | 'marketing' | 'page' | 'contextual' | 'minimal' | 'mobile';
  size?: 'sm' | 'md' | 'lg' | 'xl';
  background?: 'white' | 'brand' | 'dark' | 'transparent' | 'blur';
  sticky?: boolean;                           // Sticky on scroll (default: true)
  bordered?: boolean;                         // Bottom border
  elevated?: boolean;                         // Drop shadow

  // Sub-header
  breadcrumbs?: BreadcrumbItem[];
  subNav?: React.ReactNode;                   // Secondary nav tabs or toolbar
  pageTitle?: string;                         // For Page Header variant

  // Mobile
  mobileMenuTrigger?: React.ReactNode;        // Custom hamburger trigger
  mobileMenuContent?: React.ReactNode;        // Drawer/sheet nav content

  // Accessibility
  'aria-label'?: string;                       // e.g., "Main navigation"
  skipLinkTarget?: string;                     // ID of main content for skip link

  // Styling
  className?: string;
  style?: React.CSSProperties;

  // Children (override slots)
  startContent?: React.ReactNode;             // Override start zone
  centerContent?: React.ReactNode;            // Override center/nav zone
  endContent?: React.ReactNode;               // Override end zone
}
```

### **NavItem**

```typescript
interface NavItem {
  value: string;                               // Unique identifier
  label: string;                               // Display label
  href?: string;                               // Link destination
  icon?: React.ReactNode;                      // Optional leading icon
  badge?: string | number;                     // Badge content
  disabled?: boolean;
  children?: NavItem[];                        // Dropdown sub-items
  onClick?: () => void;                        // Custom click handler
  external?: boolean;                          // Opens in new tab
  // Mega-menu
  megaMenu?: React.ReactNode;                  // Full-width dropdown content
}
```

### **BreadcrumbItem**

```typescript
interface BreadcrumbItem {
  label: string;
  href?: string;
  icon?: React.ReactNode;
  current?: boolean;                           // Current page (aria-current="page")
}
```

---

## Code Examples

### **1. Standard App Header**
```typescript
import { Header } from '@components/header';
import { Bell, Search } from 'lucide-react';

export function AppHeader() {
  const navigation = [
    { value: 'dashboard', label: 'Dashboard', href: '/dashboard' },
    { value: 'projects', label: 'Projects', href: '/projects' },
    { value: 'team', label: 'Team', href: '/team' },
    {
      value: 'settings',
      label: 'Settings',
      children: [
        { value: 'profile', label: 'Profile', href: '/settings/profile' },
        { value: 'billing', label: 'Billing', href: '/settings/billing' },
        { value: 'security', label: 'Security', href: '/settings/security' },
      ],
    },
  ];

  return (
    <Header
      logo={<img src="/logo.svg" alt="Acme" height={28} />}
      logoHref="/"
      navigation={navigation}
      activeNavItem="projects"
      showSearch
      onSearchClick={() => {}}
      notificationCount={3}
      user={{
        name: 'Sarah Johnson',
        email: 'sarah@acme.com',
        avatarSrc: '/avatars/sarah.jpg',
      }}
      userMenuItems={[
        { value: 'profile', label: 'My Profile', href: '/profile' },
        { value: 'settings', label: 'Settings', href: '/settings' },
        { value: 'divider' },
        { value: 'logout', label: 'Sign out', onClick: () => {} },
      ]}
      sticky
      bordered
      skipLinkTarget="main-content"
      aria-label="Main navigation"
    />
  );
}
```

### **2. Marketing Site Header**
```typescript
import { Header } from '@components/header';
import { Button } from '@components/button';

export function MarketingHeader() {
  const navigation = [
    { value: 'product', label: 'Product', href: '/product' },
    { value: 'pricing', label: 'Pricing', href: '/pricing' },
    {
      value: 'resources',
      label: 'Resources',
      children: [
        { value: 'docs', label: 'Documentation', href: '/docs' },
        { value: 'blog', label: 'Blog', href: '/blog' },
        { value: 'changelog', label: 'Changelog', href: '/changelog' },
      ],
    },
    { value: 'company', label: 'Company', href: '/about' },
  ];

  return (
    <Header
      variant="marketing"
      logo={<img src="/logo.svg" alt="Acme" height={32} />}
      navigation={navigation}
      background="transparent"
      elevated={false}
      endContent={
        <div className="flex items-center gap-3">
          <a href="/login" className="text-sm font-medium text-gray-700">
            Sign in
          </a>
          <Button color="primary" size="sm" label="Get started" href="/signup" />
        </div>
      }
      aria-label="Site navigation"
    />
  );
}
```

### **3. Page Header with Breadcrumbs and Actions**
```typescript
import { Header } from '@components/header';
import { Button } from '@components/button';
import { Plus, Download } from 'lucide-react';

export function ProjectPageHeader() {
  return (
    <Header
      variant="page"
      pageTitle="Q4 Marketing Campaign"
      breadcrumbs={[
        { label: 'Projects', href: '/projects' },
        { label: 'Marketing', href: '/projects/marketing' },
        { label: 'Q4 Campaign', current: true },
      ]}
      endContent={
        <div className="flex items-center gap-2">
          <Button
            variant="outlined"
            size="sm"
            icon={<Download size={16} />}
            label="Export"
          />
          <Button
            color="primary"
            size="sm"
            icon={<Plus size={16} />}
            label="Add task"
          />
        </div>
      }
      sticky={false}
      bordered
    />
  );
}
```

### **4. Minimal Header (Editor Mode)**
```typescript
import { Header } from '@components/header';
import { Button } from '@components/button';
import { X, Save } from 'lucide-react';

export function EditorHeader({ onSave, onExit }: {
  onSave: () => void;
  onExit: () => void;
}) {
  return (
    <Header
      variant="minimal"
      logo={<img src="/logo-mark.svg" alt="Acme" height={24} />}
      size="sm"
      background="white"
      bordered
      endContent={
        <div className="flex items-center gap-2">
          <Button
            variant="ghost"
            size="sm"
            icon={<X size={16} />}
            label="Exit"
            onClick={onExit}
          />
          <Button
            color="primary"
            size="sm"
            icon={<Save size={16} />}
            label="Save"
            onClick={onSave}
          />
        </div>
      }
      aria-label="Editor toolbar"
    />
  );
}
```

### **5. Dark / Brand Header**
```typescript
import { Header } from '@components/header';

export function DarkHeader() {
  const navigation = [
    { value: 'home', label: 'Home', href: '/' },
    { value: 'explore', label: 'Explore', href: '/explore' },
    { value: 'library', label: 'My Library', href: '/library' },
  ];

  return (
    <Header
      logo={<img src="/logo-white.svg" alt="Acme" height={28} />}
      navigation={navigation}
      activeNavItem="explore"
      background="dark"
      user={{
        name: 'Alex Chen',
        avatarInitials: 'AC',
      }}
      notificationCount={7}
      aria-label="Main navigation"
    />
  );
}
```

### **6. Header with Mega-Menu**
```typescript
import { Header } from '@components/header';

const ProductMegaMenu = () => (
  <div className="grid grid-cols-3 gap-6 p-6 w-full max-w-4xl">
    <div>
      <h3 className="text-xs font-semibold uppercase tracking-wider text-gray-500 mb-3">
        Core Features
      </h3>
      <ul className="space-y-2">
        <li><a href="/features/analytics" className="text-sm hover:text-blue-600">Analytics</a></li>
        <li><a href="/features/reporting" className="text-sm hover:text-blue-600">Reporting</a></li>
        <li><a href="/features/automation" className="text-sm hover:text-blue-600">Automation</a></li>
      </ul>
    </div>
    {/* more columns... */}
  </div>
);

export function MegaMenuHeader() {
  const navigation = [
    { value: 'home', label: 'Home', href: '/' },
    { value: 'product', label: 'Product', megaMenu: <ProductMegaMenu /> },
    { value: 'pricing', label: 'Pricing', href: '/pricing' },
  ];

  return (
    <Header
      variant="marketing"
      logo={<img src="/logo.svg" alt="Acme" height={32} />}
      navigation={navigation}
      size="lg"
      sticky
      elevated
      aria-label="Site navigation"
    />
  );
}
```

### **7. Responsive Header with Mobile Drawer**
```typescript
import { Header } from '@components/header';
import { useState } from 'react';
import { Drawer } from '@components/drawer';

export function ResponsiveHeader() {
  const [mobileOpen, setMobileOpen] = useState(false);

  const navItems = [
    { value: 'dashboard', label: 'Dashboard', href: '/dashboard' },
    { value: 'projects', label: 'Projects', href: '/projects' },
    { value: 'team', label: 'Team', href: '/team' },
    { value: 'settings', label: 'Settings', href: '/settings' },
  ];

  return (
    <>
      <Header
        logo={<img src="/logo.svg" alt="Acme" height={28} />}
        navigation={navItems}
        activeNavItem="dashboard"
        mobileMenuContent={
          <Drawer
            open={mobileOpen}
            onClose={() => setMobileOpen(false)}
            placement="left"
          >
            <nav>
              {navItems.map((item) => (
                <a key={item.value} href={item.href}>{item.label}</a>
              ))}
            </nav>
          </Drawer>
        }
        showSearch
        notificationCount={2}
        user={{ name: 'Sarah', avatarInitials: 'SJ' }}
        aria-label="Main navigation"
      />
    </>
  );
}
```

### **8. Contextual Section Header (Card / Panel)**
```typescript
import { Header } from '@components/header';
import { MoreHorizontal } from 'lucide-react';
import { Button } from '@components/button';

export function CardHeader({ title }: { title: string }) {
  return (
    <Header
      variant="contextual"
      pageTitle={title}
      size="sm"
      sticky={false}
      bordered
      endContent={
        <Button
          variant="ghost"
          size="sm"
          icon={<MoreHorizontal size={16} />}
          aria-label="More options"
        />
      }
    />
  );
}
```

---

## Accessibility Specifications

### **Landmark Role**
- Header must use `<header>` element, which carries implicit `role="banner"` at the page level
- `role="banner"` should only appear once per page — it is the site-wide header
- Section-level headers (contextual/page) use `<div>` or `<section>` — not `<header>` — to avoid duplicate landmarks
- Always pair with `aria-label` when multiple navigation landmarks exist on the page

### **Skip Navigation Link**
- A "Skip to main content" link must be the first focusable element in the header
- Visually hidden by default; visible on focus (`:focus-visible`)
- Target: `<main id="main-content">` or the primary content region
- Critical for keyboard and screen reader users to bypass repeated navigation
  ```html
  <a href="#main-content" class="skip-link">Skip to main content</a>
  ```

### **Navigation Landmark**
- Wrap nav items in `<nav aria-label="Main navigation">`
- If multiple `<nav>` regions exist (header, footer, sidebar), each needs a unique `aria-label`
- Use `<ul>` + `<li>` for nav item lists (semantic list structure)

### **Active / Current Page**
- Current page nav item: `aria-current="page"`
- Current section (not exact page): `aria-current="true"`
- Never use only color to indicate active state — pair with `aria-current` and visual weight change

### **Dropdown Menus**
- Parent nav item (trigger): `aria-haspopup="true"`, `aria-expanded="false"/"true"`
- Dropdown container: `role="menu"` or `role="list"` depending on behavior
- Dropdown items: `role="menuitem"` or `<a>` elements
- Keyboard: Enter/Space opens, Arrow keys navigate, Escape closes and returns focus to trigger

### **Mega-Menu**
- Trigger: `aria-haspopup="true"`, `aria-expanded`, `aria-controls`
- Mega-menu panel: `role="region"`, `aria-label="[Menu name] navigation"`
- Full keyboard traversal within mega-menu
- Escape closes mega-menu and returns focus to trigger

### **Keyboard Navigation**
- **Tab**: Move forward through header interactive elements (logo, nav items, actions, avatar)
- **Shift+Tab**: Move backward
- **Enter / Space**: Activate nav item, open dropdown
- **Arrow Keys** (within dropdowns): Up/Down navigate items; Left/Right navigate top-level items
- **Escape**: Close open dropdown or mega-menu; return focus to trigger
- **Home / End**: Jump to first / last item in open dropdown

### **Mobile Menu**
- Hamburger trigger: `aria-label="Open navigation menu"` / `aria-label="Close navigation menu"` (toggle)
- `aria-expanded` on trigger reflects open state
- `aria-controls` points to mobile nav panel ID
- When open: Focus moves into mobile nav panel; Tab cycles through items; Escape closes
- Backdrop click closes menu; focus returns to trigger

### **Avatar / User Menu**
- Trigger: `<button>` with `aria-label="Account menu for [User Name]"`
- `aria-haspopup="true"`, `aria-expanded`
- Dropdown: `role="menu"` with `role="menuitem"` children
- Dividers inside dropdown: `role="separator"`

### **Notification Bell**
- `<button aria-label="Notifications, 3 unread">`
- Update aria-label when count changes: `aria-label="Notifications, 7 unread"`
- Badge count: `aria-hidden="true"` (the aria-label carries the count)

### **Logo Link**
- `<a href="/" aria-label="Acme — Go to homepage">`
- Avoid `aria-label="Logo"` — provide meaningful destination context

### **Screen Reader Flow**
1. Skip link announced first: "Skip to main content, link"
2. Banner landmark entered: "Banner"
3. Logo link: "Acme — Go to homepage, link"
4. Navigation landmark: "Main navigation, navigation"
5. Nav items: "Dashboard, link" / "Projects, link" / "Settings, collapsed button"
6. Actions: "Search, button" / "Notifications, 3 unread, button"
7. User: "Account menu for Sarah Johnson, button"

---

## Animation Specifications

### **Scroll → Opaque Transition** (Marketing headers)
- **Target**: Background opacity, box-shadow
- **Trigger**: Window scroll > 50px
- **Duration**: 200ms ease-out
  ```css
  background: rgba(255,255,255,0) → rgba(255,255,255,1);
  box-shadow: none → 0 2px 8px rgba(0,0,0,0.1);
  transition: background 200ms ease-out, box-shadow 200ms ease-out;
  ```

### **Dropdown Open**
- **Target**: Opacity + scaleY + translateY
- **Duration**: 150ms ease-out
- **Origin**: Top center of dropdown
  ```css
  @keyframes dropdownOpen {
    from { opacity: 0; transform: scaleY(0.95) translateY(-4px); }
    to   { opacity: 1; transform: scaleY(1) translateY(0); }
  }
  transform-origin: top center;
  animation: dropdownOpen 150ms ease-out;
  ```

### **Dropdown Close**
- **Duration**: 100ms ease-in
- **Target**: Opacity + scaleY
  ```css
  @keyframes dropdownClose {
    from { opacity: 1; transform: scaleY(1); }
    to   { opacity: 0; transform: scaleY(0.95); }
  }
  ```

### **Mobile Menu Slide In**
- **Target**: translateX (drawer) or translateY (sheet)
- **Duration**: 250ms cubic-bezier(0.4, 0, 0.2, 1)
  ```css
  transform: translateX(-100%) → translateX(0);
  ```

### **Active Indicator**
- **Target**: Width (underline) or background opacity
- **Duration**: 200ms ease-out
  ```css
  width: 0 → 100%; /* bottom border indicator */
  ```

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .header-dropdown { animation: none; opacity: 1; }
  .header-mobile-menu { transition: none; }
  .header-scroll { transition: none; }
}
```

---

## Design Tokens

### **Layout**
| Property | Value |
|----------|-------|
| Height SM | 48px |
| Height MD | 56px |
| Height LG | 64px |
| Height XL | 80px |
| Horizontal padding | 16px (mobile), 24px (tablet), 32px (desktop) |
| Logo gap | 8px (logo → product name) |
| Nav item gap | 4px |
| Action gap | 8px |
| z-index | 100 |

### **Colors (Light)**
| Purpose | Token | Value |
|---------|-------|-------|
| Background | `--header-bg` | #FFFFFF |
| Border | `--header-border` | #E5E7EB |
| Shadow | `--header-shadow` | 0 1px 3px rgba(0,0,0,0.08) |
| Nav text | `--header-nav-text` | #4B5563 |
| Nav text hover | `--header-nav-text-hover` | #111827 |
| Nav text active | `--header-nav-text-active` | #3B82F6 |
| Nav bg hover | `--header-nav-bg-hover` | #F9FAFB |
| Nav bg active | `--header-nav-bg-active` | #EFF6FF |
| Active indicator | `--header-active-indicator` | #3B82F6 |

### **Colors (Dark)**
| Purpose | Token | Value |
|---------|-------|-------|
| Background | `--header-dark-bg` | #111827 |
| Border | `--header-dark-border` | #1F2937 |
| Nav text | `--header-dark-nav-text` | #D1D5DB |
| Nav text hover | `--header-dark-nav-text-hover` | #F9FAFB |
| Nav text active | `--header-dark-nav-text-active` | #60A5FA |

---

## Responsive Behavior

### **Breakpoints**
| Breakpoint | Header Behavior |
|------------|----------------|
| **Mobile** (< 640px) | Hamburger + Logo + Actions; nav hidden in drawer |
| **Tablet** (640–1024px) | Hamburger + Logo + 3–4 nav items; some actions shown |
| **Desktop** (> 1024px) | Full header with all zones visible |
| **Wide** (> 1280px) | Increased horizontal padding; centered max-width container |

### **Nav Item Overflow**
- On tablet: Use priority+ pattern — hide lower-priority items in "More" dropdown
- On mobile: Move all nav items to mobile drawer
- Never truncate visible nav item text — hide items before truncating

### **Action Priority (Mobile)**
- Always visible: Hamburger, logo, primary action
- Hidden on mobile: Secondary actions, search (tap to expand), notifications (in drawer)
- User avatar: Always visible; reduced to icon-only on small screens

---

## Best Practices

### **Navigation Structure**
- Limit top-level nav to 5–7 items maximum
- Group related items under a dropdown rather than expanding the top level
- Use clear, single-word labels where possible ("Projects" not "Your Projects")
- Ensure every nav item has a unique and descriptive label
- Don't nest dropdowns more than one level deep

### **Logo**
- Logo must be a link to home/dashboard
- Provide meaningful `aria-label` (not just "Logo")
- Ensure logo has sufficient contrast on header background
- SVG logos preferred — scale cleanly at all sizes

### **Search**
- On desktop: Inline search bar or keyboard-triggered (⌘K / Ctrl+K)
- On mobile: Search icon tap-expands to full-width input
- Always associate search input with a visible or `aria-label` label
- Provide immediate results or navigate to search results page

### **Actions Zone**
- Show max 3–4 icons in the actions zone before grouping in overflow
- Notification badge: Don't show counts above "99+" — display "99+" instead
- Use tooltips on icon-only actions to reveal their purpose on hover

### **Performance**
- Render header server-side when possible (important for SEO and FCP)
- Lazy-load mega-menu content if complex
- Avoid large images in header — use SVG logos
- Keep header JS bundle small — it blocks above-the-fold rendering

### **Sticky Behavior**
- App headers: Almost always sticky/fixed
- Marketing headers: Sticky preferred; transparent-to-opaque on scroll
- Page headers (section level): Sticky optional; consider content density
- Sub-headers with toolbars: Sticky when table/list below has many rows

---

## Common Patterns

### **Notification Badge on Bell**
```
🔔 (with red dot or count overlay)
aria-label="Notifications, 5 unread"
```

### **User Avatar Dropdown**
```
[Avatar] ▾ → [Profile name + email] → [Divider] → [Profile] [Settings] [Sign out]
```

### **Breadcrumb Sub-Header**
```
[Home] › [Projects] › [Q4 Campaign]   ← breadcrumb row
[H1: Q4 Campaign] [Badge: Active]     [Export] [New Task]  ← page title row
```

### **Priority+ Nav (Tablet)**
```
[Logo] [Dashboard] [Projects] [Team] [More ▾] [Actions]
                                      → Settings
                                      → Billing
                                      → Help
```

---

## Related Components

- **Sidebar**: Full-height left navigation for deep hierarchies; complements or replaces header nav
- **Breadcrumb**: Trail of ancestor pages; used in page header sub-zone
- **Avatar**: User representation in header end zone
- **Dropdown / Menu**: Used for nav item dropdowns and user menu
- **Drawer**: Mobile navigation slide-out panel
- **Search**: Inline or modal search triggered from header
- **Badge**: Notification count overlaid on bell icon
- **Button**: CTA buttons in header end zone

---

## Version History

- **v1.0**: App header with logo, nav, avatar, sticky behavior
- **v1.1**: Marketing variant, transparent-to-opaque scroll, mega-menu support
- **v1.2**: Page header variant with breadcrumbs and page title
- **v2.0**: Contextual/section header, minimal variant, dark background, full token system
- **v2.1**: Mobile drawer integration, skip link, priority+ nav pattern, full ARIA audit
- **v2.2**: Sub-header zone, notification bell, improved keyboard nav for dropdowns

---

## Testing Checklist

- [ ] Skip link is first focusable element; appears on focus
- [ ] Skip link target (`#main-content`) exists and focus jumps correctly
- [ ] `<header>` element used with `role="banner"` (implicit) on global header
- [ ] `<nav aria-label="Main navigation">` wraps nav items
- [ ] Logo is a link with meaningful `aria-label`
- [ ] `aria-current="page"` set on active nav item
- [ ] Dropdown trigger has `aria-haspopup` and `aria-expanded`
- [ ] Escape closes dropdown and returns focus to trigger
- [ ] Arrow keys navigate open dropdown
- [ ] Notification bell `aria-label` includes unread count
- [ ] User menu is accessible via keyboard (Enter opens, arrow keys navigate, Escape closes)
- [ ] Mobile hamburger toggles `aria-expanded` and `aria-label`
- [ ] Mobile drawer focus trap works correctly
- [ ] Sticky header does not overlap page content (correct top padding on main)
- [ ] Scroll → opaque animation works on marketing variant
- [ ] Dropdown animations smooth at 60fps
- [ ] Reduced motion disables all animations
- [ ] Header renders correctly at mobile, tablet, and desktop breakpoints
- [ ] Nav overflow hides items at tablet breakpoint
- [ ] Dark variant maintains all contrast ratios
- [ ] Touch targets all ≥ 44×44px
