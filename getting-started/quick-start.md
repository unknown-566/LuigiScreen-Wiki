# Quick Start

Five minutes from install to a playing screen.

## 1. Build a screen

Build a flat wall, for example 4 blocks wide and 3 high. Stand in front of it,
look at its **top-left** block and run:

```text
/screen create lobby 4 3
```

Each block is one map of 128×128 pixels, so 4×3 gives 512×384.

## 2. Play a file

Copy a video, image or GIF into `plugins/LuigiScreen/media/`, then:

```text
/screen play lobby intro.mp4
```

Videos play to the end, images for 30 seconds, then the screen goes back to
what it showed before. Add a time to override it: `/screen play lobby poster.png 2m`.

With only one screen on the server you can leave out its name:
`/screen play intro.mp4`.

## 3. Play YouTube or Twitch

Links need yt-dlp once: run `/screen web`, open **System** and press
**Install** next to *YouTube and video links*. Then:

```text
/screen play lobby https://www.youtube.com/watch?v=aqz-KE-bpKQ
```

Live Twitch and YouTube streams work the same way.

## 4. Make it permanent

`play` is temporary. To set what the screen shows normally:

```text
/screen source lobby intro.mp4
```

Or let it rotate a [playlist](../screen/playlists.md) made in Web Studio:

```text
/screen playlist lobby spawn_rotation
```

## 5. Control it from the browser

```text
/screen web
```

Click the link in chat. You get every screen with a live preview, a **Play…**
button, the media library, playlists, events, automations and settings. It
works on a phone too.

## Next

- [What you can play](../screen/sources.md)
- [Playing and controlling](../screen/playback.md)
- [Commands](../reference/commands.md)
