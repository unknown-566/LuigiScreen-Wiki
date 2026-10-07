# Playlists

A playlist is a weighted rotation: the screen keeps picking the next item at
random, favouring items with a higher weight.

```text
trailer → poster → live stream → trailer → …
```

## In Web Studio

1. **Program → Playlists**, create a playlist (it starts empty).
2. Add items: media file, duration and weight. The *Chance* column shows how
   often each item will play.
3. Assign it to a screen: on the screen **Play… → Playlist**, or
   `/screen playlist <screen> <name>`.

You can also press **Add to playlist** on a file in **Media**. Deleting a
playlist clears it from the screens that used it.

`/screen playlist <screen> off` stops using the playlist; the screen goes back
to its default source.

## In `config.yml`

```yaml
playlists:
  spawn_rotation:
    history-window: 3
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
      poster:
        type: image
        value: posters/server.png
        weight: 1
        duration: 15s
```

Run `/screen reload` after editing the file. Paths are relative to
`plugins/LuigiScreen/media/`.

## Item settings

| Setting | Meaning |
| --- | --- |
| `type`, `value` | what to play, see [What you can play](sources.md) |
| `weight` | chance tickets: weight 10 vs 1 means the first wins about 10 of 11 picks |
| `duration` | how long it stays; `250ms`, `10s`, `5m`, `1h`, or a plain number of seconds |
| `enabled` | `false` skips the item |
| `cooldown` | do not pick this item again for this long |
| `category` | group related items, e.g. `advertisement` |
| `guaranteed-after` | pick this item once it has not played for this long |
| `conditions` | only play when these pass (below) |

Without `duration`, `playback.default-duration` (30 s) is used.

## Item types

Besides normal media (`video`, `image`, `gif`, `url-image`, `youtube`, `rtmp`,
`mjpeg`):

| Type | Fields | Plays |
| --- | --- | --- |
| `folder` | `folder`, optional `media-types` | one random supported file from that folder |
| `text` | `text` | a text card |
| `countdown` | `text` | a text card (no live counter yet) |

Folder contents are refreshed by the media library watcher. An empty folder is
skipped.

## Anti-repeat

| Playlist setting | Meaning |
| --- | --- |
| `history-window` | avoid the last N picked items when something else is possible |
| `category-history-window` | avoid recently used categories |

If anti-repeat would leave nothing to play, it is relaxed instead of leaving
the screen blank.

## Conditions

```yaml
      special_clip:
        type: video
        value: special.mp4
        duration: 30s
        conditions:
          min-viewers: 1
          tps-above: 18.5
          days: [FRIDAY, SATURDAY]
```

| Condition | Passes when |
| --- | --- |
| `min-online`, `max-online` | online player count is in range |
| `min-viewers`, `max-viewers` | players currently seeing the screen are in range |
| `viewer-permission` | at least one viewer has this permission |
| `all-viewers-permission` | every viewer has this permission |
| `tps-above`, `tps-below` | server TPS is in range |
| `world` | the screen is in this world |
| `days` | today is one of these weekdays |

The in-game Control Studio shows, for each item, its chance, cooldown and why
it is or is not eligible right now, and has a condition builder. See
[In-game Control Studio](../studio/control-studio.md).

## Common mistakes

| Problem | Check |
| --- | --- |
| Playlist does nothing | Did you `/screen reload` after editing? Is it assigned? Is the screen on? |
| Local file does not play | `value: trailers/update.mp4` must exist as `media/trailers/update.mp4` |
| Folder item skipped | Folder exists, has supported files, `media-types` allows them |
| One item repeats too often | Lower its `weight`, add a `cooldown` or `history-window` |

Two screens that pick the same item at the same time share one decoder.
