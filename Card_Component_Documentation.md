# Card Component Documentation

## Overview

The Card component is a flexible container for displaying grouped content with visual separation and optional interactive features. Cards organize related information into discrete, scannable units while providing consistent styling, spacing, and interaction patterns across the application. Cards are foundational to modern UI design and serve as the primary way to present content in grid layouts, feed-style interfaces, dashboards, and detail views.

---

## Component Variants

Card supports five primary usage patterns:

### 1. **Basic Card**
- **Purpose**: Simple content container with consistent styling and spacing
- **Use Cases**: Product listings, article previews, team member profiles, dashboard widgets
- **Features**: Shadow, rounded corners, padding, neutral background
- **Behavior**: Static display; optional click interaction
- **Content**: Title, description, image, metadata, actions

### 2. **Interactive Card**
- **Purpose**: Card with built-in click handling and hover feedback
- **Use Cases**: Clickable product cards, navigable team profiles, selectable options, link destinations
- **Features**: Hover effects, cursor feedback, active state, navigation
- **Behavior**: Responds to clicks; navigates to detail page or triggers action
- **Accessibility**: Semantic link wrapper; keyboard accessible; proper focus states

### 3. **Elevated Card**
- **Purpose**: Card with increased visual prominence through shadow and depth
- **Use Cases**: Featured content, promoted products, highlighted sections, important alerts
- **Features**: Larger shadow, higher z-index, visual distinction from basic cards
- **Behavior**: Draws attention; typically limited use to maintain visual hierarchy
- **Elevation**: Elevation 4+ shadows create floating effect

### 4. **Outlined Card**
- **Purpose**: Card with subtle border instead of shadow for lighter visual weight
- **Use Cases**: Dividers between sections, secondary content, form groups, conditional displays
- **Features**: Border stroke (1px), minimal shadow, lighter appearance
- **Behavior**: Deemphasized visual treatment; focuses on content over container
- **Contrast**: Maintains readability with lower visual weight

### 5. **Flat Card**
- **Purpose**: Card with minimal styling; background only with subtle distinction
- **Use Cases**: Content grids with high density, minimal designs, modern aesthetic
- **Features**: Flat background color, no shadow, minimal border, clean appearance
- **Behavior**: Content-centric design; container is secondary visual element
- **Spacing**: Relies on padding and layout to define card boundaries

---

## Size Variants

Card respects flexible sizing patterns:

| Size | Width | Height | Padding | Use Case |
|------|-------|--------|---------|----------|
| **SM** | 240-280px | Auto | 12px | Compact grids, small thumbnails, dense layouts |
| **MD** | 300-360px | Auto | 16px | **Default**, standard product cards, team profiles |
| **LG** | 400-480px | Auto | 20px | Featured content, detailed previews, focal points |
| **XL** | 500-640px | Auto | 24px | Hero sections, primary content, full attention |
| **Full** | 100% | Auto | 16px-24px | Single column, full-width displays, detail views |

---

## Style Variants

### **Solid**
- **Description**: Filled background with shadow for depth
- **Best For**: Primary card layout, emphasis on content
- **Background**: White (#FFFFFF) or light gray (#F9FAFB)
- **Shadow**: Elevation 2 default, Elevation 4 on hover
- **Border**: None or very subtle divider
- **Use When**: Presenting important content that needs visual separation

### **Outlined**
- **Description**: Bordered card with minimal shadow
- **Best For**: Secondary content, visual grouping without emphasis
- **Background**: Transparent or very light tint
- **Border**: 1px solid gray (#D1D5DB)
- **Shadow**: None or Elevation 1
- **Use When**: Multiple cards at same visual hierarchy level

### **Elevated**
- **Description**: High-shadow card for maximum depth perception
- **Best For**: Featured content, floating elements, modals
- **Background**: White with subtle tint
- **Shadow**: Elevation 8+ for prominent floating effect
- **Border**: None
- **Use When**: Card needs to appear above other content

### **Flat**
- **Description**: Minimal styling with background color differentiation only
- **Best For**: Dense content layouts, modern minimal design
- **Background**: Subtle color (#F3F4F6) to differentiate from page background
- **Shadow**: None
- **Border**: None or 1px light border
- **Use When**: Card distinction should be subtle; content is primary focus

### **Ghost**
- **Description**: Transparent background with border or underline only
- **Best For**: Lightweight grouping, dividers, subtle separation
- **Background**: Transparent
- **Border**: Bottom border (#E5E7EB) or full border
- **Shadow**: None
- **Use When**: Minimal visual weight needed; card is secondary to content

---

## Anatomy

A complete Card structure includes these optional sections:

```
┌─────────────────────────────────────┐
│  [Image] or [Cover]                 │
├─────────────────────────────────────┤
│  [Badge] Title / Heading             │
│  Subtitle or Meta Information        │
├─────────────────────────────────────┤
│  Card Body / Content Section         │
│  • Supporting text                   │
│  • Description paragraph             │
│  • Additional information            │
├─────────────────────────────────────┤
│  [Action buttons] [Menu]             │
└─────────────────────────────────────┘
```

### **Sections Breakdown**

**Media Section** (Optional)
- Image, video cover, or illustration
- Aspect ratios: 16:9, 1:1, 4:3 (configurable)
- Full-width; sits at top of card
- Overlay gradient for text readability (if needed)

**Header Section** (Optional)
- Badge (status, category, tag)
- Title/Heading (primary content label)
- Subtitle or meta information (date, author, category)
- Consistent padding and typography

**Body Section** (Optional)
- Main content area
- Description, details, or additional information
- Text, lists, or mixed content
- Consistent padding; no nested card styling

**Footer Section** (Optional)
- Action buttons (primary, secondary actions)
- Menu (options, links, overflow)
- Footer text (additional meta, disclaimers)
- Divider line separating from body

---

## State Specifications

### **Default State**
- Background: White (#FFFFFF) or light gray (#F9FAFB)
- Shadow: Elevation 2 (0 2px 4px rgba(0,0,0,0.1))
- Border: None or subtle divider
- Cursor: default (unless interactive)
- Opacity: 100%
- Interaction: None (unless specified)

### **Hover State** (Interactive Cards)
- Background: Slightly darker (#F3F4F6)
- Shadow: Elevation 4 (0 4px 12px rgba(0,0,0,0.15))
- Transform: translateY(-2px) for subtle lift
- Cursor: pointer
- Duration: 200ms ease-out
- All children maintain opacity

### **Active/Selected State**
- Background: Brand color tint (#EBF8FF - light blue)
- Border: 2px solid brand color (#3B82F6)
- Shadow: Elevation 4 with colored glow
- Outline: Removed (border provides focus)
- Indicator: Optional checkmark overlay
- Duration: 150ms ease-out

### **Focus State** (Keyboard Navigation)
- Outline: 2px solid brand color
- Outline Offset: 2px
- Duration: 100ms ease-out
- Visible on keyboard Tab navigation
- Maintained on click for accessibility

### **Disabled State**
- Background: Gray 100 (#F3F4F6)
- Text Color: Gray 400 (#9CA3AF)
- Shadow: None
- Opacity: 50-60%
- Cursor: not-allowed
- Pointer Events: none

### **Loading State**
- Background: Fades to 70% opacity
- Skeleton: Placeholder shapes for content areas
- Spinner: Center-aligned loader (optional)
- Cursor: wait
- Duration: Continuous pulse or spin
- Interaction: Disabled until loading completes

### **Error State**
- Border: 2px solid danger color (#EF4444)
- Background: Light error tint (#FEF2F2)
- Icon: Error indicator (exclamation, X)
- Text: Error message or description
- Shadow: Normal or elevated
- Interaction: May show retry action

---

## Props API

### **Card (Container)**

```typescript
interface CardProps {
  // Content & Children
  children: React.ReactNode;                   // Card content

  // Appearance
  variant?: 'solid' | 'outlined' | 'elevated' | 'flat' | 'ghost'; // Style variant
  size?: 'sm' | 'md' | 'lg' | 'xl' | 'full';  // Size variant
  shadow?: 'none' | 'sm' | 'md' | 'lg' | 'xl'; // Shadow intensity

  // Layout
  fullWidth?: boolean;                         // Expand to container width
  horizontal?: boolean;                        // Horizontal layout (image left)

  // Interactive
  onClick?: (event: React.MouseEvent) => void; // Click handler
  href?: string;                               // Navigation destination
  selectable?: boolean;                        // Enable selection mode
  selected?: boolean;                          // Selection state
  interactive?: boolean;                       // Enable hover effects

  // States
  disabled?: boolean;                          // Disable interaction
  loading?: boolean;                           // Show loading state
  error?: boolean;                             // Show error state

  // Accessibility
  as?: 'div' | 'a' | 'button' | 'article';    // HTML element type
  role?: string;                               // ARIA role
  aria-label?: string;                         // Accessible label
  aria-describedby?: string;                   // Description reference

  // Styling
  className?: string;                          // Custom CSS classes
  style?: React.CSSProperties;                 // Inline styles

  // Events
  onFocus?: () => void;                        // Focus event
  onBlur?: () => void;                         // Blur event
  onMouseEnter?: () => void;                   // Hover enter
  onMouseLeave?: () => void;                   // Hover leave
}
```

### **Card Subcomponents**

```typescript
interface CardImageProps {
  src: string;                                 // Image source URL
  alt: string;                                 // Alt text for accessibility
  aspectRatio?: '16:9' | '1:1' | '4:3' | 'auto'; // Image ratio
  objectFit?: 'cover' | 'contain' | 'fill';   // Image fit behavior
  height?: string | number;                   // Custom height
  overlay?: boolean;                           // Add dark overlay
  overlayGradient?: boolean;                   // Gradient overlay for text
}

interface CardHeaderProps {
  title?: string | React.ReactNode;            // Card title
  subtitle?: string;                           // Subtitle text
  badge?: string;                              // Badge content
  badgeColor?: string;                         // Badge color variant
  action?: React.ReactNode;                    // Action element (menu, button)
}

interface CardBodyProps {
  children: React.ReactNode;                   // Body content
  padding?: 'sm' | 'md' | 'lg';               // Custom padding
}

interface CardFooterProps {
  children: React.ReactNode;                   // Footer content
  divider?: boolean;                           // Show top divider
  action?: React.ReactNode;                    // Action buttons
  align?: 'left' | 'center' | 'right';        // Content alignment
}
```

---

## Code Examples

### **1. Basic Product Card**
```typescript
import { Card, CardImage, CardHeader, CardBody, CardFooter } from '@components/card';
import { Star } from 'lucide-react';

export function ProductCard() {
  return (
    <Card variant="solid" size="md">
      <CardImage
        src="https://images.example.com/product.jpg"
        alt="Product"
        aspectRatio="16:9"
      />
      <CardHeader
        title="Premium Wireless Headphones"
        subtitle="Electronics"
        badge="New"
        badgeColor="success"
      />
      <CardBody>
        High-quality sound with noise cancellation and 30-hour battery life.
      </CardBody>
      <CardFooter align="center">
        <div className="flex items-center gap-2">
          <Star size={16} fill="currentColor" />
          <span>4.8 (124 reviews)</span>
        </div>
      </CardFooter>
    </Card>
  );
}
```

### **2. Interactive Selectable Card**
```typescript
import { Card, CardHeader, CardBody } from '@components/card';
import { Check } from 'lucide-react';
import { useState } from 'react';

export function SelectableCard() {
  const [selected, setSelected] = useState(false);

  return (
    <Card
      selectable
      selected={selected}
      interactive
      onClick={() => setSelected(!selected)}
    >
      <CardHeader title="Option A" />
      <CardBody>
        This is a selectable card option. Click to toggle selection.
      </CardBody>
      {selected && (
        <div className="absolute top-2 right-2">
          <Check size={20} className="text-green-600" />
        </div>
      )}
    </Card>
  );
}
```

### **3. Team Member Profile Card**
```typescript
import { Card, CardImage, CardHeader, CardBody, CardFooter } from '@components/card';
import { Mail, Linkedin } from 'lucide-react';
import { Button } from '@components/button';

export function TeamMemberCard() {
  return (
    <Card variant="solid" size="md">
      <CardImage
        src="https://images.example.com/avatar.jpg"
        alt="Team member"
        aspectRatio="1:1"
      />
      <CardHeader
        title="Sarah Johnson"
        subtitle="Product Designer"
        badge="Lead"
        badgeColor="primary"
      />
      <CardBody>
        Creative problem-solver with 5+ years of UX design experience.
      </CardBody>
      <CardFooter align="center" divider>
        <div className="flex gap-2">
          <Button
            size="sm"
            variant="ghost"
            icon={<Mail size={16} />}
            aria-label="Send email"
          />
          <Button
            size="sm"
            variant="ghost"
            icon={<Linkedin size={16} />}
            aria-label="Visit LinkedIn"
          />
        </div>
      </CardFooter>
    </Card>
  );
}
```

### **4. Article Preview Card**
```typescript
import { Card, CardImage, CardHeader, CardBody, CardFooter } from '@components/card';
import { Calendar, User, ArrowRight } from 'lucide-react';
import { Button } from '@components/button';

export function ArticleCard() {
  return (
    <Card variant="solid" size="lg" interactive onClick={() => {}}>
      <CardImage
        src="https://images.example.com/article.jpg"
        alt="Article"
        aspectRatio="16:9"
        overlayGradient
      />
      <CardHeader
        title="The Future of Design Systems"
        badge="Design"
        badgeColor="info"
      />
      <CardBody>
        Exploring how AI and automation are transforming the way we build and
        maintain design systems at scale.
      </CardBody>
      <CardFooter align="right" divider>
        <div className="flex items-center gap-4 text-sm text-gray-600">
          <div className="flex items-center gap-1">
            <Calendar size={14} />
            <span>Mar 15, 2026</span>
          </div>
          <div className="flex items-center gap-1">
            <User size={14} />
            <span>By Alex Chen</span>
          </div>
          <Button
            size="sm"
            variant="ghost"
            icon={<ArrowRight size={16} />}
            aria-label="Read article"
          />
        </div>
      </CardFooter>
    </Card>
  );
}
```

### **5. Loading State Card**
```typescript
import { Card, CardImage, CardHeader, CardBody } from '@components/card';

export function LoadingCard() {
  return (
    <Card variant="solid" size="md" loading>
      <div className="animate-pulse space-y-4">
        <div className="bg-gray-300 h-40 rounded-lg" />
        <div className="space-y-2">
          <div className="bg-gray-300 h-6 rounded w-3/4" />
          <div className="bg-gray-300 h-4 rounded w-1/2" />
        </div>
        <div className="space-y-2">
          <div className="bg-gray-300 h-4 rounded" />
          <div className="bg-gray-300 h-4 rounded w-5/6" />
        </div>
      </div>
    </Card>
  );
}
```

### **6. Elevated Feature Card**
```typescript
import { Card, CardImage, CardHeader, CardBody, CardFooter } from '@components/card';
import { Zap } from 'lucide-react';
import { Button } from '@components/button';

export function FeatureCard() {
  return (
    <Card variant="elevated" size="lg">
      <CardImage
        src="https://images.example.com/feature.jpg"
        alt="Feature"
        aspectRatio="16:9"
      />
      <CardHeader
        title="Premium Features"
        badge="Featured"
        badgeColor="warning"
      />
      <CardBody>
        Unlock powerful tools to boost your productivity and collaboration.
      </CardBody>
      <CardFooter align="right">
        <Button
          color="primary"
          size="md"
          icon={<Zap size={16} />}
          label="Upgrade Now"
        />
      </CardFooter>
    </Card>
  );
}
```

### **7. Outlined Secondary Card**
```typescript
import { Card, CardHeader, CardBody } from '@components/card';

export function SecondaryCard() {
  return (
    <Card variant="outlined" size="md">
      <CardHeader title="Related Information" />
      <CardBody>
        This card uses outlined styling for secondary content that needs visual
        grouping without emphasis.
      </CardBody>
    </Card>
  );
}
```

### **8. Horizontal Card Layout**
```typescript
import { Card, CardImage, CardHeader, CardBody, CardFooter } from '@components/card';
import { Button } from '@components/button';

export function HorizontalCard() {
  return (
    <Card variant="solid" size="full" horizontal>
      <CardImage
        src="https://images.example.com/thumb.jpg"
        alt="Thumbnail"
        aspectRatio="1:1"
        height="200px"
      />
      <div className="flex flex-col flex-1">
        <CardHeader title="Horizontal Layout Card" subtitle="With image on left" />
        <CardBody>
          This card uses horizontal layout with the image positioned to the left
          of the content.
        </CardBody>
        <CardFooter align="right">
          <Button color="primary" size="sm" label="Action" />
        </CardFooter>
      </div>
    </Card>
  );
}
```

---

## Accessibility Specifications

### **ARIA Attributes**
- **role="article"**: Default semantic role for content cards
- **role="link"**: When card wraps navigation element
- **role="button"**: When card is clickable action
- **aria-label**: Required for icon-only or image-only cards
- **aria-describedby**: Links to extended description if needed
- **aria-selected**: Indicates selection state in selectable cards
- **aria-disabled**: Applied when card is disabled
- **aria-busy="true"**: Applied during loading states

### **Keyboard Navigation**
- **Tab**: Move focus to card (if interactive/selectable)
- **Shift+Tab**: Move focus to previous interactive element
- **Enter/Space**: Activate card click handler or navigation
- **Escape**: Cancel selection mode (if applicable)
- **Arrow Keys**: Navigate between cards in a group (optional implementation)

### **Focus Management**
- Focus ring visible on all interactive cards (2px solid, 2px offset)
- Focus persists through interactions for keyboard users
- Focus trap prevented; focus moves to next/previous element appropriately
- Focus indicators use sufficient contrast (3:1 minimum)

### **Screen Reader Support**
- Card heading announced clearly: "Product Card, article"
- Badge content announced: "New, badge"
- Card purpose announced if aria-label provided: "Product card for Premium Headphones"
- Image alt text announced: "Product photo"
- Button states announced: "View details, button"
- Loading state announced: "Loading, please wait"

### **Touch Accessibility**
- Minimum touch target: 44×44px (for buttons/actions within card)
- Adequate padding around interactive elements (8px minimum)
- Focus indicators visible on touch focus (for devices supporting it)
- Long-press tooltips for icon-only actions (2-3 second delay)

### **Color Contrast**
- Text-to-background: 4.5:1 minimum (WCAG AA)
- Badge to background: 3:1 minimum
- Border to background: 3:1 minimum
- Does not rely on color alone to convey state (always uses shape/pattern change)

### **Best Practices**
- Always provide semantic HTML element (article, div, a, button)
- Use meaningful heading hierarchy (h1, h2, h3)
- Include alt text for all images (never use alt="")
- Label interactive elements clearly (buttons, links)
- Test with screen readers (NVDA, JAWS, VoiceOver)
- Maintain logical tab order matching visual layout

---

## Animation Specifications

### **Hover Animation** (Interactive Cards)
- **Target**: Transform, shadow, background
- **Duration**: 200ms
- **Easing**: ease-out (cubic-bezier(0.4, 0, 0.2, 1))
- **Transform**: translateY(-2px) for subtle lift
- **Properties**:
  ```css
  transform: 200ms ease-out;
  box-shadow: 200ms ease-out;
  background-color: 200ms ease-out;
  ```

### **Selection Animation**
- **Target**: Border, background, shadow
- **Duration**: 150-200ms
- **Easing**: ease-out
- **Properties**:
  ```css
  border-color: 150ms ease-out;
  background-color: 150ms ease-out;
  box-shadow: 150ms ease-out;
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
- **Target**: Opacity pulse, skeleton shimmer
- **Duration**: 1.5s per cycle
- **Easing**: ease-in-out
- **Keyframes**: 0% opacity(1) → 50% opacity(0.7) → 100% opacity(1)
- **Properties**:
  ```css
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.7; }
  }
  animation: pulse 1.5s ease-in-out infinite;
  ```

### **Entrance Animation** (Optional)
- **Target**: Opacity, transform
- **Duration**: 300ms
- **Easing**: ease-out
- **Used for**: Cards appearing in grid on initial load
- **Properties**:
  ```css
  opacity: 0 → 1;
  transform: scale(0.95) → scale(1);
  ```

---

## Design Tokens

### **Colors**
| Purpose | Token | Value |
|---------|-------|-------|
| Background | `--color-card-bg` | #FFFFFF |
| Background Secondary | `--color-card-bg-secondary` | #F9FAFB |
| Border | `--color-card-border` | #D1D5DB |
| Text Primary | `--color-text-primary` | #1F2937 |
| Text Secondary | `--color-text-secondary` | #6B7280 |
| Hover Background | `--color-card-hover` | #F3F4F6 |
| Selected | `--color-card-selected` | #EBF8FF |
| Selected Border | `--color-card-selected-border` | #3B82F6 |

### **Spacing**
| Size | Value | Use Case |
|------|-------|----------|
| **XS** | 8px | Tight spacing between elements |
| **SM** | 12px | Internal card padding (small) |
| **MD** | 16px | **Default** internal card padding |
| **LG** | 20px | Large card padding, section spacing |
| **XL** | 24px | Extra large padding, major sections |

### **Shadows**
| Level | Value | Use Case |
|-------|-------|----------|
| **None** | none | Flat variant |
| **Sm** | 0 1px 2px rgba(0,0,0,0.05) | Outlined variant |
| **Md** | 0 2px 4px rgba(0,0,0,0.1) | **Default** solid cards |
| **Lg** | 0 4px 12px rgba(0,0,0,0.15) | Hover state, elevated |
| **Xl** | 0 8px 24px rgba(0,0,0,0.12) | Floating, very elevated |

### **Border Radius**
| Shape | Value | Use Case |
|-------|-------|----------|
| **Small** | 4px | Compact cards |
| **Medium** | 8px | **Default** cards |
| **Large** | 12px | Large featured cards |
| **Full** | 9999px | Pill-shaped cards |

### **Typography**
| Element | Font Size | Font Weight | Line Height |
|---------|-----------|------------|-------------|
| **Title** | 18px | 600 | 28px |
| **Subtitle** | 14px | 500 | 20px |
| **Body** | 14px | 400 | 20px |
| **Caption** | 12px | 400 | 16px |

---

## Best Practices

### **Content Guidelines**
- Keep titles concise (1-3 words when possible)
- Use meaningful images (not decorative filler)
- Summarize content; let users click for full details
- Limit body text to 2-3 sentences
- Use hierarchical text sizing (title > subtitle > body)
- Group related information together

### **Visual Design**
- Maintain consistent card dimensions within a grid
- Use even spacing between cards (12-16px minimum)
- Align cards to grid; avoid misaligned content
- Keep internal padding consistent (16px default)
- Use shadow consistently (all solid variant cards match)
- Limit shadows to 2-3 elevation levels

### **Interaction Patterns**
- Make interactive cards visually distinct (outlined style shows interactivity)
- Provide clear hover feedback (lift effect, shadow change)
- Use cursor pointer for clickable cards
- Show focus ring on keyboard navigation
- Disable entire card (not individual elements) when loading
- Show error state with border color change

### **Layout Considerations**
- Horizontal layout: Image on left (150-200px width), content on right
- Vertical layout: Image on top, content below
- Responsive: Single column on mobile (size: full)
- Grid: 3-4 columns on desktop, 2 on tablet, 1 on mobile
- Card spacing: Consistent gutters (12-16px)
- Consider image aspect ratio consistency

### **Accessibility**
- Always include alt text for images (never alt="")
- Use semantic HTML (article, div, a, button)
- Label interactive elements clearly
- Maintain heading hierarchy (h1, h2, h3)
- Test with screen readers regularly
- Ensure 4.5:1 text contrast minimum

### **Performance**
- Lazy-load card images for large grids
- Use image optimization (webp, srcset)
- Limit animations to hover/focus states
- Debounce click handlers for rapid interactions
- Consider virtual scrolling for 50+ cards
- Monitor Core Web Vitals (LCP, CLS)

### **Mobile Optimization**
- Use full-width cards on small screens
- Increase touch target size for actions (44×44px)
- Simplify content for mobile (shorter text)
- Stack footer buttons vertically on small screens
- Test with thumb reach patterns
- Consider notches and safe areas

---

## Component Structure

```
Card
├── CardImage (optional)
│   └── Image + Overlay (optional)
├── CardHeader (optional)
│   ├── Badge (optional)
│   ├── Title
│   ├── Subtitle (optional)
│   └── Action (optional)
├── CardBody (optional)
│   └── Content
└── CardFooter (optional)
    ├── Divider (optional)
    └── Actions
```

---

## Related Components

- **Container**: Generic container without card styling
- **Panel**: Similar to card; used for form groups
- **Modal**: Card in a modal dialog context
- **Dialog**: Similar card-based component for alerts
- **List**: Multiple cards in a vertical list layout
- **Grid**: Multiple cards in a responsive grid layout
- **Tab**: Card-like content container for tabbed interfaces
- **Drawer**: Side panel version of card layout

---

## Migration Guide

### **From Custom Cards to Card Component**
```typescript
// Before: Custom styled div
<div className="rounded-lg shadow-md p-4 bg-white">
  <h3>{title}</h3>
  <p>{content}</p>
</div>

// After: Card component
<Card variant="solid" size="md">
  <CardHeader title={title} />
  <CardBody>{content}</CardBody>
</Card>
```

### **From Link-Wrapper to Interactive Card**
```typescript
// Before: Link wrapping custom card
<a href="/products/123">
  <div className="rounded-lg shadow-md p-4">
    {content}
  </div>
</a>

// After: Interactive card with href
<Card href="/products/123" interactive>
  <CardImage src={image} alt="Product" />
  <CardHeader title={title} />
  <CardBody>{content}</CardBody>
</Card>
```

---

## Responsive Behavior

### **Breakpoints**
- **Mobile** (< 640px): Single column, full-width cards, simplified footer
- **Tablet** (640px - 1024px): 2-column grid, full-width cards on small devices
- **Desktop** (> 1024px): 3-4 column grid, fixed-width cards (MD/LG sizes)

### **Responsive Properties**
- Card width: Auto-scales to container width
- Image aspect ratio: Maintained responsively
- Padding: Reduces on mobile (12px vs 16px)
- Footer: Stack vertically on small screens
- Typography: Scale responsively (18px → 16px on mobile)

---

## Version History

- **v1.0**: Initial release with solid/outlined variants, basic structure
- **v1.1**: Added elevated and flat variants, loading states
- **v1.2**: Improved accessibility, added horizontal layout, focus management
- **v2.0**: Complete refactor with subcomponents (CardImage, CardHeader, etc.)
- **v2.1**: Added selection mode, improved mobile responsiveness
- **v2.2**: Enhanced animations, RTL support, error states

---

## Changelog

### **Latest Updates**
- Added CardImage with overlay and gradient support
- Improved focus ring visibility and accessibility
- Enhanced hover animations with translateY effect
- Added loading state with skeleton support
- Better responsive behavior on mobile devices
- Improved TypeScript typing for all props

---

## FAQ

**Q: Should I use Card or Panel?**
A: Use Card for content presentation and grouping. Use Panel for form groups and layout containers.

**Q: Can I nest cards inside cards?**
A: Generally not recommended. Use nested sections within card body instead.

**Q: How do I handle long titles or descriptions?**
A: Use text truncation with ellipsis. Limit body text to 2-3 sentences; let click action reveal full content.

**Q: Can I make a card a form submission target?**
A: Yes, use onClick handler and form submission logic. For checkbox-style selection, use selectable prop.

**Q: What's the best image aspect ratio for cards?**
A: 16:9 for articles, 1:1 for products, 4:3 for general content. Keep consistent within a grid.

**Q: How do I handle empty state in cards?**
A: Show placeholder skeleton during loading, then display error message or empty state content if no data.

**Q: Can I use card in a modal?**
A: Yes, cards work well inside modals for focused content presentation.

**Q: Should I use outlined or solid variant?**
A: Use solid for primary content, outlined for secondary grouping. Use elevated for featured/promoted content.

---

## Examples by Use Case

### **E-commerce Product Grid**
- Size: MD, variant: Solid
- Image: 16:9 aspect ratio
- Badge: "New", "Sale", "Limited"
- Footer: Price, rating, add to cart button

### **Blog Article List**
- Size: LG, variant: Solid
- Image: 16:9 with gradient overlay
- Badge: Category or author
- Body: Article excerpt
- Footer: Date, author, read more link

### **Team Member Directory**
- Size: SM, variant: Solid
- Image: 1:1 aspect ratio (avatar)
- Title: Name, subtitle: Role/Title
- Body: Short bio or department
- Footer: Contact buttons (email, LinkedIn)

### **Dashboard Widget**
- Size: MD, variant: Flat or Outlined
- No image; focus on data
- Title: Widget name/metric
- Body: Data visualization or key metrics
- Footer: View details link or action

### **Feature Comparison**
- Size: LG, variant: Outlined
- No image; content-focused
- Title: Feature name
- Body: Description, bullet points
- Badge: "Popular" on recommended option

### **Settings/Preferences Card**
- Size: Full, variant: Outlined
- Horizontal layout with toggle/switch
- Title: Setting name
- Subtitle: Description
- Interactive toggle on right side

---

## Testing Checklist

- [ ] Default state renders correctly
- [ ] All variants display as designed (solid, outlined, elevated, flat, ghost)
- [ ] All size variants responsive and properly scaled
- [ ] Hover state works and animations smooth
- [ ] Focus ring visible on keyboard Tab
- [ ] Click handler fires correctly
- [ ] Navigation (href) works with router
- [ ] Loading state shows skeleton/spinner
- [ ] Disabled state prevents interaction
- [ ] Error state displays properly with border/color
- [ ] Image displays with correct aspect ratio
- [ ] Badge renders and colors apply
- [ ] Footer buttons functional and styled
- [ ] Responsive layout works on mobile (single column)
- [ ] Screen reader announces content correctly
- [ ] Touch targets meet 44×44px minimum
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Animations perform at 60fps
- [ ] RTL layout works correctly
- [ ] Works in all modern browsers
