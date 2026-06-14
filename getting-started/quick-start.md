# Quick Start

This guide assumes Paper, OBS and MediaMTX all run on the same computer.

## 1. Generate the MediaMTX setup

Join the server as an operator and run:

```text
/screen mediamtx same-pc
```

LuigiScreen creates:

```text
plugins/LuigiScreen/mediamtx/SAME_PC/mediamtx.yml
plugins/LuigiScreen/mediamtx/SAME_PC/setup.txt
```

`setup.txt` contains private generated credentials. Do not publish it.

## 2. Install MediaMTX

Download the MediaMTX build for your operating system from the official releases.

Place the generated `mediamtx.yml` beside:

```text
mediamtx.exe
```

On Linux, place it beside:

```text
mediamtx
```

Start MediaMTX. It should report an RTMP listener on TCP port `55556`.

## 3. Configure OBS

Open:

```text
Settings -> Stream
```

Use:

| Setting | Value |
| --- | --- |
| Service | Custom |
| Server | Copy the complete OBS URL shown by LuigiScreen or `setup.txt` |
| Stream key | Empty |
| Use authentication | Disabled |

The generated username and password are already inside the server URL query. Do not enter them again into OBS authentication fields.

## 4. Configure OBS output

Recommended starting values:

| Setting | Value |
| --- | --- |
| Resolution | `896x512` for a 7x4 screen |
| FPS | `10` |
| Video bitrate | `1200-2000 Kbps` |
| Encoder | Hardware H.264 when available |
| Keyframe interval | `2 seconds` |
| B-frames | `0` if configurable |

Add a display or window capture source, then click **Start Streaming**.

## 5. Create the Minecraft screen

Build a flat vertical wall at least 7 blocks wide and 4 blocks high.

Look at the upper-left block of the wall from the front and run:

```text
/screen create 7 4
```

LuigiScreen creates the MapEngine display and connects to the RTMP stream.

## 6. Check the status

Run:

```text
/screen status
```

For live statistics:

```text
/screen debug
```

If the screen remains offline, use [Common errors](../troubleshooting/common-errors.md).
