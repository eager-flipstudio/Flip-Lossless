<div align="center">

<!-- Animated Header -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=200&section=header&text=Flip%20Tidal&fontSize=80&fontColor=fff&animation=twinkling&fontAlignY=35&desc=🎵%20Free%20LOSSLESS%20Music%20API%20powered%20by%20TIDAL&descAlignY=60&descSize=18" width="100%"/>

<!-- Badges Row 1 -->
<p>
  <img src="https://img.shields.io/badge/TIDAL-Powered-000000?style=for-the-badge&logo=tidal&logoColor=white"/>
  <img src="https://img.shields.io/badge/Quality-LOSSLESS%20%2F%20HiRes-00D4FF?style=for-the-badge&logo=audiomack&logoColor=white"/>
  <img src="https://img.shields.io/badge/Flask-Python-FF4500?style=for-the-badge&logo=flask&logoColor=white"/>
  <img src="https://img.shields.io/badge/Vercel-Deploy-000000?style=for-the-badge&logo=vercel&logoColor=white"/>
</p>

<!-- Badges Row 2 -->
<p>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/API-v5.0-blueviolet?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Free-Forever-FF6B6B?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Lyrics-Synced%20LRC-FFD700?style=for-the-badge"/>
</p>

<!-- Live Demo Button -->
<a href="https://flip-musix.vercel.app">
  <img src="https://img.shields.io/badge/🌐%20LIVE%20DEMO-flip--musix.vercel.app-000?style=for-the-badge&color=00D4FF"/>
</a>
&nbsp;
<a href="https://t.me/flipapis">
  <img src="https://img.shields.io/badge/💬%20Telegram-Join%20Channel-26A5E4?style=for-the-badge&logo=telegram"/>
</a>

<br/><br/>

<!-- Architecture Diagram -->
```
╔══════════════════════════════════════════════════════════════════╗
║                     FLIP TIDAL ECOSYSTEM                        ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║   Your App  ──►  flip-musix.vercel.app  ──►  6 TIDAL APIs       ║
║                         │                   ┌─────────────┐     ║
║                         │              ┌───►│ spotisaver  │     ║
║                         │              │    ├─────────────┤     ║
║                         ▼              ├───►│ pinkhamster │     ║
║                  ┌─────────────┐       │    ├─────────────┤     ║
║                  │  Parallel   │       ├───►│   binimum   │     ║
║                  │  Fetching   │───────┤    ├─────────────┤     ║
║                  │ (8 threads) │       ├───►│  monochrome │     ║
║                  └─────────────┘       │    ├─────────────┤     ║
║                         │              ├───►│   kinoplus  │     ║
║                         ▼              │    ├─────────────┤     ║
║               Deduplicate + Format     └───►│  tidal-eu   │     ║
║                         │                   └─────────────┘     ║
║                         ▼                                        ║
║              Clean JSON Response ◄─── BTS Manifest Decode        ║
║                                         (Direct FLAC URL)        ║
╚══════════════════════════════════════════════════════════════════╝
```

</div>

---

## ✨ What is Flip Tidal?

**Flip Tidal** is a free, open-source music API that wraps multiple TIDAL-compatible backends to give you:

- 🎵 **LOSSLESS & Hi-Res FLAC** stream URLs (direct playable `.flac` links)
- 🔍 **Unified Search** across 6 APIs simultaneously with deduplication
- 📀 **Full Metadata** — tracks, albums, artists, playlists with cover images
- 🎤 **Synced Lyrics** (LRC format) via Flip Lyrics API
- ⚡ **Parallel fetching** — responses in 1–3 seconds
- 🆓 **Completely FREE** — no API key required

> **Quality Note**: All stream URLs are LOSSLESS (16-bit / 44.1kHz FLAC) or Hi-Res Lossless (24-bit / 96kHz). This is the same quality as TIDAL HiFi — completely free.

---

## 🌐 Base URLs

| Service | URL |
|---|---|
| 🎵 **Flip Tidal** (Music API) | `https://flip-musix.vercel.app` |
| 🎤 **Flip Lyrics** (Lyrics API) | `https://flip-lyrics.vercel.app` |

---

## 🚀 Quick Start

### cURL
```bash
# Search for a song
curl "https://flip-musix.vercel.app/search?q=espresso" | jq .

# Get direct stream URL
curl "https://flip-musix.vercel.app/stream?id=382240418" | jq .data.stream_url

# Get synced lyrics
curl "https://flip-lyrics.vercel.app/api/lyrics?artist=Sabrina+Carpenter&track=Espresso" | jq .
```

### Python
```python
import requests

BASE = "https://flip-musix.vercel.app"

# Search tracks
results = requests.get(f"{BASE}/search", params={"q": "espresso"}).json()
tracks = results["data"]["tracks"]
print(f"Found {len(tracks)} tracks")

# Get stream URL
stream = requests.get(f"{BASE}/stream", params={"id": "382240418"}).json()
print(stream["data"]["stream_url"])  # Direct .flac URL

# Get lyrics
lyrics = requests.get(
    "https://flip-lyrics.vercel.app/api/lyrics",
    params={"artist": "Sabrina Carpenter", "track": "Espresso"}
).json()
print(lyrics["lyrics"]["plain"])
```

### JavaScript / Node.js
```javascript
const BASE = "https://flip-musix.vercel.app";

// Search
const search = await fetch(`${BASE}/search?q=espresso`).then(r => r.json());
console.log(search.data.tracks);

// Stream URL
const stream = await fetch(`${BASE}/stream?id=382240418`).then(r => r.json());
console.log(stream.data.stream_url); // Direct playable .flac

// Lyrics
const lyrics = await fetch(
  "https://flip-lyrics.vercel.app/api/lyrics?artist=Sabrina+Carpenter&track=Espresso"
).then(r => r.json());
console.log(lyrics.lyrics.synced); // LRC format with timestamps
```

---

## 📡 API Endpoints — Flip Tidal

### 🏠 Home & Discovery

<details>
<summary><code>GET /home</code> — Full home feed</summary>

```bash
curl "https://flip-musix.vercel.app/home" | jq .
```

**Response:**
```json
{
  "creator": "Aquib",
  "telegram": "https://t.me/flipapis",
  "status": "success",
  "message": "Home: 20 tracks, 20 albums, 10 playlists, 4 sections",
  "data": {
    "new_tracks": [...],
    "new_albums": [...],
    "featured_playlists": [...],
    "sections": [
      {"title": "The Hits", "type": "PLAYLIST_LIST", "items": [...]},
      {"title": "New Tracks", "type": "TRACK_LIST", "items": [...]},
      {"title": "New Albums", "type": "ALBUM_LIST", "items": [...]}
    ]
  }
}
```
</details>

<details>
<summary><code>GET /music</code> — Top tracks + albums + playlists</summary>

```bash
curl "https://flip-musix.vercel.app/music" | jq .
```
</details>

<details>
<summary><code>GET /top_tracks</code> — Trending tracks</summary>

```bash
curl "https://flip-musix.vercel.app/top_tracks" | jq .
```
</details>

<details>
<summary><code>GET /top_albums</code> — Trending albums</summary>

```bash
curl "https://flip-musix.vercel.app/top_albums" | jq .
```
</details>

<details>
<summary><code>GET /playlists</code> — Featured playlists</summary>

```bash
curl "https://flip-musix.vercel.app/playlists" | jq .
```
</details>

<details>
<summary><code>GET /sections</code> — All editorial sections</summary>

```bash
curl "https://flip-musix.vercel.app/sections" | jq .
```
</details>

---

### 🔍 Search

<details>
<summary><code>GET /search?q=</code> — Search everything (tracks + albums + artists + playlists)</summary>

```bash
# Basic search
curl "https://flip-musix.vercel.app/search?q=espresso" | jq .

# Search + stream URLs for each track
curl "https://flip-musix.vercel.app/search?q=espresso&stream=true" | jq .

# Search + album track lists
curl "https://flip-musix.vercel.app/search?q=espresso&tracks=true" | jq .

# Search + everything
curl "https://flip-musix.vercel.app/search?q=espresso&tracks=true&stream=true" | jq .
```

**Parameters:**

| Param | Type | Description |
|---|---|---|
| `q` | string | Search query (required) |
| `tracks` | boolean | Include track list inside each album |
| `stream` | boolean | Include stream URL for each track |

**Response:**
```json
{
  "data": {
    "query": "espresso",
    "tracks": [
      {
        "id": 356276483,
        "title": "Espresso",
        "artists": "Sabrina Carpenter",
        "album": "Espresso",
        "album_id": 356276481,
        "image": "https://resources.tidal.com/images/.../320x320.jpg",
        "duration": "2:55",
        "explicit": true,
        "quality": "LOSSLESS",
        "isrc": "USUM72403305",
        "popularity": 92
      }
    ],
    "albums": [...],
    "artists": [...],
    "playlists": [...],
    "top_hits": [...],
    "total_tracks": 25,
    "total_albums": 25,
    "total_artists": 25,
    "total_playlists": 25
  }
}
```
</details>

<details>
<summary><code>GET /search/tracks?q=</code> — Tracks only</summary>

```bash
curl "https://flip-musix.vercel.app/search/tracks?q=espresso" | jq .
```
</details>

<details>
<summary><code>GET /search/albums?q=</code> — Albums only</summary>

```bash
curl "https://flip-musix.vercel.app/search/albums?q=espresso" | jq .
```
</details>

<details>
<summary><code>GET /search/artists?q=</code> — Artists only</summary>

```bash
curl "https://flip-musix.vercel.app/search/artists?q=sabrina+carpenter" | jq .
```
</details>

<details>
<summary><code>GET /search/playlists?q=</code> — Playlists only</summary>

```bash
curl "https://flip-musix.vercel.app/search/playlists?q=chill" | jq .
```
</details>

<details>
<summary><code>GET /search/tophits?q=</code> — Top hits</summary>

```bash
curl "https://flip-musix.vercel.app/search/tophits?q=espresso" | jq .
```
</details>

---

### 🎵 Streaming

<details>
<summary><code>GET /stream?id=</code> — Get direct FLAC stream URL ⭐</summary>

```bash
curl "https://flip-musix.vercel.app/stream?id=382240418" | jq .
```

**Response:**
```json
{
  "data": {
    "track_id": "382240418",
    "stream_url": "https://lgf.audio.tidal.com/mediatracks/.../0.flac?token=...",
    "audio_quality": "LOSSLESS",
    "bit_depth": 16,
    "sample_rate": 44100,
    "replay_gain": -9.53,
    "codec": "flac"
  }
}
```

> ⚡ The `stream_url` is a direct `.flac` file — play it in any audio player, VLC, MPV, or your app.
</details>

<details>
<summary><code>GET /recommend?id=</code> — Recommended tracks by track ID</summary>

```bash
curl "https://flip-musix.vercel.app/recommend?id=382240418" | jq .
```
</details>

---

### 📀 Albums

<details>
<summary><code>GET /album?id=</code> — Album info + all tracks</summary>

```bash
curl "https://flip-musix.vercel.app/album?id=363614268" | jq .
```

**Response:**
```json
{
  "data": {
    "album_id": "363614268",
    "album": {
      "id": 363614268,
      "title": "Espresso EP",
      "artists": "Sabrina Carpenter",
      "artist_image": "https://resources.tidal.com/images/.../320x320.jpg",
      "image": "https://resources.tidal.com/images/.../320x320.jpg",
      "tracks": 6,
      "release": "2024-04-11",
      "quality": "LOSSLESS"
    },
    "tracks": [
      {"id": 363614270, "title": "Espresso", "track_number": 1, ...}
    ],
    "total": 6
  }
}
```
</details>

<details>
<summary><code>GET /album/stream?id=</code> — All album tracks with stream URLs</summary>

```bash
curl "https://flip-musix.vercel.app/album/stream?id=363614268" | jq .
```
</details>

---

### 🎤 Artists

<details>
<summary><code>GET /artist?id=</code> — Artist info + tracks</summary>

```bash
curl "https://flip-musix.vercel.app/artist?id=4751756" | jq .
```

**Response:**
```json
{
  "data": {
    "artist_id": "4751756",
    "artist": {
      "id": 4751756,
      "name": "Sabrina Carpenter",
      "image": "https://resources.tidal.com/images/.../320x320.jpg"
    },
    "tracks": [...],
    "total": 20
  }
}
```
</details>

---

### 🎧 Playlists

<details>
<summary><code>GET /playlist?id=</code> — Playlist info + all tracks</summary>

```bash
curl "https://flip-musix.vercel.app/playlist?id=ee8f8cba-6a96-445e-a443-62dd512faf68" | jq .
```

**Response:**
```json
{
  "data": {
    "playlist_id": "ee8f8cba-...",
    "playlist": {
      "title": "Imagine Dragons Essentials",
      "image": "https://resources.tidal.com/images/.../320x320.jpg",
      "tracks": 20
    },
    "tracks": [...],
    "total": 20
  }
}
```
</details>

---

## 🎤 API Endpoints — Flip Lyrics

<details>
<summary><code>GET /api/lyrics</code> — Get synced + plain lyrics</summary>

```bash
curl "https://flip-lyrics.vercel.app/api/lyrics?artist=Sabrina+Carpenter&track=Espresso" | jq .
```

**Parameters:**

| Param | Type | Description |
|---|---|---|
| `artist` | string | Artist name (required) |
| `track` | string | Track name (required) |
| `album` | string | Album name (optional, improves accuracy) |

**Response:**
```json
{
  "creator": "Aquib",
  "song": {
    "artist": "Sabrina Carpenter",
    "track": "Espresso",
    "album": "Espresso EP",
    "duration": 175.0
  },
  "lyrics": {
    "synced": "[00:17.12] I feel your breath upon my neck\n[00:20.41]...",
    "plain": "I feel your breath upon my neck\nA soft caress as cold as death\n..."
  },
  "status": true
}
```

> **`synced`** = LRC format with timestamps — perfect for karaoke/word-by-word highlight  
> **`plain`** = Clean text without timestamps

</details>

---

## 🎵 Track Object Reference

Every track in the API returns this consistent format:

```json
{
  "id": 382240418,
  "title": "Espresso",
  "artists": "Sabrina Carpenter",
  "album": "Short n' Sweet",
  "album_id": 382240404,
  "image": "https://resources.tidal.com/images/b3495216/6cbf/40ca/82ba/ecfbe13b8e95/320x320.jpg",
  "duration": "2:55",
  "explicit": false,
  "quality": "LOSSLESS",
  "tidal_url": "http://www.tidal.com/track/382240418",
  "release": "2024-08-23",
  "popularity": 47,
  "isrc": "USUM72404982",
  "track_number": 7
}
```

### Image URLs
All images follow this format — you can change the size:
```
https://resources.tidal.com/images/{uuid}/320x320.jpg
https://resources.tidal.com/images/{uuid}/640x640.jpg
https://resources.tidal.com/images/{uuid}/1280x1280.jpg
```

---

## 🚀 Deploy Your Own

Fork this repo and deploy to Vercel in 2 minutes — **free forever**.

### Step 1 — Fork
```bash
git clone https://github.com/YOUR_USERNAME/flip-tidal.git
cd flip-tidal
```

### Step 2 — Deploy to Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Follow prompts — done!
```

Or click:

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/YOUR_USERNAME/flip-tidal)

### Step 3 — Done! 🎉
Your own private instance at `https://your-project.vercel.app`

> **Note**: This repo deploys the **Flip Tidal** API only. The base API at `flip-musix.vercel.app` is maintained by Aquib. Your fork will be your own independent instance.

---

## ⚡ Performance

| Feature | Detail |
|---|---|
| **Parallel Requests** | 8 simultaneous API calls with `ThreadPoolExecutor` |
| **Response Time** | 1–3 seconds (vs 18+ seconds sequential) |
| **Deduplication** | Tracks/albums deduplicated by ID across all sources |
| **Timeout** | 4s per API — slow sources skipped automatically |
| **Sources** | 6 APIs: spotisaver, pinkhamster, binimum, monochrome, kinoplus, tidal-eu |

---

## 🎵 Audio Quality

| Quality | Bit Depth | Sample Rate | Format |
|---|---|---|---|
| **LOSSLESS** | 16-bit | 44,100 Hz | FLAC |
| **HI_RES_LOSSLESS** | 24-bit | 96,000 Hz | FLAC |

Stream URLs are **direct `.flac` files** — no segments, no DASH, playable anywhere.

---

## 📦 Response Format

Every endpoint returns:
```json
{
  "creator": "Aquib",
  "telegram": "https://t.me/flipapis",
  "status": "success",
  "message": "...",
  "data": { ... }
}
```

---

## 🛠 Local Development

```bash
# Clone
git clone https://github.com/YOUR_USERNAME/flip-tidal.git
cd flip-tidal

# Install dependencies
pip install -r requirements.txt

# Run
python api/index.py

# Test
curl "http://localhost:5000/search?q=espresso" | jq .
```

---

## 🔗 Related Projects

| Project | Description | Link |
|---|---|---|
| 🎵 **Flip Tidal** | Free LOSSLESS music API | [flip-musix.vercel.app](https://flip-musix.vercel.app) |
| 🎤 **Flip Lyrics** | Synced lyrics API | [flip-lyrics.vercel.app](https://flip-lyrics.vercel.app) |
| 💬 **Telegram** | Updates & support | [@flipapis](https://t.me/flipapis) |

---

## 📄 License

MIT License — free to use, fork, and deploy.

---

<div align="center">

<!-- Footer Wave -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=6,11,20&height=100&section=footer" width="100%"/>

**Made with ❤️ by [Aquib](https://t.me/flipapis)**

⭐ Star this repo if it helped you!

[![Telegram](https://img.shields.io/badge/Join-@flipapis-26A5E4?style=for-the-badge&logo=telegram)](https://t.me/flipapis)

</div>
