# Project Structure - ImagineMe AI Video

## 📁 Directory Overview

```
imagine-me-ai-video/
├── app/                          # Next.js App Router
│   ├── api/                     # API Routes (Server-side)
│   │   ├── analyze-story/       # Story analysis with Gemini
│   │   │   └── route.ts
│   │   ├── extract-characters/  # Character extraction
│   │   │   └── route.ts
│   │   ├── generate-video/      # Veo 3.1 video generation
│   │   │   └── route.ts
│   │   ├── video-status/        # Poll video generation status
│   │   │   └── route.ts
│   │   ├── transcribe-audio/    # Audio to text transcription
│   │   │   └── route.ts
│   │   ├── ocr-extract/         # Text extraction from images
│   │   │   └── route.ts
│   │   └── generate-image/      # Image generation (Nano Banana)
│   │       └── route.ts
│   │
│   ├── create/                  # Story input pages
│   │   ├── page.tsx            # Input method selection
│   │   ├── text/               # Text input
│   │   │   └── page.tsx
│   │   ├── audio/              # Audio recording/upload
│   │   │   └── page.tsx
│   │   └── ocr/                # Image/OCR upload
│   │       └── page.tsx
│   │
│   ├── characters/              # Character review page
│   │   └── page.tsx
│   │
│   ├── edit-scene/              # Scene editing page
│   │   └── page.tsx
│   │
│   ├── generate/                # Video generation & preview
│   │   └── page.tsx
│   │
│   ├── doppleganger/            # Character assignment
│   │   └── page.tsx
│   │
│   ├── scene-preview/           # Scene preview
│   │   └── page.tsx
│   │
│   ├── page.tsx                 # Landing page
│   ├── layout.tsx               # Root layout
│   └── globals.css              # Global styles
│
├── components/                   # React Components
│   ├── ui/                      # UI Components (shadcn/ui)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── textarea.tsx
│   │   ├── card.tsx
│   │   └── ... (40+ components)
│   └── theme-provider.tsx       # Dark/Light theme provider
│
├── lib/                          # Utility functions
│   ├── gemini.ts                # ⭐ Gemini API client setup
│   └── utils.ts                 # General utilities
│
├── hooks/                        # Custom React hooks
│   ├── use-mobile.ts
│   └── use-toast.ts
│
├── public/                       # Static assets
│   └── ... (images, icons)
│
├── styles/                       # Additional styles
│   └── globals.css
│
├── .env.local                    # ⭐ Environment variables (API keys)
├── package.json                  # Dependencies
├── tsconfig.json                 # TypeScript configuration
├── next.config.mjs              # Next.js configuration
├── tailwind.config.ts           # Tailwind CSS configuration
├── components.json              # shadcn/ui configuration
│
├── README.md                     # ⭐ Main documentation
├── INTEGRATION.md               # ⭐ Integration details
├── QUICKSTART.md                # ⭐ Quick start guide
└── .gitignore                   # Git ignore rules
```

## 🎯 Key Files & Their Purpose

### API Routes (Server-Side Only)

#### `/app/api/analyze-story/route.ts`
- **Purpose**: Analyze story text and extract scenes
- **Model**: Gemini 2.0 Flash
- **Input**: Story text
- **Output**: Array of scene objects with descriptions, camera angles, emotions

#### `/app/api/extract-characters/route.ts`
- **Purpose**: Extract characters from story
- **Model**: Gemini 2.0 Flash
- **Input**: Story text
- **Output**: Array of character objects with descriptions, traits, roles

#### `/app/api/generate-video/route.ts`
- **Purpose**: Start Veo 3.1 video generation
- **Model**: Veo 3.1 Generate Preview
- **Input**: Video prompt and configuration
- **Output**: Operation ID for polling

#### `/app/api/video-status/route.ts`
- **Purpose**: Check video generation progress
- **Input**: Operation ID
- **Output**: Status and video URI when complete

#### `/app/api/transcribe-audio/route.ts`
- **Purpose**: Convert audio to text
- **Model**: Gemini 2.0 Flash with audio input
- **Input**: Audio file (FormData)
- **Output**: Transcribed text

#### `/app/api/ocr-extract/route.ts`
- **Purpose**: Extract text from images
- **Model**: Gemini Vision (2.0 Flash)
- **Input**: Image file (FormData)
- **Output**: Extracted text

#### `/app/api/generate-image/route.ts`
- **Purpose**: Generate images for reference
- **Model**: Nano Banana (Gemini 2.5 Flash Image)
- **Input**: Text prompt
- **Output**: Generated image

### Frontend Pages

#### `/app/page.tsx` - Landing Page
- Hero section with call-to-action
- Feature highlights (Gemini & Veo capabilities)
- CTA section
- Fully updated with AI references

#### `/app/create/page.tsx` - Input Method Selection
- Choose between Text, Audio, or OCR input
- Gateway to different input workflows

#### `/app/create/text/page.tsx` - Text Input
- Text area for story input
- Real-time character count
- Integrates with analyze-story API
- Automatically extracts characters and scenes

#### `/app/create/audio/page.tsx` - Audio Input
- Voice recording functionality
- Audio file upload option
- Integrates with transcribe-audio API
- Automatic story analysis after transcription

#### `/app/create/ocr/page.tsx` - OCR Input
- Image upload interface
- Preview uploaded image
- Integrates with ocr-extract API
- Automatic text extraction and analysis

#### `/app/edit-scene/page.tsx` - Scene Editing
- Review extracted scenes
- Edit scene descriptions
- Adjust camera angles
- Modify dialogue and actions

#### `/app/characters/page.tsx` - Character Review
- View all extracted characters
- Edit character descriptions
- Manage character appearances

#### `/app/generate/page.tsx` - Video Generation
- Veo 3.1 video generation interface
- Real-time progress tracking
- Operation polling (every 10 seconds)
- Video preview and download
- Error handling and retry

### Core Library

#### `/lib/gemini.ts` - Gemini Client
```typescript
// Initialize and export Gemini client
export const getGeminiClient = () => {
  const apiKey = process.env.GEMINI_API_KEY;
  return new genai.Client({ apiKey });
};
```

### Environment Configuration

## 🔄 Data Flow

### Text Input Flow
```
User Input
  ↓
Text Entry
  ↓
POST /api/analyze-story
  ↓
Gemini Analysis (Scenes)
  ↓
POST /api/extract-characters
  ↓
Gemini Analysis (Characters)
  ↓
localStorage (scenes, characters, storyText)
  ↓
Navigate to /edit-scene
```

### Video Generation Flow
```
User Clicks "Generate"
  ↓
Create Veo Prompt from Scene
  ↓
POST /api/generate-video
  ↓
Receive Operation ID
  ↓
Poll /api/video-status every 10s
  ↓
Video Ready (1-6 minutes)
  ↓
Display Video Preview
  ↓
Download Video
```

## 📦 Dependencies

### Core Dependencies
- `next@16.0.7` - React framework
- `react@19.x` - UI library
- `@google/genai` - ⭐ Gemini API SDK
- `typescript` - Type safety

### UI Components
- `@radix-ui/*` - Headless UI components
- `tailwindcss` - Utility-first CSS
- `lucide-react` - Icon library

### Forms & Validation
- `react-hook-form` - Form management
- `zod` - Schema validation

## 🎨 Styling Architecture

### Tailwind Configuration
- Custom color schemes
- Dark/Light mode support
- Responsive breakpoints
- Custom animations

### Component Styling Pattern
```tsx
// Card with hover effects and gradients
<div className="
  rounded-xl border border-border 
  bg-card/50 hover:bg-card 
  hover:border-primary/50 
  transition-all duration-300
">
```

## 🔐 Security Measures

1. **API Keys**: Server-side only, never exposed
2. **File Uploads**: Validated and temporary
3. **Rate Limiting**: Handled by Gemini API
4. **Error Handling**: Graceful fallbacks

## 📊 State Management

### Client-Side State
- React useState for component state
- localStorage for temporary data transfer
- No global state management (Next.js handles routing state)

### Stored Data (localStorage)
- `storyText`: Original story input
- `scenes`: Extracted scene array
- `characters`: Extracted character array

## 🎯 Component Organization

### UI Components (`/components/ui/`)
- Atomic design principles
- Reusable across pages
- Theme-aware
- Fully typed with TypeScript

### Page Components (`/app/*/page.tsx`)
- Route-based organization
- Client components ("use client")
- Integration with API routes
- Navigation logic

## 🚀 Build & Deploy

### Development
```bash
npm run dev
```

### Production Build
```bash
npm run build
npm start
```

### Deployment
- Optimized for Vercel
- Environment variables via Vercel dashboard
- Automatic builds on git push

## 📝 Code Quality

- **TypeScript**: Full type safety
- **ESLint**: Code linting
- **Prettier**: Code formatting (optional)
- **Error Boundaries**: Graceful error handling

---

**Last Updated**: January 3, 2026
**Total Files**: 50+
**Lines of Code**: ~3,500+
