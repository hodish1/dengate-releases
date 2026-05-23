# dengate-releases

Public update server for the [Dengate](https://github.com/hodish1/co-op-buddy) desktop app (Tauri v2).

## How it works

Every time a new desktop version is released, the CI pipeline in `co-op-buddy` builds the app on all platforms and pushes `latest.json` to this repo.

The shipped app fetches this file on every launch:

```
https://raw.githubusercontent.com/hodish1/dengate-releases/main/latest.json
```

If the version in `latest.json` is newer than the installed version, the app shows a native update dialog and installs automatically.

## `latest.json` structure

```json
{
  "version": "1.0.0",
  "notes": "Release notes here.",
  "pub_date": "2025-01-01T00:00:00Z",
  "platforms": {
    "darwin-aarch64": {
      "url": "https://github.com/hodish1/co-op-buddy/releases/download/desktop-v1.0.0/Dengate_aarch64.app.tar.gz",
      "signature": "<signature>"
    },
    "darwin-x86_64": {
      "url": "https://github.com/hodish1/co-op-buddy/releases/download/desktop-v1.0.0/Dengate_x64.app.tar.gz",
      "signature": "<signature>"
    },
    "linux-x86_64": {
      "url": "https://github.com/hodish1/co-op-buddy/releases/download/desktop-v1.0.0/Dengate_amd64.AppImage.tar.gz",
      "signature": "<signature>"
    },
    "windows-x86_64": {
      "url": "https://github.com/hodish1/co-op-buddy/releases/download/desktop-v1.0.0/Dengate_x64-setup.nsis.zip",
      "signature": "<signature>"
    }
  }
}
```

## Release trigger

Push a `desktop-v*` tag to `co-op-buddy` to kick off a release build:

```bash
git tag desktop-v1.2.3
git push origin desktop-v1.2.3
```

The CI workflow builds all platform targets, signs the artifacts, and commits the updated `latest.json` here.

## Why a separate repo

Keeps release artifacts and the update manifest isolated from source history. `co-op-buddy` holds the code and CI; this repo is the public-facing update endpoint the shipped app talks to. Without this repo being public, the auto-update feature has nowhere to check and silently does nothing.
