# Third-party notices

## REAndroid ARSCLib

Source: https://github.com/REAndroid/ARSCLib

Pinned commit: `222c0936e3740c7eb94f9bf37f8ec955d0113d64` (library version 1.4.1).

Copyright REAndroid and individual upstream contributors. Licensed under the Apache License, Version 2.0; full text is in `vendor/arsclib/LICENSE`. Original source headers are retained. The upstream Java source is unchanged. The local Gradle file selects the Android runtime build, substitutes the version property and excludes upstream platform compatibility stubs and test/unused packages from the Android JAR.

The `android.*` and `org.xmlpull.*` compatibility interfaces are retained in source for desktop compilation/tests and excluded from the APK. Android supplies these interfaces on-device. XmlPull interfaces reference the XmlPull public-domain license; see `vendor/arsclib/XMLPULL-LICENSE.txt`.

## Android apksig

Artifact: `com.android.tools.build:apksig:8.7.3`.

Source: https://android.googlesource.com/platform/tools/apksig/

Copyright The Android Open Source Project. Licensed under the Apache License, Version 2.0; the full license text is also provided in `vendor/arsclib/LICENSE`.

## Build/test tools

JUnit 4.13.2 and Hamcrest are test-only dependencies, not included in the app. Gradle's generated wrapper files retain their upstream license headers. Android SDK, JDK and build-tool downloads are not included in the source archive.
