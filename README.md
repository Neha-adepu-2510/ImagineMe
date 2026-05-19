# ImagineMe AI Video - Google Gemini & Veo 3.1 Integration

Transform stories into stunning AI-generated videos using Google's Gemini AI and Veo 3.1.

## 🚀 Features

### AI-Powered Story Analysis
- **Gemini 2.0 Flash** for intelligent story analysis
- Automatic scene extraction and breakdown
- Character identification and profiling
- Camera angle and emotional tone suggestions

### Multiple Input Methods
1. **Text Input** - Type or paste your story
2. **Audio to Text** - Record or upload audio files with automatic transcription
3. **OCR Upload** - Scan book pages or images to extract text

### Advanced Video Generation
- **Google Veo 3.1** - State-of-the-art video generation model
- 720p/1080p high-quality output
- Native audio generation
- 8-second cinematic videos
- 16:9 and 9:16 aspect ratios

## 🛠️ Technology Stack

- **Frontend**: Next.js 16, React, TypeScript, Tailwind CSS
- **AI Models**:
  - Gemini 2.0 Flash (Story analysis, character extraction)
  - Gemini 2.5 Flash Image / Nano Banana (Image generation)
  - Gemini Vision (OCR text extraction)
  - Veo 3.1 (Video generation)
- **UI Components**: Radix UI, shadcn/ui

## 📋 Prerequisites

- Node.js 18+ and npm
- Google Gemini API key

## ⚙️ Installation

1. **Clone the repository**
```bash
git clone <your-repo-url>
cd imagine-me-ai-video
```

2. **Install dependencies**
```bash
npm install
```

3. **Set up environment variables**

Create a `.env.local` file in the root directory:
```env
GEMINI_API_KEY=your_gemini_api_key_here
```

4. **Run the development server**
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## 🎯 Usage

### 1. Choose Input Method
- Navigate to `/create` to select your input method:
  - Text input for manual story entry
  - Audio input for voice recording or file upload
  - OCR for scanning text from images

### 2. Story Analysis
- Gemini AI automatically:
  - Breaks down your story into scenes
  - Extracts characters and their descriptions
  - Identifies emotional tones and camera angles
  - Creates optimized prompts for video generation

### 3. Edit Scenes (Optional)
- Review and refine extracted scenes
- Customize character descriptions
- Adjust camera angles and settings

### 4. Generate Video
- Click "Generate Video" to start Veo 3.1 processing
- Video generation typically takes 1-6 minutes
- Progress is tracked in real-time
- Download your completed video

## 🎨 API Endpoints

### Story Analysis
```typescript
POST /api/analyze-story
Body: { text: string }
Response: { scenes: Scene[] }
```

### Character Extraction
```typescript
POST /api/extract-characters
Body: { text: string }
Response: { characters: Character[] }
```

### Video Generation
```typescript
POST /api/generate-video
Body: { 
  prompt: string,
  config: {
    aspectRatio?: "16:9" | "9:16",
    resolution?: "720p" | "1080p",
    duration?: 4 | 6 | 8,
    negativePrompt?: string
  }
}
Response: { operationId: string, status: string }
```

### Video Status Check
```typescript
POST /api/video-status
Body: { operationId: string }
Response: { 
  status: "processing" | "complete",
  done: boolean,
  videoUri?: string
}
```

### Audio Transcription
```typescript
POST /api/transcribe-audio
Body: FormData with audio file
Response: { transcription: string }
```

### OCR Text Extraction
```typescript
POST /api/ocr-extract
Body: FormData with image file
Response: { text: string }
```

## 🔧 Configuration

### Veo 3.1 Parameters

You can customize video generation in the `/api/generate-video` endpoint:

```typescript
{
  aspectRatio: "16:9" | "9:16",  // Video aspect ratio
  resolution: "720p" | "1080p",   // Output resolution
  durationSeconds: 4 | 6 | 8,     // Video length
  personGeneration: "allow_all",  // Person generation policy
  negativePrompt: string,         // What to avoid in video
}
```

### Gemini Model Selection

Models used in this project:
- `gemini-2.0-flash-exp` - Fast, efficient for text analysis
- `gemini-2.5-flash-image` - Image generation (Nano Banana)
- `veo-3.1-generate-preview` - Video generation

## 📖 Veo 3.1 Prompt Writing Best Practices

### Essential Elements
1. **Subject**: What you want to see (character, object, scenery)
2. **Action**: What's happening (walking, turning, speaking)
3. **Style**: Visual style (cinematic, film noir, animated)
4. **Camera**: Position and movement (aerial view, close-up, dolly shot)
5. **Ambiance**: Lighting and mood (warm tones, night, blue hour)

### Audio Prompts
- **Dialogue**: Use quotes for speech ("This is amazing!")
- **Sound Effects**: Explicitly describe sounds (tires screeching, wind howling)
- **Ambient**: Describe background sounds (birds chirping, distant traffic)

### Example Prompt
```
A cinematic close-up shot of a young woman with dark hair walking through 
a misty forest at dawn. She turns her head, eyes wide with wonder. 
"I've never seen anything like this," she whispers. The camera slowly 
tracks her movement as golden sunlight filters through the trees. 
Warm tones, shallow depth of field, ethereal atmosphere.
```

## 🎬 Video Generation Flow

```mermaid
graph TD
    A[User Input] --> B{Input Type}
    B -->|Text| C[Direct Story Text]
    B -->|Audio| D[Transcribe with Gemini]
    B -->|Image| E[OCR with Gemini Vision]
    
    C --> F[Analyze Story with Gemini]
    D --> F
    E --> F
    
    F --> G[Extract Characters]
    F --> H[Break into Scenes]
    
    G --> I[User Reviews/Edits]
    H --> I
    
    I --> J[Create Veo Prompt]
    J --> K[Generate Video with Veo 3.1]
    
    K --> L[Poll for Completion]
    L --> M{Video Ready?}
    M -->|No| L
    M -->|Yes| N[Download Video]
```

## 🚧 Limitations

### Veo 3.1 Limitations
- **Latency**: 11 seconds to 6 minutes (peak hours)
- **Video Retention**: Generated videos stored for 2 days
- **Regional**: Some regions have person generation restrictions
- **Length**: Maximum 8 seconds per video
- **Watermarking**: All videos watermarked with SynthID

### Rate Limits
Check [Google AI rate limits](https://ai.google.dev/gemini-api/docs/rate-limits) for current API quotas.

## 🔐 Security

- API keys are stored in `.env.local` and never exposed to the client
- All API calls are server-side only
- File uploads are processed securely and not permanently stored

## 📱 Responsive Design

The application is fully responsive and works on:
- Desktop (1920px+)
- Laptop (1280px - 1919px)
- Tablet (768px - 1279px)
- Mobile (320px - 767px)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 🙏 Acknowledgments

- **Google Gemini** for powerful AI models
- **Google Veo 3.1** for state-of-the-art video generation
- **Vercel** for hosting and deployment
- **shadcn/ui** for beautiful UI components

## 📞 Support

For issues or questions:
- Open an issue on GitHub
- Check [Gemini API documentation](https://ai.google.dev/gemini-api/docs)
- Review [Veo 3.1 documentation](https://ai.google.dev/gemini-api/docs/video)

---

Built with ❤️ using Google's latest AI technology
