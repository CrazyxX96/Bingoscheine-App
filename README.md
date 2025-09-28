# Bingo PWA (iPhone-ready)

This folder is a ready-to-host Progressive Web App (PWA) wrapper for your Bingo tool.

## How to use on iPhone (no App Store):
1. Host these files on HTTPS (e.g., GitHub Pages, Netlify, Vercel).
2. Open the URL in Safari on your iPhone.
3. Tap the Share icon → **Add to Home Screen**.
4. Launch from your Home Screen. It will run standalone like an app and work offline after first load.

### Notes for iOS/Safari
- We added `manifest.webmanifest`, icons, and a `service worker` (`sw.js`) so the app can be installed and cached.
- `<input type="file" accept="image/*" capture="environment">` lets you use the rear camera directly.
- When served over HTTPS, the service worker will cache your app shell and CDN files when first fetched.

### Tesseract.js (OCR)
- Currently it is loaded from jsDelivr. For **full offline reliability**, self-host the Tesseract bundle and any `.traineddata` files and update the script tag to point to your local copy. Also add those files to APP_SHELL or cache them on install.
- iOS can be memory-sensitive. Prefer photos with moderate resolution to avoid WKWebView memory pressure.

## Optional: Publish as a real iOS app (App Store/TestFlight)

Use Capacitor (recommended):
```bash
npm create vite@latest bingo-pwa -- --template vanilla
# copy index.html, sw.js, manifest, icons into the project public/ folder
npm install @capacitor/core @capacitor/cli
npx cap init "Bingoschein" "de.example.bingo" --web-dir=public
npm install @capacitor/ios
npx cap add ios
npx cap copy
npx cap open ios   # opens Xcode
```

In Xcode:
- Set **Signing & Capabilities** (Team, Bundle ID).
- In `Info.plist`, add camera usage description if you use the camera picker:
  - `NSCameraUsageDescription` → "Zum Fotografieren des Bingoscheins"
- Build & run on device, or archive for TestFlight/App Store.

## Developer tips
- Service workers don't run from `file://`. Use `npm serve` or any static server locally.
- To tweak caching, edit `sw.js`.
- If you self-host Tesseract, add its files to `APP_SHELL` for true offline-first.