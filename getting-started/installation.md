# Installation

## 1. Stop the server

Do not replace LuigiScreen while Paper is running. Native FFmpeg libraries remain loaded until the Java process exits.

## 2. Install MapEngine

Place the compatible MapEngine JAR in:

```text
plugins/
```

Required version for the current LuigiScreen alpha:

```text
MapEngine 1.8.12
```

## 3. Install LuigiScreen

Place the LuigiScreen JAR in:

```text
plugins/
```

Keep only one LuigiScreen JAR. For example, remove
`LuigiScreen-1.1.0-alpha.11.jar` before adding
`LuigiScreen-1.1.0-alpha.12.jar`.

## 4. Start the server

The first start creates:

```text
plugins/LuigiScreen/config.yml
plugins/LuigiScreen/messages_cs.yml
plugins/LuigiScreen/messages_en.yml
plugins/LuigiScreen/media/
```

## 5. Verify startup

The console should show both plugins enabled without an exception:

```text
Enabling MapEngine v1.8.12
Enabling LuigiScreen v1.1.0-alpha.12
```

Run:

```text
/plugins
```

MapEngine and LuigiScreen should both appear enabled.

## 6. Choose a source

Continue with [Media sources](../screen/sources.md). Only RTMP sources need a
[network setup](../streaming/overview.md).

## Updating

1. Stop Paper completely.
2. Back up `plugins/LuigiScreen/`.
3. Remove the old LuigiScreen JAR.
4. Add the new JAR.
5. Start the server.
6. Check the console.
7. Run `/screen status`.

Do not use plugin hot-reload tools for native FFmpeg updates.
