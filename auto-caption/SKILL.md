---
name: auto-caption
description: Free, local, CapCut-style karaoke captions for shorts/reels. Transcribes speech locally (faster-whisper, word timestamps), groups words for 9:16 vertical, generates animated .ass subtitles, burns them in with FFmpeg. No subscriptions, no watermarks, no uploads. Use when adding TikTok-style animated captions to a finished clip.
---

# auto-caption

Upstream: https://github.com/sebetancurch/auto-caption (MIT, 15 stars, updated 19 Nov 2026).
Installed 19 Nov 2026 by cloning the repo into this folder (1.1 MB). Scanned: no credential
exfiltration, no network uploads — everything runs locally.

## Quick start

```bash
pip install -e .   # or: pip install faster-whisper (see README.md)
autocaption clip.mp4
# -> clip.captioned.mp4 (final, captions burned in)
# -> clip.ass (animated subtitles) + clip.davinci.srt
```

## Requirements

- Python 3.10+, FFmpeg on PATH (already installed: ffmpeg 8.1.2).
- First run downloads a faster-whisper model (local inference, CPU OK).

## Use in our TikTok pipeline

After `build-test-video.sh` produces a clip with voiceover, run `autocaption` on it to add
word-by-word karaoke captions — the TikTok look. For silent clips (our current test videos),
captions are not applicable; use ffmpeg `drawtext` overlays instead (see
`~/workspace/affiliate/test-video-1/build-test-video.sh`).
