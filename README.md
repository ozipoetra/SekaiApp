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
| `versionCode` | Android versionCode of that release. Tauri derives it from `version` (major*1000000 + minor*1000 + patch), so it rises automatically with every version bump — never set it by hand. |
| `apkUrl` | Direct download URL of the APK (a release asset in this repo). |
| `notesUrl` | Release page URL (fallback / "what's new" link). |
| `mandatory` | `true` hides the "Later" button in the popup. |
| `publishedAt` | Release date, `YYYY-MM-DD`. |
| `changelog` | Array of lines, shown as the popup's bullet list. |

### Publishing a release

Automated (the usual way): in the `sekai` repo, bump `version` in
`src-tauri/tauri.conf.json` (the versionCode is derived from it
automatically), commit and push, then run the **Publish Android release** workflow
(Actions tab, manual dispatch; fill in the changelog lines, one per
line). It builds the signed APK, creates the release here with the
APK attached, and updates `animesekai.json` — no manual steps below
needed. (It needs the `DATA_REPO_TOKEN` secret in the sekai repo: a
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
