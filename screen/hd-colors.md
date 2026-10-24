# HD Colours

Minecraft maps can only show a fixed palette of about 240 colours. With HD
colours, LuigiScreen offers players a small resource pack that replaces one core
shader. Screens then show far more colours, close to the original video. It is
not a mod: the client loads it like any other resource pack.

Players who decline the pack, or whose client can't use it, keep seeing normal
map colours. Both kinds of players can watch the same screen at the same time.

Only Minecraft **1.21.11** for now. On 26.x servers the pack is not offered.

## How players get it

When a player joins, the server asks them to load the pack. The pack is
downloaded from Web Studio, so:

- Web Studio must be enabled (`web-studio.enabled: true`)
- players must be able to reach the Web Studio port, or you set
  `web-studio.public-url` to an address they can reach

If the download fails, the player simply gets normal colours.

Sodium and Iris replace the shader the pack relies on, so players using them
see normal colours.

## Turning it off

For one player:

```text
/screen rp disable            your own pack
/screen rp disable <player>   another player (needs luigiscreen.rp)
/screen rp enable [player]    turn it back on
```

The pack is removed from the client right away and that player's screens switch
to normal colours. The choice is saved in `plugins/LuigiScreen/hd-disabled.txt`,
so the player isn't asked again on the next join.

For the whole server, in `config.yml`:

```yaml
hd-colors:
  enabled: false
```

Then restart the server. The setting is also in Web Studio → System → Picture quality.

## Cost

Encoding HD frames takes a few milliseconds per frame on the server. Players
with the pack get different map data than players without it, so a screen with
both kinds of viewers does the work twice. On the client the shader costs well
under a millisecond per frame.
