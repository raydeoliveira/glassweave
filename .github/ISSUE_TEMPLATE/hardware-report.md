---
name: Hardware Compatibility Report
about: Share test results for glassweave on your physical hardware
title: "[HW] <device model> on <OS>"
labels: hardware-report
---

## Hardware Setup
- **OS & Architecture**: (e.g., macOS 15.x arm64, Ubuntu 24.04 x86_64)
- **Hardware Model / Interface**: (Glasses-free 3D display make/model (e.g., Odyssey 3D, SpatialLabs), UVC camera, and DDC/CI or USB-HID interface)
- **`glassweave --version` output**:

## `probe` Output
```json
// paste output of `glassweave probe --json` (redact any serial numbers you do not wish to share)
```

## What Worked / Quirks
- What commands succeeded cleanly?
- Did `--dry-run` or timed auto-revert / restore behave as expected?
- Any timeouts, firmware quirks, or unexpected VID/PIDs?
