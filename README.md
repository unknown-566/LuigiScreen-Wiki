# LuigiScreen

LuigiScreen is a server-side Paper plugin that plays videos, YouTube and Twitch
links, live streams, images and GIFs on walls of Minecraft maps.

Everything is sent as packets, so players need no client mod and nothing is
written into the world. One JAR is all you install.

Author: **unknown_56**

> LuigiScreen is an alpha project. Back up your server before upgrading and test new builds away from production.

## Current release

Documentation version: `1.3.0-alpha.1`

| Server | Java |
| --- | --- |
| Paper `1.21.11` | 21 |
| Paper `26.1.1`, `26.1.2`, `26.2`, `26.3` | 25 |

Windows x86_64 and Linux x86_64. No other plugin is required.

Project links:

- [Free source code](https://github.com/unknown-566/LuigiScreen)
- [Issue tracker](https://github.com/unknown-566/LuigiScreen/issues)
- [License and editions](reference/licensing.md)

## Start here

1. [Install](getting-started/installation.md) the JAR.
2. Follow the [Quick Start](getting-started/quick-start.md): build a screen and play a file or a YouTube link in two commands.
3. Open the browser control panel with `/screen web` — see [Web Studio](studio/web-studio.md).

If you prefer Czech, use the [Český rychlý start](czech/quick-start.md).

## What LuigiScreen does

- **Plays almost anything**: local videos, images and GIFs, YouTube/Twitch/other video links (through yt-dlp), RTMP from OBS and MJPEG cameras
- **Runs on its own**: weighted playlists, events (countdowns, announcements, takeovers), time-based automations and screen groups
- **Easy to operate**: one browser panel (`/screen web`) and short commands like `/screen play lobby intro.mp4`
- **Light on the server**: one decoder per source even when many screens show it, decoding pauses when nobody is nearby, frames are scaled inside FFmpeg, and only the changed part of each map is sent
- **Safe**: one-time web login links, per-role permissions, optional per-screen visibility, emergency mode
- Czech and English messages

## Not yet

- No sound in Minecraft yet.
- LuigiScreen reads a live stream; it does not create one. For OBS you also
  need MediaMTX — see [Choose an RTMP network setup](streaming/overview.md).
