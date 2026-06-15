# Release Status

LuigiScreen `1.1.0-alpha.9` is an alpha build.

## Ready for testing

- Paper 1.21.11 integration
- MapEngine 1.8.12 rendering
- Windows x86_64 native FFmpeg
- Linux x86_64 native FFmpeg
- Same-PC and remote MediaMTX profiles
- Automatic reconnect and viewer pause
- Czech and English messages
- Debug boss bar and sidebar
- Multiple named screens and URL-based shared decoders
- Independent per-screen source, FPS, distance, location, dimensions and enabled state
- Granular management permissions and optional protected screens
- 37 automated tests covering URL and error masking, screen corners, size and
  map limits, configuration clamping, adaptive FPS, MediaMTX generation,
  localization, debug formatting, source grouping, shared-frame lifetime and permissions
- Public Free source at https://github.com/unknown-566/LuigiScreen
- AGPL-3.0-only project license
- Third-party notices and license texts
- Trademark policy for unofficial modified builds
- GitHub Actions verification on Java 21

## Before a stable public release

The project should still complete:

- Broader testing on public Paper servers
- Long-duration Linux and Windows stream tests
- Clean shutdown tests during network failure
- Upload-size testing on common hosting panels
- A privacy review of generated support logs
- More automated lifecycle and integration tests

## Known alpha limitations

- No audio playback in Minecraft
- No ARM or macOS native libraries
- No Folia support
- No built-in MediaMTX process manager
- No built-in OBS replacement
- Debug native-memory values are estimates, not complete measurements
- MapEngine rendering and packets still scale with every screen, even when decoding is shared

## Reporting a problem

Use the [Diagnostic checklist](../troubleshooting/log-checklist.md) and remove credentials before posting logs.
