# Search Component Documentation

## Overview

The Search component is a specialised text input designed for filtering, querying, and navigation. It is distinguished from the general-purpose Input by its **pill (capsule) shape**, a persistent **search icon** on the left, and a **clear button** (`x-circle-fill`) that appears once the user begins typing and persists while the field has a value.

The component expresses three visual states: **Rest** (unfocused, empty), **Typing** (focused, mid-input with a blue border ring), and **Filled** (unfocused, value present). This state model maps directly to the user's interaction phase and drives both the border treatment and the clear button visibility.

**Component name:** `Search` / `PDSSearchInput`
**Figma node:** `1715-9659`
**Platform:** React / TypeScript (Web), SwiftUI (iOS — `PDSSearchInput`), Jetpack Compose (Android)
**Category:** Form / Input / Navigation

---

## Component Variants

### By State

| State | Node | Focus | Value | Clear Button | Border |
|-------|------|-------|-------|-------------|--------|
| **Rest** | `265:6866` | No | Empty | Hidden | None (grey fill) |
| **Typing** | `1715:9660` | Yes | In-progress | Visible | Blue focus ring |
| **Filled** | `1720:9868` | No | Present | Visible | None (grey fill) |

### By Context

| Context | Description |
|---------|-------------|
| **Standalone** | Full-width search bar on a page header or dedicated search screen |
| **In-list / In-sheet** | Embedded at the top of a Menu (Searchable), Modal, or List to filter visible items |
| **In-header** | Compact search in a Header's End zone, often expanding on focus |

---

## Anatomy

```
┌────────────────────────────────────────┐  ← Pill container (full border-radius)
│ 🔍  Search                             │  ← Rest: icon + placeholder
└────────────────────────────────────────┘

┌────────────────────────────────────────┐  ← Blue focus border (Typing)
│ 🔍  Typing|                        [✕] │  ← Search icon + value + caret + clear
└────────────────────────────────────────┘

┌────────────────────────────────────────┐  ← Grey fill, no border (Filled)
│ 🔍  Input                          [✕] │  ← Search icon + value + clear button
└────────────────────────────────────────┘
```

### Slot Breakdown

| Slot | Content | Visibility |
|------|---------|-----------|
| **Leading icon** | `search` icon (`🔍`) | Always |
| **Input field** | Placeholder or typed value | Always |
| **Clear button** | `CloseCircleFillIcon` (`x-circle-fill`) | Typing + Filled only |

---

## Size & Spacing Specifications

| Property | Value | Token |
|----------|-------|-------|
| Height | 44 px | Meets minimum touch target |
| Width | 100% / 370 px | Fluid |
| Border radius | 999 px (pill) | `tokens.base.border.radius.full` |
| Horizontal padding | 16 px | `tokens.base.spacing[16]` |
| Gap (icon → text) | 8 px | `tokens.base.spacing[8]` |
| Gap (text → clear) | 8 px | `tokens.base.spacing[8]` |
| Search icon size | 16 × 16 px | `Icon` size="small" |
| Clear icon size | 16 × 16 px | `Icon` size="small" |

### Typography (Input Text & Placeholder)

| Property | Value | Token |
|----------|-------|-------|
| Font family | Body | `tokens.brand.text.fontfamily.body` |
| Font weight | Regular | `tokens.brand.text.fontweight.body` |
| Font size | 14 px | `tokens.base.text.fontsize.ramp[14]` |
| Line height | 20 px | `tokens.base.text.lineheight.ramp[14]` |
| Letter spacing | 0 | `tokens.base.text.letterspacing[0]` |

---

## Style Variants

### Background & Border by State

| State | Background | Border | Text color |
|-------|-----------|--------|-----------|
| Rest | `tokens.colorScheme.background.field` (light grey) | None | Placeholder: `tokens.colorScheme.content.placeholder` |
| Typing | `tokens.colorScheme.background.default` (white) | `tokens.colorScheme.border.focus` (blue, 2 px) | `tokens.colorScheme.content.base` |
| Filled | `tokens.colorScheme.background.field` (light grey) | None | `tokens.colorScheme.content.base` |
| Disabled | `tokens.colorScheme.background.disabled` | None | `tokens.colorScheme.content.disabled` |

### Clear Button (`x-circle-fill`)

The clear icon is the `CloseCircleFillIcon` from the icon library:
- **Alt text / aria-label:** `"Clear search"`
- **Node:** `5936:7882`
- **Color:** `tokens.colorScheme.content.muted` (grey, blends with field)
- **Hover color:** `tokens.colorScheme.content.base`
- **Size:** 16 × 16 px

---

## State Specifications

### Rest
- Empty input value
- Grey pill background; no border
- Placeholder text ("Search" or custom) at 14 px, muted color
- Search icon on the left
- No clear button visible
- `inputMode="search"`, `enterKeyHint="search"` for mobile keyboards

### Typing (Focused + In-Progress)
- White background with 2 px blue focus border ring
- Search icon remains on the left
- Caret visible in the input
- Clear button (`x-circle-fill`) appears on the right
- `aria-expanded="true"` if an autocomplete/suggestions panel is shown

### Filled (Unfocused + Has Value)
- Returns to grey pill background (no border — field is blurred)
- Input value is visible in Body/Medium text
- Clear button remains visible, allowing the user to reset the field
- Tapping the field returns to Typing state

### Disabled
- Grey background, muted text color
- `pointer-events: none`; cursor: `not-allowed`
- No hover or focus interaction
- `disabled` attribute on the `<input>` element

### Loading (Results Fetching)
- A `Loader` (X-Small, inside the trailing slot) replaces the clear button while results are being fetched
- Or: an indeterminate `ProgressBar` below the Search field
- `aria-busy="true"` on the container

### Empty Results
- The Search component itself does not change appearance
- A companion `EmptyState` component is rendered in the results area below

---

## Props API

### TypeScript Interface

```typescript
import {
  InputHTMLAttributes,
  ChangeEvent,
  KeyboardEvent,
  ReactNode,
  RefObject,
} from 'react';

export type SearchState = 'rest' | 'typing' | 'filled';

export interface SearchProps
  extends Omit<InputHTMLAttributes<HTMLInputElement>, 'type' | 'size'> {
  /**
   * The current value of the search input.
   * Use in controlled mode; pair with `onChange`.
   */
  value?: string;

  /**
   * Default value for uncontrolled mode.
   */
  defaultValue?: string;

  /**
   * Callback fired when the input value changes.
   */
  onChange?: (event: ChangeEvent<HTMLInputElement>) => void;

  /**
   * Callback fired when the user submits the search (Enter key or search button).
   */
  onSearch?: (value: string) => void;

  /**
   * Callback fired when the clear button is clicked or the value is cleared.
   */
  onClear?: () => void;

  /**
   * Callback fired when the input receives focus.
   */
  onFocus?: (event: React.FocusEvent<HTMLInputElement>) => void;

  /**
   * Callback fired when the input loses focus.
   */
  onBlur?: (event: React.FocusEvent<HTMLInputElement>) => void;

  /**
   * Placeholder text shown in the Rest state.
   * @default 'Search'
   */
  placeholder?: string;

  /**
   * Explicit state override. Normally derived from focus + value.
   * @default derived
   */
  state?: SearchState;

  /**
   * When true, disables the search input.
   * @default false
   */
  disabled?: boolean;

  /**
   * When true, shows a loading indicator in place of the clear button.
   * @default false
   */
  loading?: boolean;

  /**
   * Whether the results dropdown / suggestions panel is currently open.
   * Controls aria-expanded on the input.
   * @default false
   */
  expanded?: boolean;

  /**
   * ID of the associated listbox/results panel element.
   * Used for aria-controls / aria-owns.
   */
  resultsId?: string;

  /**
   * Accessible label for the search input.
   * @default 'Search'
   */
  'aria-label'?: string;

  /**
   * ID of an element that labels the search input.
   */
  'aria-labelledby'?: string;

  /**
   * Ref forwarded to the underlying <input> element.
   */
  inputRef?: RefObject<HTMLInputElement>;

  /**
   * Additional class name(s) applied to the pill container.
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

### 1. Basic Uncontrolled Search

```tsx
import { Search } from '@ds/components';

export function ProductSearch() {
  return (
    <Search
      placeholder="Search products"
      onSearch={query => navigate(`/search?q=${encodeURIComponent(query)}`)}
      aria-label="Search products"
    />
  );
}
```

### 2. Controlled Search with Live Filtering

```tsx
import { useState } from 'react';
import { Search } from '@ds/components';

export function TransactionFilter({ transactions }) {
  const [query, setQuery] = useState('');

  const filtered = transactions.filter(t =>
    t.description.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <div>
      <Search
        value={query}
        onChange={e => setQuery(e.target.value)}
        onClear={() => setQuery('')}
        placeholder="Filter transactions"
        aria-label="Filter transactions"
        aria-controls="transaction-list"
      />

      <ul id="transaction-list" role="list" aria-live="polite" aria-label={`${filtered.length} results`}>
        {filtered.map(t => (
          <li key={t.id}>{t.description}</li>
        ))}
      </ul>
    </div>
  );
}
```

### 3. Search with Autocomplete Suggestions

```tsx
import { useState, useRef } from 'react';
import { Search, Menu, MenuItem } from '@ds/components';

export function SearchWithSuggestions() {
  const [query, setQuery]           = useState('');
  const [suggestions, setSuggestions] = useState<string[]>([]);
  const [isOpen, setIsOpen]         = useState(false);

  const handleChange = async (e: React.ChangeEvent<HTMLInputElement>) => {
    const val = e.target.value;
    setQuery(val);
    if (val.length >= 2) {
      const results = await fetchSuggestions(val);
      setSuggestions(results);
      setIsOpen(results.length > 0);
    } else {
      setSuggestions([]);
      setIsOpen(false);
    }
  };

  const handleSelect = (suggestion: string) => {
    setQuery(suggestion);
    setIsOpen(false);
    performSearch(suggestion);
  };

  return (
    <div style={{ position: 'relative' }}>
      <Search
        value={query}
        onChange={handleChange}
        onClear={() => { setQuery(''); setSuggestions([]); setIsOpen(false); }}
        onSearch={q => { setIsOpen(false); performSearch(q); }}
        placeholder="Search"
        expanded={isOpen}
        resultsId="search-suggestions"
        aria-label="Search"
        aria-autocomplete="list"
      />

      {isOpen && (
        <div
          id="search-suggestions"
          role="listbox"
          aria-label="Search suggestions"
          style={{ position: 'absolute', top: '100%', width: '100%', zIndex: 10 }}
        >
          {suggestions.map(s => (
            <div
              key={s}
              role="option"
              aria-selected="false"
              onClick={() => handleSelect(s)}
            >
              {s}
            </div>
          ))}
        </div>
      )}
    </div>
  );
}
```

### 4. Search in a Mobile Sheet (Searchable Menu)

```tsx
import { useState } from 'react';
import { MenuSearchable, MenuItem, MenuSection } from '@ds/components';

export function ContactPicker({ onSelect }) {
  const [isOpen, setIsOpen] = useState(false);
  const [query, setQuery]   = useState('');

  const contacts = useContacts();
  const filtered = contacts.filter(c =>
    c.name.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <>
      <Button onClick={() => setIsOpen(true)}>Choose recipient</Button>

      <MenuSearchable
        isOpen={isOpen}
        onClose={() => { setIsOpen(false); setQuery(''); }}
        searchPlaceholder="Search contacts"
        searchValue={query}
        onSearchChange={setQuery}
        emptyState={<EmptyState type="no-results" description={`No contacts match "${query}"`} />}
      >
        <MenuSection label={query ? `Results for "${query}"` : 'All contacts'}>
          {filtered.map(contact => (
            <MenuItem
              key={contact.id}
              label={contact.name}
              description={contact.email}
              leadingContent={<Avatar src={contact.avatar} name={contact.name} />}
              onSelect={() => { onSelect(contact); setIsOpen(false); }}
            />
          ))}
        </MenuSection>
      </MenuSearchable>
    </>
  );
}
```

### 5. Search in Page Header

```tsx
import { useState } from 'react';
import { Header, Search } from '@ds/components';

export function AppHeader() {
  const [query, setQuery]       = useState('');
  const [isExpanded, setExpanded] = useState(false);

  return (
    <Header
      variant="app"
      end={
        isExpanded ? (
          <Search
            value={query}
            onChange={e => setQuery(e.target.value)}
            onClear={() => { setQuery(''); setExpanded(false); }}
            onBlur={() => { if (!query) setExpanded(false); }}
            placeholder="Search"
            aria-label="Site search"
            autoFocus
          />
        ) : (
          <IconButton
            icon={<SearchIcon />}
            aria-label="Open search"
            onClick={() => setExpanded(true)}
          />
        )
      }
    />
  );
}
```

### 6. Search with Debounce (API Query)

```tsx
import { useState, useEffect } from 'react';
import { Search } from '@ds/components';

function useDebounce<T>(value: T, delay: number): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);
  return debounced;
}

export function DebouncedSearch({ onResults }) {
  const [query, setQuery]   = useState('');
  const [loading, setLoading] = useState(false);
  const debouncedQuery      = useDebounce(query, 300);

  useEffect(() => {
    if (!debouncedQuery) { onResults([]); return; }
    setLoading(true);
    searchAPI(debouncedQuery)
      .then(onResults)
      .finally(() => setLoading(false));
  }, [debouncedQuery]);

  return (
    <Search
      value={query}
      onChange={e => setQuery(e.target.value)}
      onClear={() => { setQuery(''); onResults([]); }}
      placeholder="Search transactions"
      loading={loading}
      aria-label="Search transactions"
      aria-busy={loading}
    />
  );
}
```

### 7. Search with Keyboard Shortcut Hint

```tsx
import { useEffect, useRef } from 'react';
import { Search } from '@ds/components';

export function GlobalSearch() {
  const inputRef = useRef<HTMLInputElement>(null);

  // Cmd+K / Ctrl+K focuses the search
  useEffect(() => {
    const handler = (e: KeyboardEvent) => {
      if ((e.metaKey || e.ctrlKey) && e.key === 'k') {
        e.preventDefault();
        inputRef.current?.focus();
      }
    };
    window.addEventListener('keydown', handler);
    return () => window.removeEventListener('keydown', handler);
  }, []);

  return (
    <div className="relative">
      <Search
        placeholder="Search"
        inputRef={inputRef}
        aria-label="Global search (⌘K)"
      />
      <kbd
        className="absolute right-3 top-1/2 -translate-y-1/2 text-xs text-muted"
        aria-hidden="true"
      >
        ⌘K
      </kbd>
    </div>
  );
}
```

### 8. Search with Recent Queries

```tsx
import { useState } from 'react';
import { Search } from '@ds/components';

const MAX_RECENT = 5;

export function SearchWithHistory() {
  const [query, setQuery]   = useState('');
  const [recent, setRecent] = useState<string[]>(
    () => JSON.parse(localStorage.getItem('recentSearches') || '[]')
  );
  const [showRecent, setShowRecent] = useState(false);

  const handleSearch = (q: string) => {
    if (!q.trim()) return;
    const updated = [q, ...recent.filter(r => r !== q)].slice(0, MAX_RECENT);
    setRecent(updated);
    localStorage.setItem('recentSearches', JSON.stringify(updated));
    setShowRecent(false);
    performSearch(q);
  };

  return (
    <div>
      <Search
        value={query}
        onChange={e => setQuery(e.target.value)}
        onFocus={() => setShowRecent(true)}
        onBlur={() => setTimeout(() => setShowRecent(false), 150)}
        onSearch={handleSearch}
        onClear={() => setQuery('')}
        placeholder="Search"
        aria-label="Search"
      />

      {showRecent && !query && recent.length > 0 && (
        <ul role="listbox" aria-label="Recent searches">
          {recent.map(r => (
            <li key={r} role="option" aria-selected="false" onClick={() => handleSearch(r)}>
              <ClockIcon aria-hidden /> {r}
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

---

## Accessibility Specifications

### Semantic HTML

```html
<div role="search" aria-label="Site search">
  <div class="search-container">
    <!-- Leading icon (decorative) -->
    <svg aria-hidden="true" focusable="false"><!-- search icon --></svg>

    <!-- Native input -->
    <input
      type="search"
      id="site-search"
      name="q"
      placeholder="Search"
      aria-label="Search"
      inputmode="search"
      enterkeyhint="search"
      autocomplete="off"
      autocorrect="off"
      autocapitalize="off"
      spellcheck="false"
    />

    <!-- Clear button (visible when value present) -->
    <button
      type="button"
      aria-label="Clear search"
      tabindex="0"
    >
      <svg aria-hidden="true" focusable="false"><!-- x-circle-fill --></svg>
    </button>
  </div>
</div>
```

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `type` | `<input>` | `"search"` — tells AT it's a search field |
| `role` | Landmark container | `"search"` |
| `aria-label` | `<input>` or container | Descriptive name (e.g., `"Search transactions"`) |
| `aria-labelledby` | `<input>` | ID of a visible heading/label above the field |
| `aria-autocomplete` | `<input>` | `"list"` (when showing suggestions), `"none"` otherwise |
| `aria-expanded` | `<input>` | `"true"` when suggestions panel is open |
| `aria-controls` | `<input>` | ID of the results listbox |
| `aria-activedescendant` | `<input>` | ID of the currently highlighted suggestion |
| `aria-busy` | Container | `"true"` while results are loading |
| `aria-label` | Clear button | `"Clear search"` |
| `role` | Results panel | `"listbox"` |
| `role` | Each suggestion | `"option"` |
| `aria-selected` | Each suggestion | `"true"` when keyboard-highlighted |

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| `Tab` | Focuses the Search input; Tab again moves to the clear button (if visible), then exits |
| `Enter` | Submits the search query; fires `onSearch` |
| `Escape` | Closes suggestions panel (if open); second press clears the value and returns to Rest state |
| `ArrowDown` | Moves focus/highlight to the first suggestion in an open panel |
| `ArrowUp` | From suggestions panel, returns focus to the input |
| `ArrowDown / Up` | Cycles through suggestions when panel is open |
| `Backspace` | Deletes characters; when value is empty, may dismiss surrounding context (e.g., a tag/chip) |

### Clear Button Accessibility

The clear button must:
1. Have `type="button"` (not submit) to prevent form submission
2. Have `aria-label="Clear search"` (icon is decorative — `aria-hidden`)
3. Be focusable (`tabindex="0"`)
4. On activation, clear the value and return focus to the `<input>`

```tsx
const handleClear = () => {
  onChange('');           // clear value
  onClear?.();            // fire callback
  inputRef.current?.focus(); // return focus to input
};
```

### Mobile Keyboard Optimisation

```html
<input
  type="search"
  inputmode="search"
  enterkeyhint="search"
  autocomplete="off"
  autocorrect="off"
  autocapitalize="none"
  spellcheck="false"
/>
```

- `inputmode="search"` hints to mobile browsers to show the "Search" return key
- `enterkeyhint="search"` shows a magnifying glass on the virtual keyboard's return key
- `autocapitalize="none"` prevents auto-capitalisation of search terms
- `autocorrect="off"` / `spellcheck="false"` prevent auto-corrections in search queries

### Screen Reader Announcements

- On focus: _"Search, edit text"_ (from `type="search"`)
- On value entry: characters are read as typed
- When suggestions open: _"[n] results available"_ via `aria-live` or `role="status"`
- When clear button is activated: focus returns to input; screen reader announces the now-empty field
- Live filter results: use `aria-live="polite"` on the results count:

```html
<div aria-live="polite" aria-atomic="true" class="sr-only">
  12 results for "visa"
</div>
```

### Color Contrast

| Element | Ratio | WCAG |
|---------|-------|------|
| Placeholder text on grey background | 4.5:1 | 1.4.3 |
| Input text on white (Typing) | 4.5:1 | 1.4.3 |
| Input text on grey (Filled) | 4.5:1 | 1.4.3 |
| Focus border on white | 3:1 | 1.4.11 |
| Search icon on grey | 3:1 | 1.4.11 |
| Clear icon on grey | 3:1 | 1.4.11 |

---

## Animation Specifications

### Focus Transition (Rest → Typing)

```css
.search-container {
  transition:
    background-color 150ms ease,
    border-color 150ms ease,
    box-shadow 150ms ease;
}

.search-container:focus-within {
  background-color: var(--tokens.colorScheme.background.default);
  box-shadow: 0 0 0 2px var(--tokens.colorScheme.border.focus);
}
```

### Clear Button Appear / Disappear

```css
.search-clear {
  transition: opacity 150ms ease, visibility 150ms ease;
}

.search-clear--hidden {
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
}

.search-clear--visible {
  opacity: 1;
  visibility: visible;
}
```

### Loading Spinner (Replaces Clear)

```css
.search-trailing {
  transition: opacity 100ms ease;
}
/* Loader fades in; clear button fades out while loading */
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .search-container,
  .search-clear {
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Container ───────────────────────────────────────────────────────── */
--search-height:              44px;
--search-border-radius:       var(--tokens.base.border.radius.full);         /* 999px — pill */
--search-padding-x:           var(--tokens.base.spacing[16]);                /* 16px */
--search-gap:                 var(--tokens.base.spacing[8]);                 /* 8px */

/* ─── Background & Border ─────────────────────────────────────────────── */
--search-bg-rest:             var(--tokens.colorScheme.background.field);    /* light grey */
--search-bg-typing:           var(--tokens.colorScheme.background.default);  /* white */
--search-bg-filled:           var(--tokens.colorScheme.background.field);    /* light grey */
--search-bg-disabled:         var(--tokens.colorScheme.background.disabled);
--search-border-focus:        var(--tokens.colorScheme.border.focus);        /* blue */
--search-border-width:        2px;

/* ─── Typography ──────────────────────────────────────────────────────── */
--search-font-family:         var(--tokens.brand.text.fontfamily.body);
--search-font-size:           var(--tokens.base.text.fontsize.ramp[14]);     /* 14px */
--search-font-weight:         var(--tokens.brand.text.fontweight.body);      /* Regular */
--search-line-height:         var(--tokens.base.text.lineheight.ramp[14]);   /* 20px */
--search-letter-spacing:      var(--tokens.base.text.letterspacing[0]);      /* 0 */
--search-text-color:          var(--tokens.colorScheme.content.base);
--search-placeholder-color:   var(--tokens.colorScheme.content.placeholder);
--search-text-disabled:       var(--tokens.colorScheme.content.disabled);

/* ─── Icons ───────────────────────────────────────────────────────────── */
--search-icon-size:           16px;
--search-icon-color:          var(--tokens.colorScheme.content.muted);
--search-icon-color-focus:    var(--tokens.colorScheme.content.base);
--search-clear-color:         var(--tokens.colorScheme.content.muted);
--search-clear-color-hover:   var(--tokens.colorScheme.content.base);
```

---

## Best Practices

### Do

- **Use `type="search"`** on the underlying `<input>` — This activates browser-native behaviours: clearing via the `×` button in some browsers, correct "Search" return key on mobile, and semantic identification for AT.
- **Always provide an accessible label** — Either `aria-label="Search [context]"` or a visible `<label>` associated via `for`/`id`. Never rely on the placeholder alone as a label.
- **Wrap the Search in a `role="search"` landmark** when it is a primary page search — This allows screen reader users to jump directly to the search region with landmark navigation.
- **Return focus to the input after clearing** — When the user clicks the clear button, move focus back to the `<input>` immediately so they can start typing a new query without an extra Tab.
- **Debounce API calls** — Fire network requests only after the user has paused typing (300–500 ms delay) to avoid flooding the server and showing intermediate results.
- **Show a loading state** — Replace the clear button with a `Loader` (X-Small) while results are being fetched so users know the system is responding.
- **Announce live result counts** — Use `aria-live="polite"` to announce the number of results as they update (e.g., "8 results for PayPal").
- **Use `inputmode="search"` and `enterkeyhint="search"`** to show the correct virtual keyboard on iOS and Android.

### Don't

- **Don't use a Search component for non-search inputs** — Search has specific visual and semantic connotations. For name fields, email fields, or filters with select inputs, use the standard `Input` component.
- **Don't rely on the placeholder as a label** — Placeholders disappear when the user starts typing. Always pair with an `aria-label` or a visible `<label>`.
- **Don't fire a request on every keystroke** — Always debounce. Immediate requests on every character cause performance problems and produce confusing intermediate states.
- **Don't show a disabled Search without explanation** — If Search is temporarily unavailable, provide context (e.g., "Search is unavailable while offline") rather than just greying it out.
- **Don't hide the clear button in the Filled state** — The persistent clear button in the Filled state is essential UX; users who have blurred the field need to be able to erase their query without triple-clicking.
- **Don't use a Search bar for navigation where a `<nav>` with links suffices** — Reserve Search for querying dynamic or large datasets, not for browsing a small list of known destinations.

---

## Search vs. Related Components

| Component | When to Use |
|-----------|------------|
| **Search** | Querying and filtering content; freeform text entry with search semantics |
| **Input (Text)** | General text input; form fields; no search semantics needed |
| **Menu (Searchable)** | Filtered selection list within a bottom sheet; Search is embedded in the Menu |
| **Select** | Choosing from a fixed, known list; no freeform text entry |
| **Chips (Filter)** | Applying pre-defined filter tags; not freeform text |
| **Input (Search type)** | Same as Search component — these map to the same implementation |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Input** | Parent pattern; Search is a specialised Input with pill shape and fixed leading icon |
| **Menu (Searchable)** | Embeds Search at the top of a bottom sheet to filter a list |
| **Icon** | `search` icon (leading) and `x-circle-fill` (clear button) used within Search |
| **Loader** | X-Small Loader replaces the clear button while results are loading |
| **Empty State** | Shown in the results area when no items match the search query |
| **Chips** | Often used alongside Search for active filter tags |
| **Header** | Search may appear collapsed in the Header's End zone, expanding on tap |
| **List** | Common results container filtered by Search value |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Standardised Rest / Typing / Filled state model; added `loading` prop; confirmed pill shape with `border-radius: full`; aligned clear button to `x-circle-fill` icon |
| 5.1 | Added `onSearch` callback for Enter-key submission |
| 5.0 | Added clear button in Typing and Filled states |
| 4.0 | Initial release |

---

## Testing Checklist

### Visual
- [ ] Rest state: grey pill background, no border, placeholder text, search icon, no clear button
- [ ] Typing state: white background, 2 px blue focus border, search icon, cursor, clear button
- [ ] Filled state: grey pill background, no border, value text, search icon, clear button
- [ ] Disabled state: muted background, muted text, no cursor
- [ ] Pill shape (full border-radius) renders correctly
- [ ] Search icon is 16 × 16 px, left-aligned with 16 px horizontal padding
- [ ] Clear icon is 16 × 16 px, right-aligned with 16 px horizontal padding
- [ ] Input text is 14 px Body/Regular
- [ ] Placeholder text is muted color; value text is base color

### Behavior
- [ ] Typing in the field switches to Typing state (blue border + clear button)
- [ ] Blurring with a value switches to Filled state (grey background)
- [ ] Blurring with empty value returns to Rest state
- [ ] Clear button click resets value and returns focus to input
- [ ] `onSearch` fires on Enter key press
- [ ] `onClear` fires when clear button is clicked
- [ ] `loading={true}` replaces clear button with Loader
- [ ] Debounce prevents excessive `onChange` calls in rapid-typing scenarios
- [ ] Suggestions panel opens/closes correctly with `expanded` prop

### Keyboard
- [ ] Tab focuses the input
- [ ] Tab while focused moves to clear button (if visible), then exits
- [ ] Enter fires `onSearch` with current value
- [ ] Escape closes suggestions (if open); second Escape clears value
- [ ] ArrowDown moves focus to first suggestion when panel is open
- [ ] ArrowUp returns focus to input from suggestions panel

### Accessibility
- [ ] `<input>` has `type="search"`
- [ ] Container has `role="search"` when used as a primary page search
- [ ] `aria-label` or `aria-labelledby` is present
- [ ] `inputmode="search"` and `enterkeyhint="search"` are set
- [ ] Clear button has `aria-label="Clear search"` and is keyboard-focusable
- [ ] `aria-expanded` reflects suggestions panel open/closed state
- [ ] `aria-controls` references the results panel ID
- [ ] Result count announced via `aria-live="polite"` on change
- [ ] Search icon is `aria-hidden="true"` and `focusable="false"`
- [ ] Placeholder text meets 4.5:1 contrast ratio
- [ ] Focus ring meets 3:1 non-text contrast ratio

### Mobile
- [ ] Virtual keyboard shows "Search" return key (`enterkeyhint="search"`)
- [ ] `autocapitalize="none"` prevents unwanted capitalisation
- [ ] Touch target height meets 44 px minimum
