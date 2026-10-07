# Creating a Screen

Build a flat wall facing north, south, east or west. Stand in front of it, look
at its **top-left** block and run:

```text
/screen create <name> [width] [height]
```

```text
/screen create lobby 4 3
/screen create cinema 10 6
```

Width and height are in blocks (one map each, 128×128 pixels). Without a size
you get 4×3. Names use lowercase letters, numbers, `_` and `-`.

The new screen starts with the default source from `config.yml`. Give it
something to show with `/screen play` or `/screen source` — see
[Media Sources](sources.md).

## Limits

```yaml
screen:
  max-width: 10
  max-height: 6
  max-total-maps: 60
```

Bigger screens cost more bandwidth for every viewer. See [Performance](../operations/performance.md).

## Wrong direction?

If the screen grows the wrong way, remove it, stand on the other side of the
wall and target the top-left block from there.

## Remove a screen

```text
/screen remove lobby
```

LuigiScreen asks for confirmation; click **[Delete]** to remove it.
