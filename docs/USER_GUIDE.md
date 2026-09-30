# User Guide

This guide covers Murmur's normal controls and expected behavior.

## The Popup

Select the Murmur toolbar icon to open the popup. It has three working areas.

### Status

The first panel shows whether Murmur is on and what it knows about the current browser tab.

- **Murmur is off**: detection can still be requested manually, but automatic song handling is disabled.
- **Open a YouTube tab to begin**: the focused tab is not a supported YouTube page.
- **Ready to check this YouTube tab**: YouTube is open, but no media state has arrived yet.
- **YouTube is open, playback is paused**: metadata exists, but the video is not currently playing.
- **Song detected**: Murmur has a title and active playback.

Use the switch on this panel to enable or disable Murmur.

### Now Playing

The second panel shows the detected title, artist or channel, and whether playback is playing or paused. YouTube titles often contain extra labels; the reader uses a cleaned track identity even when the popup displays the recognizable video title.

### Lyrics Controls

- **Open lyrics** searches for the current track and opens or toggles the reader.
- **Check tab** asks the active YouTube tab for fresh metadata and prepares a lyrics result.
- **Open when a new song starts** controls automatic reader opening and updating.

Genius is the single lyrics source in v0.7.0.

## The Lyrics Reader

The reader shows:

1. The track Murmur believes is playing.
2. The Genius title-and-artist result that Murmur selected.
3. A **View source** link to the original Genius page.
4. Lyrics formatted into section labels, lines, and stanza gaps.
5. Loading, empty, and retryable error states when needed.

Murmur does not display the remote Genius webpage inside the panel. It extracts text from the public page and builds the reader locally.

## Chrome Behavior

Chrome opens Murmur in the browser's native side panel. The panel can stay visible while you navigate and can be closed with Chrome's side-panel controls.

Chrome requires panel opening to occur from a user action in some situations. Selecting **Open lyrics** or turning Murmur on from the popup provides that user action. If the native panel cannot open, Murmur falls back to its in-tab drawer.

## Opera GX Behavior

Opera GX opens a 440-pixel-wide drawer over the right side of the current YouTube page. On narrow windows, it leaves a small part of YouTube visible instead of overflowing the viewport.

Close it with:

- The X button in the reader header.
- The `Escape` key.
- **Open lyrics** again, which toggles the drawer.
- Turning Murmur off.

The drawer is attached to that YouTube tab. It is not a separate Opera window.

## Automatic Song Changes

When a playlist, autoplay, YouTube navigation, or YouTube Music metadata update changes the song, Murmur:

1. Collects the new title, artist or channel, media ID, and playback state.
2. Compares the new track key with the previous one for that tab.
3. Clears the previous result if the identity changed.
4. Waits briefly for YouTube's metadata to settle.
5. Prepares the new track and tells an open reader to refresh.
6. Cancels any older lyrics request that is still running.

The reader also checks for context changes every 2.5 seconds, which provides a fallback when a site event is missed.

## Understanding Best Available Match

Murmur ranks results by normalized song title and artist. Exact title-and-artist matches win. It penalizes unrelated versions such as a translation, karaoke track, cover, or remix when the YouTube title did not request that version.

If no result passes those comparisons but Genius returned a valid song page, Murmur displays the first usable result. This is intentional: v0.7.0 favors showing its best guess over displaying a confidence refusal.

Always check the match label when:

- The video title is vague.
- The channel is not the artist.
- The song is a remix, cover, translation, or live recording.
- Several artists have songs with the same title.

## Privacy At A Glance

Murmur stores only two preferences: whether it is enabled and whether auto-open is enabled. Track data and lyrics stay in browser memory and are not sent to a Murmur-owned server. Searches and lyrics-page requests go directly to Genius. See [Privacy and permissions](PRIVACY.md).

