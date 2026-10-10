# About this fork

This is an unofficial, modified fork of **[LightCraft](https://github.com/storytold/lightcraft)**, the open-source,
clean-room, pure-Rust photo library and raw developer by the **[ArtCraft](https://getartcraft.com/) team and the
LightCraft contributors** (MIT OR Apache-2.0). Nearly all of the code here is theirs, and the full upstream commit
history, with its authors, is preserved. This fork is not made, sponsored or endorsed by them.

- Upstream project: https://github.com/storytold/lightcraft
- Forked at upstream commit `c435d14` (2026-10-08, v0.4.0)
- Licenses: unchanged, see [LICENSE-MIT](LICENSE-MIT), [LICENSE-APACHE](LICENSE-APACHE) and [NOTICE](NOTICE)

## Changes in this fork

1. **Two new built-in presets** in [crates/engine/src/presets.rs](crates/engine/src/presets.rs), with tests:
   - **Evening Glory** (Portrait): a backlit golden-hour look with warm amber highlights, soft low-contrast haze,
     matte lifted blacks and olive-shifted greens.
   - **Metallic** (Style): a steel-grey, gritty look. Yellows, greens, aquas and blues are pulled hard toward grey
     while reds and oranges keep skin and hair warm; blue-teal shadows, a faint cool tint in the highlights,
     strong clarity and texture, grain and a dark vignette. Tuned by eye against before/after examples.
   Both use original slider values and no third-party preset content, as the project's clean-room rules require.
2. **ArtCraft logos and marks removed** (the image files in `docs/brand/` and the README logo references), as the
   [brand terms](docs/brand/LICENSE-brand.txt) require for modified versions. Plain-text credit to the ArtCraft team
   remains in the README and NOTICE. The in-app Help menu still links to upstream's Discord and website as text.
3. **This file**, plus an unofficial-fork notice in the README and a "Modified version" section in NOTICE.

## Known issues (not fixed here)

- Two GPU AI-denoise tests fail on Apple Silicon / Metal (`nn::tests::the_gpu_gives_what_the_reference_gives`
  and `nn::tests::a_runner_serves_many_tiles_from_many_threads`), so `cargo xtask ci` stops at its test step.
  Seen on an M4 Pro; upstream tracks it as [#479](https://github.com/storytold/lightcraft/issues/479). The cause is
  a data race in `crates/gpu/src/wgsl/nn_conv.wgsl`: four threads write different components of one shared `vec4`.
  Upstream describes a fix (store the shared tile as scalars). Only the optional AI denoise path is affected.
- Chinese and Japanese text needs the optional `craft-fonts` build input (see the README).

## Building and testing

```sh
cargo build --release -p lightcraft -p lightcraft-cli
cargo run --release -p lightcraft -- --memory      # throwaway demo session
cargo test --workspace --no-fail-fast
```

Tested on macOS (Apple Silicon, Rust 1.98): release build OK; headless render and export OK; 1,888 of 1,890 tests
pass (the two failures above); fmt, clippy, layering, assets, parity and the wasm checks pass.
