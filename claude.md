# INRI Church App - Claude Development Guide

## Project Overview

This is a web application for INRI Church in Stockholm, built with React and Supabase. The app serves as a community platform for church members with features for news feed, messaging, mission information, and video library. Designed as a Progressive Web App (PWA) for mobile-first experience.

## Tech Stack

- **Frontend**: React 18 + TypeScript + Vite
- **Styling**: Tailwind CSS + Headless UI
- **Backend**: Supabase (PostgreSQL, Auth, Storage, Realtime)
- **Routing**: React Router v6
- **State Management**: Zustand + React Query
- **PWA**: Vite PWA Plugin
- **Video**: Video.js or React Player
- **Notifications**: Web Push API + Supabase Edge Functions
- **Deployment**: Vercel or Netlify

## Project Structure

```
inri-church-app/
├── public/                  # Static assets
│   ├── manifest.json        # PWA manifest
│   ├── sw.js               # Service worker
│   └── icons/              # PWA icons
├── src/
│   ├── components/         # Reusable UI components
│   ├── pages/              # Page components
│   │   ├── Home/           # News feed
│   │   ├── Messages/       # Chat functionality
│   │   ├── Mission/        # Mission information
│   │   ├── Library/        # Video library
│   │   └── Auth/           # Login/Register
│   ├── hooks/              # Custom hooks
│   ├── services/           # API services (Supabase)
│   ├── stores/             # Zustand stores
│   ├── types/              # TypeScript type definitions
│   ├── utils/              # Utility functions
│   └── constants/          # App constants and config
├── docs/                   # Documentation
├── supabase/               # Database migrations and types
└── dist/                   # Build output
```

## Design System

### Colors (Based on INRI Church Brand)
```typescript
export const colors = {
  primary: {
    dark: '#1B365D',    // Deep blue
    gold: '#D4AF37',    // Gold accent
  },
  secondary: {
    white: '#FFFFFF',
    lightGray: '#F5F5F5',
    darkGray: '#333333',
    red: '#8B0000',
  },
  ui: {
    background: '#FFFFFF',
    surface: '#F5F5F5',
    border: '#E5E5E5',
    text: '#333333',
    textSecondary: '#666666',
  }
}
```

### Typography
- **Primary Font**: System default (San Francisco iOS, Roboto Android)
- **Sizes**: 12, 14, 16, 18, 20, 24, 32px
- **Weights**: 400 (regular), 600 (semibold), 700 (bold)

## Core Features & Requirements

### 1. Authentication
- **Supabase Auth** with email/password + social logins
- User roles: `member`, `pastor`, `admin`
- Protected routes for authenticated users only
- Profile management with avatar upload
- **PWA-compatible** auth flow

### 2. Home Page (News Feed)
- Display posts from pastors and admins
- Members can view, like, and comment
- Real-time updates for new posts
- Image support in posts
- **Push notifications** via Web Push API
- **Responsive design** for mobile/desktop

### 3. Messages Page
- **1-on-1 messaging** between members
- **Group chats** for work groups
- Real-time messaging with Supabase Realtime
- Message history and read receipts
- File sharing capabilities
- **Mobile-optimized** chat interface

### 4. Mission Page
- Information about ongoing mission projects
- Photo galleries from mission trips
- Interactive elements for engagement
- Updates from mission field
- **Responsive image galleries**

### 5. Library Page
- **Video library** for sermons and teachings
- Categories and search functionality
- **Members-only access** with authentication
- **Progressive loading** for better performance
- Video player with custom controls
- **Mobile-friendly** video interface

## Database Schema (Supabase)

### Core Tables
```sql
-- User profiles (extends auth.users)
profiles: id, email, full_name, avatar_url, role, phone, created_at, updated_at

-- Posts for news feed
posts: id, author_id, title, content, image_url, created_at, updated_at

-- Comments on posts
post_comments: id, post_id, author_id, content, created_at

-- Messages
messages: id, conversation_id, sender_id, content, message_type, file_url, created_at

-- Conversations (1-on-1 and groups)
conversations: id, name, is_group, created_by, created_at

-- Conversation participants
conversation_participants: conversation_id, user_id, joined_at

-- Mission projects
missions: id, title, description, location, status, start_date, end_date, image_urls, created_at

-- Video library
videos: id, title, description, video_url, thumbnail_url, speaker, category, duration, upload_date, is_featured
```

### Row Level Security (RLS)
- **All tables have RLS enabled**
- Only authenticated members can access content
- Role-based permissions (pastors can create posts, etc.)
- Users can only see conversations they're part of

## Getting Started Commands

### **Initial Project Setup**
When starting the project, use these commands with Claude Code:

```bash
# 1. Initialize project
claude-code "Following claude.md specifications, create a new Vite + React + TypeScript project for INRI Church web app. Install Supabase client, Tailwind CSS, React Router v6, Zustand, React Query, and Vite PWA plugin. Set up the exact folder structure specified in the project structure section."

# 2. Create project status tracking
claude-code "Create projectstatus.md file as specified in claude.md with initial project status. Set up basic project files including .env.example, .gitignore, and README.md."

# 3. Configure development environment  
claude-code "Configure Tailwind CSS with the INRI Church color scheme from claude.md. Set up Supabase client configuration and create the basic authentication store using Zustand as specified."

# 4. Set up routing and layout
claude-code "Implement React Router setup with protected routes as outlined in claude.md. Create the basic layout component with navigation for Home, Messages, Mission, and Library pages."

# 5. Database setup preparation
claude-code "Based on the database schema in claude.md, provide the SQL commands needed to set up the Supabase database tables with proper RLS policies for the INRI Church app."
```

### **Development Workflow**
Always reference claude.md when giving instructions:
- "According to claude.md specifications..."
- "Following the tech stack defined in claude.md..."
- "Based on the database schema in claude.md..."
- "Using the design system from claude.md..."

## Project Status Tracking

### **IMPORTANT: projectstatus.md Management**

**Claude must ALWAYS maintain a `projectstatus.md` file to track development progress.**

#### **Initial Setup**
- **First task**: Create `projectstatus.md` in the project root
- **Purpose**: Track current development status, completed features, and next steps
- **Update frequency**: After EVERY significant change or feature implementation

#### **Required Content in projectstatus.md**
```markdown
# INRI Church App - Project Status

## Current Phase: [Phase Name]
**Last Updated**: [Date and Time]
**Updated By**: Claude

## Completed Features
- [x] Feature 1 - Description
- [x] Feature 2 - Description

## In Progress
- [ ] Feature being worked on
- [ ] Current task details

## Next Steps
1. Immediate next task
2. Following task
3. Upcoming priorities

## Technical Status
- **Dependencies installed**: Yes/No
- **Supabase configured**: Yes/No
- **Authentication working**: Yes/No
- **Database schema created**: Yes/No

## Current Issues
- Issue 1: Description and potential solution
- Issue 2: Description and potential solution

## Files Modified Since Last Update
- file1.js - What was changed
- file2.tsx - What was changed

## Environment Status
- Development: Working/Issues
- Testing: Working/Issues
- Production: Working/Issues

## Notes
- Any important observations
- Decisions made
- Technical debt to address
```

#### **Update Protocol**
1. **Before starting any new task**: Read current `projectstatus.md`
2. **After completing any task**: Update the file with:
   - Move completed items from "In Progress" to "Completed"
   - Add new items to "In Progress" or "Next Steps"
   - Update "Technical Status"
   - Note any new issues or resolutions
   - Update "Last Updated" timestamp
3. **Always reference this file** when asked about project progress

## Development Guidelines

### Code Style
- Use **TypeScript** for all new code
- Follow **React hooks** patterns and functional components
- Use **custom hooks** for reusable logic
- Implement proper **error handling** and **loading states**
- **Mobile-first responsive design**
- **Accessibility** (ARIA labels, keyboard navigation)

### State Management
- **Zustand** for global state (lightweight alternative to Redux)
- **React Query** for server state and caching
- **Custom hooks** for component-specific logic
- **Local storage** for persisting user preferences

### Performance & PWA
- **Code splitting** with React.lazy()
- **Image optimization** with next/image-like solutions
- **Service worker** for offline functionality
- **Web Push notifications**
- **Responsive images** and **lazy loading**
- **Bundle size optimization**

### Error Handling
- Global error boundary for React errors
- Try-catch blocks for async operations
- User-friendly error messages with toast notifications
- Error logging for debugging
- Graceful fallbacks for failed operations

## Supabase Integration

### Supabase Integration
```typescript
// Initialize Supabase client
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
)

// Auth with Zustand store
import { create } from 'zustand'

interface AuthStore {
  user: User | null
  signIn: (email: string, password: string) => Promise<void>
  signOut: () => Promise<void>
}

const useAuthStore = create<AuthStore>((set) => ({
  user: null,
  signIn: async (email, password) => {
    const { data, error } = await supabase.auth.signInWithPassword({
      email,
      password,
    })
    if (data.user) set({ user: data.user })
  },
  signOut: async () => {
    await supabase.auth.signOut()
    set({ user: null })
  },
}))
```

### Real-time Subscriptions
```typescript
// Subscribe to new messages with React Query
import { useQuery, useQueryClient } from '@tanstack/react-query'

const useMessages = (conversationId: string) => {
  const queryClient = useQueryClient()
  
  useEffect(() => {
    const channel = supabase
      .channel(`messages:${conversationId}`)
      .on('postgres_changes', 
        { event: 'INSERT', schema: 'public', table: 'messages' },
        (payload) => {
          queryClient.invalidateQueries(['messages', conversationId])
        }
      )
      .subscribe()
      
    return () => channel.unsubscribe()
  }, [conversationId])
  
  return useQuery(['messages', conversationId], () => 
    fetchMessages(conversationId)
  )
}
```

### PWA Configuration
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}']
      },
      manifest: {
        name: 'INRI Church',
        short_name: 'INRI',
        theme_color: '#1B365D',
        background_color: '#ffffff',
        display: 'standalone',
        icons: [
          {
            src: '/icon-192.png',
            sizes: '192x192',
            type: 'image/png'
          },
          {
            src: '/icon-512.png',
            sizes: '512x512',
            type: 'image/png'
          }
        ]
      }
    })
  ]
})
```

## Security Considerations

### Row Level Security Examples
```sql
-- Only members can view posts
CREATE POLICY "Members can view posts" ON posts
  FOR SELECT USING (
    EXISTS (SELECT 1 FROM profiles WHERE profiles.id = auth.uid())
  );

-- Only pastors/admins can create posts
CREATE POLICY "Pastors can create posts" ON posts
  FOR INSERT WITH CHECK (
    EXISTS (
      SELECT 1 FROM profiles 
      WHERE profiles.id = auth.uid() 
      AND role IN ('pastor', 'admin')
    )
  );
```

### Data Validation
- Validate all user inputs
- Sanitize content before storing
- Use TypeScript for type safety
- Implement proper file upload restrictions

## Testing Strategy

### Unit Tests
- Test custom hooks
- Test utility functions
- Test component logic

### Integration Tests
- Test Supabase integration
- Test authentication flow
- Test real-time functionality

### E2E Tests (Optional)
- Test complete user journeys
- Test critical app flows

## Deployment

### Environment Setup
```env
# .env.local
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_anon_key
VITE_APP_URL=http://localhost:5173
```

### Build Configuration
```json
// package.json scripts
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview",
    "deploy": "npm run build && vercel --prod"
  }
}
```

### Vercel Deployment
```json
// vercel.json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ],
  "headers": [
    {
      "source": "/sw.js",
      "headers": [
        { "key": "Service-Worker-Allowed", "value": "/" }
      ]
    }
  ]
}
```

## Detailed Implementation Plan

### **PHASE 1: Foundation & Setup (Week 1-2)**

#### **Step 1.1: Project Initialization**
```bash
claude-code "Following claude.md specifications, create a new Vite + React + TypeScript project for INRI Church web app. Install Supabase client, Tailwind CSS, React Router v6, Zustand, React Query, and Vite PWA plugin. Set up the exact folder structure specified in the project structure section."
```
**Expected Output:**
- Vite project with TypeScript template
- All dependencies installed
- Folder structure created as per specifications
- Basic Vite configuration

#### **Step 1.2: Environment & Configuration Setup**
```bash
claude-code "Create projectstatus.md file as specified in claude.md with initial project status. Set up .env.example with all required environment variables, create .gitignore for React projects, and configure package.json scripts for development and deployment."
```
**Expected Output:**
- projectstatus.md with initial status
- .env.example with VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY
- Proper .gitignore file
- Updated package.json scripts

#### **Step 1.3: Design System & Styling**
```bash
claude-code "Configure Tailwind CSS with the INRI Church color scheme from claude.md. Create a design system with all colors, typography, and common UI components. Set up the color palette as CSS custom properties and create base component variants."
```
**Expected Output:**
- Tailwind config with custom colors
- CSS variables for color scheme
- Base component library (Button, Input, Card, etc.)
- Typography system implemented

#### **Step 1.4: Supabase Integration**
```bash
claude-code "Set up Supabase client configuration in src/services/supabase.ts. Create TypeScript types for all database tables as specified in claude.md schema. Set up the authentication store using Zustand with sign-in, sign-out, and user state management."
```
**Expected Output:**
- Supabase client properly configured
- TypeScript interfaces for all DB tables
- Zustand auth store with complete auth flow
- Environment variable integration

#### **Step 1.5: Routing & Layout Structure**
```bash
claude-code "Implement React Router setup with protected routes as outlined in claude.md. Create the main layout component with navigation for Home, Messages, Mission, and Library pages. Include mobile-responsive navigation with proper accessibility."
```
**Expected Output:**
- React Router v6 configuration
- Protected route components
- Main layout with responsive navigation
- Basic page components created

### **PHASE 2: Authentication & User Management (Week 3)**

#### **Step 2.1: Authentication Pages**
```bash
claude-code "Create complete authentication flow including Login and Register pages. Implement form validation, error handling, and loading states. Include social login options and proper mobile-responsive design following the INRI Church design system."
```
**Expected Output:**
- Login/Register forms with validation
- Error handling and loading states
- Mobile-responsive design
- Integration with Zustand auth store

#### **Step 2.2: User Profile Management**
```bash
claude-code "Implement user profile page with avatar upload to Supabase Storage, profile editing capabilities, and role display. Include proper image handling, compression, and security measures as specified in claude.md."
```
**Expected Output:**
- Profile page with avatar upload
- Image compression and validation
- Profile editing functionality
- Role-based UI elements

#### **Step 2.3: Database Setup & RLS Policies**
```bash
claude-code "Based on the database schema in claude.md, provide the complete SQL commands to set up all Supabase database tables with proper Row Level Security policies. Include all tables: profiles, posts, post_comments, messages, conversations, conversation_participants, missions, and videos."
```
**Expected Output:**
- Complete SQL schema for all tables
- RLS policies for security
- Database functions and triggers
- Initial seed data scripts

### **PHASE 3: Home Page & Posts System (Week 4)**

#### **Step 3.1: Posts Feed Implementation**
```bash
claude-code "Create the Home page with posts feed functionality. Implement infinite scrolling, real-time updates using Supabase Realtime, and proper loading states. Include post creation form for pastors/admins with image upload capabilities."
```
**Expected Output:**
- Home page with posts feed
- Infinite scrolling with React Query
- Real-time subscriptions
- Post creation with image upload

#### **Step 3.2: Comments & Interactions**
```bash
claude-code "Implement comment system for posts with real-time updates. Add like/reaction functionality, proper comment threading, and moderation capabilities. Include mobile-optimized comment interface."
```
**Expected Output:**
- Comment system with real-time updates
- Like/reaction functionality
- Comment moderation features
- Mobile-optimized UI

#### **Step 3.3: Content Management**
```bash
claude-code "Create admin/pastor dashboard for content management. Include post moderation, user management, and analytics. Implement role-based access control throughout the application."
```
**Expected Output:**
- Admin dashboard with content management
- User role management
- Basic analytics and reporting
- Role-based access control

### **PHASE 4: Messaging System (Week 5-6)**

#### **Step 4.1: Chat Infrastructure**
```bash
claude-code "Implement the messaging system foundation with conversation creation, participant management, and message storage. Set up Supabase Realtime for live messaging and create the message data models."
```
**Expected Output:**
- Conversation management system
- Message storage and retrieval
- Real-time messaging infrastructure
- User participant management

#### **Step 4.2: Chat Interface**
```bash
claude-code "Create the Messages page with chat interface. Implement conversation list, message history, typing indicators, and read receipts. Design mobile-first responsive chat UI with proper accessibility."
```
**Expected Output:**
- Complete chat interface
- Conversation list with search
- Message history with pagination
- Typing indicators and read receipts

#### **Step 4.3: Group Chat Features**
```bash
claude-code "Extend messaging system with group chat functionality for work groups. Include group creation, member management, group settings, and file sharing capabilities."
```
**Expected Output:**
- Group chat creation and management
- Member invitation system
- Group settings and permissions
- File sharing in conversations

### **PHASE 5: Mission & Library Pages (Week 7)**

#### **Step 5.1: Mission Information Page**
```bash
claude-code "Create the Mission page with project listings, photo galleries, and interactive elements. Implement image galleries with lightbox functionality, mission project details, and progress tracking."
```
**Expected Output:**
- Mission page with project listings
- Photo galleries with lightbox
- Project details and progress tracking
- Interactive mission map (optional)

#### **Step 5.2: Video Library Implementation**
```bash
claude-code "Implement the Library page with video management, categorization, and search functionality. Include video player with custom controls, playlist features, and member-only access control."
```
**Expected Output:**
- Video library with categorization
- Custom video player interface
- Search and filter functionality
- Playlist and bookmark features

#### **Step 5.3: Media Management**
```bash
claude-code "Create media upload and management system for pastors/admins. Include video processing, thumbnail generation, and storage optimization using Supabase Storage."
```
**Expected Output:**
- Media upload interface
- Video processing pipeline
- Thumbnail generation
- Storage optimization

### **PHASE 6: PWA & Advanced Features (Week 8)**

#### **Step 6.1: Progressive Web App Setup**
```bash
claude-code "Configure PWA functionality with service worker, offline capabilities, and app installation. Implement caching strategies for posts, messages, and media content."
```
**Expected Output:**
- PWA configuration complete
- Service worker with caching
- Offline functionality
- App installation prompts

#### **Step 6.2: Push Notifications**
```bash
claude-code "Implement Web Push notifications using Supabase Edge Functions. Set up notification triggers for new posts, messages, and important updates. Include user notification preferences."
```
**Expected Output:**
- Web Push notification system
- Notification triggers and logic
- User notification preferences
- Supabase Edge Functions for notifications

#### **Step 6.3: Search & Advanced Features**
```bash
claude-code "Implement global search functionality across posts, messages, and videos. Add advanced features like content filtering, bookmarks, and user preferences."
```
**Expected Output:**
- Global search functionality
- Content filtering and sorting
- Bookmark system
- User preference management

### **PHASE 7: Testing & Optimization (Week 9)**

#### **Step 7.1: Performance Optimization**
```bash
claude-code "Optimize application performance with code splitting, lazy loading, image optimization, and bundle size reduction. Implement performance monitoring and analytics."
```
**Expected Output:**
- Code splitting implemented
- Lazy loading for routes and components
- Image optimization
- Performance monitoring setup

#### **Step 7.2: Testing Implementation**
```bash
claude-code "Set up testing framework with unit tests for components, integration tests for Supabase operations, and end-to-end tests for critical user flows."
```
**Expected Output:**
- Testing framework setup (Vitest)
- Unit tests for key components
- Integration tests for API operations
- E2E tests for critical flows

#### **Step 7.3: Security & Accessibility Audit**
```bash
claude-code "Conduct security audit of RLS policies, implement accessibility improvements, and ensure GDPR compliance. Add security headers and content security policies."
```
**Expected Output:**
- Security audit completed
- Accessibility improvements
- GDPR compliance measures
- Security headers configured

### **PHASE 8: Deployment & Launch (Week 10)**

#### **Step 8.1: Production Setup**
```bash
claude-code "Configure production environment with Vercel deployment, environment variables setup, and domain configuration. Set up CI/CD pipeline for automated deployments."
```
**Expected Output:**
- Production deployment on Vercel
- Environment variables configured
- Custom domain setup
- CI/CD pipeline active

#### **Step 8.2: Launch Preparation**
```bash
claude-code "Prepare for launch with user documentation, admin training materials, and launch checklist. Set up monitoring, error tracking, and backup procedures."
```
**Expected Output:**
- User documentation complete
- Admin training materials
- Monitoring and error tracking
- Backup and recovery procedures

#### **Step 8.3: Post-Launch Support**
```bash
claude-code "Set up post-launch monitoring, user feedback collection, and maintenance procedures. Create troubleshooting guides and support documentation."
```
**Expected Output:**
- Monitoring dashboards
- User feedback system
- Maintenance procedures
- Support documentation

### **Implementation Notes**

#### **After Each Phase:**
1. Update projectstatus.md with completed features
2. Test functionality on mobile and desktop
3. Review security and performance
4. Document any technical debt
5. Plan next phase priorities

#### **Quality Checkpoints:**
- **Week 2**: Technical foundation review
- **Week 4**: User authentication and basic features
- **Week 6**: Core functionality complete
- **Week 8**: Advanced features and PWA
- **Week 10**: Production ready

#### **Risk Mitigation:**
- Regular Supabase backup procedures
- Feature flags for gradual rollout
- Rollback procedures for each deployment
- User acceptance testing with church members

This implementation plan ensures systematic development while maintaining code quality and user experience throughout the process.

## Troubleshooting

### Common Issues
1. **Vite build issues**: Check import paths and environment variables
2. **Supabase connection**: Verify environment variables and CORS settings
3. **Authentication**: Check RLS policies and user permissions
4. **Real-time not working**: Verify Supabase real-time subscription setup
5. **PWA not installing**: Check manifest.json and service worker configuration
6. **Mobile responsiveness**: Test on different screen sizes regularly

### Debugging Tips
- Use **React Developer Tools** browser extension
- Enable **Supabase logs** in development
- Use **Network tab** to debug API calls
- Test **PWA functionality** in Chrome DevTools > Application tab
- Use **Lighthouse** for performance and PWA audits
- Test on **actual mobile devices** regularly

## Resources

### Documentation
- [Vite Documentation](https://vitejs.dev/)
- [React Documentation](https://react.dev/)
- [Supabase Documentation](https://supabase.com/docs)
- [React Router](https://reactrouter.com/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Zustand](https://zustand-demo.pmnd.rs/)
- [React Query](https://tanstack.com/query/)

### Supabase Resources
- [Supabase Auth with React](https://supabase.com/docs/guides/getting-started/tutorials/with-react)
- [Row Level Security](https://supabase.com/docs/guides/auth/row-level-security)
- [Real-time subscriptions](https://supabase.com/docs/guides/realtime)

### PWA Resources
- [PWA Developer Guide](https://web.dev/progressive-web-apps/)
- [Vite PWA Plugin](https://vite-pwa-org.netlify.app/)
- [Web Push Notifications](https://web.dev/push-notifications/)

Remember: This is a **Progressive Web App** designed for church community use, so prioritize **mobile-first design**, **accessibility**, **offline functionality**, and **security** in all development decisions.