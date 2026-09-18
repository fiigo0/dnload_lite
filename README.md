# Dnlod

A personal-use macOS app that downloads YouTube audio and video entirely on your
machine. **Status: v0.2.0 — self-contained .app** — paste a URL or search YouTube,
fetch metadata, download MP3 or MP4, and view lyrics. See
[`docs/PROJECT.md`](docs/PROJECT.md) for the full vision.

## Run from source

Requires macOS on Apple Silicon, Python 3.13, `ffmpeg` and `node` on `PATH`,
and a logged-in Chrome (for YouTube cookie auth).

```bash
brew install ffmpeg node          # ffmpeg for audio, node for yt-dlp's JS solver
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python dnlod.py
```

## Install the packaged app

Build a self-contained, double-clickable `Dnlod.app` — no Python, Node, or ffmpeg
needed on the target machine (they're bundled). **Build on an Apple Silicon Mac:**

```bash
brew install dylibbundler          # one-time build-machine tool
./build.sh                         # -> dist/Dnlod.app + dist/Dnlod-v0.2.0-macos-arm64.zip
```

**Install:** drag `Dnlod.app` to `/Applications` in Finder (it prompts for admin
if needed), or run `./install.sh` — which installs to `/Applications` when it's
writable and otherwise falls back to `~/Applications` (no sudo). A locally-built
app can also just be run in place from `dist/`.

**First launch:** the app is ad-hoc-signed but not notarized, so macOS may block
it. Clear it with **no terminal** via **System Settings → Privacy & Security →
Open Anyway** (a message appears there after the first blocked launch). `install.sh`
clears quarantine via `xattr` as a terminal alternative.

See [`docs/specs/packaging-and-distribution/spec.md`](docs/specs/packaging-and-distribution/spec.md)
and [`docs/adr/0002-packaging-py2app-bundled-deno.md`](docs/adr/0002-packaging-py2app-bundled-deno.md).

## Tests

Pure-helper unit tests (no third-party deps needed):

```bash
python3 -m unittest discover -s tests
```

## Dependencies

Runtime deps are pinned in [`requirements.lock`](requirements.lock) (SHA-256
hashes for every package). The build script installs from the lockfile
automatically. To update deps:

```bash
pip install pip-tools
pip-compile --generate-hashes -o requirements.lock requirements.txt
```
