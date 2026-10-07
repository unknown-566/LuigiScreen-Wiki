# Media Sources

## Play now or set as default

| You want | Command |
| --- | --- |
| Show something now, then go back | `/screen play <screen> <file\|link> [time]` |
| Show it after the current item | `/screen queue <screen> <file\|link> [time]` |
| Change what the screen shows normally | `/screen source <screen> <file\|link>` |

Web Studio does the same with **Play…** on a screen (tabs *Media* and
*YouTube / link*).

## What you can type

LuigiScreen works out the kind of source from what you give it:

| You type | It becomes | Notes |
| --- | --- | --- |
| `intro.mp4`, `trailers/update.mp4` | local video | from `plugins/LuigiScreen/media/`; just the file name is enough when it is unique |
| `poster.png`, `logo.jpg`, `logo.webp` | local image | |
| `loading.gif` | local GIF | |
| `https://…/image.png` | web image | loaded once |
| `https://…/animation.gif` | web GIF | |
| `https://www.youtube.com/watch?v=…`, `https://twitch.tv/…`, any other video page | online video | needs yt-dlp; live streams work too |
| `rtmp://host:port/path` | RTMP stream | OBS through MediaMTX |
| `mjpeg http://camera/video` | MJPEG camera | the only kind that needs its type written first |

You can always put a type first to be explicit: `video`, `image`, `gif`,
`url-image`, `youtube`, `rtmp`, `mjpeg`.

```text
/screen play lobby intro.mp4
/screen play lobby https://www.youtube.com/watch?v=aqz-KE-bpKQ
/screen queue lobby poster.png 20s
/screen source cinema rtmp://127.0.0.1:55556/screen
/screen source gate mjpeg http://192.168.1.50:8080/video
```

## How long `play` and `queue` last

Without a time, videos and online videos play to their end, images and GIFs
for `playback.default-duration` (30 s), live streams until you skip them
(at most 6 hours).
Times accept `30s`, `5m`, `1h`.

## Local files

Put files into `plugins/LuigiScreen/media/` (subfolders are fine). They appear
in the Media Library and in tab completion on their own; no reload needed.

| Kind | Extensions |
| --- | --- |
| Video | `mp4`, `mkv`, `webm`, `mov`, `avi` |
| Image | `png`, `jpg`, `jpeg`, `webp` |
| Animation | `gif` |

H.264 MP4 is the safest video format. Absolute paths outside the media folder
are blocked unless you set `sources.allow-absolute-paths: true`.

## YouTube, Twitch and other sites

Online videos are opened through [yt-dlp](https://github.com/yt-dlp/yt-dlp).
Install it once in **Web Studio → System → YouTube and video links**:
LuigiScreen downloads the official release into `plugins/LuigiScreen/bin/`,
checks its SHA-256 and keeps it up to date. If yt-dlp is already installed on
the server, set `online-video.yt-dlp-path` instead.

- Only the picture is downloaded, up to `online-video.max-height` (480p).
- The stream address is fetched again on every reconnect, so long playback keeps working.
- Do not run `yt-dlp.exe` yourself; LuigiScreen calls it in the background.

## Streams and cameras

RTMP sources reconnect automatically with backoff. For OBS setup see
[Choose an RTMP network setup](../streaming/overview.md).

MJPEG needs the camera's direct MJPEG address, not a web page with a player.

## Switching safely

A missing local file is rejected and the screen keeps its previous source.
A remote source that is offline is accepted; the screen shows its status card
and keeps retrying.

## In `config.yml`

```yaml
screens:
  lobby:
    source:
      type: youtube
      value: "https://www.youtube.com/watch?v=aqz-KE-bpKQ"
```

Playlist and event items use the same `type` / `value` pair. See
[Playlists and Events](playlists-events.md).
