# MediaMTX

MediaMTX receives the video from OBS and provides it to LuigiScreen.

## Use the generated configuration

Run the appropriate `/screen mediamtx ...` command first.

Copy:

```text
plugins/LuigiScreen/mediamtx/<SITUATION>/mediamtx.yml
```

beside the MediaMTX executable.

## Windows

Expected layout:

```text
MediaMTX/
  mediamtx.exe
  mediamtx.yml
```

Start `mediamtx.exe`.

## Linux

Expected layout:

```text
/opt/mediamtx/
  mediamtx
  mediamtx.yml
```

Make the binary executable and start it:

```bash
chmod +x mediamtx
./mediamtx
```

For 24/7 operation, create a systemd service or use Docker.

## Generated security model

The generated config:

- Enables RTMP
- Uses TCP port `55556` by default
- Disables RTSP, HLS, WebRTC, SRT and MoQ
- Gives `streamer` publish-only access to path `screen`
- Restricts local readers to loopback
- Creates a separate `luigiscreen` read-only account for external hosting

## Restart requirement

After replacing `mediamtx.yml`, restart MediaMTX. MediaMTX must show that its RTMP listener is active on the expected port.

## Private files

Do not upload these files publicly:

```text
setup.txt
mediamtx.yml
config.yml
```

They can contain generated credentials or private network information.
