# casparcg-usm-builds

Patched **CasparCG Server** binaries bundled by the USM Boarding desktop installer.

- **Base:** [CasparCG/server](https://github.com/CasparCG/server) @
  `69e8ad552df7a38615f0581688bd861b09de8b94` (v2.5.0-stable) — unmodified except for the
  patches below, so every DLL beside `casparcg.exe` matches the stock distribution.
- **Patches** (in `patches/`, applied in filename order):
  1. `0001-osc-profiler-time` — emit `/channel/N/profiler/{time,produce,mix,consume}` over OSC
  2. `0002-still-upload-cache` — still-image GPU upload cache (two-sighting promote)
  3. `0003-screen-ingress-depth` — screen consumer ingress queue depth 1 → 2
  4. `0004-host-pool-prewarm` — async host-pool pre-warm on first miss
  5. `0005-amcp-batch-executor-race` — verbatim cherry-pick of upstream
     [`5e27ec1c2`](https://github.com/CasparCG/server/commit/5e27ec1c22b68ec4cfd8bcdf7b786608b589c103)
     (fixes [#1687](https://github.com/CasparCG/server/issues/1687): AMCP `BEGIN`/`COMMIT`
     batches silently dropped when the batch executor loses its thread-startup race) —
     first shipped in `casparcg-2.5.0-usm-r2`; releases up to `-r1` carry patches 1–4 only
- **Built by** the `casparcg-telemetry` workflow in the (private) app repo, on `windows-2022`,
  mirroring CasparCG's own Windows build at the pinned commit.

## Licence

CasparCG Server is distributed by SVT under the **GNU GPL v3 (or later)**. The complete
corresponding source for every binary release here is the pinned upstream commit plus the
patch files in this repository.

## Why binaries live HERE and not in the app repo

electron-updater (with `allowPrerelease`) treats the newest (pre)release of the app repo as
the app update feed — a non-app release there breaks fleet updates (learned 2026-07-12).
This repo exists so CasparCG assets can never collide with the updater. Do not move them back.
