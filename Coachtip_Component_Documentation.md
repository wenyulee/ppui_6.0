# Coachtip Component Documentation

## Overview

The Coachtip component is a contextual onboarding and guidance element that highlights specific UI features for first-time or returning users. Unlike tooltips — which respond to hover and convey supplemental information — Coachtips are proactively presented as part of product tours, feature announcements, or progressive disclosure flows. They are anchored to a specific target element, include a title and descriptive copy, support step navigation, and are always dismissible. Coachtips draw user attention to where it is needed most without blocking critical workflow paths.

---

## Component Variants

Coachtip supports five primary usage patterns:

### 1. **Single Coachtip**
- **Purpose**: Standalone tip for a single feature or UI element
- **Use Cases**: New feature spotlight, first-run experience, re-engagement hint
- **Behavior**: Appears automatically or on trigger; dismissed via close button or backdrop click
- **Visual**: Popover-style panel with title, body, optional icon, and a dismiss button
- **Persistence**: Can be marked "don't show again" via local storage or user preference

### 2. **Step Coachtip (Tour)**
- **Purpose**: Multi-step guided walkthrough across several UI elements
- **Use Cases**: Product onboarding tour, workflow tutorial, feature discovery sequence
- **Behavior**: Progresses through targets one at a time; back/next/finish navigation
- **Visual**: Step indicator (e.g., "Step 2 of 5"), progress dots or bar, navigation buttons
- **Persistence**: Progress saved; user can resume or skip the tour at any point

### 3. **Feature Announcement Coachtip**
- **Purpose**: Highlights a new or updated feature to existing users
- **Use Cases**: Post-release feature education, changelog callout, A/B test exposure
- **Behavior**: Appears once per user session or until acknowledged; includes a badge
- **Visual**: Includes "New" or "Updated" badge on the anchor element; rich body with optional media
- **Persistence**: Dismissed per-user; stored server-side or in local storage

### 4. **Tooltip Coachtip** (Lightweight)
- **Purpose**: Compact, single-sentence hint with minimal chrome
- **Use Cases**: Icon label expansion, quick action reminders, field-level guidance
- **Behavior**: Triggered on hover, focus, or programmatically; auto-dismisses on interaction
- **Visual**: Compact popover; no title, no step count, minimal padding
- **Persistence**: Not persistent; re-appears on every hover or focus

### 5. **Anchored Spotlight Coachtip**
- **Purpose**: Combines a dimmed overlay with a spotlight cutout around the target element
- **Use Cases**: Complex onboarding where focus must be forced, new user walkthrough
- **Behavior**: Dims rest of page; places coachtip adjacent to spotlighted element; blocks off-target interaction
- **Visual**: Backdrop overlay + cutout highlight + coachtip panel beside target
- **Persistence**: Progress tracked; user can exit spotlight mode at any time

---

## Placement (Anchor Positions)

| Position | Description | Best For |
|----------|-------------|----------|
| **Top** | Tip appears above target, arrow points down | Elements in lower half of viewport |
| **Top Start** | Tip aligns to start (left) edge of target | Left-aligned targets |
| **Top End** | Tip aligns to end (right) edge of target | Right-aligned targets |
| **Bottom** | Tip appears below target, arrow points up | Elements in upper half of viewport |
| **Bottom Start** | Below, left-aligned | Buttons, nav items |
| **Bottom End** | Below, right-aligned | Right-side actions |
| **Left** | Tip appears to left, arrow points right | Right-side panels, sidebars |
| **Right** | Tip appears to right, arrow points left | Left-side nav, toolbars |
| **Auto** | Placement auto-calculated to avoid viewport clipping | Default recommended |

---

## Size Variants

| Size | Width | Padding | Use Case |
|------|-------|---------|----------|
| **SM** | 200–240px | 12px | Tooltip coachtip, brief one-liner hints |
| **MD** | 280–320px | 16px | **Default**, standard single coachtip |
| **LG** | 360–400px | 20px | Feature announcements with richer body |
| **XL** | 440–480px | 24px | Step tours with media, image, or video |

---

## State Specifications

### **Visible State**
- Background: White (#FFFFFF) or dark (#1F2937 for dark variant)
- Shadow: Elevation 8 (0 8px 24px rgba(0,0,0,0.12))
- Border: None, or 1px subtle border (#E5E7EB)
- Arrow: Triangle indicator pointing toward anchor element
- Opacity: 100%
- z-index: 1000+ (above most UI)

### **Entering State**
- Opacity: 0 → 1
- Transform: scale(0.95) → scale(1) + slight translateY
- Duration: 200ms ease-out
- Origin: Toward anchor element

### **Exiting/Dismissed State**
- Opacity: 1 → 0
- Transform: scale(1) → scale(0.95)
- Duration: 150ms ease-in
- Trigger: Close button, backdrop click, step completion, programmatic dismiss

### **Loading State** (async tours)
- Skeleton: Placeholder lines for title and body
- Duration: Continuous pulse (1.5s)
- Shown when: Tour step content loaded asynchronously

### **Spotlight Overlay**
- Backdrop: rgba(0,0,0,0.5) covering full viewport
- Cutout: Box or rounded rectangle revealing target element
- Cutout Padding: 4–8px around target bounds
- z-index: 999 (below coachtip, above content)

---

## Props API

### **Coachtip**

```typescript
interface CoachtipProps {
  // Content
  title?: string;                              // Heading text
  body: string | React.ReactNode;              // Main description
  icon?: React.ReactNode;                      // Leading icon or illustration
  media?: string | React.ReactNode;            // Image, video, or rich media
  badge?: string;                              // e.g., "New", "Updated"
  badgeColor?: 'primary' | 'success' | 'warning' | 'info';

  // Visibility
  open?: boolean;                              // Controlled visibility
  defaultOpen?: boolean;                       // Uncontrolled initial state
  onOpenChange?: (open: boolean) => void;      // Visibility change handler

  // Anchor & Placement
  anchor: React.RefObject<HTMLElement> | string; // Target element ref or CSS selector
  placement?:
    | 'top' | 'top-start' | 'top-end'
    | 'bottom' | 'bottom-start' | 'bottom-end'
    | 'left' | 'right' | 'auto';
  offset?: number;                             // Distance from anchor (default 8px)
  arrowVisible?: boolean;                      // Show/hide anchor arrow

  // Appearance
  size?: 'sm' | 'md' | 'lg' | 'xl';
  variant?: 'light' | 'dark';
  spotlight?: boolean;                         // Enable spotlight overlay

  // Tour / Step
  step?: number;                               // Current step (1-indexed)
  totalSteps?: number;                         // Total steps in tour
  showProgress?: boolean;                      // Show step progress indicator
  progressVariant?: 'dots' | 'bar' | 'counter'; // Progress display style

  // Actions
  primaryAction?: {
    label: string;
    onClick: () => void;
  };
  secondaryAction?: {
    label: string;
    onClick: () => void;
  };
  showDismiss?: boolean;                       // Show × close button (default true)
  dismissLabel?: string;                       // Accessible label for close button

  // Callbacks
  onDismiss?: () => void;                      // Called on any dismiss
  onNext?: (step: number) => void;             // Called on next step
  onBack?: (step: number) => void;             // Called on back step
  onFinish?: () => void;                       // Called on tour completion
  onSkip?: () => void;                         // Called on skip

  // Persistence
  storageKey?: string;                         // Key for localStorage dismiss tracking
  showOnce?: boolean;                          // Only show until first dismiss

  // Accessibility
  aria-label?: string;
  aria-describedby?: string;
  id?: string;

  // Styling
  className?: string;
  style?: React.CSSProperties;
  zIndex?: number;
}
```

### **CoachtipTour**

```typescript
interface CoachtipTourProps {
  steps: CoachtipStep[];                       // Array of step configurations
  active?: boolean;                            // Start/stop tour programmatically
  defaultActive?: boolean;
  onComplete?: () => void;
  onSkip?: () => void;
  storageKey?: string;                         // Persist tour progress in localStorage

  // Global defaults (overridable per step)
  size?: CoachtipProps['size'];
  variant?: CoachtipProps['variant'];
  spotlight?: boolean;
}

interface CoachtipStep {
  anchor: string | React.RefObject<HTMLElement>;
  title?: string;
  body: string | React.ReactNode;
  placement?: CoachtipProps['placement'];
  size?: CoachtipProps['size'];
  media?: React.ReactNode;
  primaryAction?: { label: string; onClick?: () => void }; // defaults to "Next"
  secondaryAction?: { label: string; onClick?: () => void }; // defaults to "Back"
}
```

---

## Code Examples

### **1. Single Feature Coachtip**
```typescript
import { Coachtip } from '@components/coachtip';
import { useRef, useState } from 'react';

export function NewFeatureTip() {
  const [open, setOpen] = useState(true);
  const buttonRef = useRef<HTMLButtonElement>(null);

  return (
    <>
      <button ref={buttonRef} className="btn btn-primary">
        Export
      </button>
      <Coachtip
        anchor={buttonRef}
        open={open}
        onOpenChange={setOpen}
        title="Export is here!"
        body="You can now export your data as CSV, PDF, or Excel with one click."
        badge="New"
        badgeColor="success"
        placement="bottom"
        primaryAction={{ label: 'Got it', onClick: () => setOpen(false) }}
        showOnce
        storageKey="export-coachtip-v1"
      />
    </>
  );
}
```

### **2. Multi-Step Product Tour**
```typescript
import { CoachtipTour } from '@components/coachtip';
import { useRef, useState } from 'react';

export function OnboardingTour() {
  const [tourActive, setTourActive] = useState(true);

  const steps = [
    {
      anchor: '#sidebar-nav',
      title: 'Navigate with ease',
      body: 'Use the sidebar to move between sections. Pin your favourites for quick access.',
      placement: 'right',
    },
    {
      anchor: '#search-bar',
      title: 'Find anything instantly',
      body: 'Press ⌘K or click here to search across all your projects and files.',
      placement: 'bottom',
    },
    {
      anchor: '#notifications-bell',
      title: 'Stay in the loop',
      body: 'Get notified about activity that matters to you. Manage preferences in Settings.',
      placement: 'bottom-end',
    },
    {
      anchor: '#create-button',
      title: "You're all set!",
      body: 'Click here to create your first project and get started.',
      placement: 'bottom',
      primaryAction: { label: 'Start creating' },
    },
  ];

  return (
    <CoachtipTour
      steps={steps}
      active={tourActive}
      spotlight
      onComplete={() => setTourActive(false)}
      onSkip={() => setTourActive(false)}
      storageKey="onboarding-tour-v2"
    />
  );
}
```

### **3. Tooltip Coachtip (Compact)**
```typescript
import { Coachtip } from '@components/coachtip';
import { useRef } from 'react';
import { Info } from 'lucide-react';

export function FieldHint() {
  const iconRef = useRef<HTMLButtonElement>(null);

  return (
    <div className="flex items-center gap-2">
      <label htmlFor="api-key">API Key</label>
      <button ref={iconRef} aria-label="What is an API key?">
        <Info size={16} className="text-gray-400" />
      </button>
      <Coachtip
        anchor={iconRef}
        body="Your API key authenticates requests. Keep it secret — never share it publicly."
        size="sm"
        placement="right"
        showDismiss={false}
        arrowVisible
      />
    </div>
  );
}
```

### **4. Spotlight with Forced Attention**
```typescript
import { Coachtip } from '@components/coachtip';
import { useRef, useState } from 'react';

export function SpotlightTip() {
  const [open, setOpen] = useState(true);
  const targetRef = useRef<HTMLDivElement>(null);

  return (
    <>
      <div ref={targetRef} className="p-4 border rounded-lg">
        <h3>Approval Queue</h3>
        <p>3 items awaiting your review</p>
      </div>

      <Coachtip
        anchor={targetRef}
        open={open}
        title="Action required"
        body="You have pending items that need your approval before the deadline."
        placement="right"
        spotlight
        variant="dark"
        primaryAction={{ label: 'Review now', onClick: () => setOpen(false) }}
        secondaryAction={{ label: 'Remind me later', onClick: () => setOpen(false) }}
        onDismiss={() => setOpen(false)}
      />
    </>
  );
}
```

### **5. Persistent Announcement with showOnce**
```typescript
import { Coachtip } from '@components/coachtip';
import { useRef } from 'react';

export function FeatureAnnouncement() {
  const anchorRef = useRef<HTMLButtonElement>(null);

  return (
    <>
      <button ref={anchorRef} className="btn">
        Analytics
      </button>
      <Coachtip
        anchor={anchorRef}
        title="Analytics, upgraded"
        body="Your dashboard now includes real-time charts, custom date ranges, and CSV export."
        badge="Updated"
        badgeColor="info"
        placement="bottom-start"
        showOnce
        storageKey="analytics-v3-announcement"
        primaryAction={{ label: "What's new?", onClick: () => {} }}
        secondaryAction={{ label: 'Dismiss', onClick: () => {} }}
      />
    </>
  );
}
```

### **6. Dark Variant Coachtip**
```typescript
import { Coachtip } from '@components/coachtip';
import { useRef, useState } from 'react';

export function DarkCoachtip() {
  const [open, setOpen] = useState(true);
  const ref = useRef<HTMLButtonElement>(null);

  return (
    <>
      <button ref={ref}>Settings</button>
      <Coachtip
        anchor={ref}
        open={open}
        variant="dark"
        title="Keyboard shortcuts"
        body="Press ⌘, to open Settings instantly from anywhere in the app."
        placement="top"
        primaryAction={{ label: 'Got it', onClick: () => setOpen(false) }}
        onDismiss={() => setOpen(false)}
      />
    </>
  );
}
```

---

## Accessibility Specifications

### **ARIA Attributes**
- **role="tooltip"**: For lightweight, hover-triggered coachtips
- **role="dialog"**: For modal-like coachtips with interactive elements (buttons, links)
- **role="status"** or **aria-live="polite"**: For non-modal feature announcements
- **aria-labelledby**: Points to the coachtip title element
- **aria-describedby**: Points to the coachtip body element
- **aria-modal="true"**: For spotlight coachtips that block background interaction
- **aria-expanded**: On trigger element when coachtip is open

### **Focus Management**
- On open: Focus moves into the coachtip panel (specifically onto the first focusable element — primary button or close button)
- On close: Focus returns to the anchor/trigger element
- Spotlight mode: Focus trapped within the coachtip; Tab cycles through coachtip focusables only
- Focus ring: 2px solid brand, 2px offset on all interactive elements inside coachtip

### **Keyboard Navigation**
- **Tab / Shift+Tab**: Cycle through focusable elements within coachtip
- **Enter / Space**: Activate focused button (Next, Back, Dismiss, primary/secondary actions)
- **Escape**: Dismiss coachtip; return focus to anchor
- **Arrow Keys**: Navigate tour steps (optional; Left = back, Right = next)

### **Screen Reader Announcements**
- On open: "New feature tip: Export is here! [body text]. Step 1 of 3." (role="dialog" announced on focus)
- On dismiss: Focus returns silently to anchor; no additional announcement needed
- Badge: "New, badge. Export is here!, dialog." (badge read as part of accessible name)
- Tour step navigation: "Step 2 of 5, Navigate with ease. [body]."
- Spotlight: Background described as inert; coachtip receives immediate focus

### **Touch Accessibility**
- Minimum touch targets: 44×44px for all buttons (Next, Back, Dismiss, ×)
- Dismiss × affordance clearly visible and tappable
- Coachtip does not cover more than 50% of the viewport
- Swipe left/right gesture to navigate tour steps (optional enhancement)

### **Reduced Motion**
- When `prefers-reduced-motion: reduce`: Disable scale and translateY entrance animation; use opacity fade only (100ms)

### **Best Practices**
- Always include a dismiss mechanism (close button or action)
- Never block critical actions with a coachtip unless using spotlight mode
- Limit tour to 3–7 steps; longer tours cause abandonment
- Provide a "Skip tour" option on the first step
- Store dismissal state to avoid showing to users who've already seen it
- Test with VoiceOver and NVDA to verify dialog/tooltip semantics

---

## Animation Specifications

### **Entrance Animation**
- **Target**: Opacity + scale + translateY
- **Duration**: 200ms
- **Easing**: cubic-bezier(0.16, 1, 0.3, 1) (spring-out)
  ```css
  @keyframes coachtipEnter {
    from { opacity: 0; transform: scale(0.94) translateY(4px); }
    to   { opacity: 1; transform: scale(1) translateY(0); }
  }
  animation: coachtipEnter 200ms cubic-bezier(0.16, 1, 0.3, 1);
  ```

### **Exit Animation**
- **Target**: Opacity + scale
- **Duration**: 150ms
- **Easing**: ease-in
  ```css
  @keyframes coachtipExit {
    from { opacity: 1; transform: scale(1); }
    to   { opacity: 0; transform: scale(0.94); }
  }
  animation: coachtipExit 150ms ease-in forwards;
  ```

### **Step Transition**
- **Target**: Opacity + translateX (slide left for next, right for back)
- **Duration**: 200ms
- **Easing**: ease-out
  ```css
  /* Next step */
  from { opacity: 0; transform: translateX(16px); }
  to   { opacity: 1; transform: translateX(0); }
  ```

### **Spotlight Overlay**
- **Target**: Backdrop opacity
- **Duration**: 250ms
- **Easing**: ease-out
  ```css
  background: rgba(0,0,0,0) → rgba(0,0,0,0.5);
  transition: background 250ms ease-out;
  ```

### **Progress Dots**
- **Target**: Dot scale + color on step change
- **Duration**: 200ms ease-out

---

## Design Tokens

### **Colors (Light Variant)**
| Purpose | Token | Value |
|---------|-------|-------|
| Background | `--coachtip-bg` | #FFFFFF |
| Border | `--coachtip-border` | #E5E7EB |
| Title | `--coachtip-title` | #111827 |
| Body | `--coachtip-body` | #374151 |
| Arrow Fill | `--coachtip-arrow` | #FFFFFF |
| Badge New | `--coachtip-badge-new` | #10B981 |
| Badge Updated | `--coachtip-badge-updated` | #06B6D4 |

### **Colors (Dark Variant)**
| Purpose | Token | Value |
|---------|-------|-------|
| Background | `--coachtip-dark-bg` | #1F2937 |
| Border | `--coachtip-dark-border` | #374151 |
| Title | `--coachtip-dark-title` | #F9FAFB |
| Body | `--coachtip-dark-body` | #D1D5DB |
| Arrow Fill | `--coachtip-dark-arrow` | #1F2937 |

### **Shadow**
| Level | Value |
|-------|-------|
| Default | 0 8px 24px rgba(0,0,0,0.12), 0 2px 8px rgba(0,0,0,0.08) |
| Spotlight | 0 16px 48px rgba(0,0,0,0.2) |

### **Sizing**
| Property | Value |
|----------|-------|
| Arrow Size | 8×8px |
| Offset from anchor | 8px (default) |
| Border Radius | 10px |
| Close button size | 28×28px |
| Progress dot size | 6px |
| Progress dot gap | 4px |

---

## Best Practices

### **Content Guidelines**
- Title: 3–6 words; clear and specific ("Export is now available")
- Body: 1–2 sentences max; action-oriented and benefit-led
- Primary action: Tell users what happens ("Got it", "Try it now", "Next")
- Avoid vague copy ("Click here", "Learn more" without context)
- Badge labels: "New", "Updated", "Beta" — use sparingly

### **Timing & Triggers**
- Don't show coachtips on every session — once per user is the norm
- Wait until the user has settled (500ms–1s after page load)
- Don't show if the user is mid-interaction with a form or modal
- Respect `prefers-reduced-motion` and `prefers-color-scheme`
- Queue multiple coachtips; never show more than one at a time

### **Tour Design**
- 3–5 steps is ideal; 7 is the practical maximum
- Always include "Skip" on step 1
- Sequence should follow natural task flow (top-left → bottom-right)
- Each step should be independently understandable
- Save progress so users can resume an interrupted tour

### **Persistence**
- Use `localStorage` for anonymous users or quick wins
- Use server-side preference storage for authenticated users
- Include a version suffix in storage keys (e.g., `"export-tip-v2"`) to re-show after major updates
- Provide a way for users to replay tours from Settings/Help

---

## Related Components

- **Tooltip**: Hover-triggered, non-persistent information only (no actions)
- **Banner**: Full-width site-level announcements
- **Modal / Dialog**: Full-attention interruptions requiring decision
- **Popover**: Action-triggered overlay (not proactively shown)
- **Snackbar / Toast**: Transient feedback for completed actions

---

## Version History

- **v1.0**: Single coachtip with light/dark variant, placement, dismiss
- **v1.1**: Added step tour, progress dots/bar, back/next navigation
- **v1.2**: Spotlight overlay, showOnce + storageKey persistence
- **v2.0**: CoachtipTour component, step slide animations, focus trap in spotlight
- **v2.1**: Badge support, media slot, reduced-motion compliance, RTL

---

## Testing Checklist

- [ ] Coachtip renders adjacent to anchor with correct placement
- [ ] Arrow points toward anchor element
- [ ] Auto placement avoids viewport clipping
- [ ] Entrance animation plays on open
- [ ] Exit animation plays on dismiss
- [ ] Close (×) button dismisses coachtip
- [ ] Escape key dismisses coachtip
- [ ] Focus moves into coachtip on open
- [ ] Focus returns to anchor on close
- [ ] Tab/Shift+Tab cycles within coachtip only (spotlight mode)
- [ ] Next/Back navigation updates step content correctly
- [ ] Progress indicator reflects current step
- [ ] showOnce + storageKey prevents re-show after dismiss
- [ ] Spotlight overlay renders with correct cutout
- [ ] Dark variant renders correct colors
- [ ] Badge renders with correct color
- [ ] Screen reader announces role="dialog" with title
- [ ] Reduced motion uses fade only
- [ ] Touch targets all meet 44×44px minimum
- [ ] Tour skip exits all steps immediately
