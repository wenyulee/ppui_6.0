# PayPal Design System — Components v6 Beta Documentation

This folder contains comprehensive design-to-code documentation for every component and foundation in the **PayPal Components v6 Beta** design system. Each file is generated from the official Figma source (`qqH85RTAgalgEUUtJahjAB`) and covers anatomy, design tokens, Props API (React/TypeScript), platform APIs (iOS SwiftUI + Android Jetpack Compose), accessibility specifications, and testing checklists.

---

## Contents

### Foundations

Design tokens and semantic scales that underpin every component.

| File | Description |
|------|-------------|
| [Foundations — Base](./Foundations_Base_Documentation.md) | Primitive tokens: spacing, sizing, border radius, motion, elevation, typography ramps |
| [Foundations — Semantic](./Foundations_Semantic_Documentation.md) | Semantic color roles (colorScheme), typography scale (Display → Link), font families |

---

### Components A–Z

| File | Component | Category |
|------|-----------|---------|
| [Accordion](./Accordion_Component_Documentation.md) | Collapsible content sections | Disclosure |
| [Assets](./Assets_Component_Documentation.md) | Profile, SMB, Brand, Crypto & Flag image circles | Media |
| [Avatar](./Avatar_Component_Documentation.md) | User identity circle with initials/image fallback | Identity |
| [Badge](./Badge_Component_Documentation.md) | Numeric / status indicator dot | Feedback |
| [Banner](./Banner_Component_Documentation.md) | Full-width contextual message strip | Feedback |
| [Bottom Navigation](./BottomNavigation_Component_Documentation.md) | Persistent tab bar at screen bottom | Navigation |
| [Button](./Button_Component_Documentation.md) | Primary, secondary, tertiary & danger actions | Actions |
| [Button Group](./Button_Group_Component_Documentation.md) | Stacked or inline button layout wrapper | Actions |
| [Card](./Card_Component_Documentation.md) | Elevated surface container for grouped content | Layout |
| [Checkbox](./Checkbox_Component_Documentation.md) | Multi-select boolean control | Forms |
| [Chips](./Chips_Component_Documentation.md) | Selectable filter / tag pills | Selection |
| [Coachtip](./Coachtip_Component_Documentation.md) | Anchored onboarding tooltip bubble | Onboarding |
| [Contextual Alert](./Contextual_Alert_Component_Documentation.md) | Inline status message (info / warning / error / success) | Feedback |
| [Divider](./Divider_Component_Documentation.md) | Horizontal or vertical separator line | Layout |
| [Dock](./Dock_Component_Documentation.md) | Floating action cluster | Navigation |
| [Empty State](./Empty_State_Component_Documentation.md) | Zero-data placeholder with illustration + CTA | Feedback |
| [Header](./Header_Component_Documentation.md) | Page-level heading block | Layout |
| [Icon](./Icon_Component_Documentation.md) | SVG icon primitive with size/color variants | Media |
| [Image](./Image_Component_Documentation.md) | Responsive image container with aspect ratio | Media |
| [Input](./Input_Component_Documentation.md) | Text field with label, hint, error states | Forms |
| [Legal Consent](./Legal_Consent_Component_Documentation.md) | Terms acceptance checkbox + copy block | Forms |
| [Link](./Link_Component_Documentation.md) | Inline or standalone hyperlink text | Actions |
| [List](./List_Component_Documentation.md) | Rows, sections, and dividers for list layouts | Layout |
| [Loader](./Loader_Component_Documentation.md) | Circular and linear loading indicators | Feedback |
| [Logo](./Logo_Component_Documentation.md) | PayPal Monogram (2 variants) + Wordmark (3 variants) | Brand |
| [Menu](./Menu_Component_Documentation.md) | Dropdown / contextual action menu | Overlay |
| [Modal](./Modal_Component_Documentation.md) | Centered dialog overlay with backdrop | Overlay |
| [Pagination](./Pagination_Component_Documentation.md) | Page navigation controls | Navigation |
| [Payment Card](./Payment_Card_Component_Documentation.md) | Credit/debit card display tile | Finance |
| [Progress Bar](./Progress_Bar_Component_Documentation.md) | Linear progress / step indicator | Feedback |
| [Radio](./Radio_Component_Documentation.md) | Single-select radio button control | Forms |
| [Search](./Search_Component_Documentation.md) | Search input with clear and results affordance | Forms |
| [Segmented Control](./Segmented_Control_Component_Documentation.md) | Mutually-exclusive option selector | Selection |
| [Select](./Select_Component_Documentation.md) | Dropdown select / picker field | Forms |
| [Selection Card](./Selection_Card_Component_Documentation.md) | Tappable card as a selection option | Selection |
| [Shimmer](./Shimmer_Component_Documentation.md) | Skeleton loading placeholder animation | Feedback |
| [Slider](./Slider_Component_Documentation.md) | Range / continuous value input | Forms |
| [Switch](./Switch_Component_Documentation.md) | Boolean toggle control | Forms |
| [Tabs](./Tabs_Component_Documentation.md) | Horizontal tab bar with panel switching | Navigation |
| [Timeline](./Timeline_Component_Documentation.md) | Vertical step/event history layout | Layout |
| [Toast](./Toast_Component_Documentation.md) | Ephemeral auto-dismiss notification | Feedback |
| [Tooltip](./Tooltip_Component_Documentation.md) | Hover/focus label for icon affordances | Overlay |
| [Top Navigation](./Top_Navigation_Component_Documentation.md) | 56px app top bar with three-slot layout | Navigation |
| [Web View](./Web_View_Component_Documentation.md) | In-app browser chrome (TopNav + Loader + Toolbar) | Navigation |

---

### Utilities

Internal building blocks composed into higher-level components.

| File | Utilities Covered |
|------|------------------|
| [Utilities](./Utilities_Documentation.md) | Slot (single / vertical / horizontal), Text, Chevron, Selected, Close, StateLayer (Base + Emphasis) |

---

### Reports

| File | Description |
|------|-------------|
| [Design System Audit Report](./Design_System_Audit_Report.md) | Cross-component token usage audit, inconsistency flags, and remediation recommendations |

---

## Document Structure

Every component file follows this standard structure:

1. **Overview** — purpose, when to use / not use
2. **Anatomy** — ASCII diagram of sub-elements
3. **Size & Spacing Specifications** — exact px values with token references
4. **Style Variants** — all visual variants (color, size, shape)
5. **State Specifications** — default, hover, pressed, focused, disabled, loading, error
6. **Props API** — full TypeScript interface + React component signature
7. **Code Examples** — React/TypeScript implementation + CSS custom properties
8. **Platform APIs** — iOS SwiftUI and Android Jetpack Compose equivalents
9. **Accessibility Specifications** — ARIA roles, keyboard navigation, WCAG 2.1 AA criteria
10. **Animation Specifications** — duration, easing, enter/exit transitions
11. **Design Tokens** — all token names, resolved values, and CSS custom properties
12. **Best Practices** — do / don't guidance
13. **Related Components** — cross-references
14. **Version History** — changelog since v6 Beta
15. **Testing Checklist** — visual, functional, accessibility, and platform checks

---

## Token Namespaces

| Namespace | Scope |
|-----------|-------|
| `tokens.base.*` | Primitive values — spacing, sizing, border-radius, motion, elevation, font ramps |
| `tokens.colorScheme.*` | Semantic color roles — content, background, border × state × emphasis |
| `tokens.brand.text.*` | Semantic typography — font families, weights per role (Display, Heading, Title, Label, Body, Link) |
| `tokens.responsive.*` | Breakpoint-aware values (Heading size scale) |

### Key Token Quick-Reference

| Token | Value |
|-------|-------|
| `tokens.colorScheme.content.link` | `#0066f5` |
| `tokens.colorScheme.background.utility.emphasis` | `#000000` |
| `tokens.colorScheme.border.focus` | `#0066f5` |
| `tokens.colorScheme.background.states.base.hover` | `rgba(5,55,130,0.04)` |
| `tokens.colorScheme.background.states.base.pressed` | `rgba(5,55,130,0.08)` |
| `tokens.colorScheme.background.states.emphasis.hover` | `rgba(255,255,255,0.24)` |
| `tokens.colorScheme.background.states.emphasis.pressed` | `rgba(255,255,255,0.31)` |
| `tokens.base.motion.duration.toast` | `5000ms` |

---

## Platform Coverage

| Platform | Framework | Notes |
|----------|-----------|-------|
| Web | React + TypeScript | Primary; all components covered |
| Web | CSS Custom Properties | Token-mapped CSS for all components |
| iOS | SwiftUI | Code Connect references throughout |
| Android | Jetpack Compose | Code Connect references throughout |

**iOS Code Connect references:** `PDSAvatar`, `PDSSwitch`, `PDSIcon`, `ContactCardView.swift`
**Android Code Connect references:** `TopBar.kt`, `Avatar.kt`, `List.kt`

---

## Figma Source

**File:** Components v6 – Beta
**File Key:** `qqH85RTAgalgEUUtJahjAB`
**URL:** `https://www.figma.com/design/qqH85RTAgalgEUUtJahjAB/Components-v6--Beta-`

---

*Generated from Figma Components v6 Beta — March 2026*
