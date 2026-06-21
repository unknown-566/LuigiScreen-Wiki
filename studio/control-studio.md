# Control Studio

Control Studio is LuigiScreen's administration layer. It has two interfaces
over the same playback engine:

- the in-game inventory interface added in `1.2.0-alpha.1`
- the browser-based [Web Studio](web-studio.md) added in `1.2.0-alpha.2`

Open the in-game interface with:

```text
/screen menu
```

`/screen studio` is an equivalent alias.

The studio is not a second playback system. Every button controls the same
screen registry, shared media loaders, playlists and events used by the
`/screen` commands.

Open Web Studio with `/screen web`. It adds a larger monitoring workspace,
Preview/Program live control, structured drafts and clean contextual hover
help while preserving the same permissions and safety rules.

## Dashboard

The first page shows:

- enabled and total screens
- screens with a source warning
- active events
- nearby viewers
- indexed local media

The main sections are:

| Section | Purpose |
| --- | --- |
| Screens | State, Now Playing, location, health and per-screen controls |
| Media Library | Watched local media, validation, thumbnails and cueing |
| Playlists | Weighted rotations, simulation, eligibility and draft editing |
| Events | Ordered timelines and event playback |
| Live Control | On-air item, cue, hold, next, return, voting and emergency |
| Diagnostics | Source, frame, FPS, reconnect and render information |
| Screen Groups | Start, stop or return several screens together |
| Schedule Calendar | Recurring actions and conflict warnings |
| Template Library | Install safe starter playlists and events |
| Change History | Audit entries and restore the newest config snapshot |

Shift-click a dashboard section to pin or unpin it. Pinned sections appear in
the bottom row and open the same real section.

## Selected screen

Actions such as cueing media need a target screen. In **Screens**, shift-left
click a screen to select it and open Live Control.

The selected screen stays selected while you move between Media Library,
Playlists, Events and Live Control.

## Navigation

- **Back** returns to the parent page.
- **Dashboard** always returns to the first page.
- **Previous Page** and **Next Page** appear for long lists.
- Menus showing live state refresh once per second.

## Files created by Control Studio

| Path | Contents |
| --- | --- |
| `plugins/LuigiScreen/studio.yml` | groups, schedules, favorites, audit, voting settings, statistics and thumbnail map IDs |
| `plugins/LuigiScreen/media/.thumbnails/` | generated 128x128 PNG previews |
| `plugins/LuigiScreen/history/` | config snapshots created before Publish |

Do not copy `studio.yml` between servers while either server is running.

## First walkthrough

1. Run `/screen menu`.
2. Open **Screens**.
3. Shift-left click `main`.
4. Open **Media Library**.
5. Left-click a valid file to play it now, or right-click it to queue it.
6. Open **Live Control** to inspect On Air and Cued.
7. Use **Return to Automation** when the manual segment is finished.

## Alpha boundaries

Control Studio deliberately keeps destructive file deletion out of the first
alpha. Delete local media through the server filesystem only after checking
the **References** count in Media Library.

The inventory editor covers common live operations and safe draft changes.
Very large event graphs are still easier to review in `config.yml`; the GUI
shows their timeline and can run them.

Web Studio offers a wider structured editor, but it also deliberately avoids
unrestricted remote YAML editing and remote media-file deletion.
