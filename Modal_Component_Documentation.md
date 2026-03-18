# Modal Component Documentation

## Overview

The Modal component family provides overlay surfaces that capture user attention and require a deliberate response before returning to the underlying page. It encompasses three distinct presentation patterns — a centered **Dialog** for confirmations and alerts, a **Mobile Sheet** (bottom sheet in Default, Inset, and Full Screen variants), and a **Desktop Sheet** (a positioned drawer that can dock Left, Center, or Right). All three patterns share a common scrim backdrop, focus trap behavior, and a set of composable sub-components: `Modal.Sheet.TopNav`, `Modal.Sheet.ImageStart`, and `Modal.Custom`.

**Component name:** `Modal` / `Dialog` / `Sheet`
**Figma nodes:** `42020-67535` (Mobile Sheet variants), `19031-17692` (Dialog), `174-1098` (Mobile Sheet full anatomy), `10752-4743` (Desktop Sheet), `6065-19992` (Parts)
**Platform:** React / TypeScript (Web), SwiftUI (iOS), Jetpack Compose (Android)
**Category:** Overlay / Feedback

---

## Component Variants

### By Presentation Type

| Variant | Node | Surface | Best Used For |
|---------|------|---------|---------------|
| **Dialog** | `19031-17692` | Centered floating card | Confirmations, alerts, short messages with 1–2 actions |
| **Mobile Sheet — Default** | `42020-67532` | Bottom sheet, slides up, full width | Mobile content selections, multi-step flows |
| **Mobile Sheet — Inset** | `42020-67533` | Bottom sheet with 8 px side margin and fully rounded corners | Cards, payment confirmations, rich contextual detail |
| **Mobile Sheet — Full Screen** | `42020-67534` | Sheet occupies full screen height with top rounded corners | Complex workflows, multi-step forms, embedded navigators |
| **Desktop Sheet — Left** | `10752-4744` | Side drawer anchored to the left edge | Navigation drawers, settings panels, filter sidebars |
| **Desktop Sheet — Center** | `10752-4786` | Centered dialog with custom content | Rich content modals, forms, media detail views |
| **Desktop Sheet — Right** | `10752-4758` | Side drawer anchored to the right edge | Detail panels, activity feeds, chat/help drawers |

---

## Anatomy

### Dialog

```
┌────────────────────────────────────┐
│                                    │  ← Scrim (rgba(0,0,0,0.63))
│  ┌──────────────────────────────┐  │
│  │  Title                       │  │  ← Heading/Medium (responsive)
│  │  Description (optional)      │  │  ← Body/Large
│  │                              │  │
│  │  [Primary Button           ] │  │  ← ButtonGroup (vertical)
│  │  [Tertiary Button          ] │  │
│  └──────────────────────────────┘  │
│                                    │
└────────────────────────────────────┘
```

### Mobile Sheet

```
┌───────────────────────────────────┐   ← Scrim
│                                   │
│  ━━━━━━━━━  (Grabber — optional)  │   ← 40×5 px pill
│  ┌─────────────────────────────┐  │
│  │ [←]   Title          [✕]   │  │   ← Modal.Sheet.TopNav
│  ├─────────────────────────────┤  │
│  │ [Modal.Sheet.ImageStart]    │  │   ← Optional hero image
│  ├─────────────────────────────┤  │
│  │  Title                      │  │   ← Header (title/description/amount)
│  │  Description                │  │
│  ├─────────────────────────────┤  │
│  │  [Modal.Custom — Slot 1]    │  │   ← Custom content area
│  │  [Modal.Custom — Slot 2]    │  │
│  │  [Modal.Custom — Slot 3]    │  │
│  ├─────────────────────────────┤  │
│  │  [Primary Button          ] │  │   ← Dock + ButtonGroup
│  │  [Secondary Button        ] │  │
│  │  Footer label               │  │
│  └─────────────────────────────┘  │
│  ▬  (Home indicator)              │
└───────────────────────────────────┘
```

### Desktop Sheet

```
┌─────────────────────────────────────────────────────────────────────┐  ← Scrim
│                                                                     │
│  ┌──────────────────┐              ┌────────────────────────────┐   │
│  │ [←]  Title  [✕] │   (Left)     │ (Center / Right variants)  │   │  ← TopNav
│  ├──────────────────┤              └────────────────────────────┘   │
│  │  Header          │                                               │
│  │  Content slots   │                                               │
│  ├──────────────────┤                                               │
│  │  [Button       ] │                                               │  ← Footer
│  └──────────────────┘                                               │
└─────────────────────────────────────────────────────────────────────┘
```

### Sub-Components (Parts — node `6065:19992`)

| Sub-component | Node | Dimensions | Description |
|---------------|------|-----------|-------------|
| `Modal.Sheet.TopNav` | `173:1232` | 370 × 72 px | Navigation bar at top of any sheet. Slots: back button (left), title (center), close button (right). Each slot is optional. |
| `Modal.Sheet.ImageStart` | `174:1067` | 402 × 317 px | Full-width hero image area placed at the top of a sheet, above the header. Supports aspect-ratio cropping. |
| `Modal.Custom` | `174:721` | 402 × 204 px | Freeform content slot for injecting lists, form fields, switches, or any custom body content into a sheet. |

---

## Size & Spacing Specifications

### Dialog

| Property | Value | Token |
|----------|-------|-------|
| Max width | 402 px | — |
| Border radius | 24 px | `tokens.base.border.radius[24]` |
| Padding top | 24 px | `tokens.base.spacing[24]` |
| Padding bottom | 16 px | `tokens.base.spacing[16]` |
| Padding horizontal | 16 px | `tokens.base.spacing[16]` |
| Gap (header → buttons) | 24 px | `tokens.base.spacing[24]` |
| Gap (title → description) | 8 px | `tokens.base.spacing[8]` |

### Mobile Sheet

| Property | Default | Inset | Full Screen |
|----------|---------|-------|-------------|
| Width | 100 vw | 100 vw − 16 px | 100 vw |
| Border radius (top) | 40 px | 40 px (all corners) | 40 px |
| Side margin | 0 | 8 px | 0 |
| Max height | ~80 vh (scrollable) | ~50 vh | ~100 vh |
| Grabber | Optional | Optional | Optional |

### Desktop Sheet

| Property | Value | Token |
|----------|-------|-------|
| Max width | 512 px | — |
| Border radius | 32 px | `tokens.base.border.radius[32]` |
| Outer vertical padding | 56 px | `tokens.base.spacing[56]` |
| Outer horizontal padding | 24 px | `tokens.base.spacing[24]` |
| Inner horizontal padding | 24 px | `tokens.base.spacing[24]` |
| Inner bottom padding | 24 px | `tokens.base.spacing[24]` |

### Modal.Sheet.TopNav

| Property | Value |
|----------|-------|
| Height | 72 px |
| Close / Back icon size | `IconButton` — medium, tertiary-contained |
| Title alignment | Center |
| Grabber | 40 × 5 px pill, `tokens.colorScheme.border.base`, 2 px from top |

---

## Style Variants

### Scrim

All modal variants share the same backdrop:

```
background: tokens.colorScheme.background.elevated.scrim
            → rgba(0, 0, 0, 0.63)
```

### Modal Surface

```
background: tokens.colorScheme.background.elevated.modal
            → white (light mode) / elevated dark surface (dark mode)
```

### Sheet Position (Desktop)

| Position | Alignment | Typical Use |
|----------|-----------|------------|
| Left | `align-items: flex-start` | Side navigation, settings drawers |
| Center | `align-items: center` | Rich modals, confirmations with complex content |
| Right | `align-items: flex-end` | Detail panels, auxiliary content |

---

## State Specifications

### Closed
- Scrim and modal surface are not rendered (or `display:none` / `visibility:hidden`)
- `aria-hidden="true"` on the modal portal if pre-rendered
- Scroll is enabled on the body

### Opening
- Scrim fades in (opacity 0 → 1)
- Modal surface animates in (see Animation Specifications)
- `document.body` receives `overflow:hidden` to prevent background scroll
- Focus trap is activated; focus moves to first focusable element inside the modal (or to the modal container if no focusable children)
- `aria-hidden="true"` is applied to all other page content outside the portal

### Open
- Modal surface fully visible; scrim at full opacity
- Focus is trapped inside the modal container
- Background content is inert
- `Escape` key triggers close (unless `preventClose` is set)
- Clicking the scrim triggers close (unless `preventClose` is set)

### Closing
- Exit animation plays
- Focus returns to the element that triggered the modal open
- `aria-hidden` is removed from background content
- `overflow:hidden` removed from `document.body`
- Modal portal unmounts (or hides)

### Scrollable Content
- When modal content exceeds the available height, the `Modal.Custom` content area scrolls independently
- The `Modal.Sheet.TopNav` and footer/Dock remain sticky at top and bottom respectively
- `-webkit-overflow-scrolling: touch` for smooth momentum scroll on iOS

---

## Props API

### TypeScript Interface

```typescript
import { ReactNode, CSSProperties } from 'react';

// ─── Shared ──────────────────────────────────────────────────────────────────

export type ModalPosition = 'left' | 'center' | 'right';
export type SheetVariant  = 'default' | 'inset' | 'full-screen';

// ─── Dialog ──────────────────────────────────────────────────────────────────

export interface DialogProps {
  /**
   * Whether the dialog is visible.
   */
  isOpen: boolean;

  /**
   * Callback fired when the dialog requests to close (Escape key, scrim click).
   */
  onClose: () => void;

  /**
   * Dialog heading text.
   */
  title: string;

  /**
   * Optional supporting description text rendered below the title.
   */
  description?: string;

  /**
   * Primary action button(s) rendered at the bottom of the dialog.
   * Typically a vertical ButtonGroup with primary + tertiary variants.
   */
  actions: ReactNode;

  /**
   * When true, disables Escape key and scrim-click dismissal.
   * Requires an explicit action to close.
   * @default false
   */
  preventClose?: boolean;

  /**
   * Additional class name applied to the modal surface.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Modal.Sheet.TopNav ──────────────────────────────────────────────────────

export interface ModalSheetTopNavProps {
  /**
   * Title displayed in the center of the TopNav bar.
   */
  title?: string;

  /**
   * When true, renders a back (←) button on the left side.
   * @default false
   */
  back?: boolean;

  /**
   * Callback for the back button. Required when back={true}.
   */
  onBack?: () => void;

  /**
   * When true, renders a close (✕) button on the right side.
   * @default true
   */
  showClose?: boolean;

  /**
   * Callback for the close button.
   */
  onClose?: () => void;

  /**
   * When true, renders the grabber pill at the very top of the sheet.
   * @default false
   */
  grabber?: boolean;

  /**
   * Trailing content (right slot), replaces the default close button.
   */
  trailingAction?: ReactNode;

  /**
   * Leading content (left slot), replaces the default back button.
   */
  leadingAction?: ReactNode;
}

// ─── Mobile Sheet ────────────────────────────────────────────────────────────

export interface MobileSheetProps {
  /**
   * Whether the sheet is visible.
   */
  isOpen: boolean;

  /**
   * Callback fired when the sheet requests to close.
   */
  onClose: () => void;

  /**
   * Sheet size variant.
   * @default 'default'
   */
  variant?: SheetVariant;

  /**
   * TopNav configuration for the sheet header.
   */
  topNav?: ModalSheetTopNavProps;

  /**
   * Optional hero image rendered at the top of the sheet, above the header.
   */
  imageStart?: ReactNode;

  /**
   * Main content area. Accepts any ReactNode; typically Header + Modal.Custom slots.
   */
  children: ReactNode;

  /**
   * Optional footer area (typically a Dock + ButtonGroup).
   */
  footer?: ReactNode;

  /**
   * When true, disables Escape key and backdrop-tap dismissal.
   * @default false
   */
  preventClose?: boolean;

  /**
   * Additional class name applied to the sheet surface.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Desktop Sheet ───────────────────────────────────────────────────────────

export interface DesktopSheetProps {
  /**
   * Whether the sheet is visible.
   */
  isOpen: boolean;

  /**
   * Callback fired when the sheet requests to close.
   */
  onClose: () => void;

  /**
   * Horizontal position of the sheet within the viewport.
   * @default 'left'
   */
  position?: ModalPosition;

  /**
   * TopNav configuration for the sheet header.
   */
  topNav?: ModalSheetTopNavProps;

  /**
   * Whether the header section (Header sub-component) is visible.
   * @default true
   */
  header?: boolean;

  /**
   * Whether the content section is visible.
   * @default true
   */
  content?: boolean;

  /**
   * Whether the footer section is visible.
   * @default true
   */
  footer?: boolean;

  /**
   * Main content area.
   */
  children: ReactNode;

  /**
   * When true, disables Escape key and scrim-click dismissal.
   * @default false
   */
  preventClose?: boolean;

  /**
   * Additional class name applied to the sheet surface.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Modal.Custom ────────────────────────────────────────────────────────────

export interface ModalCustomProps {
  /**
   * Custom content rendered inside the scrollable body area.
   */
  children: ReactNode;

  /**
   * Whether the content area should scroll when it overflows.
   * @default true
   */
  scrollable?: boolean;

  /**
   * Additional class name applied to the content container.
   */
  className?: string;

  /**
   * Inline style overrides.
   */
  style?: CSSProperties;
}
```

---

## Code Examples

### 1. Basic Confirmation Dialog

```tsx
import { useState } from 'react';
import { Dialog, Button, ButtonGroup } from '@ds/components';

export function DeleteConfirmation({ onDelete }: { onDelete: () => void }) {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <Button variant="danger" onClick={() => setIsOpen(true)}>
        Delete account
      </Button>

      <Dialog
        isOpen={isOpen}
        onClose={() => setIsOpen(false)}
        title="Delete your account?"
        description="This action is permanent and cannot be undone. All your data will be removed within 30 days."
        actions={
          <ButtonGroup alignment="vertical">
            <Button size="large" variant="primary" onClick={onDelete}>
              Yes, delete account
            </Button>
            <Button size="large" variant="tertiary" onClick={() => setIsOpen(false)}>
              Cancel
            </Button>
          </ButtonGroup>
        }
      />
    </>
  );
}
```

### 2. Mobile Sheet — Default (Bottom Sheet)

```tsx
import { useState } from 'react';
import { MobileSheet, ModalSheetTopNav, Header, ModalCustom, Dock, ButtonGroup, Button, List, MenuItem } from '@ds/components';

export function PaymentSheet() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <Button onClick={() => setIsOpen(true)}>Pay now</Button>

      <MobileSheet
        isOpen={isOpen}
        onClose={() => setIsOpen(false)}
        variant="default"
        topNav={{ title: 'Choose payment', showClose: true, onClose: () => setIsOpen(false) }}
        footer={
          <Dock>
            <ButtonGroup alignment="vertical">
              <Button size="large" variant="primary" onClick={handlePay}>Continue</Button>
              <Button size="large" variant="secondary" onClick={() => setIsOpen(false)}>Cancel</Button>
            </ButtonGroup>
          </Dock>
        }
      >
        <Header title="Payment method" description="Select how you'd like to pay" />
        <ModalCustom>
          <List>
            <MenuItem label="PayPal balance" description="$230.00 available" leadingContent={<PayPalIcon />} />
            <MenuItem label="Visa ••••4242"  description="Expires 04/27"     leadingContent={<VisaIcon />}   />
            <MenuItem label="Add new method" leadingContent={<PlusIcon />} />
          </List>
        </ModalCustom>
      </MobileSheet>
    </>
  );
}
```

### 3. Mobile Sheet — Inset Variant

```tsx
import { MobileSheet, Header, ModalCustom, Button, ButtonGroup } from '@ds/components';

export function QuickConfirmSheet({ isOpen, onClose, onConfirm }) {
  return (
    <MobileSheet
      isOpen={isOpen}
      onClose={onClose}
      variant="inset"
      topNav={{ grabber: true, showClose: false }}
      footer={
        <ButtonGroup alignment="vertical">
          <Button size="large" variant="primary"   onClick={onConfirm}>Confirm</Button>
          <Button size="large" variant="tertiary"  onClick={onClose}>Not now</Button>
        </ButtonGroup>
      }
    >
      <Header
        title="Send $50.00?"
        description="To John Smith · PayPal balance"
        amount="$50.00"
      />
    </MobileSheet>
  );
}
```

### 4. Mobile Sheet — Full Screen

```tsx
import { MobileSheet, ModalSheetTopNav, Header, ModalCustom, Dock, Button } from '@ds/components';
import { AddressForm } from './AddressForm';

export function AddressSheet({ isOpen, onClose, onSave }) {
  return (
    <MobileSheet
      isOpen={isOpen}
      onClose={onClose}
      variant="full-screen"
      topNav={{
        title: 'Add address',
        back: true,
        onBack: onClose,
        showClose: false,
      }}
      footer={
        <Dock footerLabel="Your address is encrypted and stored securely.">
          <Button size="large" variant="primary" onClick={onSave} fullWidth>
            Save address
          </Button>
        </Dock>
      }
    >
      <ModalCustom>
        <AddressForm />
      </ModalCustom>
    </MobileSheet>
  );
}
```

### 5. Mobile Sheet — With Hero Image

```tsx
import { MobileSheet, ModalSheetImageStart, Header, ModalCustom, Button, ButtonGroup } from '@ds/components';

export function PromoSheet({ isOpen, onClose, promo }) {
  return (
    <MobileSheet
      isOpen={isOpen}
      onClose={onClose}
      variant="inset"
      topNav={{ showClose: true, onClose }}
      imageStart={
        <ModalSheetImageStart
          src={promo.imageUrl}
          alt={promo.imageAlt}
          aspectRatio="16/9"
        />
      }
      footer={
        <ButtonGroup alignment="vertical">
          <Button size="large" variant="primary"  onClick={() => applyPromo(promo.code)}>Apply offer</Button>
          <Button size="large" variant="tertiary" onClick={onClose}>No thanks</Button>
        </ButtonGroup>
      }
    >
      <Header
        title={promo.title}
        description={promo.description}
      />
    </MobileSheet>
  );
}
```

### 6. Desktop Sheet — Left (Side Drawer)

```tsx
import { DesktopSheet, Header, ModalCustom, Button, List, MenuItem } from '@ds/components';

export function SettingsDrawer({ isOpen, onClose }) {
  return (
    <DesktopSheet
      isOpen={isOpen}
      onClose={onClose}
      position="left"
      topNav={{ title: 'Settings', showClose: true, onClose }}
    >
      <Header title="Account settings" description="Manage your preferences" />
      <ModalCustom>
        <List>
          <MenuItem label="Profile"          leadingContent={<PersonIcon />}   onSelect={() => navigate('/profile')}   />
          <MenuItem label="Security"         leadingContent={<LockIcon />}     onSelect={() => navigate('/security')}  />
          <MenuItem label="Notifications"    leadingContent={<BellIcon />}     onSelect={() => navigate('/notifs')}    />
          <MenuItem label="Privacy"          leadingContent={<ShieldIcon />}   onSelect={() => navigate('/privacy')}   />
          <MenuItem label="Payment methods"  leadingContent={<CardIcon />}     onSelect={() => navigate('/payments')}  />
        </List>
      </ModalCustom>
    </DesktopSheet>
  );
}
```

### 7. Desktop Sheet — Center (Rich Content Modal)

```tsx
import { DesktopSheet, Header, ModalCustom, Button, ButtonGroup, Form } from '@ds/components';

export function EditProfileModal({ isOpen, onClose, user }) {
  return (
    <DesktopSheet
      isOpen={isOpen}
      onClose={onClose}
      position="center"
      topNav={{ title: 'Edit profile', showClose: true, onClose }}
      footer={
        <ButtonGroup alignment="vertical">
          <Button size="large" variant="primary"   type="submit" form="profile-form">Save changes</Button>
          <Button size="large" variant="tertiary"  onClick={onClose}>Discard</Button>
        </ButtonGroup>
      }
    >
      <Header title="Your profile" description="Update your name, photo, and contact details." />
      <ModalCustom>
        <Form id="profile-form" onSubmit={handleSave}>
          <Input label="First name"   name="firstName" defaultValue={user.firstName} />
          <Input label="Last name"    name="lastName"  defaultValue={user.lastName}  />
          <Input label="Email"        name="email"     defaultValue={user.email}     />
        </Form>
      </ModalCustom>
    </DesktopSheet>
  );
}
```

### 8. Desktop Sheet — Right (Detail Panel)

```tsx
import { DesktopSheet, Header, ModalCustom, Button } from '@ds/components';
import { TransactionDetail } from './TransactionDetail';

export function TransactionPanel({ isOpen, onClose, transaction }) {
  return (
    <DesktopSheet
      isOpen={isOpen}
      onClose={onClose}
      position="right"
      topNav={{
        title: 'Transaction detail',
        showClose: true,
        onClose,
      }}
    >
      <Header
        title={transaction.merchant}
        description={transaction.date}
        amount={transaction.amount}
      />
      <ModalCustom>
        <TransactionDetail transaction={transaction} />
      </ModalCustom>
    </DesktopSheet>
  );
}
```

### 9. Controlled Dialog with Async Action

```tsx
import { useState } from 'react';
import { Dialog, Button, ButtonGroup, Loader } from '@ds/components';

export function LogoutDialog({ isOpen, onClose }) {
  const [isLoggingOut, setIsLoggingOut] = useState(false);

  const handleLogout = async () => {
    setIsLoggingOut(true);
    try {
      await logout();
      onClose();
    } catch {
      setIsLoggingOut(false);
    }
  };

  return (
    <Dialog
      isOpen={isOpen}
      onClose={onClose}
      title="Sign out?"
      description="You'll need to sign back in to access your account."
      preventClose={isLoggingOut}
      actions={
        <ButtonGroup alignment="vertical">
          <Button
            size="large"
            variant="primary"
            onClick={handleLogout}
            disabled={isLoggingOut}
            aria-busy={isLoggingOut}
          >
            {isLoggingOut
              ? <><Loader size="x-small" color="inverse" label="" aria-hidden /> Signing out…</>
              : 'Sign out'
            }
          </Button>
          <Button
            size="large"
            variant="tertiary"
            onClick={onClose}
            disabled={isLoggingOut}
          >
            Cancel
          </Button>
        </ButtonGroup>
      }
    />
  );
}
```

### 10. Multi-Step Sheet with Back Navigation

```tsx
import { useState } from 'react';
import { MobileSheet, Header, ModalCustom, Dock, Button } from '@ds/components';

type Step = 'amount' | 'recipient' | 'confirm';

export function SendMoneyFlow({ isOpen, onClose }) {
  const [step, setStep] = useState<Step>('amount');

  const STEPS: Record<Step, { title: string; prev?: Step }> = {
    amount:    { title: 'Enter amount' },
    recipient: { title: 'Choose recipient', prev: 'amount' },
    confirm:   { title: 'Review & send',    prev: 'recipient' },
  };

  const current = STEPS[step];

  return (
    <MobileSheet
      isOpen={isOpen}
      onClose={onClose}
      variant="full-screen"
      topNav={{
        title: current.title,
        back: !!current.prev,
        onBack: current.prev ? () => setStep(current.prev!) : undefined,
        showClose: !current.prev,
        onClose,
      }}
      footer={
        <Dock>
          <Button
            size="large"
            variant="primary"
            fullWidth
            onClick={() => {
              if (step === 'amount')     setStep('recipient');
              else if (step === 'recipient') setStep('confirm');
              else handleSend();
            }}
          >
            {step === 'confirm' ? 'Send now' : 'Continue'}
          </Button>
        </Dock>
      }
    >
      <ModalCustom>
        {step === 'amount'    && <AmountStep />}
        {step === 'recipient' && <RecipientStep />}
        {step === 'confirm'   && <ConfirmStep />}
      </ModalCustom>
    </MobileSheet>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern — Dialog

All Modal variants implement the [ARIA Dialog (Modal) pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/):

```html
<!-- Scrim / portal root -->
<div aria-hidden="true" id="app-root">
  <!-- all background content -->
</div>

<!-- Modal portal -->
<div>
  <!-- Scrim backdrop (presentational) -->
  <div aria-hidden="true" class="scrim" />

  <!-- Modal container -->
  <div
    role="dialog"
    aria-modal="true"
    aria-labelledby="modal-title"
    aria-describedby="modal-description"
    tabindex="-1"
  >
    <h2 id="modal-title">Delete your account?</h2>
    <p id="modal-description">
      This action is permanent and cannot be undone.
    </p>
    <button>Yes, delete account</button>
    <button>Cancel</button>
  </div>
</div>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Modal container | `"dialog"` |
| `aria-modal` | Modal container | `"true"` |
| `aria-labelledby` | Modal container | ID of the title element |
| `aria-describedby` | Modal container | ID of the description element (if present) |
| `aria-hidden` | Background content | `"true"` (while modal is open) |
| `tabindex` | Modal container | `"-1"` (to receive programmatic focus) |
| `aria-label` | Close button | `"Close"` |
| `aria-label` | Back button | `"Go back"` |

### Focus Management

**On open:**

1. All content outside the modal portal receives `aria-hidden="true"` and `inert`
2. Focus trap is activated — Tab and Shift+Tab cycle only among focusable elements inside the modal
3. Focus is moved to:
   - The first interactive element inside the modal (preferred), or
   - The modal container itself (`tabindex="-1"`) if no interactive element is a better starting point

```tsx
// Focus the first focusable element on open
useEffect(() => {
  if (!isOpen) return;
  const modal = modalRef.current;
  const focusable = modal?.querySelector<HTMLElement>(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  );
  focusable?.focus();
}, [isOpen]);
```

**On close:**

1. `aria-hidden` and `inert` are removed from background content
2. Focus is returned to the element that triggered the modal:

```tsx
const triggerRef = useRef<HTMLElement | null>(null);

const openModal = () => {
  triggerRef.current = document.activeElement as HTMLElement;
  setIsOpen(true);
};

const closeModal = () => {
  setIsOpen(false);
  triggerRef.current?.focus();
};
```

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| `Tab` | Moves focus to the next focusable element; wraps from last back to first |
| `Shift + Tab` | Moves focus to the previous focusable element; wraps from first to last |
| `Escape` | Closes the modal and returns focus to the trigger (unless `preventClose` is true) |

### Scroll Lock

When a modal opens, prevent background scroll:

```tsx
useEffect(() => {
  if (isOpen) {
    const scrollY = window.scrollY;
    document.body.style.position   = 'fixed';
    document.body.style.top        = `-${scrollY}px`;
    document.body.style.width      = '100%';
    return () => {
      document.body.style.position = '';
      document.body.style.top      = '';
      document.body.style.width    = '';
      window.scrollTo(0, scrollY);
    };
  }
}, [isOpen]);
```

### Screen Reader Announcements

When the modal opens, screen readers announce:
- `"[Title], dialog"` — from `role="dialog"` + `aria-labelledby`

When the modal closes, screen readers resume normal page context.

For multi-step flows, update `aria-labelledby` to reference the new step title on each step transition.

### Color Contrast

| Element | Minimum Ratio | WCAG |
|---------|---------------|------|
| Title text on modal surface | 4.5:1 | 1.4.3 |
| Description text | 4.5:1 | 1.4.3 |
| Button labels | 4.5:1 | 1.4.3 |
| Close / Back icon | 3:1 | 1.4.11 |
| Scrim on background | — | Not applicable (presentational) |

---

## Animation Specifications

### Dialog — Open (Fade + Scale)

```css
@keyframes dialog-open {
  from {
    opacity: 0;
    transform: scale(0.95) translateY(8px);
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

.dialog-surface[data-state='open'] {
  animation: dialog-open 200ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Dialog — Close (Fade Out)

```css
@keyframes dialog-close {
  from { opacity: 1; transform: scale(1); }
  to   { opacity: 0; transform: scale(0.96); }
}

.dialog-surface[data-state='closing'] {
  animation: dialog-close 150ms ease-in;
}
```

### Mobile Sheet — Open (Slide Up)

```css
@keyframes sheet-slide-up {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}

.mobile-sheet {
  animation: sheet-slide-up 350ms cubic-bezier(0.32, 0.72, 0, 1);
}
```

### Mobile Sheet — Close (Slide Down)

```css
@keyframes sheet-slide-down {
  from { transform: translateY(0); }
  to   { transform: translateY(100%); }
}

.mobile-sheet[data-state='closing'] {
  animation: sheet-slide-down 280ms cubic-bezier(0.4, 0, 0.6, 1);
}
```

### Desktop Sheet — Open (Slide In from Edge)

```css
/* Left drawer */
@keyframes drawer-slide-left {
  from { transform: translateX(-100%); opacity: 0; }
  to   { transform: translateX(0);     opacity: 1; }
}

/* Right drawer */
@keyframes drawer-slide-right {
  from { transform: translateX(100%); opacity: 0; }
  to   { transform: translateX(0);    opacity: 1; }
}

.desktop-sheet[data-position='left'][data-state='open'] {
  animation: drawer-slide-left 300ms cubic-bezier(0.16, 1, 0.3, 1);
}
.desktop-sheet[data-position='right'][data-state='open'] {
  animation: drawer-slide-right 300ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Scrim — Fade

```css
@keyframes scrim-in  { from { opacity: 0; } to { opacity: 1; } }
@keyframes scrim-out { from { opacity: 1; } to { opacity: 0; } }

.scrim[data-state='open']    { animation: scrim-in  200ms ease; }
.scrim[data-state='closing'] { animation: scrim-out 200ms ease; }
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .dialog-surface,
  .mobile-sheet,
  .desktop-sheet,
  .scrim {
    animation: none;
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Surfaces ────────────────────────────────────────────────────────── */
--modal-bg:                  var(--tokens.colorScheme.background.elevated.modal);
--modal-scrim-bg:            var(--tokens.colorScheme.background.elevated.scrim);  /* rgba(0,0,0,0.63) */

/* ─── Border Radius ───────────────────────────────────────────────────── */
--dialog-border-radius:      var(--tokens.base.border.radius[24]);    /* 24px */
--sheet-mobile-radius-top:   40px;                                     /* top corners only */
--sheet-inset-radius:        40px;                                     /* all corners */
--sheet-desktop-radius:      var(--tokens.base.border.radius[32]);    /* 32px */

/* ─── Spacing — Dialog ────────────────────────────────────────────────── */
--dialog-padding-top:        var(--tokens.base.spacing[24]);   /* 24px */
--dialog-padding-bottom:     var(--tokens.base.spacing[16]);   /* 16px */
--dialog-padding-horizontal: var(--tokens.base.spacing[16]);   /* 16px */
--dialog-gap-header-actions: var(--tokens.base.spacing[24]);   /* 24px */
--dialog-gap-title-desc:     var(--tokens.base.spacing[8]);    /* 8px */
--dialog-max-width:          402px;

/* ─── Spacing — Desktop Sheet ─────────────────────────────────────────── */
--sheet-desktop-outer-v:     var(--tokens.base.spacing[56]);   /* 56px */
--sheet-desktop-outer-h:     var(--tokens.base.spacing[24]);   /* 24px */
--sheet-desktop-inner-h:     var(--tokens.base.spacing[24]);   /* 24px */
--sheet-desktop-inner-b:     var(--tokens.base.spacing[24]);   /* 24px */
--sheet-desktop-max-width:   512px;

/* ─── TopNav ──────────────────────────────────────────────────────────── */
--modal-topnav-height:       72px;
--modal-grabber-width:       40px;
--modal-grabber-height:      5px;
--modal-grabber-color:       var(--tokens.colorScheme.border.base);  /* rgba(0,0,0,0.16) */
--modal-grabber-radius:      var(--tokens.base.border.radius.full);  /* 999px */

/* ─── Typography — Title ──────────────────────────────────────────────── */
--modal-title-family:        var(--tokens.brand.text.fontfamily.heading);
--modal-title-size:          var(--tokens.responsive.text.fontsize.heading.md);
--modal-title-weight:        var(--tokens.brand.text.fontweight.heading);   /* Black */
--modal-title-line-height:   var(--tokens.responsive.text.lineheight.heading.md);
--modal-title-letter-spacing: var(--tokens.responsive.text.letterspacing.heading.md);

/* ─── Typography — Description ────────────────────────────────────────── */
--modal-desc-family:         var(--tokens.brand.text.fontfamily.body);
--modal-desc-size:           var(--tokens.base.text.fontsize.ramp[16]);    /* 16px */
--modal-desc-weight:         var(--tokens.brand.text.fontweight.body);     /* Regular */
--modal-desc-line-height:    var(--tokens.base.text.lineheight.ramp[16]);  /* 24px */

/* ─── Z-index ─────────────────────────────────────────────────────────── */
--modal-z-index:             1000;
--modal-scrim-z-index:       999;
```

---

## Best Practices

### Do

- **Use the Dialog for short, high-priority interruptions** — Confirmations, destructive-action warnings, or session expiry notices are ideal. Keep the title under 6 words and the description under 2 sentences.
- **Use the Mobile Sheet for complex selections or flows** — Bottom sheets are more ergonomic on touch devices and feel less disruptive than full-page navigations for tasks like payment selection or address entry.
- **Always provide a visible close mechanism** — Whether it's a close button in the TopNav, a cancel action in the ButtonGroup, or both, users must be able to dismiss the modal without completing the action.
- **Use `preventClose` sparingly** — Only prevent scrim and Escape dismissal when losing the user's in-progress data would be genuinely harmful (e.g., mid-form with unsaved data). Confirm with a Dialog if the user tries to close a `preventClose` modal with work in progress.
- **Trap focus and hide background content** — Both are required for accessibility; do not skip `aria-hidden` on background content or the focus trap implementation.
- **Return focus to the trigger** — This is mandatory for keyboard and AT users to maintain their position in the page.
- **Use `aria-describedby` for the description** — Linking the description to the dialog container ensures assistive technology reads it on open.
- **Use the Full Screen variant for complex multi-step flows** — If a task requires 3+ steps, more than one input type, or multi-page navigation, upgrade from Default sheet to Full Screen to give content room to breathe.

### Don't

- **Don't stack modals** — Nested or stacked modals are disorienting. If a second interruption is needed from within a modal, use an inline contextual alert, a Banner, or a Toast instead.
- **Don't put too much content in a Dialog** — A Dialog is for short messages + 1–2 actions. If you need a form, a Header, and multiple content sections, use a Sheet instead.
- **Don't trigger a modal for non-interruptive feedback** — Use Toast for success confirmations, Banner for warnings, and Contextual Alert for inline errors. Modals should be reserved for situations requiring the user's explicit response.
- **Don't auto-open modals on page load** — Avoid opening a modal immediately when a page renders; this is disorienting and fails cookie-consent patterns. Use a deliberate user trigger.
- **Don't use the Inset variant for long-scrolling content** — The inset corner radius (`40 px` all-around) limits vertical space. Use Default or Full Screen for taller content.
- **Don't rely on the scrim alone as a close mechanism on mobile** — The scrim tap area is small near the sheet edge. Always provide a visible close button.
- **Don't forget scroll lock** — Without locking the background scroll, users on iOS Safari will see the page scroll beneath the modal, breaking the modal pattern.

---

## Modal vs. Other Overlay Components

| Component | When to Use |
|-----------|------------|
| **Dialog** | Short confirmations, destructive action warnings, critical alerts requiring 1–2 actions |
| **Mobile Sheet** | Touch-first selection flows, payment methods, address pickers, multi-step tasks |
| **Desktop Sheet** | Drawers (navigation, settings, detail panels), center-positioned rich content modals |
| **Menu** | Contextual action lists triggered by a specific element; no form inputs |
| **Popover** | Non-blocking contextual content anchored to a trigger; can be dismissed by clicking outside |
| **Toast / Snackbar** | Non-blocking, transient feedback that auto-dismisses |
| **Banner** | Persistent inline messages that do not interrupt the user flow |
| **Coachtip** | Onboarding guidance overlaid on a specific UI element |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Button / ButtonGroup** | Primary actions inside all Modal variants; vertical alignment in modals |
| **Header** | Standard title/description/amount block used inside sheets |
| **Dock** | Footer container with safe-area awareness; hosts ButtonGroup at the bottom of mobile sheets |
| **Loader** | X-Small Loader used inside buttons during async modal actions |
| **List / MenuItem** | Common content inside `Modal.Custom` for selection sheets |
| **Input / Form** | Content inside `Modal.Custom` for data-entry sheets |
| **Icon Button** | Used in `Modal.Sheet.TopNav` for close and back actions |
| **Banner / Contextual Alert** | Inline feedback inside an open modal for form errors or warnings |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Unified Dialog + Mobile Sheet + Desktop Sheet under single `Modal` family; introduced `Modal.Sheet.TopNav`, `Modal.Sheet.ImageStart`, `Modal.Custom` sub-components; added Inset and Full Screen mobile variants; added Left/Center/Right desktop positioning |
| 5.2 | Added `grabber` prop to Mobile Sheet; standardised `preventClose` behavior |
| 5.0 | Added Desktop Sheet (drawer) with Left and Right positions |
| 4.3 | Added `Modal.Sheet.ImageStart` for hero-image sheets |
| 4.0 | Initial Modal release with Dialog and basic Mobile Sheet |

---

## Testing Checklist

### Visual
- [ ] Dialog renders with 24 px border radius and correct max-width (402 px)
- [ ] Dialog title uses Heading/Medium (Black weight, responsive size)
- [ ] Dialog description uses Body/Large (Regular weight, 16 px)
- [ ] Dialog spacing: 24 px top padding, 16 px horizontal, 16 px bottom, 24 px header-to-buttons gap
- [ ] Mobile Sheet Default has rounded top corners (40 px), full width
- [ ] Mobile Sheet Inset has 40 px all-corner radius, 8 px side margins
- [ ] Mobile Sheet Full Screen fills viewport height
- [ ] Grabber pill renders at correct size (40 × 5 px) and position when `grabber={true}`
- [ ] Desktop Sheet max-width is 512 px with 32 px border radius
- [ ] Desktop Sheet Left/Center/Right positions render correctly
- [ ] `Modal.Sheet.TopNav` renders at 72 px height with back/close buttons
- [ ] `Modal.Sheet.ImageStart` renders hero image at full width with correct aspect ratio
- [ ] Scrim renders at correct opacity (0.63) across all variants

### Behavior
- [ ] Modal opens on trigger click
- [ ] Scrim fades in/out correctly
- [ ] Sheet slide-up animation plays on mobile open
- [ ] Dialog scale+fade animation plays on open
- [ ] Drawer slide-in animation plays on desktop open
- [ ] Modal closes on `Escape` key press (unless `preventClose={true}`)
- [ ] Modal closes on scrim click (unless `preventClose={true}`)
- [ ] Modal closes on close button click
- [ ] `preventClose={true}` blocks Escape and scrim-click dismissal
- [ ] Background scroll is locked while modal is open
- [ ] Background scroll is restored correctly on close (including scroll position)
- [ ] Multi-step sheet advances steps and updates TopNav title
- [ ] Back button in TopNav navigates to previous step

### Accessibility
- [ ] Modal container has `role="dialog"` and `aria-modal="true"`
- [ ] `aria-labelledby` references the modal title element
- [ ] `aria-describedby` references the description element (when present)
- [ ] Background content receives `aria-hidden="true"` and `inert` while modal is open
- [ ] `aria-hidden` and `inert` are removed from background on close
- [ ] Focus moves into the modal on open (to first focusable element or container)
- [ ] Focus is trapped within the modal (Tab/Shift+Tab cycle only inside)
- [ ] Focus returns to the trigger element on close
- [ ] Close button has `aria-label="Close"`
- [ ] Back button has `aria-label="Go back"`
- [ ] Screen reader announces dialog title on open
- [ ] All text meets 4.5:1 contrast ratio
- [ ] Close/back icons meet 3:1 non-text contrast

### Integration
- [ ] React portal renders modal at the `<body>` level, not nested in the trigger's DOM position
- [ ] `isOpen` / `onClose` work correctly in all variants
- [ ] `data-testid` attributes are forwarded to the correct elements
- [ ] `Loader` inside a button correctly shows `aria-busy` state during async actions
- [ ] Modal dismisses correctly after an async action completes (success and error paths)
