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

LuigiScreen masks passwords, tokens and path-based stream keys in its own status and logs.

Other software, crash reports or hosting panels can still print complete connection strings. Inspect logs before sharing them.

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
