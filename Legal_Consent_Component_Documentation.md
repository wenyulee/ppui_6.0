# Legal Consent Component Documentation

## Overview

The Legal Consent component provides a standardized, accessible, and legally defensible interface for capturing user agreement to terms, policies, and data processing conditions. Unlike a plain checkbox, Legal Consent is a purposefully designed consent surface that presents policy content inline or via links, enforces explicit opt-in mechanics, records consent metadata, and visually communicates the weight of the user's commitment. It is used in account registration flows, checkout, cookie banners, data processing agreements, age verification, and marketing opt-in scenarios. Properly implemented consent UI is not only a UX concern — it is a legal and regulatory requirement under GDPR, CCPA, COPPA, and similar frameworks.

---

## Component Variants

Legal Consent supports six primary usage patterns:

### 1. **Terms & Privacy Checkbox**
- **Purpose**: Single or paired checkboxes requiring explicit agreement to terms of service and/or privacy policy
- **Use Cases**: Account registration, app onboarding, service sign-up
- **Behavior**: Must be checked before form submission; cannot be pre-checked
- **Legal basis**: Contract performance / Terms of Service
- **Visual**: Checkbox + inline text with linked policy documents
- **Example**: "I agree to the [Terms of Service] and [Privacy Policy]"

### 2. **Marketing Opt-In**
- **Purpose**: Optional consent to receive marketing communications
- **Use Cases**: Newsletter sign-up, promotional emails, push notifications, SMS marketing
- **Behavior**: Not required for service; must be unchecked by default (opt-in, not opt-out)
- **Legal basis**: Consent (GDPR Article 6(1)(a)); must be freely given, specific, informed, and unambiguous
- **Visual**: Checkbox with description of what communications the user will receive
- **Example**: "I'd like to receive product updates and promotional offers by email"

### 3. **Cookie Consent Banner**
- **Purpose**: Inform users of cookie usage and capture granular consent by category
- **Use Cases**: First visit, cookie settings, privacy preferences center
- **Behavior**: Necessary cookies cannot be disabled; Analytics/Marketing cookies require explicit consent
- **Legal basis**: GDPR ePrivacy Directive
- **Visual**: Banner or modal with category toggles and "Accept all / Reject all / Manage preferences"
- **Persistence**: Decision stored in cookie or localStorage; reviewed annually

### 4. **Data Processing Agreement (DPA)**
- **Purpose**: Explicit consent for specific data processing activities beyond core service
- **Use Cases**: Third-party data sharing, profiling, automated decision-making, sensitive data processing
- **Behavior**: Individual consent item per processing purpose; granular and independent
- **Legal basis**: Consent (GDPR) — separate from terms acceptance
- **Visual**: Expandable disclosure with processing details + toggle or checkbox per purpose
- **Example**: "Allow [Product] to share anonymized usage data with analytics partners"

### 5. **Age Verification**
- **Purpose**: Confirm user meets minimum age requirements
- **Use Cases**: Alcohol/tobacco, adult content, financial services, age-restricted apps
- **Behavior**: May gate content behind age check; cannot be auto-filled
- **Legal basis**: COPPA / local regulations
- **Visual**: Date of birth input or checkbox affirming age; clear statement of requirement
- **Example**: "I confirm I am 18 years of age or older"

### 6. **Electronic Signature / Acknowledgment**
- **Purpose**: Formal acknowledgment or signature-equivalent for contracts, waivers, or agreements
- **Use Cases**: Contract acceptance, liability waivers, HIPAA acknowledgment, onboarding agreements
- **Behavior**: Requires typed name or initials as explicit acknowledgment; timestamped
- **Legal basis**: Electronic Signatures in Global and National Commerce Act (E-SIGN), eIDAS
- **Visual**: Document preview + name input + checkbox + submission button + confirmation summary

---

## Size Variants

| Size | Checkbox | Label | Description | Use Case |
|------|----------|-------|-------------|---------|
| **SM** | 16px | 13px | 12px | Compact forms, inline consent |
| **MD** | 18px | 14px | 13px | **Default**, standard sign-up flows |
| **LG** | 20px | 16px | 14px | Prominent consent, age verification |
| **XL** | 24px | 18px | 15px | Legal agreements, e-signature |

---

## State Specifications

### **Unchecked (Required)** — Default
- Checkbox: Unchecked, standard border
- Label: Gray 900
- Description: Gray 600
- Links: Brand color (underlined)
- Submit button: Disabled (until checked)
- Visual weight: Standard

### **Unchecked (Optional)**
- Checkbox: Unchecked, standard border
- Label: Gray 700 (slightly softer than required)
- Description: Gray 500
- Submit button: Not blocked

### **Checked**
- Checkbox: Filled brand color
- Label: Gray 900
- Checkmark icon: White
- Submit button: Enabled (if all required items checked)
- Transition: 150ms ease-out

### **Hover**
- Checkbox border: Brand color
- Background: Light brand tint (10% opacity)
- Cursor: pointer
- Duration: 150ms ease-out

### **Focus** (Keyboard)
- Outline: 2px solid brand, 2px offset
- Ring: 0 0 0 4px rgba(59,130,246,0.2)
- Duration: 100ms

### **Error / Validation**
- Checkbox border: Danger red (#EF4444)
- Error message: Below item in red, 12px
- Role: `aria-live="polite"` or `role="alert"` on error container
- Message: "You must agree to the Terms of Service to continue"

### **Disabled**
- Checkbox: Gray fill (if checked) or gray border (if unchecked)
- Label: Gray 400
- Cursor: not-allowed
- Use case: Pre-checked necessary cookies that cannot be changed

### **Read-Only / Summary**
- Checkbox: Visible but non-interactive (checked state shown)
- Label: Gray 700
- Use case: Confirmation screen showing user's consent choices before final submission

---

## Props API

### **LegalConsent**

```typescript
interface LegalConsentProps {
  // Content
  id: string;                                  // Unique identifier for consent item
  label: string | React.ReactNode;            // Primary consent text (may include links)
  description?: string | React.ReactNode;     // Expanded detail / policy excerpt
  expandable?: boolean;                        // Collapsible detail section
  defaultExpanded?: boolean;

  // Type & Requirement
  type?: 'terms' | 'marketing' | 'cookie' | 'dpa' | 'age' | 'signature';
  required?: boolean;                          // Blocks form submission if unchecked
  optional?: boolean;                          // Explicitly labeled as optional

  // Value
  checked?: boolean;                           // Controlled state
  defaultChecked?: boolean;                    // Uncontrolled initial state
  onChange?: (checked: boolean, id: string) => void;

  // Links
  links?: ConsentLink[];                       // Policy document links within label

  // Appearance
  size?: 'sm' | 'md' | 'lg' | 'xl';
  variant?: 'checkbox' | 'toggle' | 'card';   // Control style
  layout?: 'inline' | 'stacked';              // Label/description arrangement

  // Validation
  error?: boolean;
  errorMessage?: string;

  // Consent Metadata (for audit trail)
  version?: string;                            // Policy version: "v2.1"
  effectiveDate?: string;                      // ISO date string
  onConsentRecord?: (record: ConsentRecord) => void; // Callback with full metadata

  // E-Signature (type="signature")
  documentTitle?: string;
  documentSummary?: string;
  requireTypedName?: boolean;
  signerName?: string;
  onSignerNameChange?: (name: string) => void;

  // Age Verification (type="age")
  minimumAge?: number;                         // Default: 18
  requireDOB?: boolean;                        // Use DOB input instead of checkbox

  // Accessibility
  'aria-label'?: string;
  'aria-describedby'?: string;

  // Styling
  className?: string;
  style?: React.CSSProperties;
}

interface ConsentLink {
  text: string;                                // Anchor text
  href: string;                                // Policy URL
  external?: boolean;                          // Opens in new tab
  label?: string;                              // aria-label for screen readers
}

interface ConsentRecord {
  id: string;
  checked: boolean;
  timestamp: string;                           // ISO 8601
  version?: string;
  userAgent: string;
  ipAddress?: string;                          // Server-side only
}
```

### **LegalConsentGroup**

```typescript
interface LegalConsentGroupProps {
  // Items
  items: LegalConsentProps[];

  // Value Management
  values?: Record<string, boolean>;            // Controlled: { [id]: boolean }
  defaultValues?: Record<string, boolean>;
  onChange?: (values: Record<string, boolean>) => void;

  // Group Behavior
  showAcceptAll?: boolean;                     // "Accept all" button/checkbox
  showRejectAll?: boolean;                     // "Reject all" button (optional items only)
  onAcceptAll?: () => void;
  onRejectAll?: () => void;

  // Validation
  onValidate?: (isValid: boolean) => void;     // Called when required items state changes

  // Consent Recording
  onConsentSubmit?: (records: ConsentRecord[]) => void;

  // Layout
  gap?: 'sm' | 'md' | 'lg';
  dividers?: boolean;                          // Dividers between items

  // Accessibility
  'aria-label'?: string;
  role?: 'group' | 'region';

  // Styling
  className?: string;
}
```

---

## Code Examples

### **1. Standard Registration Consent**
```typescript
import { LegalConsent } from '@components/legal-consent';
import { useState } from 'react';

export function RegistrationConsent() {
  const [termsAccepted, setTermsAccepted] = useState(false);
  const [submitted, setSubmitted] = useState(false);

  const hasError = submitted && !termsAccepted;

  return (
    <LegalConsent
      id="terms-privacy"
      type="terms"
      required
      checked={termsAccepted}
      onChange={(checked) => setTermsAccepted(checked)}
      label={
        <>
          I agree to the{' '}
          <a href="/terms" target="_blank" rel="noopener noreferrer">
            Terms of Service
          </a>{' '}
          and{' '}
          <a href="/privacy" target="_blank" rel="noopener noreferrer">
            Privacy Policy
          </a>
        </>
      }
      error={hasError}
      errorMessage="You must agree to the Terms of Service to continue."
      version="2.1"
      effectiveDate="2026-01-01"
    />
  );
}
```

### **2. Marketing Opt-In (Optional, Unchecked Default)**
```typescript
import { LegalConsent } from '@components/legal-consent';
import { useState } from 'react';

export function MarketingOptIn() {
  const [optedIn, setOptedIn] = useState(false); // Must default to false

  return (
    <LegalConsent
      id="marketing-emails"
      type="marketing"
      required={false}
      optional
      checked={optedIn}
      onChange={(checked) => setOptedIn(checked)}
      label="Send me product updates, tips, and exclusive offers by email"
      description="We'll send you occasional emails about new features and promotions. You can unsubscribe at any time."
      size="md"
    />
  );
}
```

### **3. Cookie Consent Banner**
```typescript
import { LegalConsentGroup } from '@components/legal-consent';
import { useState } from 'react';

export function CookieConsentBanner() {
  const [visible, setVisible] = useState(true);
  const [values, setValues] = useState({
    necessary: true,
    analytics: false,
    marketing: false,
  });

  const handleSubmit = (vals: Record<string, boolean>) => {
    // Store consent decision
    document.cookie = `cookie_consent=${JSON.stringify(vals)}; max-age=31536000`;
    setVisible(false);
  };

  if (!visible) return null;

  return (
    <div role="dialog" aria-modal="false" aria-label="Cookie preferences">
      <h2>We use cookies</h2>
      <p>
        We use cookies to improve your experience, analyze traffic, and personalize
        content. Manage your preferences below.
      </p>
      <LegalConsentGroup
        values={values}
        onChange={setValues}
        showAcceptAll
        showRejectAll
        onAcceptAll={() => setValues({ necessary: true, analytics: true, marketing: true })}
        onRejectAll={() => setValues({ necessary: true, analytics: false, marketing: false })}
        dividers
        items={[
          {
            id: 'necessary',
            label: 'Strictly Necessary',
            description: 'Required for the website to function. Cannot be disabled.',
            checked: true,
            variant: 'toggle',
          },
          {
            id: 'analytics',
            label: 'Analytics & Performance',
            description: 'Help us understand how visitors use our site (Google Analytics, Hotjar).',
            variant: 'toggle',
          },
          {
            id: 'marketing',
            label: 'Marketing & Advertising',
            description: 'Used to deliver relevant ads and track campaign effectiveness.',
            variant: 'toggle',
          },
        ]}
      />
      <div className="flex gap-3 mt-4">
        <button onClick={() => handleSubmit(values)}>Save preferences</button>
        <button
          onClick={() =>
            handleSubmit({ necessary: true, analytics: true, marketing: true })
          }
        >
          Accept all
        </button>
      </div>
    </div>
  );
}
```

### **4. Granular Data Processing Consent**
```typescript
import { LegalConsentGroup } from '@components/legal-consent';
import { useState } from 'react';

export function DataProcessingConsents() {
  const [consents, setConsents] = useState<Record<string, boolean>>({});

  return (
    <LegalConsentGroup
      values={consents}
      onChange={setConsents}
      role="group"
      aria-label="Data processing preferences"
      dividers
      items={[
        {
          id: 'analytics',
          type: 'dpa',
          label: 'Usage analytics',
          description:
            'We process anonymized usage data to improve product features. Data retained for 24 months.',
          expandable: true,
          variant: 'toggle',
        },
        {
          id: 'third-party-sharing',
          type: 'dpa',
          label: 'Share data with analytics partners',
          description:
            'Aggregated, anonymized data may be shared with Mixpanel and Amplitude for usage analysis.',
          expandable: true,
          variant: 'toggle',
        },
        {
          id: 'personalization',
          type: 'dpa',
          label: 'Personalized experience',
          description:
            'We use your activity to customize content recommendations and feature suggestions.',
          expandable: true,
          variant: 'toggle',
        },
      ]}
    />
  );
}
```

### **5. Age Verification**
```typescript
import { LegalConsent } from '@components/legal-consent';
import { useState } from 'react';

export function AgeVerification() {
  const [confirmed, setConfirmed] = useState(false);
  const [submitted, setSubmitted] = useState(false);

  const hasError = submitted && !confirmed;

  return (
    <LegalConsent
      id="age-verification"
      type="age"
      required
      minimumAge={18}
      checked={confirmed}
      onChange={(checked) => setConfirmed(checked)}
      label="I confirm that I am 18 years of age or older"
      description="This service is only available to adults. By checking this box, you confirm you meet the age requirement in your jurisdiction."
      size="md"
      error={hasError}
      errorMessage="You must confirm you are 18 or older to access this service."
    />
  );
}
```

### **6. Electronic Signature / Acknowledgment**
```typescript
import { LegalConsent } from '@components/legal-consent';
import { useState } from 'react';

export function ServiceAgreementSignature() {
  const [signed, setSigned] = useState(false);
  const [signerName, setSignerName] = useState('');

  const isValid = signed && signerName.trim().length >= 2;

  return (
    <LegalConsent
      id="service-agreement"
      type="signature"
      required
      documentTitle="Service Level Agreement v3.0"
      documentSummary="This agreement governs the terms under which Acme provides services. Key points: 99.9% uptime SLA, 30-day notice for termination, limitation of liability."
      requireTypedName
      signerName={signerName}
      onSignerNameChange={setSignerName}
      checked={signed}
      onChange={(checked) => setSigned(checked)}
      label="I have read and agree to the Service Level Agreement"
      size="lg"
      version="3.0"
      effectiveDate="2026-01-15"
    />
  );
}
```

### **7. Full Registration Consent Block**
```typescript
import { LegalConsentGroup } from '@components/legal-consent';
import { useState } from 'react';

export function FullRegistrationConsents() {
  const [values, setValues] = useState<Record<string, boolean>>({
    terms: false,
    marketing: false,
    sms: false,
  });

  const isValid = values['terms'] === true; // Only terms is required

  return (
    <LegalConsentGroup
      values={values}
      onChange={setValues}
      onValidate={(valid) => console.log('Valid:', valid)}
      gap="md"
      items={[
        {
          id: 'terms',
          type: 'terms',
          required: true,
          label: (
            <>
              I agree to the{' '}
              <a href="/terms" target="_blank" rel="noopener noreferrer">Terms of Service</a>
              {' '}and{' '}
              <a href="/privacy" target="_blank" rel="noopener noreferrer">Privacy Policy</a>
            </>
          ),
          version: '4.0',
          effectiveDate: '2026-01-01',
        },
        {
          id: 'marketing',
          type: 'marketing',
          optional: true,
          label: 'Send me product updates and tips by email',
          description: 'Occasional emails only. Unsubscribe at any time.',
        },
        {
          id: 'sms',
          type: 'marketing',
          optional: true,
          label: 'Send me SMS notifications',
          description: 'Message and data rates may apply. Reply STOP to unsubscribe.',
        },
      ]}
    />
  );
}
```

### **8. Consent Audit Trail Recording**
```typescript
import { LegalConsent } from '@components/legal-consent';
import { useState } from 'react';

export function AuditedConsent() {
  const [accepted, setAccepted] = useState(false);

  const handleConsentRecord = (record: ConsentRecord) => {
    // Send to your backend for audit trail storage
    fetch('/api/consent', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(record),
    });
  };

  return (
    <LegalConsent
      id="terms-v4"
      type="terms"
      required
      checked={accepted}
      onChange={(checked) => setAccepted(checked)}
      label={
        <>
          I agree to the{' '}
          <a href="/terms/v4" target="_blank" rel="noopener noreferrer">Terms of Service</a>
        </>
      }
      version="4.0"
      effectiveDate="2026-01-01"
      onConsentRecord={handleConsentRecord}
    />
  );
}
```

---

## Accessibility Specifications

### **HTML Semantics**
- Checkbox-type consents: Native `<input type="checkbox">` inside `<label>` — identical to Checkbox component
- Toggle-type consents: `<button role="switch" aria-checked="true/false">`
- Groups: `<fieldset>` + `<legend>` for related consent items
- E-signature: `<form>` with appropriate field roles

### **ARIA Attributes**
- **`aria-required="true"`**: On required consent checkboxes
- **`aria-invalid="true"`**: When validation error is present
- **`aria-describedby`**: Points to error message AND policy description if present
- **`aria-checked`**: On toggle/switch variants
- **`role="switch"`**: For toggle-style consent items
- **`aria-expanded`**: On expandable detail section trigger
- **`aria-live="polite"`**: On error message container for dynamic announcement

### **Link Accessibility**
Policy links within consent text must be accessible:
```tsx
// ✅ Clear destination and context
<a href="/terms" target="_blank" rel="noopener noreferrer"
   aria-label="Terms of Service (opens in new tab)">
  Terms of Service
</a>

// ✅ Screen reader hint for new tab
// Many teams add "(opens in new tab)" as visible or sr-only text
```

- All policy links must open in a new tab (`target="_blank"`) so the user does not lose their form state
- `rel="noopener noreferrer"` required on all `target="_blank"` links
- Link text must be descriptive — "Terms of Service" not "here" or "this document"

### **Keyboard Navigation**
- **Tab**: Move focus to checkbox or toggle
- **Space**: Toggle checked state
- **Tab to link**: Move to policy link within label text
- **Enter**: Follow link (opens in new tab)
- Expandable detail: **Enter/Space** on the disclosure trigger expands/collapses

### **Screen Reader Announcements**
- Unchecked required: "I agree to the Terms of Service and Privacy Policy, required checkbox, unchecked"
- Checked: "I agree to the Terms of Service and Privacy Policy, required checkbox, checked"
- Error: On focus — "I agree to the Terms of Service, required checkbox, unchecked, invalid. You must agree to the Terms of Service to continue."
- Toggle / switch: "Analytics & Performance, switch, off" → "Analytics & Performance, switch, on"
- Marketing opt-in: "Send me product updates, optional checkbox, unchecked"

### **Pre-Check Prohibition**
- Required consent checkboxes must **never** be pre-checked (violates GDPR unbundling requirement)
- Optional marketing consent must **never** be pre-checked
- Only "Strictly Necessary" cookie items may be pre-checked (and should be non-interactive/disabled)

### **Touch Accessibility**
- Minimum touch target: 44×44px (via padding on label)
- Full label area tappable — not just the checkbox control
- Policy links: Minimum 44px height for comfortable tapping

### **Visual Design Rules**
- Do not use color alone to indicate required vs. optional
- Use asterisk (*) on required items AND `aria-required`
- Error state: Red border + red error message text (never color alone)
- Optional label: "(optional)" text adjacent to label text
- Policy links must be visually distinguishable (underlined, brand color)

---

## Legal & Regulatory Guidelines

### **GDPR Compliance (EU)**
- Consent must be: Freely given, Specific, Informed, and Unambiguous (GDPR Article 7)
- Separate consent per purpose: Don't bundle marketing consent with terms acceptance
- Easy withdrawal: Provide a clear path to withdraw consent (settings page, unsubscribe link)
- Record keeping: Store timestamp, policy version, user identifier, and consent state
- No pre-ticked boxes for optional/marketing consent
- Cookie consent: Required for non-essential cookies; necessary cookies exempt

### **CCPA Compliance (California)**
- "Do Not Sell My Personal Information" link required if applicable
- Opt-out mechanism for sale of personal data
- Different from GDPR: CCPA uses opt-out rather than opt-in for most data categories

### **COPPA Compliance (US, Children)**
- Parental consent required for users under 13
- Age gate before showing consent if service may attract minors
- Don't use dark patterns to prevent accurate age reporting

### **CAN-SPAM / CASL (Marketing Email)**
- CAN-SPAM: Requires opt-out; allows implied consent in some cases
- CASL (Canada): Requires explicit opt-in; no pre-checked boxes
- Always describe what communications the user will receive (frequency, type)

### **Record Keeping Requirements**
Every recorded consent should capture:
```typescript
interface ConsentAuditRecord {
  userId: string;                              // Or session ID for anonymous users
  consentId: string;                           // Which consent item
  policyVersion: string;                       // Which version of the terms
  decision: 'granted' | 'denied' | 'withdrawn';
  timestamp: string;                           // ISO 8601: "2026-03-18T14:32:00Z"
  method: 'explicit-checkbox' | 'toggle' | 'button';
  ipAddress: string;                           // Server-side only
  userAgent: string;
  locale: string;                              // User's language/region
  source: string;                              // Which page/flow: "registration"
}
```

---

## Animation Specifications

### **Checkbox Check Animation**
- Identical to Checkbox component
- Stroke-draw: 150ms ease-out
- Background fill: 150ms ease-out

### **Toggle Slide Animation**
- **Target**: translateX of toggle handle
- **Duration**: 200ms ease-out
- **Background**: 200ms ease-out (gray → brand)
  ```css
  .toggle-handle {
    transition: transform 200ms ease-out;
  }
  .toggle-track {
    transition: background-color 200ms ease-out;
  }
  ```

### **Expandable Detail Reveal**
- **Target**: max-height + opacity
- **Duration**: 200ms ease-out
  ```css
  .consent-detail {
    overflow: hidden;
    transition: max-height 200ms ease-out, opacity 200ms ease-out;
  }
  ```

### **Error State Appearance**
- **Target**: Border color + error message opacity
- **Duration**: 150ms ease-out

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  .legal-consent * { transition: none !important; animation: none !important; }
}
```

---

## Design Tokens

### **Colors**
| Purpose | Token | Value |
|---------|-------|-------|
| Required label | `--consent-required-color` | #111827 |
| Optional label | `--consent-optional-color` | #374151 |
| Description | `--consent-description-color` | #6B7280 |
| Link color | `--consent-link-color` | #3B82F6 |
| Link hover | `--consent-link-hover` | #1D4ED8 |
| Required marker | `--consent-required-marker` | #EF4444 |
| Error text | `--consent-error-color` | #EF4444 |
| Error border | `--consent-error-border` | #EF4444 |
| Checked fill | `--consent-checked-fill` | #3B82F6 |
| Toggle track off | `--consent-toggle-off` | #D1D5DB |
| Toggle track on | `--consent-toggle-on` | #3B82F6 |

### **Spacing**
| Property | Value |
|----------|-------|
| Checkbox → label gap | 10px (MD) |
| Label → description gap | 4px |
| Item → item gap | 16px (MD) |
| Item → error message gap | 6px |
| Expandable detail padding | 12px |

---

## UX Copy Guidelines

### **Terms Acceptance**
- ✅ "I agree to the [Terms of Service] and [Privacy Policy]"
- ❌ "Check this box to agree to our legal documents"
- ❌ "By clicking, you agree to our terms" (implied consent — not explicit)

### **Marketing Opt-In**
- ✅ "I'd like to receive product updates, tips, and offers from Acme by email"
- ❌ "Keep me in the loop" (too vague — doesn't specify channel or content)
- ❌ "Uncheck to opt out of marketing" (dark pattern — opt-out framing)

### **Cookie Consent**
- ✅ "Strictly Necessary (Required)" — can't be disabled
- ✅ "Analytics — Help us improve by sharing how you use the site"
- ❌ "Functional cookies" (too vague — explain what they do)

### **Data Processing**
- ✅ "Allow Acme to share anonymized usage data with Mixpanel for product analytics. [Learn more]"
- ❌ "Allow data sharing" (not specific enough)

### **Age Verification**
- ✅ "I confirm I am 18 years of age or older"
- ❌ "I am an adult" (imprecise — varies by jurisdiction)

### **Error Messages**
- ✅ "You must agree to the Terms of Service to create an account."
- ❌ "This field is required." (too generic)
- ❌ "You must check the box." (doesn't explain why)

---

## Common Anti-Patterns to Avoid

| Anti-Pattern | Why It's Wrong | Fix |
|---|---|---|
| Pre-checked marketing consent | Violates GDPR — consent must be freely given | Always default to unchecked |
| Bundled consent (terms + marketing in one checkbox) | Violates GDPR unbundling — each purpose needs separate consent | Separate checkboxes per purpose |
| Implied consent ("By using this site...") | Not explicit — unambiguous action required | Use a checkbox or button |
| Consent buried in T&Cs | Users must be clearly informed | Show consent item prominently at point of collection |
| No policy version recorded | Can't prove what the user agreed to | Store policy version + timestamp |
| "Agree to all" pre-selected | Dark pattern; forces unnecessary consent | Let users choose individually; "Accept all" is acceptable as a button |
| Blocking service for refusing optional consent | Coercion makes consent invalid | Never gate core service on optional consents |
| Unclear unsubscribe path | Legal requirement | Always include unsubscribe link in emails; settings page for managing consent |

---

## Related Components

- **Checkbox**: Base component used inside Legal Consent for the control itself
- **Toggle / Switch**: Used in cookie consent and DPA variant
- **Input**: Used for e-signature typed name
- **Link**: Policy links within consent text
- **Banner**: Cookie consent banner surface
- **Modal**: Privacy preferences modal
- **Form**: Container providing layout and shared validation context

---

## Version History

- **v1.0**: Terms checkbox, marketing opt-in, error state, required enforcement
- **v1.1**: Cookie consent variant with category toggles; LegalConsentGroup
- **v1.2**: DPA / granular consent, expandable detail, audit trail callback
- **v2.0**: E-signature variant, age verification, full regulatory guidelines
- **v2.1**: Toggle variant, accept/reject all, GDPR anti-pattern guidance, reduced motion

---

## Testing Checklist

- [ ] Required consent cannot be pre-checked on initial render
- [ ] Marketing and optional consents default to unchecked
- [ ] Checking a required item enables form submit (if it was the last required item)
- [ ] Unchecking a required item disables form submit
- [ ] Error message appears and is announced when form submitted without required consent
- [ ] `aria-required="true"` on required checkboxes
- [ ] `aria-invalid="true"` set on error state
- [ ] Error message linked via `aria-describedby`
- [ ] Policy links open in new tab with `target="_blank"` and `rel="noopener noreferrer"`
- [ ] Policy link has accessible label mentioning new tab behavior
- [ ] Full label area is clickable/tappable (not just checkbox control)
- [ ] Touch target ≥ 44×44px
- [ ] Toggle variant: `role="switch"` + `aria-checked` toggle correctly
- [ ] Expandable detail expands/collapses on trigger click and keyboard
- [ ] `onConsentRecord` fires with correct metadata (id, timestamp, version, decision)
- [ ] "Strictly Necessary" cookie item is non-interactive (disabled)
- [ ] "Accept all" sets all optional items to true
- [ ] "Reject all" sets all optional items to false (necessary items remain true)
- [ ] Screen reader announces "optional" for optional items
- [ ] Screen reader announces "required" for required items
- [ ] Reduced motion disables all animations
- [ ] E-signature: typed name field required before submit enabled
- [ ] Age verification: blocks continue if unchecked when submitted
