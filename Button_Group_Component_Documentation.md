# Button Group Component Documentation

## Overview

The Button Group component organizes multiple related actions into a cohesive control. It supports single and multi-selection modes, icon-based or text-based buttons, and seamless integration with form systems and navigation patterns. Button Groups are essential for presenting related mutually-exclusive or multiple-choice options while maintaining visual hierarchy and interaction clarity.

---

## Component Variants

Button Group supports five primary usage patterns:

### 1. **Single Select Group**
- **Purpose**: Radio-button-style selection where only one option is active at a time
- **Use Cases**: Filter options, view modes (list/grid), sort order selection, priority levels
- **Behavior**: Clicking a button deselects the previously selected button
- **Visual State**: Active button shows selected styling

### 2. **Multi-Select Group**
- **Purpose**: Checkbox-style selection allowing multiple concurrent options
- **Use Cases**: Filter combinations, tag selection, multiple feature toggles, permission sets
- **Behavior**: Each button toggles independently; multiple buttons can be active simultaneously
- **Visual State**: All active buttons maintain selected styling

### 3. **Icon-Only Group**
- **Purpose**: Compact representation using icons without visible labels
- **Use Cases**: Formatting toolbars, view toggles, quick actions, mobile navigation
- **Behavior**: Tooltips reveal button purpose on hover or long-press
- **Visual State**: Icon color/background changes on selection; focus ring required for accessibility

### 4. **Segmented Control**
- **Purpose**: Connected button group with no visual gaps between buttons
- **Use Cases**: Tab-like navigation, mode switching, category selection
- **Behavior**: Single selection; typically used for 2-4 options
- **Visual State**: Active segment highlighted; inactive segments show lower visual weight

### 5. **Toggle Group**
- **Purpose**: Lightweight toggle switches for binary or cycling options
- **Use Cases**: Feature toggles, on/off controls, display settings
- **Behavior**: Toggle state; may cycle through 2+ states in advanced implementations
- **Visual State**: Active state visually distinct; clear affordance for interaction

---

## Size Variants

Button Group respects consistent sizing across the system:

| Size | Height | Padding | Typography | Use Case |
|------|--------|---------|------------|----------|
| **XS** | 24px | 4px-6px | Caption (12px) | Dense interfaces, mobile compact |
| **SM** | 32px | 6px-8px | Body (13px) | Secondary controls, sidebars |
| **MD** | 40px | 8px-12px | Body (14px) | **Default**, most interfaces |
| **LG** | 48px | 12px-16px | Body (16px) | Primary controls, touch-friendly |
| **XL** | 56px | 16px-20px | Subtitle (18px) | Hero sections, marketing sites |

---

## Style Variants

### **Solid/Filled**
- **Description**: High-contrast background with foreground text color
- **Best For**: Primary controls, emphasis on selected state
- **Active State**: Brand color background
- **Inactive State**: Neutral gray background
- **Hover State**: Darker background, elevated shadow
- **Color System**: Primary (Blue), Secondary (Gray), Success (Green), Warning (Amber), Danger (Red)

### **Outlined**
- **Description**: Border-only style with transparent background
- **Best For**: Secondary controls, equal visual weight to filled buttons
- **Active State**: Colored border + filled background
- **Inactive State**: Neutral gray border, transparent background
- **Hover State**: Border color intensifies, subtle background
- **Color System**: Same semantic colors as Solid variant

### **Ghost**
- **Description**: Minimal style; no border or background
- **Best For**: Tertiary controls, dense layouts, toolbar buttons
- **Active State**: Subtle background fill
- **Inactive State**: Text only, transparent background
- **Hover State**: Faint background appears
- **Color System**: Text color indicates semantic meaning

### **Soft**
- **Description**: Subtle colored background without border
- **Best For**: Emphasis without high contrast, modern minimal design
- **Active State**: Saturated background color
- **Inactive State**: Very light background (10-20% opacity)
- **Hover State**: Background opacity increases
- **Color System**: Uses lighter tints of semantic colors

### **Gradient**
- **Description**: Gradient background from one color to another
- **Best For**: Premium/prominent controls, visual interest
- **Active State**: Saturated gradient with full opacity
- **Inactive State**: Desaturated or lighter gradient
- **Hover State**: Gradient intensifies slightly
- **Color System**: Primary-to-secondary gradients, rainbow combinations

---

## Shape Variants

### **Pill** (Default)
- Border radius: 24px or 50% (whichever is larger)
- Creates soft, approachable appearance
- Best for: General-purpose button groups, high-frequency interactions
- Accessibility: Easy touch targets; rounded shape improves perception of clickability

### **Rounded Square**
- Border radius: 8px
- Balances modern appearance with slight edge definition
- Best for: Professional interfaces, data-heavy applications
- Accessibility: Good touch target definition

### **Square**
- Border radius: 0px (no rounding)
- Minimal, utilitarian appearance
- Best for: Technical applications, grids, strict layouts
- Accessibility: Clear rectangular touch targets

---

## State Specifications

### **Default State**
- Color: Gray 500 (#6B7280) or primary brand color
- Background: Neutral gray or transparent (depending on style)
- Cursor: pointer
- Outline: None (visible on focus)
- Shadow: None
- Opacity: 100%

### **Hover State**
- Background: Darkened by 10-15% or enhanced contrast
- Shadow: Elevation 2 (0 2px 4px rgba(0,0,0,0.1))
- Cursor: pointer
- Transform: Subtle scale (1.02) for visual feedback
- Duration: 200ms ease-out

### **Active/Selected State**
- Background: Brand color (Primary Blue #3B82F6)
- Text Color: White (#FFFFFF) or dark text depending on contrast
- Border: Colored border matching background (for outlined style)
- Shadow: Elevation 4 (0 4px 12px rgba(0,0,0,0.15))
- Indicator: Optional checkmark or dot overlay
- Duration: 150ms ease-out

### **Focus State** (Keyboard Navigation)
- Outline: 2px solid brand color
- Outline Offset: 2px
- Duration: 100ms ease-out
- Maintained on click for keyboard users
- Visible on all interactive elements for accessibility

### **Disabled State**
- Background: Gray 200 (#E5E7EB)
- Text Color: Gray 400 (#9CA3AF)
- Cursor: not-allowed
- Opacity: 50-60%
- Pointer Events: none
- No hover or active states

### **Loading State**
- Visual Indicator: Spinner or skeleton animation
- Background: Fades to 70% opacity
- Text: Fades to 70% opacity (or hidden)
- Cursor: wait or loading
- Duration: Continuous rotation (1-2s per rotation)
- Interaction: Disabled until loading completes

---

## Props API

### **ButtonGroup (Container)**

```typescript
interface ButtonGroupProps {
  // Selection Management
  value?: string | string[];                    // Current selected value(s)
  defaultValue?: string | string[];             // Initial selected value
  onChange?: (value: string | string[]) => void; // Selection change handler
  selectionMode?: 'single' | 'multiple';        // Determines selection behavior

  // Appearance
  variant?: 'solid' | 'outlined' | 'ghost' | 'soft' | 'gradient'; // Style variant
  color?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'info';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';     // Size variant
  shape?: 'pill' | 'rounded' | 'square';       // Border radius

  // Layout
  orientation?: 'horizontal' | 'vertical';      // Layout direction
  fullWidth?: boolean;                          // Expand to container width
  segmented?: boolean;                          // Remove gaps between buttons

  // States
  disabled?: boolean;                           // Disable entire group
  loading?: boolean;                            // Show loading state

  // Behavior
  compact?: boolean;                            // Reduce spacing between items
  wrap?: boolean;                               // Allow buttons to wrap
  maxItems?: number;                            // Limit visible items before collapse

  // Accessibility
  aria-label?: string;                          // Group label for screen readers
  aria-describedby?: string;                    // Description for group
  role?: 'group' | 'tablist';                   // Semantic role

  // Event Handlers
  onFocus?: (index: number) => void;            // Focus event
  onBlur?: () => void;                          // Blur event

  // Styling
  className?: string;                           // Custom CSS classes
  style?: React.CSSProperties;                  // Inline styles

  // Children
  children: React.ReactNode;                    // Button Group Items
}
```

### **ButtonGroupItem**

```typescript
interface ButtonGroupItemProps {
  // Content & Value
  value: string;                                // Unique identifier for selection
  label?: string;                               // Display text
  icon?: React.ReactNode;                       // Icon component
  badge?: string | number;                      // Badge content
  badgeColor?: string;                          // Badge color variant

  // States
  selected?: boolean;                           // Override selection state
  disabled?: boolean;                           // Disable individual item
  loading?: boolean;                            // Loading state for this item

  // Appearance
  variant?: 'solid' | 'outlined' | 'ghost';     // Override group variant
  color?: string;                               // Override group color
  size?: string;                                // Override group size

  // Behavior
  href?: string;                                // Link destination
  onClick?: (event: React.MouseEvent) => void;  // Click handler
  type?: 'button' | 'submit' | 'reset';        // HTML button type

  // Accessibility
  aria-label?: string;                          // Accessible label
  aria-describedby?: string;                    // Description
  aria-selected?: boolean;                      // Selection state (auto)
  title?: string;                               // Tooltip text

  // Styling
  className?: string;                           // Custom CSS classes
  style?: React.CSSProperties;                  // Inline styles
}
```

---

## Code Examples

### **1. Single Select Filter Group**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';

export function FilterGroup() {
  const [filter, setFilter] = useState('all');

  const filters = [
    { value: 'all', label: 'All Items' },
    { value: 'active', label: 'Active' },
    { value: 'archived', label: 'Archived' },
  ];

  return (
    <ButtonGroup
      value={filter}
      onChange={setFilter}
      selectionMode="single"
      aria-label="Filter items"
    >
      {filters.map((item) => (
        <ButtonGroupItem
          key={item.value}
          value={item.value}
          label={item.label}
        />
      ))}
    </ButtonGroup>
  );
}
```

### **2. Multi-Select Permissions**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';
import { Eye, Edit, Trash } from 'lucide-react';

export function PermissionToggle() {
  const [permissions, setPermissions] = useState<string[]>(['view']);

  const handleChange = (values: string[]) => {
    setPermissions(values);
  };

  return (
    <ButtonGroup
      value={permissions}
      onChange={handleChange}
      selectionMode="multiple"
      size="sm"
      aria-label="Manage permissions"
    >
      <ButtonGroupItem value="view" icon={<Eye size={16} />} label="View" />
      <ButtonGroupItem value="edit" icon={<Edit size={16} />} label="Edit" />
      <ButtonGroupItem value="delete" icon={<Trash size={16} />} label="Delete" />
    </ButtonGroup>
  );
}
```

### **3. Icon-Only View Toggle**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';
import { LayoutList, Grid3x3 } from 'lucide-react';

export function ViewToggle() {
  const [view, setView] = useState('list');

  return (
    <ButtonGroup
      value={view}
      onChange={setView}
      selectionMode="single"
      variant="ghost"
      size="md"
      aria-label="Toggle view"
    >
      <ButtonGroupItem
        value="list"
        icon={<LayoutList size={20} />}
        aria-label="List view"
      />
      <ButtonGroupItem
        value="grid"
        icon={<Grid3x3 size={20} />}
        aria-label="Grid view"
      />
    </ButtonGroup>
  );
}
```

### **4. Segmented Control (Navigation)**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';

export function TabNavigation() {
  const [tab, setTab] = useState('overview');

  const tabs = [
    { value: 'overview', label: 'Overview' },
    { value: 'details', label: 'Details' },
    { value: 'settings', label: 'Settings' },
  ];

  return (
    <ButtonGroup
      value={tab}
      onChange={setTab}
      selectionMode="single"
      segmented
      variant="outlined"
      role="tablist"
      aria-label="Navigation tabs"
    >
      {tabs.map((t) => (
        <ButtonGroupItem
          key={t.value}
          value={t.value}
          label={t.label}
          role="tab"
          aria-selected={tab === t.value}
          aria-controls={`panel-${t.value}`}
        />
      ))}
    </ButtonGroup>
  );
}
```

### **5. Toolbar with Mixed Icons and Text**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';
import { Bold, Italic, Underline } from 'lucide-react';

export function TextToolbar() {
  const [formatting, setFormatting] = useState<string[]>([]);

  return (
    <ButtonGroup
      value={formatting}
      onChange={setFormatting}
      selectionMode="multiple"
      variant="ghost"
      size="sm"
      compact
      aria-label="Text formatting options"
    >
      <ButtonGroupItem
        value="bold"
        icon={<Bold size={16} />}
        label="Bold"
        title="Bold (Ctrl+B)"
      />
      <ButtonGroupItem
        value="italic"
        icon={<Italic size={16} />}
        label="Italic"
        title="Italic (Ctrl+I)"
      />
      <ButtonGroupItem
        value="underline"
        icon={<Underline size={16} />}
        label="Underline"
        title="Underline (Ctrl+U)"
      />
    </ButtonGroup>
  );
}
```

### **6. Size Selection with Badge**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';

export function SizeSelector() {
  const [size, setSize] = useState('md');

  const sizes = [
    { value: 'sm', label: 'Small' },
    { value: 'md', label: 'Medium' },
    { value: 'lg', label: 'Large', badge: 'Popular' },
  ];

  return (
    <ButtonGroup
      value={size}
      onChange={setSize}
      selectionMode="single"
      size="sm"
      aria-label="Select size"
    >
      {sizes.map((s) => (
        <ButtonGroupItem
          key={s.value}
          value={s.value}
          label={s.label}
          badge={s.badge}
          badgeColor="success"
        />
      ))}
    </ButtonGroup>
  );
}
```

### **7. Vertical Layout with Full Width**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';
import { Radio } from 'lucide-react';

export function VerticalOptions() {
  const [choice, setChoice] = useState('option1');

  return (
    <ButtonGroup
      value={choice}
      onChange={setChoice}
      selectionMode="single"
      orientation="vertical"
      fullWidth
      variant="outlined"
      aria-label="Choose an option"
    >
      <ButtonGroupItem value="option1" label="Option One" />
      <ButtonGroupItem value="option2" label="Option Two" />
      <ButtonGroupItem value="option3" label="Option Three" />
    </ButtonGroup>
  );
}
```

### **8. Loading State**
```typescript
import { ButtonGroup, ButtonGroupItem } from '@components/button-group';
import { useState } from 'react';

export function LoadingExample() {
  const [selected, setSelected] = useState('');
  const [isLoading, setIsLoading] = useState(false);

  const handleChange = async (value: string) => {
    setIsLoading(true);
    // Simulate API call
    await new Promise((resolve) => setTimeout(resolve, 2000));
    setSelected(value);
    setIsLoading(false);
  };

  return (
    <ButtonGroup
      value={selected}
      onChange={handleChange}
      selectionMode="single"
      loading={isLoading}
      disabled={isLoading}
    >
      <ButtonGroupItem value="option1" label="Fast" />
      <ButtonGroupItem value="option2" label="Standard" />
      <ButtonGroupItem value="option3" label="Slow" />
    </ButtonGroup>
  );
}
```

---

## Accessibility Specifications

### **ARIA Attributes**
- **role="group"**: Default for button groups (role="tablist" for tab-like navigation)
- **aria-label**: Required for screen readers to understand group purpose
- **aria-describedby**: Links to additional descriptive text if needed
- **aria-selected**: Automatically managed; indicates current selection state
- **aria-disabled**: Communicated when items are disabled
- **aria-busy="true"**: Applied during loading states

### **Keyboard Navigation**
- **Tab**: Move focus to next button in group (or next focusable element if at end)
- **Shift+Tab**: Move focus to previous button in group
- **Arrow Keys** (Horizontal groups):
  - Right Arrow: Move to next button, select it
  - Left Arrow: Move to previous button, select it
- **Arrow Keys** (Vertical groups):
  - Down Arrow: Move to next button, select it
  - Up Arrow: Move to previous button, select it
- **Space/Enter**: Toggle or activate focused button
- **Home**: Jump to first button
- **End**: Jump to last button

### **Focus Management**
- Focus ring visible on all buttons (2px solid, 2px offset)
- Focus persists through selection for keyboard users
- Focus trap prevented; focus moves to next/previous element when reaching group boundaries
- Tab order follows visual left-to-right (LTR) or right-to-left (RTL) direction

### **Screen Reader Support**
- Group name announced: "Button group: Filter items"
- Individual button labels announced: "All Items, button, not selected"
- Selected state announced: "Active, button, selected"
- Disabled state announced: "Disabled, button, disabled"
- Loading state announced: "Loading, button, disabled"

### **Touch Accessibility**
- Minimum touch target: 44×44px (CSS can be smaller if padding added)
- Adequate spacing between buttons (8px minimum)
- Focus indicators visible on touch focus for devices supporting it
- Long-press tooltips for icon-only variants (2-3 second delay)

### **Color Contrast**
- Text-to-background: 4.5:1 minimum (WCAG AA)
- Active indicator: 3:1 minimum contrast with inactive state
- Icon color to background: 3:1 minimum
- Does not rely on color alone to convey state (always uses shape/pattern change too)

### **Best Practices**
- Always provide aria-label or aria-labelledby for semantic clarity
- Use semantic HTML where possible (buttons not divs)
- Maintain logical tab order matching visual layout
- Provide explicit labels for icon-only buttons (via aria-label)
- Announce loading/disabled states clearly
- Support both single and multi-selection with intuitive behavior

---

## Animation Specifications

### **Selection Animation**
- **Target**: Background color, border color, shadow
- **Duration**: 150-200ms
- **Easing**: ease-out (cubic-bezier(0.4, 0, 0.2, 1))
- **Properties**:
  ```css
  background-color: 150ms ease-out;
  border-color: 150ms ease-out;
  box-shadow: 150ms ease-out;
  ```

### **Hover Animation**
- **Target**: Background, shadow, scale
- **Duration**: 200ms
- **Easing**: ease-out
- **Transform**: scale(1.02)
- **Properties**:
  ```css
  background-color: 200ms ease-out;
  box-shadow: 200ms ease-out;
  transform: 200ms ease-out;
  ```

### **Focus Animation**
- **Target**: Outline, outline-offset
- **Duration**: 100ms
- **Easing**: ease-out
- **Properties**:
  ```css
  outline: 100ms ease-out;
  outline-offset: 100ms ease-out;
  ```

### **Loading Animation**
- **Target**: Spinner rotation
- **Duration**: 1.5s per rotation
- **Easing**: linear
- **Keyframes**: 0% rotate(0deg) → 100% rotate(360deg)
- **Repeat**: Infinite until state changes
- **Properties**:
  ```css
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  animation: spin 1.5s linear infinite;
  ```

### **Expansion Animation** (Vertical/Wrap)
- **Target**: Max-height, opacity
- **Duration**: 200ms
- **Easing**: ease-out
- **Used when**: Group expands to show hidden items
- **Properties**:
  ```css
  max-height: 200ms ease-out;
  opacity: 200ms ease-out;
  ```

---

## Design Tokens

### **Colors**
| Purpose | Token | Value |
|---------|-------|-------|
| Primary | `--color-primary` | #3B82F6 (Blue) |
| Secondary | `--color-secondary` | #6B7280 (Gray) |
| Success | `--color-success` | #10B981 (Green) |
| Warning | `--color-warning` | #F59E0B (Amber) |
| Danger | `--color-danger` | #EF4444 (Red) |
| Info | `--color-info` | #06B6D4 (Cyan) |
| Background Default | `--color-bg-default` | #FFFFFF |
| Background Secondary | `--color-bg-secondary` | #F3F4F6 |
| Text Primary | `--color-text-primary` | #1F2937 |
| Text Secondary | `--color-text-secondary` | #6B7280 |
| Border | `--color-border` | #D1D5DB |

### **Typography**
| Size | Font Size | Line Height | Letter Spacing |
|------|-----------|-------------|----------------|
| **Caption** | 12px | 16px | 0.5px |
| **Body** | 14px | 20px | 0.25px |
| **Subtitle** | 16px | 24px | 0.15px |
| **Title** | 18px | 28px | 0px |

### **Spacing**
| Size | Value | Use Case |
|------|-------|----------|
| **XS** | 4px | Tight spacing between elements |
| **SM** | 8px | Default inter-element spacing |
| **MD** | 12px | Comfortable spacing |
| **LG** | 16px | Section separation |
| **XL** | 24px | Major section separation |

### **Borders**
| Weight | Value | Use Case |
|--------|-------|----------|
| **Thin** | 1px | Default button borders |
| **Medium** | 2px | Focus rings, emphasis |
| **Bold** | 3px | Heavy emphasis, active states |

### **Shadows**
| Level | Value | Use Case |
|-------|-------|----------|
| **Elevation 0** | none | Flat appearance |
| **Elevation 2** | 0 2px 4px rgba(0,0,0,0.1) | Subtle lift, hover state |
| **Elevation 4** | 0 4px 12px rgba(0,0,0,0.15) | Active, selected state |
| **Elevation 8** | 0 8px 24px rgba(0,0,0,0.12) | Floating, dropdown state |

### **Border Radius**
| Shape | Value | Use Case |
|-------|-------|----------|
| **Square** | 0px | Utilitarian designs |
| **Rounded** | 8px | Professional modern UI |
| **Pill** | 24px | Soft, approachable UI |
| **Full** | 50% | Circular elements |

### **Z-Index Scale**
| Level | Value | Use Case |
|-------|-------|----------|
| **Base** | auto | Button elements |
| **Elevated** | 10 | Hover/active states |
| **Floating** | 50 | Floating actions, tooltips |

---

## Best Practices

### **Visual Design**
- Maintain consistent spacing between buttons (8px default)
- Use segmented control style only for 2-4 options (max 5)
- Keep button labels short and scannable (1-3 words)
- Use icons + text for clarity when space permits
- Show loading states with clear visual feedback (spinner + opacity change)

### **Interaction Patterns**
- Single-select groups: Radio-button behavior (one active at a time)
- Multi-select groups: Checkbox behavior (independent toggles)
- Segmented controls: Tab-like navigation between exclusive options
- Icon-only buttons: Always include title attribute or tooltip
- Disabled buttons: Gray out and prevent all interactions

### **Accessibility**
- Always provide group-level aria-label for context
- Use semantic button elements (not divs)
- Support keyboard navigation fully (arrows, home, end keys)
- Maintain 4.5:1 text contrast minimum
- Test with screen readers (NVDA, JAWS, VoiceOver)
- Ensure focus rings visible and 2px minimum thickness

### **Mobile Considerations**
- Increase touch targets to 44×44px minimum
- Add adequate spacing (8px+) between buttons for finger interaction
- Icon-only groups: Use larger icons (20px+) on mobile
- Vertical layout for more than 3 buttons on small screens
- Consider wrapping behavior for responsive design
- Test on devices with notches and safe areas

### **Performance**
- Debounce onChange handlers for rapid selections
- Render large button lists virtualized (50+ items)
- Use CSS transitions (not JS animations) for better performance
- Lazy-load button content if needed
- Monitor re-render cycles in multi-select scenarios

### **Usability**
- Group related buttons logically
- Maintain consistent selection behavior across application
- Use color meaningfully (don't rely on color alone)
- Provide clear feedback for state changes
- Consider context when choosing horizontal vs. vertical layout
- Test mental models with users before shipping

---

## Component Structure

```
ButtonGroup
├── Container (handles selection logic)
├── ButtonGroupItem 1
│   ├── Icon (optional)
│   ├── Label (optional)
│   └── Badge (optional)
├── ButtonGroupItem 2
│   ├── Icon
│   ├── Label
│   └── Badge
└── ButtonGroupItem N
```

---

## Related Components

- **Button**: Individual button component; use when standalone action needed
- **Segmented Control**: Specialized button group for 2-4 exclusive options
- **Toggle Button**: Individual toggle; use for simple on/off states
- **Tabs**: Tab navigation component; similar to button group with panels
- **Select/Dropdown**: When option list exceeds 4-5 items
- **Checkbox/Radio**: When full vertical list of options preferred

---

## Migration Guide

### **From Standard Buttons to Button Group**
```typescript
// Before: Multiple individual buttons
<Button onClick={() => setView('list')}>List</Button>
<Button onClick={() => setView('grid')}>Grid</Button>

// After: Button group with unified state management
<ButtonGroup value={view} onChange={setView} selectionMode="single">
  <ButtonGroupItem value="list" label="List" />
  <ButtonGroupItem value="grid" label="Grid" />
</ButtonGroup>
```

### **From Tabs to Segmented Control**
```typescript
// Button Group can replace Tabs for simple scenarios
<ButtonGroup
  value={tab}
  onChange={setTab}
  segmented
  role="tablist"
>
  <ButtonGroupItem value="overview" label="Overview" />
  <ButtonGroupItem value="details" label="Details" />
</ButtonGroup>
```

---

## Version History

- **v1.0**: Initial release with single/multi-select, five style variants, comprehensive keyboard support
- **v1.1**: Added segmented control variant, loading states, badge support
- **v1.2**: Improved mobile touch targets, RTL support, vertical layout
- **v2.0**: Component refactor for better accessibility, improved animation performance

---

## Changelog

### **Latest Updates**
- Enhanced keyboard navigation with Home/End key support
- Improved focus management for complex multi-select scenarios
- Added tooltip support for icon-only variants
- Optimized performance for large button groups (50+ items)
- Better RTL language support

---

## FAQ

**Q: Should I use Button Group or Select dropdown?**
A: Use Button Group for 2-5 options with high visibility needed. Use Select dropdown for 6+ options to save space.

**Q: Can I change selection mode after initial render?**
A: Not recommended. Keep selectionMode consistent throughout component lifecycle.

**Q: How do I validate Button Group in forms?**
A: Wrap in form component and validate onChange callbacks. Apply form-level error states.

**Q: What's the difference between ghost and outlined variants?**
A: Ghost has no border (text only), Outlined has border. Use Outlined for equal visual weight with filled buttons.

**Q: Can I use Button Group for navigation between pages?**
A: Yes, pass href prop to items. Combine with router Link components for SPA navigation.

---

## Examples by Use Case

### **Filtering Data**
- Single-select group with filter options
- Blue highlight for active filter
- Show counts in badges for filtered results

### **View Mode Toggle**
- Icon-only horizontal group
- List/Grid/Table view options
- Compact size (SM) to save space

### **Permissions Management**
- Multi-select vertical group
- Read/Write/Delete options
- Show current role highlighted

### **Format Toolbar**
- Multi-select horizontal group
- Bold/Italic/Underline options
- Ghost variant for minimal appearance

### **Form Field Selection**
- Single-select group for radio-like behavior
- Use aria-describedby to link to form field labels
- Segmented control for 2-3 options

---

## Testing Checklist

- [ ] Single-select mode works correctly (only one active at a time)
- [ ] Multi-select mode allows multiple concurrent selections
- [ ] Keyboard navigation (Tab, Arrows, Home, End) works
- [ ] Focus rings visible and properly styled
- [ ] Loading state shows spinner and disables interaction
- [ ] Disabled state prevents selection changes
- [ ] Screen reader announces group purpose and individual states
- [ ] Touch targets meet 44×44px minimum on mobile
- [ ] Animations smooth and performant (60fps)
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Works in RTL languages
- [ ] Form integration works (value submission)
- [ ] Event handlers fire correctly (onChange)
- [ ] Badge content displays correctly
- [ ] Icon rendering works with various icon libraries
