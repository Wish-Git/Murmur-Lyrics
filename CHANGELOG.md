# Changelog

All notable public releases of Murmur are documented here.

## 0.7.0 - 2026-09-29

- Replaced Opera GX's detached lyrics window with a closeable drawer attached to the current YouTube tab.
- Removed AZLyrics and Musixmatch and standardized the reader on Genius.
- Removed the likely-song filter, confidence badge, and minimum-confidence refusal.
- Added a best-available-result fallback while retaining strict title-and-artist ranking when comparable results exist.
- Improved cleanup for titles containing featured artists, including `Desires ft. Future`.
- Excluded Genius translation menus, contributor controls, descriptions, and hidden elements from the rendered lyrics.
- Improved active YouTube tab recovery when an extension surface was recently focused.
- Added automatic new-song refresh, stale-request cancellation, and regression validation.
- Refined the rounded white, stone, slate, and black interface.

## 0.6.0

- Replaced the embedded Genius website with Murmur's themed lyrics reader.
- Added in-browser extraction from public Genius search and lyrics pages.
- Added title-and-artist ranking, URL validation, and safe text-only rendering.
- Added loading, empty, error, retry, source-link, and narrow-screen states.

## 0.5.0

- Renamed the extension to Murmur.
- Added separate Chrome and Opera GX packages.
- Introduced the white, stone, slate, and black visual system and Murmur artwork.
- Added automatic playlist, autoplay, and YouTube Music track-change detection.

## 0.4.0

- Added track identity handling for playlist and autoplay changes.
- Added a metadata-settling delay and request identity checks.

