# Link Component Documentation

## Overview

The Link component is the foundational navigation and reference element used wherever text or content needs to direct a user to another location — whether within the application, to an external resource, or to trigger a navigation action. Links are semantically distinct from buttons: links go places, buttons do things. The Link component wraps the native `<a>` element with consistent visual treatment, accessible external link handling, router integration, and support for icon embellishment, color variants, and underline styles. Used correctly, links create a predictable, accessible navigational experience that meets WCAG requirements and works seamlessly across keyboard, pointer, and assistive technology contexts.

---

## Component Variants

Link supports five primary usage patterns:

### 1. **Inline Link**
- **Purpose**: Hyperlink embedded within body text or paragraph content
- **Use Cases**: Article references, documentation cross-links, legal text, form helper text
- **Behavior**: Underlined by default; opens same tab (internal) or new tab (external)
- **Visual**: Matches surrounding text size; brand color; underline on hover or always
- **Example**: "Read our [Privacy Policy] for more details."

### 2. **Standalone Link**
- **Purpose**: Link that stands alone outside of running text — used as a navigation action
- **Use Cases**: "Forgot password?", "Back to dashboard", "View all results", footer links
- **Behavior**: No surrounding text context; underline on hover; may include leading/trailing icon
- **Visual**: Slightly heavier treatment than inline; clear hover state

### 3. **Navigation Link**
- **Purpose**: Link used in navigation contexts — header nav, sidebar, breadcrumbs, tabs
- **Use Cases**: Main nav items, sidebar menu items, breadcrumb trail, tab bar
- **Behavior**: Active state for current route; may suppress underline in favor of color/weight change
- **Visual**: No underline by default in nav contexts; active state uses color or weight

### 4. **External Link**
- **Purpose**: Link pointing to a domain outside the application
- **Use Cases**: Documentation references, third-party tools, social profiles, legal documents
- **Behavior**: Opens in new tab; includes external indicator icon; screen reader announcement
- **Visual**: External icon (↗) trailing the link text; optional visual distinction
- **Required**: `target="_blank"` + `rel="noopener noreferrer"` + accessible new-tab announcement

### 5. **Button Link (Link as Action)**
- **Purpose**: Visually styled like a link but triggering an in-page action rather than navigation
- **Use Cases**: "Show more", "Clear filters", "Undo", "Resend email"
- **Behavior**: `<button>` element styled like a link; activates on both Enter and Space
- **Visual**: Matches link styling exactly
- **Note**: Use `<button>` not `<a>` — no `href` means no navigation; use button semantics

---

## Size Variants

Link size inherits from surrounding text by default (using `currentColor` and `em` units), but can be set explicitly:

| Size | Font Size | Use Case |
|------|-----------|---------|
| **XS** | 12px | Caption links, footnotes, legal fine print |
| **SM** | 13px | Helper text links, form footnotes |
| **MD** | 14px | **Default**, body text, form links |
| **LG** | 16px | Section links, card CTAs |
| **XL** | 18px | Prominent CTAs, feature section links |
| **Inherit** | `inherit` | Matches parent font size (default) |

---

## Style Variants

### **Default**
- Color: Brand (#3B82F6)
- Underline: On hover only (`text-decoration: underline` on `:hover`)
- Visited: Optional distinct visited state (#6D28D9 purple)
- Best For: Inline body text links

### **Always Underlined**
- Color: Brand (#3B82F6)
- Underline: Always visible (`text-decoration: underline`)
- Best For: Legal text, dense documentation, contexts where link affordance must be unambiguous (WCAG 1.4.1)
- Required: For inline links within body text per WCAG when color alone distinguishes links from text

### **No Underline**
- Color: Brand (#3B82F6) or contextual color
- Underline: Never (only color distinguishes from text)
- Best For: Navigation links, footer links, standalone links where context makes clickability obvious
- Caution: Only acceptable when other visual cues (position, context) clearly identify as links

### **Subtle**
- Color: Gray 600 (#4B5563) — muted, not brand blue
- Underline: On hover only
- Best For: Secondary links, metadata links, less important references
- Use When: The link needs to exist but shouldn't compete with primary content

### **Danger**
- Color: Danger (#EF4444)
- Underline: On hover
- Best For: Destructive navigation ("Delete account", "Remove all data")

### **Muted / Disabled**
- Color: Gray 400 (#9CA3AF)
- Underline: None
- Cursor: default or not-allowed
- Pointer Events: None
- Use When: Link target is temporarily unavailable

---

## Color Variants

| Variant | Color | Hover Color | Use Case |
|---------|-------|-------------|---------|
| **Primary** | #3B82F6 | #1D4ED8 | Default navigation and reference links |
| **Secondary** | #6B7280 | #374151 | Subtle secondary links |
| **Success** | #10B981 | #065F46 | Confirmation links, positive CTAs |
| **Warning** | #F59E0B | #92400E | Caution links |
| **Danger** | #EF4444 | #991B1B | Destructive navigation |
| **Inverse** | #FFFFFF | #E5E7EB | Links on dark backgrounds |
| **Inherit** | currentColor | (darkened) | Matches surrounding text color |

---

## State Specifications

### **Default State**
- Color: Brand (#3B82F6)
- Underline: Per variant (hover-only or always)
- Cursor: pointer
- Opacity: 100%

### **Hover State**
- Color: Darkened brand (#1D4ED8)
- Underline: Always visible on hover (even for hover-only variant)
- Duration: 100ms ease-out

### **Active / Pressed State**
- Color: Darker still (#1E40AF)
- Underline: Visible
- Duration: 50ms ease-in

### **Focus State** (Keyboard)
- Outline: 2px solid brand, 2px offset
- Background: Optional very light brand tint for visibility
- Duration: 100ms ease-out
- `:focus-visible` only — not on mouse click

### **Visited State** (Optional)
- Color: Purple (#7C3AED) — follows browser convention
- Underline: Matches base variant
- Application: Only for links to distinct content pages (not UI actions)
- Opt-in: Disabled by default in app contexts; enabled for documentation/article links

### **Disabled State**
- Color: Gray 400 (#9CA3AF)
- Cursor: default or not-allowed
- Pointer Events: none
- `aria-disabled="true"` on the element
- Underline: None

---

## Props API

```typescript
interface LinkProps {
  // Destination
  href?: string;                               // URL destination (omit for ButtonLink)
  to?: string;                                 // Router-based path (React Router / Next.js)

  // Behavior
  target?: '_blank' | '_self' | '_parent' | '_top';
  rel?: string;                                // Relationship; auto-sets for external
  replace?: boolean;                           // Replace history entry (router links)
  download?: boolean | string;                 // Trigger download; optional filename

  // Appearance
  variant?: 'default' | 'underlined' | 'no-underline' | 'subtle' | 'danger' | 'muted';
  color?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'inverse' | 'inherit';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl' | 'inherit';
  weight?: 'normal' | 'medium' | 'semibold';  // Font weight override

  // Icons
  leadingIcon?: React.ReactNode;
  trailingIcon?: React.ReactNode;
  external?: boolean;                          // Auto-adds external icon + new tab behavior
  showExternalIcon?: boolean;                  // Show/hide external icon independently

  // States
  disabled?: boolean;
  active?: boolean;                            // Current/active nav link override

  // Router Integration
  exact?: boolean;                             // Exact path match for active state

  // Accessibility
  'aria-label'?: string;                       // Override for icon-only or ambiguous links
  'aria-current'?: 'page' | 'step' | 'location' | 'date' | 'time' | 'true' | 'false';
  'aria-describedby'?: string;

  // Events
  onClick?: (event: React.MouseEvent<HTMLAnchorElement>) => void;
  onFocus?: (event: React.FocusEvent<HTMLAnchorElement>) => void;
  onBlur?: (event: React.FocusEvent<HTMLAnchorElement>) => void;

  // HTML pass-through
  id?: string;
  title?: string;
  tabIndex?: number;

  // Styling
  className?: string;
  style?: React.CSSProperties;

  // Children
  children: React.ReactNode;
}
```

---

## Code Examples

### **1. Basic Inline Link**
```typescript
import { Link } from '@components/link';

export function InlineExample() {
  return (
    <p className="text-sm text-gray-600">
      By creating an account, you agree to our{' '}
      <Link href="/terms">Terms of Service</Link>
      {' '}and{' '}
      <Link href="/privacy">Privacy Policy</Link>.
    </p>
  );
}
```

### **2. External Link**
```typescript
import { Link } from '@components/link';

export function ExternalLinks() {
  return (
    <div className="space-y-2">
      {/* Auto-handles new tab + external icon + rel + aria */}
      <Link href="https://docs.example.com" external>
        View Documentation
      </Link>

      {/* Manual equivalent */}
      <Link
        href="https://status.example.com"
        target="_blank"
        rel="noopener noreferrer"
        showExternalIcon
        aria-label="System status (opens in new tab)"
      >
        System Status
      </Link>
    </div>
  );
}
```

### **3. Standalone Navigation Link**
```typescript
import { Link } from '@components/link';
import { ArrowLeft } from 'lucide-react';

export function BackLink() {
  return (
    <Link
      href="/dashboard"
      variant="subtle"
      leadingIcon={<ArrowLeft size={14} />}
    >
      Back to Dashboard
    </Link>
  );
}
```

### **4. Link with Router Integration (Next.js)**
```typescript
import { Link } from '@components/link';
import { usePathname } from 'next/navigation';

const navItems = [
  { href: '/dashboard', label: 'Dashboard' },
  { href: '/projects', label: 'Projects' },
  { href: '/settings', label: 'Settings' },
];

export function SidebarNav() {
  const pathname = usePathname();

  return (
    <nav aria-label="Sidebar navigation">
      <ul className="space-y-1">
        {navItems.map((item) => (
          <li key={item.href}>
            <Link
              href={item.href}
              variant="no-underline"
              active={pathname.startsWith(item.href)}
              aria-current={pathname.startsWith(item.href) ? 'page' : undefined}
            >
              {item.label}
            </Link>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

### **5. Button Link (Action, No Navigation)**
```typescript
import { Link } from '@components/link';

export function ActionLinks() {
  const [showAll, setShowAll] = useState(false);

  return (
    <div className="space-y-2">
      {/* Uses <button> under the hood — no href */}
      <Link as="button" onClick={() => setShowAll(!showAll)}>
        {showAll ? 'Show less' : 'Show all results'}
      </Link>

      <Link as="button" onClick={() => clearFilters()} variant="subtle">
        Clear all filters
      </Link>

      <Link as="button" onClick={() => undoLastAction()} color="secondary">
        Undo
      </Link>
    </div>
  );
}
```

### **6. Link with Icon**
```typescript
import { Link } from '@components/link';
import { ExternalLink, Download, ArrowRight } from 'lucide-react';

export function IconLinks() {
  return (
    <div className="space-y-3">
      <Link
        href="/reports/q4-2025.pdf"
        download="Q4-2025-Report.pdf"
        trailingIcon={<Download size={14} />}
      >
        Download Q4 Report
      </Link>

      <Link
        href="/features"
        trailingIcon={<ArrowRight size={14} />}
        variant="no-underline"
        weight="medium"
      >
        See all features
      </Link>
    </div>
  );
}
```

### **7. Danger Link**
```typescript
import { Link } from '@components/link';

export function DangerLinks() {
  return (
    <p className="text-sm text-gray-600">
      This action cannot be undone.{' '}
      <Link href="/account/delete" color="danger" variant="underlined">
        Delete my account permanently
      </Link>
    </p>
  );
}
```

### **8. Disabled Link**
```typescript
import { Link } from '@components/link';

export function DisabledLink({ canExport }: { canExport: boolean }) {
  return (
    <Link
      href="/export"
      disabled={!canExport}
      aria-label={
        canExport
          ? 'Export data'
          : 'Export unavailable — upgrade your plan to export data'
      }
    >
      Export data
    </Link>
  );
}
```

### **9. Link List (Footer / Nav)**
```typescript
import { Link } from '@components/link';

const footerLinks = [
  { href: '/about', label: 'About' },
  { href: '/blog', label: 'Blog' },
  { href: '/careers', label: 'Careers' },
  { href: '/contact', label: 'Contact' },
  { href: '/privacy', label: 'Privacy Policy' },
  { href: '/terms', label: 'Terms' },
];

export function FooterLinks() {
  return (
    <nav aria-label="Footer navigation">
      <ul className="flex flex-wrap gap-x-6 gap-y-2">
        {footerLinks.map((link) => (
          <li key={link.href}>
            <Link href={link.href} variant="subtle" size="sm">
              {link.label}
            </Link>
          </li>
        ))}
      </ul>
    </nav>
  );
}
```

### **10. Breadcrumb Links**
```typescript
import { Link } from '@components/link';
import { ChevronRight } from 'lucide-react';

interface Crumb {
  label: string;
  href?: string;
}

export function Breadcrumb({ crumbs }: { crumbs: Crumb[] }) {
  return (
    <nav aria-label="Breadcrumb">
      <ol className="flex items-center gap-1 text-sm">
        {crumbs.map((crumb, index) => {
          const isLast = index === crumbs.length - 1;
          return (
            <li key={crumb.href ?? crumb.label} className="flex items-center gap-1">
              {isLast ? (
                <span aria-current="page" className="text-gray-900 font-medium">
                  {crumb.label}
                </span>
              ) : (
                <>
                  <Link href={crumb.href!} variant="subtle">
                    {crumb.label}
                  </Link>
                  <ChevronRight size={14} className="text-gray-400" aria-hidden="true" />
                </>
              )}
            </li>
          );
        })}
      </ol>
    </nav>
  );
}
```

---

## Accessibility Specifications

### **Links vs. Buttons — The Core Rule**

| Element | Use When | Activated By |
|---------|----------|-------------|
| `<a href="...">` | Navigates to a URL | Enter (keyboard), click |
| `<button>` | Performs an in-page action | Enter + Space (keyboard), click |

Never use `<a>` without `href` as a button substitute — it loses keyboard Space activation and screen reader semantics. Never use `<button>` when navigation is the intent — it won't work as a link in new tab, right-click save, etc.

### **External Link Requirements**
Every external link (opens in new tab) must:
1. Set `target="_blank"` + `rel="noopener noreferrer"`
2. Inform the user it opens in a new tab — either:
   - Via `aria-label`: `aria-label="View Documentation (opens in new tab)"`
   - Via visually-hidden text: `<span class="sr-only">(opens in new tab)</span>`
   - Via visible icon with its own accessible label
3. Show an external icon (`↗`) as a visual hint

The `external` prop handles all of the above automatically.

```tsx
// ✅ All requirements met automatically
<Link href="https://docs.example.com" external>Documentation</Link>

// ✅ Manual equivalent
<a
  href="https://docs.example.com"
  target="_blank"
  rel="noopener noreferrer"
  aria-label="Documentation (opens in new tab)"
>
  Documentation
  <span aria-hidden="true"> ↗</span>
</a>
```

### **WCAG 1.4.1 — Color Not Sole Differentiator**
Links within body text must be distinguishable from surrounding text by more than color alone:
- Underline (recommended for inline body text links)
- Bold weight
- Background highlight on hover
- Or: sufficient contrast ratio between link color and surrounding text (3:1 minimum)

```tsx
// ✅ Inline link: always underlined (safest)
<Link href="/privacy" variant="underlined">Privacy Policy</Link>

// ✅ Acceptable: hover underline + brand color has sufficient contrast against gray body text
<Link href="/privacy">Privacy Policy</Link>  {/* blue on gray-700 = sufficient */}

// ❌ Risk: no underline + link color too close to body text color
<Link href="/privacy" variant="no-underline" color="inherit">Privacy Policy</Link>
```

### **WCAG 2.4.4 — Link Purpose (In Context)**
Link text must describe the destination or purpose, either standalone or in context:
```tsx
// ✅ Descriptive standalone
<Link href="/reports/q4">View Q4 Report</Link>

// ✅ Descriptive in context
<p>
  The Q4 report is now available.{' '}
  <Link href="/reports/q4">Read the full report</Link>
</p>

// ❌ Ambiguous standalone
<Link href="/reports/q4">Click here</Link>
<Link href="/reports/q4">Read more</Link>
<Link href="/reports/q4">Learn more</Link>
```

When link text must be brief for design reasons, supplement with `aria-label`:
```tsx
// ✅ Short text with descriptive aria-label
<Link href="/reports/q4" aria-label="Read the full Q4 2025 report">Read more</Link>
```

### **Active / Current Page**
```tsx
// ✅ Marks current page in navigation
<Link href="/dashboard" aria-current="page">Dashboard</Link>
// aria-current="page" announced: "Dashboard, current page, link"
```

### **Keyboard Navigation**
- **Tab**: Move focus to link
- **Shift+Tab**: Move focus to previous element
- **Enter**: Follow link / activate button-link
- **Space**: Activate button-link only (not `<a>` elements — this is intentional browser behavior)

### **Focus Visibility**
- Focus outline: 2px solid brand, 2px offset — never suppressed
- Uses `:focus-visible` to avoid focus ring on mouse clicks while maintaining keyboard visibility
- Focus ring must contrast 3:1 against adjacent colors (WCAG 1.4.11)

### **Download Links**
```tsx
<Link href="/report.pdf" download="Q4-Report.pdf">
  Download Q4 Report (PDF)
</Link>
// Announce file format and type in text when possible
// aria-label="Download Q4 2025 Annual Report, PDF, 2.3MB" for screen readers
```

### **Screen Reader Announcements**
| Scenario | Announcement |
|----------|-------------|
| Basic link | "Privacy Policy, link" |
| External link | "Documentation, link" + "opens in new tab" (from sr-only or aria-label) |
| Active nav link | "Dashboard, current page, link" |
| Disabled link | "Export data, link, dimmed" (or per screen reader) |
| Download link | "Download Q4 Report, link" |
| Button-link | "Show all results, button" |

---

## Animation Specifications

### **Hover Color Transition**
- **Target**: `color`
- **Duration**: 100ms ease-out
  ```css
  a { transition: color 100ms ease-out, text-decoration-color 100ms ease-out; }
  ```

### **Underline Appearance (Hover-only variant)**
- **Target**: `text-decoration`
- **Duration**: Instant (browsers handle this natively; animation optional)
- **Optional enhanced**: Use `text-underline-offset` + opacity for smoother effect
  ```css
  a {
    text-decoration: underline;
    text-underline-offset: 2px;
    text-decoration-color: transparent;
    transition: text-decoration-color 100ms ease-out;
  }
  a:hover { text-decoration-color: currentColor; }
  ```

### **Focus Ring**
- **Target**: outline, outline-offset
- **Duration**: 100ms ease-out

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  a { transition: none; }
}
```

---

## Design Tokens

### **Colors**
| State | Primary | Secondary | Danger |
|-------|---------|-----------|--------|
| Default | #3B82F6 | #6B7280 | #EF4444 |
| Hover | #1D4ED8 | #374151 | #991B1B |
| Active/Pressed | #1E40AF | #1F2937 | #7F1D1D |
| Visited | #7C3AED | — | — |
| Disabled | #9CA3AF | #9CA3AF | #9CA3AF |
| Inverse | #FFFFFF | #E5E7EB | #FCA5A5 |

### **Typography**
| Property | Value |
|----------|-------|
| Default weight | Inherit (400) |
| Medium weight | 500 |
| Semibold weight | 600 |
| Text decoration | underline (always/hover per variant) |
| Underline offset | 2px |
| Underline thickness | 1px |

### **Focus Ring**
| Property | Value |
|----------|-------|
| Outline | 2px solid #3B82F6 |
| Outline offset | 2px |
| Border radius | 2px |

### **External Icon**
| Property | Value |
|----------|-------|
| Icon | `external-link` (↗) |
| Size | 12px (inline), 14px (standalone) |
| Gap from text | 4px |
| Vertical alignment | middle |

---

## Best Practices

### **Link Text**
- Be specific and descriptive: "Read the Q4 report" not "Read more"
- Avoid "click here", "here", "this link", "more info" as standalone link text
- Describe the destination, not the action: "Privacy Policy" not "View our privacy policy" (in nav)
- For downloads: include format/size when helpful: "Q4 Report (PDF, 2.3MB)"
- For icon + text: the text carries meaning; icon is decorative

### **When to Use Link vs. Button**
```
Does it navigate to a URL?          → <a href>  (Link)
Does it change the page/history?    → <a href>  (Link)
Can it be opened in a new tab?      → <a href>  (Link)
Does it perform an action (no URL)? → <button>  (ButtonLink)
Does it toggle something?           → <button>  (ButtonLink)
Does it submit a form?              → <button type="submit">
```

### **External Links**
- Always open external links in a new tab (`target="_blank"`)
- Always include `rel="noopener noreferrer"` for security
- Always inform users via visible icon or `aria-label`
- Don't open internal links in new tab unless the user explicitly requests it (like a "Open in new tab" control)

### **Inline vs. Standalone**
- Inline (within paragraph): Always underline or use sufficient contrast; use `variant="underlined"` for maximum clarity
- Standalone (outside text): Underline on hover is sufficient; use `variant="default"`
- Navigation: No underline; use active state and color change

### **Visited State**
- Enable visited state for content navigation (articles, docs) to help users track what they've read
- Disable for UI links (nav, actions) to avoid visual confusion
- Never change visited link behavior in a way that misleads users

### **Avoid**
- Wrapping large blocks of content in a single link (use a card with an explicit CTA instead)
- Using `<a>` for actions that require JavaScript with no valid `href` fallback
- Non-descriptive link text ("click here", "here", "learn more")
- Opening too many links in new tabs — reserve for external content only
- Removing focus outlines (`:focus { outline: none }`) — always provide a visible alternative

---

## Router Integration Notes

### **React Router**
```typescript
// Link component uses React Router's <Link> internally when `to` prop is used
<Link to="/dashboard">Dashboard</Link>

// Equivalent to:
import { Link as RouterLink } from 'react-router-dom';
<RouterLink to="/dashboard">Dashboard</RouterLink>
```

### **Next.js**
```typescript
// Link component wraps Next.js <Link> when `href` is a path
<Link href="/dashboard">Dashboard</Link>

// Equivalent to:
import NextLink from 'next/link';
<NextLink href="/dashboard">Dashboard</NextLink>
```

### **Plain HTML Fallback**
When no router is detected, Link renders a standard `<a href>` element.

---

## Related Components

- **Button**: Use for actions; Link is for navigation
- **Breadcrumb**: Uses Link internally for ancestor path items
- **Navigation / Header**: Uses Link for nav items
- **Sidebar**: Uses Link for menu items
- **Legal Consent**: Uses Link for policy document references
- **Empty State**: Uses Link for secondary actions
- **Text / Typography**: Link inherits from type scale

---

## Version History

- **v1.0**: Basic link with color and hover state; outlined focus
- **v1.1**: External link handling (new tab, rel, icon); disabled state
- **v1.2**: Icon slots, size variants, color variants, button-link (`as="button"`)
- **v2.0**: Router integration (React Router + Next.js), `aria-current`, visited state token
- **v2.1**: `text-decoration-color` animation, underline offset, reduced motion, full a11y audit

---

## Testing Checklist

- [ ] Basic link navigates to `href` on click
- [ ] External link opens in new tab
- [ ] External link has `rel="noopener noreferrer"`
- [ ] External icon visible on external links
- [ ] Screen reader announces "opens in new tab" for external links
- [ ] `aria-current="page"` set on active nav link
- [ ] Focus ring visible on keyboard Tab focus (`:focus-visible`)
- [ ] Focus ring not shown on mouse click
- [ ] Hover changes color (100ms transition)
- [ ] Always-underlined variant shows underline at all times
- [ ] Hover-only variant shows underline only on hover
- [ ] No-underline variant never shows underline
- [ ] Disabled link is not interactive; cursor is default or not-allowed
- [ ] `aria-disabled="true"` on disabled links
- [ ] Button-link (`as="button"`) activates on both Enter AND Space
- [ ] `<a>` element activates only on Enter (not Space) — browser default preserved
- [ ] Download link triggers download (not navigation)
- [ ] Icon + text: icon has `aria-hidden="true"`
- [ ] Link text is descriptive (passes WCAG 2.4.4)
- [ ] Inline links distinguishable from body text (underline or sufficient contrast)
- [ ] Visited state applies to content links (if enabled)
- [ ] Touch target sufficient for standalone links (44px height)
- [ ] All color variants meet contrast ratios (3:1 for links vs background, 4.5:1 for text)
- [ ] Reduced motion disables color transition
- [ ] Router links use router navigation (no full page reload on SPA)
