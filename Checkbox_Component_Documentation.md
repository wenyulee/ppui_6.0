# Checkbox Component Documentation

## Overview

The Checkbox component is a binary or tri-state form control used for selecting one or multiple options from a set. Unlike radio buttons, checkboxes operate independently — each one can be checked or unchecked regardless of the others. Checkboxes support indeterminate states for parent-child hierarchies, rich labeling with descriptions, and deep integration with form validation libraries. They are a fundamental input element across settings panels, filter UIs, data tables, and multi-step forms.

---

## Component Variants

Checkbox supports five primary usage patterns:

### 1. **Single Checkbox**
- **Purpose**: Standalone binary control for a single on/off choice
- **Use Cases**: Terms acceptance, newsletter opt-in, feature toggles, notification settings
- **Behavior**: Toggles between checked and unchecked on click
- **Visual**: Box with checkmark icon; label to the right by default

### 2. **Checkbox Group**
- **Purpose**: Multiple related checkboxes for multi-option selection
- **Use Cases**: Filter panels, permissions lists, preference settings, tag selection
- **Behavior**: Each checkbox toggles independently; any combination is valid
- **Visual**: Vertically stacked or horizontally arranged list with consistent spacing

### 3. **Indeterminate Checkbox** (Tri-state)
- **Purpose**: Parent checkbox reflecting partial selection of children
- **Use Cases**: "Select all" rows in tables, parent category with child sub-items, bulk operations
- **Behavior**: Three states — unchecked, indeterminate (dash icon), checked
- **Indeterminate Logic**: Set programmatically when some (not all) children are selected
- **Visual**: Box with horizontal dash icon (−) when indeterminate

### 4. **Checkbox with Description**
- **Purpose**: Checkbox with supporting text below the label
- **Use Cases**: Settings with explanation, feature toggles with detail, permissions with context
- **Behavior**: Standard toggle; entire label+description area clickable
- **Visual**: Label on first line, lighter description text below

### 5. **Card Checkbox**
- **Purpose**: Checkbox embedded in a selectable card for richer visual selection
- **Use Cases**: Plan selection, feature choice, product variants, onboarding preferences
- **Behavior**: Clicking card area toggles checkbox; border highlights on selection
- **Visual**: Card with checkbox in top-left or top-right; highlighted border when checked

---

## Size Variants

| Size | Box Size | Label Size | Gap | Use Case |
|------|----------|------------|-----|----------|
| **XS** | 14×14px | 12px | 6px | Dense tables, compact sidebars |
| **SM** | 16×16px | 13px | 8px | Secondary forms, filter lists |
| **MD** | 18×18px | 14px | 10px | **Default**, general forms |
| **LG** | 20×20px | 16px | 12px | Prominent settings, touch UIs |
| **XL** | 24×24px | 18px | 14px | Mobile-first, accessibility-focused |

---

## Style Variants

### **Filled** (Default)
- **Checked Background**: Brand color (#3B82F6)
- **Checked Icon**: White checkmark
- **Unchecked Background**: White
- **Unchecked Border**: Gray (#D1D5DB)
- **Hover Background**: Light brand tint (#EBF8FF)
- **Best For**: Standard form use across the application

### **Outlined**
- **Checked Background**: White or very light tint
- **Checked Icon**: Brand-colored checkmark
- **Checked Border**: Brand color (2px)
- **Unchecked Background**: White
- **Unchecked Border**: Gray (#D1D5DB)
- **Best For**: Lighter visual treatment; content-heavy forms

### **Ghost**
- **Checked Background**: Transparent
- **Checked Icon**: Brand-colored checkmark; bold weight
- **Unchecked Background**: Transparent
- **Border**: Subtle or none
- **Best For**: Minimal UI, toolbar contexts, dark backgrounds

### **Soft**
- **Checked Background**: Light brand tint (10-20% opacity)
- **Checked Icon**: Brand-colored checkmark
- **Checked Border**: Brand color (1px)
- **Best For**: Modern minimal design; emphasis without high contrast

---

## Color Variants

| Color | Checked Background | Use Case |
|-------|--------------------|----------|
| **Primary** | #3B82F6 (Blue) | Default selection |
| **Secondary** | #6B7280 (Gray) | Neutral secondary selection |
| **Success** | #10B981 (Green) | Positive agreement, confirmation |
| **Warning** | #F59E0B (Amber) | Cautionary acceptance |
| **Danger** | #EF4444 (Red) | Destructive or risky selection |
| **Info** | #06B6D4 (Cyan) | Informational grouping |

---

## State Specifications

### **Unchecked State**
- Box background: White (#FFFFFF)
- Box border: 1.5px solid Gray (#D1D5DB)
- Icon: None visible
- Label color: Gray 900 (#111827)
- Cursor: pointer
- Opacity: 100%

### **Checked State**
- Box background: Brand color (#3B82F6)
- Box border: 1.5px solid brand color
- Icon: White checkmark (SVG), centered
- Label color: Gray 900 (#111827)
- Cursor: pointer
- Opacity: 100%

### **Indeterminate State**
- Box background: Brand color (#3B82F6)
- Box border: 1.5px solid brand color
- Icon: White horizontal dash (−), centered
- Label color: Gray 900 (#111827)
- Cursor: pointer
- Used when: Some (not all) children in group are selected

### **Hover State**
- Box border: Brand color (unchecked) or darker brand (checked)
- Background: Light brand tint (#EBF8FF) for unchecked
- Shadow: 0 0 0 3px rgba(59, 130, 246, 0.1) — subtle glow ring
- Cursor: pointer
- Duration: 150ms ease-out

### **Focus State** (Keyboard)
- Outline: 2px solid brand color
- Outline Offset: 2px
- Duration: 100ms ease-out
- Ring: 0 0 0 4px rgba(59, 130, 246, 0.2)
- Visible on Tab navigation only (not on mouse click)

### **Disabled Unchecked**
- Box background: Gray 100 (#F3F4F6)
- Box border: 1.5px solid Gray 300 (#D1D5DB)
- Icon: None
- Label color: Gray 400 (#9CA3AF)
- Cursor: not-allowed
- Opacity: 60%
- Pointer Events: none

### **Disabled Checked**
- Box background: Gray 300 (#D1D5DB)
- Box border: 1.5px solid Gray 300
- Icon: Gray 500 checkmark
- Label color: Gray 400 (#9CA3AF)
- Cursor: not-allowed
- Opacity: 60%

### **Error State**
- Box border: 2px solid Danger color (#EF4444)
- Box background: Light error tint (#FEF2F2) when unchecked
- Label color: Danger color (#EF4444)
- Error message: Below label in red (12px)
- Cursor: pointer

### **Read-Only State**
- Box background: Gray 50 (#F9FAFB)
- Box border: 1.5px solid Gray 200
- Icon: Gray checkmark (if checked)
- Label color: Gray 700
- Cursor: default
- Interaction: None

---

## Props API

### **Checkbox**

```typescript
interface CheckboxProps {
  // Value & State
  checked?: boolean;                           // Controlled checked state
  defaultChecked?: boolean;                    // Uncontrolled initial state
  indeterminate?: boolean;                     // Indeterminate (partial) state
  value?: string;                              // Form value when checked

  // Labels & Description
  label?: string | React.ReactNode;            // Primary label
  description?: string;                        // Supporting description text
  hint?: string;                               // Hint text (below label)

  // Appearance
  variant?: 'filled' | 'outlined' | 'ghost' | 'soft'; // Style variant
  color?: 'primary' | 'secondary' | 'success' | 'warning' | 'danger' | 'info';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';   // Size variant
  labelPlacement?: 'right' | 'left' | 'top' | 'bottom'; // Label position

  // States
  disabled?: boolean;                          // Disable interaction
  readOnly?: boolean;                          // Read-only display
  required?: boolean;                          // Required field (adds asterisk)
  error?: boolean;                             // Error state
  errorMessage?: string;                       // Error message text

  // Form Integration
  name?: string;                               // Input name for form submission
  id?: string;                                 // HTML ID

  // Handlers
  onChange?: (checked: boolean, event: React.ChangeEvent<HTMLInputElement>) => void;
  onFocus?: (event: React.FocusEvent) => void;
  onBlur?: (event: React.FocusEvent) => void;

  // Accessibility
  aria-label?: string;                         // Accessible label (if no visible label)
  aria-describedby?: string;                   // Description reference
  aria-labelledby?: string;                    // Label element reference

  // Styling
  className?: string;                          // Custom CSS classes
  style?: React.CSSProperties;                 // Inline styles
}
```

### **CheckboxGroup**

```typescript
interface CheckboxGroupProps {
  // Value Management
  value?: string[];                            // Controlled selected values
  defaultValue?: string[];                     // Uncontrolled initial values
  onChange?: (values: string[]) => void;       // Group change handler

  // Layout
  orientation?: 'vertical' | 'horizontal';    // Stack direction
  columns?: number;                            // Grid columns (horizontal)
  gap?: 'sm' | 'md' | 'lg';                   // Spacing between items

  // Group-wide Settings
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  color?: string;
  variant?: string;
  disabled?: boolean;
  required?: boolean;
  error?: boolean;
  errorMessage?: string;

  // Labels
  label?: string;                              // Group label (fieldset legend)
  description?: string;                        // Group description

  // Accessibility
  aria-label?: string;
  aria-describedby?: string;

  // Children
  children: React.ReactNode;

  // Styling
  className?: string;
  style?: React.CSSProperties;
}
```

---

## Code Examples

### **1. Basic Single Checkbox**
```typescript
import { Checkbox } from '@components/checkbox';
import { useState } from 'react';

export function TermsCheckbox() {
  const [accepted, setAccepted] = useState(false);

  return (
    <Checkbox
      checked={accepted}
      onChange={(checked) => setAccepted(checked)}
      label="I agree to the Terms of Service and Privacy Policy"
      required
    />
  );
}
```

### **2. Checkbox Group (Filters)**
```typescript
import { CheckboxGroup, Checkbox } from '@components/checkbox';
import { useState } from 'react';

export function FilterPanel() {
  const [filters, setFilters] = useState<string[]>([]);

  return (
    <CheckboxGroup
      label="Category"
      value={filters}
      onChange={setFilters}
      orientation="vertical"
    >
      <Checkbox value="electronics" label="Electronics" />
      <Checkbox value="clothing" label="Clothing" />
      <Checkbox value="books" label="Books" />
      <Checkbox value="home" label="Home & Garden" />
      <Checkbox value="sports" label="Sports & Outdoors" />
    </CheckboxGroup>
  );
}
```

### **3. Indeterminate "Select All"**
```typescript
import { Checkbox } from '@components/checkbox';
import { useState } from 'react';

const items = [
  { id: '1', label: 'Item One' },
  { id: '2', label: 'Item Two' },
  { id: '3', label: 'Item Three' },
];

export function SelectAllExample() {
  const [selected, setSelected] = useState<string[]>([]);

  const allSelected = selected.length === items.length;
  const someSelected = selected.length > 0 && !allSelected;

  const handleSelectAll = (checked: boolean) => {
    setSelected(checked ? items.map((i) => i.id) : []);
  };

  const handleItem = (id: string, checked: boolean) => {
    setSelected((prev) =>
      checked ? [...prev, id] : prev.filter((v) => v !== id)
    );
  };

  return (
    <div className="space-y-2">
      <Checkbox
        checked={allSelected}
        indeterminate={someSelected}
        onChange={handleSelectAll}
        label="Select All"
      />
      <div className="ml-6 space-y-2">
        {items.map((item) => (
          <Checkbox
            key={item.id}
            value={item.id}
            checked={selected.includes(item.id)}
            onChange={(checked) => handleItem(item.id, checked)}
            label={item.label}
          />
        ))}
      </div>
    </div>
  );
}
```

### **4. Checkbox with Description**
```typescript
import { Checkbox } from '@components/checkbox';
import { useState } from 'react';

export function SettingsCheckboxes() {
  const [settings, setSettings] = useState({
    marketing: false,
    analytics: true,
    notifications: false,
  });

  return (
    <div className="space-y-4">
      <Checkbox
        checked={settings.marketing}
        onChange={(checked) =>
          setSettings((s) => ({ ...s, marketing: checked }))
        }
        label="Marketing Emails"
        description="Receive updates about new products, promotions, and news from our team."
      />
      <Checkbox
        checked={settings.analytics}
        onChange={(checked) =>
          setSettings((s) => ({ ...s, analytics: checked }))
        }
        label="Usage Analytics"
        description="Help us improve by sharing anonymous usage data. No personal information collected."
      />
      <Checkbox
        checked={settings.notifications}
        onChange={(checked) =>
          setSettings((s) => ({ ...s, notifications: checked }))
        }
        label="Push Notifications"
        description="Get notified in real-time about activity relevant to you."
      />
    </div>
  );
}
```

### **5. Card Checkbox (Plan Selection)**
```typescript
import { Checkbox } from '@components/checkbox';
import { useState } from 'react';

const plans = [
  {
    id: 'starter',
    label: 'Starter',
    description: '5 projects, 2GB storage',
    price: 'Free',
  },
  {
    id: 'pro',
    label: 'Pro',
    description: 'Unlimited projects, 50GB storage',
    price: '$12/mo',
  },
  {
    id: 'enterprise',
    label: 'Enterprise',
    description: 'Everything in Pro + SSO, custom contracts',
    price: 'Custom',
  },
];

export function PlanSelector() {
  const [selected, setSelected] = useState('starter');

  return (
    <div className="grid grid-cols-3 gap-4">
      {plans.map((plan) => (
        <label
          key={plan.id}
          className={`
            relative flex flex-col gap-2 p-4 border-2 rounded-lg cursor-pointer transition
            ${selected === plan.id
              ? 'border-blue-500 bg-blue-50'
              : 'border-gray-200 hover:border-gray-300'}
          `}
        >
          <div className="flex items-center justify-between">
            <span className="font-semibold">{plan.label}</span>
            <Checkbox
              checked={selected === plan.id}
              onChange={() => setSelected(plan.id)}
              aria-label={`Select ${plan.label} plan`}
            />
          </div>
          <p className="text-sm text-gray-500">{plan.description}</p>
          <span className="font-bold text-blue-600">{plan.price}</span>
        </label>
      ))}
    </div>
  );
}
```

### **6. Error State Validation**
```typescript
import { Checkbox } from '@components/checkbox';
import { useState } from 'react';

export function RequiredCheckbox() {
  const [accepted, setAccepted] = useState(false);
  const [submitted, setSubmitted] = useState(false);

  const hasError = submitted && !accepted;

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    setSubmitted(true);
    if (accepted) {
      console.log('Form submitted');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <Checkbox
        checked={accepted}
        onChange={(checked) => setAccepted(checked)}
        label="I accept the terms and conditions"
        error={hasError}
        errorMessage="You must accept the terms to continue."
        required
      />
      <button type="submit" className="mt-4 btn btn-primary">
        Submit
      </button>
    </form>
  );
}
```

### **7. Horizontal Checkbox Group**
```typescript
import { CheckboxGroup, Checkbox } from '@components/checkbox';
import { useState } from 'react';

export function DaysOfWeek() {
  const [days, setDays] = useState<string[]>(['mon', 'wed', 'fri']);

  return (
    <CheckboxGroup
      label="Repeat on"
      value={days}
      onChange={setDays}
      orientation="horizontal"
      size="sm"
    >
      {['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'].map((day) => (
        <Checkbox
          key={day.toLowerCase()}
          value={day.toLowerCase()}
          label={day}
        />
      ))}
    </CheckboxGroup>
  );
}
```

### **8. Disabled and Read-Only States**
```typescript
import { Checkbox } from '@components/checkbox';

export function DisabledReadOnly() {
  return (
    <div className="space-y-3">
      <Checkbox label="Enabled, unchecked" />
      <Checkbox label="Enabled, checked" defaultChecked />
      <Checkbox label="Disabled, unchecked" disabled />
      <Checkbox label="Disabled, checked" disabled defaultChecked />
      <Checkbox label="Read-only, unchecked" readOnly />
      <Checkbox label="Read-only, checked" readOnly defaultChecked />
    </div>
  );
}
```

---

## Accessibility Specifications

### **HTML Semantics**
- Uses native `<input type="checkbox">` element for maximum compatibility
- Wrapped in `<label>` element to expand click target to entire label area
- Checkbox groups use `<fieldset>` + `<legend>` for semantic grouping
- Indeterminate state set via `inputRef.current.indeterminate = true` (JS)

### **ARIA Attributes**
- **aria-checked**: `"true"` | `"false"` | `"mixed"` (for indeterminate)
- **aria-disabled**: `"true"` when disabled
- **aria-required**: `"true"` when field is required
- **aria-invalid**: `"true"` when in error state
- **aria-describedby**: Links to description, hint, or error message
- **aria-labelledby**: Links to external label element
- **role="checkbox"**: Applied when using non-native element (avoid when possible)

### **Keyboard Navigation**
- **Tab**: Move focus to checkbox
- **Shift+Tab**: Move focus to previous element
- **Space**: Toggle checked state
- **Enter**: Does not toggle (browser default; space is the standard)
- Within groups: Tab moves between checkboxes sequentially

### **Focus Management**
- Focus ring visible on Tab navigation: 2px solid brand, 2px offset
- Focus ring suppressed on mouse click (`:focus-visible` selector)
- Indeterminate state visually distinct from checked/unchecked
- Focus moves to next focusable element after last checkbox in group

### **Screen Reader Announcements**
- Unchecked: "Label, checkbox, unchecked"
- Checked: "Label, checkbox, checked"
- Indeterminate: "Label, checkbox, mixed"
- Disabled: "Label, checkbox, unchecked, dimmed" (NVDA/JAWS wording varies)
- Required: "Label, required, checkbox, unchecked"
- Error: Error message read after label: "Label, checkbox, unchecked. Error: You must accept the terms."
- Group legend announced before items: "Category, group. Electronics, checkbox, unchecked."

### **Touch Accessibility**
- Minimum touch target: 44×44px (via padding on label element)
- Adequate spacing between checkboxes (8px minimum)
- Entire label area clickable/tappable (not just box)
- Touch tap state uses same visual feedback as hover

### **Color Contrast**
- Box border to background: 3:1 minimum
- Checkmark to box background: 4.5:1 minimum (white on brand blue ✓)
- Label text to background: 4.5:1 minimum
- Error text to background: 4.5:1 minimum
- Does not rely on color alone — checkmark icon communicates state

### **Best Practices**
- Always provide a visible label (never checkbox without label)
- Use `<fieldset>` + `<legend>` for groups for screen reader context
- Mark required checkboxes with asterisk and aria-required
- Provide clear error messages directly adjacent to the checkbox
- Ensure indeterminate state is only used in parent/child hierarchies
- Test with NVDA, JAWS, and VoiceOver

---

## Animation Specifications

### **Check Animation**
- **Target**: Checkmark SVG path drawing
- **Duration**: 150ms
- **Easing**: ease-out (cubic-bezier(0.4, 0, 0.2, 1))
- **Effect**: Stroke-dasharray animation (draws checkmark from top-left to bottom-right)
- **Properties**:
  ```css
  stroke-dashoffset: 100% → 0%;
  transition: stroke-dashoffset 150ms ease-out;
  ```

### **Background Fill Animation**
- **Target**: Box background color
- **Duration**: 150ms
- **Easing**: ease-out
- **Properties**:
  ```css
  background-color: 150ms ease-out;
  border-color: 150ms ease-out;
  ```

### **Hover Ring Animation**
- **Target**: Box shadow (focus ring)
- **Duration**: 150ms
- **Easing**: ease-out
- **Properties**:
  ```css
  box-shadow: 150ms ease-out;
  ```

### **Focus Animation**
- **Target**: Outline, ring
- **Duration**: 100ms
- **Easing**: ease-out
- **Properties**:
  ```css
  outline: 100ms ease-out;
  box-shadow: 100ms ease-out;
  ```

### **Indeterminate Swap Animation**
- **Target**: Icon change from dash to checkmark (or vice versa)
- **Duration**: 100-150ms
- **Easing**: ease-out
- **Properties**: Icon opacity + transform (scale or fade)

---

## Design Tokens

### **Colors**
| Purpose | Token | Value |
|---------|-------|-------|
| Checked Fill | `--color-checkbox-checked` | #3B82F6 |
| Unchecked Border | `--color-checkbox-border` | #D1D5DB |
| Hover Border | `--color-checkbox-hover` | #93C5FD |
| Focus Ring | `--color-checkbox-focus` | rgba(59,130,246,0.2) |
| Checkmark Icon | `--color-checkbox-icon` | #FFFFFF |
| Disabled Fill | `--color-checkbox-disabled` | #D1D5DB |
| Error Border | `--color-checkbox-error` | #EF4444 |
| Label Default | `--color-label-default` | #111827 |
| Label Disabled | `--color-label-disabled` | #9CA3AF |
| Description | `--color-label-description` | #6B7280 |
| Error Message | `--color-error-message` | #EF4444 |

### **Sizing**
| Size | Box | Border Radius | Label Font | Gap |
|------|-----|---------------|------------|-----|
| XS | 14×14px | 3px | 12px | 6px |
| SM | 16×16px | 4px | 13px | 8px |
| MD | 18×18px | 4px | 14px | 10px |
| LG | 20×20px | 5px | 16px | 12px |
| XL | 24×24px | 6px | 18px | 14px |

### **Borders**
| Weight | Value | Use Case |
|--------|-------|----------|
| Default | 1.5px | Unchecked state |
| Checked | 1.5px | Checked state (same weight, different color) |
| Focus | 2px | Focus outline |
| Error | 2px | Error state border |

### **Shadows**
| State | Value |
|-------|-------|
| Hover | 0 0 0 3px rgba(59,130,246,0.1) |
| Focus | 0 0 0 4px rgba(59,130,246,0.2) |
| Error Focus | 0 0 0 4px rgba(239,68,68,0.2) |

---

## Best Practices

### **Labeling**
- Always show a visible, descriptive label
- Keep labels concise — lead with the key information
- Use sentence case for labels (not ALL CAPS or Title Case Every Word)
- Place label to the right by default for LTR layouts
- Left-align label text; don't center
- Use description text for complex settings that need extra context

### **Group Usage**
- Wrap related checkboxes in `<fieldset>` + `<legend>` or CheckboxGroup
- Use group label to explain what's being selected
- Sort options logically (alphabetical, by frequency, or by hierarchy)
- Allow a "Select All" checkbox for groups with 4+ items
- Limit horizontal groups to 4 or fewer items

### **Error Handling**
- Show error message immediately adjacent to the checkbox
- Use red border AND error message (never color alone)
- Link error message to checkbox via `aria-describedby`
- Clear error immediately when user checks the box
- Use specific error messages: "You must agree to continue" not "Required"

### **Form Integration**
- Use `name` and `value` props for native form submission
- Support React Hook Form, Formik, and HTML form `FormData`
- Reset checkbox state on form reset events
- Pre-fill checked state from server-side data where possible

### **Indeterminate Usage**
- Only use indeterminate in parent-child selection hierarchies
- Never leave indeterminate state as permanent; it should resolve to checked/unchecked on user action
- Clearly explain the parent-child relationship in UI
- "Select All" becomes checked when all children are selected, unchecked when none, indeterminate otherwise

### **Mobile Considerations**
- Touch targets must be at least 44×44px
- Entire label area should be tappable
- Increase spacing between checkboxes on mobile (12px minimum)
- Consider larger size variant (LG or XL) for mobile forms
- Avoid horizontal groups on small screens (switch to vertical)

---

## Component Structure

```
CheckboxGroup (optional)
└── fieldset
    ├── legend (group label)
    └── Checkbox (×N)
        ├── label (wraps entire component)
        │   ├── input[type="checkbox"] (native, visually hidden)
        │   ├── CheckboxBox (visual box + icon)
        │   │   └── CheckmarkIcon or DashIcon
        │   └── LabelContent
        │       ├── LabelText
        │       └── Description (optional)
        └── ErrorMessage (optional)
```

---

## Form Library Integration

### **React Hook Form**
```typescript
import { useForm, Controller } from 'react-hook-form';
import { Checkbox } from '@components/checkbox';

export function HookFormExample() {
  const { control, handleSubmit } = useForm({
    defaultValues: { terms: false },
  });

  return (
    <form onSubmit={handleSubmit(console.log)}>
      <Controller
        name="terms"
        control={control}
        rules={{ required: 'You must accept the terms' }}
        render={({ field, fieldState }) => (
          <Checkbox
            checked={field.value}
            onChange={field.onChange}
            label="Accept Terms"
            error={!!fieldState.error}
            errorMessage={fieldState.error?.message}
          />
        )}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### **Formik**
```typescript
import { useFormik } from 'formik';
import { Checkbox } from '@components/checkbox';

export function FormikExample() {
  const formik = useFormik({
    initialValues: { terms: false },
    onSubmit: (values) => console.log(values),
    validate: (values) => {
      const errors: { terms?: string } = {};
      if (!values.terms) errors.terms = 'You must accept the terms';
      return errors;
    },
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <Checkbox
        name="terms"
        checked={formik.values.terms}
        onChange={(checked) => formik.setFieldValue('terms', checked)}
        label="Accept Terms"
        error={formik.touched.terms && !!formik.errors.terms}
        errorMessage={formik.touched.terms ? formik.errors.terms : undefined}
      />
    </form>
  );
}
```

---

## Related Components

- **Radio Button**: Use when only one option can be selected (mutually exclusive)
- **Toggle Switch**: Use for binary on/off settings with immediate effect
- **Select / Dropdown**: Use for selecting from a long list (6+ options)
- **Button Group**: Use for 2-5 mutually exclusive quick selections
- **Combobox**: Use for searching and selecting from a large options set
- **Data Table**: Integrates checkboxes for row selection and bulk actions

---

## Version History

- **v1.0**: Initial release with basic checked/unchecked, label support
- **v1.1**: Added indeterminate state, CheckboxGroup component
- **v1.2**: Added description prop, error state, form library integration
- **v2.0**: Refactor with design tokens, improved animations, full ARIA support
- **v2.1**: Added Card Checkbox variant, color variants, RTL support
- **v2.2**: Enhanced focus management, improved keyboard navigation in groups

---

## Testing Checklist

- [ ] Unchecked state renders correctly
- [ ] Checked state renders with checkmark icon
- [ ] Indeterminate state renders with dash icon
- [ ] Toggle works on label click (not just box)
- [ ] Toggle works on Space key press
- [ ] Tab navigation moves focus to checkbox
- [ ] Focus ring visible on keyboard focus
- [ ] Focus ring suppressed on mouse click
- [ ] Disabled state prevents interaction
- [ ] Read-only state prevents interaction
- [ ] Error state shows red border + error message
- [ ] Required asterisk shows with required prop
- [ ] Group legend announced by screen reader
- [ ] aria-checked="mixed" for indeterminate state
- [ ] aria-describedby links to description/error
- [ ] Touch targets meet 44×44px minimum
- [ ] Entire label area is clickable
- [ ] CheckboxGroup onChange receives correct array of values
- [ ] Select All / indeterminate logic correct
- [ ] React Hook Form and Formik integration work
- [ ] Color contrast meets WCAG AA
- [ ] Check animation smooth (150ms)
- [ ] Works RTL
