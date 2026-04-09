# Sandbox.io AI Chat Agent

## Overview

Created a new standalone Next.js AI chat application (`sandbox.io`) with a luxury NOVERA-inspired design, powered by Ollama for local LLM inference.

## Repository

- **GitHub**: https://github.com/mahdi1234-hub/sandbox.io
- **Live URL**: https://sandboxio.vercel.app

## Architecture

### Frontend
- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS with NOVERA luxury design system
- **Features**:
  - Full-screen hero background image (luxury interior from Supabase CDN)
  - Glassmorphism chat bubbles with backdrop blur
  - Real-time streaming responses via Server-Sent Events (SSE)
  - Typing indicators with animated dots
  - Responsive design for mobile and desktop
  - Suggestion chips for quick prompts
  - Ollama connection status indicator

### Backend
- **API Route**: `/api/chat` (Next.js Route Handler)
  - `GET` - Health check for Ollama connectivity
  - `POST` - Streams chat completions from Ollama
- **LLM**: Ollama with TinyLlama model (fastest for resource-constrained environments)
- **Streaming**: SSE-based streaming for real-time token delivery

### Design System (NOVERA-inspired)
- Font: System serif for headings, system sans-serif for body
- Colors: White text on dark overlay, cream (#f9f8f6) base
- Typography: 10px uppercase tracking-widest for labels
- Borders: 2px border-radius (minimal/architectural)
- Effects: Backdrop blur, white/10 backgrounds, gradient overlays
- Animations: fadeInUp entrance, smooth scroll, typing indicators

## Deployment
- Deployed to Vercel at https://sandboxio.vercel.app
- Ollama runs locally in the development environment (sandbox)
- The deployed app's `/api/chat` route connects to Ollama at `127.0.0.1:11434`

## Notes
- Ollama with TinyLlama is configured to run in the Vercel sandbox environment
- The model is CPU-only (no GPU in sandbox) but TinyLlama is optimized for fast inference
- For production use, consider using a cloud-hosted LLM API instead of local Ollama
