# Configuration

Configuration file:

```text
plugins/LuigiScreen/config.yml
```

Run `/screen reload` after editing it.

## Default and generated sections

```yaml
language: cs

stream:
  # Legacy RTMP default and reconnect settings.
  url: "rtmp://127.0.0.1:55556/screen"
  fps: 8
  reconnect-delay-seconds: 3
  reconnect-max-delay-seconds: 30

source:
  type: rtmp
  value: "rtmp://127.0.0.1:55556/screen"

sources:
  media-directory: media
  allow-absolute-paths: false
  http-timeout-seconds: 10
  max-image-bytes: 16777216
  max-image-pixels: 16777216

playback:
  default-duration: 30s
  tick-seconds: 1

playlists: {}
events: {}

screen:
  auto-start: true
  viewer-distance: 64
  glowing-frames: false
  dithering: false
  max-width: 10
  max-height: 6
  max-total-maps: 60

screens: {}

performance:
  adaptive-fps: true
  max-map-updates-per-second: 400
  minimum-fps: 0.2
  pause-rendering-without-viewers: true
  delta-updates-max-maps: 256

logging:
  ffmpeg-level: quiet

debug:
  bossbar-update-ticks: 10
  sidebar-enabled: true
  page-duration-seconds: 2
```

The old `screen.configured`, `world`, `corner-a`, `corner-b` and `facing`
fields may remain after upgrading. Alpha.8 migrates one valid legacy screen to
`screens.main` once and then uses the new section.

## Per-screen entries

Commands create entries like:

```yaml
screens:
  main:
    source:
      type: video
      value: intro.mp4
    fps: 8.0
    distance: 64.0
    playlist: spawn_rotation
    world: world
    location: "120,80,-45"
    width: 7
    height: 4
    facing: NORTH
    enabled: true
    permission-required: false
```

Every screen has its own:

- `source.type`
- `source.value`
- `fps`
- `distance`
- `playlist`
- `world`
- `location`
- `width`
- `height`
- `enabled`
- `permission-required`

`facing` is also stored because MapEngine needs the wall direction.

`playlist` is optional. If it is absent or empty, the screen plays its
configured `source`. If it is set, the configured source remains the fallback
while playlist items switch at runtime.

`permission-required` defaults to false. If enabled, a player must have
`luigiscreen.see.<screen-name>`, `luigiscreen.see.*` or
`luigiscreen.admin` before the display is spawned for them.

### Source sharing

Screens whose normalized `source.type` and `source.value` are identical share
one loader. They still render independently and may use different FPS,
distance, world, location, width, height and enabled values.

Changing a clone with `/screen source <name> <type> <value>` moves it to
another source group. The old loader remains alive while another enabled
screen still uses it.

Old `url:` entries are automatically migrated to:

```yaml
source:
  type: rtmp
  value: "the old URL"
```

## Global defaults

`source.type`, `source.value`, `stream.fps`, `screen.viewer-distance` and
`screen.auto-start` are defaults for newly created screens and legacy
migration. Existing screens retain their own saved values.

`stream.url` remains as a legacy RTMP value used by the MediaMTX wizard.
Reconnect delays remain global because they control shared remote source
workers.

## Media source settings

`sources.media-directory` selects the folder under
`plugins/LuigiScreen/` used for relative local paths.

`sources.allow-absolute-paths` is false by default so commands cannot select
arbitrary files elsewhere on the server.

`sources.http-timeout-seconds` applies to URL images.

`sources.max-image-bytes` limits the downloaded size of one URL image.

`sources.max-image-pixels` limits the decoded dimensions of local and remote
images to protect memory from highly compressed oversized files.

See [Media sources](../screen/sources.md) for every type and example.

## Playback settings

`playback.default-duration` is used when a playlist or event item does not set
its own `duration`.

`playback.tick-seconds` controls how often LuigiScreen checks whether a
playlist or event should advance.

Named `playlists:` and `events:` are configured manually. See
[Playlists and Events](../screen/playlists-events.md).

## Safety limits

`screen.max-width`, `screen.max-height` and `screen.max-total-maps` are checked
for every individual screen.

The hard limits are 50x50 and 2500 maps, but the defaults of 10x6 and 60 maps
are much safer. Multiple screens add rendering and packet cost even when they
share decoding.

## Performance

Adaptive FPS is calculated independently for each screen. A shared video
source reads at the highest effective FPS requested by its enabled screens.
Slower screens keep only the newest pending shared frame, preventing latency
buildup.

When `pause-rendering-without-viewers` is enabled, an FFmpeg source disconnects
only when none of its enabled screens has a nearby viewer. Static images keep
their already loaded frame without continuous decoding.

## Logging and debug

Allowed FFmpeg levels:

```text
quiet
error
warning
info
debug
```

FFmpeg logging is process-wide. Debug buffer values are estimates and do not
include all native FFmpeg or MapEngine allocations.
