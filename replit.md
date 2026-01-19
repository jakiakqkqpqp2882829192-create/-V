# replit.md

## Overview

This is an AI-powered chat application with a React frontend and Express backend. The application provides a conversational interface with support for text messaging, file/photo attachments, voice interactions, and AI-generated images. Messages are streamed in real-time using Server-Sent Events (SSE).

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite with hot module replacement
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state
- **Styling**: Tailwind CSS with CSS variables for theming
- **UI Components**: shadcn/ui component library built on Radix UI primitives
- **Fonts**: Inter (body text) and Plus Jakarta Sans (headings)

The frontend follows a standard React SPA pattern:
- Pages in `client/src/pages/`
- Reusable components in `client/src/components/`
- Custom hooks in `client/src/hooks/`
- UI primitives in `client/src/components/ui/`

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Database**: PostgreSQL with Drizzle ORM
- **Schema Location**: `shared/schema.ts` (shared between frontend and backend)
- **API Pattern**: REST endpoints with SSE for streaming responses

The server uses a modular integration system located in `server/replit_integrations/`:
- `chat/` - Text chat with OpenAI streaming
- `audio/` - Voice recording, speech-to-text, and text-to-speech
- `image/` - AI image generation using gpt-image-1
- `object_storage/` - File uploads with presigned URLs (Google Cloud Storage)
- `batch/` - Batch processing utilities with rate limiting

### Data Storage
- **PostgreSQL** for persistent storage via Drizzle ORM
- Database schema defines `conversations` and `messages` tables
- Messages support attachments (file URLs stored as text array)
- Migrations stored in `/migrations` directory

### API Structure
Routes are defined in `shared/routes.ts` using Zod schemas for type-safe API contracts:
- `GET /api/conversations` - List all conversations
- `POST /api/conversations` - Create new conversation
- `GET /api/conversations/:id` - Get conversation with messages
- `DELETE /api/conversations/:id` - Delete conversation
- `POST /api/conversations/:id/messages` - Send message (supports SSE streaming)

### AI Integration
- Uses OpenAI API via Replit AI Integrations
- Environment variables: `AI_INTEGRATIONS_OPENAI_API_KEY`, `AI_INTEGRATIONS_OPENAI_BASE_URL`
- Streaming responses for real-time chat experience
- Voice chat uses WebM audio from browser, converted to WAV via ffmpeg

## External Dependencies

### AI Services
- **OpenAI API** (via Replit AI Integrations) - Chat completions, image generation, speech-to-text, text-to-speech

### Cloud Storage
- **Google Cloud Storage** - Object storage for file uploads with presigned URLs
- Accessed via `@google-cloud/storage` SDK through Replit sidecar endpoint

### Database
- **PostgreSQL** - Primary database, connection via `DATABASE_URL` environment variable
- **Drizzle ORM** - Type-safe database queries and schema management

### File Upload
- **Uppy** - Frontend file upload library with AWS S3-compatible presigned URL support
- Dashboard modal interface for file selection and progress

### Key NPM Packages
- `express` / `express-session` - HTTP server and session management
- `drizzle-orm` / `drizzle-kit` - Database ORM and migrations
- `@tanstack/react-query` - Server state management
- `react-markdown` - Rendering AI responses with formatting
- `framer-motion` - Message animations
- `date-fns` - Date formatting