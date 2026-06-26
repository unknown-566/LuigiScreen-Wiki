# Changelog

## 1.2.0-alpha.5

- Reworked Web Studio playlist editing into a beginner-first builder
- New browser-created playlists now start empty instead of adding a confusing `first` item
- Added direct playlist item add/delete actions from Web Studio
- Added visible playlist delete, duplicate and assign controls
- Added Media Library **Add to playlist** buttons that use the selected target playlist
- Added manual **Play now** controls in the screen Automation tab
- Added playlist readiness, item count and assigned-screen count to Web Studio
- Reworked Web Studio Events into a beginner-first event builder
- Added direct event step add/delete actions from Web Studio
- Added visible event delete, duplicate, start and stop controls
- Kept empty events visible in the builder while preventing empty events from starting
- Reworked Web Studio Automations into a beginner-first rule builder
- Added readable **WHEN / IF / THEN** automation cards
- Added direct automation save, run now, duplicate and delete actions from Web Studio
- Added direct event, playlist, start, stop and return automation actions from the builder
- Changed the default language to English with `language: en`
- Fixed Web Studio screen-detail tabs so Overview, Automation, Location, Performance and History actually switch content
- Added direct playlist assignment from the screen Automation tab with **Assign and play**
- Added playlist clearing from the same screen detail view
- Added temporary event start/stop controls to the screen Automation tab
- Improved the screen detail layout and tab styling
- Added bundled web-resource regression checks for the new controls

## 1.2.0-alpha.4

- Made Web Studio LAN-ready by default
- Changed `/screen web` to automatically upgrade localhost-only Web Studio configs to LAN mode
- Added LAN and server-PC login link generation with detected server network addresses
- Added a firewall hint when a LAN link cannot be opened from another computer
- Added Web Studio access status to the browser snapshot
- Added a Start Here launchpad to the Web Studio dashboard
- Added tests for Web Studio link generation and LAN-ready bundled resources

## 1.2.0-alpha.3

- Removed permanent `i` icons from Web Studio
- Moved contextual help to complete label/control hover targets and accessible descriptions
- Reworked the visual language into a cleaner broadcast workspace
- Simplified sidebar navigation and removed boxed two-letter navigation marks
- Improved spacing, hierarchy, panels, metrics, cards, tables and form controls
- Hid the desktop inspector until an object is selected
- Expanded the main workspace when the inspector is closed
- Refined desktop, tablet and mobile responsive layouts
- Added a regression test preventing visible info icons from returning

## 1.2.0-alpha.2

- Added the local browser-based LuigiScreen Web Studio
- Added a professional dashboard, screen grid, media browser, playlist and
  event editors, Live Studio, schedules, groups, monitoring and diagnostics
- Added Preview/Program controls with bounded source thumbnails
- Added contextual `i` help for settings, fields, metrics and table columns
- Added one-time login links, HttpOnly sessions, CSRF checks, origin checks,
  security headers and session revocation
- Added `/screen web [open|status|revoke]` and `luigiscreen.web`
- Added browser roles for automations, monitoring, configuration and settings
- Added per-session drafts, typed validation, snapshots, safe Publish and
  external-config-change protection
- Added lightweight SSE updates and revision-based full-state refreshes
- Added Web Studio JSON, security and bundled-resource tests
- Expanded the automated suite to 64 tests

## 1.2.0-alpha.1

- Added the in-game Control Studio with dashboard and role-based sections
- Added screen Now Playing, playback reasoning, queue, location and health tools
- Added Live Control with hold, skip, repeat, return, cueing and voting
- Added a watched Media Library with validation and generated map thumbnails
- Added playlist probability simulation and current eligibility diagnostics
- Added item/category anti-repeat and `guaranteed-after`
- Added expanded online, viewer, permission, TPS, world and weekday conditions
- Added event priority and wait, manual, command, broadcast, sound, title,
  group and branch steps
- Added Screen Groups, recurring schedules, conflict detection and templates
- Added per-admin Draft/Publish editing, config snapshots, audit history and undo
- Added aggregate play, viewer-time, skip and failure statistics
- Added Czech and English Control Studio localization
- Added granular studio role permissions and `luigiscreen.vote`
- Expanded the automated suite to 58 tests

## 1.1.0-alpha.16

- Restored the Maven metadata required by JavaCPP to identify bundled FFmpeg
- Fixed `Version of org.bytedeco:ffmpeg could not be found` during startup
- Prevented Paper's related `System.out/err.print` plugin nag warning
- Kept unnecessary duplicate classifier metadata excluded
- Added a GitHub Actions check for required FFmpeg and JavaCPP metadata
- Verified the shaded JAR reports FFmpeg version `7.1.1-1.5.12`

## 1.1.0-alpha.15

- Added an asynchronous Modrinth update checker
- Added console notifications for newer public versions
- Added clickable player notifications with `luigiscreen.update`
- Added semantic ordering for alpha, beta and release versions
- Added configurable project slug/ID, interval, timeout and notification options
- Added quiet handling for unpublished Modrinth projects and optional failure logging
- Restarted the checker safely during `/screen reload`
- Expanded the automated suite to 54 tests

## 1.1.0-alpha.14

- Reused MapEngine render surfaces instead of rebuilding raster objects for every frame
- Reused delta buffers in place to reduce full-screen allocations and GC pressure
- Captured player positions once per viewer refresh for all screens
- Cached playlist folder contents during startup and `/screen reload`
- Reduced static image worker wakeups
- Cached worker settings before background decoding starts
- Added `stream.io-timeout-seconds` and `performance.worker-stop-timeout-seconds`
- Improved local video and GIF looping by seeking before reconnecting the decoder
- Enforced `sources.max-image-bytes` for local images as well as URL images
- Changed `/screen start all` and `/screen stop all` to save config once per batch
- Fixed new and cloned screens not saving to disk immediately
- Protected the shared render scheduler from a single screen failure
- Removed unused duplicate metadata from the release JAR
- Expanded the automated suite to 48 tests

## 1.1.0-alpha.13

- Added per-screen playlists with weighted random item selection
- Added `/screen playlist list`, `/screen playlist set <screen> <playlist>` and `/screen playlist clear <screen>`
- Added manual event scenes with ordered sequences
- Added `/screen event list`, `/screen event play <screen> <event>` and `/screen event stop <screen>`
- Added random folder items for local videos, images and GIFs
- Added duration and cooldown parsing with `ms`, `s`, `m` and `h` units
- Added basic playback conditions: `min-online`, `min-viewers`, `viewer-permission` and `tps-above`
- Kept configured screen sources as fallback sources while playlist/event runtime switches stay non-persistent
- Added `luigiscreen.playlist` and `luigiscreen.event`
- Expanded the automated suite to 46 tests

## 1.1.0-alpha.12

- Added typed media sources: RTMP, MJPEG, local video, local image, URL image and GIF
- Added `/screen source <name> <type> <value>` with tab completion
- Added `/screen source <name>` and `/screen source <name> types`
- Added `plugins/LuigiScreen/media/` for safe relative local paths
- Added looping playback for local videos and GIFs
- Added retry behavior for RTMP, MJPEG and temporarily unavailable URL images
- Added source sharing by normalized type and value
- Kept `/screen set <name> url ...` and old `url:` config entries as RTMP compatibility paths
- Added automatic migration to per-screen `source.type` and `source.value`
- Added `luigiscreen.source`
- Added source HTTP timeout, image download limit and absolute-path protection
- Expanded the automated suite to 44 tests

## 1.1.0-alpha.11

- Fixed `/screen reload` removing MapEngine displays
- Reload no longer destroys any existing MapEngine display
- Screens removed from config are restored; `/screen remove` is required for deletion
- Geometry edits are preserved safely until the screen is explicitly removed and recreated
- URL, FPS, distance, enabled state, permission and rendering settings update in place
- MediaMTX source changes now use the same non-destructive reconciliation
- Fixed viewer respawning when the glowing frame setting changes
- Corrected the plugin author to `unknown_56`
- Expanded the automated suite to 40 tests

## 1.1.0-alpha.10

- Changed the offline, connecting, waiting and stopped screen title to `LuigiScreen`
- Added the customizable `screen.offline-title` localization key
- Kept the new title compatible with existing language files through the bundled fallback

## 1.1.0-alpha.9

- Added a separate permission for every `/screen` subcommand
- Kept `luigiscreen.admin` as the operator-level parent permission
- Added optional per-screen visibility protection
- Added dynamic `luigiscreen.see.<screen-name>` permissions
- Added the `luigiscreen.see.*` wildcard
- Added `/screen set <name> permission <true|false>`
- Added `permission-required: false` to per-screen configuration
- Made newly created and migrated screens public by default
- Made protected clones inherit their source screen's visibility setting
- Added permission metadata and default-behavior tests

## 1.1.0-alpha.8

- Added multiple named screens
- Added `/screen create <name> [width] [height]`
- Kept `/screen create 7 4` as legacy shorthand for `main`
- Added `/screen clone <source> <new-name>`
- Added `/screen list`, named status and targeted start, stop and remove commands
- Added `/screen set <name> <url|fps|distance|enabled> <value>`
- Added independent URL, FPS, distance, world, location, width, height and enabled state per screen
- Added automatic source grouping by RTMP URL
- Added one shared FFmpeg decoder for every unique URL
- Added reference-counted shared frames so clones do not duplicate decoded images
- Added independent render pacing and latest-frame queues per screen
- Added automatic migration of the old single-screen config to `screens.main`
- Expanded debug output with total, enabled and shared-source counts
- Added tests for screen definitions, source grouping, migration geometry and shared-frame lifetime

## 1.1.0-alpha.7

- Added bundled Linux x86_64 JavaCPP and FFmpeg native libraries
- Retained Windows x86_64 support
- Fixed `UnsatisfiedLinkError: no jniavutil` on Linux amd64 hosts such as Minekeep
- Published the LuigiScreen Free source code
- Added the AGPL-3.0-only license and third-party notices
- Added a trademark policy for unofficial modified builds
- Added a Java 21 GitHub Actions verification build
- Clarified the Paper/Bukkit plugin classification across public metadata
- Hardened URL masking across status output, connection logs and error messages
- Centralized screen geometry, map-limit and adaptive-FPS policy
- Added tests for all screen directions, FPS limits and configuration validation
- Verified that no-viewer pause exits the receive loop and closes the FFmpeg grabber
- Added connecting and offline screen states during RTMP outages

## 1.1.0-alpha.6

- Made MediaMTX profile switching asynchronous
- Prevented the main server thread from waiting for FFmpeg shutdown
- Ensured a replacement decoder never starts before the previous worker terminates
- Added localized decoder-switching feedback

## 1.1.0-alpha.5

- Added a 15-line `/screen debug` sidebar
- Added personal scoreboard restoration and conflict protection
- Added Czech and English sidebar labels
- Improved localization fallback for older customized files

## Earlier prototypes

Earlier local builds established:

- RTMP decoding
- MapEngine rendering
- Reconnect behavior
- Screen persistence
- MediaMTX setup generation
- Configurable limits

They are not recommended for public installation.
