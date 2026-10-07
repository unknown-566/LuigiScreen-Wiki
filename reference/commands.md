# Commands

Everything starts with `/screen` (full name `/luigiscreen`). Run `/screen`
alone for clickable help. For most work, `/screen web` is easier.

**You can leave out the screen name** when the server has exactly one screen:
`/screen play intro.mp4` works then.

## Play something

| Command | What it does |
| --- | --- |
| `/screen play <screen> <file\|link> [time]` | Play now, then go back to the normal program |
| `/screen queue <screen> <file\|link> [time]` | Play after the current item |
| `/screen pause <screen>` | Hold the current item; run it again to continue (`resume` also works) |
| `/screen skip <screen>` | Next item (`next` also works) |
| `/screen return <screen>` | Drop manual items and go back to the playlist/default source |

`<file|link>` can be a media library file (`intro.mp4`, or just its name when
unique), a YouTube/Twitch/video page link, an image or GIF link, or
`rtmp://…`. See [Media sources](../screen/sources.md).

Without `[time]`, videos play to the end, images and GIFs for
`playback.default-duration`, live streams until skipped. Times: `30s`, `5m`, `1h`.

```text
/screen play lobby https://www.youtube.com/watch?v=aqz-KE-bpKQ
/screen queue lobby poster.png 20s
/screen skip lobby
```

## What a screen shows normally

| Command | What it does |
| --- | --- |
| `/screen source <screen>` | Show the default source |
| `/screen source <screen> <file\|link>` | Change the default source |
| `/screen source <screen> <type> <value>` | Same, with an explicit type (`mjpeg` needs it) |
| `/screen playlist <screen>` | List playlists |
| `/screen playlist <screen> <name\|off>` | Run a playlist, or stop using one |
| `/screen event <screen>` | List events |
| `/screen event <screen> <name\|stop>` | Start an event takeover, or stop it |

Playlists and events are created in Web Studio → **Program** or in
`config.yml`. See [Playlists and Events](../screen/playlists-events.md).

## Screens

| Command | What it does |
| --- | --- |
| `/screen list` | All screens with state and what plays; click one for details |
| `/screen info <screen>` | Details of one screen (`status` also works) |
| `/screen on <screen\|all>` | Turn on (`start` also works) |
| `/screen off <screen\|all>` | Turn off, keeps the screen saved (`stop` also works) |
| `/screen create <name> [width] [height]` | Create a screen on the wall you look at (top-left block); default 4×3 |
| `/screen clone <screen> <new-name>` | Copy a screen to the wall you look at; both share one decoder |
| `/screen remove <screen>` | Delete a screen; asks you to click **[Delete]** to confirm (`delete` also works) |
| `/screen set <screen> fps <0.1–20>` | Frame rate |
| `/screen set <screen> distance <8–1024>` | Viewing distance in blocks |
| `/screen set <screen> private <true\|false>` | Only players with `luigiscreen.see.<screen>` see it |

Names use `a-z`, `0-9`, `_` and `-`, up to 32 characters.

## Administration

| Command | What it does |
| --- | --- |
| `/screen web` | One-time login link for [Web Studio](../studio/web-studio.md) |
| `/screen web revoke` | Sign out all your browser sessions |
| `/screen menu` | In-game Control Studio (`studio` also works) |
| `/screen reload` | Reload `config.yml` and messages; screens stay in place |
| `/screen debug` | Toggle the personal debug boss bar and sidebar |
| `/screen obs <situation>` | Generate a MediaMTX setup for OBS: `same-pc`, `lan`, `internet`, `vpn`, `hosting` (`mediamtx` also works) |

See [Choose an RTMP network setup](../streaming/overview.md) for `obs`.

## Voting

| Command | What it does |
| --- | --- |
| `/screen vote <screen> <option>` | Cast or change your vote |
| `/screen vote start <screen> [options…]` | Start a 60-second vote (needs `luigiscreen.menu.live`) |
| `/screen vote end <screen>` | End it; the winner is queued |

## Renamed in 1.3.0

| Old | New |
| --- | --- |
| `/screen start`, `stop` | `/screen on`, `off` (old names still work) |
| `/screen status <screen>` | `/screen info <screen>` (old name still works) |
| `/screen source <screen> video intro.mp4` | `/screen source <screen> intro.mp4` (type optional) |
| `/screen playlist set <screen> <name>` | `/screen playlist <screen> <name>` |
| `/screen playlist clear <screen>` | `/screen playlist <screen> off` |
| `/screen event play <screen> <name>` | `/screen event <screen> <name>` |
| `/screen set <screen> permission true` | `/screen set <screen> private true` |
| `/screen set … enabled`, `url` | `/screen on`/`off`, `/screen source` |
| `/screen mediamtx` | `/screen obs` (old name still works) |
