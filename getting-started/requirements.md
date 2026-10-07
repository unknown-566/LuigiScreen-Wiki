# Requirements

## Minecraft server

| Requirement | Supported value |
| --- | --- |
| Server software | Paper |
| Minecraft version | `1.21.11`, `26.1.1`, `26.1.2`, `26.2`, `26.3` |
| Java | `21` for 1.21.11, `25` for 26.x |
| CPU architecture | x86_64 / amd64 |
| Operating system | Windows or Linux |

One LuigiScreen JAR works on every version above. Plain Spigot and
CraftBukkit, Folia, macOS and ARM64 are not supported.

No other plugin is required. Older releases needed MapEngine; it is no longer
used and can be removed.

## Optional tools

| You want to play | You also need |
| --- | --- |
| Files, images, GIFs from `plugins/LuigiScreen/media/` | nothing |
| Image or GIF links | nothing |
| YouTube, Twitch and other video pages | **yt-dlp** — installed with one click in Web Studio → System |
| OBS / live desktop | **MediaMTX** and **OBS Studio** — see [Choose an RTMP network setup](../streaming/overview.md) |
| An IP camera | its direct MJPEG address |

## Network

- Web Studio listens on TCP `8765` (configurable). Keep it inside your LAN or behind a VPN/HTTPS proxy.
- MediaMTX uses TCP `55556` by default, only on the machine that runs it.
- YouTube and other links need outbound HTTPS from the Minecraft server.

## Hosting providers

Managed Minecraft hosts usually only allow plugin JARs. Everything except
MediaMTX runs inside the plugin, so files, images and YouTube links work there.
For OBS, run MediaMTX on a VPS and use `/screen obs hosting`.
