# Progress Bar Component Documentation

## Overview

The Progress Bar component communicates the completion status of a task or process. It comes in two structural variants: a **Continuous** bar that shows overall progress as a single unbroken fill, and a **Stepped** bar that divides the track into discrete segments — one per step — allowing progress to be shown both at the step level and within the current step.

Both variants share the same set of **seven semantic style variants** (Info, Positive, Negative, Warning, Special, Neutral, Brand) that control the fill color of the active progress arc, mapping directly to the design system's status color tokens.

**Component name:** `ProgressBar` / `ProgressBarStepped`
**Figma nodes:** `184-5386` (Continuous), `26396-14991` (Stepped), `26395-18569` (Parts — `Progress.Item`)
**Platform:** React / TypeScript (Web), SwiftUI (iOS — `PDSProgressBar`), Jetpack Compose (Android — `ProgressBar`)
**Category:** Feedback / Status

---

## Component Variants

### By Structure

| Variant | Node | Description | Use |
|---------|------|-------------|-----|
| **Continuous** | `184-5386` | Single unbroken horizontal track; fill sweeps from left to right | Overall file uploads, page loading, indefinite-length tasks |
| **Stepped** | `26396-14991` | Track divided into `stepCount` equal segments; each segment shows 0–100% fill independently | Multi-step forms, onboarding flows, checkout pipelines |

### By Style (Both Variants)

| Style | Node (Continuous) | Fill Color | Semantic Use |
|-------|------------------|-----------|-------------|
| **Info** (default) | `185:5388` | Deep blue (`status.informative`) | General progress, data loading |
| **Positive** | `185:5390` | Dark green (`status.positive`) | Success confirmation, completed steps |
| **Negative** | `185:5392` | Dark red / maroon (`status.negative`) | Error progress, failed processes |
| **Warning** | `185:5394` | Amber / gold (`status.caution`) | Caution, quota nearing limit |
| **Special** | `185:5396` | Dark navy / indigo (`status.special`) | Premium features, special states |
| **Neutral** | `184:5385` | Black (`content.base`) | Neutral / no semantic connotation |
| **Brand** | `185:5429` | Brand blue (`brand.primary`) | Brand-aligned flows, PayPal features |

---

## Anatomy

### Continuous Progress Bar

```
Track (full width, light grey)
├─ [████████████░░░░░░░░░░░░░░░░░░] ← Fill (colored, left-aligned)
└─ [░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░] ← Remaining (unfilled track)
```

### Stepped Progress Bar (`stepCount=3`, `currentStep=2`, `progress=0.5`)

```
Step 1 (complete)    Step 2 (in progress)     Step 3 (not started)
[████████████████]  [████████░░░░░░░░]  [░░░░░░░░░░░░░░░░]
     100%                ~50%                   0%
```

### Sub-Component: `Progress.Item` (node `26399:2079`)

A single step segment used inside the Stepped variant. Five progress states:

| State | Node | Description |
|-------|------|-------------|
| `Progress=0%` | `26399:2084` | Fully empty — step not started |
| `Progress=25%` | `26399:2080` | Quarter fill |
| `Progress=50%` | `26396:16109` | Half fill |
| `Progress=75%` | `26396:16111` | Three-quarter fill |
| `Progress=100%` | `26399:2085` | Fully complete |

Each segment: **64 × 8 px** with 20 px spacing between segments.

---

## Size & Spacing Specifications

### Bar Height

| Element | Value |
|---------|-------|
| Track height | 8 px |
| Fill height | 8 px (matches track) |
| Border radius | `tokens.base.border.radius.full` (999 px — fully rounded pill) |

### Stepped Segment Dimensions

| Property | Value |
|----------|-------|
| Segment width | 64 px (base unit; scales proportionally in fluid layouts) |
| Segment height | 8 px |
| Gap between segments | ~4–8 px |
| Segment border radius | `tokens.base.border.radius.full` (pill shape per segment) |

### Container

| Property | Value |
|----------|-------|
| Width | 100% of parent (fluid) |
| Height | 8 px |
| Overflow | hidden |

---

## Style Variant Color Tokens

| Style | Fill token | Default value |
|-------|-----------|---------------|
| Info | `tokens.colorScheme.status.informative` | Deep blue |
| Positive | `tokens.colorScheme.status.positive` | Dark green |
| Negative | `tokens.colorScheme.status.negative` | Dark red / maroon |
| Warning | `tokens.colorScheme.status.caution` | Amber / gold |
| Special | `tokens.colorScheme.status.special` | Dark navy |
| Neutral | `tokens.colorScheme.content.base` | Black |
| Brand | `tokens.colorScheme.brand.primary` | Brand blue |
| Track (all) | `tokens.colorScheme.background.utility.unselected` | Light grey |

---

## State Specifications

### 0% (Not Started)
- Fill width is 0; only the track is visible
- `aria-valuenow="0"` on the `progressbar` role element

### In Progress (1–99%)
- Fill width is proportional to `progressValue / maxValue`
- `aria-valuenow` reflects the current numeric value
- Transition animation plays on value change

### 100% (Complete)
- Fill covers the full track width
- `aria-valuenow="100"` (or `aria-valuenow=maxValue` for non-normalized scales)
- Optional: `style="Positive"` swap to communicate success

### Indeterminate
- When `progressValue` is `undefined` or `null`, render an indeterminate animation (sliding shimmer)
- `aria-valuenow` is omitted; `aria-valuemin` and `aria-valuemax` remain
- Used for operations with unknown duration (e.g., server-side processing)

### Stepped — Step Complete
- Segment fill is 100%, fully colored
- Visual: full-width colored pill

### Stepped — Step In Progress
- Segment fill is `progress` (0.0–1.0) of that segment's width
- Current active segment

### Stepped — Step Not Started
- Segment fill is 0%; only grey track visible

---

## Props API

### TypeScript Interface

```typescript
// ─── Shared ──────────────────────────────────────────────────────────────────

export type ProgressBarStyle =
  | 'info'
  | 'positive'
  | 'negative'
  | 'warning'
  | 'special'
  | 'neutral'
  | 'brand';

// ─── ProgressBar (Continuous) ─────────────────────────────────────────────────

export interface ProgressBarProps {
  /**
   * Current progress value.
   * When undefined, renders as indeterminate (animated shimmer).
   * @default undefined
   */
  value?: number;

  /**
   * Minimum value of the progress range.
   * @default 0
   */
  min?: number;

  /**
   * Maximum value of the progress range.
   * @default 100
   */
  max?: number;

  /**
   * Semantic color style of the progress fill.
   * @default 'info'
   */
  style?: ProgressBarStyle;

  /**
   * Accessible label describing what is being measured.
   * Example: "Uploading file", "Profile completion"
   * Required unless the progress bar is within a region that already provides context.
   */
  'aria-label'?: string;

  /**
   * ID of an element that labels the progress bar.
   * Alternative to aria-label when a visible label exists.
   */
  'aria-labelledby'?: string;

  /**
   * Human-readable text representation of the current value.
   * Defaults to the numeric percentage (e.g., "45%").
   * Override for custom formats: "Step 2 of 5", "450 MB of 1 GB"
   */
  'aria-valuetext'?: string;

  /**
   * Additional class name(s) applied to the root element.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── ProgressBarStepped ───────────────────────────────────────────────────────

export interface ProgressBarSteppedProps {
  /**
   * Total number of steps (segments) in the progress bar.
   */
  stepCount: number;

  /**
   * Zero-based index of the current active step.
   * Steps before currentStep are rendered as 100% complete.
   * @default 0
   */
  currentStep?: number;

  /**
   * Progress (0.0–1.0) within the current active step.
   * 0.0 = step just started; 1.0 = step complete (advance currentStep).
   * @default 0
   */
  progress?: number;

  /**
   * Semantic color style of the filled segments.
   * @default 'info'
   */
  style?: ProgressBarStyle;

  /**
   * Accessible label for the stepped progress bar.
   * Example: "Checkout progress"
   */
  'aria-label'?: string;

  /**
   * ID of an element that labels the stepped progress bar.
   */
  'aria-labelledby'?: string;

  /**
   * Human-readable description of current position.
   * Defaults to "Step [n] of [total]".
   * Override for custom text: "2 of 4 steps complete"
   */
  'aria-valuetext'?: string;

  /**
   * Additional class name(s) applied to the root element.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}
```

---

## Code Examples

### 1. Basic Continuous Progress Bar

```tsx
import { ProgressBar } from '@ds/components';

export function FileUploadProgress({ uploadedBytes, totalBytes }) {
  const percentage = Math.round((uploadedBytes / totalBytes) * 100);

  return (
    <div>
      <div className="flex justify-between mb-2">
        <span>Uploading file…</span>
        <span>{percentage}%</span>
      </div>
      <ProgressBar
        value={percentage}
        min={0}
        max={100}
        style="info"
        aria-label="File upload progress"
        aria-valuetext={`${percentage}% uploaded`}
      />
    </div>
  );
}
```

### 2. All Seven Style Variants

```tsx
import { ProgressBar } from '@ds/components';

const STYLES = ['info', 'positive', 'negative', 'warning', 'special', 'neutral', 'brand'] as const;

export function StyleShowcase() {
  return (
    <div className="flex flex-col gap-4">
      {STYLES.map(style => (
        <div key={style}>
          <label className="text-sm capitalize mb-1 block">{style}</label>
          <ProgressBar
            value={50}
            style={style}
            aria-label={`${style} progress bar at 50%`}
          />
        </div>
      ))}
    </div>
  );
}
```

### 3. Indeterminate Loading State

```tsx
import { ProgressBar } from '@ds/components';

export function PageLoading() {
  return (
    <div>
      <p className="sr-only" aria-live="polite">Loading your transactions…</p>
      <ProgressBar
        style="brand"
        aria-label="Loading transactions"
        // No value prop = indeterminate
      />
    </div>
  );
}
```

### 4. Progress Bar with Visible Label and Percentage

```tsx
import { ProgressBar } from '@ds/components';

export function ProfileCompletion({ completionPct }: { completionPct: number }) {
  const isComplete = completionPct >= 100;

  return (
    <div>
      <div className="flex justify-between items-center mb-2">
        <span id="profile-progress-label" className="font-medium">
          Profile completion
        </span>
        <span
          className={isComplete ? 'text-positive' : 'text-muted'}
          aria-live="polite"
        >
          {isComplete ? 'Complete!' : `${completionPct}%`}
        </span>
      </div>
      <ProgressBar
        value={completionPct}
        style={isComplete ? 'positive' : 'brand'}
        aria-labelledby="profile-progress-label"
        aria-valuetext={isComplete ? 'Profile complete' : `${completionPct}% complete`}
      />
    </div>
  );
}
```

### 5. Stepped Progress — Checkout Flow

```tsx
import { ProgressBarStepped } from '@ds/components';

const CHECKOUT_STEPS = ['Cart', 'Shipping', 'Payment', 'Review'];

export function CheckoutProgress({
  currentStep,
  stepProgress,
}: {
  currentStep: number;
  stepProgress: number; // 0.0–1.0 within the current step
}) {
  return (
    <div>
      {/* Step labels */}
      <div className="flex justify-between mb-2">
        {CHECKOUT_STEPS.map((label, index) => (
          <span
            key={label}
            className={index < currentStep ? 'text-positive' : index === currentStep ? 'font-medium' : 'text-muted'}
            aria-current={index === currentStep ? 'step' : undefined}
          >
            {label}
          </span>
        ))}
      </div>

      {/* Stepped progress bar */}
      <ProgressBarStepped
        stepCount={CHECKOUT_STEPS.length}
        currentStep={currentStep}
        progress={stepProgress}
        style="brand"
        aria-label="Checkout progress"
        aria-valuetext={`Step ${currentStep + 1} of ${CHECKOUT_STEPS.length}: ${CHECKOUT_STEPS[currentStep]}`}
      />
    </div>
  );
}
```

### 6. Stepped Progress — Onboarding

```tsx
import { ProgressBarStepped } from '@ds/components';

export function OnboardingProgress({ totalSteps, currentStep }) {
  return (
    <div className="flex flex-col gap-1">
      <p className="text-sm text-muted text-right">
        {currentStep + 1} of {totalSteps}
      </p>
      <ProgressBarStepped
        stepCount={totalSteps}
        currentStep={currentStep}
        progress={0}
        style="info"
        aria-label="Onboarding progress"
        aria-valuetext={`Step ${currentStep + 1} of ${totalSteps}`}
      />
    </div>
  );
}
```

### 7. Storage Quota Warning (Warning Style)

```tsx
import { ProgressBar } from '@ds/components';

export function StorageUsage({ usedGB, totalGB }) {
  const usedPct = (usedGB / totalGB) * 100;
  const isNearLimit = usedPct >= 80;
  const isAtLimit   = usedPct >= 95;

  const barStyle = isAtLimit ? 'negative' : isNearLimit ? 'warning' : 'info';

  return (
    <div>
      <div className="flex justify-between mb-2">
        <span id="storage-label">Storage</span>
        <span>{usedGB} GB of {totalGB} GB used</span>
      </div>
      <ProgressBar
        value={usedPct}
        style={barStyle}
        aria-labelledby="storage-label"
        aria-valuetext={`${usedGB} GB of ${totalGB} GB used`}
      />
      {isNearLimit && (
        <p className="text-warning text-sm mt-1">
          You're running low on storage.
        </p>
      )}
    </div>
  );
}
```

### 8. Dynamic Style Change on Completion

```tsx
import { useState, useEffect } from 'react';
import { ProgressBar } from '@ds/components';

export function ProgressWithCompletion({ task }) {
  const [progress, setProgress] = useState(0);

  useEffect(() => {
    const unsub = task.onProgress(setProgress);
    return unsub;
  }, [task]);

  const isComplete = progress >= 100;
  const hasFailed  = task.status === 'error';

  return (
    <div aria-live="polite" aria-label={task.label}>
      <ProgressBar
        value={isComplete || hasFailed ? 100 : progress}
        style={hasFailed ? 'negative' : isComplete ? 'positive' : 'info'}
        aria-label={task.label}
        aria-valuetext={
          hasFailed   ? `${task.label} failed` :
          isComplete  ? `${task.label} complete` :
          `${Math.round(progress)}% complete`
        }
      />
    </div>
  );
}
```

### 9. Stepped Bar with Step-Level Status Styles

```tsx
import { ProgressBarStepped } from '@ds/components';

// Style each step individually based on its completion status
type StepStatus = 'complete' | 'in-progress' | 'error' | 'pending';

const STATUS_TO_STYLE: Record<StepStatus, string> = {
  complete:    'positive',
  'in-progress': 'info',
  error:       'negative',
  pending:     'neutral',
};

export function VerificationSteps({ steps }: { steps: { label: string; status: StepStatus }[] }) {
  // Find the current (in-progress or error) step
  const currentStepIndex = steps.findIndex(
    s => s.status === 'in-progress' || s.status === 'error'
  );
  const activeStyle = currentStepIndex >= 0
    ? STATUS_TO_STYLE[steps[currentStepIndex].status]
    : 'positive';

  return (
    <ProgressBarStepped
      stepCount={steps.length}
      currentStep={currentStepIndex >= 0 ? currentStepIndex : steps.length - 1}
      progress={currentStepIndex >= 0 ? 0.5 : 1}
      style={activeStyle as any}
      aria-label="Identity verification progress"
      aria-valuetext={`${steps.filter(s => s.status === 'complete').length} of ${steps.length} steps complete`}
    />
  );
}
```

### 10. Real-Time Upload with Bytes Label

```tsx
import { useState } from 'react';
import { ProgressBar } from '@ds/components';

function formatBytes(bytes: number): string {
  if (bytes < 1024)       return `${bytes} B`;
  if (bytes < 1024 ** 2)  return `${(bytes / 1024).toFixed(1)} KB`;
  return `${(bytes / 1024 ** 2).toFixed(1)} MB`;
}

export function MultiFileUpload({ files }) {
  const [progress, setProgress] = useState<Record<string, number>>({});

  return (
    <ul className="flex flex-col gap-3">
      {files.map(file => {
        const pct = progress[file.id] ?? 0;
        return (
          <li key={file.id}>
            <div className="flex justify-between text-sm mb-1">
              <span>{file.name}</span>
              <span>{formatBytes(file.size * pct / 100)} / {formatBytes(file.size)}</span>
            </div>
            <ProgressBar
              value={pct}
              style={pct >= 100 ? 'positive' : 'brand'}
              aria-label={`Uploading ${file.name}`}
              aria-valuetext={`${Math.round(pct)}% of ${file.name} uploaded`}
            />
          </li>
        );
      })}
    </ul>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern — `progressbar` Role

The Progress Bar implements the [ARIA progressbar role](https://www.w3.org/WAI/ARIA/apg/patterns/meter/):

```html
<!-- Determinate -->
<div
  role="progressbar"
  aria-label="File upload progress"
  aria-valuemin="0"
  aria-valuemax="100"
  aria-valuenow="45"
  aria-valuetext="45% uploaded"
>
  <div class="progress-fill" style="width: 45%;" />
</div>

<!-- Indeterminate -->
<div
  role="progressbar"
  aria-label="Loading"
  aria-valuemin="0"
  aria-valuemax="100"
  <!-- aria-valuenow omitted for indeterminate -->
>
  <div class="progress-fill progress-fill--indeterminate" />
</div>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Root element | `"progressbar"` |
| `aria-valuemin` | Root element | `"0"` (or min prop) |
| `aria-valuemax` | Root element | `"100"` (or max prop) |
| `aria-valuenow` | Root element | Current numeric value (omit for indeterminate) |
| `aria-valuetext` | Root element | Human-readable value string |
| `aria-label` | Root element | Label for the progress bar |
| `aria-labelledby` | Root element | ID of a visible label element |
| `aria-live` | Status region | `"polite"` — for announcing completion |

### Stepped Variant ARIA

The stepped progress bar uses a single `progressbar` role on the container, not one per segment:

```html
<div
  role="progressbar"
  aria-label="Checkout progress"
  aria-valuemin="0"
  aria-valuemax="4"
  aria-valuenow="2"
  aria-valuetext="Step 2 of 4: Payment"
>
  <!-- 4 visual segments -->
</div>
```

### Screen Reader Announcements

- The `aria-valuetext` attribute is the most important accessibility prop — use it to provide meaningful context rather than just a number
- Prefer `"Step 2 of 5: Shipping"` over `"40%"` for stepped bars
- Prefer `"450 MB of 1 GB used"` over `"45%"` for storage bars
- Announce completion in a live region so screen readers read it without the user navigating there:

```tsx
<div aria-live="polite" aria-atomic="true" className="sr-only">
  {progress >= 100 ? 'Upload complete.' : ''}
</div>
```

### Keyboard Interaction

The Progress Bar is not interactive — it receives no keyboard focus (`tabindex` is not set). It communicates status passively via ARIA attributes read by screen readers.

### Color and Non-Color Differentiation

Progress status must not be communicated by color alone (WCAG 1.4.1). Always pair color with:
- A text label or percentage near the bar
- `aria-valuetext` describing the semantic state
- An icon or supplemental message for critical states (negative/warning)

### Color Contrast

| Element | Minimum Ratio | WCAG |
|---------|---------------|------|
| Progress fill on track | 3:1 | 1.4.11 Non-text Contrast |
| Adjacent label text | 4.5:1 | 1.4.3 Contrast (Minimum) |
| Percentage / status text | 4.5:1 | 1.4.3 Contrast (Minimum) |

---

## Animation Specifications

### Fill Transition (Determinate)

```css
.progress-fill {
  transition: width 400ms cubic-bezier(0.4, 0, 0.2, 1);
  /* Smooth ease-in-out for value updates */
}
```

### Indeterminate Animation (Shimmer)

```css
@keyframes progress-indeterminate {
  0%   { transform: translateX(-100%) scaleX(0.5); }
  50%  { transform: translateX(0%)    scaleX(0.8); }
  100% { transform: translateX(200%)  scaleX(0.5); }
}

.progress-fill--indeterminate {
  width: 50%;
  animation: progress-indeterminate 1.5s ease-in-out infinite;
  transform-origin: left center;
}
```

### Step Segment Fill Transition

```css
.progress-segment-fill {
  transition: width 300ms cubic-bezier(0.4, 0, 0.2, 1);
}
```

### Completion Pulse (Optional)

```css
@keyframes progress-complete-pulse {
  0%   { opacity: 1; }
  50%  { opacity: 0.7; }
  100% { opacity: 1; }
}

.progress-fill--complete {
  animation: progress-complete-pulse 600ms ease-in-out 1;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .progress-fill,
  .progress-segment-fill {
    transition: none;
  }

  .progress-fill--indeterminate {
    animation: none;
    width: 100%;
    opacity: 0.4;
    /* Static half-filled bar indicates loading without motion */
  }
}
```

---

## Design Tokens

```css
/* ─── Track ───────────────────────────────────────────────────────────── */
--progress-track-height:       8px;
--progress-track-radius:       var(--tokens.base.border.radius.full);   /* 999px */
--progress-track-color:        var(--tokens.colorScheme.background.utility.unselected);

/* ─── Fill — Semantic Styles ──────────────────────────────────────────── */
--progress-fill-info:          var(--tokens.colorScheme.status.informative);
--progress-fill-positive:      var(--tokens.colorScheme.status.positive);
--progress-fill-negative:      var(--tokens.colorScheme.status.negative);
--progress-fill-warning:       var(--tokens.colorScheme.status.caution);
--progress-fill-special:       var(--tokens.colorScheme.status.special);
--progress-fill-neutral:       var(--tokens.colorScheme.content.base);
--progress-fill-brand:         var(--tokens.colorScheme.brand.primary);

/* ─── Stepped Segment ─────────────────────────────────────────────────── */
--progress-segment-height:     8px;
--progress-segment-base-width: 64px;
--progress-segment-gap:        4px;
--progress-segment-radius:     var(--tokens.base.border.radius.full);   /* 999px */

/* ─── Animation ───────────────────────────────────────────────────────── */
--progress-transition-duration:      400ms;
--progress-transition-easing:        cubic-bezier(0.4, 0, 0.2, 1);
--progress-indeterminate-duration:   1500ms;
```

---

## Best Practices

### Do

- **Always provide `aria-label` or `aria-labelledby`** — A progress bar with no accessible name tells screen reader users nothing about what is loading or progressing.
- **Provide a meaningful `aria-valuetext`** — `"Step 2 of 5: Address"` is far more useful than `"40%"`. Craft the text to match the user's mental model of the task.
- **Use the style variant that matches the semantic** — A successful completion should switch to `positive` (green); an error should switch to `negative` (red). Don't leave an error state showing the default blue `info` bar.
- **Use the Stepped variant for discrete multi-step flows** — When a task has named steps (Cart → Shipping → Payment → Review), the Stepped variant communicates both overall progress and per-step progress.
- **Use the Continuous variant for non-discrete progress** — File uploads, page loads, and quota meters don't have named steps; use Continuous.
- **Pair the bar with a visible text label** — The bar conveys proportion; the label conveys context. Both together are significantly clearer than either alone.
- **Animate value changes** — A smooth transition from the old value to the new communicates that progress is real. An instant jump can feel like a glitch.

### Don't

- **Don't use color as the only progress indicator** — Pair every style change with text or `aria-valuetext` that conveys the same meaning.
- **Don't use the Progress Bar for navigation** — Progress Bars are read-only status indicators. For step navigation, use a Stepper component that allows clicking back to a previous step.
- **Don't show a 0% bar if the process hasn't started** — Consider not rendering the bar until the process has actually begun, or use a skeleton/Loader until the first progress event fires.
- **Don't leave an indeterminate bar running indefinitely** — If a process runs for more than 10–15 seconds without reporting progress, show a time estimate or switch to a message. Indefinite spinners/bars erode trust.
- **Don't show raw bytes or decimals by default** — `"450.3 MB / 1024.0 MB"` is harder to parse than `"450 MB of 1 GB"`. Round to sensible units.
- **Don't use the Stepped variant for tasks with more than ~6 steps** — Beyond 6 segments, individual segments become too narrow to read. Use a numeric label ("Step 3 of 8") instead.

---

## Progress Bar vs. Related Indicators

| Pattern | When to Use |
|---------|------------|
| **ProgressBar — Continuous** | File upload, page load, quota meter, any process with a measurable 0–100% value |
| **ProgressBar — Stepped** | Multi-step checkout, onboarding, form wizard with 2–6 named steps |
| **Pagination Dots** | Carousel/slide position indicator (not progress — no "completion" concept) |
| **Loader (Spinner)** | When progress value is completely unknown and duration is short (<5 s) |
| **Stepper** | Multi-step form where the user can navigate back to completed steps; steps have visible labels |
| **Skeleton** | Content shape is known; the surface is loading, not a specific task |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Loader** | Alternative for short, indeterminate loading (no progress value known) |
| **Stepper** | Interactive multi-step companion; Progress Bar is the passive indicator version |
| **Contextual Alert** | Shown alongside the bar for error or warning states needing explanation |
| **Banner** | Page-level message when upload/download completes or fails |
| **Toast** | Transient completion notification after bar reaches 100% |
| **Chips / Badge** | Step status indicators for completed/active steps in a Stepper |
| **Button** | "Cancel" action adjacent to an upload progress bar |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Added `ProgressBarStepped` variant with `stepCount`, `currentStep`, `progress` props; extracted `Progress.Item` as a reusable segment sub-component; standardised all 7 style variants across both variants |
| 5.1 | Added `Brand` and `Special` style variants |
| 5.0 | Added `indeterminate` mode (no value prop); added `aria-valuetext` prop |
| 4.0 | Initial release with Continuous bar and Info/Positive/Negative/Warning/Neutral styles |

---

## Testing Checklist

### Visual
- [ ] Track renders at 8 px height with full border radius (pill shape)
- [ ] Fill renders at 8 px height, left-aligned, with correct width for `value`
- [ ] All 7 style variant fill colors render correctly
- [ ] Track color is `tokens.colorScheme.background.utility.unselected` (light grey)
- [ ] Stepped variant renders `stepCount` equal-width segments with gaps between
- [ ] Stepped: completed steps (before `currentStep`) are fully filled
- [ ] Stepped: `currentStep` segment is filled to `progress` proportion
- [ ] Stepped: steps after `currentStep` are empty (grey track only)
- [ ] Indeterminate (no value) shows animated shimmer

### Behavior
- [ ] Fill width updates smoothly with a CSS transition when `value` changes
- [ ] Stepped fill updates per-segment when `currentStep` or `progress` changes
- [ ] Switching to `value={100}` fills the bar completely
- [ ] Switching to `style="positive"` from `style="info"` changes fill color correctly
- [ ] Indeterminate animation loops continuously when no value is provided

### Accessibility
- [ ] Root element has `role="progressbar"`
- [ ] `aria-valuemin` and `aria-valuemax` are set correctly
- [ ] `aria-valuenow` matches the current numeric value (determinate)
- [ ] `aria-valuenow` is absent for indeterminate mode
- [ ] `aria-label` or `aria-labelledby` is present on the root element
- [ ] `aria-valuetext` provides a meaningful human-readable description
- [ ] Screen reader announces the label and value when navigated to
- [ ] Completion is announced via `aria-live="polite"` region
- [ ] Progress fill vs. track meets 3:1 non-text contrast ratio

### Reduced Motion
- [ ] Fill transition is suppressed under `prefers-reduced-motion: reduce`
- [ ] Indeterminate shimmer animation stops; static partial fill shown instead

### Integration
- [ ] `value={0}` renders an empty bar (no fill)
- [ ] `value={100}` renders a completely filled bar
- [ ] `value` outside 0–100 is clamped gracefully
- [ ] `stepCount` changes re-render the correct number of segments
- [ ] `data-testid` is forwarded to the root element
