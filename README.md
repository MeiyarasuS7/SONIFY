# Sonify

Sonify is a full-stack voice utility app with a polished React frontend and a FastAPI backend.

It currently supports:

- Text to speech
- Translate and speak
- Speech to text transcription
- Legacy PDF to speech support on the backend

## Stack

- Frontend: React, TypeScript, Vite, Tailwind CSS, shadcn/ui
- Backend: FastAPI, gTTS, SpeechRecognition, deep-translator, langdetect

## Main Features

### 1. Text to Speech

- Convert typed text into MP3 audio
- Choose a voice language
- Enable slow mode for a calmer speaking pace

### 2. Translate + Speak

- Optionally translate text before audio generation
- Supported translation targets:
  - English
  - Spanish
  - French
  - German
  - Italian
  - Portuguese
- Source language can be chosen manually or set to `Auto Detect`

### 3. Speech to Text

- Upload audio files and generate text transcripts
- Supported input formats include:
  - MP3
  - WAV
  - M4A
  - OGG
  - WEBM
- Supports manual language selection
- Includes a `Best Effort Auto Detect` mode

### 4. Backend Utilities

- Temporary file cleanup for generated audio and uploads
- Health endpoint
- Interactive API docs
- Legacy PDF-to-speech endpoints still available on the backend

## Project Structure

```text
Sonify/
|- backend/
|  |- app/
|  |  |- routes/
|  |  |- tts/
|  |- main.py
|  |- requirements.txt
|- frontend/
|  |- src/
|  |- package.json
|- QUICKSTART.md
|- README.md
```

## Prerequisites

### Backend

- Python 3.10+
- `pip`
- A virtual environment is recommended

### Frontend

- Node.js 18+
- `npm`

### Optional Runtime Tools

- `ffmpeg`
  - Recommended for broader audio format support through `pydub`

## Setup

### 1. Backend

From the project root:

```powershell
cd backend
python -m venv venv
.\venv\Scripts\activate
python -m pip install -r requirements.txt
python main.py
```

Backend URL:

- `http://localhost:8000`

### 2. Frontend

Open a second terminal:

```powershell
cd frontend
npm install
npm run dev
```

Frontend URL:

- `http://localhost:5173`

## Environment Variables

### Frontend

Create [frontend/.env](/d:/104/text-audio/Sonify/frontend/.env):

```env
VITE_API_URL=http://localhost:8000
```

## Frontend Routes

- `/` - landing page
- `/text-to-speech` - text to speech and translate + speak
- `/speech-to-text` - speech transcription
- `/pdf-upload` - alias to speech-to-text page

## API Overview

Base API URL:

- `http://localhost:8000/api`

### Active Endpoints

- `POST /tts`
  - Generate audio from text
  - Supports translation before speaking
- `POST /tts/json`
  - JSON version of the text-to-speech flow
- `POST /transcribe`
  - Upload audio and return transcript text

### Legacy / Compatibility Endpoints

- `POST /pdf`
  - PDF to speech audio response
- `POST /tts/pdf`
  - Legacy PDF to speech JSON response
- `POST /tts/legacy`
  - Legacy text-to-speech JSON response

### Utility Endpoints

- `GET /`
- `GET /health`
- `GET /cleanup`
- `GET /docs`
- `GET /redoc`

## Frontend Scripts

Run from `frontend/`:

- `npm run dev`
- `npm run build`
- `npm run build:dev`
- `npm run preview`
- `npm run lint`
- `npm run test`
- `npm run test:watch`

## Backend Notes

- Generated audio is stored temporarily in `backend/audio/`
- Upload processing uses `backend/temp/`
- Cleanup runs on startup and can also be triggered manually
- Translation and transcription features depend on network availability at runtime

## Known Behavior

### Translate + Speak

- Voice language selection alone does not translate text
- Translation only happens when the `Translate before speaking` option is enabled
- Very long translated preview text may be shortened in the UI preview, while full audio is still generated

### Speech to Text

- Manual language selection is more reliable than `Best Effort Auto Detect`
- Very short, noisy, or clipped audio may fail to transcribe
- Auto-detect is heuristic, not a heavyweight language-identification model

## Troubleshooting

### Backend module errors

If you see missing Python package errors:

```powershell
cd backend
.\venv\Scripts\activate
python -m pip install -r requirements.txt
```

### Frontend cannot reach backend

Check:

- backend is running on port `8000`
- [frontend/.env](/d:/104/text-audio/Sonify/frontend/.env) contains the correct `VITE_API_URL`

### Speech to text fails

Try:

- a longer audio clip
- clearer speech
- lower background noise
- selecting the spoken language manually instead of auto-detect

### Translation fails

This usually points to runtime network availability or translation service issues.

