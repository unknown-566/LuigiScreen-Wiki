# Diagnostic Checklist

Collect this information before reporting a problem.

## Versions

Provide:

```text
Paper version:
Java version:
Operating system:
CPU architecture:
LuigiScreen version:
MapEngine version:
MediaMTX version:
OBS version:
```

## LuigiScreen output

Run:

```text
/screen status
```

Remove any credential that was not masked before sharing.

Enable:

```text
/screen debug
```

Record:

```text
Stream state:
Source resolution:
Screen size:
Input/output FPS:
Render last/average:
TPS/MSPT:
Current error:
```

## Connection test

State whether the MediaMTX reader URL works in VLC.

## Network diagram

Write where each component runs:

```text
OBS:
MediaMTX:
Paper:
Tunnel/VPN:
Public port:
Local MediaMTX port:
```

Do not include passwords.

## Logs

Include:

- LuigiScreen enable section
- First RTMP connection attempt
- MediaMTX listener and connection lines
- The complete Java exception

Do not paste the entire server log unless requested.

## Temporary FFmpeg logging

For additional details:

```yaml
logging:
  ffmpeg-level: info
```

Run `/screen reload`, reproduce once, then return the level to `quiet`.

FFmpeg logging is process-wide and can affect other JavaCV plugins.
