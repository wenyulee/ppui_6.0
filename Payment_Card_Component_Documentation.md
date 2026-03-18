# Payment Card Component Documentation

## Overview

The Payment Card component renders a visual representation of a physical payment card — debit, credit, or prepaid — within the PayPal design system. It comes in two size variants: a compact **Thumbnail** for inline use in lists, menus, and selection rows, and a full-size **Display** card for confirmation screens, wallet detail views, and featured payment method surfaces.

Both variants accept a `cardImage` slot that renders the card's artwork. The default artwork is the **PayPal Debit** card — a deep blue card carrying the PayPal wordmark in light blue and the Mastercard Debit logo at the bottom-right. Custom card images (Visa, Amex, PayPal Credit, virtual cards, etc.) slot in via the `cardImage` prop.

**Component name:** `PaymentCard` / `PaymentCardThumbnail` / `PaymentCardDisplay`
**Figma nodes:** `17166-19760` (Thumbnail), `17166-19765` (Display)
**Platform:** React / TypeScript (Web), SwiftUI (iOS — `PDSPaymentCard`)
**Category:** Finance / Display

---

## Component Variants

### By Size

| Variant | Node | Dimensions | Typical Use |
|---------|------|-----------|-------------|
| **Thumbnail** | `17166:19760` | 48 × 32 px | Payment method rows in lists, menus, selectors, and transaction history |
| **Display** | `17166:19765` | 370 × 246 px | Wallet detail page, payment confirmation screen, featured payment method hero |

### By Card Type (via `cardImage`)

| Card Type | Art Description |
|-----------|----------------|
| **PayPal Debit** (default) | Deep blue background, "PayPal" wordmark in light blue, Mastercard Debit logo bottom-right |
| **PayPal Credit** | Custom PayPal Credit card artwork |
| **Visa** | Card network branding with issuer background |
| **Mastercard** | Red/yellow Mastercard circles, issuer background |
| **Amex** | American Express centurion artwork |
| **Virtual / Custom** | Any `ReactNode` or `<img>` passed to the `cardImage` slot |

---

## Anatomy

### Thumbnail

```
┌────────────────────┐
│  PayPal  [MC logo] │  ← 48 × 32 px card art image
└────────────────────┘
```

### Display

```
┌──────────────────────────────────────────┐
│                                          │
│   PayPal                                 │
│   (large wordmark, light blue)           │
│                                          │
│                                          │
│                     Debit  [●●] MC logo  │  ← bottom-right
└──────────────────────────────────────────┘
  370 × 246 px, 24 px border radius, 1 px border
```

### Sub-Component: `PayPalDebit` Card Art

| Property | Value |
|----------|-------|
| Thumbnail dimensions | 48 × 32 px |
| Display dimensions | Fills parent (370 × 246 px) |
| Object fit | `cover` |
| Pointer events | None (decorative image) |
| Alt text | `""` (decorative; card identity conveyed by accessible label on parent) |

---

## Size & Spacing Specifications

### Display Card Container

| Property | Value | Token |
|----------|-------|-------|
| Width | 370 px | Fixed |
| Height | 246 px | Fixed |
| Border radius | 24 px | `tokens.base.border.radius[24]` |
| Border width | 1 px | `tokens.base.border.size[1]` |
| Border color | rgba(5, 55, 130, 0.08) | `tokens.colorScheme.border.muted` |
| Gap | 8 px | `tokens.base.spacing[8]` |
| Overflow | `clip` | — |
| Alignment | `items-end` | Card art anchored to bottom |

### Thumbnail

| Property | Value |
|----------|-------|
| Width | 48 px |
| Height | 32 px |
| Border radius | Standard card radius (proportional) |
| Object fit | `cover` |

---

## Style Variants

### Border Treatment

The Display card uses a very subtle `border.muted` border (rgba(5,55,130,0.08) — near-transparent blue) to define the card edge against a white or light background without a heavy stroke. On dark backgrounds, the border may be omitted or replaced with a white/translucent border.

### Rounded Corners

Both variants use rounded corners consistent with ISO/IEC 7810 physical card proportions:
- **Display:** 24 px radius (`tokens.base.border.radius[24]`)
- **Thumbnail:** Proportionally scaled radius

### Card Art

Card artwork is always image-based, never recreated in CSS. Pass actual card art assets via the `cardImage` prop:
- PNG or WebP recommended for gradient-heavy card artwork
- SVG acceptable for flat, single-color card designs
- All card images should be provided at 2× resolution minimum for retina displays

---

## State Specifications

### Default / Rest
- Card art fills the container
- No interactive behavior; purely presentational

### Selected (within a selection list)
- Parent component (e.g., a `ListItem` or `RadioCard`) applies a selected indicator
- The PaymentCard itself does not apply a selected state visually — the selection affordance lives in the parent
- Pair with a checkmark, radio button, or highlighted border on the wrapping interactive element

### Loading / Skeleton
- Before card data has loaded, render a Skeleton of matching dimensions:

```tsx
<Skeleton width={370} height={246} borderRadius={24} />   // Display
<Skeleton width={48}  height={32}  borderRadius={4} />    // Thumbnail
```

### Error / Unknown Card
- When card type cannot be determined, render a neutral placeholder card art
- Avoid showing broken image states; always provide a fallback card image

---

## Props API

### TypeScript Interface

```typescript
import { ReactNode, CSSProperties } from 'react';

// ─── PaymentCardThumbnail ────────────────────────────────────────────────────

export interface PaymentCardThumbnailProps {
  /**
   * Card artwork rendered inside the thumbnail container.
   * Accepts an <img>, <svg>, or any ReactNode.
   * Defaults to the PayPal Debit card art.
   */
  cardImage?: ReactNode | null;

  /**
   * Accessible label for the card thumbnail.
   * Used when the thumbnail is standalone (not within a labeled list item).
   * Example: "PayPal Debit card ending in 4242"
   */
  'aria-label'?: string;

  /**
   * Additional class name(s) applied to the thumbnail container.
   */
  className?: string;

  /**
   * Inline style overrides.
   */
  style?: CSSProperties;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── PaymentCardDisplay ──────────────────────────────────────────────────────

export interface PaymentCardDisplayProps {
  /**
   * Card artwork rendered inside the display container.
   * Accepts an <img>, <svg>, or any ReactNode.
   * Defaults to the PayPal Debit card art (full-size).
   */
  cardImage?: ReactNode | null;

  /**
   * Accessible label for the displayed card.
   * Describes the card type and masked number to assistive technology.
   * Example: "PayPal Debit card ending in 4242"
   * @required for standalone use; can be omitted when the parent container already provides context
   */
  'aria-label'?: string;

  /**
   * Whether to show the subtle 1 px border around the card.
   * Disable on dark/colored backgrounds where the border adds visual noise.
   * @default true
   */
  showBorder?: boolean;

  /**
   * Additional class name(s) applied to the display container.
   */
  className?: string;

  /**
   * Inline style overrides.
   */
  style?: CSSProperties;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── PaymentCard (Unified) ────────────────────────────────────────────────────

export type PaymentCardSize = 'thumbnail' | 'display';

export interface PaymentCardProps {
  /**
   * Size variant of the card.
   * 'thumbnail' → 48 × 32 px
   * 'display'   → 370 × 246 px
   * @default 'thumbnail'
   */
  size?: PaymentCardSize;

  /**
   * Card network identifier used to resolve the default card artwork.
   * When cardImage is also provided, cardImage takes precedence.
   */
  network?: 'paypal-debit' | 'paypal-credit' | 'visa' | 'mastercard' | 'amex' | 'discover' | 'unknown';

  /**
   * Custom card artwork. Overrides the network-based default art.
   */
  cardImage?: ReactNode | null;

  /**
   * Last 4 digits of the card number, used for accessible labels.
   * Example: "4242"
   */
  lastFour?: string;

  /**
   * Accessible label. Auto-generated from network + lastFour when not provided.
   * Example: "Visa card ending in 4242"
   */
  'aria-label'?: string;

  /**
   * Whether to show the card border (Display variant only).
   * @default true
   */
  showBorder?: boolean;

  /**
   * Additional class name(s) applied to the root container.
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

### 1. Default PayPal Debit — Thumbnail

```tsx
import { PaymentCardThumbnail } from '@ds/components';

export function PaymentMethodRow() {
  return (
    <div className="flex items-center gap-3">
      <PaymentCardThumbnail aria-label="PayPal Debit card ending in 4242" />
      <div>
        <p className="font-medium">PayPal Debit</p>
        <p className="text-muted text-sm">••••4242</p>
      </div>
    </div>
  );
}
```

### 2. Default PayPal Debit — Display Card

```tsx
import { PaymentCardDisplay } from '@ds/components';

export function WalletCardDetail() {
  return (
    <section aria-label="Your PayPal Debit card">
      <PaymentCardDisplay aria-label="PayPal Debit card ending in 4242" />
      <div className="mt-4">
        <p>PayPal Debit • Mastercard</p>
        <p className="text-muted">••••  ••••  ••••  4242</p>
        <p className="text-muted">Expires 04/28</p>
      </div>
    </section>
  );
}
```

### 3. Custom Card Art — Visa

```tsx
import { PaymentCardDisplay } from '@ds/components';
import visaCardArt from '@/assets/cards/visa-art.png';

export function VisaCardDisplay({ card }) {
  return (
    <PaymentCardDisplay
      cardImage={
        <img
          src={visaCardArt}
          alt=""
          style={{ width: '100%', height: '100%', objectFit: 'cover' }}
        />
      }
      aria-label={`Visa card ending in ${card.lastFour}`}
    />
  );
}
```

### 4. Payment Method Selector List

```tsx
import { useState } from 'react';
import { PaymentCardThumbnail, List, ListItem } from '@ds/components';

const CARDS = [
  { id: '1', network: 'paypal-debit', label: 'PayPal Debit',   last4: '4242', art: <PayPalDebitThumbnail /> },
  { id: '2', network: 'visa',         label: 'Visa',            last4: '8888', art: <VisaThumbnail /> },
  { id: '3', network: 'mastercard',   label: 'Mastercard',      last4: '1234', art: <MastercardThumbnail /> },
];

export function CardSelector({ onSelect }) {
  const [selected, setSelected] = useState(CARDS[0].id);

  return (
    <List role="listbox" aria-label="Select payment method">
      {CARDS.map(card => (
        <ListItem
          key={card.id}
          role="option"
          aria-selected={selected === card.id}
          onClick={() => { setSelected(card.id); onSelect(card); }}
          leadingContent={
            <PaymentCardThumbnail
              cardImage={card.art}
              aria-label={`${card.label} card ending in ${card.last4}`}
            />
          }
          label={card.label}
          description={`••••${card.last4}`}
          trailingContent={selected === card.id ? <CheckIcon /> : null}
        />
      ))}
    </List>
  );
}
```

### 5. Card in Checkout Confirmation

```tsx
import { PaymentCardDisplay, Header, Button } from '@ds/components';

export function OrderConfirmation({ payment, amount }) {
  return (
    <div className="flex flex-col gap-6">
      <Header
        title="Review your payment"
        description={`You're sending ${amount} using:`}
      />
      <PaymentCardDisplay
        cardImage={payment.cardArt}
        aria-label={`${payment.network} card ending in ${payment.lastFour}`}
      />
      <Button size="large" variant="primary" onClick={confirmPayment}>
        Confirm & pay
      </Button>
    </div>
  );
}
```

### 6. Card Thumbnail in Menu Item

```tsx
import { Menu, MenuItem } from '@ds/components';
import { PaymentCardThumbnail } from '@ds/components';

export function PayWithMenu({ cards, onSelect }) {
  return (
    <Menu trigger={<Button>Pay with</Button>}>
      {cards.map(card => (
        <MenuItem
          key={card.id}
          label={`${card.network} ••••${card.lastFour}`}
          leadingContent={
            <PaymentCardThumbnail
              cardImage={card.thumbnailArt}
              aria-hidden   // label on MenuItem provides context
            />
          }
          onSelect={() => onSelect(card)}
        />
      ))}
    </Menu>
  );
}
```

### 7. Loading Skeleton State

```tsx
import { Skeleton } from '@ds/components';

export function CardDisplaySkeleton() {
  return (
    <div aria-busy="true" aria-label="Loading card details">
      <Skeleton
        width={370}
        height={246}
        style={{ borderRadius: 24 }}
      />
    </div>
  );
}

export function CardThumbnailSkeleton() {
  return (
    <Skeleton
      width={48}
      height={32}
      style={{ borderRadius: 4 }}
    />
  );
}
```

### 8. Carousel of Saved Cards (Display)

```tsx
import { useState } from 'react';
import { PaymentCardDisplay, Pagination, Button } from '@ds/components';

export function CardCarousel({ cards }) {
  const [activeIndex, setActiveIndex] = useState(0);
  const card = cards[activeIndex];

  return (
    <div className="flex flex-col items-center gap-4">
      <PaymentCardDisplay
        key={card.id}
        cardImage={card.displayArt}
        aria-label={`${card.network} card ending in ${card.lastFour}, ${activeIndex + 1} of ${cards.length}`}
      />

      <Pagination
        dotCount={cards.length}
        activeIndex={activeIndex}
        onChange={setActiveIndex}
        aria-label="Saved card navigation"
      />

      <Button size="large" variant="primary" fullWidth>
        Pay with this card
      </Button>
    </div>
  );
}
```

---

## Accessibility Specifications

### Decorative vs. Informative Images

The card artwork image itself is **decorative** (`alt=""`). The meaningful identity information — card network, issuer, masked number — must be carried by the accessible label on the **container** element, not the image:

```html
<!-- Correct: label on container, alt="" on image -->
<div
  role="img"
  aria-label="PayPal Debit card ending in 4242"
>
  <img src="paypal-debit-art.png" alt="" />
</div>

<!-- Incorrect: label on the image, no container label -->
<img src="paypal-debit-art.png" alt="Blue PayPal card" />
```

### Standalone vs. Contextual Use

| Usage | Accessibility Requirement |
|-------|--------------------------|
| Card in a labeled `<ListItem>` or `<MenuItem>` | The parent provides the accessible name; add `aria-hidden="true"` to the card itself |
| Card as a standalone element on a confirmation page | Add `role="img"` and `aria-label="[Network] card ending in [last4]"` to the container |
| Card in a `<section>` with an adjacent heading | The section heading provides context; card can be `aria-hidden` |
| Card in a carousel | Include position in `aria-label`: `"Visa card ending in 8888, 2 of 3"` |

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Display container (standalone) | `"img"` |
| `aria-label` | Display / Thumbnail container | `"[Network] card ending in [last4]"` |
| `aria-hidden` | Card when parent provides context | `"true"` |
| `alt` | Inner card art `<img>` | `""` (always decorative) |
| `aria-busy` | Container while loading | `"true"` |
| `aria-label` | Loading skeleton container | `"Loading card details"` |

### Keyboard Interaction

The PaymentCard itself is **not interactive** — it has no click or focus behavior. Interactivity (selection, navigation) lives in the parent container:

- `ListItem` / `RadioCard` wrapping a Thumbnail → standard button/radio keyboard behavior
- `Carousel` wrapping a Display card → arrow-key navigation via `Pagination`

### Screen Reader Announcement

When the card is used inside a payment selector:

```html
<div role="option" aria-selected="true">
  <!-- Card thumbnail is aria-hidden; the option label speaks instead -->
  <div aria-hidden="true">
    <img src="paypal-debit.png" alt="" />
  </div>
  <span>PayPal Debit card ending in 4242</span>
  <span aria-hidden="true">••••4242</span>
</div>
```

Screen reader output: _"PayPal Debit card ending in 4242, selected, 1 of 3"_

### Color & Contrast

The card art is a pre-designed image asset; contrast within the art is the responsibility of the card issuer's brand guidelines. The **accessible label text** adjacent to the card (card number, network name) must meet 4.5:1 WCAG AA contrast.

---

## Animation Specifications

### Card Swap (Carousel)

When the active card changes in a multi-card carousel, use a horizontal slide transition:

```css
@keyframes card-slide-in-right {
  from { opacity: 0; transform: translateX(20px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes card-slide-in-left {
  from { opacity: 0; transform: translateX(-20px); }
  to   { opacity: 1; transform: translateX(0); }
}

.payment-card-display[data-direction='next'] {
  animation: card-slide-in-right 250ms cubic-bezier(0.16, 1, 0.3, 1);
}

.payment-card-display[data-direction='prev'] {
  animation: card-slide-in-left 250ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Selection Highlight (within a list)

```css
.payment-card-thumbnail {
  transition: transform 150ms ease-in-out;
}

.payment-card-thumbnail:hover {
  transform: scale(1.04);
}
```

### Skeleton → Card Reveal (Fade In)

```css
@keyframes card-fade-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.payment-card-display[data-loaded='true'] {
  animation: card-fade-in 300ms ease-in-out;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .payment-card-display,
  .payment-card-thumbnail {
    animation: none;
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Display Card ────────────────────────────────────────────────────── */
--card-display-width:         370px;
--card-display-height:        246px;
--card-display-radius:        var(--tokens.base.border.radius[24]);        /* 24px */
--card-display-border-width:  var(--tokens.base.border.size[1]);           /* 1px */
--card-display-border-color:  var(--tokens.colorScheme.border.muted);      /* rgba(5,55,130,0.08) */
--card-display-gap:           var(--tokens.base.spacing[8]);               /* 8px */
--card-display-overflow:      clip;
--card-display-align:         flex-end;                                     /* art anchored to bottom */

/* ─── Thumbnail ───────────────────────────────────────────────────────── */
--card-thumbnail-width:       48px;
--card-thumbnail-height:      32px;

/* ─── Card Art Image ──────────────────────────────────────────────────── */
--card-art-object-fit:        cover;
--card-art-pointer-events:    none;
```

---

## Best Practices

### Do

- **Always provide an accessible label** that includes the card network and last 4 digits — _"Visa card ending in 4242"_ — when the card is the primary informational element on screen.
- **Use `aria-hidden="true"` on the card** when its parent element (a list item, menu option, or section heading) already provides the accessible name. Avoid double-announcing.
- **Supply card art at 2× resolution minimum** for retina/HiDPI screens. The Display card fills 370 × 246 px; provide 740 × 492 px source images.
- **Use the Thumbnail in lists and menus** — The Display card is a hero element for detail and confirmation screens. Using it in dense lists wastes space and disrupts hierarchy.
- **Use the Display card in payment confirmation flows** — Showing the full card art at the moment of payment confirmation increases user confidence and reduces abandonment.
- **Provide a skeleton** (`<Skeleton width={370} height={246} />`) while card data is loading rather than a broken or empty container.
- **Use `objectFit: cover`** on all card art images to prevent stretching if image dimensions differ from the container.

### Don't

- **Don't put interactive behavior on the card itself** — The card is a display element. Wrap it in a `<button>`, `<ListItem>`, or `<RadioCard>` to make it interactive.
- **Don't use a generic alt text** like `"card image"` on the `<img>` element — all semantic meaning belongs on the container's `aria-label`. The image `alt` should always be `""`.
- **Don't recreate card art in CSS** — Card artwork contains brand-regulated elements (network logos, issuer artwork, security features). Always use official, approved card art assets.
- **Don't show real card numbers** — Only display masked card numbers (`••••4242`). Full PANs must never appear in the UI.
- **Don't use the Display card in contexts where space is constrained** — It is 370 × 246 px and should only appear where that footprint is intentional. On smaller surfaces, use the Thumbnail.
- **Don't forget the loading state** — Card art often loads asynchronously (from a remote URL). Always handle the loading skeleton and potential error/fallback states.

---

## Security & Compliance Notes

| Requirement | Specification |
|-------------|---------------|
| **PAN masking** | Always display only the last 4 digits. Never show a full card number in the UI. |
| **Card art assets** | Must use issuer- and network-approved artwork. Do not alter card art colors, proportions, or logos. |
| **Network logos** | Visa, Mastercard, Amex, Discover logos are trademarked. Use only approved logo assets supplied by the respective network. |
| **Expiry & CVV** | Never display CVV in any UI. Expiry date display is permitted but must be masked during data entry fields. |
| **Screenshot prevention** | On mobile, consider using platform APIs (e.g., `FLAG_SECURE` on Android, UIScreen prevention on iOS) on screens where full card details are shown. |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **List / ListItem** | Common host for the Thumbnail in payment method selector rows |
| **Menu / MenuItem** | Thumbnail used as `leadingContent` in payment selection menus |
| **Pagination** | Dot indicator for a multi-card carousel using the Display variant |
| **Modal / MobileSheet** | Display card often appears inside a payment confirmation sheet |
| **Skeleton** | Loading placeholder for both Thumbnail and Display before card data resolves |
| **Badge** | Overlaid on the Thumbnail to indicate card status (e.g., "Expired", "Default") |
| **Chips** | Used nearby for filtering by card type in transaction history views |
| **Button** | Primary CTA below a Display card in confirmation flows |
| **Header** | Title + description above a Display card on confirmation screens |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Extracted `PaymentCardThumbnail` (48 × 32 px) and `PaymentCardDisplay` (370 × 246 px) as distinct component variants; standardised `cardImage` slot API; added `showBorder` prop |
| 5.1 | Added `cardImage` slot for custom card artwork |
| 5.0 | Added Display card variant (full-size) |
| 4.0 | Initial release — Thumbnail only |

---

## Testing Checklist

### Visual
- [ ] Display card renders at exactly 370 × 246 px
- [ ] Display card has 24 px border radius (all corners)
- [ ] Display card has 1 px `border.muted` border (rgba(5,55,130,0.08))
- [ ] Thumbnail renders at exactly 48 × 32 px
- [ ] Default PayPal Debit card art fills the container correctly (`object-fit: cover`)
- [ ] Custom `cardImage` fills the container without stretching or letterboxing
- [ ] `showBorder={false}` removes the 1 px border from the Display card
- [ ] Card art does not exceed container bounds (`overflow: clip`)
- [ ] Card art is anchored to the bottom of the Display container (`items-end`)

### Loading States
- [ ] Skeleton renders at correct dimensions for both Thumbnail and Display
- [ ] Skeleton has matching border radius to the card container
- [ ] Card fades in smoothly after art has loaded
- [ ] Error / fallback state renders a neutral placeholder when card art fails to load

### Accessibility
- [ ] Card art `<img>` has `alt=""`
- [ ] Standalone Display card has `role="img"` and descriptive `aria-label`
- [ ] Thumbnail inside a labeled `ListItem` has `aria-hidden="true"`
- [ ] `aria-busy="true"` is present on the container while loading
- [ ] Screen reader announces card identity correctly when used in a selector list
- [ ] No keyboard focus lands on the card itself (non-interactive)
- [ ] Surrounding text (card number, network, expiry) meets 4.5:1 contrast ratio

### Integration
- [ ] `cardImage` prop accepts `<img>`, `<svg>`, and arbitrary `ReactNode`
- [ ] Null `cardImage` falls back to the default PayPal Debit art
- [ ] `data-testid` is forwarded to the root container element
- [ ] Card displays correctly inside `ListItem`, `MenuItem`, `Modal`, and `MobileSheet`
- [ ] Carousel with `Pagination` navigates between cards correctly
