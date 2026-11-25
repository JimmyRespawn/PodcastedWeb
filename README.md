# This web app is used for Podcasted share feature on Podcasted for Windpws and Xbox

Migrate to github. You may fork it to your own use or commit updates in this branch to help Podcasted get better.

_Last updated: 24 May 2025_

## Web UI

The interface uses Microsoft Fluent UI Web Components 3.1.3 (Fluent 2). The pinned browser
bundle is included in `public/fluent-components.min.js`, with its MIT license in
`public/fluent-components.LICENSE.txt`. `public/fluent-ui.js` registers the controls
and follows the system light/dark theme using the official Fluent 2 web themes
in `public/fluent-themes.js` (tokens 1.0.0-alpha.24, MIT license in
`public/fluent-themes.LICENSE.txt`).

Publish the `public/` directory as static files; no server-side runtime or build
step is needed for GitHub Pages. The official prebundled classic script registers the controls directly, so local preview
also works by opening `public/index.html` directly with a `file://` URL. A local HTTP
preview is supported too. RSS loading still depends on the publisher's CORS policy.

Interface icons are Microsoft Fluent System Icons from
https://github.com/microsoft/fluentui-system-icons (MIT). Selected original SVGs
and their license are kept in `public/icons/`; `public/fluent-icons.js` contains
the inline variants used by dynamic controls. Their fills inherit the page theme.

Settings → Primary color keeps the default blue palette or accepts a custom color.
Companion tints, selection backgrounds, links and Fluent brand tokens are generated
for light/dark appearance, with readable foreground and primary-button contrast.
The choice is saved locally and can be reset to the default `#4169bd`.

## Episodes and sharing

Channel pages filter episode titles locally and sort by publication date (newest
or oldest first); undated episodes stay at the end. Select an episode title to
open its details, or use its play button to listen.

Episode shownotes preserve safe paragraph/list formatting and original links.
Plain HTTP/HTTPS and www addresses become links too, including in older saved
episodes. Web links open in a new tab; scripts, embedded media and source styling
are discarded. MM:SS and H:MM:SS chapter markers remain clickable playback jumps.

Share episode in the episode details can
include a start time, prefilled from the current playback position when applicable.
Times accept seconds, `MM:SS`, or `HH:MM:SS`. Links use the current hosted page's
path and `podurl`, `episode`, and optional `t` (seconds) query parameters, so they
work on GitHub Pages project paths without a backend. Shared episodes open their
details and wait for Play before seeking to the requested position. Clipboard
failure leaves the link selectable for manual copying. Local `file://` previews
show a reminder to create share links from the hosted website.

## Player options and Mini Player

The bottom player's More options (…) menu contains Sleep timer and Mini Player.
An active sleep timer adds a dot to the menu button. Mini Player uses Document
Picture-in-Picture when supported in a secure browser context; unsupported
browsers show a disabled menu entry. The always-on-top window controls the same
audio as the main page, including play/pause, skipping and seeking. Closing the
window keeps playback running, while leaving the main page closes the window.

## Queue

Use the + button beside an episode or Add to queue in its details to enqueue it.
The Queue navigation entry shows upcoming episodes, with move up/down, remove,
clear, and play actions. Duplicate audio URLs cannot be queued twice. Starting a
queued episode removes it from the waiting list. When playback ends naturally,
the first waiting episode starts automatically. Browser autoplay restrictions
may still require pressing Play. Pausing or a sleep timer does not advance the queue.
The queue is saved locally in this browser; it does not sync with the Windows app.

## Subscription links from Windows

Listen accepts subscriptions via `#subs=...` (preferred) or `?subs=...`.
The payload is UTF-8 JSON encoded as Base64URL:

```json
{"v":1,"feeds":["https://example.com/feed.xml","https://example.org/rss"]}
```

Only HTTP/HTTPS feed URLs without embedded credentials are accepted. Duplicate
URLs are removed. RSS titles, authors and artwork load when browser access is
permitted; otherwise the feed URL remains available as a clickable card.

Each valid incoming list replaces the current one and is saved in this browser.
Opening a URL without `subs` restores the last imported list; the original Listen
placeholder appears when no list has been saved. Clear removes the active and
saved list and both forms of the URL parameter, returning to the placeholder.
When browser storage is unavailable, imports still work during the current visit.
Invalid payloads show
an error without replacing a valid list. Keep private feed tokens out of links
that may be shared or saved in browser history.

## Home Screen installation

After deployment over HTTPS, iPhone users can open the site in Safari and choose
Share → Add to Home Screen, keeping “Open as Web App” enabled when offered.
The app uses the existing Podcasted logo, a 180px Apple touch icon, and a relative
web app manifest with 192px/512px icons. Relative paths support GitHub Pages
project subdirectories. Home Screen launch uses the name Podcasted and standalone
display mode. This configuration does not add offline playback or offline caching.
