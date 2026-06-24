# Configuration

Configuration file:

```text
plugins/LuigiScreen/config.yml
```

Run `/screen reload` after editing it.

Control Studio also creates:

```text
plugins/LuigiScreen/studio.yml
```

It stores groups, schedules, favorites, statistics, voting settings, audit
history and thumbnail map IDs. It reloads with `/screen reload`.

## Default and generated sections

```yaml
language: cs

updates:
  enabled: true
  project: luigiscreen
  modrinth-url: "https://modrinth.com/plugin/luigiscreen"
  check-interval-hours: 6
  timeout-seconds: 10
  notify-console: true
  notify-players: true
  log-failures: false

stream:
  # Legacy RTMP default and reconnect settings.
  url: "rtmp://127.0.0.1:55556/screen"
  fps: 8
  reconnect-delay-seconds: 3
  reconnect-max-delay-seconds: 30
  io-timeout-seconds: 5

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
  worker-stop-timeout-seconds: 8

logging:
  ffmpeg-level: quiet

debug:
  bossbar-update-ticks: 10
  sidebar-enabled: true
  page-duration-seconds: 2

web-studio:
  enabled: true
  bind: "0.0.0.0"
  port: 8765
  public-url: ""
  server-name: "LuigiScreen Server"
  session-hours: 8
  login-token-minutes: 5
  live-update-millis: 1000
  preview-refresh-millis: 1000
  preview-max-width: 640
```

The old `screen.configured`, `world`, `corner-a`, `corner-b` and `facing`
fields may remain after upgrading. Alpha.8 migrates one valid legacy screen to
`screens.main` once and then uses the new section.

## Update checker

The update checker requests the public version list from
`api.modrinth.com` two seconds after startup and then once per
`updates.check-interval-hours`. HTTP work runs outside the Minecraft server
thread.

`updates.project` accepts the Modrinth project slug or project ID.
`updates.modrinth-url` is the HTTPS page opened when a player clicks the update
message. It does not change the API host.

`updates.notify-console` logs a new public version once.
`updates.notify-players` notifies online players with `luigiscreen.update` once
per detected version and also checks eligible players when they join.

Set `updates.enabled: false` to disable all requests. Network failures are
quiet by default. Set `updates.log-failures: true` while diagnosing outbound
HTTPS problems. A `404` is always ignored because unpublished Modrinth
projects are not visible through the public API.

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

`sources.max-image-bytes` limits the encoded size of one local or URL image.

`sources.max-image-pixels` limits the decoded dimensions of local and remote
images to protect memory from highly compressed oversized files.

See [Media sources](../screen/sources.md) for every type and example.

## Playback settings

`playback.default-duration` is used when a playlist or event item does not set
its own `duration`.

`playback.tick-seconds` controls how often LuigiScreen checks whether a
playlist or event should advance.

The Media Library watches local folders automatically. A debounced background
scan updates files and cached playlist folders after create, modify and delete
events. Use **Rescan Media** in Control Studio for filesystems that do not emit
watch events.

Named `playlists:` and `events:` are configured manually. See
[Playlists and Events](../screen/playlists-events.md).

Playlist anti-repeat settings:

```yaml
history-window: 3
category-history-window: 1
```

Item settings include `enabled`, `weight`, `duration`, `cooldown`,
`category` and `guaranteed-after`.

Supported conditions are `min-online`, `max-online`, `min-viewers`,
`max-viewers`, `viewer-permission`, `all-viewers-permission`,
`tps-above`, `tps-below`, `world` and `days`.

## Studio configuration

Example generated `studio.yml`:

```yaml
history:
  max-entries: 20
media:
  auto-watch: true
voting:
  permission: luigiscreen.vote
  distance: 64
groups:
  global:
    screens: [main]
schedules:
  friday_cinema:
    enabled: true
    days: [FRIDAY]
    time: "20:00"
    target: main
    action: event
    value: cinema_night
    priority: 50
    conflict: priority
```

`statistics`, `audit`, `favorites` and `thumbnail-map-ids` are managed
by the plugin. Do not hand-edit those sections while the server is running.

See [Events, Groups and Schedules](../studio/events-automation.md).

## Web Studio settings

`web-studio.enabled` controls the embedded Java HTTP server. The default bind
address, `0.0.0.0`, accepts same-network connections so `/screen web` works
from another PC on the LAN.

Older configs that still use `127.0.0.1` are upgraded to LAN mode
automatically when an administrator runs `/screen web`.

`public-url` is optional. Set it only when an HTTPS reverse proxy or VPN
provides the address users should open. It does not create HTTPS or expose a
port by itself.

Login links expire after `login-token-minutes` and work once. Authenticated
cookies expire after `session-hours`. `/screen web revoke` invalidates the
issuing player's sessions and pending links immediately.

`live-update-millis` controls lightweight server-sent state updates.
`preview-refresh-millis` and `preview-max-width` limit browser preview work.
Preview capture stops when no Web Studio event connection is open.

See [Web Studio](../studio/web-studio.md) for setup, remote access and the
complete security model.

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

`stream.io-timeout-seconds` limits how long FFmpeg waits for remote network
input. `performance.worker-stop-timeout-seconds` controls how long LuigiScreen
waits for a decoder to close safely. Keep the worker stop timeout higher than
the stream I/O timeout.

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
