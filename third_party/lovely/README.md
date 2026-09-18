# Lovely Injector (bundled)

`version.dll` for Windows, as well as `liblovely.dylib` and `run_lovely_macos.sh` for MacOS are the [Lovely Injector](https://github.com/ethangreen-dev/lovely-injector),
**v0.9.0**, from the upstream release. MIT licensed by its authors.

`liblovely.dylib` is a **universal (x86_64 + arm64)** binary, built by
`lipo`-ing the upstream `lovely-aarch64-apple-darwin.tar.gz` and
`lovely-x86_64-apple-darwin.tar.gz` slices together and re-signing ad hoc:

    lipo -create liblovely-arm64.dylib liblovely-x86_64.dylib -output liblovely.dylib
    codesign --force --sign - liblovely.dylib

Both slices are required. Steam for macOS runs as x86_64 under Rosetta and
launches Balatro translated, so an arm64-only injector makes dyld abort the
game at startup ("love quit unexpectedly") with an incompatible-architecture
error. Shipping arm64 alone works only when the game is started from a
native-arm64 parent, such as a terminal.

It is bundled so `scripts/deploy.ps1` can install the mod with no downloads.
The script copies this file next to `Balatro.exe`; the game's launcher then
loads Lovely, which applies the patches in `src/lovely.toml`.

To bump the version: download the new release zip from upstream, replace this
`version.dll`, update the version number above, and commit.
