# Image Component Documentation

## Overview

The Image component is a standardized wrapper around the native `<img>` element that enforces consistent aspect ratios, loading behavior, fallback states, overlay support, and accessibility requirements. Raw `<img>` tags scattered across a codebase lead to inconsistent sizing, missing alt text, no loading states, and broken fallbacks. The Image component solves all of this in one place — providing a single API for every image context in the application, from small thumbnails to full-bleed hero banners.

---

## Component Variants

Image supports six primary usage patterns:

### 1. **Standard Image**
- **Purpose**: General-purpose image with constrained dimensions and consistent behavior
- **Use Cases**: Article thumbnails, product photos, team member portraits, gallery items
- **Features**: Aspect ratio lock, object-fit control, fallback, loading skeleton
- **Behavior**: Loads progressively; shows skeleton until loaded; shows fallback on error

### 2. **Avatar Image**
- **Purpose**: Circular or rounded user photo at small sizes
- **Use Cases**: Comment threads, user lists, navigation, message headers
- **Features**: Circle or rounded-square shape, initials fallback, status indicator support
- **Behavior**: Falls back to initials or icon if src fails to load

### 3. **Cover / Hero Image**
- **Purpose**: Full-width or large image that fills a container, often with overlay text
- **Use Cases**: Page heroes, card covers, feature banners, article headers
- **Features**: Full-width fill, gradient overlay, text overlay slot, focal point control
- **Behavior**: `object-fit: cover` by default; maintains visual center on resize

### 4. **Gallery / Grid Image**
- **Purpose**: Uniform image cells in a grid or masonry layout
- **Use Cases**: Photo gallery, product grid, portfolio grid
- **Features**: Enforced aspect ratio per cell, hover overlay, selection state
- **Behavior**: All cells same aspect ratio; hover reveals actions or caption

### 5. **Thumbnail**
- **Purpose**: Small compact image preview, often with a play or expand affordance
- **Use Cases**: Video thumbnails, file previews, attachment previews, search results
- **Features**: Fixed small size, overlay icon (play, expand), badge, caption
- **Behavior**: Click triggers full-size view or playback

### 6. **Zoomable / Lightbox Image**
- **Purpose**: Image that opens a fullscreen view on click
- **Use Cases**: Product zoom, article inline images, portfolio showcases
- **Features**: Cursor zoom-in, full-screen modal, prev/next navigation, keyboard support
- **Behavior**: Click opens lightbox; Escape closes; Arrow keys navigate gallery

---

## Aspect Ratio Variants

| Ratio | Value | Use Case |
|-------|-------|---------|
| **1:1** | Square | Avatars, product thumbnails, gallery tiles |
| **4:3** | Landscape | Classic photo, card media |
| **16:9** | Widescreen | Video thumbnails, hero banners, article covers |
| **3:2** | Photography | Blog images, editorial photos |
| **2:3** | Portrait | Book covers, posters, profile cards |
| **21:9** | Cinematic | Hero banners, panoramic images |
| **Auto** | Natural | Use image's intrinsic dimensions |

---

## Object Fit Variants

| Value | Behavior | Use Case |
|-------|----------|---------|
| **Cover** (Default) | Fills container, crops if needed | Hero images, card covers, avatars |
| **Contain** | Fits inside container, letterboxes | Product photos, logos, icons |
| **Fill** | Stretches to fill exactly | Intentional distortion, placeholders |
| **None** | Uses natural size, clips to container | Decorative textures, patterns |
| **Scale-down** | Smallest of contain or none | Mixed size catalogues |

---

## Size Variants

| Size | Dimensions | Use Case |
|------|------------|---------|
| **XS** | 32×32px | Inline avatars, dense lists |
| **SM** | 48×48px | Comment avatars, table thumbnails |
| **MD** | 64×64px | Card thumbnails, list items |
| **LG** | 96×96px | Profile cards, featured thumbnails |
| **XL** | 128×128px | Large profile photos, featured items |
| **2XL** | 192×192px | Hero thumbnails, featured cards |
| **Full** | 100% width | Cover images, hero banners |
| **Custom** | Any | via `width`/`height` props |

---

## Shape Variants

| Shape | Border Radius | Use Case |
|-------|---------------|---------|
| **Square** | 0px | Grid images, full-bleed |
| **Rounded** | 8px | Card media, standard images |
| **Rounded-lg** | 12px | Large feature images |
| **Circle** | 50% | Avatars, profile photos |
| **Pill** | 9999px | Decorative, marketing images |

---

## State Specifications

### **Loading State**
- Displays a skeleton placeholder matching the image's dimensions and shape
- Skeleton uses pulse animation (1.5s ease-in-out)
- Background: Gray 200 (#E5E7EB) pulsing to Gray 100 (#F3F4F6)
- Transition: Skeleton fades out (200ms) as image fades in (300ms)
- Progressive: Low-quality image placeholder (LQIP) can be shown first if provided

### **Loaded State**
- Image rendered at full quality
- Fade-in from opacity: 0 → 1 (300ms ease-out)
- Skeleton hidden after fade-in completes

### **Error / Fallback State**
- Shown when `src` fails to load (404, network error, CORS)
- Default fallback: Gray background + broken image icon centered
- Custom fallback: Any ReactNode via `fallback` prop (initials, placeholder illustration)
- Error icon: `image-off` or `alert-circle` icon at 40% opacity

### **Hover State** (Interactive Images)
- Overlay fades in (opacity: 0 → 0.4 black overlay)
- Scale: Optional subtle zoom (1.03×) of the image within its container
- Duration: 200ms ease-out
- Cursor: pointer (if onClick) or zoom-in (if zoomable)

### **Selected State** (Gallery)
- Border: 2px solid brand color (#3B82F6)
- Overlay: Light brand tint (10% opacity)
- Checkmark: Badge overlay top-right corner
- Scale: Slight scale-down (0.97×) on selection

### **Disabled State**
- Opacity: 50%
- Pointer events: none
- Cursor: default
- Overlay: Gray wash

---

## Props API

```typescript
interface ImageProps {
  // Source
  src: string;                                 // Image URL
  alt: string;                                 // Alt text (required)
  srcSet?: string;                             // Responsive srcset
  sizes?: string;                              // Responsive sizes hint
  lowQualitySrc?: string;                      // LQIP placeholder src

  // Dimensions
  width?: number | string;
  height?: number | string;
  aspectRatio?: '1:1' | '4:3' | '16:9' | '3:2' | '2:3' | '21:9' | 'auto' | string;
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl' | 'full';

  // Appearance
  shape?: 'square' | 'rounded' | 'rounded-lg' | 'circle' | 'pill';
  objectFit?: 'cover' | 'contain' | 'fill' | 'none' | 'scale-down';
  objectPosition?: string;                     // e.g., 'center', 'top', '30% 20%'

  // Loading
  loading?: 'lazy' | 'eager';                 // Native lazy loading
  decoding?: 'async' | 'sync' | 'auto';
  priority?: boolean;                          // Preload (disables lazy loading)
  showSkeleton?: boolean;                      // Show skeleton while loading (default: true)

  // Fallback
  fallback?: React.ReactNode;                  // Custom fallback on error
  fallbackSrc?: string;                        // Fallback image URL on error

  // Overlay
  overlay?: boolean;                           // Dark overlay on hover
  overlayContent?: React.ReactNode;            // Content shown in overlay
  overlayGradient?: 'top' | 'bottom' | 'both' | boolean; // Gradient overlay
  overlayOpacity?: number;                     // Overlay opacity (0–1)

  // Caption
  caption?: string | React.ReactNode;          // Image caption below
  captionPosition?: 'below' | 'overlay-bottom'; // Caption placement

  // Interaction
  onClick?: (event: React.MouseEvent) => void;
  zoomable?: boolean;                          // Enable lightbox on click
  selected?: boolean;                          // Selection state (gallery)
  onSelect?: (selected: boolean) => void;

  // Badge / Indicator
  badge?: React.ReactNode;                     // Overlay badge (top-right)
  indicator?: React.ReactNode;                 // Status indicator (bottom-right)

  // States
  disabled?: boolean;
  draggable?: boolean;                         // Allow drag (default: false)

  // Accessibility
  role?: string;                               // e.g., 'presentation' for decorative
  'aria-describedby'?: string;

  // Styling
  className?: string;
  style?: React.CSSProperties;
  containerClassName?: string;                 // Outer wrapper class
}
```

---

## Code Examples

### **1. Basic Responsive Image**
```typescript
import { Image } from '@components/image';

export function ArticleImage() {
  return (
    <Image
      src="https://images.example.com/article-hero.jpg"
      alt="A developer working at a standing desk with multiple monitors"
      aspectRatio="16:9"
      shape="rounded"
      loading="lazy"
    />
  );
}
```

### **2. Image with Skeleton Loading**
```typescript
import { Image } from '@components/image';

export function ProductPhoto() {
  return (
    <Image
      src="https://images.example.com/product/headphones.jpg"
      alt="Premium wireless headphones in matte black"
      aspectRatio="1:1"
      shape="rounded"
      objectFit="contain"
      showSkeleton
      loading="lazy"
    />
  );
}
```

### **3. Cover Hero Image with Gradient Overlay**
```typescript
import { Image } from '@components/image';

export function ArticleHero() {
  return (
    <Image
      src="https://images.example.com/hero.jpg"
      alt="City skyline at dusk"
      aspectRatio="21:9"
      shape="rounded-lg"
      objectFit="cover"
      objectPosition="center 30%"
      overlayGradient="bottom"
      overlayOpacity={0.6}
      overlayContent={
        <div className="absolute bottom-6 left-6 text-white">
          <h1 className="text-3xl font-bold">The Future of Design</h1>
          <p className="text-sm opacity-80 mt-1">March 18, 2026 · 8 min read</p>
        </div>
      }
      priority
    />
  );
}
```

### **4. Image with Custom Error Fallback**
```typescript
import { Image } from '@components/image';
import { ImageOff } from 'lucide-react';

export function SafeImage({ src, alt }: { src: string; alt: string }) {
  return (
    <Image
      src={src}
      alt={alt}
      aspectRatio="4:3"
      shape="rounded"
      fallback={
        <div className="flex flex-col items-center justify-center h-full gap-2 text-gray-400">
          <ImageOff size={32} />
          <span className="text-xs">Image unavailable</span>
        </div>
      }
      loading="lazy"
    />
  );
}
```

### **5. Zoomable Lightbox Image**
```typescript
import { Image } from '@components/image';

export function ZoomableProductImage() {
  return (
    <Image
      src="https://images.example.com/product/detail.jpg"
      alt="Close-up of watch dial showing sapphire crystal face and hour markers"
      aspectRatio="1:1"
      shape="rounded-lg"
      zoomable
      overlay
      overlayContent={
        <div className="flex items-center gap-1 text-white text-sm">
          <span>Click to zoom</span>
        </div>
      }
    />
  );
}
```

### **6. Gallery Grid with Selection**
```typescript
import { Image } from '@components/image';
import { useState } from 'react';

const photos = [
  { id: '1', src: '/photos/1.jpg', alt: 'Mountain landscape at sunrise' },
  { id: '2', src: '/photos/2.jpg', alt: 'Forest path in autumn' },
  { id: '3', src: '/photos/3.jpg', alt: 'Ocean waves at sunset' },
  { id: '4', src: '/photos/4.jpg', alt: 'City skyline at night' },
];

export function SelectableGallery() {
  const [selected, setSelected] = useState<string[]>([]);

  const toggle = (id: string) =>
    setSelected((prev) =>
      prev.includes(id) ? prev.filter((s) => s !== id) : [...prev, id]
    );

  return (
    <div className="grid grid-cols-2 sm:grid-cols-4 gap-2">
      {photos.map((photo) => (
        <Image
          key={photo.id}
          src={photo.src}
          alt={photo.alt}
          aspectRatio="1:1"
          shape="rounded"
          objectFit="cover"
          selected={selected.includes(photo.id)}
          onSelect={() => toggle(photo.id)}
          overlay
          loading="lazy"
        />
      ))}
    </div>
  );
}
```

### **7. Thumbnail with Play Badge**
```typescript
import { Image } from '@components/image';
import { Play } from 'lucide-react';

export function VideoThumbnail({ src, title, duration }: {
  src: string;
  title: string;
  duration: string;
}) {
  return (
    <Image
      src={src}
      alt={`Thumbnail for video: ${title}`}
      aspectRatio="16:9"
      shape="rounded"
      objectFit="cover"
      overlay
      overlayContent={
        <div className="flex items-center justify-center h-full">
          <div className="flex items-center justify-center w-12 h-12 rounded-full bg-white bg-opacity-90">
            <Play size={20} className="text-gray-900 ml-1" />
          </div>
        </div>
      }
      badge={
        <span className="bg-black bg-opacity-70 text-white text-xs px-1.5 py-0.5 rounded">
          {duration}
        </span>
      }
      loading="lazy"
    />
  );
}
```

### **8. Responsive Image with srcSet**
```typescript
import { Image } from '@components/image';

export function ResponsiveImage() {
  return (
    <Image
      src="https://images.example.com/photo-800.jpg"
      srcSet={`
        https://images.example.com/photo-400.jpg 400w,
        https://images.example.com/photo-800.jpg 800w,
        https://images.example.com/photo-1200.jpg 1200w,
        https://images.example.com/photo-2400.jpg 2400w
      `}
      sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
      alt="Mountain landscape with snow-capped peaks"
      aspectRatio="16:9"
      shape="rounded-lg"
      loading="lazy"
      decoding="async"
    />
  );
}
```

### **9. Image with Caption**
```typescript
import { Image } from '@components/image';

export function CaptionedImage() {
  return (
    <figure>
      <Image
        src="https://images.example.com/diagram.png"
        alt="Architecture diagram showing microservices communication flow"
        aspectRatio="16:9"
        shape="rounded"
        captionPosition="below"
        caption="Figure 1: Microservices architecture with event-driven communication"
      />
    </figure>
  );
}
```

### **10. Priority / Above-the-Fold Image**
```typescript
import { Image } from '@components/image';

export function HeroBanner() {
  return (
    <Image
      src="https://images.example.com/hero-banner.jpg"
      alt="Team collaboration in a modern office"
      aspectRatio="21:9"
      shape="square"
      objectFit="cover"
      objectPosition="center"
      priority              // Eagerly loaded; no lazy loading
      showSkeleton={false}  // No skeleton for above-the-fold
      loading="eager"
    />
  );
}
```

---

## Accessibility Specifications

### **The Alt Text Requirement**
The `alt` prop is **required** on the Image component. The component will warn in development if `alt` is empty or missing. Every image falls into one of three categories:

| Image Type | Alt Text Approach |
|------------|------------------|
| **Informative** | Describes the image content and purpose: `"Bar chart showing Q4 revenue growth of 32%"` |
| **Decorative** | Empty string: `alt=""` (screen readers skip it) — also set `role="presentation"` |
| **Functional** | Describes the action/destination: `"Open full-size photo"` (for linked images) |

### **Writing Good Alt Text**
- Be specific and descriptive: `"Developer typing code on a MacBook in a coffee shop"` not `"Person using laptop"`
- Don't start with "Image of" or "Photo of" — screen readers already announce it's an image
- For charts/graphs: describe the key insight, not just the format: `"Line chart showing 40% growth from Jan to June 2026"` not `"Line chart"`
- For logos: use the brand name: `"Acme Inc."` not `"Logo"`
- For decorative separators or backgrounds: `alt=""` + `role="presentation"`
- Max length: 125 characters; longer descriptions use `aria-describedby` pointing to a visible caption

### **Decorative Images**
```tsx
// ✅ Decorative background texture
<Image src="/bg-texture.jpg" alt="" role="presentation" />

// ✅ Redundant icon adjacent to labelled text
<Image src="/check-icon.png" alt="" aria-hidden="true" />
```

### **Complex Images**
For charts, diagrams, infographics, or images conveying substantial information:
```tsx
// ✅ Short alt + longer description via aria-describedby
<Image
  src="/revenue-chart.png"
  alt="Q4 2025 Revenue Chart"
  aria-describedby="chart-description"
/>
<p id="chart-description" className="sr-only">
  Bar chart showing monthly revenue from October to December 2025.
  October: $1.2M, November: $1.5M (+25%), December: $1.8M (+20%).
  Total Q4 revenue: $4.5M, representing a 32% increase year-over-year.
</p>
```

### **Interactive Images**
```tsx
// ✅ Image as button — label on button, not image
<button aria-label="Open full-size photo: Mountain landscape">
  <Image src="/thumb.jpg" alt="" aria-hidden="true" />
</button>

// ✅ Image as link
<a href="/article/design-systems" aria-label="Read: The Future of Design Systems">
  <Image src="/article-thumb.jpg" alt="" aria-hidden="true" />
</a>
```

### **Lightbox Accessibility**
- Lightbox wraps in `role="dialog"`, `aria-modal="true"`, `aria-label="Image viewer"`
- Focus moves into lightbox on open; returns to trigger on close
- `Escape` closes lightbox
- `ArrowLeft` / `ArrowRight` navigate between gallery images
- Current image announced: `"Image 3 of 8: Mountain landscape at sunrise"`
- Close button: `aria-label="Close image viewer"`

### **Loading State Accessibility**
- Skeleton placeholder: `aria-busy="true"` on container; or use `role="img"` with `aria-label="Loading image"`
- After load: `aria-busy` removed; full alt text available to screen readers

### **Zoom Cursor**
- `cursor: zoom-in` on zoomable images
- The affordance must be confirmed with text for screen reader users: include a visually-hidden hint or `title` attribute

### **Color Contrast for Overlaid Text**
- Text over image overlay: 4.5:1 minimum (use overlay to ensure sufficient contrast)
- Gradient overlay: Test text contrast at the lightest point of the gradient
- Semi-transparent overlay: `rgba(0,0,0,0.6)` on white image gives sufficient contrast for white text

---

## Animation Specifications

### **Loading → Loaded Transition**
- **Target**: Opacity of image layer
- **Duration**: 300ms
- **Easing**: ease-out
  ```css
  img { opacity: 0; transition: opacity 300ms ease-out; }
  img.loaded { opacity: 1; }
  ```

### **Skeleton Pulse**
- **Target**: Background color
- **Duration**: 1.5s per cycle
- **Easing**: ease-in-out
- **Repeat**: Infinite until image loads
  ```css
  @keyframes skeletonPulse {
    0%, 100% { background-color: #E5E7EB; }
    50%       { background-color: #F3F4F6; }
  }
  .skeleton { animation: skeletonPulse 1.5s ease-in-out infinite; }
  ```

### **Hover Overlay**
- **Target**: Overlay opacity
- **Duration**: 200ms ease-out
  ```css
  .overlay { opacity: 0; transition: opacity 200ms ease-out; }
  .container:hover .overlay { opacity: 1; }
  ```

### **Hover Zoom (Image Scale)**
- **Target**: `transform: scale()` on the inner `<img>`
- **Duration**: 400ms
- **Easing**: ease-out
- **Overflow**: `hidden` on container clips the scaled image
  ```css
  img { transition: transform 400ms ease-out; }
  .container:hover img { transform: scale(1.04); }
  ```

### **Lightbox Open**
- **Target**: Opacity + scale
- **Duration**: 200ms ease-out
  ```css
  @keyframes lightboxOpen {
    from { opacity: 0; transform: scale(0.96); }
    to   { opacity: 1; transform: scale(1); }
  }
  ```

### **Reduced Motion**
```css
@media (prefers-reduced-motion: reduce) {
  img { transition: none; }
  .skeleton { animation: none; background-color: #E5E7EB; }
  .container:hover img { transform: none; }
}
```

---

## Design Tokens

### **Skeleton**
| Token | Value |
|-------|-------|
| Base color | #E5E7EB |
| Highlight color | #F3F4F6 |
| Animation duration | 1.5s |

### **Overlay**
| Token | Value |
|-------|-------|
| Default overlay | rgba(0,0,0,0.4) |
| Gradient start | rgba(0,0,0,0) |
| Gradient end | rgba(0,0,0,0.7) |

### **Fallback**
| Token | Value |
|-------|-------|
| Background | #F3F4F6 |
| Icon color | #9CA3AF |
| Border | 1px solid #E5E7EB |

### **Shape Border Radii**
| Shape | Value |
|-------|-------|
| Square | 0px |
| Rounded | 8px |
| Rounded-lg | 12px |
| Circle | 50% |
| Pill | 9999px |

### **Selection**
| Token | Value |
|-------|-------|
| Border | 2px solid #3B82F6 |
| Overlay tint | rgba(59,130,246,0.1) |
| Checkmark bg | #3B82F6 |
| Checkmark icon | #FFFFFF |

---

## Performance Guidelines

### **Lazy Loading**
- Use `loading="lazy"` on all images below the fold (default)
- Use `loading="eager"` + `priority` only for above-the-fold images (LCP candidate)
- Limit eager-loaded images to 1–2 per page

### **Responsive Images with srcSet**
- Provide multiple resolutions: 400w, 800w, 1200w, 2400w
- Use `sizes` attribute to communicate layout width to browser
- WebP format preferred; JPEG fallback for broad compatibility
- AVIF format optional for cutting-edge compression

### **Low-Quality Image Placeholder (LQIP)**
- Provide `lowQualitySrc` for a blurry 20–40px placeholder
- Renders immediately while full image loads
- Blur-up transition: Low-quality → full quality with 300ms ease

### **Image CDN Integration**
- Use an image CDN (Cloudinary, Imgix, Next.js Image) for on-the-fly resize and format
- CDN URL patterns should be abstracted via a utility function, not hardcoded in component props
- Cache-control: Set long TTL for static images (1 year)

### **Core Web Vitals**
- **LCP**: Priority-load the hero/above-the-fold image; avoid lazy loading the LCP image
- **CLS**: Always specify `width` and `height` (or `aspectRatio`) to reserve space and prevent layout shift
- **FID / INP**: Defer non-critical image processing off the main thread

---

## Best Practices

### **Always Do**
- Provide meaningful `alt` text on every informative image
- Specify `aspectRatio` or `width`/`height` to prevent layout shift (CLS)
- Use `loading="lazy"` for all below-the-fold images
- Provide a `fallback` for images that may fail (user-uploaded content, external URLs)
- Use `objectFit="cover"` for images that fill a container with unknown content proportions
- Serve WebP or AVIF format via a CDN when possible

### **Never Do**
- Never use `alt="image"` or `alt="photo"` — meaningless alt text is worse than none
- Never omit `alt` entirely on informative images
- Never lazy-load the primary above-the-fold (LCP) image
- Never set a fixed pixel `height` without `objectFit` — it will distort the image
- Never rely on an image alone to communicate critical information (charts, diagrams need text alternatives)
- Never use images of text when actual HTML text can be used instead

### **Captioning**
- Use `<figure>` + `<figcaption>` semantics for captioned images
- Keep captions factual and complementary — don't restate the alt text
- Caption text: 12–13px, muted gray (#6B7280), italics optional

---

## Related Components

- **Avatar**: Specialized image component for user photos with initials fallback
- **Card**: `CardImage` subcomponent uses Image internally for card media
- **Empty State**: Uses placeholder illustrations when no image is available
- **Skeleton**: Standalone skeleton for non-image loading states
- **Lightbox / Modal**: Full-screen image viewer used by the `zoomable` prop
- **Carousel**: Sequences multiple Image components with prev/next navigation

---

## Version History

- **v1.0**: Basic image wrapper with alt enforcement, object-fit, shape, loading
- **v1.1**: Skeleton loading state, fallback on error, aspect ratio tokens
- **v1.2**: Hover overlay, overlayContent slot, caption support
- **v2.0**: Zoomable lightbox, gallery selection mode, badge/indicator slots
- **v2.1**: srcSet/sizes responsive support, LQIP, priority prop, AVIF hints
- **v2.2**: Gradient overlay, objectPosition, reduced motion, full a11y audit

---

## Testing Checklist

- [ ] Image renders at correct dimensions with `aspectRatio` set
- [ ] Skeleton shows while image is loading
- [ ] Skeleton fades out; image fades in on load (300ms)
- [ ] Error fallback renders when `src` returns 404
- [ ] Custom `fallback` node renders on error
- [ ] `fallbackSrc` loads alternative image on error
- [ ] `alt` text present on all informative images
- [ ] Decorative images use `alt=""` + `role="presentation"`
- [ ] `loading="lazy"` applied to below-the-fold images
- [ ] `priority` + `loading="eager"` applied to LCP image
- [ ] `objectFit="cover"` clips without distorting
- [ ] `objectFit="contain"` shows full image with letterbox
- [ ] `objectPosition` correctly positions focal point
- [ ] All shape variants render correct border radius
- [ ] Hover overlay fades in on mouse enter
- [ ] Hover zoom scales image within clipped container
- [ ] `overlayGradient="bottom"` renders bottom gradient
- [ ] `overlayContent` renders inside overlay zone
- [ ] Caption renders below image with `captionPosition="below"`
- [ ] `zoomable` opens lightbox on click
- [ ] Lightbox: Escape closes, Arrow keys navigate, focus trapped
- [ ] `selected` state renders blue border + checkmark
- [ ] `badge` renders top-right overlay
- [ ] `disabled` state applies opacity and disables interaction
- [ ] `srcSet` + `sizes` used for responsive delivery
- [ ] No layout shift (CLS = 0) when dimensions pre-specified
- [ ] Hover zoom animation disabled under `prefers-reduced-motion`
- [ ] Skeleton animation disabled under `prefers-reduced-motion`
- [ ] All text over overlay meets 4.5:1 contrast
