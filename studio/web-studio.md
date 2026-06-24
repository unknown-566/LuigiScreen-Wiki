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
| Operate | Live Studio, groups, schedules and emergency control |
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
