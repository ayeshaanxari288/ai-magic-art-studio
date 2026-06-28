# ai-magic-art-studio
# AI Magic Art Studio

> A production-grade Multimodal Image Generation Studio built on the Stability AI REST API.  
> DecodeLabs Industrial Training — Batch 2026 | Generative AI Engineering Track

---

## Live Demo

**[Launch App](https://ai-magic-art-studio-sku9dt5nnbdswxpagfry7e.streamlit.app/)**

---

## Project Overview

This project is not just about generating images. It is a complete, production-aware AI pipeline that demonstrates how professional generative media applications are architected — from prompt engineering and payload construction, to network resilience, memory-safe binary streaming, and automated quality verification.

Built as **Project 3** of the DecodeLabs Generative AI Engineering track, this studio bridges the gap between human language and synthetic visual generation through pure programmatic control.

---

## Live Preview

![App Preview](https://ai-magic-art-studio-sku9dt5nnbdswxpagfry7e.streamlit.app/)

---

## Features

### Core Pipeline
- Text-to-image generation via Stability AI REST API (v2beta)
- Natural language prompt input with negative prompt support
- Multiple aspect ratios — 1:1, 16:9, 9:16, 4:3
- Style presets — Photographic, Cinematic, Digital Art, Fantasy, Anime, Neon Punk, 3D Model, Pixel Art
- Adjustable seed for reproducible results

### Engineering Layers
- Split timeout network gateway — 3.05s connect, 90s read
- Exponential backoff retry logic with jitter on HTTP 429 and 503
- Memory-safe 64KB chunked binary streaming
- Pixel-level image integrity verification via Pillow `.load()`
- Dual security gate handling — input and output content moderation
- Server-side API key management via Streamlit Secrets

### User Experience
- Cartoon animated UI with bouncy interactions and warm color theme
- Real-time generation details card — style, dimensions, file size, seed
- Image Verified badge after pixel-level integrity check
- One-click image download
- Recent Creations history — last 5 generated images with individual save buttons
- Responsive sidebar controls

---

## Tech Stack

| Technology | Purpose |
|-----------|---------|
| Python | Core language |
| Streamlit | Web application framework |
| Stability AI API | Image generation engine |
| Pillow | Image processing and integrity verification |
| Requests | HTTP client with timeout and retry logic |

---

## Pipeline Architecture

```
User Prompt
     |
     v
1. Payload Construction     — Prompt + exact resolution mapping + style preset
     |
     v
2. Network API Gateway      — Split timeout (3.05s connect / 90s read)
     |
     v
3. Security Gate 1          — Input content moderation check
     |
     v
4. GPU Generation           — Stability AI reverse-Markov denoising loop
     |
     v
5. Security Gate 2          — Output content moderation check
     |
     v
6. Retry Logic              — Exponential backoff + jitter on failure
     |
     v
7. Binary Streaming         — Memory-safe 64KB chunked download
     |
     v
8. Integrity Verification   — Pixel-level decode via Pillow .load()
     |
     v
9. Display and Export       — Rendered image + download + history
```

---

## Supported Aspect Ratios

| Ratio | Dimensions | Best For |
|-------|-----------|----------|
| 1:1 | 1024 x 1024 px | Avatars, product images |
| 16:9 | 1344 x 768 px | Web banners, presentations |
| 9:16 | 768 x 1344 px | Mobile wallpapers, reels |
| 4:3 | 1152 x 896 px | Standard displays |

---

## How to Run Locally

### 1. Clone the repository
```bash
git clone https://github.com/your-username/ai-magic-art-studio.git
cd ai-magic-art-studio
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Add your API key
Create a file at `.streamlit/secrets.toml`:
```toml
STABILITY_API_KEY = "your-stability-ai-key-here"
```

### 4. Run the app
```bash
streamlit run app.py
```

---

## How to Deploy on Streamlit Cloud

1. Push this repository to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io)
3. Connect your GitHub repository
4. Set `app.py` as the main file
5. Go to Advanced Settings and Secrets and add:
```toml
STABILITY_API_KEY = "your-stability-ai-key-here"
```
6. Click Deploy

---

## Project Structure

```
ai-magic-art-studio/
├── app.py                                        # Main Streamlit application
├── requirements.txt                              # Python dependencies
├── README.md                                     # Project documentation
└── notebooks/
    └── Project3_Image_Generation_Studio.ipynb    # Full engineering notebook
```

---

## Key Engineering Decisions

**Split Timeout** — The Python requests library applies no timeout by default. A two-value tuple `(3.05, 90)` separates the TCP connection timeout from the GPU inference read timeout, preventing the application from hanging indefinitely.

**Exponential Backoff with Jitter** — Transient failures trigger retries with increasing wait times and randomized delays. Jitter prevents synchronized request bursts from overwhelming the API gateway.

**Memory-Safe Streaming** — Image bytes are written to disk in 64KB chunks rather than buffered entirely in RAM, keeping memory usage constant regardless of output file size.

**Pixel-Level Integrity Verification** — `Image.open().load()` forces a full pixel decode, catching truncated files that pass standard header-only checks like `verify()`.

**Server-Side API Key** — The API key is stored in Streamlit Secrets and never exposed in the source code or to end users.

**Recent Creations History** — Session state stores the last 5 generated images with metadata, allowing users to revisit and re-download previous outputs without regenerating them.

---

## Internship Details

| Field | Detail |
|-------|--------|
| Program | DecodeLabs Industrial Training |
| Batch | 2026 |
| Track | Generative AI Engineering |
| Project | Project 3 — Multimodal Image Generation Studio |

---

## Contact

**DecodeLabs**
- Website: [decodelabs.tech](https://www.decodelabs.tech)
- Email: decodelabs.tech@gmail.com

---

*Made with dedication as part of the DecodeLabs Generative AI Engineering internship program.*
