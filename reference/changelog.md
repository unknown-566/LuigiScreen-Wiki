# Changelog

## 1.1.0-alpha.11

- Fixed `/screen reload` removing MapEngine displays
- Reload now preserves a screen when its world, location, dimensions and facing are unchanged
- URL, FPS, distance, enabled state, permission and rendering settings update in place
- Removed screens are still destroyed and geometrically changed screens are recreated
- MediaMTX source changes now use the same non-destructive reconciliation
- Added regression tests for reload geometry decisions

## 1.1.0-alpha.10

- Changed the offline, connecting, waiting and stopped screen title to `LuigiScreen`
- Added the customizable `screen.offline-title` localization key
- Kept the new title compatible with existing language files through the bundled fallback

## 1.1.0-alpha.9

- Added a separate permission for every `/screen` subcommand
- Kept `luigiscreen.admin` as the operator-level parent permission
- Added optional per-screen visibility protection
- Added dynamic `luigiscreen.see.<screen-name>` permissions
- Added the `luigiscreen.see.*` wildcard
- Added `/screen set <name> permission <true|false>`
- Added `permission-required: false` to per-screen configuration
- Made newly created and migrated screens public by default
- Made protected clones inherit their source screen's visibility setting
- Added permission metadata and default-behavior tests

## 1.1.0-alpha.8

- Added multiple named screens
- Added `/screen create <name> [width] [height]`
- Kept `/screen create 7 4` as legacy shorthand for `main`
- Added `/screen clone <source> <new-name>`
- Added `/screen list`, named status and targeted start, stop and remove commands
- Added `/screen set <name> <url|fps|distance|enabled> <value>`
- Added independent URL, FPS, distance, world, location, width, height and enabled state per screen
- Added automatic source grouping by RTMP URL
- Added one shared FFmpeg decoder for every unique URL
- Added reference-counted shared frames so clones do not duplicate decoded images
- Added independent render pacing and latest-frame queues per screen
- Added automatic migration of the old single-screen config to `screens.main`
- Expanded debug output with total, enabled and shared-source counts
- Added tests for screen definitions, source grouping, migration geometry and shared-frame lifetime

## 1.1.0-alpha.7

- Added bundled Linux x86_64 JavaCPP and FFmpeg native libraries
- Retained Windows x86_64 support
- Fixed `UnsatisfiedLinkError: no jniavutil` on Linux amd64 hosts such as Minekeep
- Published the LuigiScreen Free source code
- Added the AGPL-3.0-only license and third-party notices
- Added a trademark policy for unofficial modified builds
- Added a Java 21 GitHub Actions verification build
- Clarified the Paper/Bukkit plugin classification across public metadata
- Hardened URL masking across status output, connection logs and error messages
- Centralized screen geometry, map-limit and adaptive-FPS policy
- Added tests for all screen directions, FPS limits and configuration validation
- Verified that no-viewer pause exits the receive loop and closes the FFmpeg grabber
- Added connecting and offline screen states during RTMP outages

## 1.1.0-alpha.6

- Made MediaMTX profile switching asynchronous
- Prevented the main server thread from waiting for FFmpeg shutdown
- Ensured a replacement decoder never starts before the previous worker terminates
- Added localized decoder-switching feedback

## 1.1.0-alpha.5

- Added a 15-line `/screen debug` sidebar
- Added personal scoreboard restoration and conflict protection
- Added Czech and English sidebar labels
- Improved localization fallback for older customized files

## Earlier prototypes

Earlier local builds established:

- RTMP decoding
- MapEngine rendering
- Reconnect behavior
- Screen persistence
- MediaMTX setup generation
- Configurable limits

They are not recommended for public installation.
