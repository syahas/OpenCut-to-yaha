About Me / Our Story
We Don’t Conquer Nature. We Are Allowed to Return.

For over twelve years, I have explored jungles and rainforests across the world.
From dense tropical forests to remote, unforgiving terrain, I’ve learned one truth that experience alone cannot replace:

Skill helps you move forward. Safety is what brings you home.

In August 2021, that truth came at a devastating cost.

I was deep in the central rainforest of Papua New Guinea with my expedition partner and close friend, James. We were both experienced jungle explorers—but our equipment was not sufficient for the extreme conditions we encountered.

Heavy rain, dropping temperatures, and limited shelter led to a critical situation.
James developed severe hypothermia.

In the final moments, he gave me the only remaining gear that could keep someone alive.

Because of that choice, I survived.
James did not.

He remains forever in the rainforest of Papua New Guinea.

That loss changed everything.

I realized something many adventurers learn too late:
When an expedition ends safely, it’s not because we conquered nature—it’s because nature allowed us to leave.

From that day forward, I could no longer treat outdoor gear as just products.
In extreme environments, equipment is not a feature list—it is a lifeline.

Why This Brand Exists

This store is built on responsibility, not marketing hype.

Every product I share or sell is selected through real-world jungle experience, tested with one question in mind:

Would I trust this with my life?

My goal is simple:
To help outdoor enthusiasts return home safely—to their families, their friends, their lives.

Giving Back

To honor James and his family, I have made a permanent commitment:

70% of all product profits are donated to:

Supporting James’ family and children

Environmental protection and conservation efforts

Responsible outdoor education and safety awareness

This is not a campaign.
It is a promise.

A Final Word

If you choose our equipment, you’re not just buying gear.
You’re supporting a belief:

That nature deserves respect.
That preparation matters.
And that the true success of any adventure is coming home alive.

Thank you for being part of this journey.

—

Founder & Jungle Survival Enthusiast
12+ Years of Field Experience



# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

OpenCut is a free, open-source video editor built with Next.js, focusing on privacy (no server processing), multi-track timeline editing, and real-time preview. The project is a monorepo using Turborepo with multiple apps including a web application, desktop app (Tauri), background remover tools, and transcription services.

## Essential Commands

**Development:**
```bash
# Root level development
bun dev                    # Start all apps in development mode
bun build                  # Build all apps
bun lint                   # Lint all code using Ultracite
bun format                 # Format all code using Ultracite

# Web app specific (from apps/web/)
cd apps/web
bun run dev                # Start Next.js development server with Turbopack
bun run build              # Build for production
bun run lint               # Run Biome linting
bun run lint:fix           # Fix linting issues automatically
bun run format             # Format code with Biome

# Database operations (from apps/web/)
bun run db:generate        # Generate Drizzle migrations
bun run db:migrate         # Run migrations
bun run db:push:local      # Push schema to local development database
bun run db:push:prod       # Push schema to production database
```

**Testing:**
- No unified test commands are currently configured
- Individual apps may have their own test setups

## Architecture & Key Components

### State Management
The application uses **Zustand** for state management with separate stores for different concerns:
- **editor-store.ts**: Canvas presets, layout guides, app initialization
- **timeline-store.ts**: Timeline tracks, elements, playback state
- **media-store.ts**: Media files and asset management
- **playback-store.ts**: Video playback controls and timing
- **project-store.ts**: Project-level data and persistence
- **panel-store.ts**: UI panel visibility and layout
- **keybindings-store.ts**: Keyboard shortcut management
- **sounds-store.ts**: Audio effects and sound management
- **stickers-store.ts**: Sticker/graphics management

### Storage System
**Multi-layer storage approach:**
- **IndexedDB**: Projects, saved sounds, and structured data
- **OPFS (Origin Private File System)**: Large media files for better performance
- **Storage Service** (`lib/storage/`): Abstraction layer managing both storage types

### Editor Architecture
**Core editor components:**
- **Timeline Canvas**: Custom canvas-based timeline with tracks and elements
- **Preview Panel**: Real-time video preview (currently DOM-based, planned binary refactor)
- **Media Panel**: Asset management with drag-and-drop support
- **Properties Panel**: Context-sensitive element properties

### Media Processing
- **FFmpeg Integration**: Client-side video processing using @ffmpeg/ffmpeg
- **Background Removal**: Python-based tools with multiple AI models (U2Net, SAM, Gemini)
- **Transcription**: Separate service for audio-to-text conversion

## Development Focus Areas

**✅ Recommended contribution areas:**
- Timeline functionality and UI improvements
- Project management features
- Performance optimizations
- Bug fixes in existing functionality
- UI/UX improvements outside preview panel
- Documentation and testing

**⚠️ Areas to avoid (pending refactor):**
- Preview panel enhancements (fonts, stickers, effects)
- Export functionality improvements
- Preview rendering optimizations

**Reason:** The preview system is planned for a major refactor from DOM-based rendering to binary rendering for consistency with export and better performance.

## Code Quality Standards

**Linting & Formatting:**
- Uses **Biome** for JavaScript/TypeScript linting and formatting
- Extends **Ultracite** configuration for strict type safety and AI-friendly code
- Comprehensive accessibility (a11y) rules enforced
- Zero configuration approach with subsecond performance

**Key coding standards from Ultracite:**
- Strict TypeScript with no `any` types
- No React imports (uses automatic JSX runtime)
- Comprehensive accessibility requirements
- Use `for...of` instead of `Array.forEach`
- No TypeScript enums, use const objects
- Always include error handling with try-catch

## Environment Setup

**Required environment variables (apps/web/.env.local):**
```bash
# Database
DATABASE_URL="postgresql://opencut:opencutthegoat@localhost:5432/opencut"

# Authentication
BETTER_AUTH_SECRET="your-generated-secret-here"
BETTER_AUTH_URL="http://localhost:3000"

# Redis
UPSTASH_REDIS_REST_URL="http://localhost:8079"
UPSTASH_REDIS_REST_TOKEN="example_token"

# Content Management
MARBLE_WORKSPACE_KEY="workspace-key"
NEXT_PUBLIC_MARBLE_API_URL="https://api.marblecms.com"
```

**Docker services:**
```bash
# Start local database and Redis
docker-compose up -d
```

## Project Structure

**Monorepo layout:**
- `apps/web/` - Main Next.js application
- `apps/desktop/` - Tauri desktop application
- `apps/bg-remover/` - Python background removal tools
- `apps/transcription/` - Audio transcription service
- `packages/` - Shared packages (auth, database)

**Web app structure:**
- `src/components/` - React components organized by feature
- `src/stores/` - Zustand state management
- `src/hooks/` - Custom React hooks
- `src/lib/` - Utility functions and services
- `src/types/` - TypeScript type definitions
- `src/app/` - Next.js app router pages and API routes

## Common Patterns

**Error handling:**
```typescript
try {
  const result = await processData();
  return { success: true, data: result };
} catch (error) {
  console.error('Operation failed:', error);
  return { success: false, error: error.message };
}
```

**Store usage:**
```typescript
const { tracks, addTrack, updateTrack } = useTimelineStore();
```

**Media processing:**
```typescript
import { processVideo } from '@/lib/ffmpeg-utils';
const processedVideo = await processVideo(inputFile, options);
```
