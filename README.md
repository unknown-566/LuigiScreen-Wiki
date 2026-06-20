# LuigiScreen

LuigiScreen is a server-side Paper/Bukkit plugin that displays streams,
videos, images and GIFs on walls of Minecraft maps.

It loads the selected media source, decodes its latest frame and renders it
through **MapEngine**. Players do not need a client mod.

Author: **unknown_56**

> LuigiScreen is currently an alpha project. Back up your server before upgrading and test new builds away from production.

## Current release

Documentation version: `1.2.0-alpha.2`

Supported server platforms:

- Paper `1.21.11`
- Java `21`
- Windows x86_64
- Linux x86_64

Required plugin:

- MapEngine `1.8.12`

LuigiScreen belongs to the Bukkit plugin ecosystem but currently targets
Paper APIs. Install it on Paper, not on a plain Spigot or CraftBukkit server.

Project links:

- [Free source code](https://github.com/unknown-566/LuigiScreen)
- [Issue tracker](https://github.com/unknown-566/LuigiScreen/issues)
- [License and editions](reference/licensing.md)

## Start here

Server administrators should begin with the
[Control Studio overview](studio/control-studio.md). Open the in-game interface
with `/screen menu`, or create a secure browser session with `/screen web` and
follow the [Web Studio guide](studio/web-studio.md).

Start with [Media sources](screen/sources.md) to choose RTMP, MJPEG, a local
video, a local or remote image, or a GIF.

For live OBS video, follow the [RTMP quick start](getting-started/quick-start.md).
If the Minecraft server is hosted elsewhere, first read
[Choose an RTMP network setup](streaming/overview.md).

If you prefer Czech, use the [Český rychlý start](czech/quick-start.md).

## What LuigiScreen provides

- In-game Control Studio and local browser Web Studio with role-based sections
- One-time Web Studio login links, secure sessions, CSRF protection and revocation
- Browser Live Studio with Preview/Program control and bounded live thumbnails
- Live Control, per-screen queues, groups, schedules and audience voting
- Watched Media Library with validation and generated map thumbnails
- Draft/Publish editing, config snapshots, audit history and undo
- Playback explanations, eligibility diagnostics and usage statistics
- Multiple named map screens up to the safety limits in `config.yml`
- RTMP and MJPEG live streams
- Looping local videos and GIFs
- Local and URL images
- One shared loader for screens using the same source type and value
- Independent source, FPS, distance, world, location, width, height and enabled state per screen
- Granular command permissions and optional per-screen visibility permissions
- Fully non-destructive configuration reloads; `/screen remove` is required to delete a screen
- Optional guided MediaMTX setup for RTMP
- Automatic reconnect with exponential backoff
- Viewer-distance based pause and resume
- Adaptive FPS for large screens
- Localized Czech and English messages
- Performance boss bar and 15-line debug sidebar
- Masked remote-source credentials in status output and logs
- Public AGPL-3.0-only Free source code

## Live-stream limitation

LuigiScreen reads a live stream; it does not create one by itself. When using
RTMP, MediaMTX must be running somewhere and OBS or another publisher must
send video to it. If the publishing computer is turned off, live desktop
capture stops.

For simple 24/7 playback, use a local video or GIF directly. For a remotely
managed RTMP source, see [External hosting and 24/7](network/external-hosting.md).
