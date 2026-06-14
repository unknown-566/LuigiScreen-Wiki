# Debug Statistics

Run:

```text
/screen debug
```

Run it again to disable debugging.

## Boss bar pages

The boss bar cycles through:

1. Stream state, source resolution and last-frame age
2. Total/enabled screens, shared sources, primary screen maps, viewers and FPS
3. Received, rendered and replaced frames
4. Last and average render time
5. JVM, non-heap and system memory
6. Estimated LuigiScreen image buffers
7. Process CPU, system CPU and threads
8. TPS, MSPT, uptime and garbage collection
9. Reconnect and error health

## Sidebar

The 15-line sidebar shows the most important values simultaneously:

- Stream state
- Source resolution
- Total screens, enabled screens and shared source count
- Primary screen size
- Output resolution
- Viewer count
- Input and output FPS
- Frame counters
- Queue and replaced frames
- Render timings
- Heap usage
- Estimated buffers
- CPU usage
- TPS and MSPT
- Uptime and threads
- Reconnects and current error

## Scoreboard compatibility

LuigiScreen stores the player's previous scoreboard and restores it when debug mode is disabled.

If another plugin replaces the scoreboard while LuigiScreen debug is active, LuigiScreen does not overwrite that newer scoreboard and does not restore over it.

## Memory limitations

The displayed buffer estimate includes LuigiScreen Java image buffers.

It cannot accurately assign all native FFmpeg or MapEngine memory to this plugin. Use operating-system tools or a profiler for complete process memory.
