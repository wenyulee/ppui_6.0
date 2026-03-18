# Banner Component Documentation

## Description

The Banner component is a large, prominent message container that displays important site-wide announcements, alerts, or notifications. Banners are typically displayed at the top of the page (or other prime location) and remain visible while the user interacts with the page content. They're used for system messages, maintenance notices, promotions, and urgent information that requires user awareness.

**When to use:**
- Site-wide announcements or maintenance messages
- Critical system alerts (downtime, security warnings)
- Time-sensitive promotions or offers
- Important policy or feature announcements
- Compliance messages (cookie notices, GDPR)
- Status updates (service degradation, incidents)
- Global context changes (environment, locale, workspace)

**When NOT to use:**
- Contextual feedback for specific form fields (use inline validation)
- Small notifications (use Toast instead)
- Information that can be placed in sidebars or modals
- Marketing messages that clutter the interface
- Multiple banners at once (limit to 1-2 maximum)

---

## Component Structure

```
Banner
├── BannerContainer (Full-width wrapper)
├── BannerContent
│   ├── BannerIcon (Leading icon)
│   ├── BannerText (Message content)
│   │   ├── BannerTitle (Heading)
│   │   └── BannerDescription (Additional details)
│   └── BannerAction (CTA or action button)
└── BannerClose (Optional dismiss button)
```

---

## Banner Types & Variants

### Type 1: Alert Banner
**Usage:** Important warnings, errors, or critical information

```
⚠️ System Maintenance
Our service will be unavailable on Saturday from 2-4 PM. [Learn More] ✕
```

- **Icons:** Warning, Error, Info, or custom
- **Colors:** Red (error), Yellow (warning), Blue (info)
- **Action:** Usually a link for more details
- **Dismissible:** Optional, respects user choice
- **Use cases:** Downtime notices, security alerts, system issues

### Type 2: Success Banner
**Usage:** Confirmation of completed actions or positive updates

```
✓ Changes Saved
Your settings have been updated successfully. [View Changes]
```

- **Icon:** Checkmark or success icon
- **Color:** Green (#10B981)
- **Duration:** Can auto-dismiss after 5-10 seconds
- **Action:** Usually a confirmation or view link
- **Tone:** Positive and reassuring

### Type 3: Info Banner
**Usage:** Informational announcements and updates

```
ℹ️ New Feature Available
Check out our improved dashboard. [Try Now] ✕
```

- **Icon:** Info, Lightbulb, or announcement icon
- **Color:** Blue (#3B82F6)
- **Action:** Usually a CTA button or link
- **Dismissible:** Yes, user can hide
- **Use cases:** Feature announcements, tips, education

### Type 4: Promotion Banner
**Usage:** Marketing announcements and special offers

```
🎉 Limited Time Offer
Get 20% off your first purchase. Use code SAVE20 [Shop Now] ✕
```

- **Icon:** Celebration, gift, or offer icon
- **Color:** Brand color or vibrant accent
- **Action:** Clear CTA button
- **Dismissible:** Yes
- **Duration:** Appears for full session or dismissable

### Type 5: Sticky Banner
**Usage:** Persistent banners that remain visible while scrolling

```
📢 Important Update - Read Now [Close]
(Remains visible at top while user scrolls)
```

- **Positioning:** Fixed or sticky to top/bottom
- **Style:** Minimal to avoid obstruction
- **Action:** Usually a button or link
- **Close:** Easy dismiss with minimal visual weight
- **Use cases:** Urgent announcements, support notices

---

## Size Variants

| Size | Height | Padding | Text Size | Icon Size | Use Case |
|------|--------|---------|-----------|-----------|----------|
| **Compact** | 48px | 12px 16px | 14px | 20px | Secondary messages |
| **Default** | 64px | 16px 20px | 16px | 24px | Standard announcements |
| **Large** | 80px | 20px 24px | 16px + 14px desc | 28px | Important updates |
| **Full** | 96px+ | 24px 32px | 18px + 16px desc | 32px | Critical announcements |

---

## Color Variants (by Semantic Type)

| Type | Background | Border | Icon/Text | Use Case |
|------|-----------|--------|-----------|----------|
| **Error** | #FEE2E2 | #FCA5A5 | #DC2626 | Critical warnings, errors |
| **Warning** | #FEF3C7 | #FCD34D | #D97706 | Caution, pending changes |
| **Success** | #DCFCE7 | #86EFAC | #16A34A | Confirmation, positive |
| **Info** | #DBEAFE | #93C5FD | #2563EB | Information, updates |
| **Neutral** | #F3F4F6 | #D1D5DB | #6B7280 | General announcements |
| **Brand** | Brand 10% | Brand 30% | Brand 100% | Promotions, features |

---

## States

| State | Visual | Interaction |
|-------|--------|-------------|
| **Default** | Full visibility | User can interact with content below |
| **Hover** | Subtle shadow or opacity change | Close button visible |
| **Dismissing** | Fade out / slide away | Animation while removing |
| **Dismissed** | Hidden (remembers choice) | LocalStorage or cookie |
| **Sticky/Fixed** | Always visible, top/bottom | Remains on scroll |
| **Loading** | Content with spinner | Awaiting confirmation |
| **With Action** | Action button/link highlighted | Hover/focus effects |

---

## Props / Properties

### Banner Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `variant` | `'alert' \| 'success' \| 'info' \| 'promotion' \| 'sticky'` | `'info'` | Banner type |
| `type` | `'error' \| 'warning' \| 'success' \| 'info' \| 'neutral'` | `'info'` | Semantic type for colors |
| `title` | `string` | — | Banner heading text |
| `description` | `string` | — | Additional details (optional) |
| `icon` | `ReactNode` | Auto (by type) | Leading icon |
| `action` | `BannerAction` | — | Action button/link config |
| `dismissible` | `boolean` | `true` | Show close button |
| `onDismiss` | `() => void` | — | Callback on close |
| `rememberDismissal` | `boolean` | `false` | Remember if user dismissed |
| `dismissalId` | `string` | — | Unique ID for dismissal storage |
| `sticky` | `boolean` | `false` | Sticky/fixed positioning |
| `position` | `'top' \| 'bottom'` | `'top'` | Sticky position |
| `autoClose` | `boolean \| number` | `false` | Auto-close after ms (false = no close) |
| `size` | `'compact' \| 'default' \| 'large' \| 'full'` | `'default'` | Banner size |
| `fullWidth` | `boolean` | `true` | Stretch to full viewport width |
| `maxWidth` | `string` | — | Max content width |
| `className` | `string` | — | Custom CSS class |

### BannerAction Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `label` | `string` | — | Button or link text |
| `href` | `string` | — | URL for link-type action |
| `onClick` | `() => void` | — | Callback for button action |
| `external` | `boolean` | `false` | Open in new tab (for links) |
| `icon` | `ReactNode` | — | Optional icon on action |

---

## Accessibility

### ARIA Attributes

```jsx
// Alert Banner
<div
  role="alert"
  aria-live="assertive"            // Announce immediately
  aria-atomic="true"               // Announce entire content
  aria-labelledby="banner-title"   // Link to title
>
  <h2 id="banner-title">System Maintenance</h2>
  <p>Service unavailable Saturday 2-4 PM</p>
</div>

// Info/Status Banner
<div
  role="status"
  aria-live="polite"               // Announce when convenient
  aria-atomic="true"
>
  <p>Your changes have been saved</p>
</div>

// With Action
<div role="alert" aria-labelledby="banner-title">
  <h2 id="banner-title">New Feature Available</h2>
  <p>Check out our improved dashboard</p>
  <a href="/dashboard" aria-label="Try the new dashboard">
    Try Now
  </a>
</div>
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Focus on action button or close button |
| **Enter / Space** | Activate action or close |
| **Escape** | Dismiss banner (if dismissible) |

### Screen Reader Announcement

- Alert banner: `"Alert: [Title]. [Description]. [Action]"`
- Success banner: `"Success: [Message]. [Action]"`
- Info banner: `"Information: [Message]. [Action]"`
- Auto-dismiss: Announce closure via aria-live polite

---

## Code Examples

### Basic Alert Banner

```jsx
import { Banner } from '@/components/Banner'

export function MaintenanceBanner() {
  return (
    <Banner
      type="warning"
      title="Scheduled Maintenance"
      description="Our service will be unavailable on Saturday from 2-4 PM for updates."
      action={{
        label: 'Learn More',
        href: '/maintenance-schedule'
      }}
      dismissible
    />
  )
}
```

### Success Banner

```jsx
import { Banner } from '@/components/Banner'
import { CheckCircleIcon } from '@/icons'

export function SuccessBanner() {
  return (
    <Banner
      type="success"
      title="Changes Saved"
      icon={<CheckCircleIcon />}
      autoClose={5000}  // Auto-dismiss after 5 seconds
      dismissible
    />
  )
}
```

### Error Alert with Action

```jsx
import { Banner } from '@/components/Banner'

export function ErrorBanner({ onRetry }) {
  return (
    <Banner
      type="error"
      title="Upload Failed"
      description="The file could not be processed. Please try again."
      action={{
        label: 'Retry Upload',
        onClick: onRetry
      }}
      dismissible
    />
  )
}
```

### Sticky/Fixed Banner

```jsx
import { Banner } from '@/components/Banner'

export function AnnouncementBanner() {
  return (
    <Banner
      variant="sticky"
      position="top"
      type="info"
      title="🎉 New Feature: Dark Mode"
      description="Try our new dark theme in settings"
      action={{
        label: 'Enable',
        onClick: () => enableDarkMode()
      }}
      sticky
      size="compact"
      rememberDismissal
      dismissalId="dark-mode-announcement"
    />
  )
}
```

### Promotion Banner

```jsx
import { Banner } from '@/components/Banner'

export function PromotionBanner() {
  return (
    <Banner
      type="info"
      title="Limited Time Offer: 20% Off"
      description="Use code SAVE20 at checkout. Offer expires Sunday."
      action={{
        label: 'Shop Now',
        href: '/shop'
      }}
      dismissible
      rememberDismissal
      dismissalId="promo-2026-march"
    />
  )
}
```

### Dismissal with Storage

```jsx
import { Banner } from '@/components/Banner'
import { useState, useEffect } from 'react'

export function SmartBanner() {
  const [showBanner, setShowBanner] = useState(true)
  const bannerId = 'security-update'

  useEffect(() => {
    // Check if user dismissed this banner before
    const isDismissed = localStorage.getItem(`banner-${bannerId}`) === 'true'
    setShowBanner(!isDismissed)
  }, [])

  const handleDismiss = () => {
    localStorage.setItem(`banner-${bannerId}`, 'true')
    setShowBanner(false)
  }

  if (!showBanner) return null

  return (
    <Banner
      type="warning"
      title="Security Update Available"
      description="Please update to the latest version for security patches."
      dismissible
      onDismiss={handleDismiss}
      dismissalId={bannerId}
    />
  )
}
```

### Multiple Banners (Stacked)

```jsx
export function PageWithBanners() {
  return (
    <div>
      {/* Critical/Error banners first */}
      <Banner
        type="error"
        title="Service Unavailable"
        description="Partial outage detected. We're working on it."
      />

      {/* Info banners second */}
      <Banner
        type="info"
        title="Maintenance Scheduled"
        description="Saturday 2-4 PM maintenance window."
      />

      {/* Page content */}
      <main>{/* ... */}</main>
    </div>
  )
}
```

### Custom Styled Banner

```jsx
import { Banner } from '@/components/Banner'

export function CustomBanner() {
  return (
    <Banner
      type="info"
      title="Welcome Back!"
      description="You haven't visited in 30 days. Check out what's new."
      action={{
        label: 'See Highlights',
        onClick: () => showHighlights()
      }}
      size="large"
      dismissible
      className="border-2 border-blue-500"
    />
  )
}
```

---

## Do's and Don'ts

### ✅ Do

- Use semantic types to convey meaning (red = error, green = success)
- Keep banners concise and scannable (2-3 lines maximum)
- Include a clear action or next step
- Use role="alert" for critical messages
- Make close buttons obvious but not obtrusive
- Limit to 1-2 banners maximum on a page
- Use appropriate aria-live regions (assertive for alerts, polite for info)
- Test dismissal persistence with localStorage if needed
- Ensure banners are keyboard navigable
- Use icons to reinforce the message type

### ❌ Don't

- Don't stack multiple banners regularly
- Don't hide critical information in banners only
- Don't use banners for form validation (use inline messages)
- Don't auto-dismiss critical alerts without user action
- Don't forget to test with screen readers
- Don't use banners for debugging messages in production
- Don't make close buttons hard to find or click
- Don't use color alone to convey meaning (add icons/text)
- Don't create banners that block primary content
- Don't forget to provide alternatives for auto-dismissing content

---

## Animation Specifications

### Entrance Animation
- **Duration:** 300ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Animation:**
  - Slide down: translateY(-100%) → 0
  - Opacity: 0 → 1

### Dismissal Animation
- **Duration:** 200ms
- **Easing:** ease-out
- **Animation:**
  - Slide up: translateY(0) → -100%
  - Opacity: 1 → 0

### Auto-Close Animation (if applicable)
- **Duration:** 300ms
- **Easing:** ease-out
- **Progression:**
  - Hold at full opacity for configured duration
  - Then fade out and slide away

---

## Design Tokens Used

- **Colors:**
  - Error background: `#FEE2E2`
  - Error border: `#FCA5A5`
  - Error text: `#DC2626`
  - Warning background: `#FEF3C7`
  - Warning border: `#FCD34D`
  - Warning text: `#D97706`
  - Success background: `#DCFCE7`
  - Success border: `#86EFAC`
  - Success text: `#16A34A`
  - Info background: `#DBEAFE`
  - Info border: `#93C5FD`
  - Info text: `#2563EB`

- **Typography:**
  - Title: `typography.body.bold` (16-18px)
  - Description: `typography.body.regular` (14px)
  - Action: `typography.button.medium`

- **Spacing:**
  - Horizontal padding: 16-32px
  - Vertical padding: 12-24px
  - Icon gap: 12px
  - Content gap: 8px

- **Borders:**
  - Width: 1px (left/top)
  - Radius: 0px (full-width) or 6px (contained)

- **Shadows:**
  - Default: Subtle shadow for elevation
  - Sticky: Stronger shadow for prominence

---

## Related Components

- **Alert** — Inline validation and field-specific feedback
- **Toast** — Temporary notifications (toast pop-ups)
- **Modal** — For confirmation or detailed messages
- **Badge** — For status indicators
- **Button** — For call-to-action within banner
- **Icon** — For visual reinforcement

---

## Use Case Examples

### E-Commerce
- Promotion/sale announcements
- Low stock warnings
- Shipping delay notices
- Order status updates

### SaaS Products
- Feature announcements
- Maintenance windows
- Free trial countdown
- Upgrade prompts
- Security notices

### Admin Dashboards
- System status alerts
- Permission/access warnings
- Data sync notifications
- Action required notices

### Content Platforms
- Content moderation notices
- Policy change announcements
- Spam/abuse warnings
- Publishing status updates

### Financial Platforms
- Market alerts
- Account security notices
- Rate changes
- Transaction confirmations

---

## Best Practices

1. **Hierarchy:** Use banners only for truly important messages
2. **Frequency:** Avoid banner fatigue by limiting quantity
3. **Duration:** Don't auto-dismiss critical alerts; let user acknowledge
4. **Accessibility:** Always test with screen reader
5. **Persistence:** Remember dismissals with appropriate storage
6. **Content:** Keep messages short and actionable
7. **Placement:** Top-of-page is most common, bottom for non-blocking
8. **Styling:** Maintain visual hierarchy with appropriate size/color
9. **Action:** Always provide a clear next step or CTA
10. **Testing:** Test keyboard navigation and screen reader announcement

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
**Banner Types Documented:** 5 variants (Alert, Success, Info, Promotion, Sticky)
