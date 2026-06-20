# Screens and Live Control

## Screen overview

Each screen icon represents its current state:

- live direct source
- playlist
- event
- paused
- stopped
- waiting or reconnecting
- source error

Its lore shows the current item, next queued or automatic item, controlling
playlist/event, remaining time, viewers and effective/target FPS.

Click behavior:

| Click | Action |
| --- | --- |
| Left | Open screen detail |
| Right | Start or stop the screen |
| Shift-left | Select it and open Live Control |

## Screen detail

### Playback

- **Hold/Resume** freezes or resumes automation.
- **Skip** advances to the next event, queue or playlist decision.
- **Repeat** locks the current source until repeat is disabled.
- **Return to Automation** clears event control, queue, hold and repeat.
- **Content Queue** opens the queue editor.

### Why is this playing?

This item explains the current decision. Examples:

```text
Selected by playlist spawn_rotation with weight 10.
All configured conditions passed.
```

```text
Event update_reveal took control with priority 80.
```

```text
This item was manually queued by an administrator.
```

Playback History stores the latest 20 runtime decisions for the screen.

### Location

- **Teleport** moves the admin three blocks in front of the display.
- **Highlight Bounds** draws temporary particles around both screen corners.
- **Repair and Resync** respawns the virtual MapEngine frames for viewers and
  forces a full map resend.

Repair does not change the saved location or dimensions.

### Visibility

The visibility button toggles `permission-required`.

When enabled, viewers need:

```text
luigiscreen.see.<screen>
```

### Health

Health shows:

- source state and last error
- age of the latest frame
- reconnect count
- received, rendered and replaced frame counts
- effective and target FPS
- last and average render time
- viewers and shared-screen count

Usage Statistics shows plays, planned playback time, viewer-time, average
viewers, skips and failures. Statistics are aggregate; no personal viewing
history is stored.

## Queue editor

Every screen has its own queue.

| Click | Queue action |
| --- | --- |
| Left | Play this queued item now |
| Right | Remove it |
| Shift-left | Move it one position up |
| Clear Queue | Remove all waiting items |

An active event has priority over the manual queue. After the event ends, the
queue continues before normal playlist selection.

## Live Control Room

Live Control is for operating an event without opening configuration:

- **Preview/Media Library** chooses a source.
- Left-click media to take it now.
- Right-click media to cue it as Next.
- **Take Live/Next** advances to the first queued source.
- **Hold/Resume** freezes or resumes the current item.
- **Events** opens available timelines for the selected screen.
- **End Event** returns from an active event.
- **Return** clears temporary control and restores normal automation.
- **Audience Vote** starts or ends a vote.
- **Emergency** opens a separate confirmation page.

There is no invisible second decoder for Preview. In this alpha, preview means
validating and cueing the source before Take Live. A staff-only preview screen
can be created as a normal permission-protected screen.

