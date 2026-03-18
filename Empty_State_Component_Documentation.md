# Empty State Component Documentation

## Overview

The Empty State component communicates to users that a view, list, or container has no content to display — and, crucially, guides them toward a resolution. A well-designed empty state transforms a moment of potential confusion or disappointment into a clear, actionable prompt. Empty states appear in data tables, search results, dashboards, inboxes, folders, filtered lists, and any other content area that may legitimately be empty. They are not just placeholders — they are a key touchpoint in the user journey that can reduce abandonment, encourage first-time usage, and set accurate expectations.

---

## Component Variants

Empty State supports six primary usage patterns:

### 1. **No Content (First Use)**
- **Purpose**: Shown when a section has never had any content — inviting the user to create their first item
- **Use Cases**: Empty project list, first-time inbox, no tasks created yet, blank dashboard
- **Tone**: Welcoming, encouraging, action-oriented
- **Visual**: Illustration or icon + upbeat headline + clear CTA button
- **Example**: "No projects yet — Create your first project to get started"

### 2. **No Results (Search / Filter)**
- **Purpose**: Shown when a search query or active filter returns zero results
- **Use Cases**: Search results page, filtered table, tag/category filter with no matches
- **Tone**: Helpful, solution-oriented — suggest clearing filters or adjusting the query
- **Visual**: Search/filter icon + descriptive message + clear filter action
- **Example**: "No results for 'invoices 2023' — Try a different search term or clear filters"

### 3. **Error / Failure State**
- **Purpose**: Shown when content failed to load due to an error
- **Use Cases**: API failure, network timeout, permission error, corrupt data
- **Tone**: Honest, calm, recovery-focused — not alarming
- **Visual**: Error illustration or icon + brief explanation + retry action
- **Example**: "Couldn't load your files — Check your connection and try again"

### 4. **Permissions / Access Restricted**
- **Purpose**: Shown when the user lacks permission to view the content
- **Use Cases**: Private page, role-restricted feature, subscription-gated content
- **Tone**: Clear and matter-of-fact; suggest how to gain access
- **Visual**: Lock or shield icon + explanation + upgrade/request access CTA
- **Example**: "You don't have access to this workspace — Contact your admin or upgrade your plan"

### 5. **Done / Completed State**
- **Purpose**: Shown when all items in a list have been addressed (inbox zero, all tasks done)
- **Use Cases**: Empty inbox, cleared notifications, all tasks completed, approved queue
- **Tone**: Celebratory, positive reinforcement
- **Visual**: Check/celebration illustration + congratulatory headline
- **Example**: "You're all caught up! — No pending notifications"

### 6. **Feature Not Available**
- **Purpose**: Shown when a feature exists but isn't configured or enabled for the user
- **Use Cases**: Integration not connected, feature on another plan, setup required
- **Tone**: Informative, motivating — show the value of enabling the feature
- **Visual**: Feature illustration + explanation + setup/upgrade CTA
- **Example**: "Connect your Slack to get notifications — Integrate in Settings"

---

## Size Variants

| Size | Container | Illustration | Title | Description | Use Case |
|------|-----------|--------------|-------|-------------|---------|
| **XS** | Min 120px | 32px icon | 13px | 12px | Inline widget, small panel, popover |
| **SM** | Min 200px | 48px | 14px | 13px | Sidebar list, compact card |
| **MD** | Min 320px | 64–80px | 16px | 14px | **Default**, standard table/list |
| **LG** | Min 480px | 96–120px | 20px | 15px | Full-page sections, dashboards |
| **XL** | Full page | 160–200px | 24px | 16px | Dedicated empty page, onboarding |

---

## Layout Variants

### **Centered (Default)**
- All content vertically and horizontally centered in its container
- Best For: Tables, lists, full-page views, dashboards
- Stack order: Illustration → Title → Description → Actions

### **Horizontal**
- Illustration on left; text and actions on right
- Best For: Sidebars, compact panels, inline content areas
- Stack order: [Illustration] | [Title + Description + Actions]

### **Minimal**
- Icon + single line of text only; no illustration, no description paragraph
- Best For: Very small containers, widget dropdowns, nested empty contexts
- Stack order: Icon — Message (inline)

### **Full Page**
- Takes up entire viewport; includes large illustration, heading, description, and prominent CTA
- Best For: First-run empty app state, onboarding, dedicated 404/empty pages
- Stack order: Illustration → Eyebrow → Title → Description → Actions → Secondary link

---

## Illustration Options

### **Icon-Only**
- Single semantic icon from the design system (32–96px)
- Appropriate for: Compact views, inline states, system errors
- Colors: Semantic (gray for neutral, red for error, blue for info)

### **Icon with Background**
- Icon centered inside a soft-tinted circle or rounded square
- Sizes: 64×64px (MD), 96×96px (LG)
- Background: 10–15% opacity of semantic color

### **Custom Illustration**
- SVG illustration specific to the context (e.g., empty inbox, no search results)
- Sizes: 120–200px height
- Style: Consistent with brand illustration system; typically line art or flat

### **Animation** (Optional)
- Lottie or CSS animation for delight or to communicate loading→empty transition
- Best For: Done/inbox-zero states (celebratory), first-run (welcoming)
- Duration: 1–2s; not looping by default (plays once on appear)

---

## Content Structure

Every Empty State is composed of these optional elements:

```
┌─────────────────────────────────────┐
│         [Illustration / Icon]        │
│                                      │
│           Headline / Title           │
│                                      │
│    Supporting description text       │
│  (1–2 sentences, helpful guidance)   │
│                                      │
│    [Primary Action Button]           │
│    [Secondary Action / Link]         │
└─────────────────────────────────────┘
```

### **Headline Guidelines**
- Lead with the user's situation, not the system state
- ✅ "No projects yet" / "Nothing here yet" / "You're all caught up"
- ❌ "Empty" / "No data found" / "Null results"
- Length: 3–7 words; punchy and scannable

### **Description Guidelines**
- Explain why it's empty and/or what to do about it
- Max 2 sentences; no technical jargon
- ✅ "Create your first project to start collaborating with your team."
- ❌ "The requested resource collection returned a null payload."

### **Action Guidelines**
- Primary: One clear CTA that resolves the empty state (Create, Connect, Retry, Clear filters)
- Secondary: Optional lower-commitment alternative (Learn more, Browse examples, Skip for now)
- Avoid more than 2 actions — empty states should be simple

---

## State Specifications

### **Default State**
- Background: Inherits container background (transparent)
- Opacity: 100%
- Content: Centered within available space
- Animation: Fade in on mount (200ms ease-out)

### **Loading → Empty Transition**
- Loading skeleton collapses and fades out (150ms)
- Empty state fades in after brief gap (50ms delay, 200ms fade)
- Prevents jarring instant swap

### **Error State Styling**
- Illustration/Icon: Red tint (#FEF2F2 background, #EF4444 icon)
- Title: Gray 900 (not red — avoids alarm)
- Description: Mentions the error and a remedy
- Primary Action: "Try again" / "Retry"
- Secondary Action: "Contact support"

### **Hover State** (If the entire empty state is a link/button)
- Background: Very subtle tint
- Cursor: pointer
- Border: Dashed border on container (optional)

---

## Props API

```typescript
interface EmptyStateProps {
  // Content
  title: string;                               // Headline text
  description?: string | React.ReactNode;     // Supporting description
  illustration?: React.ReactNode | string;    // Illustration, icon, or image src
  illustrationAlt?: string;                   // Alt text for image illustrations

  // Illustration Style
  illustrationVariant?: 'icon' | 'icon-bg' | 'illustration' | 'animation';
  illustrationColor?: 'neutral' | 'primary' | 'success' | 'warning' | 'error';
  illustrationSize?: 'sm' | 'md' | 'lg' | 'xl';

  // Actions
  primaryAction?: {
    label: string;
    onClick?: () => void;
    href?: string;
    icon?: React.ReactNode;
    loading?: boolean;
  };
  secondaryAction?: {
    label: string;
    onClick?: () => void;
    href?: string;
    external?: boolean;
  };

  // Layout
  layout?: 'centered' | 'horizontal' | 'minimal' | 'full-page';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  fullHeight?: boolean;                        // Stretch to fill container height

  // Variants
  type?: 'default' | 'search' | 'error' | 'permissions' | 'done' | 'feature';

  // Accessibility
  aria-label?: string;                         // Describes empty state region
  aria-live?: 'polite' | 'off';               // Announce when state appears
  role?: 'status' | 'region' | 'alert';

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

---

## Code Examples

### **1. First-Use Empty State**
```typescript
import { EmptyState } from '@components/empty-state';
import { FolderOpen } from 'lucide-react';

export function EmptyProjects() {
  return (
    <EmptyState
      illustration={<FolderOpen size={48} />}
      illustrationColor="primary"
      illustrationVariant="icon-bg"
      title="No projects yet"
      description="Create your first project to start collaborating with your team."
      primaryAction={{
        label: 'Create project',
        onClick: () => {},
        icon: <PlusIcon size={16} />,
      }}
      secondaryAction={{
        label: 'Learn about projects',
        href: '/docs/projects',
      }}
      aria-live="polite"
    />
  );
}
```

### **2. No Search Results**
```typescript
import { EmptyState } from '@components/empty-state';
import { SearchX } from 'lucide-react';

export function NoSearchResults({
  query,
  onClear,
}: {
  query: string;
  onClear: () => void;
}) {
  return (
    <EmptyState
      type="search"
      illustration={<SearchX size={48} />}
      illustrationColor="neutral"
      title={`No results for "${query}"`}
      description="Try adjusting your search terms or clearing your filters to find what you're looking for."
      primaryAction={{
        label: 'Clear search',
        onClick: onClear,
      }}
      size="md"
    />
  );
}
```

### **3. Error / Failed to Load**
```typescript
import { EmptyState } from '@components/empty-state';
import { AlertTriangle } from 'lucide-react';

export function LoadError({ onRetry }: { onRetry: () => void }) {
  return (
    <EmptyState
      type="error"
      illustration={<AlertTriangle size={48} />}
      illustrationColor="error"
      illustrationVariant="icon-bg"
      title="Couldn't load your data"
      description="Something went wrong while loading this content. Check your connection and try again."
      primaryAction={{
        label: 'Try again',
        onClick: onRetry,
      }}
      secondaryAction={{
        label: 'Contact support',
        href: 'mailto:support@example.com',
      }}
      role="alert"
    />
  );
}
```

### **4. Inbox Zero / Done State**
```typescript
import { EmptyState } from '@components/empty-state';
import { CheckCircle2 } from 'lucide-react';

export function InboxZero() {
  return (
    <EmptyState
      type="done"
      illustration={<CheckCircle2 size={64} />}
      illustrationColor="success"
      illustrationVariant="icon-bg"
      title="You're all caught up!"
      description="No new notifications. We'll let you know when something needs your attention."
      size="lg"
      aria-live="polite"
    />
  );
}
```

### **5. Permissions / Access Restricted**
```typescript
import { EmptyState } from '@components/empty-state';
import { Lock } from 'lucide-react';

export function AccessRestricted() {
  return (
    <EmptyState
      type="permissions"
      illustration={<Lock size={48} />}
      illustrationColor="neutral"
      illustrationVariant="icon-bg"
      title="You don't have access"
      description="This content is restricted to admins. Contact your workspace owner to request access."
      primaryAction={{
        label: 'Request access',
        onClick: () => {},
      }}
      secondaryAction={{
        label: 'Go back',
        onClick: () => history.back(),
      }}
    />
  );
}
```

### **6. Feature Not Enabled**
```typescript
import { EmptyState } from '@components/empty-state';
import { Zap } from 'lucide-react';

export function FeatureUpsell() {
  return (
    <EmptyState
      type="feature"
      illustration={<Zap size={48} />}
      illustrationColor="primary"
      illustrationVariant="icon-bg"
      title="Unlock advanced analytics"
      description="Get detailed insights into your team's performance, trends, and engagement. Available on the Pro plan."
      primaryAction={{
        label: 'Upgrade to Pro',
        onClick: () => {},
      }}
      secondaryAction={{
        label: 'See what's included',
        href: '/pricing',
      }}
      size="lg"
    />
  );
}
```

### **7. No Filter Results with Active Filters**
```typescript
import { EmptyState } from '@components/empty-state';
import { Filter } from 'lucide-react';

export function NoFilterResults({
  activeFilters,
  onClearAll,
}: {
  activeFilters: string[];
  onClearAll: () => void;
}) {
  return (
    <EmptyState
      type="search"
      illustration={<Filter size={40} />}
      illustrationColor="neutral"
      title="No matches found"
      description={`No items match ${activeFilters.join(', ')}. Try removing some filters.`}
      primaryAction={{
        label: `Clear all ${activeFilters.length} filters`,
        onClick: onClearAll,
      }}
      size="md"
    />
  );
}
```

### **8. Minimal Inline Empty State**
```typescript
import { EmptyState } from '@components/empty-state';
import { Inbox } from 'lucide-react';

export function EmptyCommentThread() {
  return (
    <EmptyState
      layout="minimal"
      illustration={<Inbox size={16} />}
      title="No comments yet"
      size="xs"
      aria-live="polite"
    />
  );
}
```

### **9. Full-Page First-Run Experience**
```typescript
import { EmptyState } from '@components/empty-state';

export function OnboardingEmptyState() {
  return (
    <EmptyState
      layout="full-page"
      illustration="/illustrations/onboarding.svg"
      illustrationAlt="Person setting up their workspace"
      illustrationSize="xl"
      title="Welcome to your workspace"
      description="You're just a few steps away from being set up. Start by creating your first project or inviting your team."
      primaryAction={{
        label: 'Create first project',
        onClick: () => {},
      }}
      secondaryAction={{
        label: 'Invite teammates',
        onClick: () => {},
      }}
      fullHeight
      size="xl"
    />
  );
}
```

---

## Accessibility Specifications

### **ARIA Roles**
- **`role="status"` + `aria-live="polite"`**: When empty state appears dynamically after a search, filter, or load (most common case)
- **`role="alert"`**: Only for error empty states where the failure is urgent (e.g., permissions error blocking core task)
- **`role="region"` + `aria-label`**: When the empty state occupies a named content region (e.g., `aria-label="Search results"`)
- **`role="presentation"`**: For purely decorative illustrations

### **Dynamic Insertion**
- When empty state replaces loaded content, wrap the entire content area in an `aria-live="polite"` region
- The empty state message is then announced when it appears
- Avoid `aria-live="assertive"` unless it's a critical error blocking all workflow

### **Illustration Accessibility**
- Decorative icons/illustrations: `aria-hidden="true"` (the text communicates the meaning)
- Meaningful custom images (e.g., photos): include `alt` text via `illustrationAlt`
- Lottie animations: Should include a static fallback and `aria-hidden="true"` on the animation container

### **Action Buttons**
- Primary and secondary actions use standard Button accessibility (see Button docs)
- Button labels must be descriptive: "Create project" not "Get started"
- External links include `aria-label` noting they open externally: "Learn more about projects (opens in new tab)"

### **Keyboard Navigation**
- **Tab**: Moves focus to primary action button
- **Shift+Tab**: Moves focus to previous element
- **Enter / Space**: Activates focused action
- Empty state itself is not focusable — only its interactive children are

### **Focus Management**
- When empty state replaces a loading state, focus should remain at the natural document position (not forcibly moved)
- For error states: If focus was on a trigger that caused the error (e.g., a search input), focus may remain there to allow correction
- For full-page empty states: h1 heading inside title provides a natural focus landmark

### **Screen Reader Announcements**
- No results (polite): "No results for 'invoices 2023'. Try adjusting your search terms or clearing your filters."
- Error (alert): "Couldn't load your data. Something went wrong. Try again button available."
- Done state (polite): "You're all caught up. No new notifications."
- The announcement reads the title + description together when the region updates

### **Color and Contrast**
- Title text: 4.5:1 minimum on background
- Description text: 4.5:1 minimum
- Illustration icon on tinted background: 3:1 minimum (non-text contrast)
- Never rely on illustration color alone to communicate type — always include text

### **Best Practices**
- Always include at least a title — never show a blank container without explanation
- Pair with `aria-live` region for dynamically appearing empty states
- Avoid animations that loop indefinitely (distracting to users with vestibular disorders)
- Provide at least one actionable path forward — pure informational empty states can frustrate users
- Test with screen readers to confirm announcement timing and completeness

---

## Animation Specifications

### **Mount / Entrance**
- **Target**: Opacity + translateY
- **Duration**: 250ms
- **Easing**: ease-out
- **Delay**: 50ms (to avoid flashing if content loads quickly)
  ```css
  @keyframes emptyStateEnter {
    from { opacity: 0; transform: translateY(8px); }
    to   { opacity: 1; transform: translateY(0); }
  }
  animation: emptyStateEnter 250ms ease-out 50ms both;
  ```

### **Illustration Entrance** (Stagger)
- **Target**: Opacity + scale
- **Duration**: 300ms
- **Delay**: 100ms after container enters
  ```css
  @keyframes illustrationEnter {
    from { opacity: 0; transform: scale(0.85); }
    to   { opacity: 1; transform: scale(1); }
  }
  ```

### **Done / Celebration Animation** (Optional)
- **Type**: Lottie or CSS confetti/check draw
- **Duration**: 1.2s
- **Plays**: Once on mount; does not loop
- **Reduced motion fallback**: Static check icon

### **Loading → Empty Transition**
- Skeleton fades out: 150ms ease-in
- Gap: 50ms
- Empty state fades in: 200ms ease-out
- Prevents jarring instant swap

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .empty-state { animation: none; opacity: 1; transform: none; }
  .empty-state-illustration { animation: none; }
}
```

---

## Design Tokens

### **Illustration Backgrounds**
| Color | Background | Icon Color |
|-------|-----------|------------|
| Neutral | #F3F4F6 | #6B7280 |
| Primary | #EFF6FF | #3B82F6 |
| Success | #ECFDF5 | #10B981 |
| Warning | #FFFBEB | #F59E0B |
| Error | #FEF2F2 | #EF4444 |

### **Typography**
| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Title (MD) | 16px | 600 | #111827 |
| Title (LG) | 20px | 700 | #111827 |
| Title (XL) | 24px | 700 | #111827 |
| Description | 14px | 400 | #6B7280 |
| Description (LG) | 15px | 400 | #6B7280 |

### **Spacing**
| Property | Value |
|----------|-------|
| Illustration → Title gap | 16px (MD), 20px (LG) |
| Title → Description gap | 8px |
| Description → Actions gap | 20px (MD), 24px (LG) |
| Primary → Secondary action gap | 12px |
| Container padding | 24px (MD), 40px (LG), 64px (XL) |

---

## Content Writing Guide by Type

| Type | Headline Pattern | Description Pattern | Primary CTA |
|------|-----------------|---------------------|-------------|
| First use | "No [items] yet" | "Create your first [item] to [benefit]." | "Create [item]" |
| Search | "No results for '[query]'" | "Try adjusting your search or clearing filters." | "Clear search" |
| Filter | "No [items] match these filters" | "Remove some filters to see more results." | "Clear filters" |
| Error | "Couldn't load [content]" | "Something went wrong. [Suggest fix]." | "Try again" |
| Permissions | "You don't have access" | "Contact your admin or [upgrade/request]." | "Request access" |
| Done | "You're all caught up!" | "We'll notify you when something needs attention." | (optional) |
| Feature | "[Feature benefit headline]" | "Available on [plan]. [Value proposition]." | "Upgrade / Enable" |

---

## Common Mistakes to Avoid

- **Vague copy**: "No data" — be specific about what's missing and what to do
- **No action**: Showing an empty state without any path forward increases abandonment
- **Too many actions**: More than 2 actions overwhelms; keep it focused
- **Generic illustration**: Using the same empty state graphic everywhere reduces meaning
- **Missing `aria-live`**: Dynamically injected empty states are invisible to screen readers without it
- **Looping animation**: Celebratory or loading animations that loop are distracting; play once
- **Hiding empty state behind loading**: Always show empty state after load completes; don't keep spinner running
- **Blame language**: "You haven't created anything yet" — prefer "No [items] yet"

---

## Related Components

- **Skeleton**: Loading placeholder before content or empty state appears
- **Contextual Alert**: For inline errors with adjacent content still visible
- **Banner**: For page-level error or warning messages alongside content
- **Illustration Library**: Custom SVG illustrations for empty state contexts
- **Button**: Primary and secondary action components used within empty states
- **Loading Spinner**: Shown before the empty state; transitions to empty state on completion

---

## Version History

- **v1.0**: Basic centered empty state — icon, title, description, single action
- **v1.1**: Added type variants (search, error, done), size variants
- **v1.2**: Horizontal and minimal layouts, secondary action, illustration backgrounds
- **v2.0**: Full-page layout, illustration/animation slot, aria-live patterns, token system
- **v2.1**: Feature/upsell type, loading→empty transition, reduced motion, content writing guide

---

## Testing Checklist

- [ ] All 6 type variants render correct icon color and messaging
- [ ] Centered layout vertically and horizontally centers in container
- [ ] Horizontal layout renders illustration on left, content on right
- [ ] Minimal layout renders as single-line inline empty state
- [ ] Full-page layout fills viewport with large illustration
- [ ] All size variants scale illustration, title, and description correctly
- [ ] Primary action button renders and fires `onClick`
- [ ] Secondary action renders as link or button correctly
- [ ] `aria-live="polite"` announces empty state when dynamically injected
- [ ] `role="alert"` used only on error type
- [ ] Illustration and icons have `aria-hidden="true"`
- [ ] Loading→empty transition animates smoothly (no jarring flash)
- [ ] Entrance animation plays once on mount
- [ ] Celebration animation (done type) plays once and stops
- [ ] Reduced motion disables transform animations
- [ ] Title and description text contrast meets 4.5:1 WCAG AA
- [ ] Touch targets on action buttons meet 44×44px minimum
- [ ] Screen reader announces title + description when region updates
- [ ] No empty state appears during loading (skeleton shown first)
- [ ] Works at all container sizes (widget through full page)
