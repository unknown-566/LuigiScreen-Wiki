# Creating a Screen

Build a flat vertical wall facing north, south, east or west. Stand in front
of it and look at the upper-left block.

Create a named 7x4 screen:

```text
/screen create main 7 4
```

Each map is 128x128 pixels, so 7x4 produces an 896x512 output.

## Names and sizes

```text
/screen create <name> [width] [height]
```

Examples:

```text
/screen create lobby 5 3
/screen create cinema 10 6
```

Names accept lowercase letters, numbers, `_` and `-`. The command converts
uppercase input to lowercase.

For compatibility, `/screen create 7 4` creates `main`.

Default per-screen safety limits:

```yaml
screen:
  max-width: 10
  max-height: 6
  max-total-maps: 60
```

## More than one screen

Creating a new named screen no longer removes the previous one. Use
`/screen list` to inspect the registry and `/screen remove <name>` to remove a
specific display.

Use [Multiple Screens and Clones](multiple-screens.md) when several displays
should show the same video.

## Orientation

If a screen extends in the wrong direction, remove it, stand on the opposite
side of the wall and select the upper-left block from that side.
