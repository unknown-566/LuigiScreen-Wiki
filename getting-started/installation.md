# Installation

## Requirements

| | Supported |
| --- | --- |
| Server | Paper `1.21.11`, `26.1.1`, `26.1.2`, `26.2`, `26.3` |
| Java | `21` for 1.21.11, `25` for 26.x |
| System | Windows or Linux, x86_64 / amd64 |

One JAR works on every version above. No other plugin is needed:
PacketEvents is bundled inside, and MapEngine is no longer used.

Not supported: Spigot/CraftBukkit, Folia, macOS, ARM64 (FFmpeg natives are
x86_64 only).

What you need besides the plugin depends on what you want to play:

| You want to play | You also need |
| --- | --- |
| Files, images, GIFs, image links | nothing |
| YouTube, Twitch and other video sites | **yt-dlp**, one click in Web Studio → System |
| OBS / live desktop | **MediaMTX** and **OBS Studio**, see [Streaming from OBS](../streaming/overview.md) |
| An IP camera | its direct MJPEG address |

## Install

1. Stop the server. Never hot-swap the JAR: FFmpeg libraries stay loaded until Java exits.
2. Put `LuigiScreen-1.3.0-alpha.1.jar` into `plugins/`. Keep only one LuigiScreen JAR.
3. Start the server.

The first start creates:

```text
plugins/LuigiScreen/config.yml
plugins/LuigiScreen/messages_en.yml
plugins/LuigiScreen/messages_cs.yml
plugins/LuigiScreen/media/
```

The console should show:

```text
[packetevents] Loaded packetevents v2.14.0 for LuigiScreen
[LuigiScreen] LuigiScreen Studio listening on 0.0.0.0:8765
```

Continue with the [Quick Start](quick-start.md).

## Language

English by default. For Czech set `language: cs` in `config.yml` and run
`/screen reload`. See [Localization](../reference/localization.md).

## Ports

| Port | Used by | Open it? |
| --- | --- | --- |
| TCP `8765` | Web Studio | Only inside your LAN (Windows Firewall: allow Java) |
| TCP `55556` | MediaMTX, only if you stream from OBS | Depends on the [network setup](../streaming/network.md) |

YouTube links also need outbound HTTPS from the server.

## Hosting providers

Managed hosts usually only allow plugin JARs. That is enough for files, image
links and YouTube. The host must allow native library extraction to a temp
folder and, for YouTube, running the downloaded yt-dlp from
`plugins/LuigiScreen/bin/`. For OBS, run MediaMTX on a VPS and use
`/screen obs hosting`.

## Updating

1. Stop the server and back up `plugins/LuigiScreen/`.
2. Replace the JAR.
3. Start the server and run `/screen list`.

Screens, playlists, events and automations are kept. Coming from 1.2 or
older, you can delete `MapEngine.jar`; old command names still work (see
[Commands](../reference/commands.md)). Customized `messages_*.yml` files fall
back to the bundled text for new messages; delete them to get the new wording
everywhere.
