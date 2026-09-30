# How Murmur Works

This is a step-by-step implementation walkthrough. It describes the code's decisions and control flow without reproducing the private source tree.

## Step 1: The Browser Loads Murmur

The browser reads a Manifest V3 manifest and starts a background service worker when an extension event requires it. Content scripts are registered for these origins:

- `https://www.youtube.com/*`
- `https://m.youtube.com/*`
- `https://music.youtube.com/*`

The manifest also grants access to `https://genius.com/*` so the local reader can make cross-origin search and lyrics requests.

Chrome's package declares native side-panel support. Opera GX's package leaves those Chrome-only entries out. Everything else stays in parity.

## Step 2: The Detector Attaches To YouTube

YouTube is a single-page application, so moving to another video often does not reload the whole tab. Murmur therefore combines several signals:

1. A DOM mutation observer notices title and page-content changes.
2. YouTube navigation and player events trigger a short burst of checks.
3. Video events such as play, metadata load, duration change, end, and empty trigger checks.
4. Page visibility changes trigger checks.
5. A one-second interval provides a final fallback.
6. The background worker can request an immediate collection.

The burst runs shortly after an event and then checks again while YouTube finishes replacing metadata. A small debounce prevents every DOM mutation from becoming a message.

## Step 3: Murmur Collects A Media Snapshot

The detector locates the current video element and reads:

- The current URL and video ID.
- The page title.
- The YouTube channel.
- YouTube Music's player title and byline when applicable.
- Media Session title and artist as a fallback.
- Whether the video is playing, audible, ended, or paused.
- Current time and finite duration.

The visible title is lightly cleaned by removing YouTube's tab suffix and notification count. A signature is calculated from the URL, media ID, title, artist, channel, playback state, and coarse duration. Repeated identical signatures are suppressed for up to eight seconds unless the request was forced.

## Step 4: The Coordinator Stores Per-Tab State

The background worker receives the snapshot and enriches it with the tab ID, window ID, receive time, track key, and track-change time.

State is separated by tab so two YouTube tabs do not overwrite one another. Closing a tab deletes its in-memory media, track, result, search signature, and timer entries.

## Step 5: Murmur Builds A Track Identity

Raw YouTube titles are inconsistent. Murmur converts them into a search identity in stages.

1. Look for a pattern shaped like `artist - title`, also accepting a colon and common dash characters.
2. If that pattern exists, treat the left side as the artist and the right side as the title.
3. Remove topic-channel suffixes and `VEVO` from artist text.
4. Remove bracketed or parenthesized labels containing terms such as official, audio, video, lyrics, visualizer, HD, or 4K.
5. Remove long-form labels such as `Official Music Video`.
6. Remove a trailing featured-artist clause from the searchable title.
7. Collapse whitespace.
8. Fall back to YouTube Music artist data or the uploader channel when the title did not include an artist.

Examples:

| YouTube text | Search title | Search artist |
| --- | --- | --- |
| `Drake - TSU (Audio)` | `TSU` | `Drake` |
| `Drake - Desires (Audio) ft. Future` | `Desires` | `Drake` |
| `Artist - Song [Official Video]` | `Song` | `Artist` |
| `Song Name` on an artist channel | `Song Name` | Channel or Media Session artist |

The final query contains artist plus title when both exist and are not effectively the same text.

## Step 6: Murmur Detects A New Song

The track key combines:

- The YouTube media ID, when available.
- A punctuation-insensitive artist-and-title identity.

This catches both URL/video changes and metadata-only changes in YouTube Music. When the key changes, Murmur removes the previous result and cancels the previous pending auto-search timer.

For a changed track, Murmur waits about 1.2 seconds before preparing the reader. For a stable update, it waits about 450 milliseconds. If a newer track key appears during that wait, the old work is discarded and a fresh short wait begins.

## Step 7: The Popup Requests Work

The popup asks for a fresh detection when it opens, then refreshes every three seconds and after state-change messages.

When the user selects **Check tab**, the popup requests detection and prepares the active result. When the user selects **Open lyrics**, it prepares the result and opens the browser's reader surface.

Turning Murmur off closes an in-tab reader. Turning it on attempts Chrome's native side panel first and otherwise opens the in-tab drawer.

## Step 8: The Correct YouTube Tab Is Recovered

Opening an extension popup can make tab selection feel ambiguous. The coordinator first asks for the active tab in the last-focused window. If that is not YouTube, it searches active tabs matching the supported YouTube URLs and chooses the most recently accessed one.

This lets Murmur reconnect to the intended video even when an extension surface briefly had focus.

## Step 9: The Reader Receives A Stable Context

The reader URL can contain a source tab ID. The reader asks the coordinator for that tab's media, prepared track, settings, result metadata, and track key.

If no source ID exists, the coordinator tries the active YouTube tab and then the most recently received YouTube media snapshot. This makes the reader resilient to panel focus changes.

## Step 10: Genius Search Runs In Two Passes

The reader first requests Genius's public search page using the cleaned query. It parses song-card links, titles, and artist labels.

If no comparable candidate survives ranking, Murmur requests Genius's structured multi-search endpoint and collects entries that look like songs or lyrics pages. Both sources are combined and deduplicated by canonical URL.

Network requests use:

- `cache: no-store` so a recent query is not hidden by a stale cache.
- `credentials: omit` so Murmur does not send a Genius login session.
- An abort signal so old requests stop when the song changes.

## Step 11: Candidate URLs Are Validated

Before a candidate can be used, its URL must:

1. Use HTTPS.
2. Use `genius.com` or `www.genius.com`.
3. End with a path that looks like a lyrics page.

The URL is canonicalized to `genius.com`, and its query string and fragment are removed. Other hosts and non-lyrics paths are rejected.

## Step 12: Comparable Candidates Are Ranked

Titles and artists are normalized by:

- Removing accents for comparison.
- Converting to lowercase.
- Removing common video-label words.
- Removing featured-artist suffixes.
- Replacing punctuation with spaces.
- Collapsing whitespace.

Token similarity is the shared-token count divided by the larger token-set size.

The score uses these bands:

| Comparison | Score |
| --- | ---: |
| Exact normalized title | 100 |
| Title similarity at least 0.80 | 80 |
| Title similarity at least 0.65 | 65 |
| Exact normalized artist | 70 |
| One artist text contains the other | 55 |
| Artist token similarity at least 0.60 | 45 |
| Target artist unavailable | 20 |
| Unrequested remix, demo, live, acoustic, translation, karaoke, cover, or instrumental marker | -45 |

A title below the title bands or an incompatible known artist is excluded from scored results. If one or more comparable candidates remain, the highest score wins.

## Step 13: Best-Available Fallback Prevents Confidence Refusals

Version 0.7.0 has no minimum confidence threshold. If strict comparisons reject every candidate but Genius returned at least one valid song title and URL, Murmur selects the first unique usable candidate.

This behavior intentionally trades occasional mismatch risk for availability. The reader always displays the selected candidate's title and artist and provides a source link so the user can verify it.

## Step 14: Murmur Fetches And Parses The Lyrics Page

The selected canonical URL is fetched directly. The HTML is parsed into an inert document, which means the remote page's scripts are not executed as part of parsing.

Murmur looks for Genius's current lyrics containers first and then its legacy lyrics container as a compatibility fallback.

## Step 15: Only Lyrics-Like Text Is Serialized

The extractor recursively walks the selected containers.

- Text nodes contribute their text.
- `<br>` contributes a newline.
- Block-like lyric elements add a line ending.
- Script, style, button, and SVG elements are skipped.
- `aria-hidden="true"` content is skipped.
- Elements Genius marks as excluded from selection are skipped.

That last rule removes translation menus, contributor controls, song descriptions, and similar header content that can appear inside the broader lyrics area.

The resulting text has carriage returns removed, nonbreaking spaces normalized, repeated spaces collapsed, line-edge whitespace removed, and excessive blank lines reduced. Very short output is treated as an extraction failure.

## Step 16: The Reader Renders Safe Local Elements

The reader splits the normalized text by line. A line shaped like `[Chorus]` or `[Verse 1]` becomes a section-label element. A normal line becomes a paragraph. Empty runs become a controlled stanza gap.

Every lyric line is assigned through a text property. Murmur does not inject the remote page's HTML into the reader, so remote scripts, attributes, and styling do not cross into Murmur's document.

## Step 17: Stale Requests Cannot Win

Each load receives an incrementing request token and an expected track key. Before formatting or displaying a result, the reader checks that both still match the current song.

When a new track appears:

1. The current abort controller is aborted.
2. The request token advances.
3. The current track key changes.
4. Any late old response fails the identity check.

This prevents slow lyrics for song A from replacing song B after autoplay advances.

## Step 18: Chrome And Opera GX Present The Same Reader Differently

Chrome attempts to open `sidepanel.html` through its native side-panel API. Opera GX uses the content-script drawer because the Chrome-specific side-panel manifest capability is removed from its package.

The drawer creates an `aside`, a loading status, and an iframe whose URL points to the local reader with the source tab ID and an embedded flag. Only that local HTML entry point is web-accessible on YouTube.

The embedded reader's X button sends a narrowly named close message to its parent. The host verifies that the message came from its own iframe before hiding the drawer. The `Escape` key is also handled by both layers for predictable closing.

## Step 19: The UI Is A Small Design System

The popup and reader share:

- White surfaces on a stone canvas.
- Slate secondary text.
- Black primary actions and enabled indicators.
- Thin stone borders.
- Eight-pixel panel corners and a rounded popup shell.
- Explicit narrow-width layouts.
- Visible focus rings.
- Reduced-motion behavior when requested by the operating system.

Stable widths, grid tracks, wrapping rules, and narrow-screen media queries prevent controls and long titles from overflowing.

## Step 20: Preferences And Privacy Stay Small

Only two booleans enter extension storage:

- Enabled.
- Open when a new song starts.

Media snapshots, search context, and extracted lyrics are held in memory. Murmur has no server, analytics SDK, ad code, account, or browsing-history database.

## Step 21: Release Validation Checks The Important Contracts

The release process validates:

- Required files in both packages.
- Manifest V3, product name, and version.
- Chrome-only side-panel declarations.
- Opera's absence of Chrome-only declarations.
- Exact parity of shared runtime files.
- Valid PNG signatures for all icons.
- JavaScript parseability.
- No legacy product branding.
- Correct TSU and Desires title cleanup.
- Different keys for different songs.
- Rejection of a known wrong TSU result.
- Preference for the correct Desires result over a translation.
- Best-available fallback behavior.
- Canonical Genius URL enforcement.
- Lyrics whitespace normalization.
- Exclusion of Genius non-lyrics blocks.
- Presence and close behavior of the in-tab drawer.

The public release workflow performs an additional package-level check: archive integrity, root manifest presence, browser-specific manifest shape, expected runtime files, package version, and published checksums.

## End-To-End Summary

```text
YouTube changes
  -> detector samples media
  -> coordinator stores tab state
  -> track identity and track key are built
  -> current result is prepared
  -> reader searches Genius
  -> candidates are validated and ranked
  -> best comparable or best available candidate is selected
  -> lyrics containers are extracted as text
  -> local reader elements are rendered
  -> Chrome side panel or Opera drawer stays synchronized
```

For component boundaries, messages, and state models, return to [Project architecture](ARCHITECTURE.md).

