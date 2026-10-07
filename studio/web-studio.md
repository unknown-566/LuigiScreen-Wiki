# Web Studio

Web Studio is the browser control panel. It controls the same screens,
playlists and events as the commands, and works on a PC, tablet or phone.

## Open it

```text
/screen web
```

Click the link in chat. It works once and expires after a few minutes; the
browser then stays signed in for `web-studio.session-hours` (8 h). A second
link is printed for the server PC itself.

If the link does not open from another device on your network, allow inbound
TCP port `8765` for Java in the server's firewall.

`/screen web revoke` signs out all your browser sessions.

## Layout

Four sections, in the sidebar on a PC and in the bottom bar on a phone:

| Section | What you do there |
| --- | --- |
| **Screens** | See every screen, what it plays and who watches; open one to control it |
| **Media** | Files in `plugins/LuigiScreen/media/`; play or queue them |
| **Program** | Playlists, events and automations (tabs) |
| **System** | Problems, YouTube setup, picture quality, session, recent changes |

## Play something

Open a screen and press **Play…**. The dialog has four tabs:

| Tab | Use |
| --- | --- |
| **Media** | A file from the library |
| **YouTube / link** | Paste a YouTube, Twitch or other video link, or an image/GIF link |
| **Playlist** | Run a playlist on this screen |
| **Event** | Start an event takeover |

Choose **Play now** or **Queue**. Leave the time empty for automatic length
(videos to the end, images for the default duration). **Enter** confirms.

The screen page also has **Pause**, **Skip** and **Return** (back to the normal
program), a live preview, and **Settings** for FPS, viewing distance and
private visibility.

New screens are placed in Minecraft: look at the top-left block of a wall and
run `/screen create <name> [width] [height]`.

## YouTube

The first time, open **System → YouTube and video links** and press
**Install**. LuigiScreen downloads the official yt-dlp release into
`plugins/LuigiScreen/bin/`, checks its SHA-256 and keeps it updated. Do not
run `yt-dlp.exe` yourself. There is no sound in Minecraft yet.

## Program

**Playlists** rotate media by weight. Create one, add items with duration and
weight, then assign it to a screen. The chance column shows how often each
item will play.

**Events** are temporary takeovers made of steps (media, text, countdown, wait
for operator). When an event ends, the screen returns to its playlist or
default source.

**Automations** run an event, playlist, on, off or return at a server time,
on a screen or a group. **Run now** tests a rule immediately. **Screen
groups** live on the same tab.

Program changes are saved immediately; there is no separate publish step.

## System

- **Problems**: only things that need attention. A screen paused because
  nobody is nearby, or one still connecting, is normal and not listed.
- **YouTube and video links**: install status and the last yt-dlp error.
- **Picture quality**: dithering, colour stability, glowing frames, pause
  without viewers, adaptive FPS and the map-update budget. Saving reloads the
  plugin; screens reconnect within seconds.
- **This session**: who you are signed in as and when it expires.
- **Recent changes**: audit log.

## Emergency mode

Emergency mode switches every screen to a static `MAINTENANCE` frame and
blocks playback until it is turned off. A banner shows while it is on. See
[Drafts, History, Emergency and Roles](safety-roles.md).

## Permissions

The player needs `luigiscreen.web`. The browser session receives a copy of that
player's LuigiScreen permissions at the moment the link was created, so after
changing permissions run `/screen web revoke` and open a new link. Buttons you
are not allowed to use are hidden.

See [Permissions](../reference/permissions.md) for the section permissions.

## Security

- one-time, short-lived login links
- HttpOnly, SameSite session cookies
- CSRF token and origin check on every change
- passwords and stream keys masked in the UI

Do not expose port `8765` to the internet. For remote access use a VPN or an
HTTPS reverse proxy and set `web-studio.public-url`.

## Configuration

```yaml
web-studio:
  enabled: true
  bind: "0.0.0.0"
  port: 8765
  public-url: ""
  server-name: "LuigiScreen Server"
  session-hours: 8
  login-token-minutes: 5
  live-update-millis: 1000
  preview-refresh-millis: 1000
  preview-max-width: 640
```

Live updates arrive over a light server-sent event stream. Previews are only
captured while a browser is connected. On a small host try `2000`, `2000` and
`480` for the last three values.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Link does not open from another PC | Same network? Firewall TCP `8765` for Java. Port not used by another app? |
| "Login expired" | Run `/screen web` again |
| A button is missing | Your permissions do not allow it; fix them and open a new link |
| Web Studio not running | The console says why; usually port `8765` is taken. Change `web-studio.port` |
