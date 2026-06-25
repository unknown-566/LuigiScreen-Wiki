# Playlist Studio and Conditions

## Web Studio playlist builder

For the browser workflow, open **Web Studio -> Playlist Editor**.

The normal path is:

1. Press **Create playlist**.
2. Open the playlist card.
3. Select a media source in **Add media**.
4. Choose duration and weight.
5. Press **Add item**.
6. Pick a screen in **Play this playlist**.
7. Press **Assign and play**.

New browser-created playlists start empty. There is no fake starter item to
delete. **Add item** writes the media item directly into the playlist and
reloads playback definitions safely.

Visible browser actions:

| Action | What it does |
| --- | --- |
| **Add item** | Adds selected media with duration, weight and enabled state |
| **Delete item** | Removes one playlist item |
| **Duplicate** | Creates a copy of the playlist under a new ID |
| **Delete playlist** | Removes the playlist and clears it from assigned screens |
| **Assign and play** | Saves the playlist on the chosen screen and starts it |

Advanced fields such as cooldown, conditions and enabled state are still edited
through the inspector/draft system so risky changes can be reviewed before
publishing.

## In-game Control Studio playlist editor

Open **Playlists** and click **Create Playlist**. The GUI closes and asks for
one ID in chat:

```text
cinema_night
```

The response is private to the prompt. Type `cancel` to stop.

A safe starter playlist is created for the in-game editor. It can be replaced
with real media later. The browser Web Studio path above creates an empty
playlist instead, because the browser has a direct **Add item** form.

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
