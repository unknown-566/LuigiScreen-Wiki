# Commands

All commands use `/screen`. The full command name is `/luigiscreen`.

## Create and clone

### `/screen create <name> [width] [height]`

Creates a named display on the targeted vertical wall. Look at the upper-left
block from the front.

```text
/screen create main 7 4
/screen create lobby 5 3
```

Names may contain lowercase letters, numbers, `_` and `-`, up to 32
characters. Names are normalized to lowercase.

The legacy syntax `/screen create 7 4` still creates a screen named `main`.

### `/screen clone <source> <new-name>`

Creates another screen at the wall you are looking at. The clone copies the
source screen's typed media source, FPS, distance, dimensions and enabled
state.

```text
/screen clone main lobby
```

While both screens have the same source type and value, LuigiScreen uses one
loader and fans the latest frame out to both renderers.

## Inspect

### `/screen list`

Lists every screen, its size, world, FPS, distance, enabled state, source type
and masked source value.

### `/screen status`

Shows the total number of screens, enabled screens, unique shared sources and
maps.

### `/screen status <name>`

Shows detailed state for one screen, including its shared-source count,
effective FPS, viewers, frame counters, source type and masked value.

## Control

### `/screen start <name|all>`

Enables one screen or all screens. A shared loader starts only once even when
several enabled screens use the same typed source.

### `/screen stop <name|all>`

Disables one screen or all screens without deleting their saved definitions.
The loader stops when its last enabled screen is disabled.

### `/screen remove <name>`

Destroys one MapEngine display and removes its saved definition. Other clones
and shared loaders remain active.

## Media source

### `/screen source <name>`

Shows the selected source type and its safe display value.

### `/screen source <name> types`

Lists:

```text
rtmp, mjpeg, video, image, url-image, gif
```

### `/screen source <name> <type> <value>`

Switches the source without recreating the screen:

```text
/screen source main rtmp rtmp://127.0.0.1:55556/screen
/screen source camera mjpeg http://camera.local/video
/screen source cinema video intro.mp4
/screen source poster image poster.png
/screen source news url-image https://example.com/current.png
/screen source animation gif welcome.gif
```

Relative local paths use `plugins/LuigiScreen/media/`. See
[Media sources](../screen/sources.md).

## Playlists and events

### `/screen playlist list`

Lists configured playlists from `config.yml`.

### `/screen playlist set <screen> <playlist>`

Assigns a configured playlist to a screen. The screen keeps its configured
source as a fallback, but the active source can rotate at runtime.

```text
/screen playlist set main spawn_rotation
```

### `/screen playlist clear <screen>`

Stops playlist control for the screen and returns it to its configured source.

```text
/screen playlist clear main
```

### `/screen event list`

Lists configured manual events.

### `/screen event play <screen> <event>`

Temporarily interrupts the screen and plays the configured event sequence.
When the event ends, the screen returns to its playlist or configured source.

```text
/screen event play main update_reveal
```

### `/screen event stop <screen>`

Stops the active event on a screen and returns to normal playback.

```text
/screen event stop main
```

See [Playlists and Events](../screen/playlists-events.md) for configuration
examples.

## Screen settings

### `/screen set <name> <property> <value>`

Editable properties:

```text
fps
distance
enabled
permission
url
```

Examples:

```text
/screen set lobby fps 4
/screen set lobby distance 32
/screen set lobby enabled false
/screen set lobby permission true
/screen set lobby url rtmp://127.0.0.1:55556/second
```

FPS accepts `0.1` to `20`. Distance accepts `8` to `1024` blocks.
`permission true` requires `luigiscreen.see.<name>` to view the screen.
`url` is a legacy compatibility shortcut that always selects an RTMP source.

## Administration

### `/screen reload`

Safely stops every source worker, reloads `config.yml` and localization,
preserves all existing MapEngine displays and restarts enabled shared sources.

Source, FPS, distance, enabled state and visibility permission are updated
without removing the display. A screen missing from config is restored.
Use `/screen remove <name>` when you actually want to delete it.

World, location, width, height and facing cannot be changed safely during
reload. Remove and recreate that screen to apply geometry changes.

Use a full server restart after replacing the plugin JAR or native libraries.

### `/screen debug`

Toggles the personal debug boss bar and, by default, the 15-line sidebar.
Multi-screen debug includes total screens, enabled screens and shared sources.

### `/screen mediamtx <situation>`

Valid situations:

```text
same-pc
lan
internet
vpn
hosting
```

The wizard updates the global default to an RTMP source and updates screens
still using the previous default. Custom per-screen sources are preserved.
