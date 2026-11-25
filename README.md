# Sekai

Free app to watch anime, Asian dramas and movies, and read manga — on Android, Windows, and Linux. No ads, no subscription.

This repository is Sekai's public home: it hosts the release builds and the small JSON feeds the app reads. The app's source code lives in a separate private repository.

## Download

Get the latest version from the [**Releases page**](https://github.com/ozipoetra/SekaiApp/releases/latest):

- **Android** — `sekai-v*-arm64.apk` (most phones & TVs), `sekai-v*-armv7.apk` (older 32-bit devices), `sekai-v*-x86_64.apk` (emulators / x86 TVs). Android 8.0 or newer.
- **Windows** — `sekai-v*-windows-x64.msi`
- **Linux** — `sekai-v*-linux-x64.deb` / `.rpm`

## Feeds in this repo

Both files are updated by the **Publish release** workflow — please don't edit them by hand.

- [`animesekai.json`](https://raw.githubusercontent.com/ozipoetra/SekaiApp/main/animesekai.json) — the update feed. The app checks it on launch; when it names a newer version, the app offers the update and links to the release page.
- [`sponsor.json`](https://raw.githubusercontent.com/ozipoetra/SekaiApp/main/sponsor.json) — the in-app support prompt (title, message, and link buttons).
