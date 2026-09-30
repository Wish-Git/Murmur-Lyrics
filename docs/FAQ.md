# Frequently Asked Questions

## Does Murmur support both Chrome and Opera GX?

Yes. Use the package labeled for your browser. Chrome uses its native side panel when available. Opera GX uses Murmur's in-tab drawer.

## Does Murmur work on YouTube Music?

Yes. It reads YouTube Music's player metadata and also uses Media Session metadata as a fallback.

## Why are there two ZIP files?

Chrome's package includes the Chrome-only `sidePanel` permission and manifest entry. The Opera GX package omits them and uses the in-tab reader.

## Does Murmur open a separate browser window?

No in v0.7.0. Chrome opens a native side panel. Opera GX opens a closeable drawer attached to the current YouTube tab.

## Where do the lyrics come from?

Murmur searches public Genius pages, selects a candidate, extracts lyrics text in memory, and renders that text in its own interface. It provides a **View source** link to the original page.

## Why is Genius the only provider?

Earlier provider experiments were less reliable in an extension context. Version 0.7.0 standardizes the search, ranking, extraction, and source-verification flow around Genius.

## Will Murmur always pick the correct result?

No automated match can guarantee that when source metadata is incomplete or ambiguous. Murmur strongly prefers matching title and artist, penalizes unrequested versions, and then uses a best-available fallback. Check the displayed match label for unusual tracks.

## What happened to the confidence filter?

It was removed. Murmur now delivers its best valid result even when no candidate passes strict title-and-artist comparison.

## Will the lyrics update when autoplay or a playlist advances?

Yes. Murmur tracks YouTube navigation, player events, metadata changes, video IDs, and a periodic fallback. The open reader cancels stale requests and reloads for a new track key.

## Does Murmur save my listening history?

No. It stores only the enabled and auto-open preferences. Current media and result state are held in memory.

## Does Murmur send data to a Murmur server?

No. There is no Murmur-operated backend. The browser contacts Genius directly when it searches and retrieves a lyrics page.

## Why must I use Developer mode?

These releases are unpacked extension packages rather than store-installed packages. Chromium browsers require Developer mode to load an unpacked folder.

## Can the packaged code be inspected?

Yes. Browser extensions must ship their runtime files to the user's computer. The public GitHub tree avoids publishing the development source as individual files, but an installed or extracted package is inspectable. Read [Source visibility](SOURCE_VISIBILITY.md).

## Is the private development source available?

Not in this distribution repository. Public documentation describes the behavior, module boundaries, algorithms, permissions, and release checks without publishing the private source tree.

## Can Murmur be published to a browser store later?

Yes, but store submission requires additional owner work such as store accounts, listing assets, policy review, package signing, and any changes requested during review. This repository does not claim that v0.7.0 has passed Chrome Web Store or Opera Add-ons review.

## Are lyrics included in the release ZIP?

No. Murmur does not bundle a lyrics database. Lyrics are requested when the reader needs them.

