# Creating a Screen

## Build the wall

Use a flat vertical wall. LuigiScreen supports walls facing north, south, east or west.

The default screen size is:

```text
7 maps wide x 4 maps high
```

Each map represents `128x128` pixels, so a 7x4 screen outputs:

```text
896x512 pixels
```

## Select the correct corner

Stand in front of the wall and look directly at its **upper-left block**.

Run:

```text
/screen create 7 4
```

The command ray-traces up to 10 blocks. It fails if:

- No block is targeted
- The targeted face is the floor or ceiling
- The requested size exceeds configured limits
- The previous decoder is still shutting down

## Custom sizes

Syntax:

```text
/screen create <width> <height>
```

Examples:

```text
/screen create 5 3
/screen create 10 6
```

Default safety limits:

```yaml
screen:
  max-width: 10
  max-height: 6
  max-total-maps: 60
```

Both dimension limits and the total map limit must pass.

## Recreating a screen

Creating a new screen removes the old display only after the previous RTMP decoder stops safely.

## Removing a screen

Run:

```text
/screen remove
```

This removes the display and clears its saved location.

## Orientation issues

If the screen extends in the wrong direction, stand on the opposite side of the wall and select the upper-left block from that side.
