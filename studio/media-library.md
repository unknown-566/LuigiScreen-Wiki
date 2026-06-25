# Media Library

Media Library indexes local files inside:

```text
plugins/LuigiScreen/media/
```

Supported local extensions:

| Type | Extensions |
| --- | --- |
| Video | `.mp4`, `.mkv`, `.webm`, `.mov`, `.avi` |
| Image | `.png`, `.jpg`, `.jpeg`, `.webp` |
| Animation | `.gif` |

## Automatic watcher

The media folder and its existing subfolders are watched outside the Minecraft
main thread. Adding, changing or removing a file triggers a debounced rescan.

You no longer need `/screen reload` after adding normal local media.

Use **Rescan Media** if a network-mounted filesystem does not emit file watcher
events.

## Validation

Every entry reports:

- source type
- file size
- image/GIF dimensions when available immediately
- modification time
- number of playlist/event references
- validation result
- aggregate play count and planned playback time

Images that cannot be decoded, empty files and unsupported extensions are
marked invalid. Video decoding is verified when the thumbnail or playback
worker opens the file.

## Thumbnails

Thumbnail creation runs on the dedicated Media Library executor:

- images and GIFs use their decoded image
- videos use the first usable FFmpeg video frame
- the image is fitted into a centered 128x128 map canvas
- generated PNG files are cached in `.thumbnails/`

A Minecraft map ID is allocated only when that media entry is actually shown
in the GUI. The ID is saved in `studio.yml` and reused after restart.

This avoids allocating a map for every file during startup.

## Cueing

In Web Studio, media cards have two visible production buttons:

| Button | Result |
| --- | --- |
| **Add to playlist** | Adds the media to the playlist selected at the top of Media Library |
| **Play now** | Temporarily plays the media on the selected live target |

If no playlist exists yet, open **Playlist Editor** and create one first.

For in-game Control Studio, first select a screen from the Screens page.

| Click | Result |
| --- | --- |
| Left | Play the media immediately |
| Right | Add it after the current content |

Both actions are runtime overrides. They do not rewrite the screen's configured
fallback source.

## References and deletion

References show how many `config.yml` values point to the relative media path.
The first alpha does not delete files from the GUI.

Safe deletion procedure:

1. Check the References count.
2. Remove or replace references in affected playlists/events.
3. Publish the draft.
4. Delete the file from `plugins/LuigiScreen/media/`.
5. Confirm that Media Library removes it automatically.

This conservative behavior prevents one accidental inventory click from
deleting a server file.
