# Source Visibility And Distribution

This repository is organized as a release repository rather than a public development repository.

## What Is Public Here

- Browser-ready Chrome and Opera GX ZIP files.
- Checksums for those archives.
- Installation, usage, privacy, troubleshooting, architecture, and implementation documentation.
- Issue templates, security guidance, contribution guidance, and release validation.

## What Is Not Public Here

- The private development workspace.
- Internal design drafts and test previews.
- The owner's private build-and-learning manual.
- Browseable copies of the unpacked package tree.

## Important Technical Reality

A Chromium extension cannot keep its runtime implementation secret from the person running it. The browser must receive the manifest, JavaScript, HTML, CSS, images, and related files. Those files can be inspected after a ZIP is extracted or an extension is installed.

Putting only ZIP files in this repository reduces casual browsing and keeps GitHub focused on downloads and documentation. It does not provide encryption, digital-rights management, or meaningful source-code secrecy.

Minification or obfuscation could make casual reading less convenient, but it would not prevent inspection and can make security review, debugging, store review, and maintenance harder. Murmur v0.7.0 is distributed as a normal inspectable browser extension package.

## License Effect

The included MIT License grants broad rights to copies of the software. Distribution goals and licensing goals are separate decisions. A future owner can choose a different license for future versions if they have the necessary rights, but already distributed copies remain governed by the license included with them.

This document is a technical description, not legal advice.

## Recommended Publishing Pattern

1. Create a new GitHub repository from this folder only.
2. Keep the private development workspace outside that repository.
3. Commit documentation and checksums.
4. Keep the ZIPs in `releases/` for a simple clone-and-download experience, or attach the same ZIPs to a GitHub Release.
5. Tag the commit `v0.7.0`.
6. Copy the release notes from [Releases](RELEASES.md) into the GitHub Release description.
7. Verify uploaded asset hashes after GitHub finishes processing the release.

Never add the owner's private manual, private keys, browser signing keys, store credentials, or development secrets to the public repository.

