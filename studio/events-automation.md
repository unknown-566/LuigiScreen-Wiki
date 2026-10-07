# Events, Groups and Automations

## Creating an event in Web Studio

Open **Web Studio → Program → Events**, create an event and add steps:
media, text, countdown or *wait for operator*, each with a duration.

Start it from the screen (**Play… → Event**), with
`/screen event <screen> <name>`, or from an automation. `/screen event
<screen> stop` ends it early.

An event is a temporary takeover. It interrupts the target screen, plays its
steps in order, and then returns the screen to its normal playlist or default
source. Changes are saved immediately. Control steps such as `command`,
`branch` or `group` (below) are written in `config.yml`.

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

## Automation builder

Open **Web Studio → Program → Automations**, create a rule, set the server
time, the target screen or group and the action, then save.

An automation rule is intentionally written like a small sentence:

```text
WHEN 20:00
IF every configured day
THEN event cinema_night on cinema
```

Supported actions:

| Action | Value | Behavior |
| --- | --- | --- |
| `event` | event id | starts a temporary event takeover |
| `playlist` | playlist id | assigns and starts a playlist |
| `start` | none | starts the target screen |
| `stop` | none | stops the target screen |
| `return` | none | returns the target to normal automation |

**Run now** executes a rule immediately to test it. Changes are saved
immediately; live updates never overwrite fields you are still editing.

Rules created in Web Studio run every day by default. Edit `days` in
`studio.yml` when a narrower recurring calendar is needed.

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

Automations sharing time, target and at least one day are marked as conflicts.
At runtime the highest priority wins. A lower automation is skipped unless its
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
