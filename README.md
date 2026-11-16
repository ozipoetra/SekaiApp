# data

Public metadata files

## animesekai.json — Sekai update feed

Sekai (the anime streaming app) fetches this file on launch:

```
https://raw.githubusercontent.com/ozipoetra/data/main/animesekai.json
```

When `version` here is newer than the installed app's version, the app
shows an update popup with the changelog; its button opens the
release page (`notesUrl`) in the browser, where the APK is downloaded
and installed the normal way. The app deliberately never downloads or
installs its own update — Play Protect flags apps that do.

### Fields

| Field | Meaning |
| --- | --- |
| `version` | Latest release version, `x.y.z`. The popup appears when this is greater than the installed version. |
| `versionCode` | Android versionCode of that release. Tauri derives it from `version` (major*1000000 + minor*1000 + patch), so it rises automatically with every version bump — never set it by hand. |
| `apkUrl` | Direct download URL of the Android universal APK (arm64 + armv7 in one file; a release asset in this repo). |
| `windowsUrl` | Direct download URL of the Windows x64 installer (.exe). |
| `linuxDebUrl` / `linuxRpmUrl` | Direct download URLs of the Linux x64 packages. |
| `notesUrl` | Release page URL (fallback / "what's new" link). |
| `mandatory` | `true` hides the "Later" button in the popup. |
| `publishedAt` | Release date, `YYYY-MM-DD`. |
| `changelog` | Array of lines, shown as the popup's bullet list. |

### Publishing a release

Automated (the usual way): in the `sekai` repo, bump `version` in
`src-tauri/tauri.conf.json` (the versionCode is derived from it
automatically), commit and push, then run the **Publish release** workflow
(Actions tab, manual dispatch; fill in the changelog lines, one per
line). It builds every platform (Android universal APK, Windows x64,
Linux deb + rpm), creates ONE release here with all five assets
attached (`sekai-v<version>.apk`, `-windows-x64-setup.exe`,
`-windows-x64.msi`, `-linux-x64.deb`, `-linux-x64.rpm`), and updates
`animesekai.json` — no manual steps below needed. (It needs the `DATA_REPO_TOKEN` secret in the sekai repo: a
fine-grained PAT with access to this repo, Contents: Read and write.)

Manual fallback:

1. Download the `sekai-arm64-release` artifact from the sekai repo's
   CI (the universal APK).
2. Here in `data`: create a release tagged `v<version>` and attach
   the APK as an asset (keep the asset name stable, e.g.
   `sekai-v<version>.apk`).
3. Update `animesekai.json`: new `version`, `versionCode`, `apkUrl`
   (pointing at the new asset), `notesUrl`, `publishedAt`, `changelog`.
4. Commit. Installed apps pick the update up on their next launch.
