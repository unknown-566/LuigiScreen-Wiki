# In-game Control Studio

```text
/screen menu
```

An inventory menu for operators who prefer to stay in the game. It controls
the same screens, playlists and events as the commands and
[Web Studio](web-studio.md); for most work Web Studio is easier.

## Sections

| Section | Purpose |
| --- | --- |
| Screens | state, current item, location and per-screen controls |
| Media Library | browse files with map thumbnails; play or queue them |
| Playlists | chances, eligibility and draft edits |
| Events | step timelines; start events |
| Live Control | on-air item, cue next, hold, return, voting, emergency |
| Screen Groups | start, stop or return several screens |
| Schedule Calendar | automations and conflict warnings |
| Template Library | install starter playlists and events |
| Diagnostics | source, frame, FPS, reconnect and render details |
| Change History | audit log and undo of the last config change |

**Back** returns to the parent page; long lists have page buttons. Live pages
refresh every second. Shift-click a dashboard section to pin it to the bottom
row.

## Select a screen first

Actions such as playing media need a target. In **Screens**, shift-left-click a
screen to select it and open Live Control. The selection stays while you move
between sections.

## Screens

| Click | Action |
| --- | --- |
| Left | open screen detail |
| Right | turn on or off |
| Shift-left | select it and open Live Control |

Screen detail has:

- **Hold/Resume**, **Skip**, **Repeat** (lock the current item), **Return to Automation**
- **Content Queue**: left-click plays an item now, right-click removes it, shift-left moves it up
- **Why is this playing?** and the last 20 playback decisions
- **Teleport**, **Highlight Bounds** (particles at the corners) and
  **Repair and Resync** (respawn the frames for viewers and resend all maps)
- visibility toggle (`private`) and health: source state, frame age,
  reconnects, frame counts, FPS, render time, viewers
- usage statistics: plays, play time, viewer time, skips, failures (aggregate, no per-player history)

## Media Library

Left-click a file to play it on the selected screen, right-click to queue it.
**Rescan Media** forces a rescan on file systems that do not report changes.
The map ID of a thumbnail is allocated only when the file is first shown, and
then reused.

## Playlists

Left-click a playlist to assign it to the selected screen, right-click to open
its items. The item page shows each item's chance (1,000 simulated picks),
cooldown and whether it is eligible right now, for example:

```text
Cooldown: 1m 20s remaining
Failed: min-viewers needs 3, currently 1
```

| Click on item | Action |
| --- | --- |
| Left | preview it on the selected screen |
| Right | stage enabled/disabled |
| Shift-left | stage weight +1 |
| Middle | condition builder in chat, e.g. `min-viewers=3,tps-above=18,days=FRIDAY\|SATURDAY` |

Staged changes are a draft: press **Publish Changes** to apply them all, or
**Discard Draft**. See [Safety and Roles](safety-roles.md).

**Create Playlist** asks for a name in chat (type `cancel` to stop) and creates
a starter playlist you can edit.

## Live Control

- left-click media to take it live, right-click to cue it as next
- **Take Live/Next**, **Hold/Resume**, **Events**, **End Event**, **Return**
- **Audience Vote** starts or ends a vote
- **Emergency** opens a separate confirmation page

## Files

| Path | Contents |
| --- | --- |
| `plugins/LuigiScreen/studio.yml` | groups, schedules, favorites, audit, voting, statistics, thumbnail map IDs |
| `plugins/LuigiScreen/media/.thumbnails/` | cached 128×128 previews |
| `plugins/LuigiScreen/history/` | config snapshots taken before each change |

Do not edit `studio.yml` while the server is running.
