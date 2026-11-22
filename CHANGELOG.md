# Changelog

All notable changes to GlassWeave are documented in this file.

## [0.1.0] - 2026-11-21

### Added
- Clean-room 4-vendor glasses-free 3D descriptor catalog (Samsung Odyssey 3D, Acer SpatialLabs, ASUS Spatial Vision, Lenovo ThinkVision 27 3D) with `<config-dir>/vendors.json` override support.
- VESA DDC/CI I2C codec (`0x37` / `0x6E` / `0x6F` / `0x51` / `0x50` XOR) + isolated `SPIAdapter` (`IOAVService*` `dlsym`), macOS `IOHIDDevice`, and `#if os(Linux)` `/dev/i2c-*` + `/dev/hidraw*` via `CGlassWeaveLinux`.
- Non-TTY `--yes` guard, `lstat` symlink-guarded atomic snapshots, readback verification, 15s `sigaction` (`sa_flags = 0`) auto-revert to `2d`, and free `restore` subcommand.
- `AVFoundation` + `Vision` UVC eye tracker, `--eye-coords` JSON/CSV/JSONL, `GLASSWEAVE_UVC_TRACE`, and `lost-face` hold/fade state machine.
- Runtime-compiled Metal slanted subpixel lenticular shader with dynamic eye-tracking phase shift, `shaders/lenticular.glsl`, `shaders/lenticular.wgsl`, CPU `LenticularReferenceKernel` parity, and `calibrate` sweep.
- Dual CLI (`--json`, `--describe`, `--dry-run`, `--from-json -`), stdio JSON-RPC 2.0 `glassweave mcp`, Polar.sh licensing + `-DOSS_EDITION` compile seam, and `scripts/package_release.sh`.
