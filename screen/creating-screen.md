# Creating Screens

## Create

Build a flat wall facing north, south, east or west. Stand in front of it, look
at its **top-left** block and run:

```text
/screen create <name> [width] [height]
```

```text
/screen create lobby 4 3
/screen create cinema 16 9
```

For a 16:9 picture use 8×5 (close enough), 16×9 or 32×18. Not sure? Web
Studio → **New screen** lets you pick a ratio and shows the size as a block grid.

Width and height are in blocks (one map each, 128×128 pixels). Without a size
you get 4×3. Names use lowercase letters, numbers, `_` and `-`, up to 32
characters.

A new screen starts with the default source from `config.yml`. Give it
something to show with `/screen play` or `/screen source`, see
[What you can play](sources.md).

**Wrong direction?** Remove it, stand on the other side of the wall and target
the top-left block from there.

## Several screens

Create as many as you like, in any world. Each keeps its own source, playlist,
FPS, viewing distance and visibility.

Copy an existing screen to the wall you look at:

```text
/screen clone lobby spawn
```

Screens that show exactly the same source share one decoder; each screen only
scales and sends the frame for its own size and viewers. Give one of them
something else and it gets its own decoder.

Control several at once with `/screen on all`, `/screen off all`, or with
[screen groups](automations.md#screen-groups).

## Settings

| Command | Range |
| --- | --- |
| `/screen set <screen> fps <value>` | `0.1`–`60` (default `20`) |
| `/screen set <screen> distance <blocks>` | `8`–`1024` (default `64`) |
| `/screen set <screen> private true` | only players with `luigiscreen.see.<screen>` see it |
| `/screen on <screen>`, `/screen off <screen>` | turn on or off; an off screen stays saved |

The same settings are on the screen page in Web Studio.

## Size limits

```yaml
screen:
  max-width: 32
  max-height: 18
  max-total-maps: 576
```

Configs from older versions that still have the old 10×6 limit are raised to
these values automatically.

Bigger screens cost more bandwidth for every viewer. See
[Performance](../operations/performance.md).

## Remove

```text
/screen remove lobby
```

Click **[Delete]** in chat to confirm.
