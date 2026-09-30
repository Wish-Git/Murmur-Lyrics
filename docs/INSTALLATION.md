# Installation

Murmur v0.7.0 is distributed as separate unpacked-extension packages for Chrome and Opera GX. Choose the package made for your browser.

## Before You Begin

- Use Google Chrome 114 or newer, or a current Opera GX release.
- Keep the extracted Murmur folder in a permanent location. The browser loads the extension from that folder.
- Do not select the ZIP file when the browser asks for an extension folder.
- If you obtained the package from somewhere else, verify its SHA-256 checksum against [the published checksum list](../releases/SHA256SUMS.txt).

## Google Chrome

1. Download [`murmur-chrome-v0.7.0.zip`](../releases/murmur-chrome-v0.7.0.zip).
2. Right-click the ZIP and select **Extract All**.
3. Open the extracted folder and confirm that `manifest.json` is at its top level.
4. Enter `chrome://extensions` in Chrome's address bar.
5. Turn on **Developer mode** in the upper-right corner.
6. Select **Load unpacked**.
7. Choose the extracted folder that directly contains `manifest.json`.
8. Open Chrome's Extensions menu and pin **Murmur** if desired.
9. Open a YouTube song, begin playback, and select Murmur's toolbar icon.

Chrome uses its native side panel when that API is available. Murmur also contains an in-tab reader fallback for environments where the native panel cannot be opened.

## Opera GX

1. Download [`murmur-opera-gx-v0.7.0.zip`](../releases/murmur-opera-gx-v0.7.0.zip).
2. Extract the ZIP to a permanent folder.
3. Open `opera://extensions` in Opera GX.
4. Turn on **Developer mode**.
5. Select **Load unpacked**.
6. Choose the extracted folder that directly contains `manifest.json`.
7. Pin **Murmur** from the Extensions menu if desired.
8. Open a YouTube song, begin playback, and select Murmur's toolbar icon.

Opera GX uses Murmur's own closeable drawer. It overlays the right side of the current YouTube tab and does not create a detached browser window.

## First Run

1. Start playing a song on YouTube or YouTube Music.
2. Open the Murmur popup.
3. Turn on **Murmur is on**.
4. Leave **Open when a new song starts** checked for automatic playlist and autoplay updates.
5. Select **Open lyrics**.
6. Confirm the title and artist shown under **Now playing**.
7. Confirm the selected Genius match shown above the lyrics.

## Updating Manually

1. Download the newer package for the same browser.
2. Close any open Murmur reader.
3. Extract the new ZIP into a new permanent folder, or replace all files in the old extension folder.
4. Return to the browser's extensions page.
5. Select **Reload** on Murmur.
6. Refresh any YouTube tabs that were already open.
7. Open Murmur and confirm the displayed version.

Do not combine the Chrome and Opera GX packages. Their manifests intentionally differ.

## Removing Murmur

1. Open `chrome://extensions` or `opera://extensions`.
2. Find **Murmur**.
3. Select **Remove**.
4. Delete the extracted package folder if you no longer need it.

Removing the extension also removes its locally stored on/off and auto-open preferences.

## Installation Problems

### The browser says the manifest is missing

You selected the wrong folder level. Choose the folder that directly contains `manifest.json`, `background.js`, `popup.html`, and the `assets`, `content`, and `ui` folders.

### The extension loaded, but an old YouTube tab does not respond

Refresh the YouTube tab once. Tabs that were open before installation may not yet have Murmur's content scripts.

### Chrome reports that the extension is not from the Web Store

That is expected for an unpacked developer-mode installation. Only install packages that you obtained from a trusted release and verified with the published checksum.

### Opera GX shows an error about `sidePanel`

The Chrome package was probably loaded by mistake. Remove it and load the `opera-gx` package.

For runtime problems after installation, continue with [Troubleshooting](TROUBLESHOOTING.md).

