# Contextual Alert Component Documentation

## Overview

The Contextual Alert component communicates important information inline, directly adjacent to the content it relates to. Unlike site-wide banners or toast notifications, Contextual Alerts are scoped to a specific section, form, or content block and are rendered within the normal document flow rather than overlaid. They convey feedback, validation results, warnings, or informational messages in a way that is immediate, spatially relevant, and non-disruptive. They are a critical part of form validation UX, inline data quality warnings, and localized system status communication.

---

## Component Variants

Contextual Alert supports five semantic types:

### 1. **Error Alert**
- **Purpose**: Communicate a critical problem that prevents action or signals a failure
- **Use Cases**: Form submission failure, API error, invalid data, broken connection
- **Color**: Red / Danger (#EF4444)
- **Icon**: X-circle or alert-triangle
- **Behavior**: Persists until the error is resolved; not auto-dismissible by default
- **Urgency**: High — use `aria-live="assertive"` or `role="alert"`

### 2. **Warning Alert**
- **Purpose**: Inform the user of a potential issue or risk that doesn't block action
- **Use Cases**: Unsaved changes, approaching quota, deprecated feature notice, data mismatch
- **Color**: Amber / Warning (#F59E0B)
- **Icon**: Alert-triangle or exclamation
- **Behavior**: May be dismissible; user can acknowledge and continue
- **Urgency**: Medium — use `aria-live="polite"`

### 3. **Success Alert**
- **Purpose**: Confirm that an action was completed successfully
- **Use Cases**: Form saved, file uploaded, settings updated, payment processed
- **Color**: Green / Success (#10B981)
- **Icon**: Check-circle
- **Behavior**: Often auto-dismissible after a few seconds; or persists until user dismisses
- **Urgency**: Low — use `aria-live="polite"`

### 4. **Info Alert**
- **Purpose**: Provide neutral, helpful context without implying urgency
- **Use Cases**: Feature explanation, data source disclosure, requirement summary, soft limits
- **Color**: Blue / Info (#3B82F6) or Cyan (#06B6D4)
- **Icon**: Info-circle
- **Behavior**: Persists by default; dismissible when appropriate
- **Urgency**: Low — use `aria-live="polite"`

### 5. **Neutral Alert**
- **Purpose**: General notice or guidance with no semantic color connotation
- **Use Cases**: Process status, supplementary notes, contextual help text
- **Color**: Gray (#6B7280)
- **Icon**: Optional (info or none)
- **Behavior**: Persists; non-dismissible by default
- **Urgency**: Minimal — `aria-live="off"` or `role="note"`

---

## Size Variants

| Size | Min Height | Padding | Font Size | Use Case |
|------|------------|---------|-----------|----------|
| **SM** | 36px | 8px–12px | 12px | Inline field-level validation, dense forms |
| **MD** | 48px | 12px–16px | 14px | **Default**, most form and section alerts |
| **LG** | 60px | 16px–20px | 15px | Section-level alerts with full description |
| **XL** | Auto | 20px–24px | 16px | Rich alerts with title + body + actions |

---

## Style Variants

### **Filled**
- Background: Full semantic color (Error, Warning, Success, Info)
- Text: White (#FFFFFF)
- Icon: White
- Border: None
- Best For: High-prominence errors or success confirmations

### **Tinted** (Default)
- Background: Light tint of semantic color (8–12% opacity)
- Text: Dark variant of semantic color
- Icon: Semantic color
- Border: None
- Best For: Inline form validation, standard inline feedback

### **Outlined**
- Background: White or transparent
- Border: 1–2px solid semantic color
- Text: Dark semantic variant
- Icon: Semantic color
- Best For: Lower visual weight; secondary alerts

### **Left-Bordered**
- Background: Tinted
- Left Border: 4px solid semantic color (accent bar)
- Right/Top/Bottom Border: None
- Text: Dark semantic variant
- Best For: Inline section notes, information panels, inline help text

---

## Layout Options

### **Compact (Single Line)**
- Icon + message text on one line
- No title; just a concise message
- Optional dismiss button at end of line
- Use for: Field validation messages, short warnings

### **Standard (Title + Body)**
- Icon + title on first line
- Body text below title
- Optional action link or button below body
- Optional dismiss at top right
- Use for: Form-level feedback, section-level warnings

### **Rich (Title + Body + Actions)**
- Icon (larger, 20–24px)
- Title (bold)
- Body (descriptive paragraph)
- Action buttons below body (primary + secondary)
- Dismiss × at top right
- Use for: Critical errors with recovery instructions, complex warnings

---

## State Specifications

### **Default State**
- Visibility: Rendered in document flow
- Opacity: 100%
- Border Radius: 6–8px (compact: 4px)
- Shadow: None (Elevation 0)
- Cursor: default

### **Dismissible State**
- Dismiss button (×) visible at right edge
- Hover on ×: Background darkens slightly
- Cursor on ×: pointer
- On dismiss: Exit animation, then removed from DOM

### **Entering State**
- Opacity: 0 → 1
- Max-height: 0 → auto
- Duration: 200ms ease-out

### **Exiting State**
- Opacity: 1 → 0
- Max-height: auto → 0
- Padding: collapses to 0
- Duration: 200ms ease-in
- `overflow: hidden` during collapse

### **Loading/Async State**
- Shows spinner or skeleton inside alert body
- Used when alert content is fetched asynchronously
- Background and border use neutral color until type is known

---

## Props API

```typescript
interface ContextualAlertProps {
  // Semantic Type
  type: 'error' | 'warning' | 'success' | 'info' | 'neutral';

  // Content
  title?: string;                              // Optional heading
  message: string | React.ReactNode;          // Primary message content
  icon?: React.ReactNode | boolean;            // Custom icon; false to hide default
  actions?: AlertAction[];                     // Action buttons

  // Appearance
  variant?: 'filled' | 'tinted' | 'outlined' | 'left-bordered'; // Style variant
  size?: 'sm' | 'md' | 'lg' | 'xl';
  fullWidth?: boolean;                         // Expand to container width

  // Behavior
  dismissible?: boolean;                       // Show dismiss × button
  onDismiss?: () => void;                      // Called when dismissed
  dismissLabel?: string;                       // Accessible label for dismiss button
  autoClose?: number;                          // Auto-dismiss after N milliseconds

  // Accessibility
  role?: 'alert' | 'status' | 'note';         // ARIA live role
  aria-live?: 'assertive' | 'polite' | 'off';
  aria-label?: string;
  aria-describedby?: string;
  id?: string;

  // Styling
  className?: string;
  style?: React.CSSProperties;
}

interface AlertAction {
  label: string;
  onClick: () => void;
  variant?: 'link' | 'button';
  href?: string;
  external?: boolean;
}
```

---

## Code Examples

### **1. Form Validation Error**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function FormError({ error }: { error?: string }) {
  if (!error) return null;

  return (
    <ContextualAlert
      type="error"
      message={error}
      size="sm"
      variant="tinted"
      role="alert"
    />
  );
}

// Usage
<input id="email" type="email" aria-describedby="email-error" />
<FormError error="Please enter a valid email address." />
```

### **2. Form-Level Submission Error**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function SubmissionErrorAlert({
  errors,
}: {
  errors: string[];
}) {
  if (!errors.length) return null;

  return (
    <ContextualAlert
      type="error"
      title="Unable to save changes"
      message={
        <ul className="list-disc ml-4 mt-1 space-y-1">
          {errors.map((e, i) => <li key={i}>{e}</li>)}
        </ul>
      }
      variant="tinted"
      role="alert"
      aria-live="assertive"
    />
  );
}
```

### **3. Success Confirmation**
```typescript
import { ContextualAlert } from '@components/contextual-alert';
import { useState } from 'react';

export function SaveSuccess() {
  const [visible, setVisible] = useState(true);

  if (!visible) return null;

  return (
    <ContextualAlert
      type="success"
      message="Your profile has been updated successfully."
      dismissible
      onDismiss={() => setVisible(false)}
      autoClose={5000}
      aria-live="polite"
    />
  );
}
```

### **4. Warning with Action**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function UnsavedChangesWarning({ onDiscard }: { onDiscard: () => void }) {
  return (
    <ContextualAlert
      type="warning"
      title="You have unsaved changes"
      message="Leaving this page will discard any changes you have made."
      variant="tinted"
      actions={[
        { label: 'Discard changes', onClick: onDiscard, variant: 'button' },
        { label: 'Keep editing', onClick: () => {}, variant: 'button' },
      ]}
    />
  );
}
```

### **5. Info Alert with Link**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function DataSourceInfo() {
  return (
    <ContextualAlert
      type="info"
      message="This report uses data from the last 30 days. Fiscal year data may differ."
      variant="left-bordered"
      actions={[
        { label: 'Learn about data sources', href: '/docs/data', variant: 'link', external: true },
      ]}
    />
  );
}
```

### **6. Neutral Section Note**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function ReadOnlyNotice() {
  return (
    <ContextualAlert
      type="neutral"
      message="This section is managed by your organization administrator and cannot be edited here."
      variant="outlined"
      icon={false}
      size="sm"
    />
  );
}
```

### **7. Dismissible Warning with autoClose**
```typescript
import { ContextualAlert } from '@components/contextual-alert';
import { useState } from 'react';

export function QuotaWarning({ used, limit }: { used: number; limit: number }) {
  const [dismissed, setDismissed] = useState(false);
  const pct = Math.round((used / limit) * 100);

  if (dismissed || pct < 80) return null;

  return (
    <ContextualAlert
      type="warning"
      title={`Storage ${pct}% full`}
      message={`You've used ${used}GB of your ${limit}GB plan. Consider upgrading to avoid disruptions.`}
      dismissible
      onDismiss={() => setDismissed(true)}
      actions={[{ label: 'Upgrade storage', onClick: () => {}, variant: 'button' }]}
    />
  );
}
```

### **8. Multiple Alerts Stacked**
```typescript
import { ContextualAlert } from '@components/contextual-alert';

export function ValidationSummary() {
  return (
    <div className="space-y-2">
      <ContextualAlert
        type="error"
        message="Email address is required."
        size="sm"
        role="alert"
      />
      <ContextualAlert
        type="error"
        message="Password must be at least 8 characters."
        size="sm"
        role="alert"
      />
      <ContextualAlert
        type="warning"
        message="Phone number is recommended but optional."
        size="sm"
      />
    </div>
  );
}
```

---

## Accessibility Specifications

### **ARIA Roles & Live Regions**
- **`role="alert"` + `aria-live="assertive"`**: Errors requiring immediate attention; content announced immediately by screen readers
- **`role="status"` + `aria-live="polite"`**: Success and info messages; announced after current speech completes
- **`role="note"`**: Neutral supplementary information; not automatically announced
- Alerts injected into the DOM after user action automatically trigger live region announcements
- Alerts present on initial page load do not trigger live regions (use heading/landmark instead)

### **Icon Accessibility**
- Icons are decorative (`aria-hidden="true"`) — the text message carries the meaning
- Never rely on icon alone to communicate type; color + icon + text together
- Custom icons must include `aria-hidden="true"`

### **Dismiss Button**
- Uses `<button>` element (never `<div>` or `<span>`)
- `aria-label`: "Dismiss error message" (type-specific, not just "Close")
- Focus returns to logical previous element after dismissal
- Minimum touch target: 44×44px

### **Keyboard Navigation**
- **Tab**: Moves focus to dismiss button or action links within alert
- **Enter / Space**: Activates dismiss or action buttons
- **Escape**: Optionally dismisses alert (when focus is within it)

### **Color Independence**
- All types use icon + color + text — never color alone
- Error: Red + X-circle icon + error message text
- Warning: Amber + triangle icon + warning message text
- Success: Green + check-circle icon + confirmation text
- Info: Blue + info-circle icon + informational text

### **Screen Reader Announcements**
- Error (role="alert"): "Error: Please enter a valid email address." (announced immediately)
- Success (role="status"): "Your profile has been updated successfully." (announced politely)
- Dismissed: No announcement needed on exit; focus returns silently
- Multiple errors: Each alert announced in order; or use a single `role="alert"` with list

### **Best Practices**
- Place alerts directly above or below the content they relate to
- Use `role="alert"` only for genuinely urgent messages — overuse causes alert fatigue
- Avoid stacking more than 3 alerts in one area; consider a summary instead
- Never use color as the only indicator of type
- Always provide actionable guidance in error messages ("Enter a valid email" not "Invalid input")

---

## Animation Specifications

### **Entrance (Slide Down + Fade)**
- **Target**: Opacity + max-height + padding
- **Duration**: 200ms
- **Easing**: ease-out
  ```css
  @keyframes alertEnter {
    from { opacity: 0; max-height: 0; padding-top: 0; padding-bottom: 0; }
    to   { opacity: 1; max-height: 200px; padding-top: 12px; padding-bottom: 12px; }
  }
  animation: alertEnter 200ms ease-out;
  overflow: hidden;
  ```

### **Exit (Collapse + Fade)**
- **Target**: Opacity + max-height + padding
- **Duration**: 200ms
- **Easing**: ease-in
  ```css
  @keyframes alertExit {
    from { opacity: 1; max-height: 200px; }
    to   { opacity: 0; max-height: 0; padding: 0; margin: 0; }
  }
  animation: alertExit 200ms ease-in forwards;
  overflow: hidden;
  ```

### **Dismiss Button Hover**
- Background: Darkens (rgba(0,0,0,0.08))
- Duration: 100ms ease-out

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .contextual-alert { animation: none; transition: opacity 100ms; }
}
```

---

## Design Tokens

### **Type Colors (Tinted Variant)**
| Type | Background | Border-Left | Icon/Text | Dark Text |
|------|------------|-------------|-----------|-----------|
| Error | #FEF2F2 | #EF4444 | #EF4444 | #991B1B |
| Warning | #FFFBEB | #F59E0B | #F59E0B | #92400E |
| Success | #ECFDF5 | #10B981 | #10B981 | #065F46 |
| Info | #EFF6FF | #3B82F6 | #3B82F6 | #1E40AF |
| Neutral | #F9FAFB | #9CA3AF | #6B7280 | #374151 |

### **Type Colors (Filled Variant)**
| Type | Background | Text |
|------|------------|------|
| Error | #EF4444 | #FFFFFF |
| Warning | #F59E0B | #FFFFFF |
| Success | #10B981 | #FFFFFF |
| Info | #3B82F6 | #FFFFFF |
| Neutral | #6B7280 | #FFFFFF |

### **Sizing**
| Property | SM | MD | LG |
|----------|----|----|----|
| Min Height | 36px | 48px | 60px |
| Padding V | 8px | 12px | 16px |
| Padding H | 12px | 16px | 20px |
| Icon Size | 14px | 16px | 20px |
| Border Radius | 4px | 6px | 8px |
| Left Border Width | 3px | 4px | 4px |

---

## Content Guidelines

### **Error Messages**
- Be specific: "Email address is already in use" not "Invalid input"
- Suggest resolution: "Enter a different email or sign in"
- Avoid technical jargon: "Something went wrong. Please try again." over "HTTP 500"
- Avoid blame: "Invalid email" not "You entered an invalid email"

### **Warning Messages**
- State the risk clearly: "Deleting this will also remove all linked records"
- Offer a path forward: "Save your work before leaving"
- Be proportionate: Don't use warning color for non-risky situations

### **Success Messages**
- Confirm what happened: "Password updated successfully"
- Be brief: No need to repeat all the details
- Optional: Include a next step or link

### **Info Messages**
- Provide useful context: "This field is optional"
- Don't over-explain: 1–2 sentences maximum
- Use for supplementary guidance, not primary instructions

---

## Placement Guidelines

- **Field-level errors**: Immediately below the input they relate to
- **Form-level errors**: At the top of the form, above the submit button, and above the first invalid field
- **Section alerts**: At the top of the relevant section
- **Page-level info**: Top of main content area (below navigation)
- Never place contextual alerts inside modals unless they relate to the modal's form

---

## Related Components

- **Banner**: Full-width, page-level (not inline) alerts
- **Toast / Snackbar**: Transient, auto-dismissing notifications for completed actions
- **Coachtip**: Anchored guidance overlays for onboarding
- **Form Field Error**: Minimal inline text error directly under a field (no icon/background)
- **Dialog / Modal**: For errors requiring immediate decision before continuing

---

## Version History

- **v1.0**: Four types (error, warning, success, info), tinted and filled variants
- **v1.1**: Added neutral type, left-bordered variant, dismissible support
- **v1.2**: Action buttons, autoClose, size variants
- **v2.0**: Refactor with design tokens, slide-in/out animation, full ARIA audit
- **v2.1**: Rich layout (title + body + actions), icon override, reduced motion support

---

## Testing Checklist

- [ ] All 5 types render with correct color, icon, and background
- [ ] Tinted, filled, outlined, and left-bordered variants render correctly
- [ ] Compact (message only) layout renders on single line
- [ ] Standard (title + body) layout renders correctly
- [ ] Rich (title + body + actions) layout renders correctly
- [ ] Dismiss × button visible when `dismissible` is true
- [ ] Dismissing removes alert from DOM with exit animation
- [ ] `onDismiss` callback fires correctly
- [ ] `autoClose` dismisses after specified duration
- [ ] `role="alert"` alerts announced immediately by screen reader
- [ ] `role="status"` alerts announced politely
- [ ] Icon is `aria-hidden="true"` and text carries the meaning
- [ ] Action buttons/links are keyboard accessible
- [ ] Focus returns correctly after dismiss
- [ ] Entrance/exit animations smooth at 60fps
- [ ] Reduced motion uses opacity-only transition
- [ ] Color contrast meets WCAG AA on all variants
- [ ] Multiple stacked alerts maintain correct spacing
- [ ] Touch targets (dismiss, actions) meet 44×44px minimum
