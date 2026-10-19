# YTube downlder

A lightweight, mobile-friendly YouTube audio clip cutter with timestamp selection, preview controls, local audio waveform preview, MP3/M4A/WAV output options, and batch clip ZIP export.

## Features

- Responsive, low-dependency static website
- YouTube URL validation for watch, Shorts, embed, live, and youtu.be links
- Timestamp selection in seconds, MM:SS, or HH:MM:SS
- YouTube player preview and quick range presets
- Local audio preview with a browser-generated waveform (the selected file remains local)
- MP3, M4A, and WAV export through the separate API
- Batch export of up to 10 clips as a ZIP
- Reserved advertisement spaces; ads are placeholders, not active ads

## Publish the website with GitHub Pages

1. Open **Settings → Pages** in this repository.
2. Set the build and deployment source to **GitHub Actions**.
3. Open the **Actions** tab and check the **Deploy YTube downlder to GitHub Pages** workflow.
4. Once the workflow succeeds, the expected website address is:

   https://rocccyborg.github.io/Ytube-downloader/

GitHub Pages serves the frontend only. Audio extraction requires the separate backend service described below.

## Deploy the audio-processing API

The API uses Python, yt-dlp, and FFmpeg. The included `Dockerfile` installs FFmpeg. Deploy with a host that supports Docker and outbound requests, then set `ALLOWED_ORIGINS` to `https://rocccyborg.github.io`.

For Render, use the included root `render.yaml` as a Blueprint. After deployment, copy the API service's HTTPS URL and open the website with it appended as a query parameter:

`https://rocccyborg.github.io/Ytube-downloader/?api=https://YOUR-API-HOST`

The API can also be configured for a local test using `?api=http://localhost:8080`.

## Local development

1. Install Python 3.10+ and FFmpeg.
2. Install dependencies: `pip install -r requirements.txt`.
3. Start the API: `python api.py`.
4. Serve this folder with `python -m http.server 8000`.
5. Open `http://localhost:8000/?api=http://localhost:8080`.

## Responsible use and production notes

Only process videos you own or have permission to download. Comply with YouTube's terms and applicable copyright law. Source videos may be unavailable due to access restrictions or platform changes.

The waveform is generated only for an audio file the user selects locally; it does not analyze the embedded YouTube stream. Reserved ad areas do not generate revenue until an ad network approves the site and its publisher code is added.

Before exposing the API publicly, add rate limiting, abuse controls, monitoring, and suitable resource limits. A free backend host may sleep when idle and take time to start.
