# Pagination Component Documentation

## Overview

The Pagination component is a **dot-based step and position indicator** used to show a user's current position within a sequence of panels, slides, or steps. It renders as a row of small circular dots — one dot per item — where the active item is filled in the primary selected color and all others are shown in a muted unselected color.

On **mobile**, the component renders dots only. On **desktop**, it pairs the dot row with optional **arrow navigation controls** (back and next `IconButton`s) whose visibility adapts to the user's position in the sequence: only the "next" arrow on the first step, both arrows on intermediate steps, and only the "back" arrow on the last step.

> **Important:** This is a carousel / stepper position indicator, not a traditional numbered page-turner pagination (e.g., "Page 1 of 42"). For numbered page navigation, use the Table Pagination or Numbered Pagination pattern.

**Component name:** `Pagination` / `PaginationMobile` / `PaginationDesktop`
**Figma nodes:** `1607-8717` (Mobile), `2718-9359` (Desktop), `8983-12857` (Parts — `Pagination.Dots`)
**Platform:** React / TypeScript (Web), Jetpack Compose (Android)
**Category:** Navigation / Indicator

---

## Component Variants

### By Platform

| Variant | Node | Controls | Typical Use |
|---------|------|----------|-------------|
| **Mobile** | `1607-8717` | Dots only — no arrow buttons | Carousel slides, onboarding screens, feature tours on touch devices |
| **Desktop** | `2718-9359` | Dots + optional IconButton arrows | Carousels, image galleries, multi-panel walkthroughs on larger screens |

### By State (Position in Sequence)

| State | Description | Controls Visible |
|-------|-------------|-----------------|
| **Rest** | First item is active; user has not yet advanced | Desktop: "next" arrow only (right-aligned) |
| **Advanced** | A middle item is active; user is partway through | Desktop: both "back" (left) and "next" (right) arrows |
| **Last** | Final item is active; sequence is complete | Desktop: "back" arrow only (left-aligned) |

---

## Anatomy

### Mobile (Dots Only)

```
  ●  ○  ○        ← Rest state   (dot 1 active)
  ○  ●  ○        ← Advanced     (dot 2 active)
  ○  ○  ●        ← Last state   (dot 3 active)
```

### Desktop (Dots + Controls)

```
Rest state:
                                       [→]
              ●  ○  ○

Advanced state:
[←]                                    [→]
              ○  ●  ○

Last state:
[←]
              ○  ○  ●
```

### Sub-Component: `Pagination.Dots`

| State | Node | Size | Color |
|-------|------|------|-------|
| Active | `1600:8374` | 8 × 8 px | `tokens.colorScheme.background.utility.selected` (black) |
| Inactive | `1600:8376` | 8 × 8 px | `tokens.colorScheme.background.utility.unselected` (#999) |

Both states use `border-radius: tokens.base.border.radius.full` (999 px), rendering as perfect circles.

---

## Size & Spacing Specifications

### Dot

| Property | Value | Token |
|----------|-------|-------|
| Diameter | 8 × 8 px | Fixed |
| Border radius | 999 px (full circle) | `tokens.base.border.radius.full` |
| Gap between dots | 8 px | `tokens.base.spacing[8]` |
| Overflow | `clip` | — |

### Desktop Container

| Property | Value | Token |
|----------|-------|-------|
| Width | 370 px | Fixed |
| Height | 64 px | Fixed |
| Padding | 0 px | `tokens.base.spacing[0]` |
| Gap (controls row ↔ dots row) | 24 px | `tokens.base.spacing[24]` |
| Control button size | Small | `IconButton` size="small" |

### Mobile Container

| Property | Value |
|----------|-------|
| Width | Content-sized (dots + gaps) |
| Height | Content-sized |
| Alignment | Centered horizontally |

---

## Style Variants

### Dot Colors

| State | Token | Default Value |
|-------|-------|---------------|
| Active | `tokens.colorScheme.background.utility.selected` | Black (`#000`) |
| Inactive | `tokens.colorScheme.background.utility.unselected` | Grey (`#999`) |

### Desktop Elevation

The Desktop Pagination container uses **Elevation / Level 1**:

```
shadow-1: x=0, y=0, blur=8, spread=0, color=tokens.colorScheme.shadow.emphasis
shadow-2: x=0, y=4, blur=4, spread=0, color=tokens.colorScheme.shadow.base
```

### Control Buttons

Both arrow controls use `IconButton` with:
- `size="small"`
- `variant="tertiary-contained"`
- Left icon: `arrow-left` — `aria-label="Back"` / `"Previous"`
- Right icon: `arrow-right` — `aria-label="Next"`

---

## State Specifications

### Rest (First Item Active)
- Dot index 0 is the active (filled) dot
- All other dots are inactive (muted)
- Desktop: only the "next" (→) IconButton is shown, right-aligned
- "Back" (←) button is hidden — there is no previous item

### Advanced (Middle Item Active)
- Dot at index `activeIndex` (1 through `dotCount - 2`) is active
- Desktop: both "back" (←) and "next" (→) buttons are shown
- Left button is left-aligned; right button is right-aligned; dots are centered

### Last (Final Item Active)
- Dot at index `dotCount - 1` is active
- Desktop: only the "back" (←) IconButton is shown, left-aligned
- "Next" (→) button is hidden — there are no more items

### Controlled vs. Uncontrolled
- When used in **controlled mode** (`activeIndex` + `onChange`), the parent manages the current step
- When used in **uncontrolled mode**, the component manages its own internal `activeIndex` state

---

## Props API

### TypeScript Interface

```typescript
// ─── Pagination.Dots ─────────────────────────────────────────────────────────

export interface PaginationDotProps {
  /**
   * Whether this individual dot is in the active state.
   * @default false
   */
  active?: boolean;

  /**
   * Additional class name(s) applied to the dot element.
   */
  className?: string;
}

// ─── Pagination (Unified / Mobile) ───────────────────────────────────────────

export interface PaginationProps {
  /**
   * Total number of dots (panels, slides, or steps) to render.
   */
  dotCount: number;

  /**
   * Zero-based index of the currently active dot.
   * @default 0
   */
  activeIndex?: number;

  /**
   * Callback fired when the user clicks a dot directly or an arrow button.
   * Receives the new zero-based index.
   */
  onChange?: (index: number) => void;

  /**
   * When true, renders arrow navigation buttons alongside the dots (desktop layout).
   * Has no effect on the mobile-only variant.
   * @default true
   */
  controls?: boolean;

  /**
   * Accessible label for the pagination region.
   * @default 'Slide navigation'
   */
  'aria-label'?: string;

  /**
   * Additional class name(s) applied to the root container.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── PaginationMobile ─────────────────────────────────────────────────────────

export interface PaginationMobileProps extends PaginationProps {
  /**
   * Positional state of the pagination indicator.
   * 'Rest' = first item active; 'Advanced' = any other item active.
   * When activeIndex and dotCount are provided, state is derived automatically.
   * @default 'Rest'
   */
  state?: 'Rest' | 'Advanced';
}

// ─── PaginationDesktop ────────────────────────────────────────────────────────

export interface PaginationDesktopProps extends PaginationProps {
  /**
   * Positional state of the pagination indicator.
   * Automatically derived when activeIndex and dotCount are provided.
   * @default 'Rest'
   */
  state?: 'Rest' | 'Advanced' | 'Last';

  /**
   * Accessible label for the "back" arrow button.
   * @default 'Previous'
   */
  backLabel?: string;

  /**
   * Accessible label for the "next" arrow button.
   * @default 'Next'
   */
  nextLabel?: string;
}
```

---

## Deriving State from activeIndex

Rather than passing `state` manually, compute it from `activeIndex` and `dotCount`:

```typescript
function getPaginationState(
  activeIndex: number,
  dotCount: number
): 'Rest' | 'Advanced' | 'Last' {
  if (activeIndex === 0)             return 'Rest';
  if (activeIndex === dotCount - 1)  return 'Last';
  return 'Advanced';
}
```

---

## Code Examples

### 1. Basic Mobile Dot Indicator

```tsx
import { useState } from 'react';
import { Pagination } from '@ds/components';

export function OnboardingDots({ totalSlides }: { totalSlides: number }) {
  const [activeIndex, setActiveIndex] = useState(0);

  return (
    <div className="flex flex-col items-center gap-6">
      {/* Slide content */}
      <SlideContent index={activeIndex} />

      {/* Dot indicator */}
      <Pagination
        dotCount={totalSlides}
        activeIndex={activeIndex}
        onChange={setActiveIndex}
      />
    </div>
  );
}
```

### 2. Desktop Carousel with Arrow Controls

```tsx
import { useState } from 'react';
import { PaginationDesktop } from '@ds/components';

export function FeatureCarousel({ features }: { features: Feature[] }) {
  const [activeIndex, setActiveIndex] = useState(0);

  return (
    <div className="flex flex-col items-center gap-4" style={{ width: 370 }}>
      {/* Carousel content */}
      <FeatureCard feature={features[activeIndex]} />

      {/* Pagination with arrow controls */}
      <PaginationDesktop
        dotCount={features.length}
        activeIndex={activeIndex}
        onChange={setActiveIndex}
        controls
      />
    </div>
  );
}
```

### 3. Controlled Pagination with External Navigation

```tsx
import { useState } from 'react';
import { PaginationDesktop, Button } from '@ds/components';

const STEPS = ['Account', 'Address', 'Payment', 'Review'];

export function MultiStepForm() {
  const [step, setStep] = useState(0);

  const goBack = () => setStep(prev => Math.max(0, prev - 1));
  const goNext = () => setStep(prev => Math.min(STEPS.length - 1, prev + 1));

  return (
    <div>
      <h2>{STEPS[step]}</h2>
      <StepContent step={step} />

      {/* Pagination controls drive both dot indicator and the form step */}
      <PaginationDesktop
        dotCount={STEPS.length}
        activeIndex={step}
        onChange={setStep}
        controls
        aria-label="Form step navigation"
      />

      {/* External buttons also update the same state */}
      <div className="flex gap-4 mt-4">
        <Button onClick={goBack} disabled={step === 0}>Back</Button>
        <Button onClick={goNext} disabled={step === STEPS.length - 1}>
          {step === STEPS.length - 1 ? 'Submit' : 'Continue'}
        </Button>
      </div>
    </div>
  );
}
```

### 4. Auto-Advancing Carousel

```tsx
import { useState, useEffect, useRef } from 'react';
import { Pagination } from '@ds/components';

export function AutoCarousel({ slides, intervalMs = 4000 }) {
  const [activeIndex, setActiveIndex] = useState(0);
  const timerRef = useRef<ReturnType<typeof setInterval>>();

  const resetTimer = () => {
    clearInterval(timerRef.current);
    timerRef.current = setInterval(() => {
      setActiveIndex(prev => (prev + 1) % slides.length);
    }, intervalMs);
  };

  useEffect(() => {
    resetTimer();
    return () => clearInterval(timerRef.current);
  }, [slides.length, intervalMs]);

  const handleChange = (index: number) => {
    setActiveIndex(index);
    resetTimer(); // Reset auto-advance on manual interaction
  };

  return (
    <div>
      <div role="region" aria-roledescription="carousel" aria-label="Feature highlights">
        <div
          role="group"
          aria-roledescription="slide"
          aria-label={`Slide ${activeIndex + 1} of ${slides.length}`}
        >
          <SlideContent slide={slides[activeIndex]} />
        </div>
      </div>

      <Pagination
        dotCount={slides.length}
        activeIndex={activeIndex}
        onChange={handleChange}
        aria-label="Carousel slide navigation"
      />
    </div>
  );
}
```

### 5. Mobile Sheet with Step Indicator

```tsx
import { useState } from 'react';
import { MobileSheet, Pagination, Button, Dock } from '@ds/components';

const ONBOARDING_STEPS = [
  { title: 'Welcome', content: <WelcomeStep /> },
  { title: 'Set up your account', content: <AccountStep /> },
  { title: 'Verify identity', content: <VerifyStep /> },
];

export function OnboardingSheet({ isOpen, onClose }) {
  const [step, setStep] = useState(0);
  const current = ONBOARDING_STEPS[step];
  const isLast = step === ONBOARDING_STEPS.length - 1;

  return (
    <MobileSheet
      isOpen={isOpen}
      onClose={onClose}
      variant="full-screen"
      topNav={{ title: current.title, showClose: true, onClose }}
      footer={
        <Dock>
          {/* Dots centered above the CTA button */}
          <Pagination
            dotCount={ONBOARDING_STEPS.length}
            activeIndex={step}
            onChange={setStep}
          />
          <Button
            size="large"
            variant="primary"
            fullWidth
            onClick={() => isLast ? onClose() : setStep(step + 1)}
          >
            {isLast ? 'Get started' : 'Continue'}
          </Button>
        </Dock>
      }
    >
      {current.content}
    </MobileSheet>
  );
}
```

### 6. Dots Only (No onChange — Read-Only Indicator)

```tsx
import { Pagination } from '@ds/components';

/**
 * Read-only position indicator — the user cannot click dots to navigate.
 * Used when the carousel is driven purely by swipe gestures on mobile.
 */
export function SwipeCarousel({ slides, activeIndex }) {
  return (
    <div>
      <SwipeableView
        slides={slides}
        activeIndex={activeIndex}
      />
      <Pagination
        dotCount={slides.length}
        activeIndex={activeIndex}
        // No onChange = read-only indicator
        aria-label={`Slide ${activeIndex + 1} of ${slides.length}`}
      />
    </div>
  );
}
```

### 7. Desktop — Controls Hidden

```tsx
import { PaginationDesktop } from '@ds/components';

/**
 * Desktop dots-only indicator when navigation is handled
 * by external next/prev buttons (e.g., large image gallery arrow overlays).
 */
export function GalleryDots({ totalImages, activeIndex, onChange }) {
  return (
    <PaginationDesktop
      dotCount={totalImages}
      activeIndex={activeIndex}
      onChange={onChange}
      controls={false}
      aria-label="Image gallery position"
    />
  );
}
```

### 8. Programmatically Derived State

```tsx
import { PaginationDesktop } from '@ds/components';

function getPaginationState(
  activeIndex: number,
  dotCount: number
): 'Rest' | 'Advanced' | 'Last' {
  if (activeIndex === 0)             return 'Rest';
  if (activeIndex === dotCount - 1)  return 'Last';
  return 'Advanced';
}

export function WalkthroughPagination({ steps, activeIndex, onChange }) {
  return (
    <PaginationDesktop
      dotCount={steps.length}
      activeIndex={activeIndex}
      state={getPaginationState(activeIndex, steps.length)}
      onChange={onChange}
      controls
      backLabel="Go back"
      nextLabel="Go forward"
    />
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern — Carousel / Tablist

The Pagination component should be wrapped in a semantic region when used inside a carousel:

```html
<!-- Carousel region -->
<section aria-roledescription="carousel" aria-label="Feature highlights">
  <!-- Live region for slide announcements -->
  <div aria-live="polite" aria-atomic="true" class="sr-only">
    Slide 2 of 5
  </div>

  <!-- Slide content -->
  <div
    role="group"
    aria-roledescription="slide"
    aria-label="2 of 5"
  >
    <!-- slide content -->
  </div>
</section>

<!-- Pagination indicator / controls -->
<nav aria-label="Carousel slide navigation">
  <!-- Back button -->
  <button aria-label="Previous slide" aria-disabled="false">←</button>

  <!-- Dot indicators (individual tabs) -->
  <div role="tablist" aria-label="Slides">
    <button role="tab" aria-selected="false" aria-label="Slide 1">●</button>
    <button role="tab" aria-selected="true"  aria-label="Slide 2">●</button>
    <button role="tab" aria-selected="false" aria-label="Slide 3">●</button>
  </div>

  <!-- Next button -->
  <button aria-label="Next slide" aria-disabled="false">→</button>
</nav>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Dot buttons (when clickable) | `"tab"` |
| `role` | Dot container (when clickable) | `"tablist"` |
| `aria-selected` | Each dot/tab | `"true"` for active, `"false"` otherwise |
| `aria-label` | Each dot/tab | `"Slide [n]"` or `"Step [n]"` |
| `aria-label` | Back button | `"Previous"` / `"Previous slide"` |
| `aria-label` | Next button | `"Next"` / `"Next slide"` |
| `aria-disabled` | Hidden control button | `"true"` (when at boundary) OR do not render |
| `aria-live` | Slide announcement region | `"polite"` |
| `aria-roledescription` | Carousel root | `"carousel"` |
| `aria-roledescription` | Slide container | `"slide"` |

### Keyboard Interaction

| Key | Element | Behavior |
|-----|---------|----------|
| `Tab` | Pagination region | Moves focus into the control row |
| `Tab` | Controls | Cycles through back button → dots → next button |
| `Enter` / `Space` | Dot button | Navigates to that slide |
| `Enter` / `Space` | Back / Next button | Navigates to previous / next slide |
| `ArrowLeft` / `ArrowRight` | Dot (tablist) | Moves focus and navigates to adjacent dot |
| `Home` | Dot (tablist) | Moves focus to first dot |
| `End` | Dot (tablist) | Moves focus to last dot |

### When Controls Are Hidden at Boundaries

Do not render a disabled back button on the first slide or a disabled next button on the last slide. Screen readers would encounter a button that does nothing. Instead, simply omit the button:

```tsx
{activeIndex > 0 && (
  <IconButton aria-label="Previous" onClick={() => onChange(activeIndex - 1)} />
)}

{activeIndex < dotCount - 1 && (
  <IconButton aria-label="Next" onClick={() => onChange(activeIndex + 1)} />
)}
```

### Auto-Advancing Carousels

If the carousel auto-advances:
- Provide a pause/play toggle that is always accessible
- Honor `prefers-reduced-motion` — stop auto-advance entirely when set
- Pause auto-advance on keyboard focus or mouse hover

```css
@media (prefers-reduced-motion: reduce) {
  /* handled in JS: clearInterval(timerRef.current) when media query matches */
}
```

### Screen Reader Announcement on Slide Change

Use a visually-hidden live region to announce each transition:

```tsx
const [announcement, setAnnouncement] = useState('');

const handleChange = (index: number) => {
  onChange(index);
  setAnnouncement(`Showing slide ${index + 1} of ${dotCount}`);
};

return (
  <>
    <div role="status" aria-live="polite" className="sr-only">
      {announcement}
    </div>
    {/* Pagination dots + controls */}
  </>
);
```

### Color Contrast

| Element | Minimum Ratio | WCAG |
|---------|---------------|------|
| Active dot on white background | 3:1 | 1.4.11 Non-text Contrast |
| Inactive dot on white background | 3:1 | 1.4.11 Non-text Contrast |
| Focus ring on dot/button | 3:1 | 1.4.11 Non-text Contrast |
| Arrow icon on button background | 3:1 | 1.4.11 Non-text Contrast |

---

## Animation Specifications

### Dot Transition (Active ↔ Inactive)

```css
.pagination-dot {
  transition:
    background-color 200ms ease-in-out,
    transform 200ms ease-in-out;
}

/* Optional: slight scale-up for the active dot */
.pagination-dot--active {
  transform: scale(1.25);
}
```

### Arrow Button Appearance / Disappearance

Rather than hiding buttons with `display:none` (which causes layout shift), fade them in/out:

```css
.pagination-control {
  transition: opacity 150ms ease, visibility 150ms ease;
}

.pagination-control--hidden {
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
}
```

Alternatively, reserve space for both arrow buttons at all times (even when invisible at boundaries) to prevent content from reflowing when switching between Rest → Advanced → Last states.

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .pagination-dot,
  .pagination-control {
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Dot Size ────────────────────────────────────────────────────────── */
--pagination-dot-size:          8px;
--pagination-dot-radius:        var(--tokens.base.border.radius.full);   /* 999px */
--pagination-dot-gap:           var(--tokens.base.spacing[8]);           /* 8px */

/* ─── Dot Colors ──────────────────────────────────────────────────────── */
--pagination-dot-active:        var(--tokens.colorScheme.background.utility.selected);    /* black */
--pagination-dot-inactive:      var(--tokens.colorScheme.background.utility.unselected);  /* #999 */

/* ─── Container Spacing ───────────────────────────────────────────────── */
--pagination-gap-controls-dots: var(--tokens.base.spacing[24]);          /* 24px */
--pagination-desktop-width:     370px;
--pagination-desktop-height:    64px;
--pagination-padding:           var(--tokens.base.spacing[0]);           /* 0px */

/* ─── Control Button ──────────────────────────────────────────────────── */
/* Uses IconButton tokens: size=small, variant=tertiary-contained */

/* ─── Desktop Elevation (container) ──────────────────────────────────── */
--pagination-shadow-1: 0px 0px 8px 0px var(--tokens.colorScheme.shadow.emphasis);
--pagination-shadow-2: 0px 4px 4px 0px var(--tokens.colorScheme.shadow.base);
```

---

## Best Practices

### Do

- **Use dots for short sequences (2–7 items)** — Beyond 7 dots the indicator becomes hard to parse and occupies too much space. For longer sequences, consider a numeric indicator (e.g., "3 / 12") or a thin progress bar.
- **Pair the Desktop variant with `controls={true}` by default** — Arrow buttons provide a clear affordance for navigation and are required for keyboard users who cannot swipe.
- **Announce slide transitions to screen readers** — Always pair the visual dot update with an `aria-live` region that speaks the current slide number.
- **Derive `state` programmatically** — Compute `'Rest' | 'Advanced' | 'Last'` from `activeIndex` and `dotCount` rather than passing it as a hardcoded string. This prevents the controls from being in the wrong state.
- **Reserve layout space for absent controls** — To prevent the dot row from jumping left/right when the single arrow button appears or disappears, keep invisible buttons in the DOM and use `opacity: 0; pointer-events: none` rather than conditionally rendering them (or pre-reserve a fixed-width placeholder).
- **Stop auto-advance on focus and `prefers-reduced-motion`** — Users who rely on keyboard navigation or who have motion sensitivity should never be surprised by a slide changing beneath them.

### Don't

- **Don't use dots for sequences longer than 7 items** — More than 7 dots are visually indistinguishable and unusable. Use a numbered indicator instead.
- **Don't confuse this component with numbered pagination** — This is a carousel position indicator. Do not use it for "Page 1 of 50" data table or list navigation — use a dedicated Numbered Pagination component for that.
- **Don't make dots the only navigation mechanism** — On desktop, always pair dots with arrow buttons. On mobile, combine with swipe gestures or explicit next/back buttons, especially in forms where the user may not realise dots are tappable.
- **Don't omit the `aria-label` on the navigation region** — Without a label, the `<nav>` or `role="tablist"` container has no accessible name, making it confusing for screen reader users.
- **Don't render a disabled boundary button** — At the first slide, the back button should simply not exist; at the last, the next button should not exist. A visible disabled button implies the action is possible but blocked, which is misleading here.
- **Don't reset the user's position** when the carousel content updates (e.g., after a data refresh) unless the new content count is smaller than the current index.

---

## Pagination Dots vs. Other Position Indicators

| Pattern | When to Use |
|---------|------------|
| **Pagination Dots (this component)** | 2–7 carousel slides, onboarding steps, feature tours |
| **Progress Bar / Stepper** | Multi-step flows where each step has a label and the user needs to understand overall progress |
| **Numbered indicator ("3 / 12")** | Sequences of 8+ items where total count provides meaningful context |
| **Thumbnail strip** | Image galleries where seeing the content of adjacent slides aids navigation |
| **Scrollbar** | Long continuous scrollable lists or infinite feeds |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **IconButton** | Used for the back (←) and next (→) controls in the Desktop variant |
| **Button** | Often paired below or above the dots in multi-step flows (Continue / Back) |
| **Modal / MobileSheet** | Common host for the Pagination indicator inside onboarding or walkthrough flows |
| **Dock** | Bottom dock in a mobile sheet; Pagination dots typically sit above the primary CTA in the Dock |
| **Progress Bar** | Alternative for step-based flows where each step has a label |
| **Stepper** | Alternative for multi-step forms that need visible step labels and status |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Unified Mobile and Desktop into a single `Pagination` component family; added `controls` prop; standardised `Rest` / `Advanced` / `Last` state model; extracted `Pagination.Dots` as a standalone sub-component |
| 5.1 | Added arrow controls to Desktop variant |
| 4.0 | Initial release — mobile dots only |

---

## Testing Checklist

### Visual
- [ ] Active dot renders at 8 × 8 px with `tokens.colorScheme.background.utility.selected` color (black)
- [ ] Inactive dots render at 8 × 8 px with `tokens.colorScheme.background.utility.unselected` color (#999)
- [ ] All dots are perfect circles (border-radius: full)
- [ ] Gap between dots is 8 px
- [ ] Desktop container is 370 × 64 px with 24 px gap between control row and dot row
- [ ] Desktop elevation shadow renders correctly

### State — Desktop Controls
- [ ] Rest state: only "next" (→) arrow renders, right-aligned
- [ ] Advanced state: both "back" (←) and "next" (→) arrows render in correct positions
- [ ] Last state: only "back" (←) arrow renders, left-aligned
- [ ] Controls do not cause dot row to shift position when appearing/disappearing
- [ ] `controls={false}` hides all arrow buttons regardless of state

### Behavior
- [ ] Clicking a dot updates `activeIndex` and fires `onChange`
- [ ] Clicking "next" increments `activeIndex` by 1 and fires `onChange`
- [ ] Clicking "back" decrements `activeIndex` by 1 and fires `onChange`
- [ ] `onChange` is not called when clicking the currently active dot
- [ ] Auto-advancing carousel resets timer on manual interaction
- [ ] `activeIndex` cannot exceed `dotCount - 1` or go below 0

### Keyboard Navigation
- [ ] Tab reaches the dot container and arrow buttons
- [ ] ArrowLeft / ArrowRight navigates between dots when focused
- [ ] Enter / Space activates the focused dot
- [ ] Enter / Space activates arrow buttons
- [ ] Home focuses the first dot; End focuses the last dot

### Accessibility
- [ ] Each dot has `role="tab"` and correct `aria-selected` value
- [ ] Dot container has `role="tablist"` with `aria-label`
- [ ] Active dot is `aria-selected="true"`; inactive dots are `aria-selected="false"`
- [ ] Back button has `aria-label="Previous"` (or equivalent)
- [ ] Next button has `aria-label="Next"` (or equivalent)
- [ ] Slide transition is announced via `aria-live="polite"` region
- [ ] Hidden arrow buttons at boundaries are absent from DOM (not just hidden) or have `aria-hidden="true"`
- [ ] Active and inactive dots meet 3:1 non-text contrast ratio

### Reduced Motion
- [ ] Dot transition animation is suppressed under `prefers-reduced-motion: reduce`
- [ ] Auto-advancing carousel pauses under `prefers-reduced-motion: reduce`
