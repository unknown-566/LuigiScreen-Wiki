# Permissions

LuigiScreen uses granular permissions.

## Administrator permission

```text
luigiscreen.admin
```

It is granted to operators by default and includes every command permission,
`luigiscreen.see.*`, and access to every protected screen.

## Command permissions

| Permission | Command |
| --- | --- |
| `luigiscreen.play` | `/screen play`, `queue`, `pause`, `skip`, `return` |
| `luigiscreen.create` | `/screen create` |
| `luigiscreen.clone` | `/screen clone` |
| `luigiscreen.list` | `/screen list` |
| `luigiscreen.start` | `/screen on` |
| `luigiscreen.stop` | `/screen off` |
| `luigiscreen.remove` | `/screen remove` |
| `luigiscreen.status` | `/screen info` |
| `luigiscreen.source` | `/screen source` |
| `luigiscreen.playlist` | `/screen playlist` |
| `luigiscreen.event` | `/screen event` |
| `luigiscreen.set` | `/screen set` |
| `luigiscreen.reload` | `/screen reload` |
| `luigiscreen.debug` | `/screen debug` |
| `luigiscreen.mediamtx` | `/screen obs` |
| `luigiscreen.menu.dashboard` | `/screen menu` |
| `luigiscreen.web` | `/screen web` and Web Studio login links |
| `luigiscreen.vote` | Cast a vote with `/screen vote` |

Update notifications use a separate permission:

| Permission | Purpose |
| --- | --- |
| `luigiscreen.update` | Receive a clickable message when a newer public Modrinth version exists |

All individual command permissions default to false. `luigiscreen.update`
defaults to operators. Operators also receive everything through
`luigiscreen.admin`.

## Screen visibility

New screens are public by default:

```yaml
permission-required: false
```

Protect a screen:

```text
/screen set cinema private true
```

Players now need:

```text
luigiscreen.see.cinema
```

Wildcard access:

```text
luigiscreen.see.*
```

Players without access are never sent that screen. Permission
changes are detected by the regular viewer refresh, so a restart is not
required.

## LuckPerms examples

Allow a moderator to play media and turn screens on/off:

```text
/lp user PLAYER permission set luigiscreen.play true
/lp user PLAYER permission set luigiscreen.start true
/lp user PLAYER permission set luigiscreen.stop true
/lp user PLAYER permission set luigiscreen.status true
```

Allow a player to see only `cinema`:

```text
/lp user PLAYER permission set luigiscreen.see.cinema true
```

Allow a group to see every protected screen:

```text
/lp group vip permission set luigiscreen.see.* true
```

Do not grant `luigiscreen.mediamtx` to untrusted users because its wizard
generates private credentials.

## Control Studio roles

`luigiscreen.menu.*` grants every studio section.

| Permission | Section/action |
| --- | --- |
| `luigiscreen.menu.dashboard` | Dashboard |
| `luigiscreen.menu.screens` | Screen list, details and location |
| `luigiscreen.menu.media` | Media Library |
| `luigiscreen.menu.playlists` | Playlist inspection and drafts |
| `luigiscreen.menu.events` | Event timeline inspection and drafts |
| `luigiscreen.menu.live` | Live Control, queue, event start and vote management |
| `luigiscreen.menu.groups` | Screen Groups |
| `luigiscreen.menu.schedules` | Schedule Calendar |
| `luigiscreen.menu.templates` | Template installation |
| `luigiscreen.menu.diagnostics` | Diagnostics |
| `luigiscreen.menu.history` | Audit and Undo |
| `luigiscreen.menu.emergency` | Emergency confirmation |
| `luigiscreen.menu.control` | Start/stop/hold/skip/repeat/visibility mutations |
| `luigiscreen.menu.automations` | Web Studio automation workspace |
| `luigiscreen.menu.monitoring` | Web Studio monitoring workspace |
| `luigiscreen.menu.configuration` | Structured drafts and Publish in Web Studio |
| `luigiscreen.menu.settings` | Web Studio settings and session workspace |

`luigiscreen.web` only permits creating a browser session. Every API action
still checks the section and command capabilities copied from the player when
the one-time link was created. Create a new session after changing that
player's permissions.

See [Drafts, History, Emergency and Roles](../studio/safety-roles.md) for
recommended staff roles.
