# CENTER V7 — APK Build Guide

There are **3 ways** to turn CENTER_V7.html into an Android APK.
Pick the one that fits your setup.

---

## ✅ OPTION 1 — PWABuilder (Easiest, no coding)

**Best for: anyone who wants an APK in under 10 minutes.**

### Steps

1. Host the app for free on GitHub Pages:
   - Go to https://github.com → New repository → name it `center-app`
   - Upload `CENTER_V7.html`, `manifest.json`, `sw.js`
   - Rename `CENTER_V7.html` → `index.html` (or update `start_url` in manifest.json)
   - Go to **Settings → Pages** → Source: `main` branch → Save
   - Your URL will be: `https://YOUR_USERNAME.github.io/center-app`

2. Go to **https://www.pwabuilder.com**

3. Paste your GitHub Pages URL and click **Start**

4. Click **Android** → **Generate Package**

5. Download the `.apk` or `.aab` file

6. Install on Android:
   - Enable **Settings → Install unknown apps** on your phone
   - Transfer the `.apk` and tap to install

> To publish on Google Play, use the `.aab` + signing key from PWABuilder.

---

## ✅ OPTION 2 — Capacitor (Local build, full native features)

**Best for: developers who want a proper signed APK.**

### Requirements
- Node.js 18+ → https://nodejs.org
- Android Studio → https://developer.android.com/studio
- Java 17+ (bundled with Android Studio)

### Steps

```bash
# 1. Create a project folder and copy files into it
mkdir center-apk
cd center-apk

# 2. Init npm
npm init -y

# 3. Install Capacitor
npm install @capacitor/core @capacitor/cli @capacitor/android

# 4. Init Capacitor (answer the prompts)
npx cap init CENTER "com.center.app" --web-dir www

# 5. Create the www folder and place your files there
mkdir www
# Copy CENTER_V7.html → www/index.html
# Copy manifest.json  → www/manifest.json
# Copy sw.js          → www/sw.js

# 6. Add Android platform
npx cap add android

# 7. Sync files to Android
npx cap sync android

# 8. Open in Android Studio to build the APK
npx cap open android
```

In Android Studio:
- **Build → Build Bundle(s) / APK(s) → Build APK(s)**
- Find the APK at `android/app/build/outputs/apk/debug/app-debug.apk`

---

## ✅ OPTION 3 — Bubblewrap CLI (Trusted Web Activity)

**Best for: Google Play Store submission.**

### Requirements
- Node.js 18+
- JDK 11+ → https://adoptium.net
- Android SDK

```bash
# Install Bubblewrap
npm install -g @bubblewrap/cli

# Init TWA project (replace URL with your hosted app URL)
mkdir center-twa
cd center-twa
bubblewrap init --manifest https://YOUR_USERNAME.github.io/center-app/manifest.json

# Build the APK
bubblewrap build
```

Output: `app-release-signed.apk` ready for Play Store.

---

## 📁 File Checklist

Make sure you have these 3 files together:

| File | Purpose |
|------|---------|
| `CENTER_V7.html` | The app (rename to `index.html` for hosting) |
| `manifest.json` | PWA identity & icons config |
| `sw.js` | Offline service worker |

---

## 📱 Quick Install (Android, no APK needed)

1. Open Chrome on Android
2. Go to your hosted URL
3. Tap the **⋮ menu → Add to Home Screen**
4. The app installs like a native app with its own icon

---

## 🔑 Notes

- The `manifest.json` references icon files in an `icons/` folder.
  You can generate all sizes for free at: https://realfavicongenerator.net
  (Upload any square image, download the pack, put PNG files in an `icons/` folder)
- All data is stored in `localStorage` — it persists across app restarts
- The service worker (`sw.js`) makes CENTER work fully offline
