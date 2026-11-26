# GlassWeave

> **Product Hub:** [raymonddeoliveira.com/software](https://www.raymonddeoliveira.com/software#glassweave) · **Buy GlassWeave Pro ($29):** [Polar Checkout](https://buy.polar.sh/polar_cl_RVXA4XqANyC7aXm12WybluTFQ7u2cxvoClgln2MWxuk) · **Complete 4-CLI Bundle ($49, save $57):** [Bundle Checkout](https://buy.polar.sh/polar_cl_KTtCY3iQC4bzjqp93aC6LCog8UZqzZzuDgkAs3BvF74) · **Verified MCP:** [Glama](https://glama.ai/mcp/servers/raydeoliveira/glassweave) · **Free Core:** `brew tap raydeoliveira/tap && brew install glassweave`

Cross-OS eye-tracked glasses-free 3D CLI and stdio MCP server for Samsung Odyssey 3D, Acer SpatialLabs, ASUS Spatial Vision, and Lenovo ThinkVision 27 3D (`869,584` bytes / `849 KB` stripped macOS `arm64` binary, zero external runtime dependencies).

## What It Does and Does Not Do

| Capability | Status | Notes / Requirements |
|---|---|---|
| 4-vendor optical table (`vendors`) | Supported | Samsung Odyssey 3D, Acer SpatialLabs, ASUS Spatial Vision, Lenovo ThinkVision 27 3D + `<config-dir>/vendors.json` overrides |
| DDC/CI & USB-HID lens switching (`switch`, `restore`) | Supported | VESA DDC/CI (`0x37`, `0x6E`/`0x6F`, `0x51`, `0x50` XOR) via isolated `IOAVService*` SPI (`dlsym`) + `IOHIDDevice`; `#if os(Linux)` `/dev/i2c-*` & `/dev/hidraw*` via `CGlassWeaveLinux` |
| UVC eye tracking (`track`) | Supported | macOS `AVFoundation` + `Vision` `VNDetectFaceLandmarksRequest` (requires Camera TCC permission); `#if os(Linux)` V4L2 probe; `--eye-coords`, stdin JSONL, `GLASSWEAVE_UVC_TRACE`, `--fixture` |
| Eye-tracked lenticular shaders (`weave`, `calibrate`) | Supported | Runtime-compiled Metal MSL (`lenticular`, `row-interlaced`, `col-interlaced`, `checkerboard`, `full-sbs`, `half-sbs`, `tab`, `dubois-anaglyph`), `shaders/lenticular.glsl`, `shaders/lenticular.wgsl`, CPU parity oracle |
| Proprietary vendor firmware opcodes | Honest limit | Vendor 3D lens-toggle VCP/HID registers are undocumented publicly; clean-room defaults are provided and overridable via `--config-dir <dir>/vendors.json`. Always test with `--dry-run` first |

## Install & Uninstall

The Homebrew package runs on Apple Silicon with macOS Sonoma (14) or later. Inspect the simulated Samsung Odyssey 3D display and camera.

```sh
brew install raydeoliveira/tap/glassweave
glassweave probe --fixture samsung-odyssey-3d --json
```

The release page provides the macOS archive, MCP bundle and SHA-256 checksums. [Version 0.1.0 downloads](https://github.com/raydeoliveira/glassweave/releases/tag/v0.1.0)

### Uninstall

```bash
brew uninstall glassweave
rm -rf ~/.config/glassweave
```

## Quick Start (Human + `--json` Agent Usage)

With GlassWeave installed, add the [agent instructions](SKILL.md) to your project for display profile lookup and lens-switch previews.

```sh
npx skills add raydeoliveira/glassweave
```

```bash
$ glassweave --version
glassweave 0.1.0 (commercial, arm64)

$ glassweave probe --fixture samsung-odyssey-3d --json
{"data":{"cameras":[{"id":"sim-uvc-0","is_external":true,"name":"Simulated UVC Eye Tracker (fixture:samsung-odyssey-3d)"}],"display_identity":{"manufacturer_code":"samsung","monitor_name":"Odyssey 3D G90XF","product_id_hex":"7412","vendor_id_hex":"4c2d"},"displays":[{"lens_mode":"2d","model":"Odyssey 3D G90XF","product_id_hex":"7412","vendor":"samsung","vendor_id_hex":"4c2d"}]},"error":null,"ok":true,"schema_version":1}

$ glassweave switch --mode 3d --fixture samsung --snapshot-dir /tmp/gw-snap --dry-run --json
{"data":{"applied":false,"confirmed":false,"display":{"lens_mode":"2d","model":"Odyssey 3D G90XF","product_id_hex":"7412","vendor":"samsung","vendor_id_hex":"4c2d"},"dry_run":true,"interrupted":false,"note":"Dry-run plan for Odyssey 3D G90XF: 2d -> 3d via ddc.","planned_frame_hex":"51 84 03 ea 00 01 53","previous_mode":"2d","rolled_back":false,"snapshot_path":"/tmp/gw-snap/lens_snapshot_4c2d_7412.json","target_mode":"3d","transport":"ddc","verified":false},"error":null,"ok":true,"schema_version":1}

# UNIX composability: pipe calibrate optical_profile into eye-tracked weave
$ glassweave calibrate --vendor samsung --json \
  | glassweave weave --input frame_sbs.png --output woven.png --eye-coords 0,0,650,63,0,650 --dry-run --from-json - --json
```

## Safety & Rollback

- **Pre-switch snapshot**: Persists `lens_snapshot_<vid>_<pid>.json` atomically before any DDC/CI or USB-HID write, rejecting symbolic links via `lstat` (exit `4`).
- **Non-TTY protection**: Refuses mutating `switch` calls in non-interactive pipelines without `--yes` (exit `2`).
- **15s auto-revert & signals**: Installs non-restarting `sigaction` (`sa_flags = 0`) on `SIGINT` and `SIGTERM` during `poll()`; unconfirmed 3D mode reverts to `2d` on timeout (exit `1`) or signal (exit `130`).
- **Explicit `restore`**: `glassweave restore --vendor <name>` reverts to `2d` without requiring a Pro license.
- **OS Permissions**: USB-HID requires macOS Input Monitoring; live UVC tracking requires Camera permission (`System Settings -> Privacy & Security`). Denied permissions return exit `4`.

## Exit Codes & Discovery

- `0`: Success
- `1`: Runtime failure or confirmation timeout rollback
- `2`: Invalid usage, NaN/Inf/bounds error, non-TTY without `--yes`, or upstream `--from-json` `ok: false`
- `3`: Supported 3D display or UVC camera unavailable (default when no physical hardware is attached and no `--fixture`/`--sim` is given)
- `4`: OS TCC permission denied or symlink rejected
- `130`: `SIGINT`/`SIGTERM` received after automatic rollback to `2d`

Run `glassweave --help` for human CLI usage, `glassweave --describe` for the JSON capability manifest, or `glassweave mcp` for stdio JSON-RPC 2.0 MCP tools.

## License & Security

- Security policy: [SECURITY.md](SECURITY.md)
