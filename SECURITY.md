# Security Policy — GlassWeave

## Reporting a Vulnerability
Report security vulnerabilities privately via GitHub Security Advisories on `https://github.com/raydeoliveira/glassweave`.

## Security & Privacy Model
- **Zero external dependencies**: Built strictly against system frameworks (`Metal`, `IOKit`, `AVFoundation`, `Vision`, `CoreVideo`, `ImageIO`).
- **Camera privacy**: Checks `AVCaptureDevice` camera authorization before reading UVC eye-tracking streams and fails closed with exit `4` when denied.
- **Lens rollback safety**: DDC/CI and USB-HID 3D lens activations enforce pre-change state snapshots and non-restarting `sigaction` auto-revert back to `2d`.
