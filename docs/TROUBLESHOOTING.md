# Troubleshooting

Work through the section that matches what you see. Most issues are fixed by reloading the extension and refreshing the already-open YouTube tab.

## Murmur Says To Open A YouTube Tab

1. Make sure the current page uses `www.youtube.com`, `m.youtube.com`, or `music.youtube.com`.
2. Start playback.
3. Reopen the Murmur popup.
4. Select **Check tab**.
5. If the tab was open before Murmur was installed or updated, refresh it once.

The browser does not allow extension content scripts on internal pages such as `chrome://extensions` or `opera://extensions`.

## No Track Is Detected

1. Confirm that the video has reached a playable state and is not merely a thumbnail or premiere placeholder.
2. Select **Check tab**.
3. Pause and resume playback once.
4. Refresh YouTube.
5. Open the browser's extension page and select **Reload** on Murmur.
6. Check whether another extension is replacing or heavily modifying the YouTube player.

For YouTube Music, wait until the bottom player bar displays a title and artist.

## The Reader Does Not Open

### Chrome

1. Select **Open lyrics** directly from the popup. Chrome can require a user gesture to open its native side panel.
2. Look for Chrome's side-panel area and side-panel toolbar icon.
3. Close another side-panel extension and try again.
4. Refresh YouTube so Murmur's in-tab fallback can attach.

### Opera GX

1. Confirm that you installed the Opera GX package, not the Chrome package.
2. Refresh the YouTube tab.
3. Select **Open lyrics** again.
4. Press `Escape` once in case a hidden drawer state needs to be reset, then try again.
5. Reload Murmur from `opera://extensions`.

The Opera reader should be part of the current tab. If a detached window appears, remove older Murmur or pre-Murmur builds before loading v0.7.0.

## The Lyrics Are For The Wrong Song

Murmur v0.7.0 deliberately shows the best available valid Genius result instead of refusing low-confidence results.

1. Compare **Now playing** with the YouTube title and channel.
2. Compare the match label above **View source** with the expected title and artist.
3. Open **View source** to verify the selected Genius page.
4. If the video title is unusually vague, try an official upload or YouTube Music entry with better metadata.
5. Report the YouTube URL, displayed track identity, selected match label, browser, and Murmur version. Do not paste full copyrighted lyrics into an issue.

Useful examples of ambiguous metadata include covers posted under a channel name, slowed or sped-up versions, mashups, and videos whose title omits the artist.

## The Reader Shows Translation Links Or A Song Description

Version 0.7.0 filters Genius blocks marked as excluded, hidden controls, buttons, SVGs, scripts, and styles. Confirm that the reader footer says `Murmur v0.7.0`.

If it does and the extra content remains, Genius may have changed its markup. Include the source URL and a screenshot in a bug report, but do not paste the complete lyrics.

## Lyrics Stay On The Previous Song

1. Wait two to three seconds for YouTube metadata to settle.
2. Confirm playback actually advanced to a different media ID.
3. Select **Check tab**.
4. Close and reopen the reader.
5. Refresh the YouTube tab.
6. Reload the extension if the old reader was open during an update.

Murmur listens to YouTube navigation and player events, video events, DOM mutations, and a periodic fallback. It also checks reader context every 2.5 seconds.

## Genius Returns An Error

Possible causes include:

- The device is offline.
- Genius temporarily rate-limited or rejected the request.
- The page has no current lyrics container.
- Genius changed its search or lyrics markup.
- A privacy, filtering, firewall, or ad-blocking rule blocked the request.

Try again after a short wait. Test the **View source** site in a normal tab. Do not disable security software globally; add a narrowly scoped exception only if you understand and accept it.

## Layout Looks Too Narrow Or Text Wraps Poorly

1. Confirm that you are running v0.7.0.
2. Reset browser zoom to 100 percent.
3. Widen the Chrome side panel or Opera window.
4. Reload the extension so the newest CSS is active.
5. Include the browser version, operating-system scaling, browser zoom, viewport width, and screenshot in a report.

## Reset Murmur

1. Turn Murmur off.
2. Close the reader.
3. Open the browser's extension page.
4. Reload Murmur.
5. Refresh YouTube.
6. Turn Murmur on again.

For a complete clean reinstall, remove Murmur, delete its extracted folder, extract a freshly verified ZIP, and load it again.

## Collecting A Useful Bug Report

Include:

- Browser and exact version.
- Murmur version.
- Operating system.
- Chrome or Opera GX package.
- YouTube or YouTube Music URL.
- What Murmur displayed under **Now playing**.
- The selected Genius title and artist, if any.
- Expected behavior and actual behavior.
- Reproduction steps.
- Screenshot with private account details cropped out.

Never include passwords, cookies, browser profiles, private messages, or full copyrighted lyrics.

