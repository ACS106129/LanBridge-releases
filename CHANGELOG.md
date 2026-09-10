# Changelog

All notable changes to LanBridge are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
