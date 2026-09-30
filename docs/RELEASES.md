# Releases And Verification

## Current Release

Murmur v0.7.0 was prepared on September 29, 2026.

| Browser | File |
| --- | --- |
| Chrome 114+ | [`murmur-chrome-v0.7.0.zip`](../releases/murmur-chrome-v0.7.0.zip) |
| Opera GX | [`murmur-opera-gx-v0.7.0.zip`](../releases/murmur-opera-gx-v0.7.0.zip) |

Both archives place `manifest.json` at the archive root so the extracted folder can be selected directly with **Load unpacked**.

Opera GX has been manually exercised for the main workflow. Chrome shares the validated runtime and has automated manifest, syntax, parity, and regression coverage, but a full owner-run manual Chrome smoke test was still pending when this package was assembled.

## Verify On Windows PowerShell

From the repository root:

```powershell
Get-FileHash .\releases\murmur-chrome-v0.7.0.zip -Algorithm SHA256
Get-FileHash .\releases\murmur-opera-gx-v0.7.0.zip -Algorithm SHA256
Get-Content .\releases\SHA256SUMS.txt
```

The displayed hashes must exactly match the corresponding values in `SHA256SUMS.txt`.

## Verify On macOS Or Linux

From the repository root:

```bash
cd releases
sha256sum --check SHA256SUMS.txt
```

On macOS systems without `sha256sum`, use:

```bash
shasum -a 256 releases/murmur-chrome-v0.7.0.zip
shasum -a 256 releases/murmur-opera-gx-v0.7.0.zip
cat releases/SHA256SUMS.txt
```

## Inspect An Archive Before Installation

Confirm that the package contains a root manifest and the expected runtime groups:

```text
manifest.json
background.js
popup.html
sidepanel.html
assets/
content/
ui/
LICENSE
PRIVACY.md
```

The Chrome manifest should include `sidePanel` and a `side_panel` entry. The Opera GX manifest should not include either Chrome-only declaration.

## GitHub Release Notes For v0.7.0

Murmur v0.7.0 keeps lyrics beside YouTube without a detached Opera window. It adds Opera GX's closeable in-tab drawer, preserves Chrome's native side panel, follows new songs automatically, uses Genius as its single provider, renders extracted lyrics in Murmur's own interface, and removes confidence-based refusals in favor of the best valid result.

Important behavior:

- Install the package labeled for your browser.
- This is a Developer mode, unpacked-extension release.
- The reader obtains lyrics directly from Genius at use time.
- Matching is best effort. Verify unusual songs with the displayed source label.
- Existing YouTube tabs should be refreshed after installation or update.

## Maintainer Release Checklist

1. Update the version in both manifests and public documents.
2. Run private source validation.
3. Confirm shared-file parity between browser packages.
4. Smoke-test the popup and reader at normal and narrow widths.
5. Test at least one YouTube video, one playlist transition, and one YouTube Music transition.
6. Stage each browser package separately.
7. Add `LICENSE` and `PRIVACY.md` to each staged archive.
8. Create ZIPs with `manifest.json` at the root.
9. Generate fresh SHA-256 checksums.
10. Run the public release verification workflow locally or in GitHub Actions.
11. Commit the docs, ZIPs, and checksum file together.
12. Create and push a version tag.
13. Create a GitHub Release with the two ZIPs and checksum file.
14. Download the published assets once and verify their hashes.
