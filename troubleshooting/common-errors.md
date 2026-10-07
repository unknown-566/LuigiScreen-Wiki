# Troubleshooting

Start with `/screen info <screen>`: it shows the state and the last problem.
Web Studio → System → **Problems** lists everything that needs attention.

## Screen is invisible

- Same world and within the viewing distance (`/screen set <screen> distance …`)?
- Is it on? `/screen info` says *off* → `/screen on <screen>`.
- Private? `/screen set <screen> private false`, or grant `luigiscreen.see.<screen>`.
- Does it face you? If it was built the wrong way, recreate it from the other side.
- Any PacketEvents error in the console at startup?

Walk away and back, or reconnect. In the in-game Control Studio, **Repair and
Resync** resends the screen to all viewers.

## Startup errors

| Error | Fix |
| --- | --- |
| `NoClassDefFoundError`, PacketEvents error | Remove older `LuigiScreen*.jar` files and restart. A separate PacketEvents plugin does not conflict. |
| `no jniavutil in java.library.path` | The OS/CPU is not supported (only Windows/Linux x86_64), or an old JAR is still in `plugins/`. |
| Web Studio not running | Port `8765` is taken. Change `web-studio.port`. |

## Local files

| Symptom | Fix |
| --- | --- |
| "Nothing called … in the media library" | The file must be inside `plugins/LuigiScreen/media/`. Use the path from there (`trailers/intro.mp4`) when the name is not unique. |
| File marked as a problem in Web Studio → Media | Broken or unsupported file. Re-export video as H.264 MP4. |

## YouTube and other links

| Symptom | Fix |
| --- | --- |
| "needs yt-dlp" | Web Studio → System → YouTube and video links → **Install**, or set `online-video.yt-dlp-path`. Do not double-click `yt-dlp.exe`. |
| Install fails | The server needs outbound HTTPS to GitHub. On some hosts executables in `plugins/` are blocked; set `yt-dlp-path` to an allowed one. |
| Plays briefly, then *Source unavailable*, or never starts | Private, age-restricted and region-locked videos do not work. Sites change often; yt-dlp updates itself every few days. For a quick fix delete `plugins/LuigiScreen/bin/yt-dlp*` and press **Install** again. The exact error is in Web Studio → System. |

## OBS / RTMP

| Symptom | Fix |
| --- | --- |
| `Could not open input` | MediaMTX not running, wrong host/port, firewall, router forwarding, or the host blocks outbound TCP. Check that MediaMTX reports its RTMP listener; open the reader URL in VLC. |
| `authentication failed` | Copy the complete OBS URL again, keep the stream key empty, disable OBS authentication, restart MediaMTX after replacing its config. Regenerating creates new passwords. |
| `video dimensions are missing`, `Picture size 0x0` | Start MediaMTX, then OBS; use H.264 with a 2-second keyframe interval; restart streaming; `/screen off` and `/screen on`. |
| Stream live but screen black | Is the OBS preview black? Test the URL in VLC; check source resolution in `/screen debug`. |
| `/screen obs` fails | Check write permissions for `plugins/LuigiScreen/mediamtx/` and the console. |

## Other

| Symptom | Fix |
| --- | --- |
| Decoder stuck in *stopping* | FFmpeg can take a few seconds to leave a blocked network read. Wait instead of reloading repeatedly. |
| Web Studio link does not open | See [Web Studio](../studio/web-studio.md#troubleshooting). |
| Debug sidebar fights another plugin | `debug.sidebar-enabled: false`, then `/screen reload`. |
| Lag | See [Performance and Debug](../operations/performance.md). |

## Reporting a problem

Open an [issue](https://github.com/unknown-566/LuigiScreen/issues) with:

```text
Paper version / Java version:
OS and CPU (e.g. Windows x86_64):
LuigiScreen version:
Source type (file, YouTube, RTMP…):
Where OBS / MediaMTX / Paper run (if streaming):
Output of /screen info <screen>:
```

Add the LuigiScreen part of the startup log and the full Java exception. For
more FFmpeg detail set `logging.ffmpeg-level: info`, `/screen reload`,
reproduce once, then set it back to `quiet`.

**Remove passwords and stream URLs before posting.** Never share `setup.txt`,
`mediamtx.yml` or Web Studio login links.
