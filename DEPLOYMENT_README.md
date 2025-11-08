# Voice Meeting Assistant - Deployment Guide

## Overview
This application is a FastAPI-based voice meeting assistant that transcribes audio, generates summaries, and extracts action items.

## Current Deployment Options

### 1. Render (Free Tier - Recommended)
Render provides a free tier with 750 hours/month, Docker support, and persistent disk storage.

#### Setup Steps:
1. **Create Render Account**: Go to [render.com](https://render.com) and sign up.

2. **Connect Repository**: Link your GitHub repository containing this project.

3. **Deploy Service**:
   - Use the `render.yaml` file for blueprint deployment
   - Or manually create a web service:
     - Runtime: Docker
     - Dockerfile Path: `./VoiceMeetingAssistant/render.Dockerfile`
     - Plan: Free

4. **Environment Variables**:
   - Set `GOOGLE_API_KEY` in Render dashboard (get from Google AI Studio)
   - `PORT` is automatically set by Render

5. **Persistent Storage**:
   - Add a disk named `meetings-data` mounted to `/app/meetings` (1GB free)

6. **Deploy**: Click "Create Web Service" and wait for build completion.

#### Features:
- ✅ Free tier available
- ✅ Docker support
- ✅ Persistent storage
- ✅ Automatic HTTPS
- ✅ Minimal manual work

### 2. Railway (Free Tier Alternative)
Railway offers $5/month credit for new users and Docker support.

#### Setup Steps:
1. **Create Railway Account**: Go to [railway.app](https://railway.app)

2. **Connect Repository**: Link your GitHub repo

3. **Deploy**:
   - Railway auto-detects Docker
   - Set environment variable: `GOOGLE_API_KEY`

4. **Database**: Use Railway's PostgreSQL for data persistence (upgrade needed)

### 3. DigitalOcean App Platform (Paid)
More powerful but requires payment after trial.

### 4. VPS Options (DigitalOcean Droplet)
Full control but more manual work.

#### Setup Steps:
1. **Create Droplet**: Ubuntu 22.04, $6/month

2. **Install Docker**:
   ```bash
   sudo apt update
   sudo apt install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

3. **Clone Repository**:
   ```bash
   git clone <your-repo>
   cd VoiceMeetingAssistant
   ```

4. **Run Container**:
   ```bash
   docker build -t voice-assistant .
   docker run -d -p 80:8000 -e GOOGLE_API_KEY=your_key -v $(pwd)/meetings:/app/meetings voice-assistant
   ```

5. **Setup Nginx** (optional for domain):
   - Install nginx
   - Configure reverse proxy to port 8000

## Environment Variables
- `GOOGLE_API_KEY`: Required for transcription/summarization (get from Google AI Studio)
- `PORT`: Automatically set by hosting platform

## File Structure for Deployment
- `VoiceMeetingAssistant/`: Main application code
- `render.yaml`: Render blueprint configuration
- `VoiceMeetingAssistant/render.Dockerfile`: Render-specific Dockerfile
- `VoiceMeetingAssistant/requirements.txt`: Python dependencies

## Testing Deployment
After deployment, test these endpoints:
- `GET /` - Frontend
- `POST /upload-audio` - Upload audio file
- `GET /meetings` - List meetings
- `GET /meeting/{id}/pdf` - Export PDF

## Notes
- Free tiers have limitations (storage, compute time)
- For production, consider paid plans for more resources
- Data persistence varies by platform
- Monitor usage to avoid unexpected charges
