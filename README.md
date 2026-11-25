# Yachti

**Yachti** is a free, zero-subscription 5-dice game and live synced scorecard built to run right in your phone or laptop browser — online, offline, or at 35,000 feet.

## What You Can Do

- **Play with 3D Dice, vs. Computer, or Real Dice:** Roll five top-down 3D dice on screen (solo, **2-player vs. Computer** with Casual/Pro AI, pass-and-play on one phone, or linked across up to 6 phones), or switch to **Digital Scorecard Mode** when you are rolling real dice on the table.
- **Drag Dice to the Side to Keep Them:** Between rolls, drag any die into the right-hand **Reserve Bar** (or tap it to slide it across) so your kept dice stay clearly readable while you roll the rest.
- **No-Mistake Scorecard:** On your turn, your screen focuses cleanly on your own score column and previews every legal score — including the 63-point Upper Bonus pace, +100 Yachti Bonuses, and the official Joker Rule. Tap once to score, or hit **Undo** if your thumb slips.
- **Link Phones in Seconds:** Share a **4-letter Room Code**, tap **Copy / Share Link**, or point a phone camera at the built-in **QR Code**. Everyone sees rolls, kept dice, and scores update live.
- **Play Together on a Plane (No Wi-Fi or Data):** Use **Airplane Sync** over Bluetooth Tethering or an offline personal hotspot. Two quick QR scans link devices directly with zero internet required.

## Installing on Your Phone or Laptop

Once hosted on GitHub Pages (or opened over HTTPS):

- **Android (Chrome):** Open the menu (`☰`) inside Yachti and tap **Install App**, or use Chrome's **Add to Home screen**.
- **iPhone / iPad (Safari):** Tap Safari's **Share** button and choose **Add to Home Screen**.
- **Desktop (Chrome / Edge):** Click **Install App** in the menu or the install icon in the address bar.

Once installed, Yachti launches full-screen and works completely offline.

## Quick Hosting on GitHub Pages

1. Create a public GitHub repository (e.g., `yachti`) and upload all files in this folder (`index.html`, `manifest.json`, `sw.js`, `README.md`, and `icons/`).
2. Go to **Settings → Pages**, choose **Deploy from a branch** (`main` / `/ (root)`), and save.
3. Open your live `https://<username>.github.io/yachti/` URL on any phone or computer.

---

## Technical Architecture (v1.2.0)

- **Zero External Dependencies:** Single-file static application (`index.html`) with built-in 3D projected dice renderer, Web Audio API sound synthesizer, ISO/IEC 18004 QR code encoder, and camera QR scanner (`BarcodeDetector` with manual paste fallback).
- **Deterministic Roll Sync:** Active devices broadcast a compact `ROLL_START` payload (`rollId`, `turnSeq`, `rollNumber`, `seed`, `heldMask`, `initialImpulses`, `finalValues`, `startedAt`) so linked devices animate the same trajectory and converge identically.
- **Multi-Transport Online Sync:** Combines `BroadcastChannel`, multi-broker binary MQTT 3.1.1 over WSS (automatic failover + 20s keep-alive), and DTLS-encrypted WebRTC `RTCPeerConnection` DataChannels, strictly validated by `roomId` and cryptographic `sessionToken`.
- **Serverless Offline / Airplane Sync:** Exchanges compact Offer/Answer SDP payloads via two QR scans over OS Bluetooth Tethering (PAN) or an offline local Wi-Fi Hotspot to establish direct peer-to-peer WebRTC DataChannels with zero internet servers.
- **PWA & Offline Resilience:** Network-first Service Worker (`sw.js`) caches core assets for instant offline startup; active game state persists automatically in `localStorage`.
