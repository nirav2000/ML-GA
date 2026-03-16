# Version Archive System (Reusable Guide)

This document defines a reusable pattern for shipping one current app plus archived app versions, while always using a single up-to-date version history file.

## Goals
- Keep one authoritative history file: `MLGA101.versions.json` at repository root.
- Allow any loaded app version (current or archived) to move backward/forward to other versions.
- Avoid stale history data by cache-busting the history fetch.

## Directory pattern
- Current app: `MLGA101.html`
- History file: `MLGA101.versions.json`
- Archived app files: `versions/<version>/MLGA101.html`

## History file contract
Each version entry should include:
- `version` (semantic string, e.g. `1.4.0`)
- `label`
- `appFile` (path from repo root, e.g. `versions/1.2.0/MLGA101.html`)
- `config` (optional runtime defaults)
- `changes` (string array)

## Required runtime helpers in every app file
Every app file (including archived ones) should implement:
- `getRootPathPrefix()` to detect the repo root even when current path is inside `/versions/...`
- `getRootUrl()` to build root-based URLs
- `withBust(url)` to append a timestamp query
- `getCurrentAppFile()` to compare current file vs selected `appFile`

## Fetching the history file
Always fetch root history file with no-store + cache-bust:

```js
fetch(withBust(`${getRootUrl()}MLGA101.versions.json`), { cache: 'no-store' })
```

## Version switching behavior
On changelog version click:
1. Read `entry.appFile` from history JSON.
2. If `entry.appFile` differs from current app file, navigate to that file with `withBust(...)`.
3. If same file, optionally apply profile config in place.

## Special handling for legacy versions
If an old archived build has no version footer/modal:
- add a lightweight footer and changelog modal UI
- wire it to the same root history file and `appFile` navigation logic

## Release checklist
1. Increment semantic version in current app footer and `APP_VERSION`.
2. Add top history entry in `MLGA101.versions.json` and update `latest`.
3. Ensure all archived files still fetch the root history file.
4. If creating a new archive, copy exact file from git tag/commit for accuracy.
5. Smoke test switching from current and at least one archived page.
