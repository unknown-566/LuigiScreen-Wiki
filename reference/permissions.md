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

## Web Studio and menu sections

`luigiscreen.web` lets a player create a Web Studio login link. What the
browser session may do is limited by the player's `luigiscreen.menu.*` section
permissions and command permissions, copied when the link was created. After
changing them, run `/screen web revoke` and open a new link.

The full list of `luigiscreen.menu.*` sections and example staff roles is in
[Safety and Roles](../studio/safety-roles.md#roles).
