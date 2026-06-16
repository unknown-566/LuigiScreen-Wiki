# Media Sources

Every screen has one typed media source. Change it without recreating the
screen:

```text
/screen source <screen> <type> <value>
```

Show the current source:

```text
/screen source main
```

List available types:

```text
/screen source main types
```

## Source types

| Type | Value | Behavior |
| --- | --- | --- |
| `rtmp` | `rtmp://` or `rtmps://` URL | Live stream with reconnect |
| `mjpeg` | `http://` or `https://` URL | Live MJPEG camera stream with reconnect |
| `video` | Local file path | Video loops automatically |
| `image` | Local file path | Static image loaded once |
| `url-image` | `http://` or `https://` URL | Remote image with retry after temporary failure |
| `gif` | Local file path or HTTP(S) URL | Animated GIF loops automatically |

Playlist and event configuration also supports `folder` and `text` items. They
are not direct `/screen source` types. See [Playlists and Events](playlists-events.md).

## Local media folder

Relative paths are resolved inside:

```text
plugins/LuigiScreen/media/
```

For example:

```text
plugins/LuigiScreen/media/intro.mp4
plugins/LuigiScreen/media/posters/server.png
plugins/LuigiScreen/media/animations/loading.gif
```

Use them with:

```text
/screen source main video intro.mp4
/screen source main image posters/server.png
/screen source main gif animations/loading.gif
```

Paths containing spaces are supported:

```text
/screen source main video event trailer.mp4
```

Absolute paths are disabled by default. They can be enabled with
`sources.allow-absolute-paths`, but keeping media inside the plugin folder is
safer and easier to move between servers.

## RTMP stream

```text
/screen source main rtmp rtmp://127.0.0.1:55556/screen
```

Use RTMP for OBS, MediaMTX or another live publisher. The source disconnects
when no eligible viewer is near any screen using it, then reconnects when a
viewer returns.

The old command remains as a compatibility shortcut:

```text
/screen set main url rtmp://127.0.0.1:55556/screen
```

It always selects the `rtmp` type.

## MJPEG stream

```text
/screen source camera mjpeg http://192.168.1.50:8080/video
```

Use the direct HTTP(S) MJPEG endpoint exposed by the camera or camera server.
A normal web page containing a video player is not an MJPEG endpoint.

## Local video

Put the file in the media folder and run:

```text
/screen source cinema video movie.mp4
```

LuigiScreen uses FFmpeg to decode the file and starts it again at the end.
Common FFmpeg-supported containers can work, but H.264 MP4 is a practical
starting format.

Minecraft does not receive the video's audio.

## Local image

```text
/screen source poster image event.png
```

The image is loaded once and kept as the screen frame. PNG and JPEG are the
most portable choices.

## URL image

```text
/screen source news url-image https://example.com/current.png
```

The URL must return the image itself. HTML pages are not supported. Download
size, decoded pixel count and HTTP timeout are limited by `config.yml`.

The image is not continuously polled after a successful load. Switch the
source again or restart it when the remote file changes.

## GIF

Local GIF:

```text
/screen source animation gif welcome.gif
```

Remote GIF:

```text
/screen source animation gif https://example.com/welcome.gif
```

GIF animation loops automatically and uses FFmpeg for timing and decoding.

## Switching safely

LuigiScreen validates the new source before replacing the old local source.
If a local file does not exist, the command fails and the previous source is
kept.

Remote connectivity is checked by the background worker. A valid remote URL
can therefore be selected while it is offline; the screen shows its offline
state and reconnects where supported.

## Sharing sources and clones

Screens share one loader when both their normalized source type and value are
identical:

```text
main:  video intro.mp4
lobby: video intro.mp4
```

These screens decode the video once, then independently render the latest
frame with their own FPS, distance, size, location and permissions.

Changing one screen to another type or value moves it into another source
group:

```text
/screen source lobby image lobby.png
```

## Configuration form

```yaml
screens:
  main:
    source:
      type: video
      value: intro.mp4
```

Old per-screen `url:` fields are migrated automatically to an RTMP source.
