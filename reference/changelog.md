# Changelog

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
