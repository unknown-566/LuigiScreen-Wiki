# Requirements

Check every requirement before installing LuigiScreen.

## Minecraft server

| Requirement | Supported value |
| --- | --- |
| Server software | Paper |
| Minecraft version | `1.21.11` |
| Java | `21` |
| CPU architecture | x86_64 / amd64 |
| Operating system | Windows or Linux |

LuigiScreen is distributed as a Paper/Bukkit plugin, but its current build
targets Paper APIs. Plain Spigot and CraftBukkit servers, Folia, macOS, ARM64
and older Minecraft versions are not currently supported.

## Required plugin

Install MapEngine `1.8.12` before starting the server.

LuigiScreen declares MapEngine as a required dependency. If MapEngine is missing or incompatible, Paper will not enable LuigiScreen correctly.

## Streaming software

You need:

- [MediaMTX](https://mediamtx.org/) as the media server
- [OBS Studio](https://obsproject.com/) or another RTMP publisher

MediaMTX can run:

- On the Minecraft server computer
- On another computer in the same LAN
- On a computer reachable through a public IP
- Across a VPN such as Tailscale or ZeroTier
- On an external VPS

## Network requirements

The default LuigiScreen MediaMTX port is TCP `55556`.

Only the machine that runs MediaMTX listens on this port. Do not open or forward the port on unrelated computers.

## Hosting-provider warning

Minecraft hosting panels usually allow only plugin JAR files and Minecraft ports. They often do not allow running `mediamtx` or Docker beside the server.

In that situation:

1. Run MediaMTX on a VPS or another computer.
2. Use `/screen mediamtx hosting`.
3. Configure LuigiScreen to read the remote RTMP stream.
