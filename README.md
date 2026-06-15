# LuigiScreen

LuigiScreen is a server-side Paper/Bukkit plugin that displays a live RTMP video stream on a wall of Minecraft maps.

It connects **OBS Studio** to **MediaMTX**, decodes the stream with FFmpeg, and renders the latest video frame through **MapEngine**.

Author: **unknown_56**

> LuigiScreen is currently an alpha project. Back up your server before upgrading and test new builds away from production.

## Current release

Documentation version: `1.1.0-alpha.11`

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

If everything runs on one computer, follow the [Quick start](getting-started/quick-start.md).

If the Minecraft server is hosted elsewhere, first read [Choose a network setup](streaming/overview.md).

If you prefer Czech, use the [Český rychlý start](czech/quick-start.md).

## What LuigiScreen provides

- Multiple named map screens up to the safety limits in `config.yml`
- One shared FFmpeg decoder for screens using the same RTMP URL
- Independent URL, FPS, distance, world, location, width, height and enabled state per screen
- Granular command permissions and optional per-screen visibility permissions
- Fully non-destructive configuration reloads; `/screen remove` is required to delete a screen
- RTMP input from MediaMTX
- Guided MediaMTX setup for five network situations
- Automatic reconnect with exponential backoff
- Viewer-distance based pause and resume
- Adaptive FPS for large screens
- Localized Czech and English messages
- Performance boss bar and 15-line debug sidebar
- Masked RTMP credentials in status output and logs
- Public AGPL-3.0-only Free source code

## Important limitation

LuigiScreen displays a stream; it does not create one by itself. MediaMTX must be running somewhere, and OBS or another publisher must send video to it. If the publishing computer is turned off, live desktop capture stops.

For a permanent 24/7 source, use an external VPS and publish a looping video or another always-running source. See [External hosting and 24/7](network/external-hosting.md).
