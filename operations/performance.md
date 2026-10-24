# Performance and Debug

Map screens are expensive pixels: every visible map costs bandwidth for every
nearby player. Size and FPS matter far more than anything else.

## Screen cost

| Screen | Maps | Pixels |
| --- | ---: | ---: |
| 4×3 | 12 | 512×384 |
| 7×4 | 28 | 896×512 |
| 10×6 | 60 | 1280×768 |
| 16×9 | 144 | 2048×1152 |
| 32×18 | 576 | 4096×2304 |

Starting point for a public server: 4×3 to 8×5 maps, 20 FPS, 64-block
viewing distance. 60 FPS on a big screen looks great but sends a lot of data to
every viewer, so test it with `/screen debug` first.

## What LuigiScreen already saves

- Only the changed rectangle of each map is sent, so static parts of a video cost nothing.
- *Colour stability* (`screen.color-stability`, default 4) ignores tiny colour
  changes, so video noise does not force updates. Raise it on noisy sources.
- Screens with the same source share one decoder.
- FFmpeg scales the video to the largest screen using it, not full resolution.
- Decoding pauses when nobody is near (`performance.pause-rendering-without-viewers`).
- A player whose connection is full skips frames instead of building lag.

## Adaptive FPS

With `performance.adaptive-fps: true`, a screen lowers its FPS when it would
send more than `max-map-updates-per-second` (default 600) per viewer. Only
pixels that actually change are counted, so a quarter-changed map counts as
0.25. A calm video keeps its full FPS; a 144-map screen where everything
changes every frame drops to about 600 / 144 ≈ 4 FPS. Both settings are in
Web Studio → System → Picture quality.

## Dithering

`screen.dithering` has three modes:

- `floyd` (default): error diffusion, the most accurate colours. Pixels whose
  source didn't change keep their colour, so video doesn't shimmer and still
  areas cost no bandwidth.
- `ordered`: a fixed grid pattern, a little cheaper on CPU.
- `off`: nearest colour only, visible colour bands on gradients.

## HD colours

Players with the [HD colours](../screen/hd-colors.md) pack get separately
encoded map data. A screen watched by players with and without the pack does
both conversions, a few milliseconds more per frame.

## Web Studio

Previews are captured only while a browser is open and are downscaled and rate
limited. On a small host set `web-studio.live-update-millis` and
`preview-refresh-millis` to `2000` and `preview-max-width` to `480`.

## Debug overlay

```text
/screen debug
```

Toggles a personal boss bar and a 15-line sidebar. The boss bar cycles through
stream state and source resolution; screens, viewers and FPS; received,
rendered and replaced frames; render time; memory; CPU and threads; TPS,
MSPT and GC; reconnects and errors.

Replaced frames are normal: only the newest frame is kept when decoding is
faster than drawing. Memory values cover Java image buffers, not native FFmpeg
memory.

Your previous scoreboard is restored afterwards. If the sidebar conflicts with
another plugin, set `debug.sidebar-enabled: false` and `/screen reload`.

## Finding lag

Watch for TPS below 18, MSPT near 50, render time above the frame budget, or
CPU near 100 %. Make screens smaller or lower FPS before adding hardware.

Keep `performance.worker-stop-timeout-seconds` (8) above
`stream.io-timeout-seconds` (5) so a stuck network read can end before reload
or shutdown continues.
