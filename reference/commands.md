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
source screen's URL, FPS, distance, dimensions and enabled state.

```text
/screen clone main lobby
```

While both screens have the same URL, LuigiScreen uses one FFmpeg decoder and
fans the latest frame out to both renderers.

## Inspect

### `/screen list`

Lists every screen, its size, world, FPS, distance, enabled state and masked
URL.

### `/screen status`

Shows the total number of screens, enabled screens, unique shared sources and
maps.

### `/screen status <name>`

Shows detailed state for one screen, including its shared-source count,
effective FPS, viewers, frame counters and masked URL.

## Control

### `/screen start <name|all>`

Enables one screen or all screens. A shared decoder starts only once even when
several enabled screens use its URL.

### `/screen stop <name|all>`

Disables one screen or all screens without deleting their saved definitions.
The decoder stops when its last enabled screen is disabled.

### `/screen remove <name>`

Destroys one MapEngine display and removes its saved definition. Other clones
and shared decoders remain active.

### `/screen set <name> <property> <value>`

Editable properties:

```text
url
fps
distance
enabled
permission
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

## Administration

### `/screen reload`

Safely stops every decoder, reloads `config.yml` and localization, preserves
unchanged MapEngine displays and restarts enabled shared sources.

A display is recreated only when its world, location, width, height or facing
changed. URL, FPS, distance, enabled state and visibility permission are
updated without removing the display.

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

The wizard updates the global default URL and screens still using the previous
default URL. Custom per-screen URLs are preserved.
