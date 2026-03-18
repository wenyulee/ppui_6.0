# Assets Component Documentation

**Component:** Assets
**Version:** 6.0 Beta
**Figma File:** Components v6 – Beta (`qqH85RTAgalgEUUtJahjAB`)
**Figma Nodes:**
- Custom: `17074-22555`
- Profile: `122-805`
- SMB (Small & Medium Business): `122-831`
- Brand: `122-856`
- Crypto: `122-915`
- Flags: `122-1022`

---

## Table of Contents

1. [Overview](#1-overview)
2. [Anatomy](#2-anatomy)
3. [Universal Container Specifications](#3-universal-container-specifications)
4. [Asset Categories](#4-asset-categories)
   - 4.1 [Custom](#41-custom)
   - 4.2 [Profile](#42-profile)
   - 4.3 [SMB (Small & Medium Business)](#43-smb-small--medium-business)
   - 4.4 [Brand](#44-brand)
   - 4.5 [Crypto](#45-crypto)
   - 4.6 [Flags](#46-flags)
5. [No-Photo / Default Fallback Pattern](#5-no-photo--default-fallback-pattern)
6. [Props API](#6-props-api)
7. [Code Examples](#7-code-examples)
8. [Accessibility Specifications](#8-accessibility-specifications)
9. [Design Tokens](#9-design-tokens)
10. [Best Practices](#10-best-practices)
11. [Related Components](#11-related-components)
12. [Version History](#12-version-history)
13. [Testing Checklist](#13-testing-checklist)

---

## 1. Overview

The **Assets** component family provides a standardized set of circular image containers used throughout the PayPal product suite to represent people, businesses, brands, cryptocurrencies, and countries. All assets share a universal 64×64 px full-pill container specification, ensuring visual consistency across contexts.

**Asset categories at a glance:**

| Category | Purpose | Count (active) |
|----------|---------|----------------|
| Custom | User-uploaded / custom avatar image slot | 1 |
| Profile | Human profile photo avatars | 21 + NoPhoto |
| SMB | Small & medium business logos | 10 + NoPhoto |
| Brand | Major brand / merchant logos | 31 + NoPhoto |
| Crypto | Cryptocurrency / token logos | 10 + NoPhoto |
| Flags | Country / region flags | 33+ + Default |

**When to use Assets:**
- Representing a sender or recipient in a transaction
- Identifying a merchant, brand, or business
- Showing cryptocurrency or token identities
- Representing country/region in locale selectors or payment flows

**When not to use Assets:**
- As decorative illustration — use Illustration components instead
- At sizes other than 64×64 px without explicit design guidance
- As interactive controls without wrapping in a button/link

---

## 2. Anatomy

```
┌─────────────────────────────────────┐
│                                     │
│   ┌──────────────────────────────┐  │
│   │  ╭────────────────────────╮  │  │
│   │  │                        │  │  │
│   │  │       Asset Image      │  │  │  ← 64×64px circle container
│   │  │    (object-cover fit)  │  │  │    border-radius: 999px
│   │  │                        │  │  │
│   │  ╰────────────────────────╯  │  │
│   └──────────────────────────────┘  │
│                                     │
└─────────────────────────────────────┘
```

**Sub-elements:**

1. **Container** — 64×64 px circle with `border-radius: 999px`, `overflow: hidden`
2. **Image / Graphic** — fills the container using `object-fit: cover` (photos) or SVG vector paths (flags/logos)
3. **Fallback Icon** — displayed when no image is available; centered 24×24 px icon on black background

**Exceptions:**
- **Arbitrum** and **Solana** (Crypto category) use `border-radius: 32px` instead of `999px` to match their brand identity
- **Flag** assets render inline SVG vector paths rather than raster images

---

## 3. Universal Container Specifications

All asset containers — regardless of category — share the following base specification:

### Dimensions

| Property | Value | Token |
|----------|-------|-------|
| Width | 64 px | `tokens.base.size[64]` |
| Height | 64 px | `tokens.base.size[64]` |
| Border radius (default) | 999 px | `tokens.base.borderRadius.full` |
| Border radius (Arbitrum, Solana) | 32 px | `tokens.base.borderRadius[32]` |
| Overflow | hidden | — |

### Image Rendering

| Property | Value |
|----------|-------|
| Object fit | `cover` |
| Object position | `center center` |
| Display | `block` |

### CSS

```css
.asset-container {
  width: 64px;
  height: 64px;
  border-radius: 999px;   /* full pill */
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.asset-container--rounded {
  /* Arbitrum, Solana only */
  border-radius: 32px;
}

.asset-container img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center;
  display: block;
}
```

---

## 4. Asset Categories

### 4.1 Custom

**Figma Node:** `17074-22555`

The Custom asset is a single slot representing a user-uploaded or custom avatar image — an empty 64×64 px circle container with `object-cover` behavior. It renders a placeholder checkerboard pattern in Figma to indicate the image area, and accepts any square or portrait/landscape source image (cropped to fill).

**Use cases:**
- User-defined profile photo not belonging to the curated Profile set
- Any bespoke avatar or brand image supplied at runtime

**Specifications:**

| Property | Value |
|----------|-------|
| Size | 64×64 px |
| Border radius | 999 px (full pill) |
| Image fit | `object-fit: cover` |
| Placeholder | Checkerboard (Figma only) |

---

### 4.2 Profile

**Figma Node:** `122-805`

The Profile category contains 21 diverse human profile photo avatars plus a `NoPhoto` fallback. These represent real or representative individuals and are used throughout PayPal to personalize sender/recipient fields, transaction histories, and contact cards.

#### Available Profile Avatars

| Name | Notes |
|------|-------|
| Adam White | — |
| Henrik Muller | — |
| Amy Cinq-Mars | — |
| Alisha Hurston | — |
| Grace Hamilton | — |
| Elizabeth Lopez Perez | — |
| Aisha Anwarzai | — |
| Yiling Xue | — |
| Robert Kim | Code Connect iOS: `PDSAvatar` |
| Vikram Madhusudhan | — |
| Kuru Lam | — |
| John Lee | — |
| Yamato Yuushin | — |
| Laurence Lepage | — |
| Namazzi Bigombe | — |
| Noor Ahmad | — |
| Pedro Garcia Gonzalez | — |
| Diego Burgos | — |
| Judy Ward Parker | Code Connect iOS: `PDSAvatar` |
| Francisca Silva | — |
| Wangxiu Ying | — |

#### NoPhoto Fallback

| Property | Value | Token |
|----------|-------|-------|
| Background | Black | `tokens.colorScheme.background.utility.emphasis` |
| Icon | `person` (outlined) | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Icon color | White | `tokens.colorScheme.content.utility.inverse` |
| Icon inset | `tokens.base.spacing[28]` (20 px each side) | `tokens.base.spacing[28]` |

#### iOS Code Connect

```swift
// Robert Kim, Judy Ward Parker
PDSAvatar(
  imageURL: URL(string: profileImageURL),
  size: .large, // 64pt
  placeholder: .person
)
```

---

### 4.3 SMB (Small & Medium Business)

**Figma Node:** `122-831`

The SMB category contains 10 fictional small-to-medium business logos plus a `NoPhoto` fallback. Each logo is rendered on a brand-specific background color that fills the full circular container.

#### Available SMB Logos

| Business Name | Background Color | Notes |
|--------------|-----------------|-------|
| Zenyan | — | — |
| Wonku | — | — |
| Loocaz | `#005ea6` | Blue |
| Gamma | `#de0063` | Pink/Magenta |
| Aracari Productions | `#000000` / Black | — |
| Orange Moon | `#d64003` | Orange |
| Alpha Technolab | `#2c2e2f` | Dark Charcoal |
| Cinnabar | `#c11111` | Red |
| Perfect Pet Bounty | `#ffbd5d` | Amber |
| Owl Academia | `#50c7f9` | Sky Blue |

#### NoPhoto Fallback

| Property | Value | Token |
|----------|-------|-------|
| Background | Black | `tokens.colorScheme.background.utility.emphasis` |
| Icon | `store` (outlined) | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Icon color | White | `tokens.colorScheme.content.utility.inverse` |

---

### 4.4 Brand

**Figma Node:** `122-856`

The Brand category contains 31 major global brand / merchant logos plus a `NoPhoto` fallback. These represent well-known companies whose logos appear in PayPal transaction flows, merchant directories, and payment histories.

#### Available Brand Logos

| Brand | Background Color | Notes |
|-------|----------------|-------|
| American Express | `#006fcf` | Blue |
| Apple | `#000000` | Black |
| Amazon | White | — |
| Best Buy | White | Yellow/Black logo |
| Buck Mason | `#000000` | Black |
| CVS | `#df0000` | Red |
| Fanatics | `#000000` | Black |
| Delta | White | — |
| Lulu (Lululemon) | `#d2202f` | Red |
| Dropbox | `#0f287f` | Dark Blue |
| Figma | `#f0f0f0` | Light Gray |
| J.Crew | `#000000` | Black |
| Mailchimp | `#fddd4c` | Yellow |
| McDonald's | `#db0008` | Red |
| Netflix | `#000000` | Black |
| Nike | `#000000` | Black |
| Nordstrom | White | — |
| PayPal | White | Uses `paypal-mark-color.svg` |
| Perplexity | `#000000` | Black |
| Sephora | `#000000` | Black |
| Starbucks | `#00643c` | Green |
| Steam | Dark gradient | — |
| Sonos | Dark gradient | — |
| Sony | `#000000` | Black |
| Sweetgreen | White | Code Connect Android: `Avatar.kt` |
| Temu | `#fb7701` | Orange |
| Ticketmaster | White | — |
| Walmart | White | — |
| Uber | `#000000` | Black |
| Ulta Beauty | `#f47d39` | Orange |
| Venmo | `#008cff` | Blue |

#### NoPhoto Fallback

| Property | Value | Token |
|----------|-------|-------|
| Background | Black | `tokens.colorScheme.background.utility.emphasis` |
| Icon | `store` (outlined) | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Icon color | White | `tokens.colorScheme.content.utility.inverse` |

#### Android Code Connect

```kotlin
// Sweetgreen
Avatar(
    imageUrl = merchantLogoUrl,
    size = AvatarSize.Large, // 64dp
    shape = AvatarShape.Circle,
    contentDescription = "Sweetgreen"
)
// TopBar.kt reference for brand context
```

---

### 4.5 Crypto

**Figma Node:** `122-915`

The Crypto category contains 10 cryptocurrency and blockchain token logos plus a `NoPhoto` fallback. Most use the standard full-pill (`999px`) container; **Arbitrum** and **Solana** use `border-radius: 32px` to reflect their square-cornered brand identities.

#### Available Crypto Logos

| Cryptocurrency | Background Color | Border Radius | Notes |
|---------------|----------------|--------------|-------|
| Bitcoin (BTC) | White | 999 px | Code Connect iOS: `ContactCardView.swift` |
| Bitcoin Cash (BCH) | `#4bcf51` | 999 px | Green |
| Ethereum (ETH) | `#151c2f` | 999 px | Dark Navy |
| Litecoin (LTC) | `#587be1` | 999 px | Blue |
| PayPal USD (PYUSD) | `#000000` | 999 px | Black |
| Arbitrum (ARB) | `#213147` | **32 px** | Dark Navy, Square-ish |
| Solana (SOL) | `#000000` | **32 px** | Black, Square-ish |
| Optimism (OP) | `#ff0420` | 999 px | Red |
| Base | `#0052ff` | 999 px | Blue |
| Chainlink (LINK) | `#0847f7` | 999 px | Blue |

**Note:** Arbitrum and Solana deviate from the universal `999px` border-radius due to their brand guidelines requiring a slightly rounded square shape. When rendering these assets, apply `border-radius: 32px` to the container rather than the default `999px`.

#### NoPhoto Fallback

| Property | Value | Token |
|----------|-------|-------|
| Background | Black | `tokens.colorScheme.background.utility.emphasis` |
| Icon | `crypto` (outlined, inset 25%) | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Icon color | White | `tokens.colorScheme.content.utility.inverse` |
| Inset | 25% of container (16 px each side) | — |

#### iOS Code Connect

```swift
// Bitcoin — ContactCardView.swift
ContactCardView(
    asset: .crypto(.bitcoin),
    size: .large // 64pt
)
```

---

### 4.6 Flags

**Figma Node:** `122-1022`

The Flags category contains 33+ country and regional flag assets plus a `Default` fallback (globe icon). All flags are rendered as inline SVG vector paths within the standard 64×64 px white-background circular container.

#### Available Flag Assets

| Country / Region | ISO Code |
|-----------------|---------|
| Andorra | AD |
| United Arab Emirates | AE |
| Antigua & Barbuda | AG |
| Anguilla | AI |
| Albania | AL |
| Armenia | AM |
| Netherlands | NL |
| Angola | AO |
| Argentina | AR |
| Austria | AT |
| Australia | AU |
| Brazil | BR |
| Canada | CA |
| Switzerland | CH |
| China | CN |
| Colombia | CO |
| European Union | EU |
| United Kingdom | GB |
| Hong Kong SAR | HK |
| India | IN |
| Japan | JP |
| South Korea | KR |
| New Zealand | NZ |
| Singapore | SG |
| United States | US |
| Spain | ES |
| France | FR |
| Guatemala | GT |
| South Africa | ZA |
| Sweden | SE |
| Germany | DE |
| Ireland | IE |
| Nigeria | NG |

> **Full flags reference:** For the complete country flag set, see the dedicated Flags Figma file linked from the Assets section.

#### Container Specification (Flags only)

| Property | Value |
|----------|-------|
| Background | White (`#ffffff`) |
| Border radius | 999 px (full pill) |
| Flag rendering | Inline SVG vector paths |
| Size | 64×64 px |

#### Default Fallback

| Property | Value | Token |
|----------|-------|-------|
| Background | Black | `tokens.colorScheme.background.utility.emphasis` |
| Icon | `globe` (outlined) | — |
| Icon size | 24×24 px | `tokens.base.size[24]` |
| Icon color | White | `tokens.colorScheme.content.utility.inverse` |

---

## 5. No-Photo / Default Fallback Pattern

Each category defines its own contextually appropriate fallback icon. All fallbacks share the same visual structure: black background + white centered icon.

### Fallback Specifications by Category

| Category | Icon | Background Token | Icon Token |
|----------|------|-----------------|-----------|
| Custom | *(no fallback — always shows image)* | — | — |
| Profile | `person` | `background.utility.emphasis` | `content.utility.inverse` |
| SMB | `store` | `background.utility.emphasis` | `content.utility.inverse` |
| Brand | `store` | `background.utility.emphasis` | `content.utility.inverse` |
| Crypto | `crypto` | `background.utility.emphasis` | `content.utility.inverse` |
| Flags | `globe` | `background.utility.emphasis` | `content.utility.inverse` |

### Fallback CSS

```css
.asset-fallback {
  width: 64px;
  height: 64px;
  border-radius: 999px;
  background-color: var(--tokens-colorScheme-background-utility-emphasis); /* #000000 */
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  flex-shrink: 0;
}

.asset-fallback__icon {
  width: 24px;
  height: 24px;
  color: var(--tokens-colorScheme-content-utility-inverse); /* #ffffff */
}
```

---

## 6. Props API

### TypeScript Interface

```typescript
type AssetCategory = 'custom' | 'profile' | 'smb' | 'brand' | 'crypto' | 'flag';

type ProfileAssetName =
  | 'adam-white' | 'henrik-muller' | 'amy-cinq-mars' | 'alisha-hurston'
  | 'grace-hamilton' | 'elizabeth-lopez-perez' | 'aisha-anwarzai' | 'yiling-xue'
  | 'robert-kim' | 'vikram-madhusudhan' | 'kuru-lam' | 'john-lee'
  | 'yamato-yuushin' | 'laurence-lepage' | 'namazzi-bigombe' | 'noor-ahmad'
  | 'pedro-garcia-gonzalez' | 'diego-burgos' | 'judy-ward-parker'
  | 'francisca-silva' | 'wangxiu-ying';

type SMBAssetName =
  | 'zenyan' | 'wonku' | 'loocaz' | 'gamma' | 'aracari-productions'
  | 'orange-moon' | 'alpha-technolab' | 'cinnabar' | 'perfect-pet-bounty'
  | 'owl-academia';

type BrandAssetName =
  | 'amex' | 'apple' | 'amazon' | 'best-buy' | 'buck-mason' | 'cvs'
  | 'fanatics' | 'delta' | 'lulu' | 'dropbox' | 'figma' | 'jcrew'
  | 'mailchimp' | 'mcdonalds' | 'netflix' | 'nike' | 'nordstrom' | 'paypal'
  | 'perplexity' | 'sephora' | 'starbucks' | 'steam' | 'sonos' | 'sony'
  | 'sweetgreen' | 'temu' | 'ticketmaster' | 'walmart' | 'uber'
  | 'ulta-beauty' | 'venmo';

type CryptoAssetName =
  | 'bitcoin' | 'bitcoin-cash' | 'ethereum' | 'litecoin' | 'paypal-usd'
  | 'arbitrum' | 'solana' | 'optimism' | 'base' | 'chainlink';

type FlagAssetCode =
  | 'AD' | 'AE' | 'AG' | 'AI' | 'AL' | 'AM' | 'AO' | 'AR' | 'AT' | 'AU'
  | 'BR' | 'CA' | 'CH' | 'CN' | 'CO' | 'DE' | 'ES' | 'EU' | 'FR' | 'GB'
  | 'GT' | 'HK' | 'IE' | 'IN' | 'JP' | 'KR' | 'NG' | 'NL' | 'NZ' | 'SE'
  | 'SG' | 'US' | 'ZA';

interface AssetProps {
  /**
   * Asset category — determines available `name` values and fallback behavior.
   */
  category: AssetCategory;

  /**
   * The specific asset to render.
   * - For `custom`: any image URL string
   * - For `profile`: ProfileAssetName
   * - For `smb`: SMBAssetName
   * - For `brand`: BrandAssetName
   * - For `crypto`: CryptoAssetName
   * - For `flag`: ISO 3166-1 alpha-2 country code (FlagAssetCode)
   * Omit to render the category's fallback/no-photo state.
   */
  name?: ProfileAssetName | SMBAssetName | BrandAssetName | CryptoAssetName | FlagAssetCode | string;

  /**
   * Accessible label describing the asset. Required for non-decorative usage.
   */
  alt: string;

  /**
   * Whether the asset is purely decorative (no accessible label needed).
   * Sets `aria-hidden="true"` and `alt=""`.
   * @default false
   */
  decorative?: boolean;

  /**
   * Additional CSS class names.
   */
  className?: string;

  /**
   * Test identifier.
   */
  testId?: string;
}
```

### Component Signature

```typescript
function Asset(props: AssetProps): JSX.Element;
```

---

## 7. Code Examples

### React / TypeScript

#### Profile Asset — Named Avatar

```tsx
import { Asset } from '@paypal/design-system';

// Named profile avatar
<Asset
  category="profile"
  name="robert-kim"
  alt="Robert Kim"
/>

// No-photo fallback
<Asset
  category="profile"
  alt="Unknown contact"
/>
```

#### SMB Asset

```tsx
// Named SMB logo
<Asset
  category="smb"
  name="loocaz"
  alt="Loocaz"
/>

// Fallback (no logo available)
<Asset
  category="smb"
  alt="Unknown business"
/>
```

#### Brand Asset

```tsx
// Major brand
<Asset
  category="brand"
  name="starbucks"
  alt="Starbucks"
/>
```

#### Crypto Asset — With Border Radius Exception

```tsx
// Standard crypto (full pill)
<Asset
  category="crypto"
  name="bitcoin"
  alt="Bitcoin (BTC)"
/>

// Arbitrum — rounded square, border-radius: 32px applied automatically
<Asset
  category="crypto"
  name="arbitrum"
  alt="Arbitrum (ARB)"
/>
```

#### Flag Asset

```tsx
// Country flag
<Asset
  category="flag"
  name="US"
  alt="United States"
/>

// Unknown / default globe fallback
<Asset
  category="flag"
  alt="Unknown country"
/>
```

#### Custom / Uploaded Avatar

```tsx
<Asset
  category="custom"
  name="https://cdn.example.com/users/abc123/avatar.jpg"
  alt="Your profile photo"
/>
```

#### Decorative Usage

```tsx
// When the asset is decorative (label provided elsewhere)
<Asset
  category="brand"
  name="paypal"
  decorative
  alt=""
/>
```

### CSS

```css
/* Universal asset circle */
.pds-asset {
  display: inline-flex;
  width: 64px;
  height: 64px;
  border-radius: 999px;
  overflow: hidden;
  flex-shrink: 0;
}

/* Rounded-square variant: Arbitrum, Solana */
.pds-asset--rounded-square {
  border-radius: 32px;
}

/* Image fill */
.pds-asset__image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  display: block;
}

/* No-photo / Default fallback */
.pds-asset--fallback {
  background-color: var(--tokens-colorScheme-background-utility-emphasis);
  align-items: center;
  justify-content: center;
}

.pds-asset--fallback .pds-asset__icon {
  width: 24px;
  height: 24px;
  color: var(--tokens-colorScheme-content-utility-inverse);
}

/* Flag — white background */
.pds-asset--flag {
  background-color: #ffffff;
}
```

### Platform APIs

#### iOS — SwiftUI

```swift
import PDSComponents

// Profile avatar (named)
PDSAvatar(
    imageURL: URL(string: "https://assets.paypal.com/profiles/robert-kim.jpg"),
    size: .large,          // 64pt
    placeholder: .person
)

// Crypto — Bitcoin (ContactCardView)
ContactCardView(
    asset: .crypto(.bitcoin),
    size: .large
)

// Crypto — Arbitrum (rounded square)
PDSAsset(
    category: .crypto,
    name: "arbitrum",
    size: .large,
    shape: .roundedRectangle(cornerRadius: 32)
)

// Flag
PDSAsset(
    category: .flag,
    countryCode: "US",
    size: .large,
    accessibilityLabel: "United States"
)
```

#### Android — Jetpack Compose

```kotlin
import com.paypal.pds.components.Asset
import com.paypal.pds.components.AvatarSize
import com.paypal.pds.components.AvatarShape

// Profile avatar
Avatar(
    imageUrl = "https://assets.paypal.com/profiles/sweetgreen.jpg",
    size = AvatarSize.Large,  // 64dp
    shape = AvatarShape.Circle,
    contentDescription = "Sweetgreen"
)

// Crypto — Arbitrum (rounded square)
// Avatar.kt reference
Asset(
    category = AssetCategory.Crypto,
    name = "arbitrum",
    size = AssetSize.Large,
    shape = AssetShape.RoundedSquare(cornerRadius = 32.dp),
    contentDescription = "Arbitrum (ARB)"
)

// Flag
Asset(
    category = AssetCategory.Flag,
    countryCode = "US",
    size = AssetSize.Large,
    contentDescription = "United States"
)
```

---

## 8. Accessibility Specifications

### WCAG 2.1 AA Requirements

| Criterion | Requirement |
|-----------|-------------|
| 1.1.1 Non-text Content | All non-decorative assets must have descriptive `alt` text |
| 1.3.1 Info and Relationships | Category context communicated via accessible label or surrounding content |
| 1.4.3 Contrast (Min) | Fallback icons: white on black = 21:1 (exceeds 4.5:1 minimum) |
| 4.1.2 Name, Role, Value | Assets used as interactive elements must be wrapped in `<button>` or `<a>` |

### Alt Text Guidelines

```tsx
// ✅ Correct — descriptive and specific
<Asset category="profile" name="robert-kim" alt="Robert Kim" />
<Asset category="brand" name="starbucks" alt="Starbucks" />
<Asset category="crypto" name="bitcoin" alt="Bitcoin (BTC)" />
<Asset category="flag" name="US" alt="United States" />

// ✅ Correct — no-photo fallback context
<Asset category="profile" alt="No profile photo" />

// ✅ Correct — decorative (label provided elsewhere in UI)
<Asset category="brand" name="paypal" decorative alt="" />

// ❌ Incorrect — non-specific
<Asset category="profile" name="robert-kim" alt="Profile photo" />

// ❌ Incorrect — missing alt for non-decorative usage
<Asset category="brand" name="starbucks" />
```

### Interactive Assets

When an asset is clickable (e.g., to navigate to a contact or merchant page), wrap it:

```tsx
<button
  type="button"
  aria-label="View Robert Kim's profile"
  onClick={handleClick}
>
  <Asset
    category="profile"
    name="robert-kim"
    decorative  /* label carried by the button */
    alt=""
  />
</button>

// Or as a link
<a href="/contacts/robert-kim" aria-label="Robert Kim's contact page">
  <Asset category="profile" name="robert-kim" decorative alt="" />
</a>
```

### Crypto Accessibility Note

Always include the ticker symbol in the alt text for crypto assets to distinguish tokens that share similar visual identities:

```tsx
<Asset category="crypto" name="bitcoin" alt="Bitcoin (BTC)" />
<Asset category="crypto" name="bitcoin-cash" alt="Bitcoin Cash (BCH)" />
<Asset category="crypto" name="ethereum" alt="Ethereum (ETH)" />
```

---

## 9. Design Tokens

### Shared Token Reference

| Token | Value | Usage |
|-------|-------|-------|
| `tokens.base.size[64]` | 64 px | Container width & height |
| `tokens.base.size[24]` | 24 px | Fallback icon size |
| `tokens.base.borderRadius.full` | 999 px | Default container border-radius |
| `tokens.base.borderRadius[32]` | 32 px | Arbitrum & Solana border-radius |
| `tokens.base.spacing[28]` | — | Profile NoPhoto icon inset |
| `tokens.colorScheme.background.utility.emphasis` | `#000000` | NoPhoto / Default fallback background |
| `tokens.colorScheme.content.utility.inverse` | `#ffffff` | NoPhoto / Default fallback icon color |

### CSS Custom Properties

```css
:root {
  --tokens-base-size-64: 64px;
  --tokens-base-size-24: 24px;
  --tokens-base-borderRadius-full: 999px;
  --tokens-base-borderRadius-32: 32px;
  --tokens-colorScheme-background-utility-emphasis: #000000;
  --tokens-colorScheme-content-utility-inverse: #ffffff;
}
```

### Brand-Specific Background Colors

These are hardcoded brand values, not semantic tokens:

| Brand | Background |
|-------|-----------|
| American Express | `#006fcf` |
| Apple | `#000000` |
| CVS | `#df0000` |
| Dropbox | `#0f287f` |
| Figma | `#f0f0f0` |
| Lulu | `#d2202f` |
| Mailchimp | `#fddd4c` |
| McDonald's | `#db0008` |
| Starbucks | `#00643c` |
| Temu | `#fb7701` |
| Ulta Beauty | `#f47d39` |
| Venmo | `#008cff` |
| Loocaz (SMB) | `#005ea6` |
| Gamma (SMB) | `#de0063` |
| Orange Moon (SMB) | `#d64003` |
| Alpha Technolab (SMB) | `#2c2e2f` |
| Cinnabar (SMB) | `#c11111` |
| Perfect Pet Bounty (SMB) | `#ffbd5d` |
| Owl Academia (SMB) | `#50c7f9` |
| Bitcoin Cash | `#4bcf51` |
| Ethereum | `#151c2f` |
| Litecoin | `#587be1` |
| Arbitrum | `#213147` |
| Optimism | `#ff0420` |
| Base | `#0052ff` |
| Chainlink | `#0847f7` |

---

## 10. Best Practices

### Do

- Always provide meaningful `alt` text for non-decorative assets
- Use `decorative` prop when the asset's context is communicated by surrounding text
- Always specify the correct `category` — it governs fallback behavior
- For crypto assets, always include the ticker symbol in the alt text
- Wrap interactive assets (clickable avatars, selectable flags) in `<button>` or `<a>` elements
- Use the `NoPhoto` / `Default` fallback states rather than hiding assets when image is unavailable

### Don't

- Don't render assets at sizes other than 64×64 px without design approval
- Don't override `border-radius` to a value other than `999px` or `32px` (for Arbitrum/Solana) — these are the only two specified radii
- Don't use animated GIFs or video sources as asset images
- Don't apply drop shadows to asset containers — shadows are reserved for elevation layers (e.g., Toast)
- Don't use Brand logos outside of brand-approved merchant/transaction contexts
- Don't mix `profile` and `smb`/`brand` assets in the same list without visual differentiation

### Sizing Note

The `Assets` component is specified at 64×64 px (the only defined size in Components v6). If your layout requires a smaller avatar (e.g., 40×40 px inline mentions), consult the design team — do not scale this component with `transform: scale()`.

---

## 11. Related Components

| Component | Relationship |
|-----------|-------------|
| **Avatar** | Higher-level component that may compose `Asset` + initials fallback + online indicator |
| **ContactCard** | Composes Profile/Crypto Asset with contact metadata (`ContactCardView.swift`) |
| **Logo** | Separate component for PayPal brand logo (Monogram / Wordmark) |
| **Icon** | Used as fallback content within Asset containers |
| **TopNavigation** | Often displays Profile Asset for the authenticated user |

---

## 12. Version History

| Version | Change |
|---------|--------|
| 6.0 Beta | Initial release of unified Assets component family with Custom, Profile, SMB, Brand, Crypto, and Flags categories |
| 6.0 Beta | Arbitrum and Solana introduced with `border-radius: 32px` exception |
| 6.0 Beta | iOS Code Connect: `PDSAvatar` (Profile), `ContactCardView.swift` (Crypto/Bitcoin) |
| 6.0 Beta | Android Code Connect: `Avatar.kt` (Brand/Sweetgreen) |

---

## 13. Testing Checklist

### Visual

- [ ] All 21 Profile avatars render correctly at 64×64 px with `object-cover`
- [ ] All 10 SMB logos render with correct brand background colors
- [ ] All 31 Brand logos render with correct brand background colors
- [ ] All 10 Crypto logos render correctly — Arbitrum and Solana use `border-radius: 32px`
- [ ] All 33+ Flag SVGs render within white circular containers
- [ ] `NoPhoto` fallback (black + `person` icon) displays for Profile when no image
- [ ] `NoPhoto` fallback (black + `store` icon) displays for SMB and Brand when no image
- [ ] `NoPhoto` fallback (black + `crypto` icon) displays for Crypto when no image
- [ ] `Default` fallback (black + `globe` icon) displays for Flags when no country code
- [ ] Custom asset accepts and renders arbitrary image URL

### Functional

- [ ] `decorative` prop sets `aria-hidden="true"` and `alt=""`
- [ ] Non-decorative assets expose `alt` text to screen readers
- [ ] Interactive asset wrappers (`<button>`, `<a>`) receive correct `aria-label`
- [ ] Crypto `arbitrum` and `solana` automatically apply `border-radius: 32px`
- [ ] All other crypto assets apply `border-radius: 999px`
- [ ] Flag assets render white background; all other categories render brand-specific backgrounds

### Accessibility

- [ ] All non-decorative assets pass NVDA / VoiceOver alt text read-out
- [ ] Fallback icons achieve ≥ 21:1 contrast (white on black) — WCAG 1.4.3 pass
- [ ] Interactive assets: focus ring visible, keyboard activatable (Enter / Space)
- [ ] No `<img>` with missing `alt` attribute in non-decorative contexts

### Platform

- [ ] iOS: `PDSAvatar` renders Profile assets at 64 pt
- [ ] iOS: `ContactCardView.swift` renders Bitcoin asset correctly
- [ ] Android: `Avatar.kt` renders Brand (Sweetgreen) asset at 64 dp
- [ ] Android: Crypto `arbitrum` renders with `cornerRadius = 32.dp`

---

*Documentation generated from Figma nodes `17074-22555`, `122-805`, `122-831`, `122-856`, `122-915`, `122-1022` — Components v6 Beta (`qqH85RTAgalgEUUtJahjAB`)*
