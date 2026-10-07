# Installation

1. Stop the server. FFmpeg libraries stay loaded until Java exits, so never hot-swap the JAR.
2. Put `LuigiScreen-1.3.0-alpha.1.jar` into `plugins/`. Keep only one LuigiScreen JAR.
3. Upgrading from 1.2 or older? You can delete `MapEngine.jar` — LuigiScreen no longer needs it.
4. Start the server.

The first start creates:

```text
plugins/LuigiScreen/config.yml
plugins/LuigiScreen/messages_en.yml
plugins/LuigiScreen/messages_cs.yml
plugins/LuigiScreen/media/
```

The console should show:

```text
[packetevents] Loaded packetevents v2.14.0 for LuigiScreen
[LuigiScreen] LuigiScreen Studio listening on 0.0.0.0:8765
```

Continue with the [Quick Start](quick-start.md).

## Language

The default language is English. For Czech set `language: cs` in `config.yml`
and run `/screen reload`.

## Updating

1. Stop Paper.
2. Back up `plugins/LuigiScreen/`.
3. Replace the JAR.
4. Start Paper and run `/screen list`.

Your screens, playlists, events and automations are kept. If you customized
`messages_*.yml`, new messages fall back to the bundled text automatically;
delete the files to get the new wording everywhere.
