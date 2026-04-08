# Revide - AI Visual Agent

## Overview

Created a real-time AI visual agent that can see, understand, listen, and speak. The app features a Three.js WebGL background with an interactive planet/particle system, camera/screen capture, Cerebras-powered vision understanding, ElevenLabs voice agent, and browser-native speech recognition/synthesis.

## Links

- **GitHub**: https://github.com/mahdi1234-hub/Revide
- **Live URL**: https://revide-jet.vercel.app

## Architecture

### Frontend (Next.js 14 App Router)
- Three.js WebGL background (interactive planet, particles, arc lines, stars)
- Real-time video capture via `getUserMedia` (camera) and `getDisplayMedia` (screen)
- Frame capture to JPEG for vision analysis
- Web Speech API for speech recognition (listen) and synthesis (speak)
- ElevenLabs Conversational AI widget for voice agent
- Streaming SSE responses for real-time token display

### Backend (Next.js API Routes)
- `/api/vision` - Accepts image frames + text, sends to Cerebras vision model (Llama 4 Scout)
- `/api/chat` - Text-only chat via Cerebras API

### AI Services
- **Cerebras** (OpenAI compatible) - LLM and vision understanding via `llama-4-scout-17b-16e-instruct`
- **ElevenLabs** - Conversational AI voice agent (agent ID embedded in widget)
- **Web Speech API** - Browser-native speech recognition and text-to-speech

### Real-time Capabilities
1. **See**: Camera or screen capture with frame extraction
2. **Understand**: Cerebras vision model analyzes captured frames
3. **Listen**: Web Speech API continuous speech recognition
4. **Speak**: Auto-speaks AI responses via Web Speech API synthesis
5. **Auto-Watch**: Continuous analysis mode that captures and describes every 8 seconds

## Design System
- Three.js WebGL planet with shader-based rim lighting
- 9000 animated particles with mouse interaction/repulsion
- Arc lines with traveling glow sprites
- Background stars with drift animation
- Purple (#6b5cff) and pink (#ff3aa8) accent colors
- Glassmorphism panels with backdrop blur
