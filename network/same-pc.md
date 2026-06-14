# Same Computer

Use this setup when Paper, MediaMTX and OBS all run on the same computer.

## Generate configuration

Run:

```text
/screen mediamtx same-pc
```

## Install MediaMTX

Copy the generated file from:

```text
plugins/LuigiScreen/mediamtx/SAME_PC/mediamtx.yml
```

to the MediaMTX folder and restart MediaMTX.

## Configure OBS

Copy the complete OBS server URL shown by the command or stored in the private `setup.txt`.

Keep:

```text
Stream key: empty
Use authentication: disabled
```

## Network changes

No router port forwarding is required.

The plugin reads:

```text
rtmp://127.0.0.1:55556/screen
```

This setup cannot work when the Minecraft server is hosted on another physical machine.
