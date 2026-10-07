# Streaming from OBS

To show a live desktop, game or camera from OBS, the video travels:

```text
OBS  →  MediaMTX  →  LuigiScreen
```

MediaMTX is a small free RTMP server. LuigiScreen generates a secure
configuration for it; you only pick the situation:

| Situation | Command |
| --- | --- |
| Paper, MediaMTX and OBS on one computer | `/screen obs same-pc` |
| Other setups (LAN, internet, VPN, VPS) | see [Network setups](network.md) |

Do not need a live desktop? Local videos and YouTube links need no MediaMTX.

## Same computer, step by step

1. Run `/screen obs same-pc` in game as an operator.
2. Download [MediaMTX](https://mediamtx.org/), put the generated
   `plugins/LuigiScreen/mediamtx/SAME_PC/mediamtx.yml` beside `mediamtx.exe`
   (or `mediamtx` on Linux) and start it. It must report an RTMP listener on
   TCP `55556`. See [MediaMTX](mediamtx.md).
3. In OBS open **Settings → Stream**: Service *Custom*, Server = the complete
   URL the command printed (also in `setup.txt`), Stream key empty,
   authentication off. See [OBS Studio](obs.md).
4. Click **Start Streaming** in OBS.
5. Show it on a screen:

   ```text
   /screen source lobby rtmp://127.0.0.1:55556/screen
   ```

   The wizard also makes this the default source for new screens and switches
   screens that still used the previous default.

If the screen stays on *Source unavailable*, see
[Troubleshooting](../troubleshooting/common-errors.md).

`setup.txt` contains passwords. Never publish it.
