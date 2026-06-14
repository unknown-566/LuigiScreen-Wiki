# LuigiScreen

LuigiScreen is a Paper plugin that displays a live RTMP video stream on a wall of Minecraft maps.

It connects **OBS Studio** to **MediaMTX**, decodes the stream with FFmpeg, and renders the latest video frame through **MapEngine**.

> LuigiScreen is currently an alpha project. Back up your server before upgrading and test new builds away from production.

## Current release

Documentation version: `1.1.0-alpha.7`

Supported server platforms:

- Paper `1.21.11`
- Java `21`
- Windows x86_64
- Linux x86_64

Required plugin:

- MapEngine `1.8.12`

## Start here

If everything runs on one computer, follow the [Quick start](getting-started/quick-start.md).

If the Minecraft server is hosted elsewhere, first read [Choose a network setup](streaming/overview.md).

If you prefer Czech, use the [Český rychlý start](czech/quick-start.md).

## What LuigiScreen provides

- Configurable map screens up to the safety limits in `config.yml`
- RTMP input from MediaMTX
- Guided MediaMTX setup for five network situations
- Automatic reconnect with exponential backoff
- Viewer-distance based pause and resume
- Adaptive FPS for large screens
- Localized Czech and English messages
- Performance boss bar and 15-line debug sidebar
- Masked RTMP credentials in status output and logs

## Important limitation

LuigiScreen displays a stream; it does not create one by itself. MediaMTX must be running somewhere, and OBS or another publisher must send video to it. If the publishing computer is turned off, live desktop capture stops.

For a permanent 24/7 source, use an external VPS and publish a looping video or another always-running source. See [External hosting and 24/7](network/external-hosting.md).
