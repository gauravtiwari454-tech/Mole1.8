# Mole — Android app

This folder is the complete Android app. It runs the same Mole screens you already have,
plus a small native layer that handles what WebIntoApp can't:

| What | How it works now |
|---|---|
| Back button | Closes the open receipt, Settings, Trends, editors. Exits only from the home screen. |
| Gallery | Opens your phone's Gallery app. Take photo opens the Camera, Attach file opens Files. |
| Share receipt | Opens the Android share sheet with the actual file (WhatsApp, Telegram, Gmail…). |
| Share the app | WhatsApp and Telegram buttons open those apps directly with the Play Store link. |
| Save copy / exports | Saved straight away, correct name and extension, no "Save As" box. |
| Folders | `Documents/Mole/Receipts` and `Documents/Mole/Exports` on the phone. Every receipt you add is also copied to Receipts, named like `2026-10-02_Food_500.jpg`. |
| Vibration | Works (the app declares the vibrate permission). |

The screens themselves are in `app/src/main/assets/www/` — the same files as the WebIntoApp version.

---

## Option A — Build with GitHub (nothing to install)

1. Create a free account at github.com, then **New repository** → name it `mole` → **Private** → Create.
2. On the new repo page click **uploading an existing file**, drag in **everything inside this folder**
   (including the hidden `.github` folder — on a computer, open the folder and select all; if `.github`
   doesn't show, enable "show hidden files"), then **Commit changes**.
3. Open the **Actions** tab. A build named "Build Mole app" starts automatically (about 5–8 minutes).
4. When it shows a green tick, open it and download **Mole-test-apk** at the bottom. Unzip it and
   install `app-debug.apk` on your phone (allow "install unknown apps" when asked).

## Option B — Build with Android Studio

Open this folder in Android Studio → wait for it to finish syncing → **Build ▸ Build App Bundle(s) / APK(s) ▸ Build APK(s)**.

---

## Before you publish on Google Play

1. **Choose your app ID** in `app/build.gradle` (`applicationId "com.mole.app"`), e.g. `com.yourname.mole`.
   It can never change once the app is on Play.
2. **Create an upload key** (once, keep it forever, never lose it):
   `keytool -genkey -v -keystore mole.jks -keyalg RSA -keysize 2048 -validity 10000 -alias mole`
   (Android Studio can also do this: Build ▸ Generate Signed App Bundle ▸ Create new.)
3. In your GitHub repo: **Settings ▸ Secrets and variables ▸ Actions ▸ New repository secret**, add:
   - `MOLE_KEYSTORE_BASE64` — the key file as base64 (`base64 -w0 mole.jks` on Linux/Mac,
     `certutil -encode mole.jks out.txt` on Windows and paste the middle part)
   - `MOLE_KEYSTORE_PASSWORD`, `MOLE_KEY_ALIAS` (`mole`), `MOLE_KEY_PASSWORD`
4. Re-run the build. You'll also get **Mole-play-store-release** containing the `.aab` to upload to Play Console.
5. For every update, raise `versionCode` (1 → 2 → 3…) and `versionName` in `app/build.gradle`.
6. Put your Play Store link in `app/src/main/assets/www/config.js` (`PLAY_STORE_LINK`). If left empty,
   the app builds the link from its own app ID automatically.

## Moving your existing entries over

The new app has its own storage, separate from the WebIntoApp app. In the old app tap
**Export backup**, then in the new app tap **Import backup** and pick that file.

## Updating the screens later

Replace the files in `app/src/main/assets/www/` with the new ones and rebuild.
