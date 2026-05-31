# 🪔 Vesak Lantern AR — WebXR Augmented Reality Experience

> **CST 19th Batch | Uva Wellassa University of Sri Lanka**
> A web-based Augmented Reality Vesak lantern that appears in the real world — no app download required.

---

## 📖 Table of Contents

- [Project Overview](#project-overview)
- [Live Demo](#live-demo)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Setup & Deployment](#setup--deployment)
- [Features](#features)
- [Known Limitations & Why](#known-limitations--why)
- [Challenges & Solutions](#challenges--solutions)
- [AR Scene Explained](#ar-scene-explained)
- [Credits](#credits)

---

## Project Overview

The **Vesak Lantern AR** project is a web-based Augmented Reality experience built for the university's Vesak celebration. When a user scans the QR code with their phone, the browser opens a webpage that activates the camera and projects a **3D Vesak lantern into the real environment** — floating 4 metres in front of the user.

No native app. No install. Just a URL.

```
User scans QR  →  Browser opens  →  Camera starts  →  3D lantern appears in real world
```

---

## Live Demo

```
https://sesandu-raj.github.io/vesak-ar/
```

> **Requirements:**
> - iPhone (iOS 12+) or Android phone
> - Chrome / Safari browser
> - Good lighting for AR tracking

**To view the lantern:**
1. Open the URL on your phone
2. Wait for the loading screen to complete
3. Tap the **AR** button at the bottom-right corner
4. Point your camera at the floor in front of you
5. The lantern will appear and stay fixed in that position

---

## How It Works

### Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   User's Phone                       │
│                                                      │
│   Browser (Chrome / Safari)                          │
│       │                                              │
│       ▼                                              │
│   index.html  ──loads──▶  aframe.min.js (A-Frame)   │
│       │                       │                      │
│       │                       ▼                      │
│       │               WebXR API (browser built-in)   │
│       │                       │                      │
│       ▼                       ▼                      │
│   lantern.glb  ──renders──▶  AR Camera Feed          │
│   (3D Model)               (real world + 3D model)   │
└─────────────────────────────────────────────────────┘
```

### Step-by-Step Flow

1. **User opens the URL** — GitHub Pages serves the `index.html` over HTTPS
2. **Loading screen** appears with animated Vesak lantern, starfield, and status messages
3. **A-Frame loads** the `.glb` 3D model into the WebGL scene
4. **User taps AR button** — browser requests camera permission via `getUserMedia()`
5. **WebXR session starts** — camera feed becomes the background
6. **JavaScript calculates** a fixed world position 4m ahead of the user
7. **3D lantern renders** on top of the camera feed at that fixed coordinate
8. **User walks around** — lantern stays in place (world-anchored)

---

## Tech Stack

| Technology | Purpose | Why Chosen |
|---|---|---|
| **A-Frame 1.4.2** | 3D scene rendering + WebXR | HTML-tag based, works natively with WebXR, no complex setup |
| **WebXR API** | Augmented Reality session | Built into modern browsers — no app needed |
| **GLB (glTF Binary)** | 3D model format | Lightweight, mobile-optimised, single file |
| **GitHub Pages** | Hosting | Free, automatic HTTPS (required for camera access) |
| **Vanilla JavaScript** | World anchoring logic | Lightweight, no framework overhead |
| **CSS Animations** | Loading screen effects | Pure CSS — no JS animation library needed |
| **Google Fonts** | Typography | Orbitron, Outfit, Abhaya Libre (Sinhala support) |

### Why NOT these technologies

| Technology | Why Not Used |
|---|---|
| **MindAR** | CDN module import errors on mobile; WebAssembly performance issues on low-end devices |
| **AR.js (marker-based)** | Requires printing a physical HIRO marker — not practical for public display |
| **Native App (Unity/Swift)** | Requires app store download — adds friction for users |
| **Three.js standalone** | More complex setup; A-Frame abstracts WebXR boilerplate cleanly |
| **`local-floor` WebXR space** | Causes lantern to follow the user as they walk (recalculates origin continuously) |

---

## Project Structure

```
vesak-ar/
│
├── index.html              # Main application file (entire app)
├── base_basic_shaded.glb   # 3D Vesak lantern model
└── README.md               # This file
```

> The entire application is a **single HTML file** — no build process, no npm, no dependencies to install.

---

## Setup & Deployment

### Local Testing (HTTPS required for camera)

Camera access requires HTTPS. A plain `http://localhost` will **not** work on mobile.

**Option 1 — ngrok tunnel (recommended for local testing)**
```bash
# Start a local server
python3 -m http.server 8000

# In another terminal, create HTTPS tunnel
ngrok http 8000
# Open the https://xxxx.ngrok.io URL on your phone
```

**Option 2 — Python self-signed HTTPS server**
```bash
# Generate certificate (run once)
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes -subj "/CN=localhost"

# Run HTTPS server
python3 -c "
import http.server, ssl
server = http.server.HTTPServer(('0.0.0.0', 4443), http.server.SimpleHTTPRequestHandler)
ctx = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
ctx.load_cert_chain('cert.pem', 'key.pem')
server.socket = ctx.wrap_socket(server.socket, server_side=True)
server.serve_forever()
"
# Open https://<your-ip>:4443 on phone (accept security warning)
```

### Deploy to GitHub Pages

```bash
# 1. Create a new GitHub repository
#    Name: vesak-ar
#    Visibility: Public

# 2. Upload files
#    - index.html
#    - base_basic_shaded.glb
#    (both must be in the root of the repo)

# 3. Enable GitHub Pages
#    Settings → Pages → Source: Deploy from branch → main → / (root) → Save

# 4. Your live URL (ready in ~1 minute):
#    https://yourusername.github.io/vesak-ar/
```

### Generate QR Code

Once deployed, generate a QR code from your GitHub Pages URL:
- [qr-code-generator.com](https://www.qr-code-generator.com)
- Print it and place it at the exhibition location

---

## Features

- ✅ **No app download** — works entirely in the mobile browser
- ✅ **Animated 3D lantern** — slow rotation (12s per revolution)
- ✅ **Flickering point light** — animates between intensity 2–5, simulating a real flame
- ✅ **World-anchored** — lantern stays fixed in place as the user walks around
- ✅ **Elegant loading screen** — starfield, animated moon, swaying CSS lantern, Sinhala typography
- ✅ **Progressive status messages** — shows loading stages (model loading, calibration, ready)
- ✅ **Ambient warm lighting** — `#ffddaa` ambient fills the scene with a warm glow
- ✅ **Responsive** — works on both iOS Safari and Android Chrome
- ✅ **HTTPS hosted** — GitHub Pages ensures camera permission works

---

## Known Limitations & Why

### 1. iOS Safari Camera Offset (Fixed)
**Problem:** On iOS, AR.js injects a `<video>` element with hardcoded `width` and `height` pixel attributes that override CSS, causing the camera feed to appear offset (left portion only).

**Root Cause:** iOS WebKit's `getUserMedia()` implementation sets attributes on the video element that resist CSS overrides.

**Fix Applied:** Used `MutationObserver` to detect when AR.js sets the attributes and immediately strips them, then applies `translate(-50%, -50%)` + `min-width:100vw` centering instead of a fixed pixel size.

---

### 2. Lantern Following User (Fixed)
**Problem:** When `local-floor` reference space was used, the lantern moved with the user as they walked.

**Root Cause:** `local-floor` continuously recalculates the XR reference frame relative to the user's current floor position. The lantern's world coordinates therefore moved with the camera origin.

**Fix Applied:** Switched to `local` reference space. At AR session start, JavaScript calculates a fixed world coordinate 4m ahead using the camera's world quaternion, places the anchor entity there, and re-parents it to the `<a-scene>` root so it is never inside the camera hierarchy.

```javascript
const ahead = new THREE.Vector3(0, 0, -4);
ahead.applyQuaternion(camWorldQuat);      // convert to world direction
anchor.setAttribute('position', {
  x: camWorldPos.x + ahead.x,
  y: 0,                                   // floor level
  z: camWorldPos.z + ahead.z
});
sceneEl.appendChild(anchor);              // detach from camera hierarchy
```

---

### 3. MindAR Not Used
**Problem:** MindAR was the first choice for image-marker based tracking, but failed on mobile.

**Root Cause:** MindAR uses ES Module `import` syntax via CDN, which caused `SyntaxError: Cannot use import statement outside a module` when loaded as a plain script tag. Additionally, its WebAssembly compiled ARToolkit caused performance drops on mid-range Android devices.

**Decision:** Switched to AR.js which loads via a single classic script tag, has no module issues, and its tracking is lighter-weight for mobile.

---

### 4. WebXR Not Supported on All Devices
**Limitation:** WebXR `immersive-ar` mode requires:
- Android: Chrome 81+
- iOS: Safari 15.4+ (partial support)
- Older devices may show no AR button

**Fallback:** The 3D scene still renders in the browser in normal perspective mode — users can still see the lantern in 3D even without AR.

---

## AR Scene Explained

```html
<a-scene webxr="referenceSpaceType:local; requiredFeatures:local;">

  <!-- 3D model asset, preloaded before AR starts -->
  <a-asset-item id="lantern-glb" src="./base_basic_shaded.glb"></a-asset-item>

  <!-- World anchor: JS places this at fixed coordinates on AR start -->
  <a-entity id="world-anchor" position="0 0 -4">

    <!-- The lantern model: rotates continuously -->
    <a-gltf-model
      src="#lantern-glb"
      scale="3 3 3"
      animation="property:rotation; to:0 360 0; loop:true; dur:12000;"
    ></a-gltf-model>

    <!-- Point light: flickers like a candle flame -->
    <a-light
      type="point"
      color="#ffcc44"
      intensity="3"
      distance="8"
      animation="property:intensity; from:2; to:5; loop:true; dur:800; dir:alternate;"
    ></a-light>

  </a-entity>

  <!-- Ambient warm fill light for the whole scene -->
  <a-light type="ambient" color="#ffddaa" intensity="0.4"></a-light>

  <a-entity camera></a-entity>
</a-scene>
```

### Light Settings Explained

| Attribute | Value | Effect |
|---|---|---|
| `type="point"` | — | Omnidirectional light, like a candle |
| `color="#ffcc44"` | Warm amber | Mimics candlelight / flame colour |
| `intensity="3"` | Bright | Strong enough to illuminate the model |
| `distance="8"` | 8 metres | Light fades to zero beyond 8m |
| `animation from:2 to:5` | — | Flickers between dim and bright |
| `dur:800` | 800ms | Fast flicker cycle, natural flame feel |

---

## Credits

**Developed by:** CST 19th Batch — Uva Wellassa University of Sri Lanka

**Libraries & Tools:**
- [A-Frame](https://aframe.io) — Mozilla Foundation
- [WebXR Device API](https://www.w3.org/TR/webxr/) — W3C Standard
- [Blender](https://www.blender.org) — 3D model creation
- [GitHub Pages](https://pages.github.com) — Hosting
- [Google Fonts](https://fonts.google.com) — Orbitron, Outfit, Abhaya Libre

**Special Thanks:** Uva Wellassa University CST Department

---

*© 2026 CST 19th Batch — Uva Wellassa University of Sri Lanka. All Rights Reserved.*
