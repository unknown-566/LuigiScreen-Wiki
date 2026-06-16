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
| `luigiscreen.create` | `/screen create` |
| `luigiscreen.clone` | `/screen clone` |
| `luigiscreen.list` | `/screen list` |
| `luigiscreen.start` | `/screen start` |
| `luigiscreen.stop` | `/screen stop` |
| `luigiscreen.remove` | `/screen remove` |
| `luigiscreen.status` | `/screen status` |
| `luigiscreen.source` | `/screen source` |
| `luigiscreen.playlist` | `/screen playlist` |
| `luigiscreen.event` | `/screen event` |
| `luigiscreen.set` | `/screen set` |
| `luigiscreen.reload` | `/screen reload` |
| `luigiscreen.debug` | `/screen debug` |
| `luigiscreen.mediamtx` | `/screen mediamtx` |

All individual permissions default to false. Operators receive them through
`luigiscreen.admin`.

## Screen visibility

New screens are public by default:

```yaml
permission-required: false
```

Protect a screen:

```text
/screen set cinema permission true
```

Players now need:

```text
luigiscreen.see.cinema
```

Wildcard access:

```text
luigiscreen.see.*
```

Players without access do not receive or see that MapEngine display. Permission
changes are detected by the regular viewer refresh, so a restart is not
required.

## LuckPerms examples

Allow a moderator to start, stop and inspect screens:

```text
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
