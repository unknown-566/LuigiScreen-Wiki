# Events, Groups and Schedules

## Creating an event

Open **Events**, click **Create Event**, and enter an ID in chat.

The starter timeline contains:

1. a five-second text announcement
2. a manual operator wait

Right-click an event in the event list to inspect its timeline. Left-click it
to start it on the selected screen.

## Timeline editing

| Click | Action |
| --- | --- |
| Right-click step | Stage enable/disable |
| Shift-left step | Add five seconds to staged duration |
| Publish Changes | Snapshot config and activate the draft |
| Discard Draft | Drop uncommitted timeline changes |
| Take Live | Start this event on the selected screen |

## Event priority

```yaml
events:
  normal_announcement:
    priority: 30
  emergency_message:
    priority: 100
```

A higher-priority event may interrupt a lower-priority event. A lower-priority
event is rejected while a higher-priority event controls that screen. The
reason appears in playback history.

## Step types

Media types `rtmp`, `mjpeg`, `video`, `image`, `url-image`, `gif`,
`folder`, `text` and `countdown` work in event sequences.

Control step types:

| Type | Fields | Behavior |
| --- | --- | --- |
| `wait` | `duration` | keep current visual and wait |
| `wait-manual` | `text` | hold until an operator presses Resume/Next |
| `wait-viewers` | conditions | retry until viewer conditions pass |
| `wait-stream` | none | retry until the selected screen source is live |
| `command` | `command` | run one command as console |
| `broadcast` | `message` or `text` | send a server broadcast |
| `sound` | `sound` | play a Bukkit sound to online players |
| `title` | `text` | show a title to online players |
| `group` | `target`, `action`, `value` | control a screen group |
| `branch` | `then-event`, `else-event`, conditions | jump to another event |

Example:

```yaml
events:
  update_reveal:
    priority: 80
    sequence:
      countdown:
        type: countdown
        text: "Update starts soon"
        duration: 10s
      wait_for_obs:
        type: wait-stream
        duration: 1s
      live:
        type: rtmp
        value: "rtmp://127.0.0.1:55556/screen"
        duration: 5m
      operator:
        type: wait-manual
        text: "Waiting for operator"
        duration: 1s
      finish:
        type: broadcast
        message: "The reveal has finished."
        duration: 1s
```

## Branching

```yaml
      choose_path:
        type: branch
        conditions:
          min-viewers: 10
          tps-above: 18
        then-event: crowded_live
        else-event: backup_trailer
```

The branch starts the target event from its first step. Missing target events
are recorded in the playback reason instead of crashing the scheduler.

Avoid branches that loop forever without a wait or finite media step.

## Screen groups

Click **Create Screen Group** and answer:

```text
arena arena_left,arena_center,arena_right
```

The group page can start, stop or return all members to their own automation.
Group event/source steps execute in the same server tick, which provides
practical synchronization. Screens using the exact same source also share one
loader.

Group step example:

```yaml
      global_return:
        type: group
        target: arena
        action: return
        duration: 1s
```

Supported group actions are `start`, `stop`, `return`, `playlist` and
`event`.

## Schedule Calendar

Click **Create Schedule** and answer on one line:

```text
friday_cinema 20:00 cinema event cinema_night
```

Format:

```text
<id> <HH:mm> <screen_or_group> <event|playlist|start|stop|return> [value]
```

New schedules run every day. Edit their `days`, `priority` and `conflict`
fields in `studio.yml` when a narrower recurring calendar is needed.

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

Schedules sharing time, target and at least one day are marked as conflicts.
At runtime the highest priority wins. A lower schedule is skipped unless its
`conflict` policy is `allow`.

## Audience voting

Live Control can start a 60-second vote using up to three valid media files.

Players vote with:

```text
/screen vote <screen> <option>
```

Operators can also use:

```text
/screen vote start <screen> [option1 option2 ...]
/screen vote status <screen>
/screen vote end <screen>
```

One vote is stored per player. Changing a vote subtracts the previous choice.
The voter needs `luigiscreen.vote`, must be in the screen world and must be
within `voting.distance` from the screen. The winner is queued when voting
ends.

