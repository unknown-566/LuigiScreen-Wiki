# Safety and Roles

## History and undo

Before LuigiScreen writes a change to `config.yml` from a menu (publishing a
draft, saving picture quality in Web Studio), it stores a full snapshot:

```text
plugins/LuigiScreen/history/config-YYYYMMDD-HHMMSS-SSS.yml
```

`history.max-entries` in `studio.yml` (default 20) limits how many are kept.

**Change History → Undo Last Publish** in the in-game Control Studio restores
the newest snapshot and reloads safely. It restores the whole file, not one
playlist. To roll back further, copy an older snapshot over `config.yml` and
run `/screen reload`.

## Drafts (in-game editor)

In the in-game playlist and event editor, clicks are staged as a private
draft. **Publish Changes** snapshots the config, applies everything together
and reloads; **Discard Draft** throws it away. Drafts are lost on restart.

Web Studio's playlist, event and automation builders save directly; there is
no draft step.

## Audit log

`studio.yml` keeps the latest changes with time, player (or `CONSOLE`) and
action: screen controls, queue edits, group actions, automations, templates,
publishing, undo and emergency. Web Studio shows them under
**System → Recent changes**.

## Emergency mode

Turned on from Web Studio or the in-game menu, always with a confirmation.

While on:

- every screen shows a static `MAINTENANCE` frame
- events stop and automation pauses; new events, queued media and runtime
  source switches cannot take over the screen until it is off
- unused decoders shut down

Turning it off restores every screen's source and lets playlists pick again.
The state survives restarts, so check your screens after an unexpected
shutdown during emergency mode.

## Roles

`luigiscreen.menu.*` grants every menu section. Opening Web Studio
additionally needs `luigiscreen.web`, and a browser session only gets the
permissions the player had when the link was created.

| Permission | Access |
| --- | --- |
| `luigiscreen.menu.dashboard` | open the in-game Control Studio |
| `luigiscreen.menu.screens` | screens and location |
| `luigiscreen.menu.media` | media library |
| `luigiscreen.menu.playlists` | playlists |
| `luigiscreen.menu.events` | events |
| `luigiscreen.menu.live` | play media, run events, queues, votes |
| `luigiscreen.menu.control` | turn on/off, pause, skip, repeat, visibility |
| `luigiscreen.menu.groups` | screen groups |
| `luigiscreen.menu.schedules` | schedules (in-game) |
| `luigiscreen.menu.automations` | automations (Web Studio) |
| `luigiscreen.menu.templates` | starter templates |
| `luigiscreen.menu.diagnostics` | diagnostics and debug overlay |
| `luigiscreen.menu.monitoring` | monitoring in Web Studio |
| `luigiscreen.menu.history` | audit and undo |
| `luigiscreen.menu.emergency` | emergency mode |
| `luigiscreen.menu.configuration` | picture quality and other config in Web Studio, yt-dlp install |
| `luigiscreen.menu.settings` | Web Studio session settings |

Example roles:

| Role | Permissions |
| --- | --- |
| Content manager | `menu.dashboard`, `menu.media`, `menu.playlists`, `menu.templates` |
| Event operator | `menu.dashboard`, `menu.screens`, `menu.events`, `menu.live`, `menu.groups`, `menu.control` |
| Technician | `menu.dashboard`, `menu.screens`, `menu.diagnostics`, `menu.monitoring`, `luigiscreen.status`, `luigiscreen.debug` |
| Emergency moderator | `menu.dashboard`, `menu.emergency` |

(All prefixed with `luigiscreen.`.) Do not hand out history, emergency or
`luigiscreen.mediamtx` just so someone can look at screen status.

See [Permissions](../reference/permissions.md) for command permissions.
