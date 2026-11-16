# data

Public metadata files read by ozipoetra's apps at runtime.

## animesekai.json — Sekai update feed

Sekai (the anime streaming app) fetches this file on launch:

```
https://raw.githubusercontent.com/ozipoetra/data/main/animesekai.json
```

When `version` here is newer than the installed app's version, the app
shows an update popup with the changelog and offers an in-app download
of `apkUrl` (Android), followed by the system installer.

### Fields

| Field | Meaning |
| --- | --- |
| `version` | Latest release version, `x.y.z`. The popup appears when this is greater than the installed version. |
| `versionCode` | Android versionCode of that release. Must increase with every release, together with `version` in the app's `tauri.conf.json`. |
| `apkUrl` | Direct download URL of the APK (a release asset in this repo). |
| `notesUrl` | Release page URL (fallback / "what's new" link). |
| `mandatory` | `true` hides the "Later" button in the popup. |
| `publishedAt` | Release date, `YYYY-MM-DD`. |
| `changelog` | Array of lines, shown as the popup's bullet list. |

### Publishing a release

1. In the `sekai` repo: bump `version` in `src-tauri/tauri.conf.json`
   AND `bundle.android.versionCode` (+1), commit, push, wait for CI.
2. Download the `sekai-arm64-release` artifact (the universal APK).
3. Here in `data`: create a release tagged `v<version>` and attach the
   APK as an asset (keep the asset name stable, e.g.
   `sekai-v<version>.apk`).
4. Update `animesekai.json`: new `version`, `versionCode`, `apkUrl`
   (pointing at the new asset), `notesUrl`, `publishedAt`, `changelog`.
5. Commit. Installed apps pick the update up on their next launch.
