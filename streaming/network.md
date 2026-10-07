# Network Setups

Pick the situation that matches where Paper, MediaMTX and OBS run, and run
its command in game. The wizard asks only for what it cannot detect, writes
`mediamtx.yml` and a private `setup.txt` into
`plugins/LuigiScreen/mediamtx/<SITUATION>/`, and prints the OBS server URL.

| Situation | Command |
| --- | --- |
| All on one computer | `/screen obs same-pc`, see [the overview](overview.md#same-computer-step-by-step) |
| OBS on another PC in the same home network | `/screen obs lan` |
| OBS from outside, MediaMTX at home | `/screen obs internet` |
| No public IP: VPN | `/screen obs vpn` |
| MediaMTX on a VPS or other host | `/screen obs hosting` |

## Local network (`lan`)

Paper and MediaMTX run on one computer, OBS on another in the same network.

- If several LAN addresses are detected, enter the one on the OBS computer's network (e.g. `192.168.1.64`).
- Allow inbound TCP `55556` on the MediaMTX computer. No router forwarding is needed.
- Reserve that computer's address in the router's DHCP settings, or the OBS URL breaks when it changes.

## Public internet (`internet`)

MediaMTX runs at home; OBS connects from elsewhere. You need a real public IPv4
(not carrier-grade NAT) and router access.

The wizard asks for the MediaMTX computer's LAN address (if needed), your
public IP or DNS name, and the public port. Then forward it:

```text
Public TCP 40000 -> 192.168.1.50 TCP 55556
```

Allow TCP `55556` in the MediaMTX computer's firewall. Test from another
network such as mobile data; many routers cannot reach their own public
address from inside.

## VPN and tunnels (`vpn`)

Without a public IP, use Tailscale or ZeroTier on both the OBS and MediaMTX
computers and run `/screen obs vpn` with the VPN address of the MediaMTX
computer. Most managed Minecraft hosts do not allow VPN clients, so this
suits a home or VPS server.

[Playit](https://playit.gg) or another raw-TCP tunnel also works: create a
generic TCP tunnel to local port `55556` and use its hostname and port in OBS.
Free tunnels may change addresses, limit bandwidth or disconnect long
sessions. A tunnel solves reachability, not uptime: your PC must stay on.

## External hosting and 24/7 (`hosting`)

Recommended for managed hosts such as Minekeep:

```mermaid
flowchart LR
    A["OBS or looping video"] -->|publish| B["MediaMTX on VPS"]
    B -->|read-only RTMP| C["Paper host with LuigiScreen"]
```

1. Run `/screen obs hosting` and enter the VPS's public IP/hostname and port (`default` = `55556`).
2. Upload `plugins/LuigiScreen/mediamtx/HOSTING/mediamtx.yml` next to MediaMTX on the VPS and restart it.
3. Open TCP `55556` in the VPS and provider firewall. The Minecraft host only needs outbound TCP.

Two accounts are generated: `streamer` can only publish (for OBS),
`luigiscreen` can only read (for the plugin).

**Without your PC:** something must still publish video. Either loop a file on
the VPS:

```bash
ffmpeg -re -stream_loop -1 -i video.mp4 -c copy -f flv "RTMP_PUBLISH_URL"
```

(H.264/AAC; re-encode if stream copy fails), or skip MediaMTX entirely and play
a local video or a YouTube link on the screen.

## Security

- Use only the generated config; never expose a default MediaMTX without authentication.
- Keep `setup.txt`, `mediamtx.yml` and `config.yml` private.
- Running the wizard again creates new passwords and backs up the old files. Do
  that after a leak, then update OBS.
