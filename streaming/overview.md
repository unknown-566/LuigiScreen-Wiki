# Choose an RTMP Network Setup

Live video from OBS travels OBS → MediaMTX → LuigiScreen. LuigiScreen
generates a secure MediaMTX configuration for you; pick the situation that
matches where the three programs run:

| Situation | Command | Use when |
| --- | --- | --- |
| Same computer | `/screen obs same-pc` | Paper, MediaMTX and OBS run on one computer |
| Local network | `/screen obs lan` | Paper and MediaMTX share a computer; OBS is elsewhere in the same LAN |
| Public internet | `/screen obs internet` | OBS reaches home MediaMTX through router port forwarding |
| VPN | `/screen obs vpn` | OBS reaches MediaMTX through Tailscale or ZeroTier |
| External hosting | `/screen obs hosting` | MediaMTX runs on a public VPS or another hosting machine |

(`/screen mediamtx` still works as an older name.)

## Same computer, step by step

1. Run `/screen obs same-pc` in game as an operator.
2. Download [MediaMTX](https://mediamtx.org/), put the generated
   `plugins/LuigiScreen/mediamtx/SAME_PC/mediamtx.yml` beside `mediamtx.exe`
   (or `mediamtx` on Linux) and start it. It must report an RTMP listener on
   TCP `55556`.
3. In OBS open **Settings → Stream**: Service *Custom*, Server = the complete
   URL the command printed (also in `setup.txt`), Stream key empty,
   authentication off. See [OBS Studio](obs.md) for output settings.
4. Click **Start Streaming** in OBS.
5. Show it on a screen:

   ```text
   /screen source lobby rtmp://127.0.0.1:55556/screen
   ```

   The wizard already makes this the default source for new screens.

If the screen stays on *Source unavailable*, see
[Common errors](../troubleshooting/common-errors.md).

## What the wizard does

1. Asks only for values it cannot detect (your chat answers are not broadcast).
2. Generates random publisher and reader credentials.
3. Writes a restricted `mediamtx.yml` and a private `setup.txt` under
   `plugins/LuigiScreen/mediamtx/<SITUATION>/`, backing up older files.
4. Makes the new RTMP address the default source and switches screens that
   still used the previous default.

`setup.txt` contains passwords. Never publish it.

## A tunnel is not a server

VPNs and tunnels solve reachability; they do not keep MediaMTX, OBS or your
PC running. For playback without your home PC, see
[External hosting and 24/7](../network/external-hosting.md) — or simply use a
local video or a YouTube link, which need no MediaMTX at all.
