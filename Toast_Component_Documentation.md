# Toast Component Documentation

**Component:** Toast
**Figma Node:** `225-2581`
**File:** Components v6 Beta
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

The Toast is a **transient, non-disruptive notification** that appears briefly to acknowledge a user action or surface a system message. It floats above page content at the top or bottom center of the screen, auto-dismisses after a set duration, and disappears without requiring user interaction.

**Design philosophy:** The Toast is intentionally minimal — a white floating pill with Body/Medium text and a Level 3 elevation shadow. This keeps it unobtrusive while ensuring it reads clearly over all content beneath it.

**Key characteristics:**
- Full-pill shape with maximum border-radius
- White elevated surface (Elevation Level 3)
- Body/Medium typography (14px Regular)
- Auto-dismisses after 5000ms
- Horizontally centered, floating over content
- Supports optional icon, action link, and dismiss button

---

## Component Variants

The Toast is a single-shape component. Variants are controlled via props:

| Variant | Description |
|---|---|
| **Default** | Text-only pill — the base form shown in Figma |
| **With Icon** | Adds a 16×16px status icon left of the text |
| **With Action** | Adds a tappable Link-style text action (e.g., "Undo") to the right |
| **With Dismiss** | Adds an `×` icon button to manually close before auto-dismiss |
| **Persistent** | Disables auto-dismiss; requires explicit user dismissal |

---

## Anatomy

```
┌──────────────────────────────────────────────────────┐
│   [icon?]   [message text]   [action?]  [dismiss?]   │
└──────────────────────────────────────────────────────┘
```

- **Container** — White pill with Elevation Level 3 shadow
- **Icon** (optional) — 16×16px status icon, left-aligned with text
- **Message** — Body/Medium text, `content.base` color
- **Action** (optional) — Link/Medium styled text button, `content.link` color
- **Dismiss** (optional) — 16×16px close icon button, `content.muted` color

---

## Size & Spacing Specifications

| Property | Token | Value |
|---|---|---|
| Padding vertical | `tokens.base.spacing[16]` | 16px |
| Padding horizontal | `tokens.base.spacing[24]` | 24px |
| Gap (icon → text) | `tokens.base.spacing[8]` | 8px |
| Gap (text → action) | `tokens.base.spacing[16]` | 16px |
| Gap (text/action → dismiss) | `tokens.base.spacing[8]` | 8px |
| Min height | — | 52px (16px top + 20px line-height + 16px bottom) |
| Max width | — | `calc(100vw - 2 × spacing[32])` (leaves 32px margin each side) |
| Border radius | `tokens.base.border.radius.full` | 999px (full pill) |
| Icon size | `tokens.base.size[16]` | 16×16px |

### Visual Dimensions

```
        ┌──────────────────────────────────────┐
  16px  │  [16px icon]  [message text]         │  16px
        └──────────────────────────────────────┘
             ←8px→                       ←24px padding→
```

---

## Style Specifications

### Container

| Property | Token | Resolved Value |
|---|---|---|
| Background | `tokens.colorScheme.background.elevated.popover` | `#ffffff` |
| Border radius | `tokens.base.border.radius.full` | `999px` |
| Box shadow (Level 3) | Elevation/Level 3 | `0px 4px 20px rgba(5,55,130,0.08), 0px 4px 4px rgba(5,55,130,0.04), 0px 2px 8px rgba(5,55,130,0.04)` |
| Max width | — | `calc(100% - 64px)` |
| Display | — | `inline-flex` (intrinsic width, centered) |

### Message Text

| Property | Token | Resolved Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.body` | Plain (Regular) |
| Font size | `tokens.base.text.fontsize.ramp[14]` | 14px |
| Line height | `tokens.base.text.lineheight.ramp[14]` | 20px |
| Font weight | `tokens.brand.text.fontweight.body` | 400 |
| Letter spacing | `tokens.base.text.letterspacing[0]` | 0px |
| Color | `tokens.colorScheme.content.base` | `#000000` |

### Action Text

| Property | Token | Resolved Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.link` | Plain (Medium) |
| Font size | `tokens.base.text.fontsize.ramp[14]` | 14px |
| Font weight | `tokens.brand.text.fontweight.link` | 500 |
| Color | `tokens.colorScheme.content.link` | `#0066f5` |
| Text decoration | — | underline |

### Dismiss Icon

| Property | Value |
|---|---|
| Size | 16×16px |
| Color | `tokens.colorScheme.content.muted` = `rgba(0,0,0,0.63)` |
| Touch target | 32×32px (with invisible padding) |

---

## Positioning

The Toast is portal-rendered above all page content. Default position is **bottom-center**, 24px above the safe area inset.

| Position | Placement | CSS |
|---|---|---|
| Bottom center (default) | Bottom of screen, centered | `position: fixed; bottom: calc(24px + env(safe-area-inset-bottom)); left: 50%; transform: translateX(-50%)` |
| Top center (alt) | Top of screen, centered | `position: fixed; top: calc(env(safe-area-inset-top) + 24px); left: 50%; transform: translateX(-50%)` |

**Stacking:** Multiple toasts queue sequentially. New toasts replace or stack above the previous toast with a small gap of `spacing[8]` (8px). A maximum of 2–3 toasts may be shown simultaneously in most implementations.

---

## Animation Specifications

Toast uses a **slide + fade** entrance and a **fade-out** exit.

### Entrance (Appear)

| Property | Token | Value |
|---|---|---|
| Duration | `tokens.base.motion.duration[300]` | 300ms |
| Easing | `tokens.base.motion.ease.standard.out` | `cubic-bezier(1, 0, 1, 1)` |
| Transform start | — | `translateY(16px)` |
| Transform end | — | `translateY(0px)` |
| Opacity start | — | `0` |
| Opacity end | — | `1` |

### Auto-Dismiss Timer

| Property | Token | Value |
|---|---|---|
| Display duration | `tokens.base.motion.duration.toast` | **5000ms** |
| Timer starts | — | After entrance animation completes |
| Pause on hover | — | Yes — hovering/touching pauses the timer |

### Exit (Dismiss / Auto-dismiss)

| Property | Token | Value |
|---|---|---|
| Duration | `tokens.base.motion.duration[150]` | 150ms |
| Easing | `tokens.base.motion.ease.standard.in` | `cubic-bezier(0, 0, 0, 1)` |
| Opacity start | — | `1` |
| Opacity end | — | `0` |
| Transform | — | No vertical movement on exit (fade only) |

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .toast-enter { animation: none; opacity: 1; transform: none; }
  .toast-exit { animation: toast-fade-out 150ms linear; }

  @keyframes toast-fade-out {
    from { opacity: 1; }
    to { opacity: 0; }
  }
}
```

---

## State Specifications

| State | Behavior |
|---|---|
| **Idle** | Visible, auto-dismiss timer running |
| **Hovered / Focused** | Timer paused; toast remains visible; dismiss button revealed (if `withDismiss`) |
| **Dismissed (manual)** | Exit animation triggered immediately |
| **Auto-dismissed** | Exit animation triggered after 5000ms |
| **Persistent** | No auto-dismiss timer; requires `withDismiss` or explicit imperative dismiss |
| **Queued** | Waiting behind an existing toast; not yet visible |

---

## Props API

### TypeScript Interface

```typescript
interface ToastProps {
  /** The message text displayed in the toast */
  message: string;

  /**
   * Optional 16×16px icon rendered left of message text.
   * Pass a React node (e.g. <CheckCircleIcon />)
   */
  icon?: React.ReactNode;

  /**
   * Optional action label rendered as a link-styled button right of message.
   * Requires onAction to be provided.
   */
  actionLabel?: string;

  /** Callback when the action link is tapped */
  onAction?: () => void;

  /**
   * Whether to show an explicit dismiss (×) button.
   * Recommended when actionLabel is present or duration is long.
   * @default false
   */
  withDismiss?: boolean;

  /**
   * Auto-dismiss duration in ms. Set to 0 or Infinity for persistent toasts.
   * @default 5000
   */
  duration?: number;

  /** Callback when the toast has finished its exit animation */
  onDismiss?: () => void;

  /**
   * Toast position on screen.
   * @default 'bottom-center'
   */
  position?: 'top-center' | 'bottom-center';

  /** Optional accessible label override (defaults to the message text) */
  'aria-label'?: string;

  /** CSS class override for the container */
  className?: string;
}
```

### Toast Manager (Imperative API)

Most implementations use a singleton `toast()` function or hook rather than rendering `<Toast>` directly:

```typescript
interface ToastOptions {
  message: string;
  icon?: React.ReactNode;
  actionLabel?: string;
  onAction?: () => void;
  withDismiss?: boolean;
  duration?: number;
  position?: 'top-center' | 'bottom-center';
}

interface ToastManager {
  /** Show a toast and return its ID */
  show(options: ToastOptions | string): string;
  /** Programmatically dismiss a specific toast by ID */
  dismiss(id: string): void;
  /** Dismiss all visible toasts */
  dismissAll(): void;
}

// React hook
function useToast(): ToastManager;

// Imperative singleton (for use outside React tree, e.g. in API error handlers)
const toast: ToastManager;
```

---

## Code Examples

### Basic React Usage

```tsx
import { useToast } from '@/components/Toast';

function PaymentButton() {
  const toast = useToast();

  const handlePay = async () => {
    try {
      await submitPayment();
      toast.show({ message: 'Payment sent successfully' });
    } catch {
      toast.show({
        message: 'Payment failed. Please try again.',
        duration: 0,
        withDismiss: true,
      });
    }
  };

  return <button onClick={handlePay}>Pay Now</button>;
}
```

### With Action (Undo Pattern)

```tsx
const toast = useToast();

function deleteItem(id: string) {
  performDelete(id);

  toast.show({
    message: 'Item deleted',
    actionLabel: 'Undo',
    onAction: () => {
      undoDelete(id);
      toast.show({ message: 'Deletion undone' });
    },
    duration: 5000,
    withDismiss: true,
  });
}
```

### Toast Component Implementation

```tsx
import React, { useEffect, useRef, useState } from 'react';

const Toast: React.FC<ToastProps> = ({
  message,
  icon,
  actionLabel,
  onAction,
  withDismiss = false,
  duration = 5000,
  onDismiss,
  position = 'bottom-center',
  'aria-label': ariaLabel,
}) => {
  const [visible, setVisible] = useState(true);
  const [exiting, setExiting] = useState(false);
  const timerRef = useRef<ReturnType<typeof setTimeout> | null>(null);

  const dismiss = () => {
    setExiting(true);
    // Allow exit animation to complete before unmounting
    setTimeout(() => {
      setVisible(false);
      onDismiss?.();
    }, 150);
  };

  const startTimer = () => {
    if (duration > 0 && duration !== Infinity) {
      timerRef.current = setTimeout(dismiss, duration);
    }
  };

  const pauseTimer = () => {
    if (timerRef.current) clearTimeout(timerRef.current);
  };

  useEffect(() => {
    startTimer();
    return () => pauseTimer();
  }, []);

  if (!visible) return null;

  const positionStyle: React.CSSProperties =
    position === 'bottom-center'
      ? {
          position: 'fixed',
          bottom: 'calc(24px + env(safe-area-inset-bottom, 0px))',
          left: '50%',
          transform: exiting ? 'translateX(-50%)' : 'translateX(-50%)',
          zIndex: 9999,
        }
      : {
          position: 'fixed',
          top: 'calc(env(safe-area-inset-top, 0px) + 24px)',
          left: '50%',
          transform: 'translateX(-50%)',
          zIndex: 9999,
        };

  return (
    <div
      role="status"
      aria-live="polite"
      aria-atomic="true"
      aria-label={ariaLabel ?? message}
      style={positionStyle}
      className={`toast ${exiting ? 'toast--exiting' : 'toast--entering'}`}
      onMouseEnter={pauseTimer}
      onMouseLeave={startTimer}
      onFocus={pauseTimer}
      onBlur={startTimer}
    >
      {icon && (
        <span className="toast__icon" aria-hidden="true">
          {icon}
        </span>
      )}
      <span className="toast__message">{message}</span>
      {actionLabel && onAction && (
        <button
          className="toast__action"
          onClick={() => {
            onAction();
            dismiss();
          }}
          type="button"
        >
          {actionLabel}
        </button>
      )}
      {withDismiss && (
        <button
          className="toast__dismiss"
          onClick={dismiss}
          type="button"
          aria-label="Dismiss notification"
        >
          <CloseIcon size={16} aria-hidden="true" />
        </button>
      )}
    </div>
  );
};
```

### CSS Styles

```css
.toast {
  display: inline-flex;
  align-items: center;
  gap: var(--tokens-base-spacing-8, 8px);
  padding: var(--tokens-base-spacing-16, 16px) var(--tokens-base-spacing-24, 24px);
  background: var(--tokens-colorScheme-background-elevated-popover, #ffffff);
  border-radius: var(--tokens-base-border-radius-full, 999px);
  box-shadow:
    0px 4px 20px rgba(5, 55, 130, 0.08),
    0px 4px 4px rgba(5, 55, 130, 0.04),
    0px 2px 8px rgba(5, 55, 130, 0.04);
  max-width: calc(100vw - 64px);
  white-space: nowrap;
}

.toast__icon {
  flex-shrink: 0;
  width: 16px;
  height: 16px;
  color: var(--tokens-colorScheme-content-base, #000);
}

.toast__message {
  font-family: var(--tokens-brand-text-fontfamily-body, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-14, 14px);
  line-height: var(--tokens-base-text-lineheight-ramp-14, 20px);
  font-weight: 400;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-base, #000);
}

.toast__action {
  font-family: var(--tokens-brand-text-fontfamily-link, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-14, 14px);
  line-height: var(--tokens-base-text-lineheight-ramp-14, 20px);
  font-weight: 500;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-link, #0066f5);
  text-decoration: underline;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  margin-left: var(--tokens-base-spacing-8, 8px);
}

.toast__dismiss {
  flex-shrink: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  margin: -8px -8px -8px 0;
  color: var(--tokens-colorScheme-content-muted, rgba(0, 0, 0, 0.63));
  border-radius: var(--tokens-base-border-radius-full, 999px);
}

.toast__dismiss:focus-visible {
  outline: 2px solid var(--tokens-colorScheme-border-focus, #0066f5);
  outline-offset: 2px;
}

/* Entrance animation */
.toast--entering {
  animation: toast-in 300ms cubic-bezier(1, 0, 1, 1) forwards;
}

@keyframes toast-in {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(16px);
  }
  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0px);
  }
}

/* Exit animation */
.toast--exiting {
  animation: toast-out 150ms cubic-bezier(0, 0, 0, 1) forwards;
}

@keyframes toast-out {
  from { opacity: 1; }
  to { opacity: 0; }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  .toast--entering {
    animation: none;
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
  .toast--exiting {
    animation: toast-out-reduced 150ms linear forwards;
  }
  @keyframes toast-out-reduced {
    from { opacity: 1; }
    to { opacity: 0; }
  }
}
```

---

## Accessibility Specifications

### ARIA

| Attribute | Value | Purpose |
|---|---|---|
| `role="status"` | `"status"` | Polite live region — announces to screen readers without interrupting |
| `aria-live` | `"polite"` | Waits for the user to finish their current task before announcing |
| `aria-atomic` | `"true"` | Entire toast content is announced as a single unit |
| `aria-label` | Message text (default) | Readable label when the icon-only state is used |
| `aria-label` on dismiss button | `"Dismiss notification"` | Explicit label for the close control |

### Keyboard Navigation

| Key | Action |
|---|---|
| `Tab` | Move focus to the toast (only if it contains interactive elements: action or dismiss button) |
| `Enter` / `Space` | Activate the focused action or dismiss button |
| `Escape` | Dismiss the toast immediately (when toast has focus) |

**Focus management:** The toast container itself is not focused on appearance (doing so would interrupt users mid-task). Focus is only moved to the toast if the user explicitly presses `Tab` to navigate into it.

### Screen Reader Behavior

The `role="status"` live region causes assistive technologies to announce the toast message after the current speech queue completes. The message text should be:

- **Concise** — ideally one sentence or fewer than 80 characters
- **Self-contained** — screen readers cannot re-read a toast after it dismisses, so the message must make sense without visual context
- **Action-complete** — use past tense to confirm completion ("Payment sent", not "Sending payment")

### Color Contrast

| Element | Foreground | Background | Ratio |
|---|---|---|---|
| Message text (`content.base`) | `#000` | `#fff` | 21:1 ✅ |
| Action text (`content.link`) | `#0066f5` | `#fff` | ~4.6:1 ✅ |
| Dismiss icon (`content.muted`) | `rgba(0,0,0,0.63)` | `#fff` | ~5.7:1 ✅ |

---

## Platform APIs

### Android — Jetpack Compose

```kotlin
import androidx.compose.material3.Snackbar
import androidx.compose.material3.SnackbarHost
import androidx.compose.material3.SnackbarHostState
import androidx.compose.runtime.rememberCoroutineScope
import kotlinx.coroutines.launch

@Composable
fun ToastHost(snackbarHostState: SnackbarHostState) {
    SnackbarHost(
        hostState = snackbarHostState,
        snackbar = { snackbarData ->
            // Custom Toast shape matching design spec
            Surface(
                shape = CircleShape, // full pill = RoundedCornerShape(999.dp)
                color = MaterialTheme.colorScheme.surface,
                shadowElevation = 8.dp,
                tonalElevation = 0.dp
            ) {
                Row(
                    modifier = Modifier.padding(horizontal = 24.dp, vertical = 16.dp),
                    horizontalArrangement = Arrangement.spacedBy(8.dp),
                    verticalAlignment = Alignment.CenterVertically
                ) {
                    Text(
                        text = snackbarData.visuals.message,
                        style = MaterialTheme.typography.bodyMedium,
                        color = MaterialTheme.colorScheme.onSurface
                    )
                    snackbarData.visuals.actionLabel?.let { label ->
                        TextButton(onClick = { snackbarData.performAction() }) {
                            Text(
                                text = label,
                                style = MaterialTheme.typography.labelMedium.copy(
                                    textDecoration = TextDecoration.Underline
                                ),
                                color = MaterialTheme.colorScheme.primary
                            )
                        }
                    }
                }
            }
        }
    )
}

// Usage
val snackbarHostState = remember { SnackbarHostState() }
val coroutineScope = rememberCoroutineScope()

coroutineScope.launch {
    snackbarHostState.showSnackbar(
        message = "Payment sent successfully",
        actionLabel = "Undo",
        duration = SnackbarDuration.Short // = 4000ms (closest to 5000ms token)
    )
}
```

### iOS — SwiftUI

```swift
import SwiftUI

struct ToastModifier: ViewModifier {
    @Binding var isPresented: Bool
    let message: String
    let actionLabel: String?
    let onAction: (() -> Void)?
    let duration: TimeInterval

    func body(content: Content) -> some View {
        ZStack(alignment: .bottom) {
            content
            if isPresented {
                ToastView(
                    message: message,
                    actionLabel: actionLabel,
                    onAction: onAction
                )
                .transition(.move(edge: .bottom).combined(with: .opacity))
                .onAppear {
                    DispatchQueue.main.asyncAfter(deadline: .now() + duration) {
                        withAnimation(.easeIn(duration: 0.15)) {
                            isPresented = false
                        }
                    }
                }
                .padding(.bottom, 24)
            }
        }
        .animation(.interpolatingSpring(stiffness: 300, damping: 30), value: isPresented)
    }
}

struct ToastView: View {
    let message: String
    let actionLabel: String?
    let onAction: (() -> Void)?

    var body: some View {
        HStack(spacing: 8) {
            Text(message)
                .font(.custom("Plain-Regular", size: 14))
                .foregroundColor(.primary)
            if let label = actionLabel {
                Button(label) { onAction?() }
                    .font(.custom("Plain-Medium", size: 14).underline())
                    .foregroundColor(Color(red: 0, green: 0.40, blue: 0.96))
            }
        }
        .padding(.horizontal, 24)
        .padding(.vertical, 16)
        .background(
            Capsule()
                .fill(Color.white)
                .shadow(color: Color(red: 0.02, green: 0.22, blue: 0.51, opacity: 0.08),
                        radius: 20, x: 0, y: 4)
                .shadow(color: Color(red: 0.02, green: 0.22, blue: 0.51, opacity: 0.04),
                        radius: 4, x: 0, y: 4)
                .shadow(color: Color(red: 0.02, green: 0.22, blue: 0.51, opacity: 0.04),
                        radius: 8, x: 0, y: 2)
        )
        .accessibilityElement(children: .combine)
        .accessibilityAddTraits(.updatesFrequently)
    }
}

// Usage
extension View {
    func toast(
        isPresented: Binding<Bool>,
        message: String,
        actionLabel: String? = nil,
        onAction: (() -> Void)? = nil,
        duration: TimeInterval = 5.0
    ) -> some View {
        modifier(ToastModifier(
            isPresented: isPresented,
            message: message,
            actionLabel: actionLabel,
            onAction: onAction,
            duration: duration
        ))
    }
}
```

---

## Design Tokens Reference

| Token | Resolved Value | Usage in Toast |
|---|---|---|
| `tokens.colorScheme.background.elevated.popover` | `#ffffff` | Container fill |
| `tokens.colorScheme.content.base` | `#000000` | Message text color |
| `tokens.colorScheme.content.link` | `#0066f5` | Action text color |
| `tokens.colorScheme.content.muted` | `rgba(0,0,0,0.63)` | Dismiss icon color |
| `tokens.colorScheme.border.focus` | `#0066f5` | Focus ring on dismiss button |
| `tokens.base.border.radius.full` | `999px` | Full-pill container shape |
| `tokens.base.spacing[16]` | `16px` | Vertical padding |
| `tokens.base.spacing[24]` | `24px` | Horizontal padding |
| `tokens.base.spacing[8]` | `8px` | Icon-to-text gap |
| `tokens.base.size[16]` | `16px` | Icon dimensions |
| `tokens.base.text.fontsize.ramp[14]` | `14px` | Message & action font size |
| `tokens.base.text.lineheight.ramp[14]` | `20px` | Message line height |
| `tokens.base.motion.duration[300]` | `300ms` | Entrance animation |
| `tokens.base.motion.duration[150]` | `150ms` | Exit animation |
| `tokens.base.motion.duration.toast` | `5000ms` | Auto-dismiss delay |
| `tokens.base.motion.ease.standard.out` | `cubic-bezier(1,0,1,1)` | Entrance easing |
| `tokens.base.motion.ease.standard.in` | `cubic-bezier(0,0,0,1)` | Exit easing |
| Elevation Level 3 | see box-shadow above | Container drop shadow |

---

## Best Practices

**Use toasts for transient confirmation only.** Toasts are appropriate for acknowledging a completed action ("Saved", "Sent", "Deleted"). For errors that require the user to act, use an inline error state or a Dialog — a toast that auto-dismisses may leave the user without enough time to read and respond to a critical error message.

**Keep messages under 60 characters.** The toast is a narrow pill — long messages will either wrap awkwardly or overflow. If the message cannot be shortened, consider a Banner or Alert component instead.

**Provide an action for destructive operations.** Whenever a user performs an irreversible action (delete, archive, unsubscribe), the toast should include an "Undo" action to allow recovery within the 5000ms window.

**Never auto-dismiss a toast with important information.** If the user must read the message to make a decision or the content is time-sensitive, set `duration={0}` and `withDismiss={true}` so it persists until explicitly closed.

**Do not stack more than 3 toasts.** If multiple operations complete simultaneously, queue them so only 1–2 toasts are visible at a time. Replace older toasts with newer ones if the queue exceeds 3 items.

**Pause dismissal on hover/focus.** Users who tab to a toast (e.g., to activate an action) should not have it disappear while they are interacting with it.

**Avoid toasts for errors on forms.** Inline validation messages near the form field are always preferred. A toast for a validation error is easily missed and does not direct the user's attention to the problem.

---

## Related Components

- **Banner** — Persistent in-page notification for important system messages; does not auto-dismiss
- **Alert** — Inline contextual status indicator within a form or content section
- **Dialog / Modal** — For errors or confirmations that require user action before proceeding
- **Shimmer** — Skeleton loading state; use while content is loading (Toast confirms after load completes)

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial Toast documentation from Components v6 Beta Figma file |

---

## Testing Checklist

- [ ] Toast renders with Elevation Level 3 shadow matching `0px 4px 20px rgba(5,55,130,0.08)` spec
- [ ] Full-pill border-radius (999px) applied correctly
- [ ] Body/Medium typography (14px, Regular, Plain) applied to message
- [ ] Auto-dismisses after exactly 5000ms from appearance (not from mount)
- [ ] Timer pauses on mouse hover and keyboard focus
- [ ] Timer resumes when hover/focus leaves
- [ ] Entrance animation: 300ms slide up from 16px + fade in
- [ ] Exit animation: 150ms fade out only
- [ ] `prefers-reduced-motion` disables slide; retains fade-only exit
- [ ] `role="status"` and `aria-live="polite"` on container
- [ ] Screen reader announces message on appearance without interrupting current speech
- [ ] Action button triggers `onAction` callback and dismisses toast
- [ ] Dismiss (×) button closes toast with exit animation
- [ ] `Escape` key dismisses toast when focus is within it
- [ ] Dismiss button focus ring visible with `border.focus` color (#0066f5)
- [ ] Toast position: bottom-center, 24px above safe area inset
- [ ] Max-width constrains toast to `calc(100vw - 64px)` on narrow screens
- [ ] Persistent mode (`duration={0}`) does not auto-dismiss
- [ ] Multiple toasts queue correctly without overlapping
