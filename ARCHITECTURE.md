# Napkin to Silicon - Architecture Documentation

## System Overview

Napkin to Silicon is a full-stack web application that combines a visual canvas interface with AI-powered code generation. The system is built on a modern serverless architecture using Next.js, Firebase, and Google Gemini AI.

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Client Layer                             │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌──────────┐ │
│  │   React    │  │   tldraw   │  │  Tailwind  │  │ Firebase │ │
│  │ Components │  │   Canvas   │  │    CSS     │  │   Auth   │ │
│  └────────────┘  └────────────┘  └────────────┘  └──────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ↓ HTTPS
┌─────────────────────────────────────────────────────────────────┐
│                      Application Layer                           │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │              Next.js 14 (App Router)                       │ │
│  │  ┌──────────────────┐  ┌──────────────────┐              │ │
│  │  │  API Routes      │  │  Server Actions  │              │ │
│  │  │  - /api/compile  │  │  - Auth verify   │              │ │
│  │  │  - /api/compilations│ │  - Token mgmt  │              │ │
│  │  └──────────────────┘  └──────────────────┘              │ │
│  └────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                              │
                    ┌─────────┴─────────┐
                    ↓                   ↓
┌──────────────────────────┐  ┌──────────────────────────┐
│   External Services      │  │   Data Layer             │
│  ┌────────────────────┐  │  │  ┌────────────────────┐  │
│  │  Google Gemini API │  │  │  │  Cloud Firestore   │  │
│  │  - gemini-2.0-flash│  │  │  │  - compilations    │  │
│  │  - Image → Code    │  │  │  │  - users           │  │
│  └────────────────────┘  │  │  └────────────────────┘  │
│  ┌────────────────────┐  │  │  ┌────────────────────┐  │
│  │  Firebase Auth     │  │  │  │  Firebase Storage  │  │
│  │  - Google OAuth    │  │  │  │  (Future: images)  │  │
│  └────────────────────┘  │  │  └────────────────────┘  │
└──────────────────────────┘  └──────────────────────────┘
```

## Component Architecture

### Frontend Components

```
src/app/page.tsx (Main Container)
├── LoginScreen (Conditional)
│   └── Google Sign-In Button
│
└── Main App (Authenticated)
    ├── AuthButton (Top Right)
    │   ├── User Info Display
    │   └── Sign Out Button
    │
    ├── HistorySidebar (Left, Collapsible)
    │   ├── History Items List
    │   └── Toggle Button
    │
    ├── Canvas Container (Center)
    │   ├── Tldraw Editor
    │   │   ├── Custom Toolbar (Left Overlay)
    │   │   │   ├── Drawing Tools
    │   │   │   ├── Shape Tools
    │   │   │   ├── Image Upload
    │   │   │   ├── Undo/Redo
    │   │   │   ├── Color Picker
    │   │   │   ├── Size Selector
    │   │   │   └── Clear Canvas
    │   │   │
    │   │   └── Compile Button (Bottom Center)
    │   │
    │   └── Canvas State Management
    │
    └── CodeEditorPane (Right)
        ├── Status Bar
        ├── Code Display
        └── Error Display
```

### State Management

```typescript
// Global State (React Context)
interface AppState {
  user: User | null;
  authLoading: boolean;
  code: string;
  status: CompileStatus;
  errorMessage?: string;
  history: HistoryItem[];
  historyOpen: boolean;
}

// Canvas State (tldraw)
interface CanvasState {
  shapes: Shape[];
  assets: Asset[];
  currentTool: string;
  selectedShapes: string[];
  viewport: Viewport;
}

// Compilation State
interface CompilationState {
  status: 'idle' | 'compiling' | 'streaming' | 'done' | 'error';
  code: string;
  error?: string;
  compilationId?: string;
}
```

## Data Models

### Firestore Schema

```typescript
// Collection: compilations
interface Compilation {
  id: string;                    // Auto-generated document ID
  userId: string;                // Firebase Auth UID
  base64Image: string;           // Canvas snapshot (PNG, base64)
  generatedCode: string;         // AI-generated code
  timestamp: Timestamp;          // Server timestamp
  language?: 'c' | 'tsx' | 'jsx'; // Detected language
}

// Indexes
// - (userId ASC, timestamp DESC) - For user history queries
```

### Client-Side Models

```typescript
interface HistoryItem {
  id: string;
  timestamp: number;
  codeSnippet: string;  // First 200 chars
  code: string;         // Full code
}

interface User {
  uid: string;
  email: string | null;
  displayName: string | null;
  photoURL: string | null;
}
```

## API Architecture

### REST Endpoints

#### POST /api/compile
```typescript
// Request
{
  imageBase64: string;  // PNG image, base64 encoded
}

// Headers
Authorization: Bearer <firebase-token>

// Response (Streaming)
Content-Type: text/plain; charset=utf-8
X-Compilation-Id: <firestore-doc-id>

// Body: Streamed code text
```

**Flow**:
1. Validate request body
2. Verify Firebase token (optional, defaults to anonymous)
3. Send image to Gemini API with system prompt
4. Extract code from markdown response
5. Save to Firestore (userId, image, code, timestamp)
6. Stream code back to client
7. Return compilation ID in header

#### GET /api/compilations
```typescript
// Headers
Authorization: Bearer <firebase-token>  // Required

// Response
{
  items: HistoryItem[];
}
```

**Flow**:
1. Verify Firebase token (required)
2. Query Firestore: `compilations` where `userId == uid`
3. Order by `timestamp DESC`
4. Limit to 50 results
5. Map to HistoryItem format
6. Return JSON response

## Authentication Flow

```
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │
       │ 1. Visit app
       ↓
┌─────────────────────┐
│  Check Auth State   │
│  (Firebase Client)  │
└──────┬──────────────┘
       │
       │ 2. Not authenticated
       ↓
┌─────────────────────┐
│  Show Login Screen  │
└──────┬──────────────┘
       │
       │ 3. Click "Sign in with Google"
       ↓
┌─────────────────────┐
│  Google OAuth Flow  │
│  (Firebase Auth)    │
└──────┬──────────────┘
       │
       │ 4. User authorizes
       ↓
┌─────────────────────┐
│  Firebase Session   │
│  Created            │
└──────┬──────────────┘
       │
       │ 5. onAuthStateChanged
       ↓
┌─────────────────────┐
│  Load Main App      │
│  + Fetch History    │
└─────────────────────┘
```

## Compilation Flow

```
┌──────────────┐
│ User Action  │
│ (Draw/Upload)│
└──────┬───────┘
       │
       │ 1. Click "Compile to Silicon"
       ↓
┌──────────────────────┐
│  Export Canvas       │
│  editor.exportToBlob │
└──────┬───────────────┘
       │
       │ 2. PNG blob
       ↓
┌──────────────────────┐
│  Convert to Base64   │
│  FileReader API      │
└──────┬───────────────┘
       │
       │ 3. Base64 string
       ↓
┌──────────────────────┐
│  Get Firebase Token  │
│  (if authenticated)  │
└──────┬───────────────┘
       │
       │ 4. POST /api/compile
       ↓
┌──────────────────────┐
│  Server: Verify Auth │
│  (Firebase Admin)    │
└──────┬───────────────┘
       │
       │ 5. Valid token / anonymous
       ↓
┌──────────────────────┐
│  Send to Gemini API  │
│  with system prompt  │
└──────┬───────────────┘
       │
       │ 6. AI generates code
       ↓
┌──────────────────────┐
│  Extract Code from   │
│  Markdown Response   │
└──────┬───────────────┘
       │
       │ 7. Clean code
       ↓
┌──────────────────────┐
│  Save to Firestore   │
│  (async, non-blocking)│
└──────┬───────────────┘
       │
       │ 8. Stream response
       ↓
┌──────────────────────┐
│  Client: Display     │
│  Code + Add History  │
└──────────────────────┘
```

## Security Architecture

### Authentication Layers

```
┌─────────────────────────────────────────┐
│         Client-Side Security            │
│  ┌───────────────────────────────────┐  │
│  │  Firebase Auth SDK                │  │
│  │  - Session management             │  │
│  │  - Token refresh                  │  │
│  │  - OAuth flow                     │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
                    │
                    ↓ ID Token
┌─────────────────────────────────────────┐
│         Server-Side Security            │
│  ┌───────────────────────────────────┐  │
│  │  Firebase Admin SDK               │  │
│  │  - Token verification             │  │
│  │  - User ID extraction             │  │
│  │  - Permission checks              │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
                    │
                    ↓ Verified UID
┌─────────────────────────────────────────┐
│         Data-Level Security             │
│  ┌───────────────────────────────────┐  │
│  │  Firestore Security Rules         │  │
│  │  - User-specific access           │  │
│  │  - Read/write permissions         │  │
│  │  - Data validation                │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

### API Security

```typescript
// Token Verification Flow
async function verifyRequest(request: NextRequest) {
  const authHeader = request.headers.get('authorization');
  
  if (!authHeader?.startsWith('Bearer ')) {
    return { authenticated: false, userId: 'anonymous' };
  }
  
  try {
    const token = authHeader.slice(7);
    const decodedToken = await admin.auth().verifyIdToken(token);
    return { authenticated: true, userId: decodedToken.uid };
  } catch (error) {
    return { authenticated: false, userId: 'anonymous' };
  }
}
```

## Performance Optimization

### Client-Side

1. **Code Splitting**
   - Dynamic import for tldraw (SSR disabled)
   - Lazy loading for heavy components
   - Route-based splitting (Next.js automatic)

2. **Caching Strategy**
   - Firebase session cached in localStorage
   - History cached in React state
   - Canvas state managed by tldraw

3. **Rendering Optimization**
   - React.memo for expensive components
   - useCallback for event handlers
   - Virtualization for history list (future)

### Server-Side

1. **API Optimization**
   - Streaming responses for code generation
   - Chunked transfer encoding
   - Connection pooling for Firestore

2. **Database Optimization**
   - Composite indexes for queries
   - Query result limiting (50 items)
   - Server-side timestamps

3. **Edge Functions**
   - Vercel Edge Runtime (future)
   - Geographic distribution
   - Reduced cold starts

## Scalability Considerations

### Horizontal Scaling
- Serverless functions (auto-scaling)
- Firestore (managed scaling)
- Firebase Auth (managed scaling)
- CDN for static assets (Vercel)

### Vertical Scaling
- Gemini API rate limits
- Firestore read/write quotas
- Firebase Auth quotas

### Cost Optimization
- Firestore query optimization
- Image compression before storage
- Gemini API usage monitoring
- Firebase Auth free tier (50k MAU)

## Monitoring & Observability

### Metrics to Track
- API response times
- Compilation success rate
- Authentication success rate
- Error rates by endpoint
- Firestore query performance
- User engagement metrics

### Logging Strategy
```typescript
// Client-side
console.error('Compilation failed:', error);

// Server-side
console.log('[Compile] User:', userId, 'Status:', status);
console.error('[Compile] Gemini error:', error);
```

### Error Tracking
- Client errors: Browser console
- Server errors: Vercel logs
- Future: Sentry integration

## Deployment Architecture

```
┌─────────────────────────────────────────┐
│            GitHub Repository            │
└──────────────┬──────────────────────────┘
               │
               │ Git push
               ↓
┌─────────────────────────────────────────┐
│         Vercel Build Pipeline           │
│  1. Install dependencies (npm install)  │
│  2. Build Next.js app (npm run build)   │
│  3. Optimize assets                     │
│  4. Deploy to Edge Network              │
└──────────────┬──────────────────────────┘
               │
               ↓
┌─────────────────────────────────────────┐
│         Vercel Edge Network             │
│  ┌───────────────────────────────────┐  │
│  │  Static Assets (CDN)              │  │
│  │  - HTML, CSS, JS                  │  │
│  │  - Images, fonts                  │  │
│  └───────────────────────────────────┘  │
│  ┌───────────────────────────────────┐  │
│  │  Serverless Functions             │  │
│  │  - API routes                     │  │
│  │  - Server components              │  │
│  └───────────────────────────────────┘  │
└─────────────────────────────────────────┘
```

## Technology Decisions

### Why Next.js?
- Server-side rendering for SEO
- API routes for backend logic
- File-based routing
- Optimized production builds
- Vercel deployment integration

### Why tldraw?
- Production-ready canvas library
- Infinite canvas support
- Built-in shape tools
- Extensible architecture
- Active maintenance

### Why Firebase?
- Managed authentication
- Real-time database
- Automatic scaling
- Free tier for development
- Easy integration

### Why Gemini?
- Multimodal capabilities (image → text)
- Fast response times
- Competitive pricing
- Streaming support
- Good code generation quality

## Future Architecture Improvements

1. **Real-time Collaboration**
   - WebSocket server
   - Operational transforms
   - Presence indicators

2. **Caching Layer**
   - Redis for session data
   - CDN for generated code
   - Browser cache optimization

3. **Microservices**
   - Separate compilation service
   - Dedicated image processing
   - Queue-based architecture

4. **Advanced Features**
   - Code execution sandbox
   - Version control for canvases
   - Template marketplace
   - Plugin system
