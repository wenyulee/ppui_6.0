# Chips Component Documentation

## Overview

The Chip component is a compact, interactive element used to represent discrete pieces of information, actions, or selections. Chips are ideal for tags, filter toggles, multi-value input fields, and contextual actions. They communicate attributes or selections in a scannable, visually grouped format. Unlike buttons, chips typically represent a value or state rather than a standalone action, and can be dismissible, selectable, or purely informational. Chips work both in isolation and in coordinated groups.

---

## Component Variants

Chips support five primary usage patterns:

### 1. **Input Chip**
- **Purpose**: Represents a discrete value entered or selected by the user; typically appears inside a text input field after selection
- **Use Cases**: Email recipients, tag input fields, multi-select autocomplete, assignee lists
- **Behavior**: Created on Enter or selection from dropdown; removable via × button or Backspace key
- **Visual**: Label with optional avatar/icon on left, dismiss (×) on right

### 2. **Filter Chip**
- **Purpose**: Toggle between a selected/deselected filter state; works like a checkbox but compact
- **Use Cases**: Category filtering, attribute toggles, search refinement, faceted navigation
- **Behavior**: Toggles on/off on click; supports single (radio-like) or multi-select modes
- **Visual**: Label with optional leading icon; filled/highlighted when active

### 3. **Suggestion Chip**
- **Purpose**: Presents pre-defined suggestions or quick-action shortcuts for the user to accept
- **Use Cases**: Smart reply options, topic tags, related searches, onboarding prompts
- **Behavior**: Single tap selects/applies; typically not persistent after selection
- **Visual**: Outlined or ghost style; flat appearance; no dismiss button

### 4. **Assist Chip**
- **Purpose**: Triggers a specific action or navigates to a destination
- **Use Cases**: Quick actions in chat, contextual shortcuts, workflow prompts
- **Behavior**: Activates an operation when clicked (not a toggle); may show loading state
- **Visual**: Outlined chip with optional leading icon; behaves more like a compact button

### 5. **Status/Tag Chip**
- **Purpose**: Read-only label conveying status, category, or metadata
- **Use Cases**: Status badges in tables, category tags on articles, metadata labels on profiles
- **Behavior**: Non-interactive by default; optional click to filter by tag
- **Visual**: Compact, colored, may include dot or icon; no dismiss affordance

---

## Size Variants

| Size | Height | Padding | Label Size | Icon Size | Use Case |
|------|--------|---------|------------|-----------|----------|
| **XS** | 20px | 4px–8px | 11px | 12px | Dense tables, inline metadata |
| **SM** | 24px | 4px–10px | 12px | 14px | Secondary contexts, toolbars |
| **MD** | 32px | 6px–12px | 14px | 16px | **Default**, filter bars, tag inputs |
| **LG** | 40px | 8px–16px | 16px | 18px | Prominent filters, touch-first UIs |
| **XL** | 48px | 12px–20px | 18px | 20px | Mobile hero, onboarding selections |

---

## Style Variants

### **Filled**
- **Active Background**: Brand color (#3B82F6) or semantic color
- **Active Text**: White (#FFFFFF)
- **Inactive Background**: Gray 100 (#F3F4F6)
- **Inactive Text**: Gray 700 (#374151)
- **Best For**: Filter chips, selected input chips

### **Outlined**
- **Active Border**: Brand color (2px); background: light brand tint
- **Active Text**: Brand color
- **Inactive Border**: Gray 300 (#D1D5DB); background: White
- **Inactive Text**: Gray 700
- **Best For**: Suggestion chips, secondary filter options

### **Ghost**
- **Active Background**: Light brand tint (10% opacity)
- **Active Text**: Brand color
- **Inactive Background**: Transparent
- **Inactive Text**: Gray 600
- **Best For**: Lightweight, minimal surfaces; suggestion chips

### **Soft**
- **Active Background**: Brand color at 15% opacity
- **Active Text**: Brand color (dark variant)
- **Inactive Background**: Gray at 8% opacity
- **Inactive Text**: Gray 600
- **Best For**: Modern minimal design; filter chips in sidebars

### **Elevated**
- **Background**: White with drop shadow
- **Active Shadow**: Elevation 4 with colored glow
- **Inactive Shadow**: Elevation 2
- **Best For**: Floating chip trays, suggestion surfaces

---

## Color Variants

| Color | Active Fill | Active Text | Use Case |
|-------|------------|------------|---------|
| **Primary** | #3B82F6 | #FFFFFF | Default selection |
| **Secondary** | #6B7280 | #FFFFFF | Neutral grouping |
| **Success** | #10B981 | #FFFFFF | Positive states, confirmed tags |
| **Warning** | #F59E0B | #FFFFFF | Caution, pending items |
| **Danger** | #EF4444 | #FFFFFF | Errors, removals, alerts |
| **Info** | #06B6D4 | #FFFFFF | Informational labels |
| **Purple** | #8B5CF6 | #FFFFFF | Category-specific theming |
| **Pink** | #EC4899 | #FFFFFF | Category-specific theming |

---

## State Specifications

### **Default / Inactive State**
- Background: Gray 100 (#F3F4F6)
- Border: None (or 1px Gray 200 for outlined)
- Text: Gray 700 (#374151)
- Icon: Gray 500
- Cursor: pointer (if interactive) / default (if static)
- Opacity: 100%

### **Hover State**
- Background: Gray 200 (#E5E7EB) or light brand tint
- Border: Brand color (outlined variant)
- Shadow: Elevation 1 (0 1px 2px rgba(0,0,0,0.08))
- Cursor: pointer
- Duration: 150ms ease-out

### **Active / Selected State**
- Background: Brand color (#3B82F6)
- Text: White (#FFFFFF)
- Icon: White
- Border: None or brand color
- Shadow: Elevation 2
- Leading checkmark: Optional (filter chips)
- Duration: 150ms ease-out

### **Focus State** (Keyboard)
- Outline: 2px solid brand color
- Outline Offset: 2px
- Ring: 0 0 0 4px rgba(59,130,246,0.2)
- Duration: 100ms ease-out

### **Pressed/Active State**
- Background: Darker brand (hover + 10%)
- Transform: scale(0.97)
- Duration: 100ms ease-in

### **Disabled State**
- Background: Gray 100 (#F3F4F6)
- Text: Gray 400 (#9CA3AF)
- Cursor: not-allowed
- Opacity: 50%
- Pointer Events: none
- No hover or active feedback

### **Loading State**
- Background: Fades to 70% opacity
- Spinner: Small (12–14px) aligned with label
- Cursor: wait
- Interaction: Disabled

### **Deletable/Dismissible State**
- Shows × (close) icon on right edge
- × icon hover: Background darkens slightly behind icon
- × icon click: Triggers `onDelete` callback
- × icon size: Smaller than chip height (12–14px)
- Keyboard: Delete or Backspace on focused chip triggers `onDelete`

---

## Props API

### **Chip**

```typescript
interface ChipProps {
  // Content
  label: string;                               // Primary display text
  icon?: React.ReactNode;                      // Leading icon
  avatar?: React.ReactNode;                    // Leading avatar component
  trailingIcon?: React.ReactNode;              // Trailing icon (non-dismiss)

  // Type & Selection
  variant?: 'filled' | 'outlined' | 'ghost' | 'soft' | 'elevated';
  type?: 'input' | 'filter' | 'suggestion' | 'assist' | 'status';
  color?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'info';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  shape?: 'pill' | 'rounded' | 'square';

  // Selection (Filter/Toggle)
  selected?: boolean;                          // Controlled selected state
  defaultSelected?: boolean;                   // Uncontrolled initial selected
  onSelect?: (selected: boolean) => void;      // Toggle handler

  // Dismissible (Input Chips)
  deletable?: boolean;                         // Show delete/dismiss button
  onDelete?: () => void;                       // Delete/dismiss handler
  deleteIcon?: React.ReactNode;                // Custom delete icon
  deleteAriaLabel?: string;                    // Accessible label for delete

  // States
  disabled?: boolean;
  loading?: boolean;
  active?: boolean;                            // Force active visual state

  // Interaction
  onClick?: (event: React.MouseEvent) => void; // Click handler
  href?: string;                               // Navigate on click
  as?: 'div' | 'button' | 'a' | 'span';       // HTML element

  // Accessibility
  aria-label?: string;
  aria-describedby?: string;
  aria-selected?: boolean;
  aria-pressed?: boolean;
  role?: string;

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

### **ChipGroup**

```typescript
interface ChipGroupProps {
  // Selection
  value?: string | string[];                   // Controlled selected values
  defaultValue?: string | string[];            // Initial selected values
  onChange?: (value: string | string[]) => void;
  selectionMode?: 'single' | 'multiple' | 'none';

  // Layout
  wrap?: boolean;                              // Allow chips to wrap lines
  gap?: 'xs' | 'sm' | 'md' | 'lg';            // Spacing between chips
  maxItems?: number;                           // Truncate with "+N" overflow chip
  scrollable?: boolean;                        // Horizontal scroll container

  // Group-wide Defaults
  size?: ChipProps['size'];
  variant?: ChipProps['variant'];
  color?: ChipProps['color'];
  disabled?: boolean;

  // Accessibility
  aria-label?: string;
  role?: 'listbox' | 'group';

  // Styling
  className?: string;
  style?: React.CSSProperties;

  // Children
  children: React.ReactNode;
}
```

---

## Code Examples

### **1. Filter Chips (Multi-Select)**
```typescript
import { ChipGroup, Chip } from '@components/chip';
import { useState } from 'react';

const categories = ['Design', 'Engineering', 'Marketing', 'Sales', 'HR'];

export function CategoryFilter() {
  const [selected, setSelected] = useState<string[]>([]);

  return (
    <ChipGroup
      value={selected}
      onChange={(v) => setSelected(v as string[])}
      selectionMode="multiple"
      wrap
      aria-label="Filter by category"
    >
      {categories.map((cat) => (
        <Chip
          key={cat}
          value={cat}
          label={cat}
          type="filter"
          variant="outlined"
        />
      ))}
    </ChipGroup>
  );
}
```

### **2. Input Chips (Tag Input)**
```typescript
import { Chip, ChipGroup } from '@components/chip';
import { useState, KeyboardEvent } from 'react';

export function TagInput() {
  const [tags, setTags] = useState<string[]>(['React', 'TypeScript']);
  const [inputValue, setInputValue] = useState('');

  const addTag = () => {
    const trimmed = inputValue.trim();
    if (trimmed && !tags.includes(trimmed)) {
      setTags((prev) => [...prev, trimmed]);
    }
    setInputValue('');
  };

  const removeTag = (tag: string) => {
    setTags((prev) => prev.filter((t) => t !== tag));
  };

  const handleKeyDown = (e: KeyboardEvent<HTMLInputElement>) => {
    if (e.key === 'Enter') addTag();
    if (e.key === 'Backspace' && !inputValue && tags.length > 0) {
      removeTag(tags[tags.length - 1]);
    }
  };

  return (
    <div className="flex flex-wrap items-center gap-2 p-2 border rounded-lg">
      <ChipGroup selectionMode="none" wrap gap="xs">
        {tags.map((tag) => (
          <Chip
            key={tag}
            label={tag}
            type="input"
            deletable
            onDelete={() => removeTag(tag)}
            deleteAriaLabel={`Remove ${tag}`}
          />
        ))}
      </ChipGroup>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        onKeyDown={handleKeyDown}
        placeholder="Add tag…"
        className="flex-1 outline-none text-sm"
        aria-label="Add a tag"
      />
    </div>
  );
}
```

### **3. Single-Select Filter Chips**
```typescript
import { ChipGroup, Chip } from '@components/chip';
import { useState } from 'react';

const views = [
  { value: 'all', label: 'All' },
  { value: 'active', label: 'Active' },
  { value: 'draft', label: 'Draft' },
  { value: 'archived', label: 'Archived' },
];

export function StatusFilter() {
  const [status, setStatus] = useState('all');

  return (
    <ChipGroup
      value={status}
      onChange={(v) => setStatus(v as string)}
      selectionMode="single"
      role="listbox"
      aria-label="Filter by status"
    >
      {views.map((v) => (
        <Chip
          key={v.value}
          value={v.value}
          label={v.label}
          type="filter"
        />
      ))}
    </ChipGroup>
  );
}
```

### **4. Suggestion Chips (Smart Reply)**
```typescript
import { Chip } from '@components/chip';
import { useState } from 'react';

const suggestions = [
  'Sounds good!',
  'Can we reschedule?',
  'I'll follow up later.',
  'Thanks for the update!',
];

export function SmartReply({ onSelect }: { onSelect: (text: string) => void }) {
  return (
    <div className="flex flex-wrap gap-2">
      {suggestions.map((s) => (
        <Chip
          key={s}
          label={s}
          type="suggestion"
          variant="outlined"
          onClick={() => onSelect(s)}
        />
      ))}
    </div>
  );
}
```

### **5. Chips with Icons and Avatars**
```typescript
import { ChipGroup, Chip } from '@components/chip';
import { MapPin, Tag, User } from 'lucide-react';
import { Avatar } from '@components/avatar';
import { useState } from 'react';

export function RichChips() {
  const [selected, setSelected] = useState<string[]>([]);

  return (
    <ChipGroup
      value={selected}
      onChange={(v) => setSelected(v as string[])}
      selectionMode="multiple"
      wrap
    >
      <Chip
        value="nyc"
        label="New York"
        icon={<MapPin size={14} />}
        type="filter"
      />
      <Chip
        value="design"
        label="Design"
        icon={<Tag size={14} />}
        color="info"
        type="filter"
      />
      <Chip
        value="sarah"
        label="Sarah Johnson"
        avatar={<Avatar src="/avatars/sarah.jpg" alt="Sarah" size="xs" />}
        type="input"
        deletable
        onDelete={() => {}}
      />
    </ChipGroup>
  );
}
```

### **6. Assist Chips with Actions**
```typescript
import { Chip } from '@components/chip';
import { useState } from 'react';
import { Calendar, Copy, Share2 } from 'lucide-react';

export function AssistChips() {
  const [copying, setCopying] = useState(false);

  const handleCopy = async () => {
    setCopying(true);
    await navigator.clipboard.writeText('https://example.com/shared-link');
    setTimeout(() => setCopying(false), 1500);
  };

  return (
    <div className="flex flex-wrap gap-2">
      <Chip
        label="Add to Calendar"
        icon={<Calendar size={14} />}
        type="assist"
        variant="outlined"
        onClick={() => {}}
      />
      <Chip
        label={copying ? 'Copied!' : 'Copy Link'}
        icon={<Copy size={14} />}
        type="assist"
        variant="outlined"
        loading={copying}
        onClick={handleCopy}
      />
      <Chip
        label="Share"
        icon={<Share2 size={14} />}
        type="assist"
        variant="outlined"
        onClick={() => {}}
      />
    </div>
  );
}
```

### **7. Status/Tag Chips (Read-Only)**
```typescript
import { Chip } from '@components/chip';

export function StatusChips() {
  return (
    <div className="flex flex-wrap gap-2">
      <Chip label="Active" color="success" type="status" variant="soft" />
      <Chip label="Pending" color="warning" type="status" variant="soft" />
      <Chip label="Rejected" color="danger" type="status" variant="soft" />
      <Chip label="Draft" color="secondary" type="status" variant="soft" />
      <Chip label="Archived" color="info" type="status" variant="soft" />
    </div>
  );
}
```

### **8. Overflow Truncation with "+N" Chip**
```typescript
import { ChipGroup, Chip } from '@components/chip';
import { useState } from 'react';

const allTags = ['React', 'TypeScript', 'Node', 'GraphQL', 'Docker', 'AWS', 'Kubernetes'];

export function TruncatedChips() {
  const [expanded, setExpanded] = useState(false);
  const maxVisible = 3;
  const visible = expanded ? allTags : allTags.slice(0, maxVisible);
  const overflow = allTags.length - maxVisible;

  return (
    <div className="flex flex-wrap items-center gap-2">
      <ChipGroup selectionMode="none" wrap>
        {visible.map((tag) => (
          <Chip key={tag} label={tag} type="status" variant="soft" />
        ))}
      </ChipGroup>
      {!expanded && overflow > 0 && (
        <Chip
          label={`+${overflow} more`}
          type="assist"
          variant="outlined"
          size="sm"
          onClick={() => setExpanded(true)}
          aria-label={`Show ${overflow} more tags`}
        />
      )}
    </div>
  );
}
```

---

## Accessibility Specifications

### **HTML Semantics**
- Interactive chips: `<button>` element (dismissible, filter, assist)
- Navigation chips: `<a>` element with `href`
- Read-only status chips: `<span>` element
- Input chips inside a text field: Group within a `role="listbox"` container
- ChipGroup: `<ul>` with each Chip inside `<li>` for list context

### **ARIA Attributes**
- **role="option"**: Chips inside a `role="listbox"` group (filter/input contexts)
- **role="button"**: Interactive chips that aren't `<button>` elements
- **aria-selected**: `"true"` | `"false"` for filter chips
- **aria-pressed**: `"true"` | `"false"` for toggle chips
- **aria-disabled**: `"true"` when chip is disabled
- **aria-label**: Required for icon-only or avatar-only chips
- **aria-describedby**: Links to additional context or description
- **aria-live="polite"**: Announced on chip addition or removal in input chip contexts

### **Keyboard Navigation**
- **Tab**: Move focus into chip group (or onto standalone chip)
- **Shift+Tab**: Move focus to previous element
- **Enter / Space**: Activate chip (toggle, select, or trigger action)
- **Delete / Backspace**: Remove deletable chip when focused
- **Arrow Keys** (within a group):
  - Right/Down Arrow: Move focus to next chip
  - Left/Up Arrow: Move focus to previous chip
  - Home: Jump to first chip
  - End: Jump to last chip

### **Screen Reader Announcements**
- Filter chip selected: "Design, button, selected"
- Filter chip deselected: "Design, button, not selected"
- Chip added (input): "React added. 3 tags total."
- Chip removed: "React removed. 2 tags remaining."
- Disabled chip: "Design, button, dimmed"
- Status chip (read-only): "Active, status"
- Overflow chip: "Show 4 more tags, button"

### **Touch Accessibility**
- Minimum touch target: 44×44px (via padding; visual size may be smaller)
- Dismiss button has its own 24×24px+ tap target, separate from chip label
- Adequate spacing between chips in groups (6–8px minimum)
- Long-press tooltip for icon-only chips where supported

### **Color Contrast**
- Text on filled chips: 4.5:1 minimum (white on #3B82F6 = 3.1:1 — supplement with other indicators)
- Text on soft/ghost chips: 4.5:1 minimum
- Border to background (outlined): 3:1 minimum
- Never rely on color alone to convey state — combine with icon or label change

### **Best Practices**
- Always use `<button>` for interactive chips (never `<div>` with onClick)
- Provide aria-label for icon-only chips
- Announce chip additions/removals to screen readers in input chip context
- Keep chip labels short (1–3 words); use tooltip for longer text
- Provide keyboard access to delete button separate from chip body

---

## Animation Specifications

### **Selection Animation**
- **Target**: Background color, border color, text color
- **Duration**: 150ms
- **Easing**: ease-out
  ```css
  background-color: 150ms ease-out;
  border-color: 150ms ease-out;
  color: 150ms ease-out;
  ```

### **Hover Animation**
- **Target**: Background, shadow, border
- **Duration**: 150ms
- **Easing**: ease-out
  ```css
  background-color: 150ms ease-out;
  box-shadow: 150ms ease-out;
  ```

### **Enter Animation** (Input Chip added)
- **Target**: Opacity + scale
- **Duration**: 200ms
- **Easing**: ease-out (spring-like)
  ```css
  @keyframes chipEnter {
    from { opacity: 0; transform: scale(0.8); }
    to   { opacity: 1; transform: scale(1); }
  }
  animation: chipEnter 200ms ease-out;
  ```

### **Exit Animation** (Input Chip removed)
- **Target**: Opacity + scale + width
- **Duration**: 150ms
- **Easing**: ease-in
  ```css
  @keyframes chipExit {
    from { opacity: 1; transform: scale(1); max-width: 200px; }
    to   { opacity: 0; transform: scale(0.8); max-width: 0; padding: 0; }
  }
  animation: chipExit 150ms ease-in forwards;
  ```

### **Checkmark Reveal** (Filter chip selected)
- **Target**: Checkmark icon opacity + translateX
- **Duration**: 150ms
- **Easing**: ease-out
  ```css
  opacity: 0 → 1;
  transform: translateX(-4px) → translateX(0);
  ```

### **Press Animation**
- **Target**: scale
- **Duration**: 100ms
- **Easing**: ease-in
  ```css
  transform: scale(0.97);
  ```

---

## Design Tokens

### **Colors**
| Purpose | Token | Value |
|---------|-------|-------|
| Default Background | `--chip-bg-default` | #F3F4F6 |
| Default Text | `--chip-text-default` | #374151 |
| Default Border | `--chip-border-default` | #D1D5DB |
| Active Background | `--chip-bg-active` | #3B82F6 |
| Active Text | `--chip-text-active` | #FFFFFF |
| Hover Background | `--chip-bg-hover` | #E5E7EB |
| Disabled Background | `--chip-bg-disabled` | #F3F4F6 |
| Disabled Text | `--chip-text-disabled` | #9CA3AF |
| Delete Icon | `--chip-delete-icon` | #6B7280 |
| Delete Hover | `--chip-delete-hover` | #374151 |

### **Shape & Size**
| Property | Value |
|----------|-------|
| Border Radius (pill) | 9999px |
| Border Radius (rounded) | 8px |
| Border Radius (square) | 4px |
| Border Width | 1px (default), 2px (active/focus) |
| Focus Ring | 2px solid #3B82F6, offset 2px |

### **Spacing**
| Size | Height | Horizontal Padding | Gap Between Chips |
|------|--------|-------------------|-------------------|
| XS | 20px | 8px | 4px |
| SM | 24px | 10px | 6px |
| MD | 32px | 12px | 8px |
| LG | 40px | 16px | 8px |
| XL | 48px | 20px | 10px |

---

## Best Practices

### **Content Guidelines**
- Labels should be 1–3 words maximum
- Sentence case for filter and status chips ("Active" not "ACTIVE")
- Use nouns for tags/status; verbs for action/assist chips ("Add to calendar")
- Don't use chips for single-option controls — use a checkbox or toggle instead
- Provide a tooltip via `title` prop for truncated chip labels

### **Chip Type Selection**
| Scenario | Recommended Type |
|----------|-----------------|
| Filtering a list | Filter Chip |
| Adding/removing tags | Input Chip |
| Smart reply / prompt | Suggestion Chip |
| Quick actions | Assist Chip |
| Read-only labels | Status/Tag Chip |

### **Groups**
- Limit visible chips to 5–8 before truncating with "+N more"
- Allow wrapping for filter groups; horizontal scroll for input chip trays
- Single-select filter groups behave like a segmented control (consider using that instead for 2–4 options)
- Show a clear affordance for "all deselected" state (e.g., default neutral chip style)

### **Deletable Chips**
- Dismiss icon (×) must have a separate, accessible tap/click target
- Provide `deleteAriaLabel` explicitly: "Remove React tag" not just "Remove"
- Announce removal to screen readers via `aria-live` region
- Allow Backspace key to remove the last chip when input is empty

### **Mobile Considerations**
- MD size minimum on mobile for comfortable tap interactions
- Wrap chips rather than scroll when possible for discoverability
- Increase gap between chips to 8–10px on small screens
- Avoid icon-only chips on mobile without tooltip fallback

---

## Component Structure

```
ChipGroup
└── ul[role="listbox" | "group"]
    └── li (×N)
        └── Chip
            ├── button | span | a
            │   ├── LeadingIcon / Avatar (optional)
            │   ├── CheckmarkIcon (filter chip, selected)
            │   ├── Label
            │   └── TrailingIcon (optional)
            └── DeleteButton (deletable chips only)
                └── ×  icon
```

---

## Related Components

- **Badge**: Static, non-interactive count or status indicator
- **Tag**: Simpler read-only version of Status Chip
- **Button Group**: For 2–5 exclusive action selections
- **Checkbox**: For multi-select in a form context with full labels
- **Select / Combobox**: When list of options is too long for chips (6+)
- **Autocomplete**: Often paired with Input Chips for multi-select search

---

## Version History

- **v1.0**: Initial release — input and filter chips with filled/outlined variants
- **v1.1**: Added suggestion and assist chip types; group component
- **v1.2**: Color variants, icon/avatar support, deletable chips
- **v2.0**: Refactor with design tokens; enter/exit animations; full a11y audit
- **v2.1**: Overflow truncation, scrollable group mode, RTL support
- **v2.2**: Loading state, keyboard navigation improvements, press animation

---

## Testing Checklist

- [ ] Default inactive state renders correctly
- [ ] Selected/active state shows correct colors and icon
- [ ] Hover state appears with correct background change
- [ ] Focus ring visible on keyboard Tab focus
- [ ] Enter/Space toggles filter chip
- [ ] Delete button triggers `onDelete` callback
- [ ] Backspace removes last chip in input chip context
- [ ] Arrow keys navigate between chips in group
- [ ] Disabled chips cannot be interacted with
- [ ] `aria-selected` reflects selection state correctly
- [ ] Screen reader announces chip addition/removal
- [ ] Touch targets meet 44×44px minimum
- [ ] Chips wrap correctly in wrapping group
- [ ] Overflow "+N more" chip renders and expands correctly
- [ ] Enter/exit animations smooth at 60fps
- [ ] All color variants display correct contrast
- [ ] Icon-only chips have aria-label
- [ ] Deletable chip's × has its own accessible label
- [ ] RTL layout correct
- [ ] Works across NVDA, JAWS, and VoiceOver
