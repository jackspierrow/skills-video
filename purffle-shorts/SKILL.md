---
name: purffle-shorts
description: Free, MIT-licensed AI YouTube Shorts / TikTok / Reels generator. Topic in, finished vertical video out (script, voiceover, footage, captions, sound effects). Runs locally or in Docker/Colab, ffmpeg-only render, no watermark. Use when generating faceless short-form videos from a topic/script at volume.
---

# purffle-shorts

Upstream: https://github.com/Chamanrajragu/purffle-shorts (MIT).
Installed 19 Nov 2026 via codeload tarball (git clone failed twice: network disconnect;
tarball worked, 14 MB). Scanned: MIT LICENSE present, no credential exfiltration found.

## What it does

- LLM writes the script (OpenAI, Claude, Gemini, Groq, DeepSeek, Mistral, Together, xAI,
  Ollama / LM Studio / any OpenAI-compatible API — pick one).
- Voiceover + footage search per scene (portrait-first 9:16 crop, Ken Burns on photos),
  word-by-word captions with highlighted keywords, sound effects.
- Render is plain ffmpeg, one final encode (a 25s Short renders in ~30-40s on an Intel Mac
  in the author's testing). No MoviePy, no ImageMagick, no GPU needed.
- Output: 9:16 / 16:9 / 1:1 / 4:5, can upload/schedule to YouTube, TikTok, Instagram.

## Requirements / keys

- An LLM API key (any OpenAI-compatible provider; Ollama local = free) + ffmpeg.
- See `README.md` + `examples/` in this folder for `make --script` usage.
- **Do NOT use its auto-upload/scheduling to TikTok without bos approval.**

## Use in our TikTok pipeline

Best for faceless explainer/story Shorts at volume. Less suited than the ffmpeg photo
pipeline for OOTD product videos (its footage comes from stock search, not the bos's
product photos) — for product OOTD use `~/workspace/affiliate/test-video-1/build-test-video.sh`.
