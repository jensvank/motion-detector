# 🛡️ Motion Detector

> A zero-dependency, browser-based motion detection app that runs entirely on your local machine. No installation. No server. Just open and go.

Capture any screen window — VLC, a browser stream, a webcam — draw detection zones directly on the video, and receive instant visual and audio alerts when motion is detected. Automatically saves screenshots on alarm.

**[🚀 Live Demo](https://jensvank.github.io/motion-detector/motion-detector.html)**







***

## 📸 Screenshots

<img width="1508" height="961" alt="image" src="https://github.com/user-attachments/assets/123d1fd4-44b6-46b1-a717-d9247def4417" />
<img width="3416" height="1858" alt="image" src="https://github.com/user-attachments/assets/b273e15a-2e97-4832-9184-8fd32976cece" />


***

## ✨ Features

| Feature | Details |
|---------|---------|
| 🖥️ **Screen capture** | Select any open window (VLC, browser, any app) as the video source |
| 📷 **Webcam support** | Use any connected camera as an alternative source |
| 🎯 **Custom zones** | Draw unlimited detection zones by clicking and dragging on the video |
| 🔔 **Audio alarm** | Four synthesised tones: Siren, Beep, Pulse, Horn — adjustable volume |
| 👁️ **Visual alarm** | Fullscreen red flash overlay + banner notification |
| 📸 **Auto-screenshot** | Saves a clean PNG on every alarm trigger — toggle on/off |
| 📊 **Live stats** | Per-zone motion percentage, FPS counter, alarm count, activity log |
| ⚙️ **Tunable detection** | Sensitivity, minimum area threshold, and cooldown all configurable |
| 🔒 **100% private** | All processing happens locally — no data ever leaves your machine |

***

## 🚀 Quick Start

### Option 1 — GitHub Pages (recommended)

```
[https://jensvank.github.io/motion-detector/motion-detector.html]
```

Open the URL in **Safari or Chrome** — no installation needed.

### Option 2 — Run locally

1. [Download `index.html`](index.html)
2. Open the file in **Safari or Chrome** (double-click or drag onto the browser)
3. Done — the app runs fully offline

> ⚠️ **Do not open via `file://` for screen capture.** Either use GitHub Pages (HTTPS) or a local server like `npx serve .` — `getDisplayMedia()` requires a secure context.

***

## 🍎 macOS Setup (M1 / M2 / M3)

Screen capture requires a one-time permission grant:

1. Open **System Settings → Privacy & Security → Screen Recording**
2. Enable your browser (**Safari** or **Chrome**)
3. Restart the browser, then open the app

VLC does **not** need to be in fullscreen — select the VLC window from the macOS screen picker.

***

## 🎯 How to Use

```
1. Click "Screen / VLC window"  →  select your video source in the macOS picker
2. Draw a zone                  →  click and drag on the video feed
3. Motion triggers alarm        →  visual flash + sound + optional auto-screenshot
```

**Toolbar controls:**

| Button | Action |
|--------|--------|
| Draw zone | Click and drag to create a detection zone |
| View | Switch to pan mode (no accidental zone drawing) |
| Mute alarm | Silence the current alarm without waiting for cooldown |
| Screenshot | Manually capture a clean video frame |

***

## ⚙️ Settings Reference

### Detection

| Setting | Range | Default | Description |
|---------|-------|---------|-------------|
| Sensitivity | 1 – 80% | 30% | Pixel-change threshold. Higher = triggers on subtler movement. |
| Min. motion area | 1 – 30% | 3% | % of zone pixels that must change to fire an alarm. |
| Alarm cooldown | 1 – 30s | 3s | Minimum time between consecutive alarms. |
| Detection active | toggle | On | Pause/resume detection without stopping the stream. |

### Alarm

| Setting | Options | Default | Description |
|---------|---------|---------|-------------|
| Sound alert | toggle | On | Play tone on alarm. |
| Visual alarm | toggle | On | Red flash overlay. |
| **Auto-screenshot** | **toggle** | **On** | **Save PNG automatically on every alarm.** |
| Volume | 0 – 100% | 80% | Alarm tone volume. |
| Tone | Siren / Beep / Pulse / Horn | Siren | Synthesised alarm sound. |

***

## 📸 Auto-Screenshot

When **Auto-screenshot** is enabled, the app saves a PNG to your Downloads folder every time an alarm fires.

- **Filename format:** `motiondetect_YYYYMMDD_HHMMSS.png`
- **Example:** `motiondetect_20260415_143022.png`
- Screenshots capture the **raw video frame only** — zone overlays are not included
- A brief white flash confirms each capture
- The screenshot counter in the sidebar tracks the session total

Toggle it off if you only want alerts without saving files.

***

## 🔧 How It Works

The app combines three browser-native APIs — no libraries or frameworks:

```
getDisplayMedia() / getUserMedia()  →  video stream from screen or webcam
Canvas getImageData()               →  per-pixel frame differencing
Web Audio API                       →  synthesised alarm tones (no audio files)
```

### Motion Detection Algorithm

Every third frame, each zone is scanned with a 2×2 pixel stride:

```
delta = (|R₁-R₀| + |G₁-G₀| + |B₁-B₀|) / 3

If delta > threshold  →  pixel marked as changed
If changed_pixels / total_pixels > minArea  →  zone triggers alarm
```

Sensitivity maps to threshold via: `threshold = (100 − sensitivity) × 2.5`

A sensitivity of 30% → threshold ≈ 175 out of 255.

***

## 🌐 Browser Compatibility

| Browser | Screen Capture | Webcam | Notes |
|---------|:--------------:|:------:|-------|
| Safari 17+ (macOS) | ✅ | ✅ | Requires Screen Recording permission |
| Chrome 120+ | ✅ | ✅ | Prompts on first use |
| Edge 120+ | ✅ | ✅ | Same as Chrome |
| Firefox | ⚠️ | ✅ | Window-level selection limited |

> `getDisplayMedia()` requires **HTTPS or localhost**. GitHub Pages provides HTTPS automatically.

***

## 📁 Repository Structure

```
motion-detector/
├── index.html    # Complete application — single self-contained file (~40 KB)
└── README.md     # This file
```

No `package.json`. No build step. No node_modules. The entire app is one HTML file.

***

## 🛠️ Deploying to GitHub Pages

1. Fork or clone this repository
2. Go to **Settings → Pages**
3. Set Source to **Deploy from branch → main → / (root)**
4. Save — your app is live at `https://YOUR-USERNAME.github.io/motion-detector/` within ~1 minute

Every push to `main` automatically redeploys.

***

## 🔒 Privacy

- No backend, no API calls, no analytics
- Video frames never leave your device
- Screenshots are saved directly to your local Downloads folder
- The only external request is loading [DM Sans & DM Mono](https://fonts.google.com) from Google Fonts CDN (can be removed for fully offline use)

***

## 📄 License

[MIT](LICENSE) — free to use, modify, and distribute.

***

## 💡 Use Cases

- Monitor a doorway or entrance via a VLC network stream
- Watch a specific screen area for changes during unattended sessions
- Simple webcam security monitor for home or office
- Detect activity in a timelapse or security feed played back in VLC
- Starting point for browser-based computer vision experiments
