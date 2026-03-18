# Web View Component Documentation

**Component:** Web View (WebView Chrome)
**Figma Nodes:**
- `23560-1046` — Web View App Frame (TopNav + Loader)
- `23594-2664` — Web View Toolbar (bottom bar)
- `9075-31282` — Parts (sub-component library)
**File:** Components v6 Beta
**Android Code Connect:** `TopBar.kt` (core-common-ui)
**iOS Code Connect:** `PDSSwitch.swift`, `PDSButton.swift` (pds_core)
**Last Updated:** 2026-03-18
**Version:** 6.0.0-beta

---

## Overview

The Web View is the **in-app browser chrome** rendered when a native app loads external web content. It provides a complete browsing UI without leaving the app: a top navigation bar showing the URL and page title, an optional progress loader, the web content area, and a bottom toolbar with navigation controls and actions.

**Design philosophy:** The chrome is deliberately lightweight — it communicates context (where the user is) and control (back, actions) without competing with the loaded web content. The TopNav is compact at 48px, the loader is a hairline 4px bar, and the toolbar dissolves into the content using a fade-to-white gradient.

**The WebView chrome consists of four sub-components:**

| Sub-component | Node | Description |
|---|---|---|
| `WebView.TopNav` | `26974:13230` | 48px address bar with URL title, leading/trailing icon buttons |
| `WebView.Title` | `23560:722` | URL domain + description stacked label in the TopNav center |
| `WebView.Loader` | `23554:11219` | 4px horizontal progress bar shown while page is loading |
| `WebView.Toolbar` | `23594:2664` | Bottom floating bar with back button, center slot, trailing actions |
| `WebView.Button` | `31168:1271` | Circular white pill button used within the Toolbar |

---

## Full Chrome Anatomy

```
┌─────────────────────────────────────────────────────┐  iOS Status Bar
├─────────────────────────────────────────────────────┤  (safe-area-inset-top)
│  [◀ leading]    Title          [trailing ▶]        │  WebView.TopNav — 48px
│                 Description                         │
├────────────────────────────────────────────────────-┤
│▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│  WebView.Loader — 4px (optional)
├─────────────────────────────────────────────────────┤
│                                                     │
│              Web Content Area                       │
│                                                     │
│                                        (scrollable) │
├ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ░ ┤  Gradient fade (transparent → white)
│  [←]        [    Slot    ]      [⋯ ☆ ♡]           │  WebView.Toolbar
├─────────────────────────────────────────────────────┤
│                 ▬ Home Indicator                     │  iOS home bar
└─────────────────────────────────────────────────────┘
```

---

## 1. WebView.TopNav

The address bar sits directly below the iOS status bar. It is 48px tall and uses the standard three-slot layout (leading / center / trailing).

### Dimensions

| Property | Token | Value |
|---|---|---|
| Height | — | 48px |
| Width | — | 100% (402px on standard iPhone) |
| Background | `tokens.colorScheme.background.base` | `#ffffff` |
| Padding horizontal | `tokens.base.spacing[8]` | 8px |
| Padding vertical | `tokens.base.spacing[16]` | 16px (top + bottom within 48px bar) |
| Bottom border | `tokens.colorScheme.border.base` | `1px solid rgba(0,0,0,0.16)` |

### Slot Layout

| Slot | Width | Content |
|---|---|---|
| Leading | 40px | `IconButton` size="medium" variant="tertiary" (e.g., ✕ close or ⟨ back) |
| Center | 185px (flex, capped) | `WebView.Title` component |
| Trailing | 40px | `IconButton` size="medium" variant="tertiary" (e.g., share, reload, more) |

---

## 2. WebView.Title

The title is the center content of the TopNav. It is a two-line stacked label that shows the page domain (URL) above and a description/status below.

### Dimensions & Layout

| Property | Token | Value |
|---|---|---|
| Height | — | 40px total |
| Gap (URL → Description) | `tokens.base.spacing[2]` | 2px |
| Layout | — | `flex-col`, items centered |

### URL Line (top)

| Property | Token | Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.label` | Plain (Medium) |
| Font size | `tokens.base.text.fontsize.ramp[12]` | 12px |
| Line height | `tokens.base.text.lineheight.ramp[12]` | 16px |
| Font weight | `tokens.brand.text.fontweight.label` | 500 |
| Letter spacing | `tokens.base.text.letterspacing[0]` | 0px |
| Color | `tokens.colorScheme.content.base` | `#000000` |
| White-space | — | `nowrap` |
| Alignment | — | centered |

The URL line displays the page title or domain name (e.g., `"paypal.com"` or `"Checkout"`).

### Description Line (bottom, optional)

| Property | Token | Value |
|---|---|---|
| Font family | `tokens.brand.text.fontfamily.body` | Plain (Regular) |
| Font size | `tokens.base.text.fontsize.ramp[12]` | 12px |
| Line height | `tokens.base.text.lineheight.ramp[12]` | 16px |
| Font weight | `tokens.brand.text.fontweight.body` | 400 |
| Letter spacing | `tokens.base.text.letterspacing[0]` | 0px |
| Color | `tokens.colorScheme.content.role.base.info` | `#0066f5` (blue) |
| White-space | — | `nowrap` |
| Gap (icon → text) | `tokens.base.spacing[2]` | 2px |

The description line typically shows the full URL or a security indicator (e.g., a lock icon + `"paypal.com/checkout"`). The blue color matches `content.link` to signal that this text relates to the web address. An optional 16×16px icon (e.g., lock/shield for HTTPS) can precede the description text.

### Title States

| State | URL content | Description content | Icon |
|---|---|---|---|
| Loading | Page title or domain | Full URL (blue) | — |
| Secure (HTTPS) | Domain | Full URL | 🔒 lock icon |
| Description hidden | Domain only | — | — |

---

## 3. WebView.Loader

A hairline progress indicator that appears immediately below the TopNav while the page is loading. It disappears when loading completes.

### Dimensions

| Property | Token | Value |
|---|---|---|
| Height | — | **4px** |
| Width | — | 100% of viewport |
| Border radius | — | 0px (flush, no rounding) |

### Colors

| Element | Token | Value |
|---|---|---|
| Track (background) | `tokens.colorScheme.background.role.base.neutral` | `rgba(5,55,130,0.08)` |
| Fill (progress) | `tokens.colorScheme.content.role.base.info` | `#0066f5` |

### Animation

The fill bar animates from left to right as the page loads. Progress is driven by network load events:

```
Initial:  [░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]  0%
Loading:  [▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]  ~22%
Nearly:   [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░]  ~85%
Complete: [▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓]  100% → fade out
```

```css
.webview-loader {
  width: 100%;
  height: 4px;
  background: var(--tokens-colorScheme-background-role-base-neutral, rgba(5,55,130,0.08));
  overflow: hidden;
}

.webview-loader__fill {
  height: 4px;
  background: var(--tokens-colorScheme-content-role-base-info, #0066f5);
  width: var(--progress, 0%);
  transition: width 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

/* Fade out when complete */
.webview-loader--complete {
  animation: loader-fade-out 300ms ease forwards;
}

@keyframes loader-fade-out {
  from { opacity: 1; }
  to { opacity: 0; }
}
```

---

## 4. WebView.Toolbar

The bottom toolbar floats above the web content, anchored to the bottom of the safe area. It uses a **vertical fade gradient** to visually blend into the content above it.

### Container

| Property | Token | Value |
|---|---|---|
| Background | Gradient | `linear-gradient(to bottom, rgba(255,255,255,0), #ffffff)` |
| Backdrop blur | — | `blur(4px)` |
| Width | — | 100% |
| Padding horizontal | `tokens.base.spacing[16]` | 16px |
| Padding vertical | `tokens.base.spacing[8]` | 8px |
| Gap (slots) | `tokens.base.spacing[8]` | 8px |

**Gradient tokens:**
- Top (transparent): `tokens.colorScheme.background.overlay.fade.bottomStart` = `rgba(255,255,255,0)`
- Bottom (opaque): `tokens.colorScheme.background.overlay.fade.bottomEnd` = `#ffffff`

The gradient height typically spans 64–80px. The opaque white base of the toolbar holds the interactive controls.

### Toolbar Slot Layout

| Slot | Width | Content |
|---|---|---|
| Back button (left, fixed) | 40×40px | `WebView.Button` with chevron-left icon |
| Center slot (flex, optional) | flex: 1, 48px tall | Any component — typically a Toggle/Switch or URL bar |
| Trailing slot (right, optional) | auto | Icon button group (up to 3 actions) |

### Toolbar Inner Flex Row

```
[WebView.Button ←]  [    Center Slot (flex:1)    ]  [trailing icons ▸]
      40px                    flex fills                  auto
  ←16px pad                  8px gaps                    16px pad→
```

---

## 5. WebView.Button

A circular white pill button used exclusively within the Toolbar. Distinct from the tertiary `IconButton` (transparent background) — this button has an explicit white fill to ensure it reads clearly on top of the gradient backdrop.

### Dimensions

| Property | Token | Value |
|---|---|---|
| Size | `ref.size.550` | **40×40px** |
| Padding | `tokens.base.spacing[8]` | 8px |
| Background | `tokens.colorScheme.background.base` | `#ffffff` |
| Border radius | `tokens.base.border.radius.full` | `999px` (full circle) |
| Shadow | — | none (no elevation) |
| Icon size | — | 20×20px |

### States

| State | Visual change |
|---|---|
| **Enabled** | White fill, full opacity icon (`content.base`) |
| **Disabled** | White fill, reduced opacity icon (~40% opacity, `content.faint`) |
| **Pressed** | `background.muted` fill (`#f5f7fa`), 100ms transition |

> **Note:** Both Enabled and Disabled states share identical container styling (white background, full pill, 40×40px). The only distinction in the disabled state is icon opacity reduction — the container itself does not change.

---

## Props API

### WebViewAppFrame Props

```typescript
interface WebViewAppFrameProps {
  /**
   * Whether to show the 4px loading progress bar beneath the TopNav.
   * True while the web page is actively loading; false when load completes.
   * @default false
   */
  loader?: boolean;

  /**
   * Progress value for the loader (0–100). Only relevant when loader=true.
   * @default 0
   */
  loadProgress?: number;
}
```

### WebViewTopNav Props

```typescript
interface WebViewTopNavProps {
  /** Content for the leading (left) slot. Typically a close or back IconButton. */
  leading?: React.ReactNode;

  /** URL or page title shown in the top line of the center title area. */
  url?: string;

  /**
   * Description shown below the URL in the center title area.
   * Typically the full URL, a security label, or connection status.
   * Hidden when falsy.
   */
  description?: string;

  /**
   * Optional icon displayed left of the description text.
   * Typically a lock icon for secure connections.
   */
  descriptionIcon?: React.ReactNode;

  /** Content for the trailing (right) slot. Typically share/reload/more icon buttons. */
  trailing?: React.ReactNode;
}
```

### WebViewToolbar Props

```typescript
interface WebViewToolbarProps {
  /**
   * Whether the back button is enabled.
   * Disabled when there is no navigation history to go back to.
   * @default false
   */
  canGoBack?: boolean;

  /** Called when the back button is pressed. */
  onBack?: () => void;

  /**
   * Optional content for the center slot.
   * Common uses: Toggle switch (e.g., reader mode), URL address bar.
   */
  center?: React.ReactNode;

  /**
   * Whether to render the trailing icon group.
   * @default true
   */
  trailing?: boolean;

  /**
   * Content for the trailing slot.
   * Typically 1–3 icon buttons (share, bookmark, more options).
   */
  trailingContent?: React.ReactNode;
}
```

### WebViewTitle Props

```typescript
interface WebViewTitleProps {
  /** Domain name or page title shown in Label/Small on the top line. */
  url?: string;

  /**
   * Whether to render the description line.
   * @default true
   */
  description?: boolean;

  /** Description text (full URL or security label) shown in Body/Small blue below URL. */
  description1?: string;

  /**
   * Whether to render an icon before the description text.
   * @default false
   */
  icon?: boolean;
}
```

---

## Code Examples

### Complete WebView Frame (React)

```tsx
import { useState, useEffect } from 'react';
import { WebViewAppFrame } from '@/components/WebView';
import { IconButton } from '@/components/IconButton';
import { CloseIcon, ShareIcon, MoreVerticalIcon } from '@/icons';

function InAppBrowser({ url, onClose }: { url: string; onClose: () => void }) {
  const [loading, setLoading] = useState(true);
  const [loadProgress, setLoadProgress] = useState(0);
  const [pageTitle, setPageTitle] = useState('');
  const [domain, setDomain] = useState('');

  useEffect(() => {
    try {
      const parsed = new URL(url);
      setDomain(parsed.hostname.replace('www.', ''));
    } catch {
      setDomain(url);
    }
  }, [url]);

  return (
    <div className="webview-container">
      <WebViewAppFrame
        loader={loading}
        loadProgress={loadProgress}
        topNav={{
          leading: (
            <IconButton
              size="medium"
              variant="tertiary"
              aria-label="Close browser"
              icon={<CloseIcon />}
              onPress={onClose}
            />
          ),
          url: pageTitle || domain,
          description: domain,
          descriptionIcon: <LockIcon size={12} aria-hidden="true" />,
          trailing: (
            <IconButton
              size="medium"
              variant="tertiary"
              aria-label="Share page"
              icon={<ShareIcon />}
              onPress={() => shareUrl(url)}
            />
          ),
        }}
      />
      <iframe
        src={url}
        className="webview-content"
        title={pageTitle || domain}
        onLoad={() => {
          setLoading(false);
          setLoadProgress(100);
        }}
      />
      <WebViewToolbar
        canGoBack={false}
        onBack={() => {/* navigate back in iframe history */}}
        trailingContent={
          <IconButton
            size="medium"
            variant="tertiary"
            aria-label="More options"
            icon={<MoreVerticalIcon />}
            onPress={openOptionsMenu}
          />
        }
      />
    </div>
  );
}
```

### WebView.Title Component

```tsx
const WebViewTitle: React.FC<WebViewTitleProps> = ({
  url = 'Title',
  description = true,
  description1 = 'Description',
  icon = false,
}) => (
  <div className="webview-title">
    {/* URL / page title */}
    <div className="webview-title__url-row">
      <span className="webview-title__url">{url}</span>
    </div>

    {/* Description row */}
    {description && (
      <div className="webview-title__desc-row">
        {icon && (
          <span className="webview-title__desc-icon" aria-hidden="true">
            <LockIcon size={16} />
          </span>
        )}
        <span className="webview-title__desc">{description1}</span>
      </div>
    )}
  </div>
);
```

### WebView.Loader with Progress

```tsx
const WebViewLoader: React.FC<{ progress: number; visible: boolean }> = ({
  progress,
  visible,
}) => {
  if (!visible && progress >= 100) return null;

  return (
    <div
      className={`webview-loader ${progress >= 100 ? 'webview-loader--complete' : ''}`}
      role="progressbar"
      aria-valuenow={progress}
      aria-valuemin={0}
      aria-valuemax={100}
      aria-label="Page loading"
    >
      <div
        className="webview-loader__fill"
        style={{ width: `${progress}%` }}
      />
    </div>
  );
};
```

### CSS Styles

```css
/* ─── WebView.TopNav ─── */
.webview-topnav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 48px;
  width: 100%;
  padding: var(--tokens-base-spacing-16, 16px) var(--tokens-base-spacing-8, 8px);
  background: var(--tokens-colorScheme-background-base, #fff);
  border-bottom: 1px solid var(--tokens-colorScheme-border-base, rgba(0,0,0,0.16));
}

.webview-topnav__leading,
.webview-topnav__trailing {
  flex-shrink: 0;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.webview-topnav__center {
  flex: 1;
  max-width: 185px;
  display: flex;
  justify-content: center;
}

/* ─── WebView.Title ─── */
.webview-title {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--tokens-base-spacing-2, 2px);
  height: 40px;
  justify-content: center;
}

.webview-title__url-row,
.webview-title__desc-row {
  display: flex;
  align-items: center;
  gap: var(--tokens-base-spacing-2, 2px);
  flex-shrink: 0;
}

.webview-title__url {
  font-family: var(--tokens-brand-text-fontfamily-label, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-12, 12px);
  line-height: var(--tokens-base-text-lineheight-ramp-12, 16px);
  font-weight: 500;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-base, #000);
  text-align: center;
  white-space: nowrap;
}

.webview-title__desc {
  font-family: var(--tokens-brand-text-fontfamily-body, 'Plain', sans-serif);
  font-size: var(--tokens-base-text-fontsize-ramp-12, 12px);
  line-height: var(--tokens-base-text-lineheight-ramp-12, 16px);
  font-weight: 400;
  letter-spacing: 0;
  color: var(--tokens-colorScheme-content-role-base-info, #0066f5);
  text-align: center;
  white-space: nowrap;
}

.webview-title__desc-icon {
  width: 16px;
  height: 16px;
  flex-shrink: 0;
  color: var(--tokens-colorScheme-content-role-base-info, #0066f5);
}

/* ─── WebView.Loader ─── */
.webview-loader {
  width: 100%;
  height: 4px;
  background: var(--tokens-colorScheme-background-role-base-neutral, rgba(5,55,130,0.08));
  overflow: hidden;
  flex-shrink: 0;
}

.webview-loader__fill {
  height: 4px;
  background: var(--tokens-colorScheme-content-role-base-info, #0066f5);
  transition: width 300ms cubic-bezier(0.4, 0, 0.2, 1);
}

.webview-loader--complete {
  animation: loader-fade-out 300ms ease 200ms forwards;
}

@keyframes loader-fade-out {
  from { opacity: 1; }
  to { opacity: 0; }
}

/* ─── WebView.Toolbar ─── */
.webview-toolbar {
  position: sticky;
  bottom: 0;
  width: 100%;
  background: linear-gradient(
    to bottom,
    rgba(255, 255, 255, 0) 0%,
    #ffffff 40%
  );
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  padding-bottom: env(safe-area-inset-bottom, 0px);
}

.webview-toolbar__inner {
  display: flex;
  align-items: center;
  gap: var(--tokens-base-spacing-8, 8px);
  padding: var(--tokens-base-spacing-8, 8px) var(--tokens-base-spacing-16, 16px);
}

.webview-toolbar__center {
  flex: 1;
  min-width: 0;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.webview-toolbar__trailing {
  flex-shrink: 0;
  display: flex;
  align-items: center;
  gap: var(--tokens-base-spacing-4, 4px);
}

/* ─── WebView.Button ─── */
.webview-button {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  min-width: 40px;
  min-height: 40px;
  padding: var(--tokens-base-spacing-8, 8px);
  background: var(--tokens-colorScheme-background-base, #fff);
  border-radius: var(--tokens-base-border-radius-full, 999px);
  border: none;
  cursor: pointer;
  flex-shrink: 0;
  color: var(--tokens-colorScheme-content-base, #000);
  transition: background 100ms ease;
}

.webview-button:disabled {
  color: var(--tokens-colorScheme-content-faint, rgba(0,0,0,0.48));
  cursor: not-allowed;
}

.webview-button:not(:disabled):active {
  background: var(--tokens-colorScheme-background-muted, #f5f7fa);
}

.webview-button:focus-visible {
  outline: 2px solid var(--tokens-colorScheme-border-focus, #0066f5);
  outline-offset: 2px;
}

.webview-button__icon {
  width: 20px;
  height: 20px;
  flex-shrink: 0;
}
```

---

## Accessibility Specifications

### TopNav Accessibility

| Element | Role / Attribute | Value | Purpose |
|---|---|---|---|
| TopNav container | `<header>` | — | Navigation region |
| URL/title text | — | — | Visible label; not a heading (the web page's `<title>` serves as the page heading) |
| Description text | — | — | Supplementary URL context |
| Leading IconButton | `aria-label` | e.g., `"Close browser"` | Descriptive action label |
| Trailing IconButton | `aria-label` | e.g., `"Share page"` | Descriptive action label |

### Loader Accessibility

```tsx
<div
  role="progressbar"
  aria-valuenow={loadProgress}
  aria-valuemin={0}
  aria-valuemax={100}
  aria-label="Page loading"
  aria-live="polite"
/>
```

For screen readers, supplement the progressbar with a `role="status"` live region that announces when loading begins and completes:

```tsx
<div role="status" aria-live="polite" className="sr-only">
  {loading ? 'Loading page…' : loadComplete ? 'Page loaded' : ''}
</div>
```

### Toolbar Accessibility

| Element | Role / Attribute | Value |
|---|---|---|
| Toolbar container | `<nav aria-label="Browser controls">` | Groups browser navigation controls |
| Back button | `aria-label="Go back"` + `aria-disabled` | Describes action; disables when no history |
| Center slot | varies | Depends on slot content (e.g., Toggle has its own ARIA role) |
| Trailing icon group | `aria-label="Page actions"` | Groups trailing action buttons |

### Back Button Disabled State

The back `WebView.Button` must use `aria-disabled="true"` (not the HTML `disabled` attribute alone) to ensure screen readers still announce the button and its unavailable state:

```tsx
<button
  className="webview-button"
  onClick={canGoBack ? onBack : undefined}
  aria-label="Go back"
  aria-disabled={!canGoBack}
  type="button"
>
  <ChevronLeftIcon className="webview-button__icon" aria-hidden="true" />
</button>
```

### Keyboard Navigation

| Key | Action |
|---|---|
| `Tab` | Move focus through leading icon → trailing icon → toolbar buttons |
| `Enter` / `Space` | Activate focused button |
| `Escape` | Close the WebView (if close button is present) |

---

## Platform APIs

### Android — Jetpack Compose

```kotlin
import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import com.google.accompanist.web.WebView
import com.google.accompanist.web.rememberWebViewState

@Composable
fun InAppWebView(
    url: String,
    onClose: () -> Unit
) {
    val state = rememberWebViewState(url = url)

    Scaffold(
        topBar = {
            // WebView TopNav
            TopAppBar(
                navigationIcon = {
                    IconButton(onClick = onClose) {
                        Icon(Icons.Default.Close, contentDescription = "Close browser")
                    }
                },
                title = {
                    Column(horizontalAlignment = Alignment.CenterHorizontally) {
                        Text(
                            text = state.pageTitle ?: "Loading...",
                            style = MaterialTheme.typography.labelSmall,
                            color = MaterialTheme.colorScheme.onBackground,
                            maxLines = 1,
                            overflow = TextOverflow.Ellipsis
                        )
                        Text(
                            text = extractDomain(url),
                            style = MaterialTheme.typography.bodySmall,
                            color = MaterialTheme.colorScheme.primary, // info blue
                            maxLines = 1
                        )
                    }
                },
                actions = {
                    IconButton(onClick = { shareUrl(url) }) {
                        Icon(Icons.Default.Share, contentDescription = "Share page")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.background
                )
            )
        },
        bottomBar = {
            // WebView Toolbar
            BottomAppBar(
                containerColor = Color.Transparent,
                modifier = Modifier.background(
                    brush = Brush.verticalGradient(
                        colors = listOf(Color.Transparent, Color.White)
                    )
                )
            ) {
                // Back button
                Box(
                    modifier = Modifier
                        .size(40.dp)
                        .clip(CircleShape)
                        .background(MaterialTheme.colorScheme.background)
                        .clickable(enabled = state.canGoBack) { /* go back */ },
                    contentAlignment = Alignment.Center
                ) {
                    Icon(
                        Icons.AutoMirrored.Default.ArrowBack,
                        contentDescription = "Go back",
                        tint = if (state.canGoBack)
                            MaterialTheme.colorScheme.onBackground
                        else
                            MaterialTheme.colorScheme.onBackground.copy(alpha = 0.38f)
                    )
                }
                Spacer(modifier = Modifier.weight(1f))
                // Trailing actions
                IconButton(onClick = { /* more */ }) {
                    Icon(Icons.Default.MoreVert, contentDescription = "More options")
                }
            }
        }
    ) { padding ->
        Column(modifier = Modifier.padding(padding)) {
            // Loading indicator
            if (state.isLoading) {
                LinearProgressIndicator(
                    modifier = Modifier
                        .fillMaxWidth()
                        .height(4.dp),
                    color = MaterialTheme.colorScheme.primary,
                    trackColor = MaterialTheme.colorScheme.primary.copy(alpha = 0.08f)
                )
            }
            // Web content
            WebView(
                state = state,
                modifier = Modifier.fillMaxSize()
            )
        }
    }
}
```

### iOS — SwiftUI

```swift
import SwiftUI
import WebKit

struct InAppWebView: View {
    let url: URL
    let onClose: () -> Void

    @StateObject private var viewModel = WebViewModel()
    @State private var showToolbar = true

    var body: some View {
        VStack(spacing: 0) {
            // TopNav
            HStack {
                // Leading close button
                Button(action: onClose) {
                    Image(systemName: "xmark")
                        .foregroundColor(.primary)
                        .frame(width: 40, height: 40)
                }

                Spacer()

                // Center Title
                VStack(spacing: 2) {
                    Text(viewModel.pageTitle ?? "Loading...")
                        .font(.custom("Plain-Medium", size: 12))
                        .foregroundColor(.primary)
                        .lineLimit(1)

                    HStack(spacing: 2) {
                        if viewModel.isSecure {
                            Image(systemName: "lock.fill")
                                .font(.system(size: 10))
                                .foregroundColor(Color(red: 0, green: 0.40, blue: 0.96))
                        }
                        Text(viewModel.domain)
                            .font(.custom("Plain-Regular", size: 12))
                            .foregroundColor(Color(red: 0, green: 0.40, blue: 0.96))
                            .lineLimit(1)
                    }
                }

                Spacer()

                // Trailing share button
                Button(action: { shareURL(url) }) {
                    Image(systemName: "square.and.arrow.up")
                        .foregroundColor(.primary)
                        .frame(width: 40, height: 40)
                }
            }
            .frame(height: 48)
            .padding(.horizontal, 8)
            .background(Color.white)
            .overlay(
                Divider().frame(maxHeight: 1),
                alignment: .bottom
            )

            // Progress loader
            if viewModel.estimatedProgress < 1.0 {
                ProgressView(value: viewModel.estimatedProgress)
                    .progressViewStyle(LinearProgressViewStyle(tint: Color(red: 0, green: 0.40, blue: 0.96)))
                    .frame(height: 4)
                    .scaleEffect(x: 1, y: 0.5, anchor: .center) // scale to 4px height
            }

            // Web content
            WebViewRepresentable(url: url, viewModel: viewModel)
                .frame(maxWidth: .infinity, maxHeight: .infinity)

            // Toolbar
            HStack(spacing: 8) {
                // Back button (circular white)
                Button(action: { viewModel.goBack() }) {
                    Image(systemName: "chevron.left")
                        .foregroundColor(viewModel.canGoBack ? .primary : .secondary)
                        .frame(width: 20, height: 20)
                }
                .frame(width: 40, height: 40)
                .background(Color.white)
                .clipShape(Circle())
                .disabled(!viewModel.canGoBack)
                .accessibilityLabel("Go back")

                Spacer()

                // Trailing actions
                PDSIconButtonGroup(
                    buttons: [
                        PDSIconButtonGroupItemConfiguration(icon: .star, style: .primary, action: bookmarkPage),
                        PDSIconButtonGroupItemConfiguration(icon: .ellipsis, style: .primary, action: showOptions),
                    ],
                    size: .medium
                )
            }
            .padding(.horizontal, 16)
            .padding(.vertical, 8)
            .background(
                LinearGradient(
                    gradient: Gradient(colors: [.clear, .white]),
                    startPoint: .top,
                    endPoint: .bottom
                )
            )
            .padding(.bottom, 0) // safe area handled by SwiftUI
        }
    }
}
```

---

## Design Tokens Reference

### WebView.TopNav

| Token | Value | Usage |
|---|---|---|
| `tokens.colorScheme.background.base` | `#ffffff` | TopNav background |
| `tokens.colorScheme.border.base` | `rgba(0,0,0,0.16)` | Bottom 1px divider |
| `tokens.base.spacing[8]` | `8px` | Horizontal padding |
| `tokens.base.spacing[16]` | `16px` | Vertical padding |

### WebView.Title

| Token | Value | Usage |
|---|---|---|
| `tokens.brand.text.fontfamily.label` | Plain (Medium) | URL line font |
| `tokens.brand.text.fontfamily.body` | Plain (Regular) | Description line font |
| `tokens.base.text.fontsize.ramp[12]` | `12px` | Both lines font size |
| `tokens.base.text.lineheight.ramp[12]` | `16px` | Both lines line height |
| `tokens.colorScheme.content.base` | `#000000` | URL line color |
| `tokens.colorScheme.content.role.base.info` | `#0066f5` | Description line color |
| `tokens.base.spacing[2]` | `2px` | Gap between URL and description rows; icon-to-text gap |

### WebView.Loader

| Token | Value | Usage |
|---|---|---|
| `tokens.colorScheme.background.role.base.neutral` | `rgba(5,55,130,0.08)` | Track fill |
| `tokens.colorScheme.content.role.base.info` | `#0066f5` | Progress fill |

### WebView.Toolbar

| Token | Value | Usage |
|---|---|---|
| `tokens.colorScheme.background.overlay.fade.bottomStart` | `rgba(255,255,255,0)` | Gradient top (transparent) |
| `tokens.colorScheme.background.overlay.fade.bottomEnd` | `#ffffff` | Gradient bottom (opaque) |
| `tokens.base.spacing[16]` | `16px` | Horizontal padding |
| `tokens.base.spacing[8]` | `8px` | Vertical padding, slot gap |

### WebView.Button

| Token | Value | Usage |
|---|---|---|
| `tokens.colorScheme.background.base` | `#ffffff` | Button fill |
| `tokens.base.border.radius.full` | `999px` | Circle shape |
| `tokens.base.spacing[8]` | `8px` | Button padding |
| `ref.size.550` | `40px` | Button size |

---

## Best Practices

**The TopNav URL display has two distinct typographic roles for a reason.** The URL line (Label/Small, black) is the page's identity — prominent and legible. The description line (Body/Small, blue) is context — the full URL or security status. Never swap their colors or weights; the blue description signals that it relates to a web address without making it the primary visual element.

**The WebView.Button (white circle) is distinct from tertiary IconButton (transparent).** In the Toolbar, the back button must be a `WebView.Button` — not a standard tertiary `IconButton`. The white fill ensures the back button reads clearly above the gradient/blurred background. Using a transparent `IconButton` here would cause it to disappear against light web page content.

**Disable the back button when there's no navigation history.** The back `WebView.Button` should be visually disabled (icon opacity reduced to `content.faint`) and `aria-disabled="true"` on the very first page load. Once the user navigates deeper into the web content, the back button becomes enabled.

**The gradient toolbar height should be generous.** The fade gradient from transparent to white needs sufficient height (~64–80px total including the inner content row) to create a smooth dissolve. Too short a gradient looks abrupt; too tall a gradient obscures too much web content.

**Keep the center toolbar slot intentionally optional.** Most WebView implementations do not need content in the Toolbar center slot. Reserve it for reader mode toggles or URL re-entry bars in use cases where users frequently switch modes. An empty center slot is perfectly valid.

**Never autofocus content inside the WebView on open.** On iOS and Android, focus management when opening the WebView should land on the TopNav's primary action (close button), not inside the web content frame. This allows users to immediately dismiss the WebView without becoming trapped inside the web content's focus order.

---

## Related Components

- **Top Navigation** — Standard in-app page nav bar; the WebView.TopNav follows the same three-slot structure but with URL-specific center content
- **IconButton** — Medium/tertiary variant used in both TopNav slots
- **Shimmer** — Skeleton loading state for the web content area while the WebView initializes
- **Progress Bar** — Standalone progress component; the WebView.Loader is a specialized inline variant

---

## Version History

| Version | Date | Changes |
|---|---|---|
| 6.0.0-beta | 2026-03-18 | Initial Web View documentation from Components v6 Beta Figma file |

---

## Testing Checklist

- [ ] TopNav renders at 48px height (excluding status bar safe area)
- [ ] TopNav background is `background.base` (#ffffff)
- [ ] TopNav 1px bottom border `border.base` visible
- [ ] URL line: Label/Small (12px, 500, Plain, black), centered, nowrap
- [ ] Description line: Body/Small (12px, 400, Plain, #0066f5 blue), centered, nowrap
- [ ] 2px gap between URL and description rows
- [ ] Description icon (16×16px) renders left of description text with 2px gap
- [ ] `description={false}` hides description row; URL row vertically centers in 40px
- [ ] Loader renders at exactly 4px height, 100% width
- [ ] Loader track color: `rgba(5,55,130,0.08)`
- [ ] Loader fill color: `#0066f5` (info blue)
- [ ] Loader fill width animates smoothly with 300ms transition on progress change
- [ ] Loader fades out after page load completes
- [ ] `loader={false}` removes loader element completely (no empty 4px space)
- [ ] Loader has `role="progressbar"` with `aria-valuenow` / `aria-valuemin` / `aria-valuemax`
- [ ] Toolbar gradient: transparent at top → white at bottom
- [ ] Toolbar backdrop blur: 4px blur applied
- [ ] Toolbar sticks to bottom, respects `env(safe-area-inset-bottom)`
- [ ] WebView.Button: 40×40px, white fill, 999px border-radius (full circle)
- [ ] WebView.Button disabled state: icon at `content.faint` opacity (~48%), not full black
- [ ] WebView.Button back: `aria-disabled="true"` when canGoBack=false
- [ ] WebView.Button back: `aria-label="Go back"`
- [ ] Focus ring visible on all focusable elements in chrome (2px `border.focus` blue)
- [ ] Tab order: leading icon → trailing icon → toolbar back button → trailing actions
- [ ] `Escape` key dismisses WebView when applicable
- [ ] iOS safe-area-inset-top applied to entire chrome frame
