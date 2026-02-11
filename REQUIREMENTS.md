# Napkin to Silicon - Requirements Document

## Project Vision

Create an intuitive, AI-powered visual compiler that transforms hand-drawn sketches and diagrams into executable code. The application makes programming more accessible through visual thinking, supporting multiple programming languages and providing a seamless experience across desktop and mobile devices.

## Target Users

- **Primary**: Developers who prefer visual thinking and rapid prototyping
- **Secondary**: Students learning programming concepts through visualization
- **Tertiary**: Technical architects designing systems and documenting architecture
- **Additional**: Teams collaborating on whiteboard sessions and design discussions

---

## Core Requirements

### 1. User Authentication

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to sign in with Google so that my work is saved securely
- As a user, I want to see my profile information after signing in
- As a user, I want to sign out when I'm done
- As a user, I want my session to persist across page refreshes

**Acceptance Criteria**:
- [x] Google OAuth integration via Firebase
- [x] Persistent session management
- [x] User profile display (email)
- [x] Sign out functionality
- [x] Protected routes (require authentication)
- [x] Automatic session restoration
- [x] Responsive auth button (compact on mobile)

**Technical Implementation**:
- Firebase Authentication
- Google OAuth 2.0
- Token-based API authentication
- Client-side session management

---

### 2. Beautiful Landing Page

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a new user, I want to understand what the app does immediately
- As a new user, I want to see how the app works step-by-step
- As a new user, I want to know all available features
- As a new user, I want to see real-world use cases
- As a new user, I want an easy way to sign in

**Acceptance Criteria**:
- [x] Hero section with compelling value proposition
- [x] "How It Works" section (3 clear steps)
- [x] Features showcase (6 feature cards)
- [x] Use cases section (4 use case cards)
- [x] Call-to-action sections
- [x] Footer with branding
- [x] Sign-in button in header
- [x] Smooth scrolling between sections
- [x] Modern design with 3D animations
- [x] Gradient text effects
- [x] Glassmorphism design elements
- [x] Fully responsive (mobile & desktop)

**Design Features**:
- 3D card transformations on hover
- Floating animations
- Pulse glow effects
- Gradient backgrounds
- Modern typography (Inter + Space Grotesk)
- Professional color palette

---

### 3. Drawing Canvas

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to draw freehand on an infinite canvas
- As a user, I want to use different drawing tools
- As a user, I want to create shapes quickly
- As a user, I want to add text and notes
- As a user, I want to undo/redo my actions
- As a user, I want to upload images to the canvas
- As a user, I want to clear the canvas when needed
- As a user, I want the canvas to work on mobile devices

**Acceptance Criteria**:
- [x] Infinite canvas with pan and zoom
- [x] Drawing tools: pen, highlighter, eraser
- [x] Shape tools: rectangle, circle, triangle, diamond, arrow, line
- [x] Text and sticky note tools
- [x] Image upload functionality
- [x] Undo/redo support
- [x] Clear canvas option with confirmation
- [x] Color picker (8 colors)
- [x] Brush size selector (4 sizes: S, M, L, XL)
- [x] Custom toolbar with glassmorphism design
- [x] Touch-optimized for mobile devices
- [x] Responsive toolbar (compact on mobile)

**Tools Available**:
1. Select (⬆) - Selection tool
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

---

### 4. Multi-Language Support

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to select my preferred programming language
- As a user, I want to see all available language options
- As a user, I want the AI to generate code in my selected language
- As a user, I want language-specific code conventions
- As a user, I want to switch languages easily

**Acceptance Criteria**:
- [x] Language selector dropdown in header
- [x] Support for 11 programming languages:
  - [x] C - Low-level systems programming
  - [x] C++ - Object-oriented systems
  - [x] Python - General-purpose scripting
  - [x] JavaScript - Web scripting
  - [x] TypeScript - Typed JavaScript
  - [x] React/TSX - UI components
  - [x] Java - Enterprise applications
  - [x] Go - Concurrent systems
  - [x] Rust - Memory-safe systems
  - [x] Swift - iOS/macOS development
  - [x] Kotlin - Android development
- [x] Language preference persisted in session
- [x] Language included in compilation request
- [x] AI prompt dynamically adjusted per language
- [x] Generated code follows language conventions
- [x] Language icons for visual identification
- [x] Responsive dropdown (compact on mobile)

**Technical Implementation**:
- Dynamic system prompt generation
- Language-specific code formatting
- File extension mapping for downloads
- Syntax highlighting per language

---

### 5. AI-Powered Compilation

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to compile my canvas drawing into code
- As a user, I want to see the code generation progress in real-time
- As a user, I want to see the generated code clearly formatted
- As a user, I want to know if compilation fails with clear error messages
- As a user, I want the compilation to work on mobile devices

**Acceptance Criteria**:
- [x] Compile button visible and accessible
- [x] Canvas exported as PNG image (transparent background)
- [x] Image sent to Gemini AI for processing
- [x] Code streamed back in real-time
- [x] Code displayed in dedicated pane with syntax highlighting
- [x] Comprehensive error handling
- [x] Loading states (compiling, streaming, done, error)
- [x] Status indicators visible to user
- [x] Works on both desktop and mobile
- [x] Responsive compile button positioning

**AI Model**:
- Google Gemini 2.0 Flash
- Streaming responses
- Language-aware prompts
- High accuracy for diagram-to-code conversion

---

### 6. Code Output & Actions

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to see generated code with syntax highlighting
- As a user, I want to copy code to clipboard easily
- As a user, I want to download code with the correct file extension
- As a user, I want to see line numbers in the code
- As a user, I want to know the compilation status
- As a user, I want to see error messages if compilation fails

**Acceptance Criteria**:
- [x] Dedicated code pane (right side on desktop, bottom sheet on mobile)
- [x] Syntax highlighting with react-syntax-highlighter
- [x] VS Code Dark Plus theme
- [x] Line numbers displayed
- [x] Copy to clipboard button with visual feedback
- [x] Download button with correct file extensions:
  - .c, .cpp, .py, .js, .ts, .tsx, .java, .go, .rs, .swift, .kt
- [x] Status indicators (idle, compiling, streaming, done, error)
- [x] Error message display
- [x] Scrollable content
- [x] Word wrap for long lines
- [x] Responsive layout (bottom sheet on mobile)

**Code Actions**:
1. Copy to Clipboard - One-click copy with "✓ Copied!" feedback
2. Download Code - Downloads with correct file extension
3. Explain Code - AI-powered explanation (see next section)

---

### 7. Code Explanation

**Priority**: Medium  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to understand what the generated code does
- As a user, I want to see a detailed explanation of the code structure
- As a user, I want to learn programming concepts from the explanation
- As a user, I want to see best practices highlighted

**Acceptance Criteria**:
- [x] "💡 Explain" button in code pane header
- [x] API endpoint for code explanation
- [x] Comprehensive explanation covering:
  - [x] Code overview and purpose
  - [x] Structure breakdown
  - [x] Key components and their roles
  - [x] Logic flow explanation
  - [x] Programming concepts used
  - [x] Best practices and recommendations
- [x] Explanation displayed below code in styled section
- [x] Loading state while generating ("⏳ Explaining...")
- [x] Error handling for failed explanations
- [x] Language-aware explanations

**Technical Implementation**:
- Gemini API for explanations
- Separate API endpoint (/api/explain)
- Markdown-style formatting
- Responsive display

---

### 8. Template System

**Priority**: Medium  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to start with pre-built templates
- As a user, I want to browse templates by category
- As a user, I want to see template previews and descriptions
- As a user, I want to load templates onto my canvas with one click
- As a user, I want templates to work on mobile devices

**Acceptance Criteria**:
- [x] Template selector button in header
- [x] Beautiful modal with template gallery
- [x] 8 pre-built templates:
  - [x] Linked List (Data Structures)
  - [x] Binary Tree (Data Structures)
  - [x] REST API (Web)
  - [x] React Component (Web)
  - [x] Database Schema (Database)
  - [x] Class Diagram (Diagrams)
  - [x] Flowchart (Diagrams)
  - [x] State Machine (Diagrams)
- [x] Category filtering (All, Data Structures, Web, Database, Diagrams)
- [x] Template cards with icons, names, and descriptions
- [x] One-click template loading
- [x] Language suggestions per template
- [x] Preview text for each template
- [x] Responsive modal (1/2/3 column grid)
- [x] Touch-friendly on mobile

**Template Categories**:
- Data Structures: Linked List, Binary Tree
- Web: REST API, React Component
- Database: Database Schema
- Diagrams: Class Diagram, Flowchart, State Machine

---

### 9. Compilation History

**Priority**: Medium  
**Status**: ✅ Implemented

**User Stories**:
- As a user, I want to see my past compilations
- As a user, I want to restore a previous compilation
- As a user, I want to see when each compilation was created
- As a user, I want to see a preview of each compilation
- As a user, I want to access history on mobile devices

**Acceptance Criteria**:
- [x] History sidebar with toggle button
- [x] List of past compilations (50 most recent)
- [x] Timestamp display for each item
- [x] Code snippet preview (120 characters)
- [x] Click to restore code to editor
- [x] Automatic loading on sign-in
- [x] Sorted by date (newest first)
- [x] Responsive design:
  - Desktop: Toggleable sidebar (0-256px width)
  - Mobile: Full-screen overlay with backdrop
- [x] Auto-close on mobile after selection
- [x] Loading and error states

**Data Storage**:
- Cloud Firestore database
- User-specific collections
- Automatic synchronization
- Secure access with Firebase rules

---

### 10. Mobile Responsiveness

**Priority**: High  
**Status**: ✅ Implemented

**User Stories**:
- As a mobile user, I want the app to work perfectly on my phone
- As a mobile user, I want touch-friendly controls
- As a mobile user, I want to see all features without horizontal scrolling
- As a mobile user, I want the layout to adapt to my screen size
- As a tablet user, I want an optimized experience

**Acceptance Criteria**:
- [x] Responsive layout for all screen sizes
- [x] Mobile-specific features:
  - [x] Mobile header with compact controls
  - [x] Bottom sheet for code output
  - [x] Overlay history sidebar
  - [x] Touch-optimized toolbar
  - [x] Larger touch targets (≥32px)
- [x] Desktop-specific features:
  - [x] Three-panel layout
  - [x] Side-by-side panels
  - [x] Hover effects
  - [x] Full labels and text
- [x] Breakpoint at 1024px (mobile vs desktop)
- [x] No horizontal scrolling on any device
- [x] Proper viewport configuration
- [x] Touch event optimization
- [x] Smooth animations and transitions
- [x] Responsive typography
- [x] Adaptive button sizes
- [x] Mobile-friendly modals

**Responsive Features**:
- Mobile header (< 1024px)
- Bottom sheet code pane (< 1024px)
- Overlay history sidebar (< 1024px)
- Compact controls on mobile
- Full-featured desktop layout (≥ 1024px)

---

## Non-Functional Requirements

### Performance

**Requirements**:
- Canvas should render smoothly (60 FPS)
- Compilation should start within 1 second
- Code streaming should be visible within 3 seconds
- History should load within 2 seconds
- Page load time < 3 seconds
- Smooth animations on all devices
- No jank or lag on mobile

**Status**: ✅ Met

**Optimizations**:
- Hardware-accelerated CSS transitions
- Efficient React rendering
- Lazy loading for components
- Optimized bundle size
- Tree-shaken Tailwind CSS

---

### Security

**Requirements**:
- All API endpoints must be authenticated
- User data must be isolated (no cross-user access)
- API keys must be server-side only
- HTTPS required for all connections
- Firebase security rules enforced
- Token-based authentication
- Secure session management

**Status**: ✅ Implemented

**Security Measures**:
- Firebase Authentication
- Server-side token verification
- Firestore security rules
- Environment variable protection
- No client-side API keys
- CORS configuration

---

### Scalability

**Requirements**:
- Support 1000+ concurrent users
- Handle 10,000+ compilations per day
- Firestore queries optimized with indexes
- Serverless architecture for auto-scaling
- Efficient data storage
- CDN for static assets

**Status**: ✅ Architecture supports

**Scalability Features**:
- Next.js serverless functions
- Firebase auto-scaling
- Optimized database queries
- Efficient caching strategies

---

### Accessibility

**Requirements**:
- Keyboard navigation support
- Screen reader compatibility
- High contrast mode support
- ARIA labels on interactive elements
- Focus indicators visible
- Touch targets ≥ 44px (mobile)
- Semantic HTML structure

**Status**: 🔄 Partial (ongoing improvements)

**Implemented**:
- ARIA labels on buttons
- Semantic HTML
- Keyboard navigation
- Focus states
- Touch-friendly controls

**Planned**:
- Enhanced screen reader support
- Keyboard shortcuts
- High contrast theme
- Voice control support

---

### Browser Compatibility

**Requirements**:
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)
- Mobile browsers (iOS Safari, Chrome Mobile)
- Samsung Internet
- Firefox Mobile

**Status**: ✅ Supported

**Testing**:
- Cross-browser testing completed
- Mobile browser testing completed
- Progressive enhancement approach

---

### Device Compatibility

**Requirements**:
- Desktop (≥ 1024px)
- Laptop (≥ 1024px)
- Tablet (768px - 1023px)
- Mobile (< 768px)
- Touch devices
- Mouse/trackpad devices

**Status**: ✅ Fully supported

**Responsive Breakpoints**:
- Mobile: < 1024px
- Desktop: ≥ 1024px
- Adaptive layouts for all sizes

---

## Future Requirements

### Phase 3 Features

**Priority**: Medium

1. **Advanced Collaboration**
   - Real-time multi-user editing
   - Presence indicators
   - Comments and annotations
   - Share canvas via public link
   - Team workspaces

2. **Code Editing**
   - In-app code editor
   - Code execution/preview
   - Live code testing
   - Debug mode
   - Code refactoring suggestions

3. **Customization**
   - Dark/light theme toggle
   - Custom color palettes
   - Toolbar customization
   - Keyboard shortcuts
   - User preferences

4. **Export Options**
   - Export canvas as image (PNG, SVG)
   - Multi-file project generation
   - ZIP download with project structure
   - GitHub repository integration
   - Direct deployment options

5. **Mobile Enhancements**
   - Progressive Web App (PWA)
   - Offline support
   - Install on home screen
   - Push notifications
   - Native app feel

---

### Phase 4 Features

**Priority**: Low

1. **AI Enhancements**
   - Multiple AI model options (GPT-4, Claude)
   - Custom prompts and instructions
   - Code optimization suggestions
   - Bug detection and fixes
   - Code refactoring
   - Performance analysis

2. **Version Control**
   - Canvas version history
   - Diff visualization
   - Branching and merging
   - Restore to any point in time
   - Commit messages

3. **Integration**
   - GitHub repository integration
   - VS Code extension
   - API for third-party apps
   - Webhook support
   - CI/CD pipeline integration
   - Slack/Discord notifications

4. **Analytics & Insights**
   - Usage statistics dashboard
   - Popular patterns and templates
   - Success metrics tracking
   - User behavior analytics
   - Performance monitoring

5. **Community Features**
   - Share templates publicly
   - Template marketplace
   - User profiles
   - Social features (likes, comments)
   - Leaderboards
   - Featured compilations

---

## Technical Requirements

### Frontend

- **Framework**: Next.js 14+ (App Router)
- **UI Library**: React 18+
- **Canvas**: tldraw 2.4+
- **Styling**: Tailwind CSS 3+
- **Language**: TypeScript 5+
- **Fonts**: Inter, Space Grotesk, JetBrains Mono
- **Syntax Highlighting**: react-syntax-highlighter

### Backend

- **Runtime**: Node.js 20+
- **API**: Next.js API Routes
- **Authentication**: Firebase Auth
- **Database**: Cloud Firestore
- **AI**: Google Gemini 2.0 Flash API
- **Storage**: Firebase Storage (future)

### DevOps

- **Hosting**: Vercel
- **CI/CD**: GitHub Actions (future)
- **Monitoring**: Vercel Analytics
- **Error Tracking**: Console logging (Sentry planned)
- **Version Control**: Git

---

## Success Metrics

### User Engagement
- Daily active users (DAU)
- Average session duration
- Compilations per user
- Return user rate (7-day, 30-day)
- Feature adoption rate

### Technical Metrics
- Compilation success rate (target: >95%)
- Average compilation time (target: <5s)
- API error rate (target: <1%)
- Page load time (target: <3s)
- Mobile performance score (target: >90)

### Business Metrics
- User sign-ups
- User retention (7-day, 30-day)
- Feature usage statistics
- User satisfaction score
- Net Promoter Score (NPS)

---

## Constraints

### Technical Constraints
- Gemini API rate limits
- Firebase free tier limits
- Browser canvas size limits
- Image size limits (20MB)
- Mobile device capabilities
- Network bandwidth on mobile

### Business Constraints
- Free tier for all users (initially)
- No monetization (MVP phase)
- Open source considerations
- Privacy regulations (GDPR, CCPA)

### Time Constraints
- MVP completed: ✅
- Phase 2 features: ✅
- Mobile responsiveness: ✅
- Phase 3 features: Q2 2025
- Phase 4 features: Q4 2025

---

## Dependencies

### External Services
- Google Gemini API (critical)
- Firebase Authentication (critical)
- Cloud Firestore (critical)
- Vercel hosting (critical)

### Libraries
- tldraw (critical)
- Next.js (critical)
- React (critical)
- Tailwind CSS (important)
- react-syntax-highlighter (important)

---

## Risk Assessment

### High Risk
- **Gemini API availability**: Mitigation - fallback to other AI models
- **Firebase quota limits**: Mitigation - monitoring and alerts
- **tldraw breaking changes**: Mitigation - version pinning
- **Mobile performance**: Mitigation - optimization and testing

### Medium Risk
- **Browser compatibility issues**: Mitigation - extensive testing
- **Performance degradation**: Mitigation - performance monitoring
- **Security vulnerabilities**: Mitigation - regular audits
- **Mobile network issues**: Mitigation - offline support (future)

### Low Risk
- **UI/UX issues**: Mitigation - user feedback and iteration
- **Documentation gaps**: Mitigation - continuous documentation
- **Feature creep**: Mitigation - strict prioritization

---

## Compliance

### Data Privacy
- GDPR compliance (EU users)
- CCPA compliance (California users)
- User data deletion on request
- Privacy policy published
- Cookie consent (future)
- Data encryption

### Security
- OWASP Top 10 compliance
- Regular security audits
- Dependency vulnerability scanning
- Secure coding practices
- Penetration testing (future)

---

## Acceptance Criteria

### MVP (Phase 1) - ✅ Complete
- [x] User can sign in with Google
- [x] User can draw on canvas with multiple tools
- [x] User can upload images to canvas
- [x] User can compile canvas to code
- [x] User can see generated code
- [x] User can view compilation history
- [x] User can restore previous compilations
- [x] Application is deployed and accessible
- [x] Basic error handling implemented
- [x] Documentation completed

### Phase 2 - ✅ Complete
- [x] User can select programming language (11 languages)
- [x] User can download generated code
- [x] Syntax highlighting implemented
- [x] Copy to clipboard functionality
- [x] User can use templates (8 templates)
- [x] User can explain generated code
- [x] Modern design with 3D effects
- [x] Beautiful landing page
- [x] Mobile responsive design
- [x] Touch-optimized controls

### Phase 3 - 📋 Planned
- [ ] Real-time collaboration
- [ ] Code editing in-app
- [ ] Dark/light theme toggle
- [ ] PWA support
- [ ] Offline functionality
- [ ] Custom keyboard shortcuts
- [ ] Export canvas as image
- [ ] GitHub integration

---

## Glossary

- **Canvas**: The drawing area where users create diagrams
- **Compilation**: The process of converting a canvas image to code
- **tldraw**: The canvas library used for drawing
- **Gemini**: Google's AI model for code generation
- **Firestore**: Firebase's NoSQL database
- **History**: List of past compilations
- **Streaming**: Real-time code delivery as it's generated
- **Template**: Pre-built canvas diagram for quick start
- **Bottom Sheet**: Mobile UI pattern for code display
- **Overlay**: Mobile UI pattern for history sidebar
- **Responsive**: Adapts to different screen sizes
- **Breakpoint**: Screen width where layout changes

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2024-01-01 | Team | Initial requirements |
| 1.1 | 2024-01-15 | Team | Added language selection |
| 2.0 | 2024-02-01 | Team | Added Phase 2 features |
| 3.0 | 2024-02-11 | Team | Added mobile responsiveness |

---

## Approval

This requirements document has been reviewed and approved for implementation.

**Status**: Approved  
**Current Phase**: Phase 2 Complete ✅  
**Next Review**: 2024-05-01

---

## Summary

**Napkin to Silicon** is a fully-featured, production-ready application that transforms sketches into code using AI. With comprehensive mobile support, 11 programming languages, templates, code explanations, and a beautiful modern design, it provides an exceptional user experience across all devices.

**Key Achievements**:
- ✅ MVP Complete
- ✅ Phase 2 Complete
- ✅ Mobile Responsive
- ✅ Production Ready
- ✅ Zero Build Errors
- ✅ Comprehensive Documentation

**Next Steps**: Phase 3 planning and implementation
