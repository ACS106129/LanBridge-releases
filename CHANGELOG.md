# Changelog

All notable changes to LanBridge are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.4] - 2026-09-12

### Fixed

- **The update window showed the release notes as their Markdown source** — hashes,
  asterisks and backticks — rather than rendering them, which made the thing written to be
  read painful to read. Headings, bullets, emphasis and inline code are now formatted.
- **Updating did not close the application first.** The installer was started while the
  tunnel and the elevated helper were still holding the files it was about to replace. The
  session is now shut down and this process exits before the installer runs, and the
  installer terminates a stray instance rather than letting one turn an update into a
  request to reboot.
- **The window and taskbar kept a generic placeholder icon** while the notification area
  and Add or remove programs showed the real one. An unpackaged window does not take the
  icon from the executable on its own.
- **The Chinese label for "Keep the rest of the machine off the tunnel" described the wrong
  setting.** It read as "keep other applications' traffic out of the tunnel", which is what
  the separate per-application confinement does, leaving the two options looking like
  duplicates. They are orthogonal: one limits which destinations use the tunnel, the other
  limits which process may.

### Changed

- **The name and tagline no longer occupy the top of the window.** The title bar already
  says what this is, and the details moved to the application information dialog.
- **The application information dialog no longer repeats the name it is titled with**, and
  leaves opening the log folder to the log panel, where that button already lives. The
  version it exists to answer is now shown large enough to read at a glance.

## [0.5.3] - 2026-09-12

### Fixed

- **A game hosted on one machine could be seen but not joined from the other.** Only the
  discovery port was opened inbound, and that is not necessarily the port a host listens
  on: Warcraft III takes 6112 when it can and walks up to 6119 when it cannot, advertising
  whichever it got. A host pushed off 6112 was therefore visible and unreachable — and only
  in that one direction, which is what made it look like one machine was at fault. The
  whole hosting range is now opened, still to the VPN subnet alone.
- **A room stayed in the other player's list after the host left it.** Warcraft III
  announces a closed room by broadcast, and a broadcast can leave by the VPN adapter, where
  the relay is deliberately not listening — so the announcement was never picked up and the
  peer went on offering a room that no longer existed. The relay now notices that the host
  has stopped answering its probes and retracts the room itself, using the advertisement it
  last forwarded to say which one.
- **Switching language emptied the target-application and LAN-discovery dropdowns**, and
  choosing *System default* emptied the language dropdown itself. Retranslating a list
  means replacing the entries inside it, and a dropdown treats the replacement of the
  entry it has selected as that entry being gone — so it cleared its selection, and the
  binding wrote that emptiness back over the choice. Entries now keep their identity and
  only their text changes, so there is nothing left to clear. Two earlier attempts put the
  selection back afterwards; this removes the cause.
- **The settings window kept the old language in its own title and button** when the
  language was changed from inside it. Everything in the window's content was relabelled,
  but the title and the close button are not part of that content and were missed.
- **The activity log did not reliably follow new lines.** It scrolled before the new line
  had been laid out, so it went to where the bottom used to be and stayed one line behind
  for ever. It now scrolls after the layout, and stops following the moment you scroll up
  to read something — resuming when you scroll back down.
- **Upgrading rewrote every file, changed or not.** The old version was removed in full
  before a single new file was written, so each upgrade rewrote the whole install. The new
  version is now written first and the old one removed afterwards, which lets the
  installer skip files that are identical and leaves only what actually changed to write.
- **One of the log buttons was laid out and clickable but never painted.** *Open log
  folder* occupied its space and responded to clicks while showing nothing at all. The log
  actions now sit in a single horizontal run instead of a column each, removing the
  per-column arrangement that was going wrong.

### Added

- **An application information button** beside the settings one: which version is running,
  the copyright, and a link to the notes and downloads for that version.
- **A *Launch LanBridge* checkbox on the installer's last page**, ticked by default. It
  starts the application unelevated, which is how LanBridge is meant to run — consent is
  asked for when a session starts, not before.

### Changed

- **Closing the target application now ends the session only when the tunnel is bound to
  it.** With *Only this application may use the VPN* switched on, the tunnel exists for
  that one process and goes down with it — what would otherwise be left is a tunnel nothing
  on the machine is allowed to use. Without it, the tunnel is only scoped by destination
  and may still be carrying traffic for something else, so it stays up until you stop it.

## [0.5.2] - 2026-09-11

### Fixed

- **Release notes in the update window showed only their first heading.** Notes are
  normalized to bare line feeds when they are pulled out of the changelog, and a Windows
  text control breaks lines on a carriage return — so everything after the first line was
  never drawn. An English release body arrives with carriage returns already in it, which
  is why only the translated notes looked empty. Notes are now converted before display
  and shown in a scrollable, selectable block.

## [0.5.1] - 2026-09-11

### Fixed

- **Games were visible but could not be joined, or did not appear at all.** Everything
  that makes a game joinable arrives *inbound* over the tunnel, and Windows blocks all of
  it by default: the advertisement a peer's relay forwards is inbound UDP, and joining is
  an inbound TCP connection. The command-line version opened both; the application never
  did, so whichever machine had no rule left over from it was unreachable in one or both
  directions. A session now opens the discovery port for the VPN subnet only — not for
  every network the machine is attached to — moves the tunnel adapter out of the *public*
  category Windows assigns it, and puts both back when the session ends.
- **The interface fell apart at any text size above the default, and restarting never
  brought it back.** The scaled layer was centred first and then grown from its own top
  left corner, so the whole interface started lower and further right than it should and
  ran off the bottom and right edges — taking the settings button with it. Because the
  text size is remembered, every restart landed in the same broken state with no way to
  reach the setting that caused it.
- **Downloading an update showed no progress at all.** The window closed the moment
  *Download* was pressed and the transfer ran with nothing on screen, which is
  indistinguishable from a download that never started. The release notes now stay open
  with a progress bar, the amount transferred, and a *Cancel* that works.
- **Switching automatic update checks back on did nothing for up to four hours.** The
  background check only reconsidered the setting at its next scheduled run; it now looks
  straight away.
- **The close-window choice looked as though it had been discarded** when the language was
  changed during the same visit to Settings. Translating the list replaces the selected
  entry, which clears the selection — the other dropdowns restore themselves, this one did
  not.

### Added

- **Update checks now run while the application sits in the notification area**, every
  four hours rather than only at startup. A new release is announced with a
  notification-area balloon, and the icon's tooltip keeps saying so after the balloon
  fades.
- **A *Check now* button in Settings**, for when waiting for the next scheduled check is
  not the point.

## [0.5.0] - 2026-09-11

### Fixed

- **The VPN dropped the moment a game room was opened.** The search for the process a
  launcher hands off to only matched executables with the same name, so a game that
  continues under a different name was never found and the tunnel came down. Anything
  still running from the same install folder now counts, and the search reports what it
  looked for.
- **Stop did nothing once a session had already ended.** Closing the pipe to the helper
  threw when the other end was gone, and that error escaped the cleanup path — so the
  application went on believing a finished session was still running.
- **Switching language emptied every dropdown** instead of translating it. Replacing a
  list's contents clears the selection, and the binding wrote that empty selection back.
- **The activity log did not follow new lines.** It now scrolls to the newest entry, and
  stops following as soon as you scroll up to read something.

### Added

- **Release notes in your language.** Translated changelogs are published alongside the
  builds, and the update dialog shows the one matching your interface language.
- **Export error report**, a button that appears with a marker once anything has failed.
  It bundles the on-screen activity with the log files from both processes, so a problem
  can be reported without knowing where logs live.
- The text size setting now scales the whole interface rather than only the activity log.

## [0.4.0] - 2026-09-10

### Added

- **Automatic update checks.** The app can look for newer releases on GitHub and show
  what changed before you install. On by default; switch it off in Settings.
- **Settings dialog** behind the gear button, holding language, activity font size,
  close behaviour and update checking — so a remembered choice is never a dead end.
- **Ten more interface languages**: Simplified Chinese, Japanese, Korean, Spanish,
  French, German, Portuguese, Russian and Italian, alongside English and Traditional
  Chinese. Status text, dropdown entries, dialogs and the tray menu are all translated.
- **Activity log is now selectable text**, with *Copy all* and *Export…* buttons.
- **Adjustable activity font size** (10–22pt), remembered between runs.
- **Application icon**, used by the window, the taskbar, the notification area and
  Add/Remove Programs.
- **Installer now asks where to install**, and offers the Start menu shortcut and the
  desktop shortcut as separate, independent choices.

### Fixed

- **The VPN dropped the moment the game finished loading.** Warcraft III — like most
  titles with a launcher or updater stub — exits its first process and hands off to
  another. That first exit was treated as "the target closed", tearing down the tunnel
  at exactly the wrong moment. The session now follows the application across the
  handoff.
- **The tray icon never appeared.** The icon handle was destroyed before Windows used
  it, and a destroyed handle makes `Shell_NotifyIcon` show nothing without reporting an
  error.
- **Reopening from the tray started a second copy** instead of restoring the running
  one. Only one instance runs per user now, and launching again brings the existing
  window forward.
- **`WinDivert64.sys` stayed locked after closing the app.** Closing the driver handles
  is not enough — opening one registers a kernel service that keeps running, and the
  file stays locked until it is stopped. The service is now stopped and removed when a
  session ends. Closing the window also stops the elevated helper, which it previously
  did not.
- **Switching the language back to "System default" did nothing.** The change was
  announced with a single "everything changed" notification, which WinUI does not act on
  reliably; each string is now announced by name.
- **Uninstalling asked for a restart.** The installer now closes the application and its
  helper first, so no files are left in use.
- Activity log rows had list-item padding that left half a blank line between entries.

### Changed

- Log actions moved to the activity panel on the right, instead of sitting at the bottom
  of the configuration column.

## [0.3.0] - 2026-09-09

### Added

- Error logging to `%LOCALAPPDATA%\LanBridge\logs\`, one file per process per run, with
  unhandled exceptions caught in three places and recorded rather than terminating the
  app silently.
- Settings persistence: choices are written on every change, so they survive a crash or
  a forced termination.
- Notification-area support with a prompt on close, offering exit or minimise.
- Traditional Chinese interface alongside English.
- MSI installer with Start menu and desktop shortcuts, version metadata and an
  Add/Remove Programs entry.

### Fixed

- The published build launched and then died inside the XAML runtime: publishing an
  unpackaged WinUI app does not carry its compiled markup, so `InitializeComponent`
  had nothing to load.

## [0.2.0] - 2026-09-09

### Fixed

- **The window never appeared after the elevation prompt.** WinUI 3 cannot run elevated
  — WinRT activation fails and the process exits without showing anything. The interface
  now runs unelevated and hands privileged work to a separate helper process, which asks
  for consent once per session. The interface itself holds no privileges, and the helper
  holds them only while a session is running.

## [0.1.0] - 2026-09-09

### Added

- Per-application VPN: imports an OpenVPN profile and refuses the pushed default route
  and DNS, so only the VPN subnet crosses the tunnel and the rest of the machine keeps
  its normal network path.
- Optional per-process confinement using WinDivert, dropping VPN-subnet traffic from
  every process except the target's.
- LAN discovery relay for tunnels that cannot carry broadcast, including Warcraft III's
  W3GS protocol — whose game information is only ever sent as a unicast reply, never
  broadcast, and so has to be requested from the local game and forwarded.
- Generic UDP broadcast relay for other games, configured by port.
- Headless command-line driver for the same engine.

[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
