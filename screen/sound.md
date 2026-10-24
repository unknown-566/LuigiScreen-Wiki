# Sound

Screens can play the sound of videos, YouTube links and streams through the
[Simple Voice Chat](https://modrinth.com/plugin/simple-voice-chat) plugin.

- Install Simple Voice Chat on the server. LuigiScreen finds it on its own.
- Players need the Simple Voice Chat mod to hear anything. Players without it
  still see the picture.
- The sound comes from the screen, so it gets quieter as you walk away.
- Players set the volume in the voice chat menu.

Without Simple Voice Chat on the server, screens are silent.

```yaml
audio:
  enabled: true
  # 0.0 - 2.0, applied before voice chat's own per-player volume
  volume: 1.0
```
