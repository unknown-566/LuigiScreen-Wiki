# Playlist Studio and Conditions

## Creating a playlist

Open **Playlists** and click **Create Playlist**. The GUI closes and asks for
one ID in chat:

```text
cinema_night
```

The response is private to the prompt. Type `cancel` to stop.

A safe starter playlist is created with one text item. It can play immediately
and can be replaced with real media later.

## Inspect and assign

On the playlist list:

- left-click assigns the playlist to the selected screen
- right-click opens its item editor and probability simulation

Each item shows type, value, weight, estimated chance, duration, cooldown,
conditions and current eligibility for the selected screen.

## Item actions

| Click | Action |
| --- | --- |
| Left | Preview the source immediately on the selected screen |
| Right | Stage enabled/disabled |
| Shift-left | Increase staged weight by one |
| Middle-click | Open Condition Builder in chat |

Changes are drafts. The live playlist continues using the published values.

Use **Publish Changes** to create a config snapshot and activate every staged
change together. **Discard Draft** removes uncommitted changes.

## Weight simulation

The simulation performs 1,000 weighted rolls and reports approximate
percentages. It is not a prediction of the exact future order.

Conditions, cooldown and runtime anti-repeat state are intentionally excluded
from the raw probability simulation. The Eligibility line explains whether an
item may be used now.

## Anti-repeat

Playlist options:

```yaml
playlists:
  spawn_rotation:
    history-window: 3
    category-history-window: 1
    items:
      discord:
        type: video
        value: ads/discord.mp4
        weight: 1
        category: advertisement
        cooldown: 5m
        guaranteed-after: 30m
```

| Setting | Meaning |
| --- | --- |
| `history-window` | Exclude items used in the latest N choices when another item is eligible |
| `category-history-window` | Avoid recently used categories |
| `category` | Group related items, such as `advertisement` |
| `cooldown` | Block the item for a fixed time after use |
| `guaranteed-after` | Select the eligible item once it has been absent this long |

If anti-repeat would remove every eligible item, LuigiScreen relaxes the
history filter instead of leaving the screen blank.

## Condition Builder

Middle-click an item and answer with comma-separated blocks:

```text
min-viewers=3,tps-above=18,world=world,days=FRIDAY|SATURDAY
```

Supported keys:

| Key | Test |
| --- | --- |
| `min-online`, `max-online` | total online player range |
| `min-viewers`, `max-viewers` | nearby authorized screen viewers |
| `viewer-permission` | at least one viewer has the permission |
| `all-viewers-permission` | every current viewer has the permission |
| `tps-above`, `tps-below` | current one-minute TPS range |
| `world` | screen is in this world |
| `days` | real weekdays separated with `|` |

The values are staged and require **Publish Changes**.

## Why not playing?

Eligibility can report:

- item disabled
- cooldown time remaining
- a failed condition with current and required values
- exclusion by the anti-repeat window
- eligible but not chosen by the latest weighted roll

This is evaluated against the screen currently selected in Control Studio.

