# 🚀 Deployment

CLIMO is a static site, so it can be deployed three ways from the same folder.

| Target | How | Output |
|---|---|---|
| 🌐 **GitHub Pages** | Serve the repository root | `https://<user>.github.io/CLIMO-Web-Mobile-Application/` |
| 📦 **Android APK (CI)** | Push to `main`/`master` | `climo-app-debug-apk` workflow artifact |
| 🛠️ **Android APK (local)** | Cordova CLI | `app-debug.apk` / signed release |

---

## 1. GitHub Pages

The app lives in `CLIMO-fixed/CLIMO-project/`. The repository-root `index.html` exists only to redirect visitors there (by meta refresh, `location.replace`, and a fallback link), so the Pages URL never shows a 404.

1. Open **Settings → Pages**.
2. Under **Source**, choose *Deploy from a branch*, then `main` and `/ (root)`.
3. Save, and wait for the *pages-build-deployment* run to finish.

> Geolocation (**My Location**, GPS buttons) needs HTTPS. Pages provides it.

---

## 2. Android APK in CI

Workflow: [`.github/workflows/android.yml`](../.github/workflows/android.yml)
Triggers: `push` and `pull_request` on `main` / `master`

| Step | Detail |
|---|---|
| Checkout | `actions/checkout@v4` |
| Node | `actions/setup-node@v4`, Node **20** |
| Wrap | `npm i -g cordova` → `cordova create mobileapp com.climo.webapp "Climo App"` → copy the web files into `mobileapp/www/` → `cordova platform add android` |
| Java | `actions/setup-java@v4`, Zulu **17**, Gradle cache |
| Build | `cordova build android --debug` |
| Upload | `actions/upload-artifact@v4` → **`climo-app-debug-apk`** |

**To download:** open **Actions → Build CLIMO Web App to APK → (latest run) → Artifacts → climo-app-debug-apk**. Unzip it, copy `app-debug.apk` to your phone, and allow *Install unknown apps*.

> The splash page (`index.html`) becomes Cordova's start page, so the app opens on the splash screen and then goes to the landing page.

---

## 3. Build the APK locally

**Prerequisites:** Node 20+, JDK 17, Android SDK (with `ANDROID_HOME` set) and Gradle.

Install the Cordova CLI:

```bash
npm install -g cordova
```

Create a Cordova project next to the web app:

```bash
cd CLIMO-fixed/CLIMO-project && cordova create mobileapp com.climo.webapp "Climo App"
```

Copy the web files into it:

```bash
cp -R index.html CLIMO.html CLIMO_backend.html css js mobileapp/www/
```

Add Android and build:

```bash
cd mobileapp && cordova platform add android && cordova build android --debug
```

The APK is written to `platforms/android/app/build/outputs/apk/debug/app-debug.apk`.

### Release build (signed)

Create a keystore once, and keep it **out of git**:

```bash
keytool -genkey -v -keystore climo-release.jks -alias climo -keyalg RSA -keysize 2048 -validity 10000
```

Then build a signed release:

```bash
cordova build android --release -- --keystore=climo-release.jks --alias=climo --storePassword=<pass> --password=<pass> --packageType=apk
```

In CI, keep the keystore and passwords in **GitHub Actions secrets**, never in the repository.

### Recommended `config.xml` additions

```xml
<preference name="Orientation" value="default" />
<preference name="AndroidWindowSplashScreenBackground" value="#020817" />
<allow-navigation href="https://*/*" />
<edit-config file="app/src/main/AndroidManifest.xml" mode="merge" target="/manifest">
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</edit-config>
```

For GPS inside the APK, add `cordova-plugin-geolocation` as well.

---

## 4. Checks before you ship

- [ ] Replace or proxy the OpenRouteService key (see [SECURITY.md](../SECURITY.md))
- [ ] Bump the version in `config.xml`
- [ ] Smoke test: the splash moves on, 90 stations load, a route works, the CSV downloads, the language switches
- [ ] Check the mobile layout at 360 px width
- [ ] Remember Open-Meteo's free-tier [terms](https://open-meteo.com/en/terms): it is for non-commercial use, and commercial use needs an API plan
