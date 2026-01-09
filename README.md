# Screen Recorder

A lightweight browser-based screen recorder with optional microphone/system audio mixing and RNNoise-based noise reduction. This project is designed as a Progressive Web App (PWA) and can be installed on supported platforms.

## Features
- Record your screen (with cursor).
- Mix system/tab audio and microphone input (selectable).
- Optional RNNoise noise reduction for microphone input.
- Live preview during recording and automatic download of WebM recordings.
- PWA-ready (manifest + icons + service worker).

## Important files
- `www/index.html` — main web UI and recorder logic.
- `www/manifest.json` — PWA manifest referencing icons and display options.
- `www/icons/` — icon files used by the manifest. Add a maskable `icons/icon.svg` for best install results.
- `libs/rnnoise.js` — optional RNNoise WebAssembly wrapper for noise filtering.
- `server.js` — small Node static server (optional) at project root.

## Quick start (local)
Open the UI directly in a modern browser by serving the `www` folder. Serving is recommended for proper PWA and service worker behavior.

From the project root you can run a simple static server:

```bash
# using Python 3 built-in server
python3 -m http.server 8000 --directory www

# or using Node (http-server)
npx http-server www -p 8000
```

Then open:

```
http://localhost:8000
```

## Usage
1. Choose the audio source: `None`, `Microphone`, `System`, or `Microphone + System`.
2. If using microphone, select the preferred mic device from the dropdown.
3. Optionally enable `Enable Noise Filtering (RNNoise)` to reduce microphone background noise.
4. Click `Record` to start. Use `Pause` and `Stop` as needed. When recording stops the WebM file will be downloaded automatically.

## PWA / Icons
- For a proper maskable install icon, include `www/icons/icon.svg` (maskable SVG) and add this entry to `www/manifest.json`:

```json
{
  "src": "icons/icon.svg",
  "sizes": "any",
  "type": "image/svg+xml",
  "purpose": "any maskable"
}
```

- Also keep PNG fallbacks (72, 96, 128, 144, 152, 192, 384, 512) referenced in the manifest for broader compatibility.

## Generate PNGs from SVG
If you have ImageMagick installed:

```bash
convert -background none www/icons/icon.svg -resize 512x512 www/icons/icon-512.png
```

Or with `rsvg-convert`:

```bash
rsvg-convert -w 512 -h 512 www/icons/icon.svg -o www/icons/icon-512.png
```

## Notes & Troubleshooting
- Some browsers restrict `getDisplayMedia` (system audio) to secure contexts (HTTPS) or localhost. Use a local server or HTTPS for system audio capture.
- If microphone labels are empty, grant audio permission and reload; the app temporarily requests permission to enumerate devices.
- RNNoise requires the `libs/rnnoise.js` module to be available and may fail to initialize on unsupported browsers.

## License
This project uses the Apache 2.0 License (see `LICENSE`).
