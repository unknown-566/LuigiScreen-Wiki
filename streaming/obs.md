# OBS Studio

## Stream settings

Open:

```text
Settings -> Stream
```

Use:

| Setting | Value |
| --- | --- |
| Service | Custom |
| Server | Complete generated URL |
| Stream key | Empty |
| Use authentication | Disabled |

Example format only:

```text
rtmp://HOST:PORT/screen?user=streamer&pass=GENERATED_PASSWORD
```

Never publish the real generated URL.

## Why authentication is disabled in OBS

LuigiScreen puts the generated publisher credentials directly into the RTMP URL. Enabling OBS's separate authentication fields can produce a different connection format and cause authentication failures.

## Recommended output

For a 7x4 screen:

```text
896x512
```

Recommended starting values:

```text
FPS: 10
Video bitrate: 1500 Kbps
Audio bitrate: 96 Kbps
Keyframe interval: 2 seconds
B-frames: 0
Tune: zerolatency
```

Use a hardware H.264 encoder when available. Software x264 also works but consumes more CPU.

## Add a source

In the OBS Sources panel:

1. Click `+`.
2. Choose **Display Capture**, **Window Capture** or **Media Source**.
3. Confirm that the preview contains video.
4. Click **Start Streaming**.

A black OBS preview means MediaMTX receives a valid stream containing a black frame. Fix the OBS scene before debugging LuigiScreen.

## Test with VLC

On a computer that can reach MediaMTX, open the LuigiScreen reader URL in VLC:

```text
Media -> Open Network Stream
```

If VLC cannot read the stream, the problem is before LuigiScreen: OBS, MediaMTX, authentication, firewall or routing.
