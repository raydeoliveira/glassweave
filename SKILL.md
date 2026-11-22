---
name: glassweave
description: >-
  Cross-OS eye-tracked glasses-free 3D CLI and MCP server for Samsung Odyssey 3D,
  Acer SpatialLabs, ASUS Spatial Vision, and Lenovo ThinkVision 27 3D. Use when
  probing 3D monitors, switching 2D/3D lenticular lenses over DDC/CI or USB-HID with
  15s auto-revert, reading UVC eye-tracking coordinates, calibrating optical profiles,
  or rendering eye-tracked lenticular/interlaced/SBS/TAB/anaglyph frames.
---

# Skill: GlassWeave (`glassweave`)

## Install & Discovery
```bash
brew install raydeoliveira/tap/glassweave
glassweave --describe
```

## Safe Lens Switch Workflow
1. Probe displays and cameras (read-only):
   ```bash
   glassweave probe --json
   glassweave vendors --json
   ```
2. Dry-run lens switch before touching hardware or snapshots:
   ```bash
   glassweave switch --mode 3d --vendor samsung --dry-run --json
   ```
3. Switch lens with atomic snapshot and 15s auto-revert watchdog (never pass `--yes` blindly on an untested physical monitor):
   ```bash
   glassweave switch --mode 3d --vendor samsung --confirm-timeout 15 --json
   ```
4. Restore 2D mode deterministically at any time (always free):
   ```bash
   glassweave restore --vendor samsung --json
   ```

## Eye Tracking, Calibration & Lenticular Weaving
- Calibrate optical profile (`optimal_view_mm`, `lens_pitch`, `lens_slant`, `lens_phase`) and pipe into `weave`:
  ```bash
  glassweave calibrate --vendor samsung --json \
    | glassweave weave --input frame_sbs.png --out-layout lenticular --eye-coords 0,0,650,63,0,650 --output woven.png --from-json - --json
  ```
- Stream deterministic eye-tracking trace (`sweep`, `lost-face`, `center`) or UVC camera samples:
  ```bash
  glassweave track --fixture sweep --stream
  ```

## Native MCP Mode
Run `glassweave mcp` over stdio (JSON-RPC 2.0, `protocolVersion: 2024-11-05`) to invoke `probe_lenticular_displays`, `list_vendor_profiles`, `solve_lenticular_calibration`, `switch_lens_mode` (`dry_run: true` default), and `weave_lenticular_frame` (`dry_run: true` default).

## Exit Codes
- `0` ok | `1` runtime/rollback | `2` invalid usage/validation/upstream error | `3` hardware unavailable | `4` permission/TCC/symlink denied | `5` Pro license required (`glassweave license activate --key <KEY>`) | `130` `SIGINT`/`SIGTERM` after automatic `2d` rollback
