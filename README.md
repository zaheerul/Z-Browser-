# Z-Browser (v0.1)

Android browser for OnePlus 13R (Android 16 / OxygenOS 16, arm64-v8a).

- **Rust core (`rust/zcore`)**: omnibox resolution, HTTPS upgrade, Brave's `adblock` engine for ad/tracker blocking. Exposed to Kotlin via UniFFI.
- **Android shell (`app`)**: plain-Views Kotlin UI (no Compose, for fast startup) around the system WebView.
- **Rendering/JS engine**: the system Android System WebView (Chromium). It is *not* a Rust engine.

## Versions (checked 26 Oct 2026)
| Item | Version | Source |
|---|---|---|
| uniffi | 0.32 (0.32.2 on crates.io) | crates.io |
| adblock | 0.12 (0.12.4 seen) | docs.rs |
| url | 2.5 | not re-checked |
| Kotlin | 2.3.21 | developer.android.com |
| AGP | 8.13.0 | NOT re-checked; newer may exist |
| JNA | 5.17.0 | NOT re-checked |
| compileSdk/targetSdk | 36 (Android 16) | |

## Build
Prereqs: Rust 1.85+, `cargo install cargo-ndk`, Android Studio, NDK r28+ (`ANDROID_NDK_HOME`), Gradle wrapper (open in Android Studio, or `gradle wrapper`).

```
./scripts/build-android.sh      # builds libzcore.so + Kotlin bindings
# then in Android Studio: Run on the phone (USB debugging on)
# or: ./gradlew assembleRelease && adb install app/build/outputs/apk/release/app-release.apk
```
Optional: `cd rust/zcore && cargo test` (host) to run the core's unit tests.

## Not verified
This was written without a Rust/Android toolchain, so **nothing has been compiled or run yet**. Expect to fix small API or version mismatches on first build (most likely: `adblock` API names, UniFFI flags, AGP version).

## Next steps
Tabs, bookmarks/history (SQLite via `rusqlite`), downloads, dark mode, EasyList auto-update, cosmetic filtering, find-in-page.

## Build the APK in the cloud (no local setup)
1. Create a free GitHub repo and upload this folder's contents (including the `.github` folder).
2. Open the **Actions** tab, run **Build Z-Browser APK** (it also runs on every push).
3. When it finishes, download the **Z-Browser-apk** artifact, unzip, and copy the `.apk` to the phone.
4. On the OnePlus 13R, open the file and allow "Install unknown apps" for your file manager.
If the run fails, open the failed step and send me its log.
