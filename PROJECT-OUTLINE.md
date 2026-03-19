# Dream Screens — Project Outline

A browser-based immersive audiovisual experience blending album, generative cinema, and narrative. Single-page WebGL application with particle organism shaders, video morphs, and Web Audio API music.

---

## 1. Project Structure

```
DREAM SCREENS MVP/
├── index.html          # Main experience (single-page app)
├── whitepaper.html     # Research overview / concept document
├── proposal.html       # Artistic proposition
├── dream-screens-v2.html  # Legacy/alternate version
├── vercel.json         # Deployment config (COEP/COOP headers)
├── .gitignore          # Excludes Media/, node_modules/, .DS_Store
├── SPEC.md             # Pitch spec for Simon Meek
├── PROJECT-OUTLINE.md  # This document
└── Media/
    ├── Movies/         # Video assets (MP4)
    └── mp3/            # Audio assets
```

---

## 2. Experience Flow (7 Stages)

| Stage | ID | Description | Duration |
|-------|-----|-------------|----------|
| **0** | s0 | Intro — particle organism, wordmark, ENTER button, links (artistic proposition, research overview) | Until user clicks |
| **1** | s1 | System log — wave shader, log lines, organism fade | ~18s |
| **2** | s2 | Video 1 — memory fragment with overlay text | Until click |
| **3** | s3 | Video 1→2 crossfade — memory fragment | Until click |
| **4** | s4 | Video 2→3 crossfade — memory fragment | Until click |
| **5** | s5 | Resolution organism — "Memory fragment archived", Run again | ~8s |
| **6** | s6 | Archive overlay — track listing, framing text, dreamscreens.io | ~30s |

**User interaction:** Click ENTER to start; click "· next memory ·" to advance through video stages.

---

## 3. Media Assets

### 3.1 Video (CONFIG.videos)

| Path | File | Size | Notes |
|------|------|------|-------|
| `Media/Movies/White Matter 2.mp4` | White Matter 2 | 384 MB | Used |
| `Media/Movies/Shimmering Thirds.mp4` | Shimmering Thirds | 349 MB | Used |
| `Media/Movies/Movement TWO.mp4` | Movement TWO | 511 MB | Used |

**Video behaviour:**
- All videos seek to **30 seconds** on load (skips title cards)
- Order shuffled on each run (Fisher–Yates) — different sequence per playthrough
- Preloaded in background; `preload="auto"`, `loop`, `muted`, `playsinline`
- Used as WebGL textures for luma reveal, chroma drift, crossfade

**Other video files (not in current config):**
- Movement One.mp4 (563 MB)
- White Matter.mp4 (342 MB)
- White Matter_La chunky sess.mp4 (168 MB)
- Movement TWO small.mov (139 MB)

### 3.2 Music (CONFIG.music)

| Path | Stage | Size |
|------|-------|------|
| `Media/mp3/Virta.mp3` | intro | 8.4 MB |
| `Media/mp3/Signal Dreams.mp3` | stage1 | 10 MB |
| `Media/mp3/Feedback Memory.(Minus 197).mp3` | stage2 | 3.2 MB |
| `Media/mp3/Nature module 1.mp3` | stage3 | 7.8 MB |
| `Media/mp3/Brainwaves (grain Pysche).mp3` | stage4 | 6.0 MB |
| `Media/mp3/Module n5 v2.mp3` | stage5 | 7.2 MB |

**Audio behaviour:** Web Audio API, slow crossfades (30s), `setTargetAtTime`.

---

## 4. Media Hosting

### 4.1 Local Development

- **Server:** `python3 -m http.server 3001`
- **URL:** http://localhost:3001
- **Media:** Served from same origin as `index.html` (relative paths)
- **CONFIG.mediaBaseUrl:** `''` (empty)

### 4.2 Production / CDN Hosting

Media is **not** deployed with the app (excluded via `.gitignore`). For production:

1. **Upload media to a CDN** (e.g. Cloudflare R2, AWS S3, CloudFront, Bunny CDN, Vercel Blob).

2. **Set `CONFIG.mediaBaseUrl`** in `index.html`:
   ```javascript
   mediaBaseUrl: 'https://your-bucket.r2.dev/',
   ```

3. **Path structure:** Keep the same relative paths. The CDN base URL is prepended:
   - `Media/Movies/White Matter 2.mp4` → `https://your-bucket.r2.dev/Media/Movies/White Matter 2.mp4`
   - `Media/mp3/Virta.mp3` → `https://your-bucket.r2.dev/Media/mp3/Virta.mp3`

4. **CORS:** Configure the CDN to allow:
   - `Access-Control-Allow-Origin: *` (or your domain)
   - For video textures in WebGL, same-origin or CORS-enabled sources are required.

5. **File size:** Videos are ~350–510 MB each. Consider:
   - Streaming (HLS/DASH) for large files
   - Lower-resolution versions for faster load
   - Compressed/optimised encodes

### 4.3 Vercel Deployment

- **vercel.json** sets COEP/COOP headers for SharedArrayBuffer / advanced APIs if needed.
- **Static files:** HTML, JS, CSS deploy from repo; **Media/** is excluded.
- **Media:** Must be hosted separately (CDN) and referenced via `mediaBaseUrl`.

---

## 5. Technical Stack

| Component | Technology |
|-----------|------------|
| **Rendering** | WebGL (vanilla), custom GLSL fragment shaders |
| **Video** | HTML5 `<video>` → texture upload via `texImage2D` |
| **Audio** | Web Audio API, `AudioContext`, `createBufferSource` |
| **Layout** | Vanilla HTML/CSS, no framework |
| **Fonts** | IBM Plex Mono, IBM Plex Sans (Google Fonts) |

---

## 6. Key Features

- **Particle organism:** 150 particles, golden-angle layout, shader-driven
- **Video luma reveal:** Noise → signal → memory → corruption
- **Chroma drift / RGB split:** Glitch effect on video
- **Archive overlay:** Cooler palette, brightness clamp, glitch on organism
- **Ghost pulses:** 25s interval, 12s sine, 0.12 peak during video stages
- **Session ID:** Unique per run (e.g. DS-XXXXX)

---

## 7. Archive Tracks (UI)

| # | Title | Status |
|---|-------|--------|
| 01 | Signal Dreams | available |
| 02 | Feedback Memory | available |
| 03 | Nature Module | available |
| 04 | Brainwaves | available |
| 05 | Module N5 | available |
| 06 | Co-Dreaming | soon |
| 07 | Free Will | soon |
| 08 | Glitches Are Portals | soon |
| 09 | The Dream Ends | soon |
| 10 | Virta | soon |

---

## 8. Quick Reference

| Command | Purpose |
|---------|---------|
| `python3 -m http.server 3001` | Run locally |
| `http://localhost:3001` | Open in browser |
| `CONFIG.mediaBaseUrl` | Change for CDN |
| `CONFIG.videos` | Swap video paths |
| `CONFIG.media(path)` | Resolve full media URL |

---

*Dream Screens — Memory Reconstruction v4.1*
