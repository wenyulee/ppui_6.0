# Shimmer Component Documentation

**Component:** Shimmer
**Version:** 6.0 Beta
**Figma Node:** `1764-15286`
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

The **Shimmer** component is a skeleton loading placeholder that communicates to users that content is in the process of loading. It renders an animated gradient sweep ("shimmer") across a shaped placeholder element to imply the presence and approximate layout of incoming content before it arrives.

Shimmer placeholders should mirror the shape, size, and rough layout of the actual content they are replacing — a circular shimmer for avatar images, text-line shimmers for body copy, and container shimmers for card or block-level content.

### When to Use

- While data is being fetched asynchronously (API calls, image loads, database queries)
- When you can predict the rough layout of the incoming content and want to reduce perceived load time
- As a preferred alternative to spinner/loader overlays when content renders progressively (list items, feeds, cards)
- When preserving layout stability to avoid cumulative layout shift (CLS)

### When Not to Use

- When the load time is expected to be under ~300ms — showing a skeleton for very fast loads can feel jarring
- When the shape of incoming content is unpredictable — use a generic Loader instead
- When an operation has a known progress percentage — use a Progress Bar
- As a replacement for error states — if loading fails, replace the shimmer with an actual error state

---

## Component Variants

The Shimmer component exposes three **style** variants corresponding to different content shape categories:

| Style Variant | Figma Node | Description |
|---|---|---|
| `Round` | `1764-15282` | Circular placeholder; used for avatars, icons, and profile images |
| `Container` | `1764-15287` | Rounded rectangular placeholder; used for card content, buttons, tags, and image blocks |
| `Text` | `1764-15298` | Full-width horizontal bar(s); used for body copy, headings, and label text |

---

## Anatomy

### Full Skeleton Card (Composite Example)

The screenshot from Figma shows a representative composite skeleton that combines all three style variants to mock a content card:

```
┌──────────────────────────────────────────────────────┐
│                                                      │
│  ●    ██████████                                     │
│  ↑         ↑                                         │
│ Round   Container                                    │
│                                                      │
│  ████████████████████████████████  ← Text (full)    │
│  ████████████████████████████      ← Text (~85%)    │
│  ████████████████████████          ← Text (~75%)    │
│                                                      │
└──────────────────────────────────────────────────────┘
```

### Individual Part Descriptions

| Part | Shape | Description |
|---|---|---|
| **Round** | Circle | Placeholder for circular image/avatar content |
| **Container** | Rounded rectangle | Placeholder for rectangular block content (cards, images, tags, buttons) |
| **Text** | Thin rounded rectangle | Placeholder for a single line of text; stacked for multi-line paragraphs |
| **Shimmer gradient** | CSS gradient overlay | The animated highlight sweep that moves left-to-right continuously |

---

## Size & Spacing Specifications

### Round Variant

| Property | Value | Notes |
|---|---|---|
| Default diameter | 40px | Matches common avatar sizes |
| Border-radius | 50% | Full circle |
| Minimum size | 16px | Icon-scale placeholder |
| Common sizes | 16px, 24px, 32px, 40px, 48px, 56px, 64px | Match to actual avatar/icon sizes used |

### Container Variant

| Property | Value | Notes |
|---|---|---|
| Default height | 32px | Matches tag/button scale |
| Default width | 100% of parent | Fills available space |
| Border-radius | `--radius-md` (8px) | Matches card/block corner radius |
| Height range | Flexible | Set height to match actual content block height |
| Width range | Flexible | Set width to match actual content block width or use a percentage |

### Text Variant

| Property | Value | Notes |
|---|---|---|
| Default height | 12px | Matches default line-height for body text |
| Border-radius | `--radius-sm` (4px) | Subtle rounding on text bars |
| Width | Variable (see below) | Varies per line to simulate natural text layout |
| Line gap | 8px | Vertical spacing between stacked text shimmer lines |

**Recommended text line width pattern** (for multi-line text blocks):

| Line | Width | Purpose |
|---|---|---|
| Line 1 | 100% | First line of body text — full width |
| Line 2 | 85% | Second line — slightly shorter |
| Line 3 | 70% | Third line — shortest; simulates natural paragraph trailing |
| Heading | 60% | Single heading line — narrower than body |

### Vertical Spacing Within a Skeleton Card

| Section | Value |
|---|---|
| Round + Container row top padding | 16px |
| Round + Container internal gap | 12px |
| Row → Text block gap | 16px |
| Text line gap | 8px |
| Card bottom padding | 16px |

---

## Style Variants

All three style variants share the same visual language: a light grey base fill with an animated shimmer gradient overlay.

### Base Fill

| Token | Value | Usage |
|---|---|---|
| `--shimmer-base-color` | `--color-neutral-subtle` | The base grey background of all shimmer shapes |

### Shimmer Gradient

The shimmer effect is implemented as an animated linear gradient moving from left to right:

```css
background: linear-gradient(
  90deg,
  var(--shimmer-base-color) 0%,
  var(--shimmer-highlight-color) 50%,
  var(--shimmer-base-color) 100%
);
background-size: 200% 100%;
```

| Token | Light Mode | Dark Mode |
|---|---|---|
| `--shimmer-base-color` | `#ECEEF0` (Neutral / Subtle) | `#2A2D30` |
| `--shimmer-highlight-color` | `#F5F6F7` (near-white) | `#3A3D42` |

---

## State Specifications

### Loading (Active)

- Shimmer gradient animation is running
- Element is present in the DOM with `aria-hidden="true"` or within an `aria-busy` container
- Shape dimensions match the expected content dimensions

### Resolved (Exit)

- Shimmer is unmounted and replaced with actual content
- Transition may use a simple fade-out (opacity 1 → 0) before content fades in (opacity 0 → 1)
- Avoid layout shift: ensure shimmer dimensions exactly match the rendered content dimensions

### Reduced Motion

- Animation is disabled; the static base color is shown without movement
- Content still renders in the shimmer shape, so layout position is preserved

---

## Props API

### Web / React

```typescript
interface ShimmerProps {
  /**
   * Visual style of the shimmer placeholder.
   * - 'Round'     → circular (avatars, icons)
   * - 'Container' → rounded rectangle (cards, images, tags)
   * - 'Text'      → thin bar (text lines)
   * @default 'Round'
   */
  style?: 'Round' | 'Container' | 'Text';

  /** Width of the shimmer element (CSS value string or number in px) */
  width?: number | string;

  /** Height of the shimmer element (CSS value string or number in px) */
  height?: number | string;

  /** Border-radius override (CSS value). Defaults per style variant. */
  borderRadius?: number | string;

  /** Additional CSS class applied to the root element */
  className?: string;

  /** Inline style overrides */
  style?: React.CSSProperties;
}
```

> **Note:** The `style` prop name conflict (React's native `style` prop vs. the variant `style` prop) may be resolved in the implementation by renaming the variant prop to `variant`:
>
> ```typescript
> interface ShimmerProps {
>   variant?: 'Round' | 'Container' | 'Text';
>   width?: number | string;
>   height?: number | string;
>   className?: string;
>   style?: React.CSSProperties; // native React style
> }
> ```

### Android — Jetpack Compose

```kotlin
@Composable
fun Shimmer(
    style: ShimmerStyle = ShimmerStyle.Rounded,
    modifier: Modifier = Modifier,
    content: @Composable () -> Unit = {}
)

enum class ShimmerStyle {
    Rounded,    // Circular shimmer (avatar/icon)
    Container,  // Rectangular shimmer (card/block)
    Text        // Text-line shimmer
}
```

**Usage:**

```kotlin
// Avatar placeholder
Shimmer(style = ShimmerStyle.Rounded) { }

// Card block placeholder
Shimmer(style = ShimmerStyle.Container) { }

// Text line placeholder
Shimmer(style = ShimmerStyle.Text) { }
```

---

## Code Examples

### 1. Round Shimmer (Avatar Placeholder)

```tsx
import { Shimmer } from '@design-system/components';

export function AvatarSkeleton() {
  return (
    <Shimmer
      variant="Round"
      width={40}
      height={40}
    />
  );
}
```

---

### 2. Container Shimmer (Card Image Placeholder)

```tsx
import { Shimmer } from '@design-system/components';

export function CardImageSkeleton() {
  return (
    <Shimmer
      variant="Container"
      width="100%"
      height={200}
    />
  );
}
```

---

### 3. Text Line Shimmer (Single Paragraph Line)

```tsx
import { Shimmer } from '@design-system/components';

export function TextLineSkeleton() {
  return (
    <Shimmer
      variant="Text"
      width="85%"
      height={12}
    />
  );
}
```

---

### 4. Composite Skeleton Card

This is the primary usage pattern: combining all three variants to build a skeleton that matches the shape of a content card.

```tsx
import { Shimmer } from '@design-system/components';

export function ContentCardSkeleton() {
  return (
    <div
      role="status"
      aria-label="Loading content"
      style={{
        padding: 16,
        borderRadius: 12,
        border: '1px solid var(--color-border-subtle)',
        display: 'flex',
        flexDirection: 'column',
        gap: 16,
      }}
    >
      {/* Header row: avatar + tag */}
      <div style={{ display: 'flex', alignItems: 'center', gap: 12 }}>
        <Shimmer variant="Round" width={40} height={40} />
        <Shimmer variant="Container" width={80} height={32} />
      </div>

      {/* Text block */}
      <div style={{ display: 'flex', flexDirection: 'column', gap: 8 }}>
        <Shimmer variant="Text" width="100%" height={12} />
        <Shimmer variant="Text" width="85%" height={12} />
        <Shimmer variant="Text" width="70%" height={12} />
      </div>

      {/* Visually hidden accessible label */}
      <span className="sr-only">Loading…</span>
    </div>
  );
}
```

---

### 5. List of Skeleton Cards

```tsx
import { Shimmer } from '@design-system/components';
import { ContentCardSkeleton } from './ContentCardSkeleton';

export function FeedSkeleton({ count = 3 }: { count?: number }) {
  return (
    <ul
      role="list"
      aria-label="Loading feed"
      aria-busy="true"
      style={{ display: 'flex', flexDirection: 'column', gap: 12 }}
    >
      {Array.from({ length: count }).map((_, i) => (
        <li key={i} aria-hidden="true">
          <ContentCardSkeleton />
        </li>
      ))}
    </ul>
  );
}
```

---

### 6. Conditional Render: Skeleton → Content

```tsx
import { useState, useEffect } from 'react';
import { Shimmer } from '@design-system/components';

interface UserProfile {
  name: string;
  avatarUrl: string;
  bio: string;
}

export function UserProfileCard({ userId }: { userId: string }) {
  const [profile, setProfile] = useState<UserProfile | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchUserProfile(userId).then((data) => {
      setProfile(data);
      setLoading(false);
    });
  }, [userId]);

  if (loading) {
    return (
      <div role="status" aria-label="Loading profile">
        <div style={{ display: 'flex', gap: 12, alignItems: 'center' }}>
          <Shimmer variant="Round" width={48} height={48} />
          <div style={{ flex: 1, display: 'flex', flexDirection: 'column', gap: 8 }}>
            <Shimmer variant="Text" width="60%" height={14} />
            <Shimmer variant="Text" width="40%" height={12} />
          </div>
        </div>
        <div style={{ marginTop: 12, display: 'flex', flexDirection: 'column', gap: 8 }}>
          <Shimmer variant="Text" width="100%" height={12} />
          <Shimmer variant="Text" width="90%" height={12} />
          <Shimmer variant="Text" width="75%" height={12} />
        </div>
        <span className="sr-only">Loading profile…</span>
      </div>
    );
  }

  return (
    <div>
      <div style={{ display: 'flex', gap: 12, alignItems: 'center' }}>
        <img src={profile!.avatarUrl} alt={profile!.name} width={48} height={48} style={{ borderRadius: '50%' }} />
        <div>
          <h2>{profile!.name}</h2>
        </div>
      </div>
      <p>{profile!.bio}</p>
    </div>
  );
}
```

---

### 7. Heading + Body Skeleton

```tsx
import { Shimmer } from '@design-system/components';

export function ArticleSkeleton() {
  return (
    <article role="status" aria-label="Loading article">
      {/* Hero image */}
      <Shimmer variant="Container" width="100%" height={240} style={{ marginBottom: 24 }} />

      {/* Heading */}
      <Shimmer variant="Text" width="70%" height={24} style={{ marginBottom: 8 }} />
      <Shimmer variant="Text" width="55%" height={24} style={{ marginBottom: 20 }} />

      {/* Body paragraph 1 */}
      <Shimmer variant="Text" width="100%" height={14} style={{ marginBottom: 8 }} />
      <Shimmer variant="Text" width="100%" height={14} style={{ marginBottom: 8 }} />
      <Shimmer variant="Text" width="88%" height={14} style={{ marginBottom: 20 }} />

      {/* Body paragraph 2 */}
      <Shimmer variant="Text" width="100%" height={14} style={{ marginBottom: 8 }} />
      <Shimmer variant="Text" width="100%" height={14} style={{ marginBottom: 8 }} />
      <Shimmer variant="Text" width="65%" height={14} />

      <span className="sr-only">Loading article…</span>
    </article>
  );
}
```

---

### 8. Compose — Full Skeleton Card

```kotlin
@Composable
fun ContentCardSkeleton() {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .padding(16.dp),
        verticalArrangement = Arrangement.spacedBy(16.dp)
    ) {
        // Header row
        Row(
            horizontalArrangement = Arrangement.spacedBy(12.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Shimmer(style = ShimmerStyle.Rounded) {
                Box(modifier = Modifier.size(40.dp))
            }
            Shimmer(style = ShimmerStyle.Container) {
                Box(modifier = Modifier.width(80.dp).height(32.dp))
            }
        }

        // Text lines
        Column(verticalArrangement = Arrangement.spacedBy(8.dp)) {
            Shimmer(style = ShimmerStyle.Text) {
                Box(modifier = Modifier.fillMaxWidth().height(12.dp))
            }
            Shimmer(style = ShimmerStyle.Text) {
                Box(modifier = Modifier.fillMaxWidth(0.85f).height(12.dp))
            }
            Shimmer(style = ShimmerStyle.Text) {
                Box(modifier = Modifier.fillMaxWidth(0.70f).height(12.dp))
            }
        }
    }
}
```

---

### 9. Custom Hook: `useSkeletonVisible`

Delays showing the skeleton until a minimum threshold has passed, preventing flash for fast loads:

```tsx
import { useState, useEffect } from 'react';

/**
 * Returns true only after `delayMs` has elapsed, preventing
 * skeleton flicker for fast-resolving loads.
 */
export function useSkeletonVisible(loading: boolean, delayMs = 300): boolean {
  const [showSkeleton, setShowSkeleton] = useState(false);

  useEffect(() => {
    if (!loading) {
      setShowSkeleton(false);
      return;
    }
    const timer = setTimeout(() => setShowSkeleton(true), delayMs);
    return () => clearTimeout(timer);
  }, [loading, delayMs]);

  return showSkeleton;
}

// Usage
export function SmartProfileCard({ userId }: { userId: string }) {
  const [loading, setLoading] = useState(true);
  const showSkeleton = useSkeletonVisible(loading, 300);

  if (showSkeleton) return <UserProfileCardSkeleton />;
  return <UserProfileCard userId={userId} />;
}
```

---

### 10. Shimmer with Fade Transition

```tsx
import { useState, useEffect } from 'react';
import { Shimmer } from '@design-system/components';

export function FadingCard({ userId }: { userId: string }) {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetchData(userId).then(setData);
  }, [userId]);

  return (
    <div style={{ position: 'relative' }}>
      {/* Skeleton fades out */}
      <div
        style={{
          opacity: data ? 0 : 1,
          transition: 'opacity 200ms ease',
          pointerEvents: 'none',
          position: data ? 'absolute' : 'relative',
          inset: 0,
        }}
        aria-hidden={!!data}
      >
        <ContentCardSkeleton />
      </div>

      {/* Content fades in */}
      {data && (
        <div style={{ opacity: 1, animation: 'fadeIn 200ms ease' }}>
          <ContentCard data={data} />
        </div>
      )}
    </div>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern

The Shimmer component itself renders with `aria-hidden="true"` on individual shimmer elements, since they carry no meaningful information. The loading state is communicated at the **container level** using `role="status"` and `aria-busy`:

```html
<!-- Container with role="status" -->
<div role="status" aria-label="Loading profile" aria-busy="true">

  <!-- Individual shimmer shapes are hidden from AT -->
  <div aria-hidden="true" class="shimmer shimmer--round" style="width:40px;height:40px;"></div>
  <div aria-hidden="true" class="shimmer shimmer--text" style="width:60%;height:12px;"></div>
  <div aria-hidden="true" class="shimmer shimmer--text" style="width:45%;height:12px;"></div>

  <!-- Visually hidden label for screen readers -->
  <span class="sr-only">Loading…</span>
</div>
```

| ARIA Attribute | Placement | Value | Purpose |
|---|---|---|---|
| `role="status"` | Skeleton container | — | Declares the region as a live status zone |
| `aria-busy="true"` | Skeleton container | `"true"` while loading, `"false"` or removed when resolved | Informs AT that the region is being updated |
| `aria-label` | Skeleton container | e.g., `"Loading profile"` | Provides a human-readable description of what is loading |
| `aria-hidden="true"` | Each shimmer shape | `"true"` | Hides decorative placeholder shapes from screen readers |

### When Content Loads

1. Remove or set `aria-busy="false"` on the container
2. Replace shimmer elements with actual content
3. If using `role="status"`, the update to the region will be announced by AT with `aria-live="polite"` semantics

### Screen Reader Behavior

- With `role="status"`, screen readers announce the region's accessible name or contents with `aria-live="polite"` semantics — non-interruptive
- VoiceOver and NVDA will typically say "Loading profile, status" on focus or on live region update
- Individual shimmer shapes are invisible to AT (`aria-hidden="true"`) to prevent noise

### Focus Management

- Shimmer shapes are not focusable (`tabindex` is not set)
- The container may receive focus if it has `tabindex="0"` and is part of a focus flow — ensure the accessible name is descriptive
- When content replaces the skeleton, manage focus intentionally: if the skeleton occupied a primary content area, move focus to the first interactive element of the loaded content

---

## Animation Specifications

### Shimmer Gradient Sweep

The signature animation is a linear gradient that sweeps from left to right, creating a moving highlight:

```css
@keyframes shimmer-sweep {
  0% {
    background-position: -200% 0;
  }
  100% {
    background-position: 200% 0;
  }
}

.shimmer {
  background: linear-gradient(
    90deg,
    var(--shimmer-base-color) 0%,
    var(--shimmer-highlight-color) 40%,
    var(--shimmer-base-color) 80%
  );
  background-size: 200% 100%;
  animation: shimmer-sweep 1.6s ease-in-out infinite;
}
```

### Animation Parameters

| Property | Value | Notes |
|---|---|---|
| Duration | 1.6s | One full sweep cycle |
| Timing function | `ease-in-out` | Smooth acceleration and deceleration |
| Iteration | `infinite` | Continuous loop while loading |
| Direction | Left → Right | Matches natural reading direction |
| Delay | 0s for first item; optional stagger for lists | See staggered loading below |

### Staggered Loading (Lists)

For list skeletons, stagger the animation delay slightly per item to avoid a synchronised "wave" that can look mechanical:

```css
.shimmer-item:nth-child(1) .shimmer { animation-delay: 0ms; }
.shimmer-item:nth-child(2) .shimmer { animation-delay: 100ms; }
.shimmer-item:nth-child(3) .shimmer { animation-delay: 200ms; }
/* etc. */
```

Or programmatically in React:

```tsx
{items.map((_, i) => (
  <Shimmer
    key={i}
    variant="Text"
    width="100%"
    height={12}
    style={{ animationDelay: `${i * 100}ms` }}
  />
))}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .shimmer {
    animation: none;
    /* Static base color with no movement */
    background: var(--shimmer-base-color);
  }
}
```

With reduced motion enabled, the placeholder shapes are shown as static grey blocks — still communicating "loading" without triggering vestibular sensitivity.

---

## Design Tokens

### Color Tokens

| Token | Light Mode Value | Dark Mode Value | Usage |
|---|---|---|---|
| `--shimmer-base-color` | `#ECEEF0` | `#2A2D30` | Base grey of all shimmer shapes |
| `--shimmer-highlight-color` | `#F5F6F7` | `#3A3D42` | Moving highlight gradient peak |

### Shape Tokens

| Token | Value | Usage |
|---|---|---|
| `--shimmer-radius-round` | `50%` | Round variant border-radius |
| `--shimmer-radius-container` | `--radius-md` (8px) | Container variant border-radius |
| `--shimmer-radius-text` | `--radius-sm` (4px) | Text variant border-radius |

### Animation Tokens

| Token | Value | Usage |
|---|---|---|
| `--shimmer-duration` | `1.6s` | Animation cycle duration |
| `--shimmer-timing` | `ease-in-out` | Easing function |
| `--shimmer-delay-step` | `100ms` | Per-item stagger step for list skeletons |

### Spacing Tokens (Skeleton Layouts)

| Token | Value | Usage |
|---|---|---|
| `--shimmer-text-gap` | `8px` | Vertical gap between stacked text shimmer lines |
| `--shimmer-section-gap` | `16px` | Vertical gap between skeleton sections |

---

## Best Practices

### Do

- **Match the shimmer dimensions to actual content dimensions.** A 40px avatar should have a 40px Round shimmer — dimension parity prevents layout shift when content loads.
- **Use `aria-busy="true"` on the loading container** and set it to `false` (or remove it) when the real content replaces the skeleton.
- **Provide a visually hidden "Loading…" text** inside the skeleton container for screen reader users who cannot perceive animated placeholders.
- **Use `useSkeletonVisible` (or similar)** to delay skeleton rendering by ~300ms so fast loads never flash a skeleton at all.
- **Stagger animation delays** in list skeletons (100ms per item) to avoid a mechanical synchronised wave effect.
- **Apply `prefers-reduced-motion`** to disable animation for users with vestibular sensitivities; show a static base color instead.
- **Unmount shimmer completely** once data loads — do not leave hidden shimmer elements in the DOM.

### Don't

- **Don't use Shimmer when you can render content immediately.** Only show skeleton states for genuine asynchronous loads.
- **Don't misrepresent content layout.** A shimmer that looks like a single line of text should not be replaced by a 5-line paragraph — this causes jarring layout shift.
- **Don't animate indefinitely beyond a reasonable timeout.** If loading exceeds ~10 seconds, replace the shimmer with an error/retry state.
- **Don't make shimmer shapes interactive** (no `onClick`, no `tabindex`).
- **Don't use shimmer for action feedback** (e.g., after a button click) — use a Loader/spinner for in-button loading states.
- **Don't expose animated content to AT** — always mark individual shimmer shapes with `aria-hidden="true"`.

### Skeleton Fidelity Levels

Choose the right fidelity for your use case:

| Fidelity | Approach | When to Use |
|---|---|---|
| **High** | Exact dimensions + exact layout matching actual content | Profile cards, article pages — high-visibility areas where layout shift is very noticeable |
| **Medium** | Approximate dimensions + correct component structure | Feed items, list rows — fast-loading lists where exact matching is impractical |
| **Low** | Generic container shimmer | Unknown content structures, fallback loading for any dynamic content |

---

## Related Components

| Component | Relationship |
|---|---|
| **Loader** | Use Loader for bounded, percentage-based progress or for in-button loading. Use Shimmer for content placeholder loading |
| **Progress Bar** | Use when a deterministic load percentage is available. Progress Bar's indeterminate mode also uses a shimmer animation, but for the bar fill itself |
| **Empty State** | Use when content is loaded but there is no data to show |
| **Image** | The Image component may internally use a Shimmer while the image src is loading |
| **Card** | A common host component for `ContentCardSkeleton` patterns |

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0 | 2026-03-18 | Initial documentation. Three style variants (Round, Container, Text); Compose API (`ShimmerStyle` enum); shimmer gradient animation; `aria-busy` + `role="status"` accessibility pattern |

---

## Testing Checklist

### Visual

- [ ] Round variant renders as a circle (border-radius: 50%) at specified diameter
- [ ] Container variant renders as a rounded rectangle with `--radius-md` corners
- [ ] Text variant renders as a thin bar with `--radius-sm` corners
- [ ] All variants show the shimmer base color (`--shimmer-base-color`) as background
- [ ] Shimmer gradient highlight is visible and moves left-to-right
- [ ] Composite skeleton card matches the layout of the actual content card it replaces
- [ ] No layout shift occurs when content replaces the skeleton

### Animation

- [ ] Shimmer sweep animation runs at 1.6s cycle, `ease-in-out`, infinite
- [ ] List skeleton items have staggered animation delays (100ms per item)
- [ ] `prefers-reduced-motion: reduce` disables animation and shows static base color
- [ ] Animation does not play after skeleton is unmounted

### Accessibility

- [ ] Skeleton container has `role="status"` and `aria-label="Loading [content type]"`
- [ ] `aria-busy="true"` is set while loading; removed or set to `"false"` on resolution
- [ ] Individual shimmer shapes have `aria-hidden="true"`
- [ ] Visually hidden "Loading…" text is present inside the skeleton container (`class="sr-only"`)
- [ ] No shimmer shape is focusable (`tabindex` not set)
- [ ] Screen reader (VoiceOver / NVDA) announces loading state on region entry
- [ ] Screen reader announces when loading is complete (via live region update)

### Interaction & Lifecycle

- [ ] Skeleton is unmounted (not just hidden) when content loads
- [ ] `useSkeletonVisible` (or equivalent) prevents flash-of-skeleton for fast loads (<300ms)
- [ ] Skeleton shown if load exceeds delay threshold
- [ ] Error state replaces skeleton after timeout (no indefinite animation)

### Platform (Native)

- [ ] **Android (Compose):** `Shimmer(style = ShimmerStyle.Rounded)`, `.Container`, `.Text` all render with correct shapes and animation
- [ ] **Android:** Shimmer responds to system reduced-motion accessibility setting
- [ ] **iOS (SwiftUI):** If using a custom Shimmer view, animation and shapes match spec

### Responsive

- [ ] Text variant shimmers respect percentage-based widths and reflow correctly at all viewport sizes
- [ ] Container variant fills parent width at narrow viewports
- [ ] Skeleton card maintains structural layout at mobile breakpoints
