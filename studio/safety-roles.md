# Drafts, History, Emergency and Roles

## Draft mode

Playlist and event editor clicks do not immediately change live playback.
They are kept per admin as an in-memory draft.

**Publish Changes**:

1. creates a complete `config.yml` snapshot
2. applies all staged paths together
3. saves `config.yml`
4. reloads playback definitions
5. records the actor and change count

**Discard Draft** removes that admin's pending changes.

Drafts are intentionally lost on server restart. A draft is unfinished work,
not persistent configuration.

## Snapshots and undo

Snapshots are stored in:

```text
plugins/LuigiScreen/history/config-YYYYMMDD-HHMMSS-SSS.yml
```

The number retained follows `history.max-entries` in `studio.yml`, default
20.

Open **Change History** and click **Undo Last Publish** to copy the newest
snapshot back to `config.yml` and run the normal safe reload.

Undo restores the whole configuration, not only one playlist. The used
snapshot is removed after a successful restore.

## Audit

`studio.yml` keeps the latest changes with:

- timestamp
- player name or `CONSOLE`
- action

Examples include screen controls, queue edits, group actions, schedule runs,
template installation, publishing, undo and emergency state.

## Emergency mode

Emergency requires a separate confirmation page.

Enabling it:

- stops active events
- pauses automation on every screen
- detaches media sources
- allows unused decoders to stop
- shows a static `MAINTENANCE` frame

Disabling it:

- clears pause
- restores every configured source
- resets playback so playlists may select again

Emergency state is persisted in `studio.yml`. Always verify screens after an
unexpected server shutdown during emergency mode.

## Role permissions

`luigiscreen.menu.*` grants all Control Studio sections.

Individual sections:

| Permission | Access |
| --- | --- |
| `luigiscreen.menu.dashboard` | open Control Studio |
| `luigiscreen.menu.screens` | inspect screens and location |
| `luigiscreen.menu.media` | inspect Media Library |
| `luigiscreen.menu.playlists` | inspect and draft playlist changes |
| `luigiscreen.menu.events` | inspect and draft event changes |
| `luigiscreen.menu.live` | cue media, run events, queues and votes |
| `luigiscreen.menu.groups` | inspect and control groups |
| `luigiscreen.menu.schedules` | inspect and create schedules |
| `luigiscreen.menu.templates` | install starter templates |
| `luigiscreen.menu.diagnostics` | diagnostics and debug overlay |
| `luigiscreen.menu.history` | audit and config undo |
| `luigiscreen.menu.emergency` | confirm global emergency mode |
| `luigiscreen.menu.control` | mutate basic screen/playback state |

`luigiscreen.admin` includes every section and action.

Suggested roles:

### Content Manager

```text
luigiscreen.menu.dashboard
luigiscreen.menu.media
luigiscreen.menu.playlists
luigiscreen.menu.templates
```

### Event Operator

```text
luigiscreen.menu.dashboard
luigiscreen.menu.screens
luigiscreen.menu.events
luigiscreen.menu.live
luigiscreen.menu.groups
luigiscreen.menu.control
```

### Technician

```text
luigiscreen.menu.dashboard
luigiscreen.menu.screens
luigiscreen.menu.diagnostics
luigiscreen.status
luigiscreen.debug
```

### Emergency Moderator

```text
luigiscreen.menu.dashboard
luigiscreen.menu.emergency
```

Do not grant History/Undo, MediaMTX or Emergency permissions merely so someone
can inspect screen status.

