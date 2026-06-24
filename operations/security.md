# Security

LuigiScreen generates credentials because an exposed RTMP publisher can replace the content shown on your Minecraft screen.

## Never publish secrets

Keep these private:

```text
plugins/LuigiScreen/config.yml
plugins/LuigiScreen/mediamtx/*/mediamtx.yml
plugins/LuigiScreen/mediamtx/*/setup.txt
```

Do not include them in:

- GitHub repositories
- Discord screenshots
- Public support logs
- Modrinth source archives

## Generated permissions

The generated MediaMTX users are restricted:

| User | Permission |
| --- | --- |
| `streamer` | Publish only to `screen` |
| `luigiscreen` | Read only from `screen`, external hosting only |
| Loopback user | Local read and disabled-service administration permissions |

## URL masking

LuigiScreen masks passwords, tokens and path-based stream keys in its status,
connection logs and error messages.

Other software, crash reports or hosting panels can still print complete connection strings. Inspect logs before sharing them.

## Modrinth update requests

When enabled, LuigiScreen makes an outbound HTTPS GET request only to:

```text
https://api.modrinth.com/v2/project/<project>/version
```

The request contains the configured public project slug or ID and a LuigiScreen
user-agent. It does not send player names, server addresses, stream URLs,
credentials or screen configuration.

Disable it with:

```yaml
updates:
  enabled: false
```

## Internet exposure

When exposing MediaMTX:

- Open only the required TCP port
- Keep unused MediaMTX protocols disabled
- Use generated authentication
- Rotate credentials after accidental disclosure
- Restrict firewall source addresses when practical

## Credential rotation

Run the matching wizard again:

```text
/screen mediamtx hosting
```

This creates new credentials and timestamped backups. Replace the MediaMTX config and update OBS with the newly generated URL.

Delete obsolete backups after confirming the new setup.

## Web Studio

Web Studio listens on TCP port `8765` and is LAN-ready by default. This makes
same-network setup simple, but it is still an admin surface. Do not expose
port `8765` directly to the public internet.

Authentication uses a short-lived, one-time URL created by `/screen web`.
After it is consumed, LuigiScreen stores an HttpOnly, SameSite session cookie.
State-changing requests also require a matching CSRF token and origin.

When `/screen web` runs on an older localhost-only config, LuigiScreen switches
the web listener to LAN mode automatically and prints a LAN login link plus a
server-PC fallback link.

The session contains only the LuigiScreen capabilities of the player who
created the link. It does not inherit later permission changes. Use:

```text
/screen web revoke
```

after staff changes, a leaked link or a shared-browser session. It revokes the
issuing player's links and sessions, not other administrators.

For remote access outside the LAN, use an authenticated VPN or an HTTPS reverse
proxy. Configure `web-studio.public-url` to the address users should open.
`public-url` is presentation metadata, not a security layer.

Never share one-time login URLs, session cookies, browser storage or screenshots
containing authenticated source URLs.
