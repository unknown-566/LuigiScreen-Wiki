# CGNAT, VPNs and Tunnels

Use this page when you do not have a public IPv4 address.

## Tailscale or ZeroTier

Use:

```text
/screen mediamtx vpn
```

The wizard asks for the VPN address or hostname that the OBS computer uses to reach MediaMTX.

Both relevant computers must join the same VPN network.

This option works best when:

- You control both machines
- You can install the VPN client on both machines
- Your Minecraft hosting provider allows the VPN client

Most managed Minecraft hosts do not allow installing Tailscale or ZeroTier in their server container.

## Playit

Playit can expose a TCP service without router port forwarding.

Conceptually:

```text
OBS -> Playit public TCP address -> Playit agent -> local MediaMTX:55556
```

Important:

- Create a generic TCP tunnel, not a Minecraft-only tunnel.
- Point the local side to the MediaMTX TCP port.
- Use the Playit hostname and assigned public port in OBS.
- The Playit agent and MediaMTX computer must remain running.

Playit solves reachability, not 24/7 hosting.

## Other tunnel options

Other TCP tunnel services can work if they provide:

- Raw TCP forwarding
- A stable hostname and port
- Enough bandwidth for continuous video
- No protocol restriction that rejects RTMP

Free tunnels can change addresses, limit bandwidth, sleep, or disconnect long-running sessions.

## Best choice

For temporary streaming from home without a public IP, use Playit or a VPN.

For reliable 24/7 operation without a home PC, use an external VPS and the `hosting` profile.
