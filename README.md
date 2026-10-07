# LuigiScreen

LuigiScreen is a server-side Paper plugin that plays videos, YouTube and Twitch
links, live streams, images and GIFs on walls of Minecraft maps.

Everything is sent as packets, so players need no client mod and nothing is
written into the world. One JAR is all you install.

Author: **unknown_56** · Documentation for `1.3.0-alpha.1`

> LuigiScreen is an alpha project. Back up your server before upgrading and
> test new builds away from production.

## Start here

1. [Install](getting-started/installation.md) the JAR (Paper 1.21.11 or 26.x).
2. [Quick Start](getting-started/quick-start.md): build a screen and play a file or a YouTube link.
3. Open the browser panel with `/screen web`, see [Web Studio](studio/web-studio.md).

## What it can do

| | |
| --- | --- |
| **Play** | Local videos, images and GIFs, image links, YouTube/Twitch/other video sites, RTMP from OBS, MJPEG cameras |
| **Control** | Play now, queue, pause, skip, return; from commands, the browser or an in-game menu |
| **Automate** | Weighted playlists with conditions, events (countdowns, announcements, takeovers), timed automations, screen groups, audience voting |
| **Scale** | Any number of screens; screens with the same source share one decoder; decoding pauses when nobody is near |
| **Stay safe** | One-time web login, per-role permissions, private screens, emergency mode, config history with undo |

Messages in English and Czech.

## Not yet

- No sound in Minecraft (planned).
- No upload of media from the browser; copy files into `plugins/LuigiScreen/media/`.
- LuigiScreen reads a live stream but does not create one: streaming from OBS
  also needs MediaMTX, see [Streaming from OBS](streaming/overview.md).

## Links

- [Source code](https://github.com/unknown-566/LuigiScreen) · [Issues](https://github.com/unknown-566/LuigiScreen/issues)
- [Changelog](reference/changelog.md) · [License and editions](reference/licensing.md)
