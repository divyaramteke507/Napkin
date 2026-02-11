# Napkin to Silicon - Design Document

## Project Overview

Napkin to Silicon is an AI-powered visual compiler that transforms whiteboard sketches and diagrams into executable code using Google Gemini AI. The application provides an infinite canvas powered by tldraw where users can draw, sketch, and upload images, then compile them into working code in 11 different programming languages. The application is fully responsive, working seamlessly on both desktop and mobile devices.

## Architecture

### Technology Stack

**Frontend**:
- Framework: Next.js 14 (App Router)
- UI Library: React 18
- Canvas: tldraw 2.4.0
- Styling: Tailwind CSS 3
- Language: TypeScript 5
- Fonts: Inter, Space Grotesk, JetBrains Mono
- Syntax Highlighting: react-syntax-highlighter

**Backend**:
- Runtime: Node.js 20+
- API: Next.js API Routes (serverless)
- AI Model: Google Gemini 2.0 Flash
- Authentication: Firebase Authentication
- Database: Cloud Firestore

**DevOps**:
- Hosting: Vercel
- Version Control: Git
- Environment: .env.local configuration

### System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Client (Browser)                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Canvas     │  │   Toolbar    │  │  Code Pane   │      │
│  │  (tldraw)    │  │  (Custom)    │  │  (Output)    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   History    │  │     Auth     │  │  Templates   │      │
│  │  (Sidebar)   │  │   (Google)   │  │   (Modal)    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│  ┌──────────────┐  ┌──────────────┐                        │
│  │   Language   │  │   Landing    │                        │
│  │  (Selector)  │  │    (Page)    │                        │
│  └──────────────┘  └──────────────┘                        │
└─────────────────────────────────────────────────────────────┘
                            │
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                  Next.js API Routes                          │
│  ┌──────────────────────┐  ┌──────────────────────┐        │
│  │   /api/compile       │  │  /api/compilations   │        │
│  │  (Image → Code)      │  │  (History Fetch)     │        │
│  └──────────────────────┘  └──────────────────────┘        │
│  ┌──────────────────────┐                                   │
│  │   /api/explain       │                                   │
│  │  (Code Explanation)  │                                   │
│  └──────────────────────┘                                   │
└─────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                ↓                       ↓
┌──────────────────────┐    ┌──────────────────────┐
│   Google Gemini API  │    │   Firebase Services  │
│   (Code Generation)  │    │  - Authentication    │
│   (Code Explanation) │    │  - Firestore DB      │
└──────────────────────┘    └──────────────────────┘
```


## Responsive Design Strategy

### Desktop Layout (≥ 1024px)

```
┌────────────────────────────────────────────────────────────────┐
│                    [Template] [Language] [Sign Out] ← Top Right│
├──────────┬─────────────────────────────────┬───────────────────┤
│ HISTORY  │                                 │   CODE PANE       │
│ ────────│         CANVAS                  │   ─────────────   │
│          │                                 │                   │
│ Item 1   │                                 │  [Copy] [Down]    │
│ Item 2   │   [Toolbar]                     │  [Explain]        │
│ Item 3   │     ✏️                           │                   │
│ Item 4   │     🖍️                           │  Generated Code   │
│ Item 5   │     🧹                           │  with syntax      │
│ ...      │     ...                         │  highlighting     │
│          │                                 │                   │
│ [Toggle] │   [Compile Button]              │                   │
│          │                                 │                   │
├──────────┴─────────────────────────────────┴───────────────────┤
│  256px   │      Flexible Width             │      42%          │
└──────────┴─────────────────────────────────┴───────────────────┘
```

### Mobile Layout (< 1024px)

```
┌─────────────────────────────────────┐
│  📚  [Template] [Lang] [Sign Out]   │ ← Mobile Header (Fixed)
├─────────────────────────────────────┤
│                                     │
│                                     │
│          CANVAS (Full Screen)       │
│                                     │
│  [Toolbar]                          │
│    ✏️                                │
│    🖍️                                │
│    🧹                                │
│    ...                              │
│                                     │
│                                     │
│         [Compile Button]            │
│                                     │
├─────────────────────────────────────┤
│  CODE PANE (Bottom Sheet - 40vh)    │ ← Slides up when active
│  ┌─────────────────────────────┐   │
│  │ [Copy] [Download] [Explain] │   │
│  ├─────────────────────────────┤   │
│  │  Generated Code Here...     │   │
│  │  with syntax highlighting   │   │
│  └─────────────────────────────┘   │
└─────────────────────────────────────┘
```

### Responsive Breakpoints

- **Mobile**: < 1024px (single column, bottom sheet, overlays)
- **Desktop**: ≥ 1024px (three panels, side-by-side)
- **Tablet**: 768px - 1023px (mobile layout with optimizations)
- **Small Mobile**: < 640px (most compact layout)


## Core Features

### 1. Landing Page

**Implementation**: Modern scrollable landing page with 3D animations

**Sections**:

1. **Hero Section**
   - Large gradient title with 3D text effects
   - Compelling subtitle explaining the value proposition
   - CTA button with shine animation
   - Radial gradient background with depth
   - Floating animation effects

2. **How It Works** (3 Steps)
   - Step 1: Draw - Sketch your ideas with tools
   - Step 2: Select Language - Choose from 11 languages
   - Step 3: Get Code - AI generates executable code
   - Numbered cards with icons and feature tags
   - 3D card hover effects

3. **Features** (6 Cards)
   - Professional Drawing Tools
   - AI-Powered Compilation
   - Code Explanations
   - Compilation History
   - Secure & Private
   - Lightning Fast
   - Glassmorphism card design
   - Hover transformations

4. **Use Cases** (4 Cards)
   - Developers - Rapid prototyping
   - Students - Learning programming
   - Architects - System design
   - Teams - Collaboration
   - Icon-based cards with descriptions

5. **CTA Section**
   - Final call-to-action
   - Sign-in button
   - Gradient background

6. **Footer**
   - Branding with logo
   - Copyright information
   - Minimal design

**Design Features**:
- Glassmorphism effects (frosted glass)
- 3D card transformations on hover
- Gradient text (white → purple → blue)
- Smooth scroll behavior
- Floating animations (6s loop)
- Pulse glow effects (2s loop)
- Neon glow on titles
- Modern fonts (Inter + Space Grotesk)
- Responsive layout (mobile & desktop)

**Responsive Behavior**:
- Desktop: Full-width sections with large text
- Mobile: Stacked sections with adjusted typography
- Tablet: Optimized grid layouts


### 2. Authentication System

**Implementation**: Firebase Authentication with Google Sign-In

**Flow**:
1. User visits app → Landing page displayed
2. Click "Sign in with Google" (header) → Google OAuth popup
3. Successful authentication → Main app loads
4. User info displayed in header
5. Sign out option available

**Components**:
- `AuthButton.tsx` - Handles sign-in/sign-out UI
- Landing page with sign-in in header

**Responsive Behavior**:
- Desktop: Full text "Sign In with Google"
- Mobile: Compact text "Sign In"
- Email hidden on mobile, visible on desktop

**Security**:
- Google OAuth 2.0
- Firebase session management
- Token-based API authentication
- Secure token storage

### 3. Drawing Canvas

**Implementation**: tldraw SDK with custom toolbar

**Features**:
- Infinite canvas with pan and zoom
- Multiple drawing tools (13 total)
- Shape creation
- Text and sticky notes
- Image upload and placement
- Undo/redo functionality
- Clear canvas option with confirmation

**Tools Available**:
1. Select (⬆) - Selection and manipulation
2. Pen (✏️) - Freehand drawing
3. Highlighter (🖍️) - Transparent highlighting
4. Eraser (🧹) - Remove elements
5. Text (T) - Add text labels
6. Sticky Note (📝) - Add notes
7. Rectangle (▭) - Draw rectangles
8. Circle (○) - Draw circles
9. Triangle (△) - Draw triangles
10. Diamond (◇) - Draw diamonds
11. Arrow (→) - Draw arrows
12. Line (/) - Draw lines
13. Image Upload (🖼️) - Add images from gallery

**Color Palette** (8 colors):
- Black (#000000)
- Red (#ff0000)
- Blue (#0000ff)
- Green (#00ff00)
- Yellow (#ffff00)
- Magenta (#ff00ff)
- Cyan (#00ffff)
- White (#ffffff)

**Brush Sizes** (4 sizes):
- Small (2px)
- Medium (4px)
- Large (8px)
- Extra Large (12px)

**Responsive Behavior**:
- Desktop: 40px buttons, full canvas
- Mobile: 32px buttons, full-screen canvas with top padding
- Touch-optimized for mobile devices
- Pinch to zoom support (native tldraw)


### 4. Image Upload Feature

**Implementation**: File input with tldraw asset creation

**Flow**:
1. User clicks 🖼️ icon in toolbar
2. File picker opens (accepts image/*)
3. Image loaded and converted to data URL
4. Asset created in tldraw
5. Image shape placed at viewport center
6. Image becomes part of canvas (included in compilation)

**Supported Formats**: All browser-supported image formats (PNG, JPG, GIF, WebP, SVG, etc.)

**Technical Details**:
- FileReader API for image loading
- Data URL conversion
- tldraw asset system
- Automatic positioning at viewport center
- Scalable image shapes

### 5. Language Selection

**Implementation**: Dropdown selector with 11 programming languages

**Languages Supported**:
1. **C** - Low-level systems programming
2. **C++** - Object-oriented systems
3. **Python** - General-purpose scripting
4. **JavaScript** - Web scripting
5. **TypeScript** - Typed JavaScript
6. **React/TSX** - UI components
7. **Java** - Enterprise applications
8. **Go** - Concurrent systems
9. **Rust** - Memory-safe systems
10. **Swift** - iOS/macOS apps
11. **Kotlin** - Android apps

**Features**:
- Dropdown menu in top-right corner (desktop) or mobile header
- Language icons (🔤)
- Selected language highlighted
- Checkmark on active language
- Glassmorphism design
- Language sent to compilation API
- AI prompt adjusted per language

**Component**: `LanguageSelector.tsx`

**API Integration**:
- Language included in `/api/compile` request
- System prompt dynamically generated based on language
- Code output matches selected language conventions

**Responsive Behavior**:
- Desktop: Full language name visible
- Mobile: Language name hidden, icon only
- Compact dropdown on mobile


### 6. Compilation System

**Implementation**: Canvas export → Gemini API → Code generation

**Flow**:
1. User clicks "Compile to Silicon" button
2. Canvas exported as PNG (transparent background)
3. Image converted to base64
4. Sent to `/api/compile` with auth token and language
5. Gemini processes image with language-specific prompt
6. Code streamed back to client in real-time
7. Displayed in code pane with syntax highlighting
8. Saved to Firestore with user ID

**System Prompt** (Dynamic based on language):
```
You are a strict visual compiler. You receive a single image of a 
hand-drawn or digital diagram.

Rules:
- Output ONLY raw, executable {LANGUAGE} code
- No explanations, no markdown headers
- Single markdown fenced block (```{language})
- Code must be complete enough to compile/run
- Follow {LANGUAGE} best practices and idioms
```

**API Endpoint**: `POST /api/compile`
- Input: `{ imageBase64: string, language: string }`
- Output: Streaming text response
- Headers: `X-Compilation-Id` (Firestore document ID)
- Authentication: Bearer token required

**Technical Details**:
- tldraw getSvg() for canvas export
- SVG to PNG conversion using Canvas API
- Base64 encoding for transmission
- Streaming response with ReadableStream
- Real-time code display as chunks arrive

**Responsive Behavior**:
- Desktop: Button at bottom center
- Mobile: Button at bottom center with smaller padding
- Works on all devices

### 7. Code Explanation

**Implementation**: AI-powered code explanation using Gemini

**Flow**:
1. User compiles code successfully
2. Clicks "💡 Explain" button in code pane
3. Code and language sent to `/api/explain` endpoint
4. Gemini generates comprehensive explanation
5. Explanation displayed below code in styled section

**Explanation Includes**:
- Overview of what the code does
- Code structure breakdown
- Key components and their roles
- Logic flow explanation
- Programming concepts used
- Best practices highlighted

**API Endpoint**: `POST /api/explain`
- Input: `{ code: string, language: string }`
- Output: JSON `{ explanation: string }`
- Authentication: Not required (code already generated)

**Component**: Integrated in `CodeEditorPane.tsx`

**UI States**:
- Idle: "💡 Explain" button
- Loading: "⏳ Explaining..." button (disabled)
- Success: Explanation displayed below code
- Error: Error message displayed

**Responsive Behavior**:
- Desktop: Full explanation visible
- Mobile: Scrollable explanation in bottom sheet


### 8. Template System

**Implementation**: Pre-built canvas templates for quick start

**Templates Available** (8 total):

1. **Linked List** (Data Structures)
   - Node structure with pointers
   - Visual representation of linked nodes
   - Language: C/C++

2. **Binary Tree** (Data Structures)
   - Tree structure with nodes
   - Parent-child relationships
   - Language: Python

3. **REST API** (Web)
   - API endpoints diagram
   - HTTP methods (GET, POST, PUT, DELETE)
   - Language: JavaScript/TypeScript

4. **React Component** (Web)
   - Component hierarchy
   - Props flow diagram
   - Language: React/TSX

5. **Database Schema** (Database)
   - Tables and relationships
   - Foreign keys and constraints
   - Language: SQL (generates as Python/Java)

6. **Class Diagram** (Diagrams)
   - Classes and inheritance
   - UML notation
   - Language: Java/C++

7. **Flowchart** (Diagrams)
   - Process flow with decision points
   - Start/end nodes
   - Language: Python

8. **State Machine** (Diagrams)
   - States and transitions
   - Event handling
   - Language: JavaScript

**Features**:
- Modal with template gallery
- Category filtering (All, Data Structures, Web, Database, Diagrams)
- Template cards with icons, names, descriptions
- One-click loading to canvas
- Language suggestions per template
- Beautiful animations and hover effects
- Glassmorphism design

**Component**: `TemplateSelector.tsx`

**UI Flow**:
1. Click "🎨 Templates" button
2. Modal opens with template grid
3. Filter by category (optional)
4. Click template card
5. Alert shows template info
6. Language auto-selected (if specified)
7. User draws based on template guidance

**Responsive Behavior**:
- Desktop: 3-column grid
- Tablet: 2-column grid
- Mobile: 1-column grid
- Compact modal on mobile


### 9. Code Output Features

**Implementation**: Enhanced code display with multiple actions

**Features**:

1. **Syntax Highlighting**
   - Library: react-syntax-highlighter
   - Theme: VS Code Dark Plus
   - Line numbers enabled
   - Language-specific highlighting
   - 11 languages supported

2. **Copy to Clipboard**
   - One-click copy button (📋)
   - Visual feedback ("✓ Copied!")
   - 2-second confirmation message
   - Works on all browsers

3. **Download Code**
   - Downloads with correct file extension
   - Extensions: .c, .cpp, .py, .js, .ts, .tsx, .java, .go, .rs, .swift, .kt
   - Filename: `generated-code.{ext}`
   - Blob-based download

4. **Code Explanation**
   - Explain button (💡)
   - Loading state while generating
   - Explanation displayed below code
   - Styled section with icon
   - Comprehensive breakdown

**Component**: `CodeEditorPane.tsx`

**Status Indicators**:
- `idle` - Waiting for compilation
- `compiling` - Processing request (animated)
- `streaming` - Receiving code chunks
- `done` - Compilation complete (✓)
- `error` - Compilation failed (error message)

**Responsive Behavior**:
- Desktop: Fixed right panel (42% width)
- Mobile: Bottom sheet (40vh height)
- Auto-hide on mobile when idle
- Slides up when code generated

### 10. History System

**Implementation**: Firestore collection with real-time sync

**Data Model**:
```typescript
interface Compilation {
  id: string;
  userId: string;
  base64Image: string;
  generatedCode: string;
  timestamp: Timestamp;
  language?: string;
}
```

**Features**:
- Auto-loads on user sign-in
- Displays last 50 compilations
- Click to restore code to editor
- Sorted by timestamp (newest first)
- Retractable sidebar
- Loading and error states

**API Endpoint**: `GET /api/compilations`
- Requires: Authorization header with Firebase token
- Returns: `{ items: HistoryItem[] }`
- Sorted by timestamp descending
- Limited to 50 items

**Component**: `HistorySidebar.tsx`

**Responsive Behavior**:
- Desktop: Toggleable sidebar (0-256px width)
- Mobile: Full-screen overlay with dark backdrop
- Auto-close on mobile after selection
- Smooth slide animations


## Component Structure

```
src/
├── app/
│   ├── page.tsx                 # Main app with landing + canvas
│   ├── layout.tsx               # Root layout with fonts & viewport
│   ├── globals.css              # Global styles + animations
│   └── api/
│       ├── compile/route.ts     # Compilation endpoint
│       ├── compilations/route.ts # History endpoint
│       └── explain/route.ts     # Code explanation endpoint
├── components/
│   ├── AuthButton.tsx           # Sign in/out button
│   ├── CompileButton.tsx        # Compile trigger
│   ├── CustomToolbar.tsx        # Drawing tools sidebar
│   ├── CodeEditorPane.tsx       # Code output with actions
│   ├── HistorySidebar.tsx       # Compilation history
│   ├── LanguageSelector.tsx     # Language dropdown
│   └── TemplateSelector.tsx     # Template gallery modal
└── lib/
    └── firebase.ts              # Firebase client config
```

## Data Flow

### Compilation Flow

```
User draws on canvas
       ↓
Clicks "Compile to Silicon"
       ↓
Canvas exported to PNG blob
       ↓
Converted to base64 string
       ↓
Sent to /api/compile with auth token + language
       ↓
Server validates token (Firebase Admin)
       ↓
Image sent to Gemini API with language-specific prompt
       ↓
Gemini generates code
       ↓
Code extracted from markdown
       ↓
Saved to Firestore (userId, image, code, timestamp, language)
       ↓
Streamed back to client
       ↓
Displayed in code pane + added to history
```

### Authentication Flow

```
User visits app
       ↓
Check auth state (Firebase)
       ↓
Not authenticated → Show landing page
       ↓
User clicks "Sign in with Google"
       ↓
Google OAuth popup
       ↓
User authorizes
       ↓
Firebase creates session
       ↓
Main app loads
       ↓
History fetched from Firestore
```

### Explanation Flow

```
User has generated code
       ↓
Clicks "💡 Explain" button
       ↓
Code + language sent to /api/explain
       ↓
Gemini generates explanation
       ↓
Explanation returned as JSON
       ↓
Displayed below code in styled section
```


## Security

### Authentication
- Google OAuth 2.0
- Firebase session management
- Token-based API authentication
- Secure token storage in browser

### Authorization
- All API routes require valid Firebase token
- Firestore rules enforce user-specific access
- Users can only read/write their own compilations
- Server-side token verification

### Firestore Security Rules
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /compilations/{docId} {
      allow read: if request.auth != null 
                  && request.auth.uid == resource.data.userId;
      allow create: if request.auth != null 
                    && request.resource.data.userId == request.auth.uid;
      allow update, delete: if false;
    }
  }
}
```

### API Security
- Environment variables for sensitive keys
- Server-side API key management
- No client-side exposure of secrets
- HTTPS required for all connections
- CORS configuration

## Performance Considerations

### Canvas Optimization
- tldraw handles rendering optimization
- Shapes rendered on demand
- Viewport culling for off-screen elements
- Hardware-accelerated transforms

### Image Handling
- Images stored as data URLs in assets
- Lazy loading for history thumbnails
- Base64 encoding for API transmission
- Efficient blob conversion

### API Optimization
- Streaming responses for code generation
- Chunked transfer encoding
- Client-side caching of history
- Debounced API calls

### Database Optimization
- Composite index on (userId, timestamp)
- Limit queries to 50 results
- Server-side timestamps
- Efficient query patterns

### Mobile Optimization
- Touch event optimization
- Hardware-accelerated CSS animations
- Efficient re-renders with React
- Lazy loading of components
- Optimized bundle size


## Styling System

### Design Tokens
```css
--canvas: #0a0a0f       (Deep space background)
--surface: #13131a      (Panel background)
--border: #1f1f2e       (Border color)
--muted: #8b8b9e        (Secondary text)
--accent: #ffffff       (Primary text)
--primary: #6366f1      (Indigo)
--primary-light: #818cf8 (Light indigo)
--primary-dark: #4f46e5  (Dark indigo)
```

### Typography
- **Body Font**: Inter (Google Fonts)
- **Heading Font**: Space Grotesk (Google Fonts)
- **Code Font**: JetBrains Mono (monospace)
- **Code Size**: 14px (0.875rem)
- **UI Size**: 12px (0.75rem)

### Animations
- **Float**: Vertical floating motion (6s loop)
- **Pulse Glow**: Pulsing shadow effect (2s loop)
- **Slide In Up**: Entry animation (0.6s)
- **Gradient Shift**: Animated gradient background (15s loop)
- **Particle Float**: Floating particles (4s loop)

### 3D Effects
- Card tilt on hover: `perspective(1000px) rotateX(5deg) rotateY(5deg)`
- Scale on hover: `scale(1.02)` to `scale(1.1)`
- Transform style: `preserve-3d`
- Smooth transitions: 300ms ease

### Glassmorphism
- Background: `rgba(255, 255, 255, 0.05)`
- Backdrop filter: `blur(10px)`
- Border: `1px solid rgba(255, 255, 255, 0.1)`
- Shadow: `0 8px 32px rgba(0, 0, 0, 0.1)`

### Gradients
- **Primary**: `linear-gradient(135deg, #667eea 0%, #764ba2 100%)`
- **Button**: `linear-gradient(to right, #6366f1, #a855f7, #ec4899)`
- **Text**: `linear-gradient(135deg, #ffffff 0%, #a78bfa 50%, #667eea 100%)`
- **Background**: `radial-gradient(ellipse at top, #1e1b4b 0%, #0a0a0f 50%)`

### Layout

**Desktop (≥ 1024px)**:
- Three-panel layout
- History sidebar: 256px (collapsible to 0px)
- Canvas: Flexible width
- Code pane: 42% of viewport (320px - 640px)
- Compile button: Fixed bottom center (z-index: 300)
- Toolbar: Fixed left center (z-index: 300)
- Controls: Top-right corner (z-index: 400)

**Mobile (< 1024px)**:
- Single-column stacked layout
- Mobile header: Fixed top (z-index: 400)
- Canvas: Full screen with top padding
- Code pane: Bottom sheet (40vh, z-index: 350)
- History: Overlay (z-index: 395)
- Toolbar: Compact left side (z-index: 300)
- Compile button: Bottom center (z-index: 300)

### Responsive Utilities

**Button Sizes**:
- Mobile: `w-8 h-8` (32px)
- Desktop: `w-10 h-10` (40px)

**Padding**:
- Mobile: `p-2` (8px)
- Desktop: `p-3` (12px)

**Text Sizes**:
- Mobile: `text-xs` (12px)
- Desktop: `text-sm` (14px)

**Spacing**:
- Mobile: `gap-1` (4px)
- Desktop: `gap-2` (8px)


## Mobile-Specific Design

### Touch Optimization
- Minimum touch target: 32px × 32px
- Recommended touch target: 44px × 44px
- Spacing between targets: 8px minimum
- `-webkit-tap-highlight-color: transparent`
- `touch-action: manipulation`

### Mobile Header
```tsx
<div className="lg:hidden fixed top-0 left-0 right-0 z-[400]">
  <button>📚 History</button>
  <TemplateSelector />
  <LanguageSelector />
  <AuthButton />
</div>
```

**Features**:
- Fixed positioning at top
- Compact controls
- History toggle button
- All primary actions accessible

### Bottom Sheet (Code Pane)
```tsx
<div className={`
  fixed lg:relative
  bottom-0 lg:bottom-auto
  w-full lg:w-[42%]
  h-[40vh] lg:h-full
  ${status === 'idle' ? 'translate-y-full lg:translate-y-0' : 'translate-y-0'}
`}>
```

**Behavior**:
- Hidden when idle (translate-y-full)
- Slides up when code generated
- 40% of viewport height
- Swipeable (native browser behavior)
- Fixed at bottom

### Overlay Sidebar (History)
```tsx
<>
  {open && <div className="lg:hidden fixed inset-0 bg-black/50" />}
  <div className={`
    fixed lg:relative
    ${open ? 'translate-x-0' : '-translate-x-full lg:translate-x-0'}
  `}>
</>
```

**Behavior**:
- Slides in from left
- Dark backdrop overlay
- Auto-closes after selection
- Full-screen on mobile
- Toggleable width on desktop

### Responsive Components

**Toolbar**:
- Mobile: 32px buttons, compact spacing
- Desktop: 40px buttons, standard spacing
- Scrollable if needed
- Touch-optimized

**Language Selector**:
- Mobile: Icon only, hidden text
- Desktop: Full language name
- Compact dropdown on mobile

**Auth Button**:
- Mobile: "Sign In" / "Sign Out"
- Desktop: "Sign In with Google" / email + "Sign Out"

**Template Modal**:
- Mobile: 1-column grid, compact padding
- Tablet: 2-column grid
- Desktop: 3-column grid


## Error Handling

### Client-Side
- Empty canvas validation before compilation
- Image upload format validation
- Network error handling with user feedback
- Auth state error handling
- API timeout handling
- Graceful degradation

### Server-Side
- Missing API key detection
- Invalid request body handling
- Gemini API error handling
- Firestore write error handling
- Token verification errors
- Rate limit handling

### User Feedback
- Clear error messages
- Status indicators
- Loading states
- Success confirmations
- Retry options

## Environment Variables

### Required
```env
# Gemini API
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-2.0-flash

# Firebase Client (Public)
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# Firebase Admin (Server-side)
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_CLIENT_EMAIL=your_service_account@project.iam.gserviceaccount.com
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
```

## Deployment

### Vercel Configuration
- Framework: Next.js
- Build Command: `npm run build`
- Output Directory: `.next`
- Install Command: `npm install`
- Node Version: 20.x
- Environment: Production

### Build Process
1. Install dependencies
2. Type checking (TypeScript)
3. Linting (ESLint)
4. Build Next.js app
5. Generate static pages
6. Optimize assets
7. Deploy to Vercel

### Environment Setup
1. Add all environment variables in Vercel dashboard
2. Enable Firebase Authentication (Google provider)
3. Create Firestore database
4. Deploy security rules
5. Create composite index (userId, timestamp)
6. Configure custom domain (optional)


## Future Enhancements

### Implemented Features ✅
- Beautiful landing page with 3D animations
- Multi-language support (11 languages)
- Syntax highlighting with line numbers
- Copy to clipboard functionality
- Download code with correct extensions
- Code explanation with AI
- Template system (8 templates)
- Modern UI with glassmorphism
- Full mobile responsiveness
- Touch-optimized controls

### Potential Future Features

**Phase 3**:
1. **Real-time Collaboration**
   - Multi-user canvas editing
   - Presence indicators
   - Live cursors
   - Comments and annotations

2. **Code Editing**
   - In-app code editor (Monaco/CodeMirror)
   - Live code execution
   - Debugging tools
   - Code refactoring

3. **Customization**
   - Dark/light theme toggle
   - Custom color palettes
   - Toolbar customization
   - Keyboard shortcuts

4. **PWA Features**
   - Install on home screen
   - Offline support
   - Background sync
   - Push notifications

**Phase 4**:
1. **Advanced AI**
   - Multiple AI models (GPT-4, Claude)
   - Custom prompts
   - Code optimization
   - Bug detection

2. **Integration**
   - GitHub integration
   - VS Code extension
   - API for third-party apps
   - Webhook support

3. **Analytics**
   - Usage dashboard
   - Popular patterns
   - Success metrics
   - User insights

4. **Community**
   - Template marketplace
   - User profiles
   - Social features
   - Leaderboards


## Testing Strategy

### Unit Tests
- Component rendering
- Utility functions
- API route handlers
- State management

### Integration Tests
- Authentication flow
- Compilation flow
- History loading
- Template loading

### E2E Tests
- Full user journey
- Canvas interactions
- Code generation
- Mobile interactions

### Responsive Testing
- Desktop (≥ 1024px)
- Tablet (768px - 1023px)
- Mobile (< 768px)
- Touch interactions
- Orientation changes

### Browser Testing
- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers

### Performance Testing
- Lighthouse scores
- Core Web Vitals
- Load time
- Time to Interactive
- First Contentful Paint

## Maintenance

### Dependencies
- Regular updates for security patches
- tldraw version compatibility
- Firebase SDK updates
- Next.js framework updates
- React version updates

### Monitoring
- API response times
- Error rates
- User authentication success
- Compilation success rate
- Firestore query performance
- Mobile performance metrics

### Backup Strategy
- Firestore automatic backups
- Environment variable documentation
- Code repository (Git)
- Configuration backups

## Accessibility

### Current Implementation
- ARIA labels on buttons
- Semantic HTML structure
- Keyboard navigation
- Focus indicators
- Touch-friendly controls (≥32px)

### Future Improvements
- Screen reader optimization
- High contrast mode
- Keyboard shortcuts
- Voice control support
- WCAG 2.1 AA compliance


## Technical Specifications

### Browser Requirements
- Modern browsers with ES6+ support
- Canvas API support
- FileReader API support
- Fetch API support
- WebSocket support (future)

### Device Requirements
- Minimum screen width: 320px
- Touch support (optional)
- Internet connection required
- JavaScript enabled

### Performance Targets
- Page load: < 3 seconds
- Time to Interactive: < 5 seconds
- First Contentful Paint: < 1.5 seconds
- Compilation start: < 1 second
- Code streaming: < 3 seconds
- Canvas rendering: 60 FPS

### Bundle Size
- Initial JS: ~87 KB
- Total First Load: ~719 KB
- Code splitting enabled
- Tree-shaking enabled
- Gzip compression

## License

This project uses open-source libraries:
- Next.js (MIT)
- React (MIT)
- tldraw (Apache 2.0)
- Firebase (Apache 2.0)
- Tailwind CSS (MIT)
- react-syntax-highlighter (MIT)

## Summary

**Napkin to Silicon** is a production-ready, fully-featured application that combines:
- Modern web technologies (Next.js, React, TypeScript)
- AI-powered code generation (Google Gemini)
- Beautiful, responsive design (Tailwind CSS)
- Secure authentication (Firebase)
- Real-time data sync (Firestore)
- Mobile-first approach
- Professional UI/UX

**Key Achievements**:
- ✅ 11 programming languages supported
- ✅ 8 pre-built templates
- ✅ Full mobile responsiveness
- ✅ Syntax highlighting and code actions
- ✅ AI-powered code explanation
- ✅ Beautiful 3D animations
- ✅ Touch-optimized controls
- ✅ Production-ready build
- ✅ Zero errors
- ✅ Comprehensive documentation

**Architecture Highlights**:
- Serverless Next.js API routes
- Streaming responses for real-time feedback
- Firebase for authentication and database
- Responsive design with Tailwind CSS
- Component-based React architecture
- TypeScript for type safety

**Design Highlights**:
- Glassmorphism and 3D effects
- Gradient text and backgrounds
- Smooth animations and transitions
- Mobile-first responsive design
- Touch-optimized interactions
- Professional color palette

---

**Status**: Production Ready ✅  
**Version**: 2.0 (MVP + Phase 2 + Mobile Complete)  
**Last Updated**: February 11, 2026  
**Build Status**: ✅ Passing  
**Test URL**: http://localhost:3001
