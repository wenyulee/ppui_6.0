# Menu Component Documentation

## Overview

The Menu component presents a list of actions, options, or navigation targets in a temporary overlay surface. It adapts across platforms and viewport sizes: on desktop it renders as a compact floating dropdown/popover anchored to a trigger element; on mobile it rises as a bottom sheet. Menus can be simple flat lists, sectioned with labels, enriched with icons or metadata, or made searchable for large option sets.

**Component name:** `Menu`
**Figma nodes:** `242-27099` (Mobile), `833-9921` (Mobile Searchable), `2718-9159` (Desktop), `9075-27936` (Parts)
**Platform:** React / TypeScript (Web), SwiftUI (iOS), Jetpack Compose (Android)
**Category:** Overlay / Navigation

---

## Component Variants

### By Platform / Presentation

| Variant | Node | Surface | Trigger Pattern |
|---------|------|---------|-----------------|
| **Desktop** | `2718-9159` | Floating dropdown / popover | Button, icon button, or any interactive element |
| **Mobile** | `242-27099` | Bottom sheet modal | Tap on trigger; slides up from bottom edge |
| **Mobile Searchable** | `833-9921` | Bottom sheet with search input | Same as Mobile; search field appears at top |

### By Content Structure

| Variant | Description |
|---------|-------------|
| **Basic** | Title (mobile) or anchor (desktop) + flat list of items |
| **Sectioned** | One or more labeled sections separating groups of items |
| **Multi-Section** | Two independent sections with individual `SectionLabel` headers |
| **Searchable** | Search input field at top filters the item list in real time |
| **With Footer** | Optional footer area for attribution, actions, or a call to action |
| **With Icons / Avatars** | List items include a leading icon or avatar slot |
| **With Descriptions** | List items include a secondary description line beneath the label |

---

## Anatomy

### Desktop Menu

```
┌─────────────────────────────────┐
│ Section label           (muted) │  ← Menu.SectionLabel (optional)
├─────────────────────────────────┤
│ [Icon]  Label                   │  ← MenuItem
│ [Icon]  Label                   │  ← MenuItem
│ [Icon]  Label                   │  ← MenuItem
│ [Icon]  Label                   │  ← MenuItem
├─────────────────────────────────┤
│ Section label           (muted) │  ← Menu.SectionLabel (optional, section 2)
│ [Icon]  Label                   │
│ [Icon]  Label                   │
├─────────────────────────────────┤
│ [Footer content]                │  ← Menu.Footer (optional)
└─────────────────────────────────┘
```

### Mobile Menu (Bottom Sheet)

```
┌─────────────────────────────────┐
│ ████████████████  (drag handle) │
│ Title                    [✕]    │  ← TopNav / Header
├─────────────────────────────────┤
│ [Icon]  Label                   │
│         Description             │  ← MenuItem with description
│ [Icon]  Label                   │
│         Description             │
│ [Icon]  Label                   │
│         Description             │
├─────────────────────────────────┤
│ [Home indicator]                │
└─────────────────────────────────┘
```

### Mobile Searchable Menu

```
┌─────────────────────────────────┐
│ ████████████████  (drag handle) │
│                          [✕]    │
├─────────────────────────────────┤
│ [🔍  Label              ]       │  ← Search input (Rest state)
│ [🔍  Value              ]       │  ← Search input (Typing state, focused)
├─────────────────────────────────┤
│ Section header                  │
│ Label                           │
│ Label                           │
│ Label                           │
├─────────────────────────────────┤
│ [Home indicator]                │
└─────────────────────────────────┘
```

### Sub-Components (Parts)

| Part | Node | Description |
|------|------|-------------|
| `Menu.SectionLabel` | `834:10720` | Small muted label above a group of items; 12 px Label/Small font; `tokens.colorScheme.content.muted` color |
| `Menu.Footer` | `754:9131` | Optional footer row; accommodates attribution (e.g., a logo), secondary buttons, or legal links |

---

## Size Variants

### Desktop

| Dimension | Value | Token |
|-----------|-------|-------|
| Width | 370 px | Fixed; can be configured per usage |
| Min height | Content-driven | — |
| Padding top | 16 px | `tokens.base.spacing[16]` |
| Padding bottom | 12 px | `tokens.base.spacing[12]` |
| Padding horizontal | 16 px | `tokens.base.spacing[16]` |
| Border radius | 12 px | `tokens.base.border.radius[12]` |
| Item vertical padding | 12 px each side | `tokens.base.spacing[12]` |
| Item gap | 0 px | `tokens.base.spacing[0]` |
| Section label gap | 8 px | `tokens.base.spacing[8]` |

### Mobile (Bottom Sheet)

| Dimension | Value |
|-----------|-------|
| Width | Full viewport width |
| Max height | ~80 vh (scrollable beyond) |
| Border radius (top) | 16 px |
| Item min-height | 64 px (with description) / 48 px (label only) |
| Horizontal padding | 16 px |

---

## Style Variants

### Desktop Menu Elevation

The desktop Menu uses **Elevation / Level 1** — two stacked drop-shadows:

```
shadow-1: x=0, y=0,  blur=8,  spread=0, color=tokens.colorScheme.shadow.emphasis (rgba(5,55,130,0.08))
shadow-2: x=0, y=4,  blur=4,  spread=0, color=tokens.colorScheme.shadow.base    (rgba(5,55,130,0.04))
```

### Item States

| State | Background | Text color |
|-------|-----------|-----------|
| Default | transparent | `tokens.colorScheme.content.base` |
| Hover | `tokens.colorScheme.background.hover` | `tokens.colorScheme.content.base` |
| Pressed / Active | `tokens.colorScheme.background.pressed` | `tokens.colorScheme.content.base` |
| Focused | `tokens.colorScheme.background.hover` + focus ring | `tokens.colorScheme.content.base` |
| Disabled | transparent | `tokens.colorScheme.content.disabled` |
| Selected | `tokens.colorScheme.background.selected` | `tokens.colorScheme.content.base` |
| Destructive | transparent | `tokens.colorScheme.content.role.danger` |

---

## State Specifications

### Closed (Default)
- Menu surface is not rendered in the DOM (or conditionally rendered with `display:none` if pre-rendered for performance)
- Trigger element shows default state

### Opening
- Trigger activates (`click` / `Enter` / `Space`)
- Menu surface mounts and enters with animation (see Animation Specifications)
- Focus moves to the first non-disabled menu item (or to the search input for searchable menus)
- `aria-expanded="true"` set on trigger

### Open
- Menu surface is fully visible
- Arrow key navigation moves focus between items
- `Escape` closes menu and returns focus to trigger
- Clicking outside (backdrop) closes menu

### Closing
- Menu exit animation plays
- Menu surface unmounts or becomes `display:none`
- Focus returns to the element that triggered the menu
- `aria-expanded="false"` set on trigger

### Mobile Searchable — Rest State
- Search input shows placeholder label
- Full item list is visible below

### Mobile Searchable — Typing State
- Search input is focused with an active value
- Item list filters in real time to match the query
- Empty state shown if no items match

### Item — Disabled
- Item is not interactive (`pointer-events: none`)
- `aria-disabled="true"` prevents keyboard selection
- Reduced opacity or muted text color

### Item — Destructive
- Rendered with `tokens.colorScheme.content.role.danger` (typically red)
- Used for delete, remove, or sign-out actions

---

## Props API

### TypeScript Interface

```typescript
import { ReactNode, HTMLAttributes, MouseEvent, KeyboardEvent } from 'react';

// ─── Menu Item ───────────────────────────────────────────────────────────────

export interface MenuItemProps {
  /**
   * Primary text label for the item.
   */
  label: string;

  /**
   * Optional secondary description text rendered below the label.
   */
  description?: string;

  /**
   * Leading icon or avatar rendered before the label.
   */
  leadingContent?: ReactNode;

  /**
   * Trailing content rendered after the label (e.g., shortcut hint, chevron, badge).
   */
  trailingContent?: ReactNode;

  /**
   * When true, the item cannot be interacted with.
   * @default false
   */
  disabled?: boolean;

  /**
   * Marks the item as currently selected (for use in selection menus).
   * @default false
   */
  selected?: boolean;

  /**
   * Renders the item label in a danger/destructive color.
   * Use for destructive actions such as "Delete" or "Remove".
   * @default false
   */
  destructive?: boolean;

  /**
   * Callback fired when the item is clicked or activated via keyboard.
   */
  onSelect?: (event: MouseEvent | KeyboardEvent) => void;

  /**
   * Unique identifier for the item; used as the key and for controlled selection.
   */
  value?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Menu Section ────────────────────────────────────────────────────────────

export interface MenuSectionProps {
  /**
   * Optional section label rendered above the group of items.
   */
  label?: string;

  /**
   * The MenuItem(s) within this section.
   */
  children: ReactNode;
}

// ─── Menu Footer ─────────────────────────────────────────────────────────────

export interface MenuFooterProps {
  /**
   * Content to render inside the footer area.
   */
  children: ReactNode;

  /**
   * Additional class name(s) applied to the footer container.
   */
  className?: string;
}

// ─── Menu (Desktop Dropdown) ─────────────────────────────────────────────────

export interface MenuProps {
  /**
   * The trigger element that opens the menu.
   * Should be a Button or IconButton.
   */
  trigger: ReactNode;

  /**
   * The menu items and/or sections to render inside the menu surface.
   */
  children: ReactNode;

  /**
   * Controls whether the menu is open (controlled mode).
   * If omitted, the component manages open state internally (uncontrolled).
   */
  isOpen?: boolean;

  /**
   * Callback fired when the open state should change.
   */
  onOpenChange?: (isOpen: boolean) => void;

  /**
   * Placement of the menu relative to the trigger.
   * @default 'bottom-start'
   */
  placement?: 'bottom-start' | 'bottom-end' | 'bottom' | 'top-start' | 'top-end' | 'top';

  /**
   * Gap (px) between the trigger and the menu surface.
   * @default 4
   */
  offset?: number;

  /**
   * Width of the menu surface.
   * 'trigger' matches the width of the trigger element.
   * @default 370
   */
  width?: number | 'trigger';

  /**
   * When true, closes the menu when an item is selected.
   * @default true
   */
  closeOnSelect?: boolean;

  /**
   * When true, closes the menu when the user clicks outside of it.
   * @default true
   */
  closeOnClickOutside?: boolean;

  /**
   * Additional class name(s) applied to the menu surface.
   */
  className?: string;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Menu (Mobile Bottom Sheet) ──────────────────────────────────────────────

export interface MenuMobileProps {
  /**
   * Controls visibility of the bottom sheet.
   */
  isOpen: boolean;

  /**
   * Callback fired when the sheet requests to close (backdrop tap, swipe down, close button).
   */
  onClose: () => void;

  /**
   * Title rendered in the TopNav bar of the sheet.
   */
  title?: string;

  /**
   * The menu items and/or sections to render inside the sheet.
   */
  children: ReactNode;

  /**
   * Optional footer content.
   */
  footer?: ReactNode;

  /**
   * When true, prevents the sheet from being dismissed by tapping the backdrop.
   * @default false
   */
  preventClose?: boolean;

  /**
   * Test ID for automated testing.
   */
  'data-testid'?: string;
}

// ─── Menu (Mobile Searchable) ─────────────────────────────────────────────────

export interface MenuSearchableProps extends MenuMobileProps {
  /**
   * Placeholder text shown in the search input.
   * @default 'Search'
   */
  searchPlaceholder?: string;

  /**
   * Controlled search query value.
   */
  searchValue?: string;

  /**
   * Callback fired when the search query changes.
   */
  onSearchChange?: (value: string) => void;

  /**
   * Content shown when no items match the search query.
   */
  emptyState?: ReactNode;
}
```

---

## Code Examples

### 1. Basic Desktop Menu

```tsx
import { Menu, MenuItem } from '@ds/components';

export function ActionMenu() {
  return (
    <Menu
      trigger={<Button variant="secondary">Options</Button>}
      placement="bottom-start"
    >
      <MenuItem label="Edit"   onSelect={() => handleEdit()}   />
      <MenuItem label="Duplicate" onSelect={() => handleDuplicate()} />
      <MenuItem label="Archive" onSelect={() => handleArchive()} />
      <MenuItem
        label="Delete"
        destructive
        onSelect={() => handleDelete()}
      />
    </Menu>
  );
}
```

### 2. Menu with Section Labels

```tsx
import { Menu, MenuSection, MenuItem } from '@ds/components';

export function AccountMenu() {
  return (
    <Menu trigger={<IconButton aria-label="Account options" icon={<UserIcon />} />}>
      <MenuSection label="Account">
        <MenuItem label="Profile"   leadingContent={<PersonIcon />} onSelect={() => goToProfile()} />
        <MenuItem label="Settings"  leadingContent={<GearIcon />}   onSelect={() => goToSettings()} />
        <MenuItem label="Billing"   leadingContent={<CardIcon />}   onSelect={() => goToBilling()} />
      </MenuSection>
      <MenuSection label="Support">
        <MenuItem label="Help Centre"  leadingContent={<HelpIcon />}  onSelect={() => openHelp()} />
        <MenuItem label="Send Feedback" leadingContent={<FlagIcon />} onSelect={() => openFeedback()} />
      </MenuSection>
      <MenuSection>
        <MenuItem label="Sign out" destructive onSelect={() => signOut()} />
      </MenuSection>
    </Menu>
  );
}
```

### 3. Multi-Section Menu with Footer

```tsx
import { Menu, MenuSection, MenuItem, MenuFooter } from '@ds/components';

export function PaymentMenu() {
  return (
    <Menu trigger={<Button>Pay with</Button>} width={370}>
      <MenuSection label="Saved methods">
        <MenuItem
          label="Visa ••••4242"
          leadingContent={<VisaIcon />}
          trailingContent={<CheckIcon />}
          selected
          onSelect={() => selectCard('visa')}
        />
        <MenuItem
          label="Mastercard ••••8888"
          leadingContent={<McIcon />}
          onSelect={() => selectCard('mc')}
        />
      </MenuSection>
      <MenuSection>
        <MenuItem
          label="Add new card"
          leadingContent={<PlusIcon />}
          onSelect={() => openAddCard()}
        />
      </MenuSection>
      <MenuFooter>
        <p className="text-xs text-muted">Secured by PayPal</p>
      </MenuFooter>
    </Menu>
  );
}
```

### 4. Mobile Bottom Sheet Menu

```tsx
import { useState } from 'react';
import { MenuMobile, MenuItem } from '@ds/components';

export function MobileActionMenu() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <Button onClick={() => setIsOpen(true)}>More options</Button>

      <MenuMobile
        isOpen={isOpen}
        onClose={() => setIsOpen(false)}
        title="Choose an action"
      >
        <MenuItem
          label="Share"
          description="Send to a friend or group"
          leadingContent={<ShareIcon />}
          onSelect={() => { handleShare(); setIsOpen(false); }}
        />
        <MenuItem
          label="Download"
          description="Save to your device"
          leadingContent={<DownloadIcon />}
          onSelect={() => { handleDownload(); setIsOpen(false); }}
        />
        <MenuItem
          label="Report"
          description="Flag inappropriate content"
          leadingContent={<FlagIcon />}
          destructive
          onSelect={() => { handleReport(); setIsOpen(false); }}
        />
      </MenuMobile>
    </>
  );
}
```

### 5. Mobile Searchable Menu

```tsx
import { useState } from 'react';
import { MenuSearchable, MenuItem, MenuSection } from '@ds/components';

const COUNTRIES = [
  { code: 'US', name: 'United States' },
  { code: 'GB', name: 'United Kingdom' },
  { code: 'CA', name: 'Canada' },
  { code: 'AU', name: 'Australia' },
  // ...more countries
];

export function CountryPicker({ onSelect }: { onSelect: (code: string) => void }) {
  const [isOpen, setIsOpen] = useState(false);
  const [query, setQuery] = useState('');

  const filtered = COUNTRIES.filter(c =>
    c.name.toLowerCase().includes(query.toLowerCase())
  );

  return (
    <>
      <Button onClick={() => setIsOpen(true)}>Select country</Button>

      <MenuSearchable
        isOpen={isOpen}
        onClose={() => { setIsOpen(false); setQuery(''); }}
        searchPlaceholder="Search countries"
        searchValue={query}
        onSearchChange={setQuery}
        emptyState={<p>No countries match "{query}"</p>}
      >
        <MenuSection label="Results">
          {filtered.map(country => (
            <MenuItem
              key={country.code}
              value={country.code}
              label={country.name}
              onSelect={() => { onSelect(country.code); setIsOpen(false); }}
            />
          ))}
        </MenuSection>
      </MenuSearchable>
    </>
  );
}
```

### 6. Controlled Menu (Open State Managed Externally)

```tsx
import { useState } from 'react';
import { Menu, MenuItem } from '@ds/components';

export function ControlledMenu() {
  const [isOpen, setIsOpen] = useState(false);
  const [selectedAction, setSelectedAction] = useState<string | null>(null);

  const handleSelect = (action: string) => {
    setSelectedAction(action);
    setIsOpen(false);
    trackEvent('menu_item_selected', { action });
  };

  return (
    <div>
      <Menu
        isOpen={isOpen}
        onOpenChange={setIsOpen}
        trigger={<Button variant="tertiary">Actions</Button>}
        closeOnSelect={false} // we handle close manually
      >
        <MenuItem label="View details" onSelect={() => handleSelect('view')} />
        <MenuItem label="Edit"         onSelect={() => handleSelect('edit')} />
        <MenuItem label="Export"       onSelect={() => handleSelect('export')} />
      </Menu>
      {selectedAction && <p>Last action: {selectedAction}</p>}
    </div>
  );
}
```

### 7. Menu with Trailing Shortcuts

```tsx
import { Menu, MenuItem } from '@ds/components';

const ShortcutHint = ({ keys }: { keys: string }) => (
  <span className="text-xs text-muted">{keys}</span>
);

export function EditMenu() {
  return (
    <Menu trigger={<Button variant="ghost">Edit</Button>}>
      <MenuItem label="Undo"  trailingContent={<ShortcutHint keys="⌘Z" />}  onSelect={undo}  />
      <MenuItem label="Redo"  trailingContent={<ShortcutHint keys="⌘⇧Z" />} onSelect={redo}  />
      <MenuItem label="Cut"   trailingContent={<ShortcutHint keys="⌘X" />}  onSelect={cut}   />
      <MenuItem label="Copy"  trailingContent={<ShortcutHint keys="⌘C" />}  onSelect={copy}  />
      <MenuItem label="Paste" trailingContent={<ShortcutHint keys="⌘V" />}  onSelect={paste} />
    </Menu>
  );
}
```

### 8. Context Menu (Right-Click)

```tsx
import { useState, useCallback } from 'react';
import { Menu, MenuItem } from '@ds/components';

export function FileContextMenu({ fileName }: { fileName: string }) {
  const [position, setPosition] = useState<{ x: number; y: number } | null>(null);

  const handleContextMenu = useCallback((e: React.MouseEvent) => {
    e.preventDefault();
    setPosition({ x: e.clientX, y: e.clientY });
  }, []);

  return (
    <div onContextMenu={handleContextMenu}>
      <p>{fileName}</p>

      {position && (
        <Menu
          isOpen
          onOpenChange={() => setPosition(null)}
          trigger={<div style={{ position: 'fixed', ...position }} />}
          placement="bottom-start"
          offset={0}
        >
          <MenuItem label="Open"    onSelect={() => openFile(fileName)}   />
          <MenuItem label="Rename"  onSelect={() => renameFile(fileName)} />
          <MenuItem label="Move to" onSelect={() => moveFile(fileName)}   />
          <MenuItem label="Delete"  destructive onSelect={() => deleteFile(fileName)} />
        </Menu>
      )}
    </div>
  );
}
```

### 9. Navigation Drawer (Mobile)

```tsx
import { MenuMobile, MenuSection, MenuItem, MenuFooter } from '@ds/components';
import { useNavigate } from 'react-router-dom';

export function NavDrawer({ isOpen, onClose }: { isOpen: boolean; onClose: () => void }) {
  const navigate = useNavigate();

  const go = (path: string) => { navigate(path); onClose(); };

  return (
    <MenuMobile isOpen={isOpen} onClose={onClose} title="Menu">
      <MenuSection label="Main">
        <MenuItem label="Home"         leadingContent={<HomeIcon />}         onSelect={() => go('/')}         />
        <MenuItem label="Transactions" leadingContent={<TransactionIcon />}  onSelect={() => go('/transactions')} />
        <MenuItem label="Send & Receive" leadingContent={<SendIcon />}       onSelect={() => go('/send')}     />
        <MenuItem label="Wallet"       leadingContent={<WalletIcon />}       onSelect={() => go('/wallet')}   />
      </MenuSection>
      <MenuSection label="Account">
        <MenuItem label="Profile"      leadingContent={<PersonIcon />}       onSelect={() => go('/profile')}  />
        <MenuItem label="Settings"     leadingContent={<GearIcon />}         onSelect={() => go('/settings')} />
      </MenuSection>
      <MenuFooter>
        <MenuItem label="Sign out" destructive onSelect={signOut} />
      </MenuFooter>
    </MenuMobile>
  );
}
```

### 10. Select-Style Menu (Single Selection)

```tsx
import { useState } from 'react';
import { Menu, MenuSection, MenuItem } from '@ds/components';

const SORT_OPTIONS = [
  { value: 'newest',   label: 'Newest first' },
  { value: 'oldest',   label: 'Oldest first' },
  { value: 'amount-hi', label: 'Amount: High to Low' },
  { value: 'amount-lo', label: 'Amount: Low to High' },
];

export function SortMenu() {
  const [sortBy, setSortBy] = useState('newest');
  const current = SORT_OPTIONS.find(o => o.value === sortBy);

  return (
    <Menu
      trigger={<Button variant="secondary" trailingIcon={<ChevronDownIcon />}>
        Sort: {current?.label}
      </Button>}
    >
      <MenuSection label="Sort by">
        {SORT_OPTIONS.map(option => (
          <MenuItem
            key={option.value}
            value={option.value}
            label={option.label}
            selected={sortBy === option.value}
            trailingContent={sortBy === option.value ? <CheckIcon /> : null}
            onSelect={() => setSortBy(option.value)}
          />
        ))}
      </MenuSection>
    </Menu>
  );
}
```

---

## Accessibility Specifications

### ARIA Pattern — `menu` / `menuitem`

The Menu follows the [ARIA Menu Button pattern](https://www.w3.org/WAI/ARIA/apg/patterns/menu-button/):

```html
<!-- Trigger -->
<button
  aria-haspopup="menu"
  aria-expanded="true"
  aria-controls="menu-surface-id"
  id="menu-trigger-id"
>
  Options
</button>

<!-- Menu surface -->
<div
  role="menu"
  id="menu-surface-id"
  aria-labelledby="menu-trigger-id"
  tabindex="-1"
>
  <div role="menuitem" tabindex="-1">Edit</div>
  <div role="menuitem" tabindex="-1">Duplicate</div>
  <div role="menuitem" tabindex="-1" aria-disabled="true">Archive</div>
  <div role="menuitem" tabindex="-1">Delete</div>
</div>
```

For menus used as selection controls (sort, filter), use `role="listbox"` / `role="option"` + `aria-selected` instead of `role="menu"` / `role="menuitem"`.

### ARIA Attributes Summary

| Attribute | Element | Value |
|-----------|---------|-------|
| `role` | Menu surface | `menu` (action list) or `listbox` (selection list) |
| `role` | Menu item | `menuitem`, `menuitemcheckbox`, or `option` |
| `aria-haspopup` | Trigger | `"menu"` |
| `aria-expanded` | Trigger | `"true"` / `"false"` |
| `aria-controls` | Trigger | ID of menu surface |
| `aria-labelledby` | Menu surface | ID of trigger or section heading |
| `aria-disabled` | Disabled item | `"true"` |
| `aria-selected` | Selected item (listbox) | `"true"` / `"false"` |
| `aria-checked` | Checked item (menuitemcheckbox) | `"true"` / `"false"` |
| `aria-label` | Section label | Text of the section (if not using a visible heading) |

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| `Enter` / `Space` | Opens menu from trigger; activates focused item |
| `Escape` | Closes menu and returns focus to trigger |
| `ArrowDown` | Moves focus to the next item; wraps from last to first |
| `ArrowUp` | Moves focus to the previous item; wraps from first to last |
| `Home` | Moves focus to the first non-disabled item |
| `End` | Moves focus to the last non-disabled item |
| `Tab` | Closes menu and moves focus to the next focusable element in the page |
| `A–Z` | Type-ahead: moves focus to the next item whose label starts with the typed character |

For searchable menus:

| Key | Behavior |
|-----|----------|
| Character keys | Types into the search input |
| `ArrowDown` | Moves focus from search input to first item in the list |
| `ArrowUp` | Moves focus from first item back to the search input |
| `Escape` | Clears search query first (if non-empty); second press closes menu |

### Focus Management

**On open:**
- For action menus: focus moves to the first non-disabled menu item
- For searchable menus: focus moves to the search input
- For selection menus with a selected item: focus moves to the currently selected item

**On close:**
- Focus always returns to the trigger element that opened the menu

**Inside the menu:**
- Only one item is in the tab order at a time (roving `tabindex`)
- The focused item has `tabindex="0"`; all others have `tabindex="-1"`

### Screen Reader Announcements

When a menu opens, the screen reader announces: _"[Menu label], menu"_

When a menu item is focused, it announces: _"[Item label][, [description]][, disabled][, selected]"_

When a menu closes after selection, the trigger should reflect the selection (e.g., "Sort: Newest first, button, collapsed").

### Mobile Accessibility

- The bottom sheet uses `role="dialog"` and `aria-modal="true"` to trap assistive technology focus within the sheet
- The sheet title is announced on open via `aria-labelledby`
- The close button has an explicit `aria-label="Close menu"`
- Drag/swipe gestures are supplemented with the close button for pointer-only/switch-access users

---

## Animation Specifications

### Desktop — Open (Scale + Fade In)

```css
@keyframes menu-open {
  from {
    opacity: 0;
    transform: scale(0.95) translateY(-4px);
    transform-origin: top left; /* or top right for bottom-end placement */
  }
  to {
    opacity: 1;
    transform: scale(1) translateY(0);
  }
}

.menu-surface[data-state='open'] {
  animation: menu-open 150ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

### Desktop — Close (Fade Out)

```css
@keyframes menu-close {
  from { opacity: 1; transform: scale(1); }
  to   { opacity: 0; transform: scale(0.97); }
}

.menu-surface[data-state='closing'] {
  animation: menu-close 100ms ease-in;
}
```

### Mobile — Open (Slide Up)

```css
@keyframes sheet-slide-up {
  from { transform: translateY(100%); }
  to   { transform: translateY(0); }
}

.bottom-sheet {
  animation: sheet-slide-up 300ms cubic-bezier(0.32, 0.72, 0, 1);
}
```

### Mobile — Close (Slide Down)

```css
@keyframes sheet-slide-down {
  from { transform: translateY(0); }
  to   { transform: translateY(100%); }
}

.bottom-sheet[data-state='closing'] {
  animation: sheet-slide-down 250ms cubic-bezier(0.4, 0, 0.6, 1);
}
```

### Backdrop

```css
@keyframes backdrop-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

.backdrop {
  background: rgba(0, 0, 0, 0.5);
  animation: backdrop-in 200ms ease;
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  .menu-surface,
  .bottom-sheet,
  .backdrop {
    animation: none;
    transition: none;
  }
}
```

---

## Design Tokens

```css
/* ─── Surface ─────────────────────────────────────────────────────────── */
--menu-bg:                  var(--tokens.colorScheme.background.elevated.modal);  /* white */
--menu-border-radius:       var(--tokens.base.border.radius[12]);                /* 12px */
--menu-shadow-base:         0px 4px 4px 0px var(--tokens.colorScheme.shadow.base);
--menu-shadow-emphasis:     0px 0px 8px 0px var(--tokens.colorScheme.shadow.emphasis);

/* ─── Spacing ─────────────────────────────────────────────────────────── */
--menu-padding-top:         var(--tokens.base.spacing[16]);   /* 16px */
--menu-padding-bottom:      var(--tokens.base.spacing[12]);   /* 12px */
--menu-padding-horizontal:  var(--tokens.base.spacing[16]);   /* 16px */
--menu-item-padding-y:      var(--tokens.base.spacing[12]);   /* 12px */
--menu-item-gap:            var(--tokens.base.spacing[12]);   /* 12px (icon to text) */
--menu-section-label-gap:   var(--tokens.base.spacing[8]);    /* 8px */

/* ─── Typography — Item Label ─────────────────────────────────────────── */
--menu-item-font-family:    var(--tokens.brand.text.fontfamily.body);
--menu-item-font-size:      var(--tokens.base.text.fontsize.ramp[14]);   /* 14px */
--menu-item-line-height:    var(--tokens.base.text.lineheight.ramp[14]); /* 20px */
--menu-item-font-weight:    var(--tokens.brand.text.fontweight.body);

/* ─── Typography — Section Label ──────────────────────────────────────── */
--menu-section-font-family: var(--tokens.brand.text.fontfamily.label);
--menu-section-font-size:   var(--tokens.base.text.fontsize.ramp[12]);   /* 12px */
--menu-section-line-height: var(--tokens.base.text.lineheight.ramp[12]); /* 16px */
--menu-section-font-weight: var(--tokens.brand.text.fontweight.label);   /* Medium */

/* ─── Colors ──────────────────────────────────────────────────────────── */
--menu-item-color:          var(--tokens.colorScheme.content.base);
--menu-item-color-muted:    var(--tokens.colorScheme.content.muted);
--menu-item-color-disabled: var(--tokens.colorScheme.content.disabled);
--menu-item-color-danger:   var(--tokens.colorScheme.content.role.danger);
--menu-section-color:       var(--tokens.colorScheme.content.muted);
--menu-item-bg-hover:       var(--tokens.colorScheme.background.hover);
--menu-item-bg-pressed:     var(--tokens.colorScheme.background.pressed);
--menu-item-bg-selected:    var(--tokens.colorScheme.background.selected);

/* ─── Z-index ─────────────────────────────────────────────────────────── */
--menu-z-index:             1000;
--menu-backdrop-z-index:    999;
```

---

## Best Practices

### Do

- **Use `MenuSection` with a `label` for groups of 3+ related items** — Section labels help users scan a long menu quickly. Unlabeled sections are appropriate only when the grouping is obvious (e.g., a single destructive item at the bottom).
- **Put destructive actions last** — Place "Delete", "Remove", or "Sign out" at the bottom of the menu, separated by a divider or a new section, to prevent accidental selection.
- **Keep menu items short** — Item labels should be 1–4 words. Descriptions can provide additional context but should not be the primary means of explaining an action.
- **Use icons consistently** — Either all items in a section have icons, or none do. Mixing iconless and icon items in the same section creates visual misalignment.
- **Prefer the Searchable variant for lists > 7 items** — If a menu has more than 7 items, consider the searchable variant to reduce scrolling.
- **Return focus to the trigger on close** — This is mandatory for keyboard and AT users to maintain their position in the page.
- **Disable rather than hide unavailable items** — Show disabled items with `aria-disabled="true"` and a tooltip explaining why, rather than hiding them entirely. This preserves discoverability.

### Don't

- **Don't use a Menu as a form** — Menus are for selecting an action or option, not for entering data. Use a Popover or Modal for form interactions.
- **Don't nest menus more than one level deep** — Flyout/submenu patterns are difficult to use on touch devices and with keyboards. Use a secondary menu or a dedicated page instead.
- **Don't exceed ~12 items in an unscrolled menu** — For larger lists, either use the searchable variant or break the list into multiple sections.
- **Don't rely on hover to reveal the trigger** — The trigger that opens the menu must always be visible; hover-only triggers fail touch, keyboard, and AT users.
- **Don't place a Menu inside a form and use `role="menu"`** — If the user is selecting a value to submit, use `role="listbox"` (or a `<select>`) so the semantics convey the correct intent to assistive technology.
- **Don't use a Menu for primary navigation** — Use `<nav>` with links for primary navigation. Menus are for contextual actions and options.
- **Don't forget to close the menu after a selection** — Always call `onClose` / `setIsOpen(false)` after an item `onSelect` fires unless you specifically need `closeOnSelect={false}` for multi-select scenarios.

---

## Menu vs. Other Overlay Components

| Component | When to Use |
|-----------|------------|
| **Menu** | Contextual actions or options for a specific item or trigger |
| **Select / Dropdown** | Form input where the user picks a value from a predefined list |
| **Popover** | Rich content (forms, filters, previews) that requires more space |
| **Modal / Dialog** | Actions requiring confirmation, data entry, or full user attention |
| **Bottom Navigation** | Persistent top-level navigation tabs on mobile |
| **Contextual Alert** | Inline feedback that does not require user interaction to open |
| **Coachtip** | Onboarding guidance overlaid on a specific UI element |

---

## Related Components

| Component | Relationship |
|-----------|-------------|
| **Button / IconButton** | Primary trigger for opening desktop menus |
| **List** | The underlying List component powers MenuItem rendering |
| **Bottom Sheet / Modal** | Infrastructure used by the Mobile Menu variant |
| **Select** | Form-field alternative to Menu when a value is being selected |
| **Popover** | Sibling overlay pattern for richer non-action content |
| **Header** | Often contains a Menu trigger in its End zone (account/settings) |
| **Icon** | Used as `leadingContent` within MenuItem |
| **Chips** | Alternative to a Menu for compact, persistent filter selections |

---

## Version History

| Version | Change |
|---------|--------|
| 6.0 | Unified Desktop + Mobile + Searchable into single `Menu` component family; added `Menu.Footer` and `Menu.SectionLabel` as named sub-components; standardised `MenuSection` prop API |
| 5.3 | Added `searchable` variant for mobile |
| 5.0 | Separated desktop and mobile implementations; added `placement` prop for desktop |
| 4.2 | Added `destructive` prop on `MenuItem`; added multi-section support |
| 4.0 | Initial release |

---

## Testing Checklist

### Visual
- [ ] Desktop menu surface renders at 370 px width with 12 px border radius
- [ ] Desktop menu has correct double-shadow (Elevation/Level 1)
- [ ] Section label renders in 12 px Label/Small font with muted color
- [ ] Item label renders in 14 px Body/Medium font
- [ ] Item description renders in 12 px Body/Small font beneath label
- [ ] Leading icon aligns correctly with the item label text
- [ ] Trailing content aligns to the right edge of the item
- [ ] Disabled items render with muted/reduced-opacity text
- [ ] Destructive items render with danger color
- [ ] Selected item shows checkmark and/or selected background
- [ ] Footer renders correctly with proper padding and separator
- [ ] Mobile bottom sheet has correct top border radius and drag handle
- [ ] Mobile searchable shows search input in Rest and Typing states

### Behavior
- [ ] Menu opens on trigger click
- [ ] Menu closes on `Escape`
- [ ] Menu closes on backdrop click (desktop: click outside; mobile: tap backdrop)
- [ ] Menu closes after item selection (when `closeOnSelect={true}`)
- [ ] `closeOnSelect={false}` keeps menu open after selection
- [ ] Disabled items cannot be selected via click or keyboard
- [ ] Searchable menu filters items in real time as the query changes
- [ ] Empty state renders when no items match search query
- [ ] Clearing search query restores full item list

### Keyboard Navigation
- [ ] `Enter`/`Space` on trigger opens menu and focuses first item
- [ ] `ArrowDown` moves focus through items; wraps at bottom
- [ ] `ArrowUp` moves focus through items; wraps at top
- [ ] `Home` focuses first non-disabled item
- [ ] `End` focuses last non-disabled item
- [ ] Type-ahead moves focus to matching item
- [ ] `Tab` closes menu and moves focus forward in page
- [ ] In searchable menu, `ArrowDown` from input focuses first item
- [ ] In searchable menu, `Escape` clears query first, then closes

### Accessibility
- [ ] Trigger has `aria-haspopup="menu"` and `aria-expanded`
- [ ] Menu surface has `role="menu"` (or `role="listbox"` for select-style)
- [ ] All items have `role="menuitem"` (or `role="option"`)
- [ ] Disabled items have `aria-disabled="true"` (not the HTML `disabled` attribute alone)
- [ ] Selected items have `aria-selected="true"` (listbox) or `aria-checked="true"` (menuitemcheckbox)
- [ ] Section labels are referenced via `aria-labelledby` on the group
- [ ] Mobile bottom sheet has `role="dialog"` and `aria-modal="true"`
- [ ] Focus is trapped within mobile bottom sheet
- [ ] Focus returns to trigger element when menu closes
- [ ] Color contrast of item text meets 4.5:1 minimum
- [ ] Danger-colored items have 4.5:1 contrast against background

### Integration
- [ ] `isOpen` / `onOpenChange` work correctly in controlled mode
- [ ] Uncontrolled mode manages open state internally without external props
- [ ] `placement` positions desktop menu correctly in all four quadrants
- [ ] Menu remains within viewport when near screen edges (flip/collision detection)
- [ ] `onSelect` callback fires with correct event object
- [ ] React Router `navigate()` within `onSelect` works without memory leaks
