# Playlists and Events

The in-game editor and runtime controls are documented in
[Playlist Studio and Conditions](../studio/playlists-conditions.md) and
[Events, Groups and Schedules](../studio/events-automation.md).

LuigiScreen does not have to show only one source forever.

With playlists, one screen can rotate between several sources:

```text
RTMP stream -> random trailer -> poster image -> RTMP stream -> ...
```

With events, one screen can temporarily interrupt that normal rotation:

```text
normal playlist -> update announcement event -> normal playlist
```

## The Mental Model

Every screen has two source concepts:

| Concept | What it means |
| --- | --- |
| Configured source | The normal saved source in `screens.<name>.source` |
| Active source | What the screen is playing right now |

The configured source is the fallback. It stays saved in `config.yml`.

Playlist and event items only change the active source at runtime. That means a
playlist can rotate through files and streams without rewriting the screen's
saved source every few seconds.

When you clear the playlist, LuigiScreen returns the screen to its configured
source.

## What A Playlist Does

A playlist is a named rotation in `config.yml`.

Example:

```yaml
playlists:
  spawn_rotation:
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

This creates a playlist called `spawn_rotation`.

It has three possible items:

| Item | What happens |
| --- | --- |
| `live_stream` | Plays the RTMP stream for 60 seconds |
| `random_trailer` | Picks one random file from `media/trailers/` and plays it for 20 seconds |
| `poster` | Shows `media/posters/server.png` for 15 seconds |

Enable it on a screen:

```text
/screen reload
/screen playlist set main spawn_rotation
```

Disable it:

```text
/screen playlist clear main
```

## Step By Step Setup

### 1. Create or choose a screen

```text
/screen create main 7 4
```

### 2. Put local media into the media folder

Relative paths start inside:

```text
plugins/LuigiScreen/media/
```

For the example above, this file:

```yaml
value: posters/server.png
```

means:

```text
plugins/LuigiScreen/media/posters/server.png
```

And this folder:

```yaml
folder: trailers
```

means:

```text
plugins/LuigiScreen/media/trailers/
```

Example folder contents:

```text
plugins/LuigiScreen/media/trailers/intro.mp4
plugins/LuigiScreen/media/trailers/update.gif
plugins/LuigiScreen/media/trailers/poster.png
```

### 3. Add the playlist to `config.yml`

Put the playlist under the top-level `playlists:` section.

```yaml
playlists:
  spawn_rotation:
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
```

### 4. Reload LuigiScreen

```text
/screen reload
```

### 5. Assign the playlist to a screen

```text
/screen playlist set main spawn_rotation
```

Now `main` is controlled by the playlist.

## Weight Explained

`weight` controls how likely an item is to be picked.

```yaml
live_stream:
  weight: 10

poster:
  weight: 1
```

This does not mean "play stream exactly 10 times, then poster once."

It means the stream has a much higher chance to be selected whenever the
playlist chooses the next item.

Simple way to think about it:

| Item | Weight | Rough chance |
| --- | --- | --- |
| `live_stream` | 10 | 10 tickets |
| `poster` | 1 | 1 ticket |

Total: 11 tickets.

The stream owns 10 of them, so it will usually appear much more often.

## Duration Explained

`duration` says how long that item stays active.

```yaml
duration: 20s
```

Supported formats:

```text
250ms
10s
5m
1h
```

A plain number means seconds:

```yaml
duration: 30
```

That means 30 seconds.

## Cooldown Explained

`cooldown` stops one item from being selected again too soon.

```yaml
random_trailer:
  type: folder
  folder: trailers
  duration: 20s
  cooldown: 2m
```

After `random_trailer` is selected, LuigiScreen will avoid selecting that same
playlist item again for 2 minutes.

Cooldown is useful for:

- random trailer folders
- rare announcement images
- special videos that should not repeat constantly

## Folder Items

`type: folder` selects one random media file from a folder.

```yaml
random_trailer:
  type: folder
  folder: trailers
  media-types: [video, image, gif]
  duration: 20s
```

Folder items currently detect:

| Extension | Treated as |
| --- | --- |
| `mp4`, `mov`, `mkv`, `webm` | `video` |
| `png`, `jpg`, `jpeg`, `webp` | `image` |
| `gif` | `gif` |

If the folder is empty or contains unsupported files, that item is skipped.

Folder contents are cached outside normal playback. The Media Library watcher
refreshes them after local file changes, so a normal add/remove/rename no
longer requires `/screen reload`.

## Conditions

Conditions decide whether an item is allowed to play.

```yaml
special_clip:
  type: video
  value: special.mp4
  duration: 30s
  conditions:
    min-online: 3
    min-viewers: 1
    viewer-permission: luigiscreen.special.viewer
    tps-above: 18.5
```

Meaning:

| Condition | Meaning |
| --- | --- |
| `min-online` | At least this many players must be online |
| `max-online` | At most this many players may be online |
| `min-viewers` | At least this many players must currently see the screen |
| `max-viewers` | At most this many players may currently see the screen |
| `viewer-permission` | At least one current viewer must have this permission |
| `all-viewers-permission` | Every current viewer must have this permission |
| `tps-above` | Server TPS must be above this value |
| `tps-below` | Server TPS must be below this value |
| `world` | Screen must be in this world |
| `days` | Current real weekday must be listed |

If a condition fails, that item is ignored for this selection.

## What An Event Does

An event is a manual sequence.

It is not random. It plays in order.

Example:

```yaml
events:
  update_reveal:
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

What happens:

| Step | Result |
| --- | --- |
| `title` | Shows an internal LuigiScreen text frame for 5 seconds |
| `trailer` | Plays `media/trailers/update.mp4` for 30 seconds |
| `live` | Shows the RTMP stream for 2 minutes |
| end | Returns to the screen's playlist or fallback source |

Stop it early:

```text
/screen event stop main
```

## Playlist vs Event

| Feature | Playlist | Event |
| --- | --- | --- |
| Purpose | Normal rotation | Temporary interruption |
| Order | Weighted random | Fixed sequence |
| Command | `/screen playlist set` | `/screen event play` |
| Ends by itself | Keeps rotating | Yes, after the sequence |
| Returns to normal playback | Not needed | Yes |

## Item Types

Playlist and event items support:

| Type | Value |
| --- | --- |
| `rtmp` | RTMP or RTMPS URL |
| `mjpeg` | HTTP(S) MJPEG URL |
| `video` | Local file under `plugins/LuigiScreen/media/` |
| `image` | Local image under `plugins/LuigiScreen/media/` |
| `url-image` | HTTP(S) image URL |
| `gif` | Local or HTTP(S) GIF |
| `folder` | Random file from a local media folder |
| `text` | Internal LuigiScreen text frame |
| `countdown` | Internal LuigiScreen text frame for now |

## Sharing Loaders

If two screens play the same active source, LuigiScreen uses one loader.

Example:

```text
main  -> video trailers/update.mp4
lobby -> video trailers/update.mp4
```

The video is decoded once. Both screens render the latest shared frame with
their own size, FPS, distance and viewers.

This also works with playlists. If two playlist-controlled screens select the
same active item at the same time, they can share the loader.

## Common Mistakes

### The playlist does nothing

Check:

- Did you run `/screen reload` after editing `config.yml`?
- Did you assign it with `/screen playlist set <screen> <playlist>`?
- Is the playlist name spelled the same in the command and config?
- Is the screen enabled?

### A local file does not play

Check that the path starts inside:

```text
plugins/LuigiScreen/media/
```

For example:

```yaml
value: trailers/update.mp4
```

must exist as:

```text
plugins/LuigiScreen/media/trailers/update.mp4
```

### A folder item is skipped

Check:

- The folder exists.
- The folder contains supported files.
- `media-types` allows the files you placed there.

### The same item repeats too often

Increase its cooldown:

```yaml
cooldown: 5m
```

Or lower its weight:

```yaml
weight: 1
```

## Notes

- Playlist rotation is checked once per `playback.tick-seconds`.
- Events are manual in this alpha. Automatic triggers can be added later.
- Text and countdown items do not need a media file.
- Minecraft clients still do not receive audio.
