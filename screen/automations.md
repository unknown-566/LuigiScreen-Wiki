# Automations, Groups and Voting

## Automations

An automation does something at a server time: run an event or a playlist, or
turn a screen on, off or back to its program.

In Web Studio open **Program → Automations**, create a rule, choose the time,
the target screen or group and the action, then save. **Run now** tests a
rule immediately.

| Action | Value | Does |
| --- | --- | --- |
| `event` | event name | starts the event |
| `playlist` | playlist name | assigns and starts the playlist |
| `start` | – | turns the target on |
| `stop` | – | turns the target off |
| `return` | – | back to the normal program |

Rules made in Web Studio run every day. For specific weekdays edit `days` in
`plugins/LuigiScreen/studio.yml`:

```yaml
schedules:
  friday_cinema:
    enabled: true
    days: [FRIDAY]
    time: "20:00"
    target: cinema
    action: event
    value: cinema_night
    priority: 50
    conflict: priority
```

Rules with the same time, target and a shared day conflict. The highest
`priority` wins; a lower rule is skipped unless its `conflict` is `allow`.

## Screen groups

A group lets one button, rule or event step control several screens together.
Create groups in **Program → Automations** (or the in-game Control Studio).
Group actions are `start`, `stop`, `return`, `playlist` and `event`; all
members switch in the same server tick.

```yaml
groups:
  arena:
    screens: [arena_left, arena_center, arena_right]
```

## Audience voting

A vote lets nearby players pick what plays next. The winner is queued when the
vote ends (after 60 seconds or when ended manually).

| Command | Who |
| --- | --- |
| `/screen vote start <screen> [option1 option2 …]` | operators (`luigiscreen.menu.live`); without options, up to three library files are used |
| `/screen vote <screen> <option>` | players with `luigiscreen.vote`, in the screen's world and within `voting.distance` |
| `/screen vote end <screen>` | operators |

Each player has one vote; voting again changes it. Votes can also be started
from Live Control in the in-game Control Studio.
