# How It Works

LuigiScreen is the final part of a small video pipeline.

```mermaid
flowchart LR
    A["Desktop, game or video"] --> B["OBS Studio"]
    B -->|RTMP publish| C["MediaMTX"]
    C -->|RTMP read| D["LuigiScreen / FFmpeg"]
    D --> E["Latest-frame queue"]
    E --> F["MapEngine"]
    F --> G["Minecraft map wall"]
```

## Component responsibilities

### OBS Studio

Captures and encodes the source into H.264 video. OBS publishes the stream but does not communicate with Minecraft directly.

### MediaMTX

Accepts the published RTMP stream and makes it available to readers. The generated configuration gives OBS publish-only access and, for external hosting, gives LuigiScreen a separate read-only account.

### FFmpeg and JavaCV

LuigiScreen uses bundled native FFmpeg libraries to decode video frames. The current artifact includes Windows x64 and Linux x64 natives.

### Latest-frame queue

Only the newest waiting frame is retained. If decoding is faster than map rendering, an older queued frame is replaced instead of building latency.

### MapEngine

Creates the client-side map display and sends map updates to nearby players.

## Main-thread safety

RTMP decoding runs on a dedicated worker thread. Map frame preparation runs on a separate scheduled executor. Bukkit player and screen lifecycle operations are coordinated with the server thread.

## Viewer pause

By default, the RTMP decoder disconnects when no players are within `screen.viewer-distance`. When a viewer returns, LuigiScreen reconnects.

This saves CPU but means the first returning viewer sees a short reconnect delay.

## Offline frames

Instead of leaving a frozen video frame, LuigiScreen can show states such as:

- Connecting
- Waiting for stream
- Stream offline
- Stream stopped

## Screen persistence

The screen world, corners and facing are stored in `config.yml`. LuigiScreen recreates the screen after a normal server restart when the saved location remains valid.
