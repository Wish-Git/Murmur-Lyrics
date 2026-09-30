# Security Policy

## Supported Version

| Version | Supported |
| --- | --- |
| 0.7.x | Yes |
| Earlier versions | No |

Users should install the newest package and verify its SHA-256 checksum.

## Report A Vulnerability

Use GitHub's private vulnerability-reporting or security-advisory feature for this repository. Do not open a public issue for a vulnerability that could expose users.

Include:

- A concise description and impact.
- Affected browser and Murmur version.
- Reproduction steps or a minimal proof of concept.
- Whether user interaction is required.
- Any suggested mitigation.

Do not include real credentials, cookies, private user data, or destructive payloads.

## Security Model

Murmur is a local browser extension, not a hosted service. Its main trust boundaries are:

- YouTube page metadata entering the extension through content scripts.
- Genius search and lyrics responses entering the local reader.
- Messages crossing between page-attached content scripts, the service worker, and extension pages.
- A web-accessible local reader loaded into Murmur's own YouTube drawer.

The release validates Genius HTTPS URLs, restricts accepted hosts and lyrics paths, parses remote HTML without executing it, skips hidden and non-lyrics elements, and renders extracted content through text-only DOM assignments.

## Out Of Scope

- General vulnerabilities in Chrome, Opera GX, YouTube, or Genius that do not depend on Murmur.
- Wrong lyrics caused only by ambiguous public metadata, unless the mismatch reveals a security boundary failure.
- Issues in modified or repackaged builds whose checksum differs from the official release.
- Social engineering that does not exploit Murmur behavior.

