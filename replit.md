# Real-Time Messaging Application

## Overview

This is a modern real-time messaging application built with React and Express, featuring WhatsApp-inspired design and functionality. The application provides private and group chat capabilities with real-time message delivery, read receipts, online status indicators, file sharing, and a responsive interface optimized for both desktop and mobile devices.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework & Build System**
- React 18 with TypeScript for type-safe component development
- Vite as the build tool and dev server for fast development and optimized production builds
- Wouter for lightweight client-side routing (replaces React Router)
- React Query (@tanstack/react-query) for server state management and data fetching

**UI Component Library**
- Radix UI primitives for accessible, unstyled components (@radix-ui/react-*)
- Tailwind CSS for utility-first styling with custom design tokens
- shadcn/ui design system (New York style) for consistent component patterns
- Custom CSS variables for theming with light/dark mode support

**State Management Strategy**
- React Context API for authentication state (AuthContext)
- React Query for server state, caching, and data synchronization
- Local component state with React hooks for UI state
- No global state management library (Redux/Zustand) - intentionally kept simple

**Design System**
- WhatsApp/Telegram-inspired visual language defined in design_guidelines.md
- Color palette: Primary green (#25D366), accent blue (#34B7F1), neutral backgrounds
- Typography: SF Pro Display with Roboto fallback
- Consistent spacing using Tailwind's spacing scale (2, 3, 4, 6, 8 units)
- Three-column desktop layout: sidebar (280px), chat list (320px), conversation area (flexible)

### Backend Architecture

**Server Framework**
- Express.js with TypeScript for the REST API
- HTTP server created with Node's built-in http module
- Custom middleware for request logging and error handling
- Session-based or token-based authentication (implementation in progress)

**Data Layer**
- Drizzle ORM for type-safe database operations
- PostgreSQL as the primary database (via Neon serverless)
- Schema-first approach with shared types between client and server
- Database schema includes: users, chats, chat_participants, messages tables

**Storage Interface Pattern**
- Abstract IStorage interface defining data operations (getUser, getUserByUsername, createUser)
- MemStorage implementation for in-memory development/testing
- Designed to be swapped with database-backed storage without changing business logic
- Enables easy testing and gradual database integration

**API Design**
- RESTful API structure with /api prefix for all endpoints
- JSON request/response format
- Centralized error handling with status codes and error messages
- Route registration pattern in server/routes.ts for organization

### Database Schema

**Users Table**
- UUID primary key with auto-generation
- Username and email with unique constraints
- Hashed password storage (bcryptjs for hashing)
- Profile fields: avatarUrl, status message, online flag, lastSeen timestamp
- Timestamps for account creation tracking

**Chats Table**
- Supports both private and group chat types
- Optional name and avatar for group chats
- Created/updated timestamps for sorting and tracking

**Chat Participants Table**
- Many-to-many relationship between users and chats
- Tracks when users joined and last read timestamp
- Cascade deletion when chat or user is deleted

**Messages Table**
- Links to chat and sender via foreign keys
- Text content and optional file attachments (fileUrl, fileType)
- Message status tracking: sent, delivered, read
- Timestamp for message ordering and display

### Real-Time Communication

**Supabase Integration**
- Supabase client for real-time subscriptions and authentication
- Real-time message updates using Supabase's WebSocket connections
- Configured with persistent sessions and auto token refresh
- Events throttled at 10 per second for performance

**Real-Time Features**
- Message delivery with live updates via subscribeToMessages function
- Online/offline status tracking via updateUserStatus
- Typing indicators (UI implemented, backend integration pending)
- Read receipts with timestamp tracking in chat_participants table

### Authentication & Authorization

**Authentication Flow**
- Supabase Auth for user registration and login
- Email/password authentication with secure password hashing
- Session persistence in browser with automatic refresh
- User profile creation in custom users table upon registration

**Protected Routes**
- ProtectedRoute component wrapping authenticated pages
- AuthContext providing user state and loading status throughout app
- Automatic redirect to /auth for unauthenticated users
- Online status updated on login/logout

### File Upload & Storage

**File Handling**
- Supabase Storage for file uploads (chat-files bucket)
- Separate upload functions for chat files and user avatars
- File type detection based on file extension
- Public URL generation for stored files
- Cache control headers for optimal performance

**Supported Operations**
- uploadFile: General file uploads with user-specific paths
- uploadAvatar: User avatar uploads with overwrite capability
- getFileType: Utility for determining file MIME types

## External Dependencies

### Core Services

**Supabase** (https://supabase.com)
- Backend-as-a-Service providing authentication, database, storage, and real-time subscriptions
- Environment variables required: VITE_SUPABASE_URL, VITE_SUPABASE_ANON_KEY
- Used for user auth, real-time messaging, and file storage

**Neon Database** (https://neon.tech)
- Serverless PostgreSQL database
- Environment variable required: DATABASE_URL
- Accessed via Drizzle ORM with WebSocket support

### Third-Party Libraries

**UI & Styling**
- @radix-ui/react-* (v1.x): Headless UI primitives for accessibility
- tailwindcss: Utility-first CSS framework
- class-variance-authority: Component variant management
- lucide-react: Icon library

**Data & State Management**
- @tanstack/react-query (v5.x): Server state management and caching
- @supabase/supabase-js (v2.x): Supabase JavaScript client
- drizzle-orm: TypeScript ORM for database operations
- react-hook-form + @hookform/resolvers: Form handling with validation

**Development Tools**
- vite: Fast build tool and dev server
- tsx: TypeScript execution for Node.js
- drizzle-kit: Database schema migrations
- @replit/vite-plugin-*: Replit-specific development enhancements

**Utilities**
- date-fns: Date formatting and manipulation
- bcryptjs: Password hashing
- wouter: Lightweight routing
- zod + drizzle-zod: Runtime type validation and schema generation

### Development Workflow

**Database Migrations**
- Run `npm run db:push` to sync schema changes to database
- Migrations stored in /migrations directory
- Schema defined in shared/schema.ts for client-server sharing

**Environment Setup**
- Requires DATABASE_URL for Neon PostgreSQL connection
- Requires VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY for Supabase
- Development mode uses Vite dev server with HMR
- Production build bundles client and server separately