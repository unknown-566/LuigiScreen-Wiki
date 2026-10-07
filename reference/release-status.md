# Release Status

LuigiScreen `1.3.0-alpha.1` is an alpha build.

## Ready for testing

- Paper 1.21.11 and 26.x
- Built-in packet renderer (no MapEngine) with partial map updates
- Windows and Linux x86_64 native FFmpeg
- Local videos, images and GIFs; web images and GIFs
- YouTube, Twitch and other video sites through yt-dlp
- RTMP (OBS through MediaMTX) and MJPEG cameras
- Multiple screens sharing one decoder per source
- Playlists with weights, conditions and anti-repeat; events; automations; groups; voting
- Web Studio for desktop and phone, with one-time login
- In-game Control Studio
- Draft/Publish snapshots, audit history and undo
- Czech and English messages

## Known alpha limitations

- No sound in Minecraft yet (planned: resource pack and Simple Voice Chat)
- No ARM or macOS native libraries
- No Folia support
- No built-in MediaMTX process manager
- Online video depends on yt-dlp keeping up with site changes
- Rendering and packets scale with every visible screen, even when decoding is shared
- Web Studio does not provide HTTPS itself; remote access requires a secure reverse proxy or VPN
- Media file upload and deletion are not available from Web Studio

## Before a stable public release

- Broader testing on public servers and real clients on every supported version
- Long-duration Linux and Windows stream tests
- Clean shutdown tests during network failure
- More automated lifecycle and integration tests

## Reporting a problem

Use the [Diagnostic checklist](../troubleshooting/log-checklist.md) and remove credentials before posting logs.
