# Common Errors

## `no jniavutil in java.library.path`

Example:

```text
java.lang.UnsatisfiedLinkError: no jniavutil in java.library.path
```

Cause:

The plugin JAR does not include native FFmpeg libraries for the server operating system or CPU architecture.

Fix:

- Use LuigiScreen `1.1.0-alpha.7` or newer on Linux x86_64.
- Confirm the server reports `Linux ... amd64` or Windows x86_64.
- ARM and macOS are not currently supported.
- Remove older LuigiScreen JARs before restarting.

## `RTMP connection failed: Could not open input`

Causes:

- MediaMTX is not running
- Wrong host or port
- Firewall blocks TCP
- Router forwarding is wrong
- The hosting provider blocks outbound traffic

Checks:

1. Confirm MediaMTX reports the RTMP listener.
2. Test the reader URL in VLC from the Minecraft server network when possible.
3. Confirm the correct setup situation was used.
4. Check `/screen status`.

## `authentication failed`

Causes:

- Old OBS URL after regenerating credentials
- Wrong `mediamtx.yml`
- Authentication enabled separately in OBS
- Password copied incompletely

Fix:

1. Copy the complete generated OBS server URL.
2. Leave the stream key empty.
3. Disable OBS authentication.
4. Restart MediaMTX after replacing its config.

## `video dimensions are missing` or `Picture size 0x0 is invalid`

Cause:

FFmpeg connected before it received enough valid H.264 stream metadata, or OBS published an incomplete stream.

Fix:

- Start MediaMTX first.
- Start OBS and confirm its preview.
- Use H.264 with a two-second keyframe interval.
- Stop and start OBS streaming.
- Run `/screen stop`, wait a few seconds, then `/screen start`.

## Screen is created but invisible

Check:

- You are in the same world
- You are within `screen.viewer-distance`
- MapEngine is enabled
- The screen faces the player
- `/screen status` shows a saved display

Try moving away and returning, or reconnecting to the server.

## Stream is live but the Minecraft screen is black

Check the OBS preview first.

If OBS is not black:

- Test the MediaMTX reader URL in VLC
- Confirm source resolution is reported in `/screen debug`
- Check render errors
- Disable unusual H.264 encoder options

## Decoder remains in `stopping`

Native FFmpeg can take several seconds to leave a blocked network read.

Do not repeatedly run reload or create commands. Wait and check `/screen status`.

MediaMTX profile switching in `alpha.6` and newer happens asynchronously and never starts a second decoder before the old one terminates.

## `/screen mediamtx` fails

Check write permissions for:

```text
plugins/LuigiScreen/mediamtx/
```

Also check the server console for the exact `MediaMTX setup failed` message.

## Sidebar conflicts with another plugin

Disable it:

```yaml
debug:
  sidebar-enabled: false
```

Then run:

```text
/screen reload
```

The rotating boss bar remains available.
