# Avatar Component Documentation

## Description

The Avatar component displays a user's profile image, initials, or placeholder icon. Avatars are used throughout applications to represent users, accounts, and identities in a compact, visually distinctive way. They appear in comment threads, user lists, navigation bars, chat interfaces, and team rosters.

**When to use:**
- Displaying user profile pictures in comments, posts, or messages
- Team member rosters and user lists
- User profile pages and account headers
- Chat and collaboration interfaces
- Author attribution and user references
- Navigation bars and account menus
- Status indicators and online presence
- Multiple user selections (avatars in a group)

**When NOT to use:**
- As decorative icons unrelated to user identity
- For brand logos (use Logo component instead)
- When sufficient space isn't available to render clearly (use icon only)
- For non-user entities that don't need visual representation

---

## Component Structure

```
Avatar
├── AvatarImage (User profile photo)
├── AvatarFallback (Initials or icon when image unavailable)
└── StatusIndicator (Optional - online/offline/away status)
```

---

## Avatar Types & Variants

### Type 1: Image Avatar
**Usage:** Primary avatar type using actual user profile photos

```
┌─────────┐
│ [Photo] │  User profile photo with fallback to initials
└─────────┘
```

- **When to use:** Social platforms, team apps, collaboration tools
- **Fallback behavior:** Shows initials when image fails to load
- **Image requirements:** Square images (1:1 aspect ratio), min 24×24px
- **Supported formats:** JPG, PNG, WebP, SVG
- **Optimization:** Use appropriate image size (2x resolution for retina)

### Type 2: Initials Avatar
**Usage:** Fallback display using user's first and last name initials

```
┌─────────┐
│   JD    │  White text on colored background
└─────────┘
```

- **When to use:** When image unavailable, new users without photos
- **Text format:** 1-2 characters (first letter of first + last name)
- **Colors:** Deterministic color based on user ID or name hash
- **Contrast:** AA compliant (minimum 4.5:1 ratio)
- **Font:** Bold, centered, 60% of avatar size

### Type 3: Icon Avatar
**Usage:** Generic user icon or placeholder for unidentified users

```
┌─────────┐
│   👤    │  Generic user icon
└─────────┘
```

- **When to use:** Anonymous users, system accounts, placeholders
- **Icon options:** User silhouette, generic person, robot (for bots)
- **Background:** Gray or neutral color
- **Use cases:** Default avatars, system messages, service accounts

### Type 4: Status Indicator Avatar
**Usage:** Avatar with online/offline/away status badge

```
┌─────────┐ ●  Green dot = Online
│ [Photo] │-   Yellow dot = Away
└─────────┘    Gray dot = Offline
```

- **Status options:**
  - **Online (Green):** #10B981 — User currently active
  - **Away (Yellow):** #F59E0B — User idle (15+ min inactive)
  - **Offline (Gray):** #6B7280 — User not logged in
  - **Do Not Disturb (Red):** #EF4444 — User unavailable

- **Position:** Bottom-right corner (overlapping)
- **Size:** 12-16px diameter indicator
- **Border:** 2px white border around indicator

### Type 5: Group Avatar
**Usage:** Multiple avatars overlapping to show a group of users

```
┌─────────────────────────┐
│ [Photo] [Photo] [Photo] │  Up to 3-4 users shown
│        +2 more          │  Overflow indicator for additional users
└─────────────────────────┘
```

- **Display:** Overlapping avatars (50% overlap)
- **Max shown:** 3-4 avatars before showing "+X more"
- **Overflow badge:** "+2", "+5", etc. on final avatar
- **Hover behavior:** Show full list of all users
- **Use cases:** Team assignments, group chats, shared items

---

## Size Variants

| Size | Px | Use Case | Icon Size |
|------|-----|----------|-----------|
| **XS** | 24 | Compact lists, chat sidebars, small UI | 12px |
| **SM** | 32 | Comment threads, user lists, sidebar | 16px |
| **MD** | 40 | Standard lists, navigation | 20px |
| **LG** | 48 | User profiles, team rosters | 24px |
| **XL** | 64 | Large profile headers, hero sections | 32px |
| **2XL** | 80+ | Large profile pages, banner headers | 40px+ |

---

## Shape Variants

| Shape | Use Case | Notes |
|-------|----------|-------|
| **Circle** | Standard, most common | Default for user avatars |
| **Square** | Alternative, modern style | Better for brand/app icons |
| **Rounded Square** | Balanced option | 8-12px border radius |

---

## States

| State | Visual | Interaction |
|-------|--------|-------------|
| **Default** | Static avatar, no interaction | Informational |
| **Hover** | Subtle shadow lift, 5% opacity change | Pointer cursor |
| **Clickable** | Adds subtle border or highlight on hover | Opens user profile |
| **Active** | Border highlight (2-3px) | Currently selected user |
| **Disabled** | Grayed out, 50% opacity | User account inactive |
| **Loading** | Skeleton or pulse animation | Image loading |
| **Error** | Fallback to initials/icon | Image failed to load |
| **Unread** | Badge with number | New messages/notifications |

---

## Props / Properties

### Avatar Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `src` | `string` | — | Image URL for avatar |
| `alt` | `string` | — | Alt text for image (screen reader) |
| `initials` | `string` | — | Fallback text (e.g., "JD") |
| `size` | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl' \| '2xl'` | `'md'` | Avatar size |
| `shape` | `'circle' \| 'square' \| 'rounded'` | `'circle'` | Avatar shape |
| `onClick` | `() => void` | — | Click handler for profile link |
| `status` | `'online' \| 'away' \| 'offline' \| 'dnd'` | — | Status indicator |
| `badge` | `string \| number` | — | Badge text or count |
| `disabled` | `boolean` | `false` | Grays out avatar |
| `loading` | `boolean` | `false` | Shows loading state |
| `className` | `string` | — | Custom CSS class |

### AvatarImage Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `src` | `string` | — | Image URL (required) |
| `alt` | `string` | — | Alt text for accessibility |
| `onError` | `() => void` | — | Callback if image fails |
| `onLoad` | `() => void` | — | Callback on image load |

### AvatarFallback Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `ReactNode` | — | Initials or icon to display |
| `delayMs` | `number` | `600` | Delay before showing fallback |

### StatusIndicator Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `status` | `'online' \| 'away' \| 'offline' \| 'dnd'` | — | Status type (required) |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Indicator size |
| `animated` | `boolean` | `false` | Pulse animation for online |

### AvatarGroup Props

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `children` | `Avatar[]` | — | Avatar components |
| `max` | `number` | `3` | Max avatars shown before overflow |
| `onOverflowClick` | `() => void` | — | Handler for "+X more" click |
| `size` | `string` | `'md'` | Size of all avatars in group |
| `spacing` | `number` | `-8` | Overlap amount (negative = overlap) |

---

## Accessibility

### ARIA Attributes

```jsx
// Image Avatar
<img
  src="user.jpg"
  alt="Jane Doe"                    // Clear user identification
  role="img"                        // Semantic role
/>

// Initials Avatar
<div
  role="img"
  aria-label="Jane Doe"             // Who this avatar represents
>
  JD
</div>

// Status Indicator
<div
  aria-label="Jane Doe, online"     // Include status in label
  title="Online"                    // Tooltip on hover
>
  ...
</div>

// Avatar Group
<div
  role="group"
  aria-label="Team members: John, Jane, and 3 others"
>
  ...
</div>
```

### Color Contrast

- **Initials on colored background:** AA compliant (4.5:1 minimum)
- **Status indicator:** Distinct colors for colorblind users
  - Online: Green (#10B981)
  - Away: Yellow (#F59E0B)
  - Offline: Gray (#6B7280)
  - Do Not Disturb: Red (#EF4444)

### Keyboard Interaction

| Key | Behavior |
|-----|----------|
| **Tab** | Focus on clickable avatar |
| **Enter** | Open user profile or action menu |
| **Space** | Activate click handler |

### Screen Reader Announcement

- Simple avatar: `"Jane Doe, image"`
- With status: `"Jane Doe, image, online"`
- Group avatar: `"Team members: John, Jane, and 3 others, group image"`
- Unread badge: `"Jane Doe, image, 3 unread messages"`

---

## Code Examples

### Basic Image Avatar

```jsx
import { Avatar, AvatarImage, AvatarFallback } from '@/components/Avatar'

export function UserAvatar() {
  return (
    <Avatar>
      <AvatarImage
        src="https://api.example.com/users/jane-doe.jpg"
        alt="Jane Doe"
      />
      <AvatarFallback>JD</AvatarFallback>
    </Avatar>
  )
}
```

### With Status Indicator

```jsx
<Avatar status="online" size="lg">
  <AvatarImage
    src="https://api.example.com/users/john-smith.jpg"
    alt="John Smith"
  />
  <AvatarFallback>JS</AvatarFallback>
</Avatar>
```

### Initials Only (No Image)

```jsx
<Avatar size="md" shape="circle">
  <AvatarFallback>AB</AvatarFallback>
</Avatar>
```

### Size Variants

```jsx
<div className="flex gap-4 items-center">
  <Avatar size="xs"><AvatarFallback>A</AvatarFallback></Avatar>
  <Avatar size="sm"><AvatarFallback>B</AvatarFallback></Avatar>
  <Avatar size="md"><AvatarFallback>C</AvatarFallback></Avatar>
  <Avatar size="lg"><AvatarFallback>D</AvatarFallback></Avatar>
  <Avatar size="xl"><AvatarFallback>E</AvatarFallback></Avatar>
</div>
```

### Shape Variants

```jsx
<div className="flex gap-4">
  <Avatar shape="circle"><AvatarFallback>C</AvatarFallback></Avatar>
  <Avatar shape="square"><AvatarFallback>S</AvatarFallback></Avatar>
  <Avatar shape="rounded"><AvatarFallback>R</AvatarFallback></Avatar>
</div>
```

### Group Avatar

```jsx
import { AvatarGroup } from '@/components/Avatar'

export function TeamAvatars() {
  const users = [
    { name: 'Jane Doe', initials: 'JD', image: '...' },
    { name: 'John Smith', initials: 'JS', image: '...' },
    { name: 'Alice Johnson', initials: 'AJ', image: '...' },
    { name: 'Bob Wilson', initials: 'BW', image: '...' },
  ]

  return (
    <AvatarGroup max={3} onOverflowClick={() => showFullList()}>
      {users.map(user => (
        <Avatar key={user.name} size="md">
          <AvatarImage src={user.image} alt={user.name} />
          <AvatarFallback>{user.initials}</AvatarFallback>
        </Avatar>
      ))}
    </AvatarGroup>
  )
}
```

### With Badge (Unread Count)

```jsx
<div className="relative">
  <Avatar size="md">
    <AvatarImage src="user.jpg" alt="Jane Doe" />
    <AvatarFallback>JD</AvatarFallback>
  </Avatar>
  <span className="absolute -top-1 -right-1 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">
    3
  </span>
</div>
```

### Clickable Profile Link

```jsx
<Avatar
  size="md"
  onClick={() => navigate('/profile/jane-doe')}
  className="cursor-pointer hover:opacity-80"
>
  <AvatarImage src="user.jpg" alt="Jane Doe" />
  <AvatarFallback>JD</AvatarFallback>
</Avatar>
```

### Status Indicator Variants

```jsx
<div className="flex gap-6">
  <Avatar status="online">
    <AvatarFallback>Online</AvatarFallback>
  </Avatar>
  <Avatar status="away">
    <AvatarFallback>Away</AvatarFallback>
  </Avatar>
  <Avatar status="offline">
    <AvatarFallback>Offline</AvatarFallback>
  </Avatar>
  <Avatar status="dnd">
    <AvatarFallback>DND</AvatarFallback>
  </Avatar>
</div>
```

### In Comment Thread

```jsx
export function CommentItem({ author, comment }) {
  return (
    <div className="flex gap-3">
      <Avatar size="sm">
        <AvatarImage src={author.avatar} alt={author.name} />
        <AvatarFallback>{author.initials}</AvatarFallback>
      </Avatar>
      <div>
        <p className="font-semibold">{author.name}</p>
        <p className="text-gray-600">{comment}</p>
      </div>
    </div>
  )
}
```

### In User List

```jsx
export function UserList({ users }) {
  return (
    <ul className="space-y-2">
      {users.map(user => (
        <li key={user.id} className="flex items-center gap-3 p-2 hover:bg-gray-50">
          <Avatar size="sm">
            <AvatarImage src={user.avatar} alt={user.name} />
            <AvatarFallback>{user.initials}</AvatarFallback>
          </Avatar>
          <span>{user.name}</span>
          {user.status && (
            <span className={`w-2 h-2 rounded-full ${
              user.status === 'online' ? 'bg-green-500' :
              user.status === 'away' ? 'bg-yellow-500' :
              'bg-gray-300'
            }`} />
          )}
        </li>
      ))}
    </ul>
  )
}
```

---

## Do's and Don'ts

### ✅ Do

- Use actual profile photos when available for better user recognition
- Provide meaningful alt text for screen readers (e.g., "Jane Doe")
- Show initials as fallback when images don't load
- Use consistent colors for initials based on user ID (deterministic coloring)
- Include status indicators in chat/collaboration apps
- Make clickable avatars have clear hover states
- Use appropriate size for context (small in lists, large in headers)
- Test image URLs to ensure they load correctly
- Use square 1:1 aspect ratio images for avatars
- Show group avatars with clear overflow indicators

### ❌ Don't

- Don't use generic icons as primary avatar display
- Don't forget alt text on images (accessibility critical)
- Don't use randomly colored initials (be deterministic)
- Don't make avatars too large in dense layouts
- Don't crop user photos to unrecognizable shapes
- Don't use low-resolution or blurry images
- Don't show status indicators inconsistently
- Don't make non-clickable avatars look clickable
- Don't forget to handle image load errors
- Don't use ambiguous initials (e.g., "M" for Michael or Mary)

---

## Color System for Initials

Use deterministic coloring based on user ID or name hash. This ensures consistency and helps with user recognition.

```jsx
const colorPalette = [
  '#EF4444', // Red
  '#F97316', // Orange
  '#EAB308', // Yellow
  '#22C55E', // Green
  '#06B6D4', // Cyan
  '#3B82F6', // Blue
  '#8B5CF6', // Purple
  '#EC4899', // Pink
]

function getAvatarColor(userId) {
  const hash = userId.split('').reduce((acc, char) => {
    return acc + char.charCodeAt(0)
  }, 0)
  return colorPalette[hash % colorPalette.length]
}
```

---

## Animation Specifications

### Status Indicator Pulse (Online)
- **Duration:** 2 seconds (infinite loop)
- **Easing:** ease-in-out
- **Animation:** Scale 1 → 1.2 → 1, Opacity 1 → 0.5 → 1

### Image Load Animation
- **Duration:** 200ms
- **Easing:** ease-out
- **Animation:** Opacity 0 → 1

### Badge Entrance
- **Duration:** 300ms
- **Easing:** cubic-bezier(0.16, 1, 0.3, 1)
- **Animation:** Scale 0 → 1, Opacity 0 → 1

---

## Design Tokens Used

- **Colors:**
  - Initials background: Palette of 8 colors (see Color System)
  - Initials text: White (#FFFFFF)
  - Online indicator: `#10B981`
  - Away indicator: `#F59E0B`
  - Offline indicator: `#6B7280`
  - Border: `#E5E7EB`

- **Typography:**
  - Initials: `typography.button.bold`
  - Sizes adjusted by avatar size

- **Spacing:**
  - Status indicator border: 2px
  - Group overlap: 50%
  - Badge offset: 4px from corner

- **Borders:**
  - Avatar border: 2px (when active/selected)
  - Radius: 50% (circle), 8px (rounded square)

---

## Related Components

- **Badge** — For notification counts on avatars
- **Tooltip** — For showing user name on hover
- **Button** — For clickable avatar actions
- **Menu** — For avatar dropdown menu
- **Dialog** — For profile preview modal

---

## Performance Considerations

- **Image optimization:** Use appropriate image sizes (2x for retina)
- **Lazy loading:** Consider lazy loading images in long lists
- **Caching:** Cache avatar images to reduce requests
- **CDN:** Serve images from CDN for faster delivery
- **Fallback:** Always provide initials or icon fallback
- **File size:** Keep avatar images under 50KB each

---

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile browsers (iOS Safari 14+, Chrome Mobile)

**CSS Requirements:**
- CSS Grid or Flexbox
- CSS Custom Properties (for token coloring)
- CSS mask-image (for circular avatars on older browsers)

---

## Resources

- [Avatars in design systems](https://www.nngroup.com/articles/avatar-representations/)
- [WCAG color contrast guidelines](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html)
- [Image optimization best practices](https://web.dev/image-optimization/)
- [Radix UI Avatar](https://www.radix-ui.com/docs/primitives/components/avatar)
- [Material Design Avatar](https://m3.material.io/components/avatar/overview)

---

**Documentation Generated:** March 16, 2026
**Last Updated:** March 16, 2026
**Version:** 1.0
**Component Types Documented:** 5 Avatar variants (Image, Initials, Icon, Status Indicator, Group)
