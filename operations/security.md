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

Web Studio listens on `127.0.0.1:8765` by default. This loopback-only default
is intentional. Do not bind it directly to a public interface without a
carefully configured firewall and HTTPS reverse proxy.

Authentication uses a short-lived, one-time URL created by `/screen web`.
After it is consumed, LuigiScreen stores an HttpOnly, SameSite session cookie.
State-changing requests also require a matching CSRF token and origin.

The session contains only the LuigiScreen capabilities of the player who
created the link. It does not inherit later permission changes. Use:

```text
/screen web revoke
```

after staff changes, a leaked link or a shared-browser session. It revokes the
issuing player's links and sessions, not other administrators.

For remote access, keep the plugin bound to loopback and configure
`web-studio.public-url` to the HTTPS address provided by your reverse proxy or
authenticated VPN. `public-url` is presentation metadata, not a security
layer.

Never share one-time login URLs, session cookies, browser storage or screenshots
containing authenticated source URLs.
