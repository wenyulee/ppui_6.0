# Loader Component Documentation

## Overview

The Loader component provides visual feedback to users that a process is actively running and that they should wait. It communicates system activity, preventing users from assuming a process has stalled or that their action had no effect. The Loader renders as an animated circular spinner available in four size variants — Large, Medium, Small, and X Small — to suit different contexts, from full-page blocking overlays to inline button states.

**Component name:** `Loader`
**Figma node:** `1700-10843` — Components v6 Beta
**Platform:** React / TypeScript (Web), Jetpack Compose (Android)
**Category:** Feedback / Status

---

## Component Variants

### By Size

| Variant | Token Size | Typical Use |
|---------|-----------|-------------|
| **Large** | 48 × 48 dp | Full-page loading, modal overlay, content skeleton replacement |
| **Medium** | 32 × 32 dp | Section or card loading, inline content areas |
| **Small** | 24 × 24 dp | Form field loading, table cell, compact panels |
| **X Small** | 16 × 16 dp | Button loading state, icon-replacement, dense lists |

---

## Size Variants

### Large (default)
- **Dimensions:** 48 × 48 px
- **Stroke width:** 4 px
- **Animation duration:** 1000 ms per rotation
- **Usage:** Primary loading state for page-level or modal-level waits; pair with an optional loading label beneath the spinner.

### Medium
- **Dimensions:** 32 × 32 px
- **Stroke width:** 3 px
- **Animation duration:** 900 ms per rotation
- **Usage:** Card content loading, data fetching within sections, secondary panels.

### Small
- **Dimensions:** 24 × 24 px
- **Stroke width:** 2.5 px
- **Animation duration:** 800 ms per rotation
- **Usage:** Compact areas, inline with text labels, form validation loading.

### X Small
- **Dimensions:** 16 × 16 px
- **Stroke width:** 2 px
- **Animation duration:** 750 ms per rotation
- **Usage:** Button loading states (replaces button label/icon), dense data rows, avatar placeholder.

---

## Style Variants

### Spinner Color

| Style | Description | Token |
|-------|-------------|-------|
| **Default** | Inherits brand primary color | `--color-loader-default` |
| **Inverse** | White spinner for dark/colored backgrounds | `--color-loader-inverse` |
| **Subtle** | Muted/secondary color for low-emphasis areas | `--color-loader-subtle` |
| **Inherit** | Inherits `currentColor` from parent | `currentColor` |

### Track Visibility

| Style | Description |
|-------|-------------|
| **With track** | Shows a faint circular track behind the spinning arc (default) |
| **Without track** | Arc only, no background ring (cleaner on transparent surfaces) |

---

## State Specifications

### Default (Spinning)
- Arc animates continuously with a `rotate` CSS animation
- Partial arc (approximately 270° sweep) gives the appearance of motion
- No user interaction possible; purely presentational

### Determinate (Progress Variant)
- `strokeDasharray` and `strokeDashoffset` controlled by a `progress` prop (0–100)
- Arc sweeps from 0% to the given percentage in a smooth transition
- Used when the actual progress value is known (file upload, multi-step form)

### Paused / Stopped
- Animation can be paused via `animation-play-state: paused` when the component is off-screen or hidden
- Honors `prefers-reduced-motion`: animation stops; arc remains static at a fixed position

### Error State
- Loader is replaced by an error icon or message — the Loader itself does not have an error visual state
- Parent component is responsible for transitioning from Loader → Error UI

### Hidden / Unmounted
- Loader should be conditionally rendered (`isLoading && <Loader />`) rather than hidden with CSS visibility
- Prevents unnecessary animation overhead when not needed

---

## Props API

### TypeScript Interface

```typescript
import { CSSProperties, HTMLAttributes } from 'react';

// ─── Loader ─────────────────────────────────────────────────────────────────

export type LoaderSize = 'large' | 'medium' | 'small' | 'x-small';
export type LoaderColor = 'default' | 'inverse' | 'subtle' | 'inherit';

export interface LoaderProps extends Omit<HTMLAttributes<SVGElement>, 'children'> {
  /**
   * Controls the diameter and stroke weight of the spinner.
   * @default 'large'
   */
  size?: LoaderSize;

  /**
   * Color variant of the spinning arc.
   * 'inherit' uses currentColor from the parent element.
   * @default 'default'
   */
  color?: LoaderColor;

  /**
   * When true, shows a faint circular track behind the arc.
   * @default true
   */
  showTrack?: boolean;

  /**
   * Controlled progress value (0–100) for the determinate variant.
   * When undefined, the loader spins indefinitely (indeterminate).
   * @default undefined
   */
  progress?: number;

  /**
   * Accessible label announced by screen readers.
   * Defaults to "Loading" when no label is provided.
   * Set to empty string ("") only when a sibling element already describes the loading state.
   * @default 'Loading'
   */
  label?: string;

  /**
   * When true, renders the label text visually below the spinner.
   * The label is always present in the accessibility tree regardless of this prop.
   * @default false
   */
  showLabel?: boolean;

  /**
   * Additional class name(s) to apply to the root SVG element.
   */
  className?: string;

  /**
   * Inline style overrides applied to the root SVG element.
   */
  style?: CSSProperties;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── LoaderOverlay ───────────────────────────────────────────────────────────

export interface LoaderOverlayProps {
  /**
   * Controls visibility of the overlay.
   * @default false
   */
  isVisible: boolean;

  /**
   * Size of the centered Loader within the overlay.
   * @default 'large'
   */
  size?: LoaderSize;

  /**
   * Optional label rendered below the spinner inside the overlay.
   */
  label?: string;

  /**
   * When true, the overlay fills the nearest positioned ancestor.
   * When false, it covers the full viewport.
   * @default true
   */
  bounded?: boolean;

  /**
   * Backdrop opacity (0–1).
   * @default 0.6
   */
  backdropOpacity?: number;

  /**
   * z-index applied to the overlay.
   * @default 1000
   */
  zIndex?: number;

  /**
   * Additional class name(s) applied to the overlay container.
   */
  className?: string;
}
```

---

## Code Examples

### 1. Basic Indeterminate Loader (Large)

```tsx
import { Loader } from '@ds/components';

export function PageLoader() {
  return (
    <div className="flex items-center justify-center min-h-screen">
      <Loader size="large" label="Loading page content" />
    </div>
  );
}
```

### 2. All Four Sizes

```tsx
import { Loader } from '@ds/components';

export function SizeShowcase() {
  return (
    <div className="flex items-end gap-6">
      <Loader size="large"   label="Large loader"   />
      <Loader size="medium"  label="Medium loader"  />
      <Loader size="small"   label="Small loader"   />
      <Loader size="x-small" label="X-Small loader" />
    </div>
  );
}
```

### 3. Button Loading State

```tsx
import { useState } from 'react';
import { Button, Loader } from '@ds/components';

export function SubmitButton() {
  const [isSubmitting, setIsSubmitting] = useState(false);

  const handleSubmit = async () => {
    setIsSubmitting(true);
    try {
      await submitForm();
    } finally {
      setIsSubmitting(false);
    }
  };

  return (
    <Button
      onClick={handleSubmit}
      disabled={isSubmitting}
      aria-busy={isSubmitting}
    >
      {isSubmitting ? (
        <>
          <Loader size="x-small" color="inverse" label="" aria-hidden="true" />
          <span>Submitting…</span>
        </>
      ) : (
        'Submit'
      )}
    </Button>
  );
}
```

### 4. Inline Loader with Visible Label

```tsx
import { Loader } from '@ds/components';

export function DataFetching() {
  return (
    <div className="flex flex-col items-center gap-3 py-12">
      <Loader
        size="medium"
        label="Fetching your transactions"
        showLabel
      />
    </div>
  );
}
```

### 5. Section-Level Loader (Bounded Overlay)

```tsx
import { LoaderOverlay } from '@ds/components';

export function DataTable({ isLoading, data }) {
  return (
    <div style={{ position: 'relative', minHeight: 200 }}>
      <LoaderOverlay
        isVisible={isLoading}
        size="medium"
        label="Loading table data"
        bounded
        backdropOpacity={0.4}
      />
      {!isLoading && <table>{/* table content */}</table>}
    </div>
  );
}
```

### 6. Full-Viewport Overlay

```tsx
import { LoaderOverlay } from '@ds/components';
import { useAppSelector } from '@/store';

export function AppLoader() {
  const isGlobalLoading = useAppSelector(state => state.app.isLoading);

  return (
    <LoaderOverlay
      isVisible={isGlobalLoading}
      size="large"
      label="Loading application"
      bounded={false}
      zIndex={9999}
    />
  );
}
```

### 7. Determinate Progress Loader

```tsx
import { useState, useEffect } from 'react';
import { Loader } from '@ds/components';

export function FileUploadProgress({ file }: { file: File }) {
  const [progress, setProgress] = useState(0);

  useEffect(() => {
    const upload = uploadFile(file, (pct) => setProgress(pct));
    return () => upload.cancel();
  }, [file]);

  return (
    <div className="flex flex-col items-center gap-2">
      <Loader
        size="large"
        progress={progress}
        label={`Uploading: ${progress}%`}
        showLabel
      />
    </div>
  );
}
```

### 8. Loader with Skeleton Replacement Pattern

```tsx
import { Loader } from '@ds/components';
import { Skeleton } from '@ds/components';

export function UserCard({ userId }: { userId: string }) {
  const { data, isLoading } = useUser(userId);

  if (isLoading) {
    return (
      <div
        role="status"
        aria-label="Loading user profile"
        aria-live="polite"
        aria-busy="true"
      >
        <Skeleton variant="card" />
      </div>
    );
  }

  return <UserCardContent user={data} />;
}
```

### 9. Inverse Color on Dark Background

```tsx
import { Loader } from '@ds/components';

export function DarkPanelLoader() {
  return (
    <div
      style={{ background: '#1a1a2e', padding: '48px', borderRadius: '12px' }}
      className="flex justify-center"
    >
      <Loader size="large" color="inverse" label="Loading content" />
    </div>
  );
}
```

### 10. Conditional Loading with Transition

```tsx
import { Loader } from '@ds/components';
import { Transition } from '@headlessui/react';

export function AsyncContent({ isLoading, children }) {
  return (
    <>
      <Transition
        show={isLoading}
        enter="transition-opacity duration-200"
        enterFrom="opacity-0"
        enterTo="opacity-100"
        leave="transition-opacity duration-150"
        leaveFrom="opacity-100"
        leaveTo="opacity-0"
      >
        <div className="flex justify-center py-8">
          <Loader size="medium" label="Loading" />
        </div>
      </Transition>

      <Transition
        show={!isLoading}
        enter="transition-opacity duration-200"
        enterFrom="opacity-0"
        enterTo="opacity-100"
      >
        {children}
      </Transition>
    </>
  );
}
```

---

## Accessibility Specifications

### ARIA Roles and Attributes

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `role` | `status` | Communicates that the region contains a live status message |
| `aria-label` | `"Loading"` (or custom label) | Readable name for the spinner |
| `aria-live` | `"polite"` | Announces loading state without interrupting the user |
| `aria-busy` | `"true"` | Signals to assistive technology that content is updating |
| `aria-hidden` | `"true"` | Applied to the SVG arc itself; the label carries the meaning |

### Screen Reader Behavior

The Loader renders a visually hidden `<span>` containing the `label` text inside a `role="status"` wrapper. This causes screen readers to announce the loading state when the component mounts:

```html
<div role="status" aria-live="polite" aria-label="Loading transactions">
  <svg aria-hidden="true" focusable="false">
    <!-- animated arc -->
  </svg>
  <span class="sr-only">Loading transactions</span>
</div>
```

When the Loader unmounts (loading complete), the parent should use an `aria-live` region to announce the completion:

```tsx
// announce completion
<div role="status" aria-live="polite" className="sr-only">
  {isLoading ? 'Loading…' : 'Content loaded successfully.'}
</div>
```

### Keyboard Interaction

The Loader is not interactive and receives no keyboard focus. It is purely presentational.

| Behavior | Specification |
|----------|---------------|
| Focusable | No — `tabIndex` not set, `focusable="false"` on SVG |
| Focus trap | None (overlays should trap focus on the overlay container, not the loader itself) |
| Escape to dismiss | Not applicable — Loader dismisses only when the async operation completes |

### Focus Management

When a full-page or section overlay Loader appears, focus should not be forcibly moved to the Loader. When it disappears, focus should return to the element that triggered the loading state:

```tsx
const triggerRef = useRef<HTMLButtonElement>(null);
const [isLoading, setIsLoading] = useState(false);

const handleAction = async () => {
  setIsLoading(true);
  await performAction();
  setIsLoading(false);
  // Return focus when loading completes
  triggerRef.current?.focus();
};
```

### Color Contrast

| Element | Minimum Ratio | WCAG Criterion |
|---------|---------------|----------------|
| Spinner arc on white background | 3:1 | 1.4.11 Non-text Contrast |
| Spinner arc on colored overlay | 3:1 | 1.4.11 Non-text Contrast |
| Visible label text | 4.5:1 | 1.4.3 Contrast (Minimum) |
| Inverse spinner on dark background | 3:1 | 1.4.11 Non-text Contrast |

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .loader-arc {
    animation: none;
    /* Keep arc visible at a fixed position to indicate 'busy' state */
    stroke-dashoffset: 60;
  }
}
```

Do not hide the Loader entirely for reduced-motion users — the static arc still communicates a loading state.

---

## Animation Specifications

### Rotation Animation

```css
@keyframes loader-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.loader-arc {
  transform-origin: center;
  animation: loader-spin var(--loader-duration, 1000ms) linear infinite;
}
```

### Arc Dash Animation (Pulsing sweep)

The arc uses a continuous dash-offset animation layered with the rotation to create a more organic, "breathing" motion:

```css
@keyframes loader-dash {
  0% {
    stroke-dasharray: 1, 200;
    stroke-dashoffset: 0;
  }
  50% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -35px;
  }
  100% {
    stroke-dasharray: 89, 200;
    stroke-dashoffset: -124px;
  }
}

.loader-arc {
  animation:
    loader-spin 2000ms linear infinite,
    loader-dash 1500ms ease-in-out infinite;
}
```

### Duration by Size

| Size | Rotation Duration | Reasoning |
|------|------------------|-----------|
| Large | 1000 ms | Slower, calmer for prominent contexts |
| Medium | 900 ms | Slightly faster |
| Small | 800 ms | Proportionally faster |
| X Small | 750 ms | Fastest; subtle and quick |

### Appearance / Disappearance Transitions

The Loader itself does not apply enter/exit animations — its parent container is responsible for fade transitions:

```css
/* Parent container fade */
.loader-wrapper {
  transition: opacity 200ms ease-in-out;
}

.loader-wrapper[data-hidden='true'] {
  opacity: 0;
  pointer-events: none;
}
```

### Determinate Progress Animation

```css
.loader-arc--determinate {
  animation: none; /* No rotation */
  transition: stroke-dashoffset 300ms ease-in-out;
  transform: rotate(-90deg); /* Start at 12 o'clock */
  transform-origin: center;
}
```

---

## Design Tokens

```css
/* ─── Loader Sizes ────────────────────────────────────────────────────── */
--loader-size-large:   48px;
--loader-size-medium:  32px;
--loader-size-small:   24px;
--loader-size-xsmall:  16px;

/* ─── Stroke Widths ───────────────────────────────────────────────────── */
--loader-stroke-large:   4px;
--loader-stroke-medium:  3px;
--loader-stroke-small:   2.5px;
--loader-stroke-xsmall:  2px;

/* ─── Colors ──────────────────────────────────────────────────────────── */
--color-loader-default:  var(--color-brand-primary);       /* Primary brand color */
--color-loader-inverse:  var(--color-neutral-0);           /* White */
--color-loader-subtle:   var(--color-neutral-400);         /* Muted grey */
--color-loader-track:    var(--color-neutral-200);         /* Track ring color */

/* ─── Animation Durations ─────────────────────────────────────────────── */
--loader-duration-large:   1000ms;
--loader-duration-medium:  900ms;
--loader-duration-small:   800ms;
--loader-duration-xsmall:  750ms;

/* ─── Overlay ─────────────────────────────────────────────────────────── */
--loader-overlay-bg:       rgba(255, 255, 255, 0.6);
--loader-overlay-bg-dark:  rgba(0, 0, 0, 0.6);
--loader-overlay-z-index:  1000;
--loader-overlay-blur:     2px;

/* ─── Track ───────────────────────────────────────────────────────────── */
--loader-track-opacity:    0.2;
```

---

## Best Practices

### Do

- **Always provide a meaningful `label`** — Even though the label is visually hidden by default, it is critical for screen reader users to understand what is loading. Use specific labels: "Loading your transactions" rather than just "Loading".
- **Use the smallest appropriate size** — Match the Loader size to the context. Use Large for full-page waits, X Small for button states.
- **Show the Loader immediately** — Display the Loader synchronously when an async action begins to prevent users from re-triggering the action.
- **Disable interactive elements during loading** — When a Loader is active for a form submission, disable the submit button and set `aria-busy="true"` on the form or section.
- **Return focus after loading** — When a loading overlay dismisses, programmatically return focus to the element that triggered the action.
- **Use the determinate variant when progress is known** — A determinate progress loader (0–100%) is significantly more reassuring to users than an indeterminate spinner for long operations like file uploads.
- **Pair with a loading label for waits > 3 seconds** — For any operation that may take 3 seconds or more, show a visible text label (e.g., "Uploading file…") alongside the Loader.

### Don't

- **Don't use multiple competing Loaders on the same view** — One Loader per logical context is enough; multiple spinners create visual noise.
- **Don't animate the Loader in/out with a 0 ms transition** — Always fade the Loader in to reduce jarring visual changes.
- **Don't hide the Loader for `prefers-reduced-motion` users** — Keep a static representation to communicate the loading state.
- **Don't rely on color alone** — The Loader's animated motion is its primary signal; color is secondary. Ensure the arc has sufficient contrast against its background.
- **Don't forget to dismiss the Loader** — Ensure every code path (including error states) removes the Loader. A stuck spinner is one of the most frustrating UX failure modes.
- **Don't use a Loader for instant interactions** — If an operation completes in under 100 ms, do not show a Loader at all; it will flash briefly and disorient users.
- **Don't place a Loader inside a button without `aria-busy`** — Pair visual loader-in-button with `aria-busy="true"` on the button to communicate the busy state to AT.

---

## Loading Delay Best Practice

To prevent "flash of loader" for fast operations, delay showing the Loader by 200–300 ms:

```tsx
import { useState, useEffect } from 'react';

function useDelayedLoader(isLoading: boolean, delay = 250) {
  const [showLoader, setShowLoader] = useState(false);

  useEffect(() => {
    if (!isLoading) {
      setShowLoader(false);
      return;
    }

    const timer = setTimeout(() => setShowLoader(true), delay);
    return () => clearTimeout(timer);
  }, [isLoading, delay]);

  return showLoader;
}

// Usage
export function SmartLoader({ isLoading }: { isLoading: boolean }) {
  const showLoader = useDelayedLoader(isLoading);
  if (!showLoader) return null;
  return <Loader size="medium" label="Loading" />;
}
```

---

## When to Use Loader vs. Skeleton

| Scenario | Use Loader | Use Skeleton |
|----------|-----------|--------------|
| Unknown content shape | ✅ | ❌ |
| Known content shape (cards, lists, tables) | ❌ | ✅ |
| Full-page initial load | ✅ | ✅ |
| Refetching existing data | ✅ | ❌ |
| Button / form submission | ✅ | ❌ |
| Long list loading | ❌ | ✅ |
| File upload progress | ✅ (determinate) | ❌ |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Skeleton** | Preferred alternative to Loader when content shape is known |
| **Progress Bar** | Horizontal determinate progress indicator for multi-step processes |
| **Button** | Host component for X-Small Loader during form submissions |
| **Empty State** | Shown after loading completes with no results |
| **Banner / Toast** | Used to announce loading completion or errors after Loader dismisses |
| **Overlay / Modal** | Container that hosts a Loader for blocking interactions |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Introduced `x-small` size; added `showTrack` prop; updated animation to combined rotation + dash; added `LoaderOverlay` sub-component |
| 5.2 | Added `progress` prop for determinate variant |
| 5.0 | Added `color` prop with `inverse` and `subtle` variants |
| 4.0 | Initial release with Large / Medium / Small sizes |

---

## Testing Checklist

### Visual
- [ ] Large Loader renders at 48 × 48 px
- [ ] Medium Loader renders at 32 × 32 px
- [ ] Small Loader renders at 24 × 24 px
- [ ] X Small Loader renders at 16 × 16 px
- [ ] Stroke widths match size tokens
- [ ] Track ring is visible at correct opacity when `showTrack={true}`
- [ ] Track ring is hidden when `showTrack={false}`
- [ ] `color="default"` uses brand primary color
- [ ] `color="inverse"` uses white arc
- [ ] `color="subtle"` uses neutral-400
- [ ] Visible label renders below spinner when `showLabel={true}`
- [ ] Label is hidden visually but present in DOM when `showLabel={false}`

### Animation
- [ ] Spinner rotates continuously in indeterminate mode
- [ ] Rotation speed varies correctly by size
- [ ] Determinate variant arc sweeps to the correct percentage
- [ ] Animation is paused when `prefers-reduced-motion: reduce` is set
- [ ] Static arc is still visible with reduced motion (not hidden)
- [ ] Dash animation creates natural-looking arc sweep

### Accessibility
- [ ] Root element has `role="status"`
- [ ] `aria-live="polite"` is set on root
- [ ] `aria-label` or visually-hidden label text is present
- [ ] SVG element has `aria-hidden="true"` and `focusable="false"`
- [ ] Screen reader announces loading state on mount
- [ ] Spinner does not receive keyboard focus (not in tab order)
- [ ] Color contrast of arc meets 3:1 ratio on its background
- [ ] Visible label (when shown) meets 4.5:1 contrast ratio

### Interaction
- [ ] Spinner is not interactive (no click/hover effects)
- [ ] `LoaderOverlay` blocks pointer events on content beneath
- [ ] `bounded={true}` overlay is constrained to the nearest `position: relative` ancestor
- [ ] `bounded={false}` overlay covers the full viewport
- [ ] Focus is not trapped in the Loader itself (overlay traps focus at the overlay level)
- [ ] Loader unmounts cleanly without memory leaks (timeout/animation frame cleanup)

### Integration
- [ ] X Small Loader inside Button correctly communicates `aria-busy` state
- [ ] useDelayedLoader hook prevents flash for fast (<250 ms) operations
- [ ] Completion announcement fires when Loader unmounts from a live region
- [ ] Loading state returned from `useQuery`/`useSWR` correctly drives `isLoading` prop
