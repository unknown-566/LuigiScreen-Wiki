# What You Can Play

Anywhere LuigiScreen asks for media (`/screen play`, `/screen source`,
playlists, events, Web Studio) you can give it a file name or a link. The kind
of source is detected from what you type:

| You type | It becomes | Notes |
| --- | --- | --- |
| `intro.mp4`, `trailers/update.mp4` | local video | from the media folder; the file name alone is enough when it is unique |
| `poster.png`, `logo.jpg`, `logo.webp` | local image | |
| `loading.gif` | local GIF | loops |
| `https://…/image.png` | web image | loaded once |
| `https://…/animation.gif` | web GIF | loops |
| `https://www.youtube.com/watch?v=…`, `https://twitch.tv/…`, other video pages | online video | needs yt-dlp; live streams work |
| `rtmp://host:port/path` | RTMP stream | OBS through MediaMTX |
| `mjpeg http://camera/video` | MJPEG camera | the only kind that needs its type written first |

You can always write the type first to be explicit: `video`, `image`, `gif`,
`url-image`, `youtube`, `rtmp`, `mjpeg`.

```text
/screen play lobby intro.mp4
/screen play lobby https://www.youtube.com/watch?v=aqz-KE-bpKQ
/screen source cinema rtmp://127.0.0.1:55556/screen
/screen source gate mjpeg http://192.168.1.50:8080/video
```

Playlists and events have three extra item types: `folder` (a random file
from a folder), `text` and `countdown`. See [Playlists](playlists.md).

There is no sound in Minecraft yet.

## Local files: the media library

Put files into `plugins/LuigiScreen/media/`. Subfolders are fine.

| Kind | Extensions |
| --- | --- |
| Video | `mp4`, `mkv`, `webm`, `mov`, `avi` |
| Image | `png`, `jpg`, `jpeg`, `webp` |
| Animation | `gif` |

H.264 MP4 is the safest video format.

- The folder is watched: new, changed and removed files show up by themselves,
  no reload needed. (On network drives without file events, use **Rescan** in
  the in-game Control Studio.)
- Every file is checked. Broken images, empty files and unsupported extensions
  are marked as problems in Web Studio → **Media**.
- Thumbnails are generated in the background and cached in `media/.thumbnails/`.
- Web Studio shows *in use* on files that playlists or events refer to. Check
  that before deleting a file. Files are deleted on the server, not from the
  browser.

Paths outside the media folder are blocked unless you set
`sources.allow-absolute-paths: true`.

## YouTube, Twitch and other sites

Online videos are opened through [yt-dlp](https://github.com/yt-dlp/yt-dlp).

1. Run `/screen web`, open **System → YouTube and video links**, press **Install**.
2. LuigiScreen downloads the official release into `plugins/LuigiScreen/bin/`,
   checks its SHA-256 and updates it every few days.

If yt-dlp is already on the server, set `online-video.yt-dlp-path` instead.
Do not start `yt-dlp.exe` yourself; LuigiScreen runs it in the background.

- Only the picture is downloaded, up to `online-video.max-height` (480p).
- The title and length are read automatically, so `play` and `queue` know when the video ends.
- The stream address is fetched again on every reconnect, so long playback keeps working.
- Private, age-restricted and region-locked videos cannot be played.

## Streams and cameras

RTMP sources reconnect automatically with growing delays. For OBS setup see
[Streaming from OBS](../streaming/overview.md).

MJPEG needs the camera's direct MJPEG address, not a web page with a player.

## When a source fails

A missing local file is rejected and the screen keeps its previous source. A
remote source that is offline is accepted: the screen shows a short status
card and keeps retrying.

## In `config.yml`

```yaml
screens:
  lobby:
    source:
      type: youtube
      value: "https://www.youtube.com/watch?v=aqz-KE-bpKQ"
```

Playlist and event items use the same `type` / `value` pair.
