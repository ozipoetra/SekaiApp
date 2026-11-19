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
| `notesUrl` | The release page URL — the one link the update flow uses. The popup's button opens this page, which lists every downloadable file. (The feed used to carry per-file download URLs; they were dropped to keep it simple.) |
| `mandatory` | `true` hides the "Later" button in the popup. |
| `publishedAt` | Release date, `YYYY-MM-DD`. |
| `changelog` | Array of lines, shown as the popup's bullet list. |

### Publishing a release

Automated (the usual way): in the `sekai` repo, bump `version` in
`src-tauri/tauri.conf.json` (the versionCode is derived from it
automatically), commit and push, then run the **Publish release** workflow
(Actions tab, manual dispatch; fill in the changelog lines, one per
line). It builds every platform (Android universal APK, Windows x64,
Linux deb + rpm), creates ONE release here with all four assets
attached (`sekai-v<version>.apk`, `-windows-x64.msi`,
`-linux-x64.deb`, `-linux-x64.rpm` — Windows ships the MSI only), and updates
`animesekai.json` — no manual steps below needed. (It needs the `DATA_REPO_TOKEN` secret in the sekai repo: a
fine-grained PAT with access to this repo, Contents: Read and write.)

Manual fallback:

1. Download the artifacts from the sekai repo's CI (the universal
   APK from `sekai-android-release`, the MSI from `sekai-windows`,
   the deb + rpm from `sekai-linux`).
2. Here in `data`: create a release tagged `v<version>` and attach
   the files as assets (keep the asset names stable, e.g.
   `sekai-v<version>.apk`).
3. Update `animesekai.json`: new `version`, `versionCode`,
   `notesUrl` (the release page), `publishedAt`, `changelog`.
4. Commit. Installed apps pick the update up on their next launch.

## sponsor.json — Sekai support prompt

Sekai fetches this file on launch:

```
https://raw.githubusercontent.com/ozipoetra/data/main/sponsor.json
```

When the prompt is due, the app shows a support modal (title, message
and link buttons from this file). It never appears while an update
popup is offered — updates always win — and the About tab's Support
button can open it on demand. Edit this file to change the copy or
links; no app release is needed.

### Fields

| Field | Meaning |
| --- | --- |
| `enabled` | Master switch. `false` = the prompt never auto-shows. |
| `id` | Campaign id. Dismissals are remembered per id — bump this to show the prompt again to everyone. |
| `title` | Modal headline. |
| `message` | Modal body text. |
| `buttons` | Array of `{label, url}` (https only). The first button is styled primary. Put real donation links here (Saweria, Ko-fi, GitHub Sponsors, ...) whenever they exist. |
| `cooldownDays` | Days after a dismissal before the prompt may show again for the same `id`. |
