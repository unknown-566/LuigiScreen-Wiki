# Events

An event is a temporary takeover: it interrupts a screen, plays its steps in
order and then returns the screen to its playlist or default source. Use it
for restart warnings, update reveals, countdowns or a short live segment.

| | Playlist | Event |
| --- | --- | --- |
| Order | weighted random | fixed steps |
| Ends by itself | no, keeps rotating | yes |
| Start | `/screen playlist <screen> <name>` | `/screen event <screen> <name>` |

## In Web Studio

1. **Program → Events**, create an event.
2. Add steps: media, text, countdown or *wait for operator*, each with a duration.
3. Start it on a screen: **Play… → Event**, `/screen event <screen> <name>`,
   or an [automation](automations.md).

`/screen event <screen> stop` (or **Return**) ends it early. Steps are saved
immediately. An event without steps cannot be started.

## In `config.yml`

```yaml
events:
  update_reveal:
    priority: 80
    sequence:
      countdown:
        type: countdown
        text: "Update starts soon"
        duration: 10s
      trailer:
        type: video
        value: trailers/update.mp4
        duration: 30s
      wait_for_obs:
        type: wait-stream
        duration: 1s
      live:
        type: rtmp
        value: "rtmp://127.0.0.1:55556/screen"
        duration: 5m
      finish:
        type: broadcast
        message: "The reveal has finished."
        duration: 1s
```

## Step types

Any media type (`video`, `image`, `gif`, `url-image`, `youtube`, `rtmp`,
`mjpeg`, `folder`, `text`, `countdown`) shows that media for `duration`.

Control steps (written in `config.yml`):

| Type | Fields | Does |
| --- | --- | --- |
| `wait` | `duration` | keep the current picture and wait |
| `wait-manual` | `text` | hold until an operator presses Pause/Skip |
| `wait-viewers` | `conditions` | retry until the viewer conditions pass |
| `wait-stream` | – | retry until the screen's source is live |
| `command` | `command` | run a console command |
| `broadcast` | `message` or `text` | chat broadcast |
| `sound` | `sound` | play a sound to online players |
| `title` | `text` | show a title to online players |
| `group` | `target`, `action`, `value` | control a [screen group](automations.md#screen-groups) |
| `branch` | `then-event`, `else-event`, `conditions` | jump to another event |

Branch example:

```yaml
      choose:
        type: branch
        conditions:
          min-viewers: 10
        then-event: crowded_live
        else-event: backup_trailer
```

Avoid branches that loop forever without a wait or a finite media step.

## Priority

```yaml
events:
  announcement:
    priority: 30
  emergency_message:
    priority: 100
```

A higher-priority event may interrupt a lower one; a lower one is rejected
while a higher one runs. Events always take precedence over the queue and the
playlist.
