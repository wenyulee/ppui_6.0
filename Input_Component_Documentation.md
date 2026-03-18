# Input Component Documentation

## Overview

The Input component is the primary text entry element across forms, search bars, filters, and data-entry interfaces. It wraps the native `<input>` element with a consistent visual treatment, comprehensive state management, and rich slot support for icons, prefixes, suffixes, clear buttons, and character counts. A well-designed input reduces form abandonment, communicates validation state clearly, and guides users to correct errors efficiently. The Input component works standalone or as a building block inside more complex components like Autocomplete, Combobox, DatePicker, and PhoneInput.

---

## Component Variants

Input supports six primary usage patterns:

### 1. **Text Input**
- **Purpose**: Single-line free text entry
- **Use Cases**: Name, email, username, URL, search query, short answers
- **Types**: `text`, `email`, `url`, `tel`, `search`, `password`
- **Behavior**: Single line; no line breaks; optional character limit

### 2. **Textarea**
- **Purpose**: Multi-line free text entry for longer content
- **Use Cases**: Comments, descriptions, bio, message body, notes
- **Types**: `textarea` element; `rows` prop controls initial height
- **Behavior**: Vertically resizable by default; optional auto-resize to content height

### 3. **Number Input**
- **Purpose**: Numeric entry with optional stepper controls
- **Use Cases**: Quantity, price, age, percentage, dimension values
- **Types**: `number` with `min`, `max`, `step`
- **Behavior**: Increment/decrement arrows; keyboard Up/Down; optional prefix/suffix ($ / %)

### 4. **Password Input**
- **Purpose**: Masked text entry for credentials
- **Use Cases**: Login password, PIN, security codes, confirmation fields
- **Types**: `password` with reveal toggle
- **Behavior**: Characters masked; reveal-toggle button unmasks; optional strength meter

### 5. **Search Input**
- **Purpose**: Specialized input for search queries with clear affordance
- **Use Cases**: Site search, table filter, list filter, inline search
- **Types**: `search` with magnifier icon and clear (×) button
- **Behavior**: Clear button appears when non-empty; submit on Enter; optional loading state

### 6. **Input Group** (Addons)
- **Purpose**: Input with attached prefix or suffix elements — text, icons, selects, or buttons
- **Use Cases**: Currency input ($ prefix), domain input (.com suffix), phone with country code, URL with protocol
- **Behavior**: Addon is visually attached to the input; single cohesive control
- **Variants**: Text addon, icon addon, button addon, select addon

---

## Size Variants

| Size | Height | Padding H | Font | Icon | Use Case |
|------|--------|-----------|------|------|---------|
| **XS** | 28px | 8px | 12px | 14px | Dense tables, inline editing |
| **SM** | 32px | 10px | 13px | 14px | Compact forms, sidebars |
| **MD** | 40px | 12px | 14px | 16px | **Default**, standard forms |
| **LG** | 48px | 14px | 16px | 18px | Prominent search, hero inputs |
| **XL** | 56px | 16px | 18px | 20px | Landing page, large breakpoints |

---

## Style Variants

### **Outlined** (Default)
- Border: 1px solid #D1D5DB
- Background: White (#FFFFFF)
- Focus: 2px brand ring
- Best For: Standard forms, high-contrast surfaces

### **Filled**
- Background: Gray 100 (#F3F4F6)
- Border: None (only bottom border or none)
- Focus: Bottom border brand color or full ring
- Best For: Dense forms, search bars, Material-style UIs

### **Flushed / Underline**
- Border: Bottom only (1px #D1D5DB)
- Background: Transparent
- Focus: Bottom border thickens to 2px brand
- Best For: Minimal UIs, inline editing, form sections with clear headings

### **Unstyled**
- No border, no background
- Full style control via `className`
- Best For: Custom-styled wrappers, rich text editors, composable patterns

---

## State Specifications

### **Default State**
- Border: 1px solid #D1D5DB
- Background: White (#FFFFFF)
- Text: Gray 900 (#111827)
- Placeholder: Gray 400 (#9CA3AF)
- Cursor: text

### **Hover State**
- Border: 1px solid #9CA3AF (darker)
- Duration: 150ms ease-out

### **Focus State**
- Border: 1px solid Brand (#3B82F6)
- Ring: 0 0 0 3px rgba(59,130,246,0.2)
- Background: White
- Duration: 100ms ease-out

### **Filled / Has Value**
- Border: 1px solid #D1D5DB (same as default; value is in the input)
- Text: Gray 900
- Placeholder: Hidden

### **Disabled State**
- Background: Gray 100 (#F3F4F6)
- Border: 1px solid #E5E7EB
- Text: Gray 400 (#9CA3AF)
- Cursor: not-allowed
- Opacity: 60%
- Pointer Events: none

### **Read-Only State**
- Background: Gray 50 (#F9FAFB)
- Border: 1px solid #E5E7EB
- Text: Gray 700
- Cursor: default
- Not editable; selectable/copyable

### **Error State**
- Border: 1px solid Danger (#EF4444)
- Ring: 0 0 0 3px rgba(239,68,68,0.15) on focus
- Icon: Error icon inside input (optional)
- Error message: Below input in red (#EF4444), 12px
- Label: Optionally colored red

### **Warning State**
- Border: 1px solid Warning (#F59E0B)
- Ring: 0 0 0 3px rgba(245,158,11,0.15) on focus
- Warning message: Below input in amber

### **Success State**
- Border: 1px solid Success (#10B981)
- Ring: 0 0 0 3px rgba(16,185,129,0.15) on focus
- Check icon: Inside input trailing slot
- Helper: "Looks good!" or field-specific success message

### **Loading State**
- Spinner: 14–16px spinner in trailing slot
- Background: Normal
- Interaction: Not disabled (user can keep typing)
- Used for: Async validation, search-as-you-type

---

## Anatomy

```
┌─────────────────────────────────────────────────────┐
│  Label                          [Optional indicator] │
│                                                      │
│  ┌─ Prefix ─┬─────────── Input ──────────┬─Suffix ─┐│
│  │ $  / icon│ Placeholder or value       │ icon / × ││
│  └──────────┴────────────────────────────┴─────────┘│
│                                                      │
│  Helper text / error message        Character count  │
└─────────────────────────────────────────────────────┘
```

### **Slots Breakdown**

**Label** (Above input)
- Required marker (asterisk)
- Optional label
- Tooltip / hint icon beside label

**Leading / Prefix Slot**
- Icon (search, email, lock)
- Text addon ($, +1, https://)
- Avatar (for user search inputs)

**Input Area**
- Placeholder text
- Value text
- Password masking

**Trailing / Suffix Slot**
- Clear button (×)
- Password reveal toggle
- Validation icon (check, error)
- Loading spinner
- Character counter
- Dropdown caret (for select-like inputs)
- Action button (Send, Copy)

**Helper Zone** (Below input)
- Helper text (hint, instruction)
- Error message
- Warning message
- Success message
- Character count (right-aligned)

---

## Props API

### **Input**

```typescript
interface InputProps {
  // Value
  value?: string;
  defaultValue?: string;
  onChange?: (e: React.ChangeEvent<HTMLInputElement>) => void;
  onChangeValue?: (value: string) => void;     // Convenience: receives value directly

  // Type & Behavior
  type?: 'text' | 'email' | 'password' | 'search' | 'number'
       | 'tel' | 'url' | 'date' | 'time' | 'datetime-local';
  placeholder?: string;
  autoComplete?: string;
  autoFocus?: boolean;
  spellCheck?: boolean;
  inputMode?: 'text' | 'numeric' | 'decimal' | 'tel' | 'email' | 'url' | 'search';
  enterKeyHint?: 'enter' | 'done' | 'go' | 'next' | 'previous' | 'search' | 'send';

  // Number-specific
  min?: number;
  max?: number;
  step?: number;

  // Constraints
  minLength?: number;
  maxLength?: number;
  pattern?: string;
  required?: boolean;

  // Appearance
  variant?: 'outlined' | 'filled' | 'flushed' | 'unstyled';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  fullWidth?: boolean;

  // Label & Help
  label?: string;
  labelPosition?: 'top' | 'left' | 'floating';
  optionalLabel?: string;                      // e.g., "(optional)"
  helperText?: string;
  characterCount?: boolean;

  // Validation State
  status?: 'default' | 'error' | 'warning' | 'success' | 'loading';
  errorMessage?: string;
  warningMessage?: string;
  successMessage?: string;

  // Slots
  leadingIcon?: React.ReactNode;
  trailingIcon?: React.ReactNode;
  prefix?: string | React.ReactNode;           // Text/element before input
  suffix?: string | React.ReactNode;           // Text/element after input

  // Built-in Behaviors
  clearable?: boolean;                         // Show × to clear value
  onClear?: () => void;
  showPasswordToggle?: boolean;                // For type="password"
  passwordStrength?: boolean;                  // Show strength meter

  // States
  disabled?: boolean;
  readOnly?: boolean;
  loading?: boolean;

  // Form
  name?: string;
  id?: string;
  form?: string;

  // Accessibility
  'aria-label'?: string;
  'aria-describedby'?: string;
  'aria-invalid'?: boolean | 'true' | 'false';
  'aria-required'?: boolean;

  // Events
  onFocus?: (e: React.FocusEvent<HTMLInputElement>) => void;
  onBlur?: (e: React.FocusEvent<HTMLInputElement>) => void;
  onKeyDown?: (e: React.KeyboardEvent<HTMLInputElement>) => void;
  onKeyUp?: (e: React.KeyboardEvent<HTMLInputElement>) => void;
  onPaste?: (e: React.ClipboardEvent<HTMLInputElement>) => void;

  // Styling
  className?: string;
  inputClassName?: string;                     // Class on the inner <input> only
  style?: React.CSSProperties;
}
```

### **Textarea**

```typescript
interface TextareaProps extends Omit<InputProps, 'type' | 'min' | 'max' | 'step'> {
  rows?: number;                               // Initial visible rows (default: 3)
  maxRows?: number;                            // Max rows before scroll (auto-resize)
  autoResize?: boolean;                        // Grow height with content
  resize?: 'none' | 'vertical' | 'horizontal' | 'both';
}
```

### **InputGroup**

```typescript
interface InputGroupProps {
  children: React.ReactNode;                   // Input + InputAddon elements
  size?: InputProps['size'];
  variant?: InputProps['variant'];
  status?: InputProps['status'];
  fullWidth?: boolean;
  className?: string;
}

interface InputAddonProps {
  placement: 'left' | 'right';
  children: React.ReactNode;
  asButton?: boolean;                          // Renders addon as clickable button
  onClick?: () => void;
  className?: string;
}
```

---

## Code Examples

### **1. Basic Text Input**
```typescript
import { Input } from '@components/input';
import { useState } from 'react';

export function BasicInput() {
  const [value, setValue] = useState('');

  return (
    <Input
      label="Full name"
      placeholder="Enter your full name"
      value={value}
      onChangeValue={setValue}
      autoComplete="name"
    />
  );
}
```

### **2. Input with Validation States**
```typescript
import { Input } from '@components/input';
import { useState } from 'react';

export function ValidatedEmailInput() {
  const [email, setEmail] = useState('');
  const [touched, setTouched] = useState(false);

  const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  const showError = touched && email && !isValid;
  const showSuccess = touched && email && isValid;

  return (
    <Input
      type="email"
      label="Email address"
      placeholder="you@example.com"
      value={email}
      onChangeValue={setEmail}
      onBlur={() => setTouched(true)}
      status={showError ? 'error' : showSuccess ? 'success' : 'default'}
      errorMessage="Please enter a valid email address."
      successMessage="Looks good!"
      required
      autoComplete="email"
    />
  );
}
```

### **3. Password Input with Reveal Toggle**
```typescript
import { Input } from '@components/input';
import { useState } from 'react';

export function PasswordInput() {
  const [password, setPassword] = useState('');

  return (
    <Input
      type="password"
      label="Password"
      placeholder="Min. 8 characters"
      value={password}
      onChangeValue={setPassword}
      showPasswordToggle
      passwordStrength
      minLength={8}
      helperText="Must include uppercase, number, and special character."
      autoComplete="new-password"
    />
  );
}
```

### **4. Search Input with Clear**
```typescript
import { Input } from '@components/input';
import { Search } from 'lucide-react';
import { useState } from 'react';

export function SearchInput() {
  const [query, setQuery] = useState('');

  return (
    <Input
      type="search"
      placeholder="Search projects…"
      value={query}
      onChangeValue={setQuery}
      leadingIcon={<Search size={16} />}
      clearable
      onClear={() => setQuery('')}
      size="md"
      aria-label="Search projects"
    />
  );
}
```

### **5. Input with Prefix / Suffix Addons**
```typescript
import { InputGroup, Input, InputAddon } from '@components/input';

export function CurrencyInput() {
  return (
    <InputGroup>
      <InputAddon placement="left">$</InputAddon>
      <Input
        type="number"
        placeholder="0.00"
        min={0}
        step={0.01}
        label="Amount"
        aria-label="Amount in dollars"
      />
      <InputAddon placement="right">USD</InputAddon>
    </InputGroup>
  );
}
```

### **6. Input with Button Addon**
```typescript
import { InputGroup, Input, InputAddon } from '@components/input';
import { Copy } from 'lucide-react';
import { useState } from 'react';

export function CopyableInput({ value }: { value: string }) {
  const [copied, setCopied] = useState(false);

  const handleCopy = async () => {
    await navigator.clipboard.writeText(value);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  };

  return (
    <InputGroup>
      <Input
        value={value}
        readOnly
        label="API Key"
        aria-label="API Key"
      />
      <InputAddon placement="right" asButton onClick={handleCopy} aria-label="Copy API key">
        <Copy size={16} />
        {copied ? 'Copied!' : 'Copy'}
      </InputAddon>
    </InputGroup>
  );
}
```

### **7. Textarea with Auto-Resize**
```typescript
import { Textarea } from '@components/input';
import { useState } from 'react';

export function CommentBox() {
  const [comment, setComment] = useState('');

  return (
    <Textarea
      label="Comment"
      placeholder="Share your thoughts…"
      value={comment}
      onChangeValue={setComment}
      autoResize
      rows={3}
      maxRows={8}
      maxLength={500}
      characterCount
      helperText="Be respectful and constructive."
    />
  );
}
```

### **8. Input Group with Select Addon**
```typescript
import { InputGroup, Input, InputAddon } from '@components/input';
import { Select } from '@components/select';
import { useState } from 'react';

export function PhoneInput() {
  const [countryCode, setCountryCode] = useState('+1');
  const [phone, setPhone] = useState('');

  return (
    <InputGroup>
      <InputAddon placement="left">
        <Select
          value={countryCode}
          onChange={setCountryCode}
          size="md"
          variant="unstyled"
          options={[
            { value: '+1', label: '🇺🇸 +1' },
            { value: '+44', label: '🇬🇧 +44' },
            { value: '+61', label: '🇦🇺 +61' },
          ]}
          aria-label="Country code"
        />
      </InputAddon>
      <Input
        type="tel"
        placeholder="(555) 000-0000"
        value={phone}
        onChangeValue={setPhone}
        label="Phone number"
        inputMode="tel"
        autoComplete="tel"
      />
    </InputGroup>
  );
}
```

### **9. Floating Label Input**
```typescript
import { Input } from '@components/input';
import { useState } from 'react';

export function FloatingLabelForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  return (
    <div className="space-y-4">
      <Input
        label="Full name"
        labelPosition="floating"
        value={name}
        onChangeValue={setName}
        autoComplete="name"
      />
      <Input
        label="Email address"
        labelPosition="floating"
        type="email"
        value={email}
        onChangeValue={setEmail}
        autoComplete="email"
      />
    </div>
  );
}
```

### **10. React Hook Form Integration**
```typescript
import { Input, Textarea } from '@components/input';
import { useForm } from 'react-hook-form';

interface FormData {
  name: string;
  email: string;
  message: string;
}

export function ContactForm() {
  const { register, handleSubmit, formState: { errors } } = useForm<FormData>();

  return (
    <form onSubmit={handleSubmit(console.log)} className="space-y-4">
      <Input
        label="Name"
        {...register('name', { required: 'Name is required' })}
        status={errors.name ? 'error' : 'default'}
        errorMessage={errors.name?.message}
      />
      <Input
        label="Email"
        type="email"
        {...register('email', {
          required: 'Email is required',
          pattern: { value: /\S+@\S+\.\S+/, message: 'Invalid email' },
        })}
        status={errors.email ? 'error' : 'default'}
        errorMessage={errors.email?.message}
      />
      <Textarea
        label="Message"
        rows={4}
        {...register('message', { required: 'Message is required' })}
        status={errors.message ? 'error' : 'default'}
        errorMessage={errors.message?.message}
      />
      <button type="submit">Send message</button>
    </form>
  );
}
```

---

## Accessibility Specifications

### **Label Association**
Every input must have an associated label — either visible or accessible:

```tsx
// ✅ Visible label via `label` prop (preferred)
<Input id="email" label="Email address" />
// Renders: <label for="email">Email address</label> + <input id="email">

// ✅ Aria-label when no visible label (e.g., search bar)
<Input aria-label="Search projects" />

// ✅ Aria-labelledby for external label element
<h2 id="section-heading">Contact</h2>
<Input aria-labelledby="section-heading" />

// ❌ Never: Input with no label association
<Input placeholder="Email" />  // Placeholder is NOT a label
```

### **Placeholder vs. Label**
- Placeholder disappears on typing — never use it as a label substitute
- Placeholder is for example format: `"e.g. john@example.com"` not `"Email address"`
- WCAG 2.1 1.3.5: Inputs with autocomplete should use `autoComplete` values

### **Error Message Association**
```tsx
<Input
  id="email"
  label="Email"
  status="error"
  errorMessage="Please enter a valid email."
  // Renders: aria-describedby="email-error" on input
  // Renders: <span id="email-error" role="alert">Please enter a valid email.</span>
/>
```

- Error message: `role="alert"` or `aria-live="polite"` so screen readers announce it
- `aria-invalid="true"` on the input when in error state
- Error message linked via `aria-describedby` to the input

### **Required Fields**
```tsx
// Renders asterisk * and aria-required="true"
<Input label="Email" required />
```

- `aria-required="true"` on the `<input>` element
- Visual asterisk in the label (decorative; meaning carried by aria-required)
- Group-level instruction: "Fields marked with * are required" — placed before the form

### **Keyboard Navigation**
- **Tab**: Move focus into input
- **Shift+Tab**: Move focus to previous element
- **Type**: Enter or edit text
- **Ctrl+A**: Select all text
- **Escape**: Clear search input (optional convenience)
- **Enter**: Submit form or trigger search
- Number inputs — **Up/Down Arrow**: Increment/decrement by `step` value

### **Password Reveal Button**
- Toggle button: `aria-label="Show password"` / `aria-label="Hide password"` (toggles)
- `aria-pressed` reflects visible/hidden state
- Changing label communicates state change to screen readers without announcing password content

### **Character Count**
- Live region: `aria-live="polite"` announces remaining characters at threshold (e.g., 20 remaining)
- Not announced on every keystroke — only when near limit
- Visual counter: `"142 / 500"` or `"358 remaining"`

### **Autocomplete**
- Use standard HTML `autocomplete` attribute values (WCAG 1.3.5)
- Common values: `"name"`, `"email"`, `"tel"`, `"current-password"`, `"new-password"`, `"one-time-code"`

### **Screen Reader Announcements**
- On focus: "[Label], edit text" (NVDA) / "[Label], text field" (JAWS)
- Error state: "[Label], invalid, edit text. Error: [message]" on focus
- Required: "[Label], required, edit text"
- Character limit approaching: "80 characters remaining" (aria-live polite)
- Clear button: "Clear [label], button"
- Password toggle: "Show password, button" / "Hide password, button"

### **Touch Accessibility**
- Minimum touch target for addons and buttons: 44×44px
- `inputMode` prop for correct mobile keyboard:
  - Numeric: `inputMode="numeric"`
  - Phone: `inputMode="tel"`
  - Email: `inputMode="email"`
  - URL: `inputMode="url"`
- `enterKeyHint` for correct mobile keyboard return key label

---

## Animation Specifications

### **Focus Ring**
- **Target**: box-shadow (ring) + border-color
- **Duration**: 100ms ease-out
  ```css
  input { transition: border-color 100ms ease-out, box-shadow 100ms ease-out; }
  input:focus {
    border-color: #3B82F6;
    box-shadow: 0 0 0 3px rgba(59,130,246,0.2);
  }
  ```

### **Error State Appearance**
- **Target**: border-color + box-shadow
- **Duration**: 150ms ease-out
- **Shake animation** (optional): brief horizontal shake on submit with errors
  ```css
  @keyframes inputShake {
    0%, 100% { transform: translateX(0); }
    20%, 60% { transform: translateX(-4px); }
    40%, 80% { transform: translateX(4px); }
  }
  .input-error-shake { animation: inputShake 400ms ease-out; }
  ```

### **Floating Label**
- **Target**: translateY + font-size + color
- **Duration**: 150ms ease-out
- **Resting**: Inside input, gray, full size
- **Floating**: Above input, smaller, brand color
  ```css
  label {
    transition: transform 150ms ease-out, font-size 150ms ease-out, color 150ms ease-out;
  }
  input:focus ~ label,
  input:not(:placeholder-shown) ~ label {
    transform: translateY(-24px);
    font-size: 12px;
    color: #3B82F6;
  }
  ```

### **Clear Button Appearance**
- **Target**: Opacity + scale
- **Duration**: 100ms ease-out
- **Appears**: When input has value (opacity: 0 → 1, scale: 0.8 → 1)

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  input, label { transition: none; }
  .input-error-shake { animation: none; }
}
```

---

## Design Tokens

### **Colors**
| State | Border | Ring | Background | Text |
|-------|--------|------|------------|------|
| Default | #D1D5DB | — | #FFFFFF | #111827 |
| Hover | #9CA3AF | — | #FFFFFF | #111827 |
| Focus | #3B82F6 | rgba(59,130,246,0.2) | #FFFFFF | #111827 |
| Error | #EF4444 | rgba(239,68,68,0.15) | #FFFFFF | #111827 |
| Warning | #F59E0B | rgba(245,158,11,0.15) | #FFFFFF | #111827 |
| Success | #10B981 | rgba(16,185,129,0.15) | #FFFFFF | #111827 |
| Disabled | #E5E7EB | — | #F3F4F6 | #9CA3AF |
| Read-only | #E5E7EB | — | #F9FAFB | #374151 |

### **Typography**
| Element | Size | Color |
|---------|------|-------|
| Label | 14px / 500 | #374151 |
| Placeholder | 14px / 400 | #9CA3AF |
| Value | 14px / 400 | #111827 |
| Helper text | 12px / 400 | #6B7280 |
| Error message | 12px / 400 | #EF4444 |
| Character count | 12px / 400 | #9CA3AF |

### **Sizing**
| Size | Height | H-Padding | Font | Border Radius |
|------|--------|-----------|------|---------------|
| XS | 28px | 8px | 12px | 4px |
| SM | 32px | 10px | 13px | 6px |
| MD | 40px | 12px | 14px | 8px |
| LG | 48px | 14px | 16px | 8px |
| XL | 56px | 16px | 18px | 10px |

---

## Best Practices

### **Labels**
- Always show a visible label — placeholder text alone is insufficient
- Keep labels short (1–3 words); use helper text for instructions
- Use sentence case: "Email address" not "EMAIL ADDRESS"
- Mark required fields with asterisk AND `required` prop — not just one

### **Placeholder Text**
- Use for format hints: `"MM/DD/YYYY"`, `"e.g. john@example.com"`
- Never use as a replacement for labels
- Keep extremely short; it disappears on focus

### **Error Messages**
- Appear inline below the field, not in a toast or top-of-form summary alone
- Be specific: "Email already in use" not "Invalid input"
- Suggest resolution: "Try a different email or sign in instead"
- Show on blur (not on every keystroke) to avoid premature errors
- Clear the error immediately when the user starts correcting the field

### **Character Limits**
- Show character count when users are likely to write near the limit
- Show remaining (not used): "82 remaining" not "418 / 500 used"
- Begin showing count when 20–25% of limit remains
- Don't prevent typing past limit — show over-limit state and block submission

### **Grouping**
- Use InputGroup for visually attached addons (currency symbol, domain suffix)
- Keep addons short — long addon labels break layout at small sizes
- Test addon buttons have sufficient touch target (44×44px minimum)

### **Mobile UX**
- Set `inputMode` for the appropriate software keyboard
- Set `enterKeyHint` for the correct return key affordance
- Set `autoComplete` to enable password managers and autofill
- Avoid placing multiple inputs side by side on mobile — stack vertically

### **Form Integration**
- Always use `name` prop for native form submission and React Hook Form
- Use `id` consistently (matches `for` on `<label>`)
- Support uncontrolled mode (`defaultValue`) for forms that don't need reactive state

---

## Password Strength Meter

When `passwordStrength` is true, a strength indicator appears below the input:

| Strength | Criteria | Color | Label |
|----------|----------|-------|-------|
| Very Weak | < 6 chars | #EF4444 | Very weak |
| Weak | 6+ chars, no complexity | #F97316 | Weak |
| Fair | 8+ chars, 1 complexity type | #F59E0B | Fair |
| Strong | 8+ chars, 2 complexity types | #84CC16 | Strong |
| Very Strong | 12+ chars, 3+ complexity types | #10B981 | Very strong |

Complexity types: uppercase, lowercase, number, special character

---

## Related Components

- **Select**: Dropdown selection; use when options are predefined
- **Combobox / Autocomplete**: Input + dropdown suggestions; for search-as-you-type
- **DatePicker**: Specialized input for date/time selection
- **NumberInput / Stepper**: Increment/decrement number input
- **PhoneInput**: Input with country code selector
- **Textarea**: Multi-line version; same token system
- **Form**: Wrapper providing layout, spacing, and shared validation context
- **Checkbox / Radio**: For boolean or single-choice form fields

---

## Version History

- **v1.0**: Text, email, password types; outlined variant; label, placeholder, error state
- **v1.1**: Filled + flushed variants; leading/trailing icon slots; clearable
- **v1.2**: Number input; Textarea with auto-resize; character count
- **v2.0**: InputGroup + InputAddon; password strength meter; floating label; full token system
- **v2.1**: Search type; `status` prop (warning/success/loading); React Hook Form docs
- **v2.2**: `inputMode`, `enterKeyHint`; mobile keyboard improvements; reduced motion

---

## Testing Checklist

- [ ] Default state renders with correct border and placeholder
- [ ] Hover darkens border color
- [ ] Focus shows brand ring (3px rgba)
- [ ] Error state shows red border + ring + error message
- [ ] Warning state shows amber border
- [ ] Success state shows green border + check icon
- [ ] Disabled state prevents interaction and applies opacity
- [ ] Read-only state prevents editing but allows selection/copy
- [ ] Label is associated with input via `for`/`id`
- [ ] Required asterisk + `aria-required="true"` on required inputs
- [ ] `aria-invalid="true"` set when in error state
- [ ] Error message linked via `aria-describedby`
- [ ] Error message has `role="alert"` for screen reader announcement
- [ ] Clearable × button appears when input has value
- [ ] Clear button fires `onClear` and clears value
- [ ] Password toggle shows/hides password and updates `aria-label`
- [ ] Password strength meter shows correct level
- [ ] Character count shows and approaches-limit warning fires
- [ ] Leading and trailing icon slots render correctly
- [ ] Prefix/suffix addons render as visually attached
- [ ] Button addon is keyboard accessible (44×44px touch target)
- [ ] Textarea auto-resize grows with content up to `maxRows`
- [ ] `inputMode` sets correct mobile keyboard
- [ ] `autoComplete` attribute present on form fields
- [ ] Focus ring animation 100ms ease-out
- [ ] Floating label animates up on focus/value
- [ ] All animations disabled under `prefers-reduced-motion`
- [ ] React Hook Form integration: register, error display, reset
