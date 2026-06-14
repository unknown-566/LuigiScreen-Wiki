# Commands

All commands use:

```text
/screen
```

The full command name is `/luigiscreen`.

## `/screen create [width] [height]`

Creates a map display on the targeted vertical wall.

Defaults:

```text
width: 7
height: 4
```

The command must be used by a player looking at the upper-left wall block.

## `/screen start`

Starts the RTMP decoder for the existing screen.

It does not start MediaMTX or OBS.

## `/screen stop`

Requests a safe decoder shutdown and shows the configured stopped frame.

Native FFmpeg shutdown can take several seconds.

## `/screen remove`

Stops the decoder, destroys the MapEngine display and clears the saved screen location.

If the decoder cannot stop safely, the display remains intact.

## `/screen status`

Shows:

- Screen dimensions
- Stream state
- Stream error
- Effective FPS
- Nearby viewers
- Received and rendered frame counts
- Render error
- World and facing
- Masked stream URL

## `/screen reload`

Reloads:

- `config.yml`
- Selected language file
- Screen definition
- Renderer
- RTMP connection
- Debug update schedule

Use a full server restart after replacing the plugin JAR or native libraries.

## `/screen debug`

Toggles the personal debug boss bar and, by default, the 15-line sidebar.

Only players can use this command.

## `/screen mediamtx`

Shows the available setup situations.

## `/screen mediamtx <situation>`

Valid situations:

```text
same-pc
lan
internet
vpn
hosting
```

The wizard times out after two minutes. Type `cancel` as the requested chat answer to stop it.
