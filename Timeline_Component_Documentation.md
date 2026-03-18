# Timeline Component Documentation

**Component:** Timeline
**Version:** 6.0 Beta
**Figma Nodes:** `31349-855` (Vertical), `31349-457` (Horizontal), `31349-526` (Parts)
**Last Updated:** 2026-03-18
**Status:** Beta

---

## Table of Contents

1. [Overview](#overview)
2. [Component Variants](#component-variants)
3. [Anatomy](#anatomy)
4. [Size & Spacing Specifications](#size--spacing-specifications)
5. [Style Variants](#style-variants)
6. [State Specifications](#state-specifications)
7. [Props API](#props-api)
8. [Code Examples](#code-examples)
9. [Accessibility Specifications](#accessibility-specifications)
10. [Animation Specifications](#animation-specifications)
11. [Design Tokens](#design-tokens)
12. [Best Practices](#best-practices)
13. [Related Components](#related-components)
14. [Version History](#version-history)
15. [Testing Checklist](#testing-checklist)

---

## Overview

The **Timeline** component displays a sequence of steps or events along a visual track, communicating progress through a multi-step process or chronological history. Each item in the timeline carries a status indicator (Not Started, Started, or Completed), a connecting track between steps, and structured text content (title, description, metadata). The component ships in two layout orientations: **Vertical** and **Horizontal**.

### When to Use

- Showing the progress of a multi-step process (e.g., order tracking, onboarding flow, application status)
- Displaying a chronological history of events (e.g., transaction history, activity log, version history)
- Communicating where a user currently is within a sequential workflow
- Providing a read-only step-progress overview at the top of a detailed process view

### When Not to Use

- When the user can interactively navigate between steps — use a Stepper component with navigation controls instead
- When the list of events is purely informational with no concept of progress state — use a plain List
- When there are only 2 steps — a single Divider or progress indicator may be sufficient
- When the number of steps is dynamic and unbounded — consider a paginated list

---

## Component Variants

### Layout Orientation

| Variant | Figma Node | Description |
|---|---|---|
| **Vertical** | `31349-855` | Steps stacked top-to-bottom; track runs along the left edge; text content to the right of each step indicator |
| **Horizontal** | `31349-457` | Steps laid out left-to-right; track runs along the top; text content below each step indicator |

### Step Status States

Both layout variants share the same three step status states, applied per item:

| Status | Figma Node | Icon | Connector Color |
|---|---|---|---|
| **Not Started** | `31349-504` | Hollow outline circle (`timeline-not-started`, 16×16px) | Muted on both sides (`border.base`) |
| **Started** | `31349-508` | Filled solid circle dot (`timeline-started`, 16×16px) | Dark before + muted after |
| **Completed** | `31349-512` | Filled circle with checkmark (`check-circle-fill`, 16×16px) | Dark on both sides |

---

## Anatomy

### Vertical Timeline

```
  ○ ─ Label                   ← Not Started: hollow circle, muted track
  │   Description
  │   Metadata
  │
  ● ─ Label                   ← Started: filled dot, dark track above, muted below
  │   Description
  │   Metadata
  │
  ✓ ─ Label                   ← Completed: check-circle-fill, dark track both sides
      Description
      Metadata
```

**Parts (Vertical Item):**

| Part | Element | Description |
|---|---|---|
| **Step indicator** | `<span>` with icon | 16×16px icon indicating step status (circle outline / filled dot / check-circle) |
| **Connector line (top)** | `<div>` border | 2px vertical line above the indicator; connects to the previous step |
| **Connector line (bottom)** | `<div>` border | 2px vertical line below the indicator; connects to the next step |
| **Title** | `<span>` Label/Large | Step title; 16px, Medium weight |
| **Description** | `<span>` Body/Medium | Supporting description text; 14px, Regular |
| **Metadata** | `<span>` Body/Small | Supplementary metadata (date, time, label); 12px, Regular, muted color |

### Horizontal Timeline

```
  ──○──  ──●──  ──✓──  ──○──    ← Tracker row (connectors + indicators)
  Label  Label  Label  Label
  Desc.  Desc.  Desc.  Desc.
  Meta   Meta   Meta   Meta
```

**Parts (Horizontal Tracker):**

| Part | Element | Description |
|---|---|---|
| **Left connector** | `<div>` border (h-[2px], flex: 1) | Horizontal line to the left of the step indicator |
| **Step indicator** | `<span>` with icon | 16×16px icon; centered between connectors |
| **Right connector** | `<div>` border (h-[2px], flex: 1) | Horizontal line to the right of the step indicator |
| **Title** | `<span>` Label/Large | Step title below the tracker |
| **Description** | `<span>` Body/Medium | Supporting description |
| **Metadata** | `<span>` Body/Small | Muted supplementary text |

---

## Size & Spacing Specifications

### Step Indicator

| Property | Value | Token |
|---|---|---|
| Icon size | 16×16px | `--timeline-icon-size: 16px` |
| Icon type — Not Started | `timeline-not-started` (hollow circle outline) | — |
| Icon type — Started | `timeline-started` (filled solid circle/dot) | — |
| Icon type — Completed | `check-circle-fill` (filled circle + checkmark) | — |

### Connector Lines

| Property | Value | Token |
|---|---|---|
| Connector thickness | 2px | `--timeline-connector-width: 2px` |
| Connector style | solid border | — |
| Connector color — completed / emphasis side | `tokens.colorScheme.border.role.emphasis.neutral` (near-black) | `--timeline-connector-emphasis` |
| Connector color — pending / base side | `tokens.colorScheme.border.base` (rgba(0,0,0,0.16) — light muted grey) | `--timeline-connector-base` |
| Connector gap from icon | 4px (`gap-[4px]`) | `--timeline-connector-gap: 4px` |

### Horizontal Tracker Item

| Property | Value |
|---|---|
| Tracker row height | 40px |
| Tracker item width (per item) | 88px (connector + icon + connector, flex-distributed) |
| Item container width | 92.5px (per `Timeline.Horizontal.Item` node) |
| Item container height | 118px (tracker row 40px + text content ~78px) |

### Vertical Item

| Property | Value |
|---|---|
| Vertical item frame width | 370px |
| Vertical item frame height | 90px minimum |
| Track column width | ~20px (icon 16px + left padding) |
| Content column width | Remainder of container width |

### Typography

| Text Element | Style | Size | Weight | Token |
|---|---|---|---|---|
| Title | Label/Large | 16px | 500 (Medium) | `tokens.brand.text.fontfamily.label` + `fontsize.ramp[16]` + `fontweight.label` |
| Description | Body/Medium | 14px | 400 (Regular) | `tokens.brand.text.fontfamily.body` + `fontsize.ramp[14]` + `fontweight.body` |
| Metadata | Body/Small | 12px | 400 (Regular) | `tokens.brand.text.fontfamily.body` + `fontsize.ramp[12]` + `fontweight.body` |

### Text Spacing

| Property | Value |
|---|---|
| Title → Description gap | 2px |
| Description → Metadata gap | 2px |
| Content block → next item gap (vertical) | Part of the 90px item height; connector fills remaining space |

---

## Style Variants

### Connector Color Logic

The connector line color communicates which side of a step is "done" vs. "pending":

| Connector Position | Condition | Color |
|---|---|---|
| Before **Completed** step | Both sides | Dark (`border.role.emphasis.neutral`) |
| Before **Started** step | Left/above | Dark (`border.role.emphasis.neutral`) |
| After **Started** step | Right/below | Muted (`border.base`) |
| Before **Not Started** step | Both sides | Muted (`border.base`) |
| First item (no left/top connector) | — | No connector rendered |
| Last item (no right/bottom connector) | — | No connector rendered |

### Step Indicator Icon

| Status | Visual | Fill | Stroke |
|---|---|---|---|
| Not Started | Empty circle | None | `border.base` (muted grey outline) |
| Started | Solid dot | `border.role.emphasis.neutral` (dark/black) | None |
| Completed | Filled circle with checkmark | `border.utility.emphasis` (dark/black) | None |

---

## State Specifications

### Not Started

- **Icon:** `timeline-not-started` — thin hollow circle outline (16×16px)
- **Connectors:** Both left/top and right/bottom use `border.base` (rgba(0,0,0,0.16)) — muted grey
- **Title color:** `--color-text-default`
- **Description color:** `--color-text-secondary`
- **Metadata color:** `--color-text-muted`

### Started (Current Step)

- **Icon:** `timeline-started` — filled solid dark circle/dot (16×16px)
- **Left/above connector:** `border.role.emphasis.neutral` (near-black/dark) — the "done" side
- **Right/below connector:** `border.base` (muted grey) — the "pending" side
- **Title color:** `--color-text-default` (same; visually distinguished by the solid dot icon)
- **Description color:** `--color-text-secondary`
- **Metadata color:** `--color-text-muted`

### Completed

- **Icon:** `check-circle-fill` — filled dark circle with white checkmark (16×16px)
- **Connectors:** Both sides use emphasis color (`border.role.emphasis.neutral` / `border.utility.emphasis`) — dark connectors confirming full completion
- **Title color:** `--color-text-default`
- **Description color:** `--color-text-secondary`
- **Metadata color:** `--color-text-muted`

---

## Props API

### Web / React

#### `Timeline` (container)

```typescript
interface TimelineProps {
  /**
   * Layout orientation.
   * - 'vertical'   → track on the left, content on the right (default)
   * - 'horizontal' → track on the top, content below
   * @default 'vertical'
   */
  orientation?: 'vertical' | 'horizontal';

  /** Timeline items. Should be an array of `Timeline.Item` elements. */
  children: React.ReactNode;

  /** Additional CSS class for the timeline container. */
  className?: string;

  /** Inline style overrides. */
  style?: React.CSSProperties;
}
```

#### `Timeline.Item` (individual step)

```typescript
type TimelineStatus = 'not-started' | 'started' | 'completed';

interface TimelineItemProps {
  /**
   * The progress status of this step.
   * Controls the step indicator icon and connector colors.
   * @default 'not-started'
   */
  status?: TimelineStatus;

  /**
   * Primary label / step title.
   * Rendered in Label/Large (16px, Medium weight).
   */
  title: string;

  /**
   * Supporting description text.
   * Rendered in Body/Medium (14px, Regular).
   */
  subtitle?: string;

  /**
   * Supplementary metadata (e.g., date, time, status label).
   * Rendered in Body/Small (12px, Regular, muted color).
   */
  metadata?: string;

  /**
   * Optional custom content rendered in the content area,
   * below the title/subtitle/metadata text block.
   */
  children?: React.ReactNode;

  /** Additional CSS class. */
  className?: string;
}
```

### Android — Jetpack Compose

#### Vertical Timeline

```kotlin
@Composable
fun Timeline(
    modifier: Modifier = Modifier,
    content: @Composable TimelineScope.() -> Unit
)

// Per item
fun TimelineScope.TimelineVerticalItem(
    title: String,
    subtitle: String = "",
    metadata: String = "",
    status: TimelineStatus = TimelineStatus.NotStarted
)

enum class TimelineStatus {
    NotStarted,
    Started,
    Completed
}
```

**Usage:**

```kotlin
Timeline {
    TimelineVerticalItem(
        title = "",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.Completed
    )
    TimelineVerticalItem(
        title = "",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.Started
    )
    TimelineVerticalItem(
        title = "",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.NotStarted
    )
}
```

#### Horizontal Timeline

```kotlin
Timeline {
    TimelineHorizontalItem(
        title = "Label",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.Completed
    )
    TimelineHorizontalItem(
        title = "Label",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.Started
    )
    TimelineHorizontalItem(
        title = "Label",
        subtitle = "Description",
        metadata = "Metadata",
        status = TimelineStatus.NotStarted
    )
}
```

---

## Code Examples

### 1. Basic Vertical Timeline

```tsx
import { Timeline } from '@design-system/components';

export function OrderStatusTimeline() {
  return (
    <Timeline orientation="vertical">
      <Timeline.Item
        status="completed"
        title="Order Placed"
        subtitle="Your order has been received"
        metadata="Mar 15, 2026 · 9:42 AM"
      />
      <Timeline.Item
        status="completed"
        title="Payment Confirmed"
        subtitle="Payment processed successfully"
        metadata="Mar 15, 2026 · 9:43 AM"
      />
      <Timeline.Item
        status="started"
        title="In Transit"
        subtitle="Your package is on its way"
        metadata="Mar 17, 2026 · 2:10 PM"
      />
      <Timeline.Item
        status="not-started"
        title="Delivered"
        subtitle="Estimated delivery by March 19"
        metadata="Est. Mar 19, 2026"
      />
    </Timeline>
  );
}
```

---

### 2. Horizontal Timeline (Step Progress)

```tsx
import { Timeline } from '@design-system/components';

export function ApplicationProgress() {
  return (
    <Timeline orientation="horizontal">
      <Timeline.Item
        status="completed"
        title="Applied"
        subtitle="Application submitted"
        metadata="Mar 1"
      />
      <Timeline.Item
        status="completed"
        title="Review"
        subtitle="Under review"
        metadata="Mar 5"
      />
      <Timeline.Item
        status="started"
        title="Interview"
        subtitle="Scheduled for March 20"
        metadata="Mar 20"
      />
      <Timeline.Item
        status="not-started"
        title="Decision"
        subtitle="Awaiting outcome"
        metadata="TBD"
      />
    </Timeline>
  );
}
```

---

### 3. Timeline Driven by Status Data

```tsx
import { Timeline } from '@design-system/components';

type StepStatus = 'completed' | 'started' | 'not-started';

interface ProcessStep {
  id: string;
  title: string;
  subtitle: string;
  metadata: string;
  status: StepStatus;
}

function deriveStepStatuses(currentIndex: number, steps: Omit<ProcessStep, 'status'>[]): ProcessStep[] {
  return steps.map((step, i) => ({
    ...step,
    status:
      i < currentIndex ? 'completed'
      : i === currentIndex ? 'started'
      : 'not-started',
  }));
}

export function ProcessTimeline({ currentStep }: { currentStep: number }) {
  const steps = deriveStepStatuses(currentStep, [
    { id: '1', title: 'Identity Verified', subtitle: 'ID check complete', metadata: 'Completed' },
    { id: '2', title: 'Documents Uploaded', subtitle: 'Proof of address', metadata: 'Completed' },
    { id: '3', title: 'Account Review', subtitle: 'Being reviewed by our team', metadata: '1–2 business days' },
    { id: '4', title: 'Account Activated', subtitle: 'Ready to use', metadata: 'Pending' },
  ]);

  return (
    <Timeline orientation="vertical">
      {steps.map((step) => (
        <Timeline.Item
          key={step.id}
          status={step.status}
          title={step.title}
          subtitle={step.subtitle}
          metadata={step.metadata}
        />
      ))}
    </Timeline>
  );
}
```

---

### 4. All Completed (History View)

```tsx
import { Timeline } from '@design-system/components';

export function TransactionHistory() {
  const events = [
    { title: 'Payment Received',  subtitle: '$250.00 from John D.',  metadata: 'Today, 11:02 AM' },
    { title: 'Refund Issued',     subtitle: '$12.50 to Sarah K.',    metadata: 'Yesterday, 3:45 PM' },
    { title: 'Payment Received',  subtitle: '$89.99 from Mike R.',   metadata: 'Mar 16 · 8:20 AM' },
    { title: 'Withdrawal',        subtitle: '$500.00 to bank',       metadata: 'Mar 14 · 9:00 AM' },
  ];

  return (
    <Timeline orientation="vertical">
      {events.map((event, i) => (
        <Timeline.Item
          key={i}
          status="completed"
          title={event.title}
          subtitle={event.subtitle}
          metadata={event.metadata}
        />
      ))}
    </Timeline>
  );
}
```

---

### 5. Timeline with Custom Content Slot

```tsx
import { Timeline } from '@design-system/components';
import { Button } from '@design-system/components';

export function ActionableTimeline() {
  return (
    <Timeline orientation="vertical">
      <Timeline.Item
        status="completed"
        title="Profile Created"
        subtitle="Basic information saved"
        metadata="Mar 10, 2026"
      />
      <Timeline.Item
        status="started"
        title="Verify Email"
        subtitle="Check your inbox for a verification link"
        metadata="Pending"
      >
        {/* Custom content below the text block */}
        <Button variant="secondary" size="small" style={{ marginTop: 8 }}>
          Resend verification email
        </Button>
      </Timeline.Item>
      <Timeline.Item
        status="not-started"
        title="Add Payment Method"
        subtitle="Required to complete setup"
        metadata="Not started"
      />
    </Timeline>
  );
}
```

---

### 6. Controlled Progress (Stepper Pattern)

```tsx
import { useState } from 'react';
import { Timeline } from '@design-system/components';
import { Button } from '@design-system/components';

const STEPS = [
  { title: 'Personal Info',  subtitle: 'Name and contact details' },
  { title: 'Address',        subtitle: 'Your billing address' },
  { title: 'Payment',        subtitle: 'Card or bank account' },
  { title: 'Confirmation',   subtitle: 'Review and submit' },
];

export function SetupWizardProgress({ currentStep }: { currentStep: number }) {
  return (
    <Timeline orientation="vertical" aria-label="Setup progress">
      {STEPS.map((step, i) => (
        <Timeline.Item
          key={i}
          status={
            i < currentStep ? 'completed'
            : i === currentStep ? 'started'
            : 'not-started'
          }
          title={step.title}
          subtitle={step.subtitle}
          metadata={i < currentStep ? 'Done' : i === currentStep ? 'In progress' : ''}
        />
      ))}
    </Timeline>
  );
}
```

---

### 7. Horizontal Shipping Tracker

```tsx
import { Timeline } from '@design-system/components';

export function ShippingTracker({ stage }: { stage: number }) {
  const stages = [
    { title: 'Ordered',   subtitle: 'Confirmed',    metadata: 'Mar 15' },
    { title: 'Packed',    subtitle: 'Ready to ship', metadata: 'Mar 16' },
    { title: 'Shipped',   subtitle: 'In transit',   metadata: 'Mar 17' },
    { title: 'Delivered', subtitle: 'At your door', metadata: 'Mar 19' },
  ];

  return (
    <Timeline orientation="horizontal">
      {stages.map((s, i) => (
        <Timeline.Item
          key={i}
          status={
            i < stage ? 'completed'
            : i === stage ? 'started'
            : 'not-started'
          }
          title={s.title}
          subtitle={s.subtitle}
          metadata={s.metadata}
        />
      ))}
    </Timeline>
  );
}
```

---

### 8. Compose — Vertical Timeline with Mixed Status

```kotlin
@Composable
fun KYCTimeline() {
    Timeline {
        TimelineVerticalItem(
            title = "Identity Verified",
            subtitle = "Document scan complete",
            metadata = "Mar 10, 2026",
            status = TimelineStatus.Completed
        )
        TimelineVerticalItem(
            title = "Address Confirmed",
            subtitle = "Proof of address accepted",
            metadata = "Mar 12, 2026",
            status = TimelineStatus.Completed
        )
        TimelineVerticalItem(
            title = "Manual Review",
            subtitle = "Under review by compliance team",
            metadata = "1–3 business days",
            status = TimelineStatus.Started
        )
        TimelineVerticalItem(
            title = "Account Approved",
            subtitle = "Access to all features",
            metadata = "Pending",
            status = TimelineStatus.NotStarted
        )
    }
}
```

---

### 9. Compose — Horizontal Timeline (Onboarding Steps)

```kotlin
@Composable
fun OnboardingProgress(currentStep: Int) {
    val steps = listOf(
        Triple("Sign Up",  "Create your account", "Done"),
        Triple("Verify",   "Email confirmed",     "Done"),
        Triple("Profile",  "Add your details",    "In progress"),
        Triple("Done",     "Setup complete",      "Pending")
    )

    Timeline {
        steps.forEachIndexed { index, (title, subtitle, metadata) ->
            TimelineHorizontalItem(
                title = title,
                subtitle = subtitle,
                metadata = metadata,
                status = when {
                    index < currentStep -> TimelineStatus.Completed
                    index == currentStep -> TimelineStatus.Started
                    else -> TimelineStatus.NotStarted
                }
            )
        }
    }
}
```

---

### 10. Async Timeline (Data-Fetched Steps)

```tsx
import { useState, useEffect } from 'react';
import { Timeline } from '@design-system/components';
import { Shimmer } from '@design-system/components';

interface TimelineStep {
  id: string;
  title: string;
  subtitle: string;
  metadata: string;
  status: 'completed' | 'started' | 'not-started';
}

export function AsyncOrderTimeline({ orderId }: { orderId: string }) {
  const [steps, setSteps] = useState<TimelineStep[] | null>(null);

  useEffect(() => {
    fetchOrderSteps(orderId).then(setSteps);
  }, [orderId]);

  if (!steps) {
    return (
      <div role="status" aria-label="Loading order timeline" style={{ display: 'flex', flexDirection: 'column', gap: 16 }}>
        {[1, 2, 3, 4].map((i) => (
          <div key={i} style={{ display: 'flex', gap: 12 }}>
            <Shimmer variant="Round" width={16} height={16} />
            <div style={{ flex: 1, display: 'flex', flexDirection: 'column', gap: 4 }}>
              <Shimmer variant="Text" width="40%" height={14} />
              <Shimmer variant="Text" width="65%" height={12} />
              <Shimmer variant="Text" width="30%" height={10} />
            </div>
          </div>
        ))}
        <span className="sr-only">Loading order timeline…</span>
      </div>
    );
  }

  return (
    <Timeline orientation="vertical">
      {steps.map((step) => (
        <Timeline.Item
          key={step.id}
          status={step.status}
          title={step.title}
          subtitle={step.subtitle}
          metadata={step.metadata}
        />
      ))}
    </Timeline>
  );
}
```

---

## Accessibility Specifications

### Semantic Structure

The Timeline is a read-only display component. It represents ordered sequential information and should be marked up as an ordered list:

```html
<ol
  role="list"
  aria-label="Order status"
  class="timeline timeline--vertical"
>
  <li class="timeline-item" aria-current="step">
    <!-- "started" item gets aria-current="step" -->
    <span aria-hidden="true" class="timeline-icon timeline-icon--started"></span>
    <div class="timeline-content">
      <span class="timeline-title">In Transit</span>
      <span class="timeline-subtitle">Your package is on its way</span>
      <span class="timeline-metadata">Mar 17, 2026 · 2:10 PM</span>
    </div>
  </li>

  <li class="timeline-item">
    <span aria-hidden="true" class="timeline-icon timeline-icon--not-started"></span>
    <div class="timeline-content">
      <span class="timeline-title">Delivered</span>
      <span class="timeline-subtitle">Estimated delivery by March 19</span>
      <span class="timeline-metadata">Est. Mar 19, 2026</span>
    </div>
  </li>
</ol>
```

### Required ARIA Attributes

| Attribute | Element | Value | Purpose |
|---|---|---|---|
| `aria-label` or `aria-labelledby` | `<ol>` container | e.g., `"Order status"` | Names the timeline for screen readers |
| `aria-current="step"` | The currently active (`started`) item | `"step"` | Identifies the current step in the sequence |
| `aria-hidden="true"` | Step indicator icons | — | Hides decorative icons from AT; status communicated via text |

### Screen Reader Status Communication

The step indicator icons (hollow circle, filled dot, check-circle) are decorative and carry `aria-hidden="true"`. The status must be communicated through text — either visually visible text within the item or visually hidden text:

```html
<!-- Option A: Visually hidden status label -->
<li class="timeline-item" aria-current="step">
  <span aria-hidden="true" class="timeline-icon"></span>
  <div class="timeline-content">
    <span class="sr-only">Current step: </span>
    <span class="timeline-title">In Transit</span>
    …
  </div>
</li>

<!-- Option B: Status included in metadata text -->
<li class="timeline-item">
  <span class="timeline-title">Delivered</span>
  <span class="timeline-metadata">Not started · Est. Mar 19, 2026</span>
</li>
```

### Keyboard Navigation

The Timeline is a display-only component and contains no interactive elements by default. It is not focusable unless custom interactive elements (e.g., Buttons, Links) are placed within individual items via the `children` slot. If interactive, standard Tab navigation applies to those child elements.

If the Timeline is used as a step indicator alongside a navigable stepper, the step navigation controls (Previous / Next buttons) should be adjacent to the timeline, not embedded within it.

### Accessible Name for the Container

Always provide an accessible name for the `<ol>` container that reflects the context:

```html
<!-- Order tracking context -->
<ol aria-label="Order status timeline">

<!-- Onboarding context -->
<ol aria-labelledby="onboarding-heading">
<h2 id="onboarding-heading">Setup Progress</h2>
```

### Contrast Requirements

| Element | Minimum Contrast | Requirement |
|---|---|---|
| Title text vs. background | 4.5:1 (WCAG 1.4.3) | Label/Large 16px text |
| Description text vs. background | 4.5:1 | Body/Medium 14px text |
| Metadata text vs. background | 4.5:1 | Body/Small 12px text (≥ 4.5:1 even at small size) |
| Emphasis connector vs. background | 3:1 (non-text, WCAG 1.4.11) | Dark 2px connector line |
| Step indicator icon vs. background | 3:1 | 16×16px icon area |

---

## Animation Specifications

The Timeline is primarily a static display component. State transitions (e.g., a step moving from Not Started → Started → Completed) may animate when data updates:

### Connector Fill Transition

When a step transitions from Not Started to Completed, the connector line color can transition from muted to emphasis:

```css
.timeline-connector {
  transition: border-color 300ms ease;
}
```

### Icon Swap Transition

When the step indicator icon changes state, a subtle cross-fade prevents jarring visual jumps:

```css
.timeline-icon {
  transition: opacity 200ms ease;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .timeline-connector,
  .timeline-icon {
    transition: none;
  }
}
```

---

## Design Tokens

### Sizing Tokens

| Token | Value | Usage |
|---|---|---|
| `--timeline-icon-size` | `16px` | Step indicator icon diameter |
| `--timeline-connector-thickness` | `2px` | Connector line width/height |
| `--timeline-connector-gap` | `4px` | Gap between connector and icon |
| `--timeline-tracker-height` | `40px` | Horizontal tracker row height |

### Color Tokens

| Token | Value | Usage |
|---|---|---|
| `--timeline-connector-emphasis` | `tokens.colorScheme.border.role.emphasis.neutral` (near-black) | Connector before completed / on started left side |
| `--timeline-connector-base` | `tokens.colorScheme.border.base` (rgba(0,0,0,0.16)) | Connector after not-started / on started right side |
| `--timeline-icon-not-started` | `border.base` (muted outline) | Not started icon stroke |
| `--timeline-icon-started` | `border.role.emphasis.neutral` (dark fill) | Started icon fill |
| `--timeline-icon-completed` | `border.utility.emphasis` (dark fill) | Completed icon fill |

### Typography Tokens

| Token | Style | Value |
|---|---|---|
| `--timeline-title-font-family` | Label/Large | `tokens.brand.text.fontfamily.label` |
| `--timeline-title-font-size` | Label/Large | `16px` (ramp[16]) |
| `--timeline-title-font-weight` | Label/Large | `500` (Medium) |
| `--timeline-subtitle-font-family` | Body/Medium | `tokens.brand.text.fontfamily.body` |
| `--timeline-subtitle-font-size` | Body/Medium | `14px` (ramp[14]) |
| `--timeline-subtitle-font-weight` | Body/Medium | `400` (Regular) |
| `--timeline-metadata-font-family` | Body/Small | `tokens.brand.text.fontfamily.body` |
| `--timeline-metadata-font-size` | Body/Small | `12px` (ramp[12]) |
| `--timeline-metadata-font-weight` | Body/Small | `400` (Regular) |

---

## Best Practices

### Do

- **Map all steps to a status** — every item in the timeline must have an explicit `status` prop. Derive statuses programmatically from a `currentStep` index rather than hardcoding them.
- **Use `aria-current="step"`** on the currently active (`started`) item to identify it for screen readers.
- **Provide an accessible name** for the timeline container (`aria-label` or `aria-labelledby`).
- **Include meaningful metadata text** (dates, durations, status labels) — this provides context that sighted users can skim quickly and screen reader users depend on.
- **Use Shimmer placeholders** while timeline data is loading to preserve layout and prevent jarring content shifts.
- **Use Vertical orientation for long content or when items have multi-line descriptions** — horizontal timelines become crowded when text is lengthy.
- **Use Horizontal orientation for short, equally-weighted steps** with brief labels (1–2 words) and brief metadata.

### Don't

- **Don't mix Vertical and Horizontal items** within the same `<Timeline>` container — choose one orientation per timeline.
- **Don't rely solely on icon color or shape to communicate status** to users — always provide a text equivalent (visually hidden or visible metadata).
- **Don't show more than ~6–8 items in a horizontal timeline** — overflow is not handled and items will become too narrow.
- **Don't use Timeline as an interactive stepper** — it has no built-in Previous/Next controls. Pair it with navigation buttons placed outside the timeline if step-by-step interaction is needed.
- **Don't show a timeline with all items as Not Started** without any context — if the process hasn't begun, consider using an Empty State instead.
- **Don't omit the `title` prop** — it is the primary accessible and visual label for each step.

### Status Derivation Pattern

The recommended pattern is to derive all statuses from a single `currentStepIndex` integer rather than managing each item's status individually:

```typescript
function getStepStatus(
  index: number,
  current: number
): 'completed' | 'started' | 'not-started' {
  if (index < current)  return 'completed';
  if (index === current) return 'started';
  return 'not-started';
}
```

---

## Related Components

| Component | Relationship |
|---|---|
| **Progress Bar (Stepped)** | Use for a compact visual progress indicator when step labels and details are not needed |
| **Pagination** | Dot-based step indicator for carousel/slide content; not for process tracking |
| **Segmented Control / Tabs** | Use for navigable view switching; Timeline is display-only |
| **List** | Use for non-sequential, status-free content listings |
| **Shimmer** | Use as loading placeholder while timeline data is being fetched |
| **Loader** | Use for indeterminate loading states before the timeline can be rendered |

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Vertical + Horizontal layout orientations; three step statuses (Not Started / Started / Completed); `timeline-not-started`, `timeline-started`, `check-circle-fill` icons; 2px connectors with emphasis/base color distinction; Compose `TimelineVerticalItem` + `TimelineHorizontalItem` API; Label/Large + Body/Medium + Body/Small typography |

---

## Testing Checklist

### Visual

- [ ] **Vertical:** Track runs vertically on the left; text (title, subtitle, metadata) aligns to the right of each indicator
- [ ] **Horizontal:** Track runs horizontally on top; text content aligns below each indicator
- [ ] Not Started: hollow circle outline icon; muted (rgba(0,0,0,0.16)) connectors on both sides
- [ ] Started: filled solid dark dot; dark emphasis connector on left/above; muted connector on right/below
- [ ] Completed: `check-circle-fill` icon; dark emphasis connectors on both sides
- [ ] First item has no connector on the leading side; last item has no connector on the trailing side
- [ ] Connector thickness is 2px
- [ ] Icon size is 16×16px
- [ ] Title renders at 16px / 500 weight (Label/Large)
- [ ] Description renders at 14px / 400 weight (Body/Medium)
- [ ] Metadata renders at 12px / 400 weight (Body/Small), muted color

### Interaction

- [ ] Timeline is non-interactive (no hover states, no click handlers) unless custom children are provided
- [ ] Custom children (e.g., Buttons) in an item are interactive and receive focus correctly

### Keyboard

- [ ] Default timeline (no interactive children) is not in the Tab order
- [ ] If custom interactive children are present, they receive focus in document order
- [ ] No unexpected focus trapping within the timeline

### Screen Reader

- [ ] Timeline container is an `<ol>` with `aria-label` or `aria-labelledby`
- [ ] Each step is an `<li>` within the `<ol>`
- [ ] The currently active (`started`) step has `aria-current="step"`
- [ ] Step indicator icons have `aria-hidden="true"`
- [ ] Status information available as text (visually or via `sr-only` text)
- [ ] Screen reader reads steps in correct sequential order

### Status Logic

- [ ] Exactly one step can be `started` at a time (or zero if all are completed or not-started)
- [ ] Steps before the `started` step are all `completed`
- [ ] Steps after the `started` step are all `not-started`
- [ ] `getStepStatus` utility correctly derives status from index and currentStep

### Responsive

- [ ] Vertical timeline reflows correctly at all widths; text wraps within content column
- [ ] Horizontal timeline remains legible at minimum supported viewport; consider truncating long metadata at narrow breakpoints

### Platform (Native)

- [ ] **Android (Compose):** `TimelineVerticalItem` and `TimelineHorizontalItem` render correct icon per status
- [ ] **Android:** Connector color changes correctly based on step status
- [ ] **Android:** TalkBack reads step titles in sequence; `started` step is announced as current
