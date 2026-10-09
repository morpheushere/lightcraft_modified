# Local review notes (fork baseline)

Baseline: upstream `main` at c435d14 (2026-10-08), v0.4.0, MIT OR Apache-2.0.

## Architecture in one glance
geom/color/raster/tiff -> raw/codecs/meta/develop -> pipeline (+gpu, preview, merge) -> catalog -> engine -> ui-egui / mcp -> apps.
Everything is a command (`Session::execute`); edits are plain data (`DevelopSettings`).

## Known gaps (upstream's own assessment)
- No measured camera colour calibration (RAW colour is estimated from embedded JPEGs)
- CR3 partly verified; compressed ORF, some NEF, sRAW are preview-only
- AI masks/denoise limited; 100% zoom capped by preview size; no HDR/video/Print/Book/Map
- docs/parity.md "Top gaps" has duplicated entries (lines 62-67)

## Test plan
1. cargo build --release -p lightcraft -p lightcraft-cli
2. cargo run --release -p lightcraft -- --memory   (throwaway demo session)
3. lightcraft-cli render <jpg> -o out.jpg --set light.exposure=0.5
4. cargo test --workspace ; cargo xtask ci (needs wasm32-unknown-unknown target)
5. Try real JPEG / DNG / own-camera RAW on a COPY of a folder

## Candidate directions
Camera colour calibration, visible-region 100% zoom, AI masks, Print/Map module, custom MCP/agent workflows.
