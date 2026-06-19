# Performance

Minecraft maps are expensive video pixels. A larger screen increases server CPU, bandwidth and client rendering work.

## Pixel and map cost

| Screen | Maps | Pixels |
| --- | ---: | ---: |
| 5x3 | 15 | 640x384 |
| 7x4 | 28 | 896x512 |
| 10x6 | 60 | 1280x768 |
| 20x10 | 200 | 2560x1280 |
| 50x50 | 2500 | 6400x6400 |

A 50x50 screen is technically within the hard code limit but is not a sensible public-server configuration.

## Recommended starting point

Use:

```text
7x4 maps
8-10 FPS
896x512 OBS output
1200-2000 Kbps
64-block viewer distance
```

## Adaptive FPS

With the default budget:

```yaml
max-map-updates-per-second: 400
```

A 60-map screen is limited to approximately:

```text
400 / 60 = 6.67 FPS
```

## Viewer pause

Keep this enabled:

```yaml
pause-rendering-without-viewers: true
```

It closes the FFmpeg reader while no players are near the screen, reducing idle CPU and network use.

## Latest-frame behavior

LuigiScreen stores only one pending frame. Replaced-frame counts are normal when the input arrives faster than map rendering.

A growing delay is not expected. The plugin drops waiting frames instead.

## What alpha.14 reuses

LuigiScreen avoids rebuilding large helper objects for every frame:

- Each screen keeps one reusable image view over its MapEngine pixel buffer.
- A delta comparison buffer is allocated once and updated in place.
- Player positions are captured once per viewer refresh and shared by all screens.
- Screens using the same source still share one decoder or image loader.
- Playlist folders are scanned during startup or `/screen reload`, not while an item is selected.

This mainly reduces garbage collection pressure and main-thread disk work. It
does not make map packets free: every visible screen must still convert and
send its own map updates.

## Safe decoder shutdown

Keep the worker stop timeout above the remote I/O timeout:

```yaml
stream:
  io-timeout-seconds: 5

performance:
  worker-stop-timeout-seconds: 8
```

This gives FFmpeg enough time to leave a blocked network read before reload or
shutdown continues.

## Dithering

Leave dithering disabled unless the visual improvement is worth extra conversion cost.

## Diagnose lag

Use:

```text
/screen debug
```

Watch:

- TPS below `18`
- MSPT approaching or exceeding `50`
- Render time exceeding the frame budget
- Very high replaced-frame rate
- CPU near saturation
- Heap continuously approaching its maximum

Reduce screen size or FPS before increasing server resources.
