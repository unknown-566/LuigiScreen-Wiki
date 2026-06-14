# Choose a Network Setup

Run the setup command matching where Minecraft, MediaMTX and OBS are located.

| Situation | Command | Use when |
| --- | --- | --- |
| Same computer | `/screen mediamtx same-pc` | Paper, MediaMTX and OBS run on one computer |
| Local network | `/screen mediamtx lan` | Paper and MediaMTX share a computer; OBS is elsewhere in the same LAN |
| Public internet | `/screen mediamtx internet` | OBS reaches home MediaMTX through router port forwarding |
| VPN | `/screen mediamtx vpn` | OBS reaches MediaMTX through Tailscale or ZeroTier |
| External hosting | `/screen mediamtx hosting` | MediaMTX runs on a public VPS or another hosting machine |

## What the wizard does

The wizard:

1. Asks only for values it cannot detect.
2. Cancels wizard chat messages so they are not broadcast as normal chat.
3. Generates strong random credentials.
4. Creates a restricted MediaMTX configuration.
5. Creates a private `setup.txt`.
6. Updates LuigiScreen's `stream.url`.
7. Safely switches the RTMP decoder in the background.

Generated files are stored under:

```text
plugins/LuigiScreen/mediamtx/<SITUATION>/
```

Existing generated files are copied to timestamped backups before replacement.

## Important distinction

A tunnel solves network reachability. It does not keep MediaMTX, OBS or your source computer running.

If you want a source available without your home PC, use [External hosting and 24/7](../network/external-hosting.md).
