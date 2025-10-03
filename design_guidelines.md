# Design Guidelines: Real-Time Messaging Application

## Design Approach
**Reference-Based:** WhatsApp Web and Telegram interfaces - focusing on clean, intuitive chat layouts with premium messaging experience polish.

## Color Palette

### Primary Colors
- **Primary Green:** #25D366 (WhatsApp signature green) - for primary actions, online status, send buttons
- **Dark Green:** #128C7E - for hover states, headers, and depth
- **Accent Blue:** #34B7F1 - for links, selected states, and secondary actions

### Background & Surface
- **Background:** #F0F2F5 (light grey) - main app background
- **Chat Bubble (Sent):** #DCF8C6 (light green) - user's messages
- **Chat Bubble (Received):** #FFFFFF (white) - other users' messages
- **Text:** #303030 (dark grey) - primary text color

### Status Indicators
- Online: Primary Green (#25D366)
- Away/Last Seen: Grey (#8696A0)
- Typing: Accent Blue (#34B7F1)

## Typography
- **Font Families:** SF Pro Display (primary), Roboto (fallback), system fonts
- **Message Text:** 14px regular weight
- **User Names:** 16px medium weight
- **Timestamps:** 12px regular, muted grey
- **Headers:** 18-20px medium weight

## Layout System

### Three-Column Desktop Layout
1. **Left Sidebar (280px):** User profile, search, contact list
2. **Middle Column (320px):** Active chats list with previews
3. **Right Main Area (flexible):** Active conversation view

### Spacing Units
Use Tailwind units: **2, 3, 4, 6, 8** for consistent spacing
- Chat bubbles: p-3 (12px padding as specified)
- Section spacing: p-4, p-6
- Component gaps: gap-2, gap-4

## Component Library

### Chat Bubbles
- Rounded corners (rounded-2xl)
- Maximum width: 65% of chat area
- Sent messages: align right, light green background
- Received messages: align left, white background
- Tail/pointer optional for premium feel

### Message Status Indicators
- Single checkmark (✓): Sent
- Double checkmark (✓✓): Delivered
- Blue double checkmark: Read
- Clock icon: Pending

### User Status
- Green dot: Online
- Grey text: Last seen timestamp
- Blue text: Typing indicator with animated dots

### File Attachments
- Image previews: Rounded corners, max 300px width
- File cards: Icon + filename + size, downloadable
- Media grid: 2-3 columns for multiple images

## Interactive Elements

### Animations (Smooth & Subtle)
- Message send: Slide-in from right (150ms ease-out)
- New message arrival: Fade-in + slide-up (200ms)
- Typing indicator: Bouncing dots animation
- Status changes: Fade transition (300ms)
- No excessive animations - keep app feeling responsive

### Mobile Responsive Behavior
- **Mobile (<768px):** Single column view with slide-in transitions between contacts → chat list → conversation
- **Tablet (768px-1024px):** Two-column (chat list + conversation)
- **Desktop (>1024px):** Full three-column layout

## Key UI Patterns

### Header Bar
- User avatar (40px circular)
- Online status indicator (overlaid dot)
- Action buttons (call, video, menu) with icon-only design
- Background: White with subtle bottom border

### Chat List Item
- Avatar (48px)
- Name + last message preview (truncated)
- Timestamp (right-aligned)
- Unread badge (green circle with count)
- Hover state: Light grey background

### Input Area
- Rounded text input with emoji picker
- Attachment button (clip icon)
- Send button (paper plane icon, primary green)
- Voice message button (microphone icon)

### Search
- Prominent search bar at top of contacts/chat list
- Real-time filtering as user types
- Recent searches with quick clear

## Premium Polish Elements
- Smooth micro-interactions on all clickable elements
- Consistent drop shadows for elevation (subtle, not heavy)
- Frosted glass effects for overlays (backdrop-blur)
- Proper loading states (skeleton screens, not spinners)
- Empty states with helpful illustrations/messages

## Images
No hero images needed - this is a utility-focused messaging application. Focus on:
- User avatars (circular, uploadable)
- Group chat icons
- Shared media thumbnails
- Empty state illustrations (minimal, friendly)

## Dark Mode (Future Enhancement)
Maintain color relationships with adjusted palette for OLED-friendly dark theme when implemented.