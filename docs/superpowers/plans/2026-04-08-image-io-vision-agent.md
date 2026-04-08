# Image.io AI Vision Agent

## Overview

Created a new standalone Next.js AI vision chat application (`image.io`) with Spatial Design Atelier styling, TensorFlow.js for client-side computer vision, Cerebras LLM for text conversations, and Pinecone for RAG.

## Repository

- **GitHub**: https://github.com/mahdi1234-hub/image.io
- **Live URL**: https://imageio-sigma.vercel.app

## Architecture

### Frontend
- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS with Spatial Design Atelier theme (Instrument Serif + Plus Jakarta Sans, earth tones)
- **Computer Vision**: TensorFlow.js running in-browser
  - COCO-SSD: Object detection with bounding boxes (80+ categories)
  - MobileNet: Scene classification (1000+ ImageNet categories)

### Backend
- **API Route**: `/api/chat`
  - Cerebras LLM (llama3.1-8b) for streaming text responses
  - Pinecone RAG for knowledge base augmentation
  - Image analysis context injection from client-side TF.js results

### Key Features
- Upload images (click or drag-and-drop)
- Automatic object detection with bounding boxes and confidence labels
- Scene classification with probability bars
- Natural language conversation on any topic
- Streaming responses via Server-Sent Events
- Fully responsive with mobile header

### Design System (Spatial Design Atelier)
- Fonts: Instrument Serif (headings), Plus Jakarta Sans (body)
- Colors: #181512 primary, #2F5D50 accent, #f5f1ea cream, #d9d1c5 sand
- Typography: 10px uppercase tracking-[0.14em] labels
- Minimal borders, earth tone palette, light weight text
