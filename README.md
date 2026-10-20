# Package Forge

An offline Android app that turns an installed app, APK, or compatible split archive into a renamed, signed standalone APK. Android 8.0+ (API 26). No root, account, server or internet permission. Useful for converting emulators and games into a "Benchmark version", since phones boost their GPU and CPU clocks dramatically if they think they are running some sort of benchmarking app. Built entirely with vibe-coding.

## Use it

1. Install `PackageForge-1.0.0.apk` on your phone. Android may ask you to allow APK installation from the app you opened it with.
2. Open **Package Forge**. Choose **Installed apps** or **Open file**. For separate split files, select the base APK and all its required splits together.
3. Pick AnTuTu Benchmark, Geekbench 6 or Geekbench 5, or enter a custom package name. Add an app name if desired.
4. Tap **Create APK**. You can leave the screen while the foreground service works. Cancellation takes effect at the next processing checkpoint.
5. **Save APK**, **Share**, or **Install APK**. Installation always goes through Android's installer and requires your action. Installing from Package Forge requires its “Allow from this source” setting.

The original app stays installed. A clone starts with fresh data; this app does not copy saves, settings, accounts or private app data. Exporting an installed app includes its current base and installed splits, so the result may only suit the device configurations and features actually present on that device.

## What it changes

- Merges a single compatible split set using REAndroid ARSCLib, including resources, assets, native libraries, feature manifests and additional DEX files.
- Changes the manifest package and compiled resource package.
- Resolves relative component class names to their original full names so activities, services and providers can still load.
- Renames declared provider authorities and custom permissions, and updates matching manifest uses. Updates package-based task affinities, processes and custom actions/categories. Removes shared-user and split-install requirements.
- Optionally patches exact package and mapped identity string literals and matching `content://` authorities in every `classes*.dex`, preserving class namespaces. This is a targeted patch, not a blanket text replacement or complete code refactor.
- Removes the original APK signature, writes a standalone APK, signs it with v1/v2 signatures, verifies the signatures and asks Android to parse the resulting package before offering it for export.

Each Package Forge installation generates its own RSA signing key inside Android Keystore. Repeated exports from the same installation use that key. Uninstalling Package Forge or clearing its data loses the key. A later clone may need a fresh installation instead of updating an existing clone; removing that clone normally deletes its data. Exported APKs are kept in private app storage until you clear them or uninstall the app.

## Compatibility limits

Renaming does **not** directly control CPU/GPU clocks or guarantee a performance increase. Whether benchmark package names trigger different scheduling or clocks depends on the phone and firmware.

Supported inputs are standalone APKs, compatible APK-only APKM/XAPK/APKS ZIP archives with one base APK, and explicitly selected base-plus-split APK files. If an archive contains `universal.apk`, that APK is preferred.

The following are intentionally rejected or not repaired:

- AAB source bundles, encrypted archives, archives without APKs, multiple bases/device variants, duplicate split identities, and mismatched app versions/packages.
- XAPK archives with OBB expansion files. OBB data and Play Asset Delivery packs are not relocated or converted.
- Resource-based provider authorities. Literal authorities are supported.
- Native-code package checks, string construction, class reflection tied to a package, signature/integrity checks, store licensing, billing, OAuth certificate restrictions, and code expecting split-install paths or on-demand delivery. Some apps will install but fail to run.
- Package collisions with already installed benchmark apps. Android refuses a conflicting signature; Package Forge does not remove other apps.

The parser limits inputs to 4 GiB combined and 512 APKs. Large apps can still exceed available memory. Keep roughly four times the original input size free before starting; the service checks for at least three times the imported size after copying.

## Build

Open this projects's folder in Android Studio, or install JDK 17 and Android SDK Platform 35, set `ANDROID_HOME` (or create your own `local.properties`), then run:

```powershell
.\gradlew.bat :app:assembleDebug :engine:test :app:lintDebug
```

On macOS/Linux, use `sh ./gradlew` instead. The wrapper downloads Gradle 8.10.2. Maven dependencies are Android Gradle Plugin/apksig 8.7.3 and JUnit 4.13.2 for tests. ARSCLib is vendored with its source and license; no JitPack build is needed. First builds need internet access to fetch the development dependencies; the installed app runs offline.

The test `fixture` module is a tiny separate app used to exercise real compiled manifests, resources, locale variants and multiple DEX files. It is not included in Package Forge. `engine:test` builds it automatically.

For a release APK, use `:app:assembleRelease`, then align and sign the unsigned APK with your own Android signing key, or configure Android Studio's **Generate Signed APK**. Keep the same app signing key for future Package Forge updates. The key that signs Package Forge itself is separate from the per-installation Keystore key that signs its exported clones.

### Device smoke test

The framework-only test runner requires a disposable Android emulator or test device. It creates a signed clone of the fixture, so do not run it on a device that already has AnTuTu installed.

```powershell
.\gradlew.bat :app:assembleDebug :app:assembleDebugAndroidTest :fixture:assembleDebug
adb install -r fixture/build/outputs/apk/debug/fixture-debug.apk
adb install -r app/build/outputs/apk/debug/app-debug.apk
adb install -r app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk
adb shell am instrument -w dev.packageforge.test/dev.packageforge.SmokeInstrumentation
```

The test exercises extraction, manifest/resource/DEX changes, foreground processing, the actual Android Keystore signing path, signature verification and UI launch. It writes `smoke-clone.apk` and `smoke-screen.png` under the app's external files directory. Install that clone on the disposable emulator and launch `com.antutu.ABenchMark/dev.packageforge.fixture.ProbeActivity` to check its `PKG_OK` and `PROVIDER_OK` results. This runner is invoked explicitly with `am instrument`; it is not a JUnit test-discovery runner.

## Project layout

- `app/`: Android interface, foreground service, device-local signing, and read-only export provider.
- `engine/`: platform-independent import, merge and identity rewrite pipeline with regression tests.
- `fixture/`: compiled Android fixtures for validation.
- `vendor/arsclib/`: pinned upstream resource and DEX editing library.

See `THIRD_PARTY_NOTICES.md` for provenance and licenses.
"# PackageForge" 
