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
