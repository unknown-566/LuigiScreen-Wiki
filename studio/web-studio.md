# Web Studio

Web Studio is LuigiScreen's browser control room. It uses the same screens,
shared media sources, playlists, events, schedules and safety rules as the
in-game Control Studio. The browser is only an operator panel; it does not run
a second playback engine.

## Fast path

Run one command:

```text
/screen web
```

LuigiScreen now prepares Web Studio for LAN access automatically when it was
still bound to localhost. It then prints clickable one-time login links:

- **LAN**: use this from another computer on the same network
- **Server PC**: use this only on the Minecraft server computer
- **Public**: shown only when `web-studio.public-url` is configured

The login link is short-lived and single-use. After it succeeds, the browser
gets a secure session cookie. Do not share the link.

If the LAN link does not load from another PC, the usual cause is Windows
Firewall on the server computer. Allow inbound TCP port `8765` for Java or
OpenJDK on the server PC.

## First screen

The dashboard starts with a **Start Here** launchpad instead of dropping you
straight into raw metrics. It guides the setup in this order:

1. Place the canvas in Minecraft with `/screen create main 7 4`.
2. Drop images, GIFs or videos into `plugins/LuigiScreen/media/`.
3. Create a playlist if the screen should run by itself.
4. Use Live Studio to preview a source and take it live.

The deeper metrics, alerts and diagnostics stay below the launchpad for admins
who need them.

## Beginner workflow

For normal automatic playback, use this path:

```text
Media Library -> Playlist Editor -> Screen Automation
```

You do not need to edit YAML for this path.

1. Put images, GIFs or videos into `plugins/LuigiScreen/media/`.
2. Open **Media Library** and check that the files are marked ready.
3. Open **Playlist Editor** and create a playlist.
4. Add media items to that playlist.
5. Choose a screen and press **Assign and play**.

The default language is English. Set `language: cs` in `config.yml` if you want
the Czech interface and messages.

## Build a playlist

Open **Playlist Editor** from the left sidebar.

What changed in `1.2.0-alpha.5`:

- new playlists start empty instead of creating a confusing `first` item
- **Add item** writes the selected media directly into the playlist
- duration and weight are visible while adding an item
- **Delete item**, **Delete playlist** and **Duplicate** are visible buttons
- playlist cards show item count, assigned screen count and readiness
- advanced item settings still live in the inspector so the main builder stays
  simple

Fast build path:

1. Press **Create playlist**.
2. Open the playlist card.
3. Select a media source in **Add media**.
4. Keep `30s` duration and weight `1`, or change them.
5. Press **Add item**.
6. Repeat for more media.
7. Pick a screen in **Play this playlist**.
8. Press **Assign and play**.

You can also open **Media Library**, choose the target playlist at the top, and
press **Add to playlist** directly on a media card.

Use **Duplicate** when you want to experiment without changing an existing
playlist. Use **Delete playlist** only when you really want to remove it; any
screen assigned to that playlist is cleared automatically.

## Make a playlist play from a screen

You can also start from the screen:

1. Open **Screens**.
2. Select the screen card you want to control.
3. Open the **Automation** tab.
4. Choose a playlist under **1. Normal playback**.
5. Press **Assign and play**.

This saves the playlist on that screen and starts its weighted rotation
immediately. You do not need to edit `config.yml` for this common path.

If the screen should stop using a playlist, press **Clear playlist**. That only
removes the saved playlist assignment; it does not delete the playlist itself.

Events are separate. Use **2. Temporary event** when you want a temporary scene,
announcement or interruption. When the event ends, the screen returns to its
playlist or direct source.

Use **3. Manual source** for testing one media item immediately. **Return auto**
cancels manual media and lets the saved playlist/source take over again.

## Build an event

Open **Events** when you want a temporary override instead of a normal
playlist. Good examples are restart warnings, update reveals, countdowns,
server announcements or a short live stream takeover.

What changed in `1.2.0-alpha.5`:

- event cards now open a beginner-friendly timeline builder
- **Add step** writes directly to the event instead of staging a draft
- the builder supports media, text, countdown and wait/hold steps
- each timeline step has a visible **Delete** button
- event cards and detail pages have visible **Delete event** controls
- **Duplicate** lets you experiment without changing the original event
- **Start event** and **Stop event** live beside the target screen picker
- empty events stay visible in the builder, but cannot be started until they
  have at least one valid step

Fast event path:

1. Press **Create event**.
2. Open the event card.
3. Choose **Media from library**, **Text card**, **Countdown** or **Wait / hold**.
4. Set a duration such as `10s`, `30s` or `2m`.
5. Press **Add step**.
6. Pick a target screen in **Run this event**.
7. Press **Start event**.

Use **Stop event** if you need to end the takeover early. The screen returns to
its normal playlist or direct source.

## Build an automation

Open **Automations** when you want LuigiScreen to run a playlist, event or
screen action at a server time.

What changed in `1.2.0-alpha.5`:

- automations now use a rule builder instead of a plain schedule form
- cards show the rule as **WHEN / IF / THEN**
- **Open builder** exposes the editable time, target, action and value
- **Save rule** writes the automation directly without YAML editing
- **Run now** tests the automation immediately, ignoring the clock
- **Duplicate** and **Delete rule** are visible in the same workspace
- the same page can still create screen groups for multi-screen targets
- unsaved automation edits stay in the browser while live refreshes arrive
- time editing is protected from live refreshes so the picker does not reset
- select boxes use a bundled searchable picker instead of raw browser selects
- the old **Schedule** navigation entry opens the same automation builder

Fast automation path:

1. Press **Create rule**.
2. Open the automation card.
3. Set **WHEN** to the server time.
4. Choose a target screen or group.
5. Choose **THEN** as `event`, `playlist`, `start`, `stop` or `return`.
6. Pick the event or playlist value when needed.
7. Press **Save rule**.

Use **Run now** to test the rule safely before waiting for the scheduled time.
If two rules target the same screen at the same time, the higher priority rule
wins unless the conflict policy is set to `allow`.

## Screen detail tabs

Every screen detail page is split into focused tabs:

| Tab | Use it for |
| --- | --- |
| Overview | current source, controller and placement summary |
| Automation | playlist assignment and temporary event override |
| Location | world, coordinates, facing and view permission |
| Performance | target FPS, real FPS, frame drops and reconnects |
| History | recent playback decisions for that screen |

## Useful commands

```text
/screen web
/screen web status
/screen web revoke
```

`status` reports whether the embedded HTTP server is running.
`revoke` invalidates your Web Studio sessions and pending login links without
kicking other administrators out.

## Interface

The left sidebar separates work into these areas:

| Area | Contents |
| --- | --- |
| Overview | start guide, dashboard and server health |
| Build | screens, media, playlists and events |
| Operate | Live Studio, groups, automations and emergency control |
| System | monitoring, diagnostics, configuration and settings |
| Inspector | details and actions for the selected object |

The bottom status strip reports connection, draft and live state. Use `Ctrl+K`
to open the command palette.

Settings, properties, metrics and table columns expose contextual help without
permanent info icons. Hover the complete label or control to see what the value
means and when it is used. Assistive technologies receive the same description.

## Live Studio

Live Studio follows a preview/program workflow:

- **Preview** shows the selected source before it goes on air.
- **Program** shows the source currently rendered on the selected screen.
- **Take Live** places the selected source on air.
- **Queue** prepares media without interrupting the current item.
- **Pause**, **Skip**, **Repeat** and **Return** operate the existing playback
  state machine.

Browser previews are generated only while a Web Studio client is actively
connected. LuigiScreen keeps a bounded, low-resolution copy at the configured
refresh rate; it does not start a second decoder for cloned screens using the
same source.

The preview has no Minecraft audio. LuigiScreen displays map pixels only.

## Draft and Publish

Structured configuration edits are staged in a private draft for the current
web session. They do not change live playback immediately.

1. Change a supported field in Configuration, a playlist or an event.
2. Review the draft counter in the top bar.
3. Select **Publish**.
4. LuigiScreen validates every value and creates a config snapshot.
5. All staged paths are written together and the plugin performs its safe
   non-destructive reload.

Select **Discard** to remove the current session's pending changes.

If `config.yml` changes outside Web Studio after the draft was created,
publishing is locked. Reload the page, review the external changes and create
a fresh draft. This prevents one administrator from silently overwriting
another administrator or a manual file edit.

## Security model

Web Studio is LAN-ready by default, but it is still authenticated. It uses:

- one-time login tokens
- short login-link expiry
- HttpOnly and SameSite session cookies
- CSRF tokens for every state-changing request
- origin validation
- strict browser security headers
- permissions captured from the Minecraft player who created the link
- masked passwords, stream keys and authenticated source URLs
- personal session and pending-link revocation with `/screen web revoke`

Do not expose port `8765` directly to the public internet. For remote access,
use an authenticated VPN or an HTTPS reverse proxy and set `public-url` to the
address users should open.

## Configuration

Default Web Studio settings:

```yaml
web-studio:
  enabled: true
  bind: "0.0.0.0"
  port: 8765
  public-url: ""
  server-name: "LuigiScreen Server"
  session-hours: 8
  login-token-minutes: 5
  live-update-millis: 1000
  preview-refresh-millis: 1000
  preview-max-width: 640
```

Most servers do not need to edit this. `/screen web` also upgrades older
localhost-only configs to LAN mode automatically.

| Option | Meaning |
| --- | --- |
| `enabled` | starts the embedded web server |
| `bind` | network address receiving HTTP connections; `0.0.0.0` means LAN-ready |
| `port` | local HTTP port |
| `public-url` | optional public HTTPS address generated behind a proxy or VPN |
| `server-name` | label displayed in the top bar |
| `session-hours` | maximum authenticated browser-session lifetime |
| `login-token-minutes` | maximum one-time link lifetime |
| `live-update-millis` | lightweight SSE state update interval |
| `preview-refresh-millis` | minimum time between captured browser previews |
| `preview-max-width` | maximum preview width before downscaling |

Run `/screen reload` after changing these values manually. Existing map
displays remain preserved.

## Roles and permissions

The player must first have:

```text
luigiscreen.web
```

The created session then contains only that player's LuigiScreen permissions.
For example, a technician with monitoring and diagnostics cannot publish a
playlist merely by opening Web Studio.

See [Drafts, History, Emergency and Roles](safety-roles.md) for every section
permission and recommended role examples.

## Performance

The initial page load requests a full snapshot. Live updates then send a small
SSE payload containing changing screen and server state. Large media,
playlist, event and audit lists are fetched again only when their revision
changes.

Live-preview capture is inactive with no connected browser. While active, the
preview is rate-limited and downscaled according to `preview-refresh-millis`
and `preview-max-width`.

If Web Studio affects a small server, first increase both update intervals to
`2000` and lower preview width to `480`.

## Troubleshooting

### LAN link does not open

- Run `/screen web` again and use the link labeled **LAN**.
- Make sure the other PC is on the same network as the Minecraft server PC.
- Allow inbound TCP port `8765` for Java/OpenJDK in Windows Firewall on the
  server PC.
- Run `/screen web status` and check that the HTTP server is online.
- Check that another application is not already using port `8765`.

### Login expired

Run `/screen web` again. Login links are deliberately short-lived and
single-use.

### Action is forbidden

The session was created without the required permission. Fix the player's
permissions, revoke the old session and create a new link.

### Publish is locked

`config.yml` changed outside the current draft. Reload Web Studio and stage the
changes again after reviewing the newer file.

### Preview is blank

Check the selected screen's source state in Diagnostics. Static offline and
connecting frames can still appear, but a failed source cannot produce a live
frame.

## Alpha boundaries

Web Studio is an alpha administration interface. Keep backups and test it on a
staging server first. It intentionally provides structured safe editors, not an
unrestricted remote YAML editor or remote filesystem deletion tool.
