# Configuration

Configuration file:

```text
plugins/LuigiScreen/config.yml
```

Run `/screen reload` after editing it.

## Complete default configuration

```yaml
language: cs

stream:
  url: "rtmp://127.0.0.1:55556/screen"
  fps: 8
  reconnect-delay-seconds: 3
  reconnect-max-delay-seconds: 30

screen:
  auto-start: true
  viewer-distance: 64
  glowing-frames: false
  dithering: false
  max-width: 10
  max-height: 6
  max-total-maps: 60
  configured: false
  world: ""
  corner-a: ""
  corner-b: ""
  facing: "NORTH"

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

## Language

### `language`

Selected message file code.

```yaml
language: en
```

Loads `messages_en.yml`.

## Stream

### `stream.url`

RTMP reader URL used by LuigiScreen.

Prefer `/screen mediamtx <situation>` instead of writing credentials manually.

### `stream.fps`

Requested render FPS, clamped internally between `0.1` and `20`.

### `stream.reconnect-delay-seconds`

Initial reconnect delay after stream failure.

### `stream.reconnect-max-delay-seconds`

Maximum exponential reconnect delay.

## Screen

### `screen.auto-start`

Starts the decoder automatically when a saved display loads.

### `screen.viewer-distance`

Maximum player distance from the display center for receiving map updates.

### `screen.glowing-frames`

Controls MapEngine glowing display frames.

### `screen.dithering`

Enables Floyd-Steinberg color conversion. It can improve gradients but costs additional CPU.

### `screen.max-width`

Maximum allowed map width. Hard-clamped to `50`.

### `screen.max-height`

Maximum allowed map height. Hard-clamped to `50`.

### `screen.max-total-maps`

Maximum width multiplied by height. Hard-clamped to `2500`.

The default `60` is much safer than the hard maximum.

### Saved screen fields

These fields are managed automatically:

```yaml
configured: false
world: ""
corner-a: ""
corner-b: ""
facing: "NORTH"
```

Do not edit them unless repairing a known configuration problem.

## Performance

### `performance.adaptive-fps`

Reduces effective FPS when the screen would exceed the update budget.

### `performance.max-map-updates-per-second`

Approximate budget used by adaptive FPS.

Effective FPS is limited by:

```text
max-map-updates-per-second / total maps
```

### `performance.minimum-fps`

Lowest FPS adaptive mode can select.

### `performance.pause-rendering-without-viewers`

Disconnects the RTMP decoder when nobody is near the screen.

### `performance.delta-updates-max-maps`

Screens at or below this map count may keep an additional previous-frame buffer for delta updates.

## Logging

### `logging.ffmpeg-level`

Allowed values:

```text
quiet
error
warning
info
debug
```

JavaCV's FFmpeg log level is process-wide and can affect another plugin using the same native FFmpeg process.

## Debug

### `debug.bossbar-update-ticks`

Boss bar and sidebar refresh interval. The minimum is five ticks.

### `debug.sidebar-enabled`

Controls whether `/screen debug` also installs a personal sidebar.

### `debug.page-duration-seconds`

Number of seconds each boss bar statistics page remains visible.
