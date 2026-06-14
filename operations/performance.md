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
