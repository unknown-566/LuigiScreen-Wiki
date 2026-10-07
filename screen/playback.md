# Playing and Controlling

## What a screen shows

Every screen has a **default source**, saved in `config.yml`. On top of it
LuigiScreen stacks temporary layers; the highest active one wins:

| Layer | Started by | Ends |
| --- | --- | --- |
| 1. Emergency mode | Web Studio or in-game menu | when turned off |
| 2. Event | `/screen event`, **Play… → Event**, an automation | after its last step, or `stop` |
| 3. Queue | `/screen play`, `/screen queue`, **Play…** | each item after its time |
| 4. Playlist | `/screen playlist`, Web Studio | never, it keeps rotating |
| 5. Default source | `/screen source` | – |

When a layer ends, the screen falls back to the next one down. While a source
connects or fails, the screen shows a short status card instead of a frozen frame.

## Commands

| Command | What it does |
| --- | --- |
| `/screen play <screen> <file\|link> [time]` | Play now, then go back |
| `/screen queue <screen> <file\|link> [time]` | Play after the current item |
| `/screen pause <screen>` | Hold the current item; run again to continue |
| `/screen skip <screen>` | Next item: queue first, then the playlist |
| `/screen return <screen>` | End the event, clear the queue and pause; back to playlist or default source |
| `/screen source <screen> <file\|link>` | Change the default source |
| `/screen playlist <screen> <name\|off>` | Use a playlist, or stop using one |
| `/screen event <screen> <name\|stop>` | Start or stop an event |

The screen name can be left out when only one screen exists.

In Web Studio the same actions are on each screen: **Play…** (tabs Media,
YouTube / link, Playlist, Event), **Pause**, **Skip** and **Return**.

## How long items last

Without a time:

| Media | Plays for |
| --- | --- |
| Video, online video | its full length |
| Image, GIF | `playback.default-duration` (30 s) |
| Live stream, camera | until skipped (at most 6 hours) |

Times accept `250ms`, `30s`, `5m`, `1h`; a plain number means seconds.

## What is playing?

`/screen list` shows every screen with its state and current item; click a
line for `/screen info <screen>`, which adds the default source, location,
viewers, FPS and the last problem.
