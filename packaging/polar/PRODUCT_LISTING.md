# Polar.sh Product Listing — GlassWeave Pro (macOS & Linux/SteamOS)

> Use this copy when creating the GlassWeave Pro product in your Polar.sh dashboard (`polar.sh`).
> Enable Polar's **License Keys** benefit (prefix: `GW3D`, activation limit: `3` devices per buyer, expiration: perpetual).

---

## Product Title
**GlassWeave Pro — Eye-Tracked Glasses-Free 3D Lens Switcher, UVC Tracker & Lenticular Weaver**

## Price & Billing Model
- **$29 USD** — One-time purchase (Perpetual License, v0.x updates included, up to 3 devices).
- **Free Core Tier** included in the `869,584`-byte (`849 KB`) native binary: `probe`, `vendors`, `calibrate`, `restore`, `track`, `--dry-run`, `weave --out-layout dubois-anaglyph`, and read-only MCP tools.

## Tagline
Control 2D/3D lenticular lenses over DDC/CI and USB-HID with 15s auto-revert, track viewer pupils over UVC, and weave eye-tracked lenticular frames on Metal, GLSL 450, and WGSL from one `849 KB` native CLI + MCP server.

## Storefront Description

Glasses-free 3D displays (Samsung Odyssey 3D, Acer SpatialLabs, ASUS Spatial Vision, Lenovo ThinkVision 27 3D) normally lock lens switching and eye-tracked lenticular rendering behind heavy Windows-only vendor runtimes.

**GlassWeave** delivers clean-room DDC/CI and USB-HID lens switching, `AVFoundation` + `Vision` UVC eye tracking, and runtime-compiled Metal + GLSL 450 + WGSL lenticular shaders in a single zero-dependency `849 KB` binary.

### What Is Included in Pro ($29 Perpetual)
- **4-Vendor 2D/3D Lens Switcher (`switch`)**: VESA DDC/CI (`0x37`, `0x6E`/`0x6F`, `0x51`) and USB-HID lens switching with atomic symlink-guarded snapshots, readback verification, non-TTY `--yes` guard, and 15s `sigaction` auto-revert to `2d`.
- **Eye-Tracked GPU Lenticular Weaver (`weave`)**: Runtime-compiled Metal compute kernels for slanted RGB/BGR subpixel `lenticular` (with live $\Delta\phi$ eye-tracking phase compensation), `row-interlaced`, `col-interlaced`, `checkerboard`, `full-sbs`, `half-sbs`, `tab`, and `dubois-anaglyph`, plus `shaders/lenticular.glsl` and `shaders/lenticular.wgsl`.
- **UVC Eye Tracking & Calibration (`track`, `calibrate`)**: Live macOS `AVFoundation` + `Vision` pupil projection, `--eye-coords` JSON/CSV/JSONL, `GLASSWEAVE_UVC_TRACE` deterministic trace playback with lost-face hold/fade, and synthetic/frame optical sweep calibration.
- **Native AI-Agent MCP Server (`glassweave mcp`)**: Stdio JSON-RPC 2.0 (`2024-11-05`) with `--describe`, `--json`, `--from-json -` UNIX piping, and safe `dry_run: true` defaults.

### Requirements & Honest Limits
- **OS & Hardware**: macOS 14.0+ (`arm64`) with Metal GPU; `#if os(Linux)` adapters (`/dev/i2c-*`, `/dev/hidraw*`, V4L2) and GLSL/WGSL compute shaders included for Linux/SteamOS builds.
- **Vendor Firmware Note**: Proprietary 3D lens-toggle VCP/HID register codes vary across display firmware and are not publicly documented by display vendors. Built-in clean-room defaults can be overridden without recompiling via `~/.config/glassweave/vendors.json` (`--config-dir`). Test with `--dry-run` first.
- **Activation & Privacy**: Activates once via Polar.sh (`glassweave license activate --key <KEY>`), caches locally with `0600` permissions and 14-day offline grace, and makes zero background network calls.
