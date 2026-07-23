# Building Wellwish CRM as a real .apk

This folder is a ready-to-go **Capacitor** wrapper around your CRM. Capacitor takes
your `www/index.html` and packages it into a real native Android project, which you
then build into a `.apk` (or `.aab` for the Play Store) using Android Studio.

You need to run these steps on your own computer — they require tools (Android SDK,
Java, Gradle) that this chat environment doesn't have.

## Prerequisites (install once)

1. **Node.js** (v18+) — https://nodejs.org
2. **Android Studio** — https://developer.android.com/studio
   (this installs the Android SDK and an emulator for you)

## Steps

1. Unzip this `capacitor-project` folder and open a terminal inside it.

2. Install dependencies:
   ```
   npm install
   ```

3. Add the Android platform:
   ```
   npx cap add android
   ```
   This generates a full native `android/` folder — your real Android project.

4. Every time you edit `www/index.html`, re-sync it into the native project:
   ```
   npx cap sync android
   ```

5. Open the project in Android Studio:
   ```
   npx cap open android
   ```

6. In Android Studio: **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
   The finished file appears at:
   ```
   android/app/build/outputs/apk/debug/app-debug.apk
   ```
   You can install that file directly on any Android phone (enable
   "Install unknown apps" for your file manager/browser first).

7. **For a Play Store–ready release build** (signed, optimized): in Android
   Studio choose **Build → Generate Signed Bundle / APK**, create a signing
   key when prompted, and follow the wizard. Keep that keystore file safe —
   you'll need the same one for every future update.

## Notes

- The app works fully offline after first load — the service worker
  (`sw.js`) caches the app shell, and your CRM data is stored locally in
  the browser storage inside the app (nothing is sent to a server).
- App name and package ID are set in `capacitor.config.json` — change
  `appId` (e.g. `com.yourbusiness.crm`) before your first build if you want
  something other than `com.wellwish.crm`.
- App icon: replace `www/icon-192.png` and `www/icon-512.png` with your own
  artwork, then re-run `npx cap sync android` — Capacitor will pick it up
  as the launcher icon on the next build (or set proper adaptive icons via
  Android Studio's Image Asset tool for a polished result).
