# Validation — 20 October 2026

## Passed

- Release and debug Android builds; test runner APK build.
- Eight JVM regression tests, using a compiled Android fixture APK with multiple DEX files, resources, and locale variants.
- Package/resource package renaming; app and activity labels; declared custom permissions and provider authorities.
- Relative and unqualified component class-name preservation, and removal of shared-user metadata.
- Targeted DEX package/authority/content-URI rewrites while preserving original class descriptors.
- Byte-for-byte DEX preservation with the code-patching option disabled.
- Merging extra split DEX code, assets and a Spanish resource configuration; split manifest sanitation.
- Rejection of duplicate bases, unchanged/invalid package names, non-APK archives and OBB data; extraction path containment, byte limits and cancellation checks.
- Android lint: **0 errors, 20 warnings**. Warnings include localization, advisory SDK/library updates, backup configuration and worker-thread preference writes. Broad installed-package visibility is intentionally declared for this sideloaded app picker.
- Final release APK: ZIP alignment check and Android `apksigner` verification passed; v2 and v3 signatures.
- Package metadata: `dev.packageforge`, version `1.0.0`, minimum API 26, target API 35, no INTERNET permission.

## Not verified here

Actual device installation, UI rendering, the Android Keystore signing path, foreground-service behavior, and launching an exported clone remain unverified. Two official local emulator versions were attempted; neither reached a usable Android boot in this host environment. The on-device smoke-test runner is included for reproducible follow-up testing.

The test suite establishes structural behavior for the included fixtures. It does not establish compatibility with every emulator/game/app, native package checks, integrity-protected apps, or any specific phone's performance policy. No performance boost was measured or claimed.

## Release identity

File: `PackageForge-1.0.0.apk`

SHA-256:

```
66db22bd451636521a62cadc563ab1c50d65102954d99d72b2e51b99c20a5daa
```

Signing certificate SHA-256:

```
a1bb70a35660dc647ce350d3a9c57ec49e774116c14d23018230c6d795ef1b9d
```

The source archive excludes local SDK paths, generated build folders, machine caches and private signing keys. It includes the Gradle wrapper and pinned ARSCLib source. The distributed release is not marked debuggable.
