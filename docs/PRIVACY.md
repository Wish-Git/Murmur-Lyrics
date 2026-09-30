# Privacy And Permissions

Murmur is designed to do its work inside the browser. It does not operate a project server and does not include analytics, advertising, tracking pixels, or an account system.

## Data Murmur Reads

While enabled or when manually asked to check a tab, Murmur reads limited information from supported YouTube pages:

- Page URL and YouTube media ID.
- Video or song title.
- Channel or artist name.
- Playing, paused, ended, muted, volume, time, and duration state.
- Browser Media Session title and artist when YouTube page elements are insufficient.

This information is used to decide whether media is playing, recognize track changes, and build a lyrics search.

## Data Murmur Stores

Murmur stores only two preferences in the browser's local extension storage:

- Whether Murmur is enabled.
- Whether the reader should open automatically when a new song starts.

Recent tab, track, and result context is kept in extension memory. It can disappear when the Manifest V3 service worker stops, when the tab closes, or when the browser exits. Extracted lyric text is kept in the open reader document and is not written to extension storage.

Murmur does not maintain a browsing-history database.

## External Requests

When lyrics are requested, Murmur sends a search query directly from the browser to Genius and then requests the selected public Genius lyrics page. Genius can therefore receive normal web-request information such as the user's IP address, user agent, request time, and search terms, subject to Genius's own privacy policy and terms.

Murmur does not proxy those requests through a project-owned server. Requests omit browser login credentials.

## Permission Explanation

| Permission or host | Why it is needed |
| --- | --- |
| `activeTab` | Work with the YouTube tab the user is actively using |
| `tabs` | Identify the active YouTube tab, tie state to a tab, and clean up when it closes |
| `scripting` | Recover detector or drawer scripts when an already-open tab missed normal injection |
| `storage` | Save the enabled and auto-open preferences |
| `sidePanel` in Chrome | Open Murmur in Chrome's native side panel |
| YouTube host access | Run detection on supported YouTube pages |
| Genius host access | Search for a song and retrieve its public lyrics page |

## What Murmur Does Not Do

- It does not collect names, email addresses, passwords, or payment information.
- It does not read non-YouTube page content.
- It does not sell or share a user profile.
- It does not upload lyrics to a Murmur server.
- It does not run third-party Genius scripts in the Murmur reader.
- It does not store a lyrics catalog.

## Deleting Local Data

Disable and remove Murmur from the browser's extensions page. Browser-managed extension storage is removed with the extension. You can also clear extension data through the browser's site and extension settings.

## Changes To This Document

Privacy-affecting changes should appear in both this document and the project changelog before a new package is published.

