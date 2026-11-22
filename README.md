# Sekai — Free Anime & Asian Drama Streaming App

**Sekai** is a free streaming app to **watch anime online** and enjoy **Asian dramas and movies** — Korean dramas, Chinese dramas, Japanese dramas, and more — on **Android, Windows, and Linux**. No ads, no account, no subscription: open the app, pick a title, and watch.

Sekai brings two catalogs into one app:

- **Kuronime** — anime series and movies, latest episode updates, completed series, top rated titles, and a weekly release schedule.
- **Oppa** — Korean, Chinese, Japanese, Thai, and Western dramas and movies.

Each catalog stays separate and can be switched at any time from the sidebar.

## Features

- **Home rails for everything** — Latest Update, Movies, dramas by country, Completed, Top Rated, and the weekly **release schedule** with estimated release times.
- **Custom video player** — resume where you left off, a quality picker with real codec information, double-tap to seek, auto-next episode, playback speed, and picture-in-picture on Android.
- **Offline downloads** — download episodes in the background (they keep going with the screen off) and watch without a connection. Interrupted downloads resume, and expired links are refreshed automatically.
- **TV friendly** — full navigation with a **TV remote, gamepad, or keyboard** on every page, including the player.
- **Watch history & favorites** — continue watching across both catalogs, with progress saved per episode.
- **Two languages** — English and Bahasa Indonesia interface.
- **Lightweight** — built with Tauri and Rust; the catalog loads fast and the app stays small.

## Download

Get the latest version from the [**Releases page**](https://github.com/ozipoetra/SekaiApp/releases/latest):

| Platform | File |
| --- | --- |
| Android (most phones & TVs) | `sekai-v*-arm64.apk` |
| Android (older 32-bit devices) | `sekai-v*-armv7.apk` |
| Android (x86_64 emulators / TVs) | `sekai-v*-x86_64.apk` |
| Windows | `sekai-v*-windows-x64.msi` |
| Linux | `sekai-v*-linux-x64.deb` / `.rpm` |

Android 8.0 (API 26) or newer is required. The app checks this repository for updates and lets you know when a new version is out.

## Nonton Anime & Drama Gratis

Sekai adalah aplikasi streaming gratis untuk **nonton anime** dan **drama Asia** — drama Korea, drama China, drama Jepang, sampai film — di Android, Windows, dan Linux. Tanpa iklan dan tanpa langganan. Unduh episode untuk ditonton offline, lanjutkan tontonan terakhir, dan navigasi penuh memakai remote TV. Antarmuka tersedia dalam Bahasa Indonesia dan Inggris.

## Support Sekai

Sekai is free and ad-free, built and maintained by one person ([@ozipoetra](https://github.com/ozipoetra)). If it makes your watch time better, consider supporting its development — the app itself shows the support options, or reach out through the developer's GitHub profile.

## Tech

Tauri 2 · Rust · React · TypeScript · Tailwind CSS

---

## Maintainer notes — the feeds in this repo

This repository is Sekai's public home: it hosts the release builds (the app's source code lives in a separate private repository) and the small JSON feeds the app reads. Both feeds are updated by the **Publish release** workflow in this repo — please don't edit them by hand.

### animesekai.json — Sekai update feed

Sekai fetches this file on launch:

```
https://raw.githubusercontent.com/ozipoetra/SekaiApp/main/animesekai.json
```

When `version` here is newer than the installed app's version, the app shows an update popup with the changelog; its button opens the release page (`notesUrl`) in the browser, where the APK is downloaded and installed the normal way. The app deliberately never downloads or installs its own update — Play Protect flags apps that do.

| Field | Meaning |
| --- | --- |
| `version` | Latest release version, `x.y.z`. The popup appears when this is greater than the installed version. |
| `versionCode` | Android versionCode of that release. Tauri derives it from `version` (major×1000000 + minor×1000 + patch), so it rises automatically with every version bump — never set it by hand. |
| `notesUrl` | The release page URL — the one link the update flow uses. The popup's button opens this page, which lists every downloadable file. |
| `mandatory` | `true` hides the "Later" button in the popup — the update cannot be postponed. |
| `publishedAt` | Release date, `YYYY-MM-DD`. |
| `changelog` | Array of lines, shown as the popup's bullet list. |

Publishing a release: in the private `sekai` repo, bump `version` in `src-tauri/tauri.conf.json` (the versionCode derives automatically), commit and push, then run the **Publish release** workflow here (Actions tab, manual dispatch; changelog lines one per line, and the mandatory flag). It builds every platform (Android arm64/armv7/x86_64 APKs, Windows x64 MSI, Linux deb + rpm), creates one release here with all assets attached, and updates `animesekai.json`.

### sponsor.json — Sekai support prompt

Sekai fetches this file on launch:

```
https://raw.githubusercontent.com/ozipoetra/SekaiApp/main/sponsor.json
```

When the prompt is due, the app shows a support modal (title, message and link buttons from this file). It never appears while an update popup is offered — updates always win — and the About tab's Support button can open it on demand. Edit this file to change the copy or links; no app release is needed.

| Field | Meaning |
| --- | --- |
| `enabled` | Master switch. `false` = the prompt never auto-shows. |
| `id` | Campaign id. Dismissals are remembered per id — bump this to show the prompt again to everyone. |
| `title` | Modal headline. |
| `message` | Modal body text. |
| `buttons` | Array of `{label, url}` (https only). The first button is styled primary. |
| `cooldownDays` | Days after a dismissal before the prompt may show again for the same `id`. |
