# Release Status

LuigiScreen `1.2.0-alpha.3` is an alpha build.

## Ready for testing

- Paper 1.21.11 integration
- MapEngine 1.8.12 rendering
- Windows x86_64 native FFmpeg
- Linux x86_64 native FFmpeg
- RTMP and MJPEG live sources
- Looping local videos and GIFs
- Local and URL images
- Per-screen playlists with weighted random media rotation
- Manual event scenes
- Random media folder items
- Same-PC and remote MediaMTX profiles for RTMP
- Automatic reconnect and viewer pause
- Czech and English messages
- Debug boss bar and sidebar
- Multiple named screens and typed shared media loaders
- Independent per-screen source, FPS, distance, location, dimensions and enabled state
- Granular management permissions and optional protected screens
- Fully non-destructive screen reloads
- Reusable MapEngine render and delta buffers
- Shared player-position snapshots for viewer checks
- Cached playlist folder contents
- Asynchronous Modrinth update notifications
- In-game Control Studio, Live Control and per-role section permissions
- Watched Media Library with generated map thumbnails
- Queues, screen groups, schedules, templates and voting
- Draft/Publish snapshots, audit history and undo
- Advanced anti-repeat, condition diagnostics and event branching
- Browser Web Studio with secure one-time login and session revocation
- Preview/Program Live Studio with bounded live thumbnails
- Structured per-session drafts with validation and external-change protection
- Contextual hover help and accessible descriptions without permanent info icons
- 64 automated tests covering URL and error masking, screen corners, size and
  map limits, configuration clamping, adaptive FPS, MediaMTX generation,
  localization, debug formatting, source grouping, shared-frame lifetime,
  permissions, reload geometry, typed source validation, playback timing and
  semantic Modrinth version selection
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
- URL images are loaded once after success rather than continuously polled
- The inventory timeline is not a free-form visual node canvas
- Media file deletion and reference replacement remain deliberate filesystem/config operations
- Debug native-memory values are estimates, not complete measurements
- MapEngine rendering and packets still scale with every screen, even when decoding is shared
- Video duration metadata is read by the playback worker rather than pre-probed for every library item
- The updater remains quiet until the Modrinth project and a newer version are public
- Web Studio does not provide HTTPS itself; remote access requires a secure
  reverse proxy or VPN
- Web Studio is a structured editor, not a remote unrestricted YAML or file
  deletion interface

## Reporting a problem

Use the [Diagnostic checklist](../troubleshooting/log-checklist.md) and remove credentials before posting logs.
