# CortexAlpha 🧠

An AI chat interface with neural network visualization, built with Next.js 15, React 19, TypeScript, and the Vercel AI SDK.

## Features

### 💬 Chat Interface
- **Multi-session management** — Create, pin, rename, search, delete conversations
- **Persistent history** — Stored in localStorage via Zustand
- **Streaming responses** — Real-time token streaming from AI
- **Markdown rendering** — With syntax highlighting (react-syntax-highlighter)
- **Artifact panel** — Side-by-side code/artifact viewing

### 📎 File Attachments
- Drag & drop or click to upload
- Support: Images, PDFs, text, code files (max 10MB)
- Base64 encoding for API transmission

### ⚙️ Settings & Customization
- **System prompt** — Customize AI behavior
- **Temperature control** — Adjust creativity (0–1)
- **Model selection** — Via API configuration

### 🔄 Hydra Protocol (API Resilience)
- **Multi-key rotation** — Automatic failover across Gemini API keys
- **Model fallback chain** — Primary → Fallback → Last resort
- **Probe validation** — Pre-flight key/quota check before streaming
- **Retryable error handling** — 429, 403, 503, quota, network blips

### 🎨 Visual Experience
- **Neural network animation** — Interactive canvas background
- **Wireframe globe** — Alternative ambient visualization
- **Framer Motion** — Smooth transitions & micro-interactions
- **Dark theme** — Optimized for extended use

### 📱 Responsive Design
- Mobile-first sidebar (collapsible)
- Touch-friendly interactions
- Adaptive layouts

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Framework** | Next.js 15 (App Router) |
| **Language** | TypeScript (strict) |
| **AI SDK** | Vercel AI SDK (`ai`, `@ai-sdk/google`) |
| **State** | Zustand + persistence middleware |
| **Styling** | Tailwind CSS + CSS Variables |
| **Animation** | Framer Motion |
| **Markdown** | react-markdown + react-syntax-highlighter |
| **Icons** | Lucide React |
| **Toast** | Sonner |
| **Deployment** | Vercel |

## Quick Start

```bash
# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Add your GOOGLE_GENERATIVE_AI_API_KEY(s)

# Run development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000).

## Environment Variables

```bash
# Required: At least one Gemini API key
GOOGLE_GENERATIVE_AI_API_KEY=your_primary_key

# Optional: Additional keys for Hydra rotation
GOOGLE_GENERATIVE_AI_API_KEY_1=backup_key_1
GOOGLE_GENERATIVE_AI_API_KEY_2=backup_key_2

# Optional: Model overrides (defaults shown)
# PRIMARY_MODEL=gemini-flash-latest
# FALLBACK_MODEL=gemini-flash-lite-latest
# LAST_RESORT_MODEL=gemini-pro-latest
```

## Hydra Protocol: How It Works

```
Request → Probe Key (Ping) → Stream Response
                ↓ (on 429/403/503/quota)
         Rotate to Next Key
                ↓ (all keys exhausted)
         Try Fallback Model
                ↓ (all models exhausted)
         Return 503 with friendly message
```

**Key features:**
- Pre-flight "ping" validates key/quota before streaming
- Randomized key order prevents thundering herd
- Per-model, per-key attempt logging
- Graceful degradation with user-friendly errors

## Project Structure

```
src/
├── app/
│   ├── api/chat/route.ts    # Hydra Protocol endpoint
│   ├── layout.tsx           # Root layout
│   ├── page.tsx             # Home (NeuralNetwork + ChatInterface)
│   ├── globals.css          # Global styles
│   └── robots.ts / sitemap.ts
├── components/
│   ├── canvas/
│   │   └── NeuralNetwork.tsx    # WebGL neural net visualization
│   └── ui/
│       ├── ChatInterface.tsx    # Main chat component (76KB)
│       ├── SettingsModal.tsx    # System prompt, temperature
│       ├── ArtifactPanel.tsx    # Code/artifact side panel
│       ├── animated-backgrounds.tsx
│       ├── globe.tsx            # WireframeGlobe
│       └── ...
├── store/
│   └── chat-store.ts          # Zustand store with persistence
├── hooks/
│   └── use-mobile.ts          # Responsive breakpoint hook
├── lib/
│   └── utils.ts               # cn() classname utility
└── types/                     # Shared TypeScript types
```

## Scripts

```bash
npm run dev       # Development server (Turbopack)
npm run build     # Production build
npm run start     # Production server
npm run lint      # ESLint check
```

## Architecture Highlights

### Streaming with Resilience
The `/api/chat` route implements a three-layer fallback:
1. **Key rotation** — Multiple API keys per model
2. **Model cascade** — Flash → Flash-Lite → Pro
3. **Probe-then-stream** — Validates key health before committing to stream

### Client-Side State
Zustand store persists to localStorage:
- Sessions with messages & file attachments
- Pinned conversations (sort to top)
- User settings (system prompt, temperature)

### Performance
- Dynamic imports for heavy components
- Memoized message lists
- Virtualized sidebar (planned)
- Optimistic UI updates

## Deploy on Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FAniket-Solanki%2FAniket-s-Ai)

Add `GOOGLE_GENERATIVE_AI_API_KEY` in Vercel project settings.

---

*Built with ❤️ by [Aniket Solanki](https://github.com/Aniket-Solanki)*
