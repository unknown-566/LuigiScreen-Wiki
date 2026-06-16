# Playlists and Events

LuigiScreen can play more than one media source on the same screen.

There are two playback layers:

- **Playlist**: normal rotation for a screen.
- **Event**: manual sequence that interrupts the playlist, then returns to it.

The screen's configured `source.type` and `source.value` remain the fallback.
Playlist and event switches are runtime-only, so normal rotation does not keep
rewriting `config.yml`.

## Commands

```text
/screen playlist list
/screen playlist set <screen> <playlist>
/screen playlist clear <screen>

/screen event list
/screen event play <screen> <event>
/screen event stop <screen>
```

## Playlist example

```yaml
playlists:
  spawn_rotation:
    default-duration: 30s
    items:
      live_stream:
        type: rtmp
        value: "rtmp://127.0.0.1:55556/screen"
        weight: 10
        duration: 60s

      random_trailer:
        type: folder
        folder: trailers
        media-types: [video, image, gif]
        weight: 3
        duration: 20s
        cooldown: 2m
        conditions:
          min-viewers: 1

      poster:
        type: image
        value: posters/server.png
        weight: 1
        duration: 15s
```

Enable it:

```text
/screen playlist set main spawn_rotation
```

## Event example

```yaml
events:
  update_reveal:
    priority: 50
    sequence:
      title:
        type: text
        text: "Update starts soon"
        duration: 5s

      trailer:
        type: video
        value: trailers/update.mp4
        duration: 30s

      live:
        type: rtmp
        value: "rtmp://127.0.0.1:55556/screen"
        duration: 2m
```

Play it:

```text
/screen event play main update_reveal
```

Stop it early:

```text
/screen event stop main
```

## Item types

Playlist and event items support these `type` values:

| Type | Value |
| --- | --- |
| `rtmp` | RTMP or RTMPS URL |
| `mjpeg` | HTTP(S) MJPEG URL |
| `video` | Local file under `plugins/LuigiScreen/media/` |
| `image` | Local image under `plugins/LuigiScreen/media/` |
| `url-image` | HTTP(S) image URL |
| `gif` | Local or HTTP(S) GIF |
| `folder` | Random file from a media folder |
| `text` | Internal LuigiScreen text frame |
| `countdown` | Internal LuigiScreen text frame for now |

Folder items currently detect:

- `mp4`, `mov`, `mkv`, `webm` as `video`
- `png`, `jpg`, `jpeg`, `webp` as `image`
- `gif` as `gif`

## Timing

Durations and cooldowns accept:

```text
250ms
10s
5m
1h
```

A plain number such as `30` means 30 seconds.

## Conditions

Items can include a `conditions:` block:

```yaml
conditions:
  min-online: 3
  min-viewers: 1
  viewer-permission: luigiscreen.special.viewer
  tps-above: 18.5
```

Meaning:

| Condition | Meaning |
| --- | --- |
| `min-online` | At least this many players are online |
| `min-viewers` | At least this many players can currently see the screen |
| `viewer-permission` | At least one current viewer has this permission |
| `tps-above` | Server TPS must be above this value |

## Sharing loaders

If two screens are playing the same active source type and value, LuigiScreen
uses one loader and sends the latest frame to both screens.

This also applies when a clone uses the same playlist and both screens happen
to select the same item.

## Notes

- Playlist rotation is checked once per `playback.tick-seconds`.
- Events are manual in this alpha. Automatic event triggers can be added later.
- Text and countdown items render an internal LuigiScreen frame and do not need
  a media file.
- Audio is not sent to Minecraft clients.
