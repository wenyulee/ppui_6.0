# Top Navigation Component Documentation

**Component:** Top Navigation (TopNav / TopBar)
**Figma Nodes:** `139-4654` (TopNav with icons), `135-78` (Parts — Wordmark + Title)
**File:** Components v6 Beta
**Android Code Connect:** `TopBar.kt` (`com.paypal.oslo.core.commonui.components`)
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

The Top Navigation bar is the **primary screen-level navigation surface** for mobile and web experiences. It sits at the top of each screen, providing wayfinding (back/close navigation), screen identity (title or wordmark), and quick access to contextual actions (icon buttons).

**Design philosophy:** The bar is intentionally minimal — white surface with no decorative fills or gradients. Hierarchy is established purely through content: the PayPal wordmark anchors brand identity on root screens, a centered title identifies sub-pages, and tertiary icon buttons provide low-visual-weight actions that don't compete with page content.

**Key characteristics:**
- White background, no elevation shadow in default state
- 56px bar height (content area below status bar)
- Three content zones: left slot (back/logo), center slot (title), right slot (actions)
- PayPal wordmark: 28px tall, left-aligned, 16px from edge
- Title: Title/Medium (16px, 500, Plain), centered
- Icon buttons: medium size (40×40px), tertiary variant
- Thin 1px bottom divider in `border.base`

---

## Component Variants

The TopNav is composed of slot content. Named configurations are:

| Variant | Left Slot | Center Slot | Right Slot | Usage |
|---|---|---|---|---|
| **Wordmark** | PayPal logo (28px) | — | Icon button(s) | Root / home screens |
| **Title** | Back `IconButton` | Title text | Icon button(s) | Sub-pages and detail screens |
| **Title only** | Back `IconButton` | Title text | — | Simple back-nav pages |
| **Wordmark + Title** | PayPal logo | Title text | Icon button(s) | Branded sub-flows |
| **Icon only** | `IconButton` | — | `IconButton` × 1–3 | Minimal chrome screens |

---

## Anatomy

```
┌─────────────────────────────────────────────────────────────────┐ 56px
│  [left slot 40px]    [    center (title / wordmark)   ]  [actions 40×40 each]  │
└─────────────────────────────────────────────────────────────────┘
                                                                  1px border.base
```

- **Bar container** — full-width white surface, fixed to top of viewport
- **Left slot** — back IconButton (chevron-left) or PayPal wordmark
- **Center slot** (optional) — screen title text (Title/Medium)
- **Right slot** (optional) — 1–3 tertiary icon buttons, right-aligned
- **Bottom divider** — 1px `border.base` separator from page content

---

## Size & Spacing Specifications

### Bar Container

| Property | Token | Value |
|---|---|---|
| Height | — | 56px (content area; excludes status bar safe area) |
| Width | — | 100% of viewport |
| Background | `tokens.colorScheme.background.base` | `#ffffff` |
| Bottom border | `tokens.colorScheme.border.base` | `1px solid rgba(0,0,0,0.16)` |
| Padding left | `tokens.base.spacing[8]` | 8px |
| Padding right | `tokens.base.spacing[8]` | 8px |

### Slots

| Slot | Width | Height | Alignment |
|---|---|---|---|
| Left slot | 40px (icon) or 79px (wordmark) | 40px (icon) or 28px (logo) | Vertically centered |
| Center slot | flex: 1 (fills remaining space) | auto | Vertically + horizontally centered |
| Right slot | 40px × n icons | 40px each | Vertically centered, right-aligned |

### PayPal Wordmark

| Property | Token | Value |
|---|---|---|
| Height | `tokens.base.size[28]` | **28px** |
| Width | — | ~79px (intrinsic, ~2.8:1 aspect ratio) |
| Left margin | `tokens.base.spacing[16]` | 16px |
| Vertical alignment | — | centered (top offset: (56px − 28px) / 2 = 14px) |
| Source | — | `paypal-wordmark-color.svg` (from PayPal Assets CDN) |
| Alt text | — | `"PayPal"` |

### Title Text

| Property | Token | Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.title` | Plain (Medium) |
| Font size | `tokens.base.text.fontsize.ramp[16]` | 16px |
| Line height | `tokens.base.text.lineheight.ramp[16]` | 24px |
| Font weight | `tokens.brand.text.fontweight.title` | 500 |
| Letter spacing | `tokens.base.text.letterspacing[0]` | 0px |
| Color | `tokens.colorScheme.content.base` | `#000000` |
| Alignment | — | center (in the center slot) |
| Overflow | — | `truncate` (ellipsis at max-width) |
| Max width | — | `calc(100% − 2 × 56px)` — accounts for left/right icon zones |

### Icon Buttons

| Property | Value |
|---|---|
| Size | `medium` = 40×40px touch target |
| Variant | `tertiary` = transparent background, `content.base` icon color |
| Icon size | 16×16px (Medium icon button spec) |
| Gap between icons | `tokens.base.spacing[4]` = 4px |

---

## Style Specifications

### Bar Container

```css
.top-nav {
  display: flex;
  align-items: center;
  width: 100%;
  height: 56px;
  padding: 0 var(--tokens-base-spacing-8, 8px);
  background: var(--tokens-colorScheme-background-base, #fff);
  border-bottom: 1px solid var(--tokens-colorScheme-border-base, rgba(0,0,0,0.16));
  position: sticky;
  top: 0;
  z-index: 100;
  /* Account for safe area on devices with notch/island */
  padding-top: env(safe-area-inset-top, 0px);
}
```

### Center Title

```css
.top-nav__title {
  flex: 1;
  font-family: var(--tokens-brand-text-fontfamily-title, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-16, 16px);
  line-height: var(--tokens-base-text-lineheight-ramp-16, 24px);
  font-weight: 500;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-base, #000);
  text-align: center;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  /* Prevent overlap with left/right icon zones */
  padding: 0 var(--tokens-base-spacing-8, 8px);
}
```

### PayPal Wordmark

```css
.top-nav__wordmark {
  height: var(--tokens-base-size-28, 28px);
  width: auto; /* ~79px intrinsic */
  flex-shrink: 0;
  margin-left: calc(var(--tokens-base-spacing-16, 16px) - var(--tokens-base-spacing-8, 8px));
  /* Offset the container's 8px padding to achieve 16px from edge */
}

.top-nav__wordmark img {
  height: 100%;
  width: auto;
  display: block;
}
```

### Right Actions Group

```css
.top-nav__actions {
  display: flex;
  align-items: center;
  gap: var(--tokens-base-spacing-4, 4px);
  flex-shrink: 0;
}
```

---

## Slot Layout Details

The three-zone layout is achieved with `display: flex` and specific flex behavior:

```
Left slot            Center slot          Right slot
[flex-shrink: 0]     [flex: 1]            [flex-shrink: 0]
    ↑                    ↑                     ↑
Fixed width          Fills available       Fixed width
(40px or logo)       space                 (40px × n icons)
```

**Title centering:** Because the left and right slots may have different widths (e.g., logo at 79px vs. back button at 40px), the title is centered relative to the full bar using absolute positioning when pixel-perfect centering is required:

```css
/* Option A: Flex centering (may not be perfectly centered if slots differ) */
.top-nav__title { flex: 1; text-align: center; }

/* Option B: Absolute centering (always centered relative to bar) */
.top-nav { position: relative; }
.top-nav__title {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  max-width: calc(100% - 120px); /* 60px each side for icon zones */
}
```

---

## Props API

### TypeScript Interface

```typescript
interface TopNavProps {
  /**
   * Content for the left slot.
   * Typically a back IconButton, close IconButton, or the PayPal wordmark.
   */
  left?: React.ReactNode;

  /**
   * Screen title displayed in the center slot.
   * Omit when left slot contains the wordmark and no title is needed.
   */
  title?: string;

  /**
   * Content for the right slot.
   * Typically 1–3 tertiary IconButton elements.
   */
  actions?: React.ReactNode;

  /**
   * Whether to show the 1px bottom border divider.
   * Disable for pages where the first content block provides visual separation.
   * @default true
   */
  showDivider?: boolean;

  /**
   * Whether the bar sticks to the top of the viewport on scroll.
   * @default true
   */
  sticky?: boolean;

  /**
   * Optional accessible label for the nav landmark.
   * @default 'Main navigation'
   */
  'aria-label'?: string;

  className?: string;
}

// Convenience sub-components
interface TopNavBackButtonProps {
  /** Called when the back button is pressed */
  onPress: () => void;
  /** Override default "Go back" aria-label */
  'aria-label'?: string;
}

interface TopNavWordmarkProps {
  /** Href for the wordmark logo link (usually '/') */
  href?: string;
  /** Override default "PayPal" alt text */
  alt?: string;
}
```

---

## Code Examples

### Root Screen (Wordmark + Actions)

```tsx
import { TopNav } from '@/components/TopNav';
import { IconButton } from '@/components/IconButton';
import { TopNavWordmark } from '@/components/TopNav';
import { NotificationIcon, SearchIcon } from '@/icons';

<TopNav
  left={<TopNavWordmark href="/" />}
  actions={
    <>
      <IconButton
        size="medium"
        variant="tertiary"
        aria-label="Search"
        icon={<SearchIcon />}
      />
      <IconButton
        size="medium"
        variant="tertiary"
        aria-label="Notifications"
        icon={<NotificationIcon />}
      />
    </>
  }
/>
```

### Sub-Page (Back button + Title + Action)

```tsx
import { useRouter } from 'next/router';

const router = useRouter();

<TopNav
  left={
    <IconButton
      size="medium"
      variant="tertiary"
      aria-label="Go back"
      icon={<ChevronLeftIcon />}
      onPress={() => router.back()}
    />
  }
  title="Transaction Details"
  actions={
    <IconButton
      size="medium"
      variant="tertiary"
      aria-label="More options"
      icon={<MoreVerticalIcon />}
      onPress={openMenu}
    />
  }
/>
```

### Title Only (No Actions)

```tsx
<TopNav
  left={
    <IconButton
      size="medium"
      variant="tertiary"
      aria-label="Close"
      icon={<CloseIcon />}
      onPress={onClose}
    />
  }
  title="Add a bank account"
/>
```

### TopNav Component Implementation

```tsx
import React from 'react';

export const TopNav: React.FC<TopNavProps> = ({
  left,
  title,
  actions,
  showDivider = true,
  sticky = true,
  'aria-label': ariaLabel = 'Main navigation',
  className,
}) => (
  <header
    role="banner"
    aria-label={ariaLabel}
    className={[
      'top-nav',
      showDivider && 'top-nav--divider',
      sticky && 'top-nav--sticky',
      className,
    ].filter(Boolean).join(' ')}
  >
    {/* Left slot */}
    <div className="top-nav__left">
      {left}
    </div>

    {/* Center slot */}
    {title && (
      <h1 className="top-nav__title">{title}</h1>
    )}

    {/* Right slot */}
    {actions && (
      <div className="top-nav__actions" aria-label="Navigation actions">
        {actions}
      </div>
    )}
  </header>
);

// Wordmark sub-component
export const TopNavWordmark: React.FC<TopNavWordmarkProps> = ({
  href = '/',
  alt = 'PayPal',
}) => (
  <a href={href} className="top-nav__wordmark-link" aria-label={alt}>
    <img
      src="https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg"
      alt={alt}
      className="top-nav__wordmark"
      height="28"
      width="79"
    />
  </a>
);
```

---

## Scroll Behavior

The TopNav has two scroll behaviors depending on context:

| Mode | CSS | Behavior |
|---|---|---|
| **Sticky** (default) | `position: sticky; top: 0` | Bar stays visible while content scrolls beneath it |
| **Static** | `position: static` | Bar scrolls away with page content |
| **Elevated on scroll** | Add `box-shadow` when `scrollY > 0` | Bar gains Level 1 shadow when content scrolls beneath it |

### Elevation on Scroll

When page content scrolls under a sticky nav, adding a subtle elevation reinforces the z-layering:

```typescript
const [isScrolled, setIsScrolled] = useState(false);

useEffect(() => {
  const handleScroll = () => setIsScrolled(window.scrollY > 0);
  window.addEventListener('scroll', handleScroll, { passive: true });
  return () => window.removeEventListener('scroll', handleScroll);
}, []);
```

```css
.top-nav--elevated {
  box-shadow:
    0px 4px 4px rgba(5, 55, 130, 0.04),
    0px 0px 8px rgba(5, 55, 130, 0.08);
  /* Elevation Level 1 */
  border-bottom: none; /* Replace divider with shadow when elevated */
}
```

---

## Accessibility Specifications

### Landmark & Heading

| Element | Role / Tag | Purpose |
|---|---|---|
| Bar container | `<header role="banner">` | Page-level banner landmark; screen readers can navigate directly to it |
| Title text | `<h1>` | Page heading — the nav title is the primary `h1` of the screen |
| Wordmark | `<a>` with `aria-label="PayPal"` | Branded home link; labeled for screen readers that cannot see the logo image |
| Wordmark image | `<img alt="">` | Empty alt when the parent `<a>` already provides the label |
| Right actions group | `<div aria-label="Navigation actions">` | Groups action buttons with a readable container label |

> **Note on `<h1>`:** On mobile screens with a single content hierarchy, the TopNav title doubles as the page's `<h1>`. On web pages with a richer layout, use `aria-level` or a semantic heading level appropriate to the screen hierarchy.

### Keyboard Navigation

| Key | Action |
|---|---|
| `Tab` | Move focus through left slot → right slot icon buttons (center title is non-interactive) |
| `Enter` / `Space` | Activate the focused icon button (back, close, action) |
| `Alt+Left` (browser) | Browser back navigation (supplement to back button) |

### Back Button

The back `IconButton` must have a descriptive `aria-label`. Avoid generic labels:

```tsx
// ✅ Descriptive
<IconButton aria-label="Go back to Wallet" ... />
<IconButton aria-label="Close" ... />   // for modal/sheet close

// ❌ Too generic
<IconButton aria-label="Back" ... />
<IconButton aria-label="Button" ... />
```

### Focus Management on Navigation

When a user activates the back button and navigates to the previous screen, focus should be managed to a logical location on the previous page (typically the element that originally triggered navigation to the sub-page, or the page's `<h1>`).

### Screen Reader Announcements

Page title changes during in-app navigation (SPA) must be announced. Use a live region or update `document.title`:

```typescript
// On route change, update document title to match TopNav title
useEffect(() => {
  document.title = `${pageTitle} — PayPal`;
}, [pageTitle]);
```

### Touch Target Sizes

All icon buttons in the TopNav are `40×40px` — meeting the WCAG 2.5.5 minimum recommended touch target of 44×44px when combined with the 8px bar padding on each side. For critical actions (back/close), consider using `44×44px` minimum.

---

## Platform APIs

### Android — Jetpack Compose (`TopBar.kt`)

The Code Connect reference points to `TopBar` in the Oslo design system:

```kotlin
import com.paypal.oslo.core.commonui.components.TopBar

// Wordmark variant (root screen)
@Composable
fun RootScreen() {
    TopBar(
        navigationIcon = {
            // No back button on root screen
        },
        title = {
            // Optional center title
        },
        actions = {
            IconButton(onClick = { /* search */ }) {
                Icon(Icons.Default.Search, contentDescription = "Search")
            }
            IconButton(onClick = { /* notifications */ }) {
                Icon(Icons.Default.Notifications, contentDescription = "Notifications")
            }
        }
    )
}

// Title variant (sub-page)
@Composable
fun DetailScreen(navController: NavController) {
    TopBar(
        navigationIcon = {
            IconButton(onClick = { navController.navigateUp() }) {
                Icon(
                    Icons.AutoMirrored.Filled.ArrowBack,
                    contentDescription = "Go back"
                )
            }
        },
        title = {
            Text(
                text = "Transaction Details",
                style = MaterialTheme.typography.titleMedium
            )
        },
        actions = {
            IconButton(onClick = { /* more */ }) {
                Icon(Icons.Default.MoreVert, contentDescription = "More options")
            }
        }
    )
}
```

**TopBar styling (Material3 overrides to match design system):**

```kotlin
TopAppBar(
    colors = TopAppBarDefaults.topAppBarColors(
        containerColor = MaterialTheme.colorScheme.background,          // white
        titleContentColor = MaterialTheme.colorScheme.onBackground,     // content.base = black
        actionIconContentColor = MaterialTheme.colorScheme.onBackground, // content.base
        navigationIconContentColor = MaterialTheme.colorScheme.onBackground
    ),
    scrollBehavior = TopAppBarDefaults.pinnedScrollBehavior()
)
```

### iOS — SwiftUI

```swift
import SwiftUI

// Root screen with wordmark
struct RootView: View {
    var body: some View {
        NavigationStack {
            ContentView()
                .navigationBarTitleDisplayMode(.inline)
                .toolbar {
                    ToolbarItem(placement: .navigationBarLeading) {
                        Image("paypal-wordmark-color")
                            .resizable()
                            .scaledToFit()
                            .frame(height: 28)
                            .accessibilityLabel("PayPal")
                    }
                    ToolbarItem(placement: .navigationBarTrailing) {
                        Button {
                            // search action
                        } label: {
                            Image(systemName: "magnifyingglass")
                                .foregroundColor(.primary)
                        }
                        .accessibilityLabel("Search")
                    }
                }
        }
    }
}

// Sub-page with back button + title
struct DetailView: View {
    var body: some View {
        VStack { /* content */ }
            .navigationTitle("Transaction Details")
            .navigationBarTitleDisplayMode(.inline)
            .toolbar {
                ToolbarItem(placement: .navigationBarTrailing) {
                    Button {
                        // more options
                    } label: {
                        Image(systemName: "ellipsis")
                    }
                    .accessibilityLabel("More options")
                }
            }
        // iOS automatically adds back button from NavigationStack
    }
}

// Custom TopNav styling via UINavigationBarAppearance
extension UINavigationBarAppearance {
    static var appDefault: UINavigationBarAppearance {
        let appearance = UINavigationBarAppearance()
        appearance.configureWithOpaqueBackground()
        appearance.backgroundColor = .white
        appearance.shadowColor = UIColor.black.withAlphaComponent(0.16) // border.base
        appearance.titleTextAttributes = [
            .font: UIFont(name: "Plain-Medium", size: 16)!,
            .foregroundColor: UIColor.black
        ]
        return appearance
    }
}
```

---

## Design Tokens Reference

| Token | Resolved Value | Usage in TopNav |
|---|---|---|
| `tokens.colorScheme.background.base` | `#ffffff` | Bar background |
| `tokens.colorScheme.border.base` | `rgba(0,0,0,0.16)` | Bottom 1px divider |
| `tokens.colorScheme.content.base` | `#000000` | Title text, icon color |
| `tokens.base.size[28]` | `28px` | PayPal wordmark height |
| `tokens.base.spacing[8]` | `8px` | Bar horizontal padding |
| `tokens.base.spacing[16]` | `16px` | Wordmark left margin from edge |
| `tokens.base.spacing[4]` | `4px` | Gap between right-side icon buttons |
| `tokens.brand.text.fontfamily.title` | Plain (Medium) | Title font family |
| `tokens.base.text.fontsize.ramp[16]` | `16px` | Title font size |
| `tokens.base.text.lineheight.ramp[16]` | `24px` | Title line height |
| `tokens.brand.text.fontweight.title` | `500` | Title font weight |
| `tokens.base.text.letterspacing[0]` | `0px` | Title letter spacing |
| Elevation Level 1 (on scroll) | `0px 4px 4px rgba(5,55,130,0.04), 0px 0px 8px rgba(5,55,130,0.08)` | Optional elevated state |

---

## Best Practices

**Use a single `<h1>` per screen and make it the TopNav title.** On mobile screens, the nav bar title is the primary visual heading. Aligning it with the semantic `<h1>` creates a consistent experience for both sighted and screen reader users. Avoid placing additional `<h1>` elements deeper in the page content.

**Wordmark belongs on root-level screens only.** The PayPal wordmark should appear in the TopNav only on root/home screens where no back navigation is needed. Sub-pages should display a title and a back button instead.

**Never put more than 3 icon buttons in the right slot.** More than 3 actions creates visual congestion in the limited 40px-per-button space. If a screen requires more actions, consolidate into a single "More" (`⋯`) icon button that opens a menu.

**Keep titles short enough to avoid truncation.** The title has `max-width: calc(100% − 2 × 56px)` to avoid overlapping the left/right zones. Aim for titles under 20 characters on mobile. Longer titles will truncate with ellipsis — if the full title is important, it should also appear as a heading within the page content.

**The PayPal wordmark is sourced from the CDN.** Always reference the official asset: `https://www.paypalobjects.com/paypal-ui/logos/svg/paypal-wordmark-color.svg`. Do not embed or recreate the logo as custom SVG paths. Height must be **28px** when used in a nav bar.

**Apply safe-area padding for notch/island devices.** The bar must account for `env(safe-area-inset-top)` on iOS devices with a home-indicator notch. The 56px height is the content area only; total rendered height on newer iPhones will be 56px + ~44–59px safe area inset.

**Do not hide the TopNav on scroll unless explicitly designed for fullscreen content.** Hiding the nav bar on downward scroll (common in some apps) must be carefully handled with a show-on-upward-scroll pattern and must not trap users without a way to navigate back.

---

## Related Components

- **IconButton** — Used within TopNav slots; always `size="medium"` (`variant="tertiary"`) in this context
- **Tabs** — Secondary horizontal navigation that lives below the TopNav bar on screens with section-level navigation
- **Bottom Navigation** — Mobile tab bar for persistent app-level destination switching; complements the TopNav
- **Toast** — Transient notification that renders below the TopNav (not overlapping)

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial Top Navigation documentation from Components v6 Beta Figma file |

---

## Testing Checklist

- [ ] Bar height is 56px content area (plus safe-area-inset-top on iOS)
- [ ] Background is `background.base` (#ffffff), no tint or gradient
- [ ] Bottom 1px divider in `border.base` = `rgba(0,0,0,0.16)` is visible
- [ ] `showDivider={false}` correctly removes the bottom border
- [ ] PayPal wordmark renders at 28px tall, ~79px wide
- [ ] Wordmark has 16px margin from left edge of viewport
- [ ] Wordmark is vertically centered in the 56px bar
- [ ] Wordmark image `alt=""` with parent link `aria-label="PayPal"`
- [ ] Title text: Title/Medium (16px, 500, Plain), `content.base` (black)
- [ ] Title is centered horizontally in the bar
- [ ] Title truncates with ellipsis when it exceeds max-width
- [ ] Icon buttons are 40×40px touch targets
- [ ] Icon buttons use `tertiary` variant (transparent background)
- [ ] Right-side action icons have 4px gap between them
- [ ] Back button has a descriptive `aria-label` (not just "Back")
- [ ] `<header role="banner">` wraps the bar
- [ ] Title text is rendered as `<h1>` (or appropriate heading level)
- [ ] Screen reader announces title on page navigation
- [ ] `document.title` updates to match page title on SPA navigation
- [ ] Sticky behavior: bar stays at top while content scrolls
- [ ] Optional elevation (Level 1 shadow) applied when content scrolls beneath bar
- [ ] No box-shadow in default (unscrolled) state
- [ ] `env(safe-area-inset-top)` applied correctly on notched devices
- [ ] Maximum 3 icon buttons in right slot before overflow
