# Changelog

All notable changes to LanBridge are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.13] - 2026-09-12

### Added

- **Sound.** WinUI has a sound system built into every control — focus, invocation,
  dialogs opening and closing — and it is silent unless an application asks for it. This
  one never had, so every press has been quiet by omission rather than by choice. It is a
  choice now, it is spatial, and it is a checkbox in settings for anyone who wants a
  utility to keep its mouth shut.
- **Motion where something happened.** The two columns fade in as the window assembles
  itself, the status text fades up when it changes, the relayed count kicks when it goes
  up, the status light breathes while a session runs, and a caution line slides its
  neighbours down instead of appearing from nowhere.
- **The diagram reports rather than mimes.** It ran the same animation whether or not
  anything was happening, which is decoration in the clothes of instrumentation. It is
  dimmed and still while there is no session, it runs when there is, and the blocked cell
  shows the number of packets the guard has actually dropped — which the interface was
  never told before, because nothing had ever subscribed to the event carrying it.
- **An installer that looks like this product.** Generated banner and welcome artwork
  instead of the WiX placeholders, which are the visual equivalent of shipping with the
  default icon.
- **An installer in your language.** One MSI per language rather than English for
  everyone: WiX supplies its own page titles and buttons for the culture, and the parts
  this project wrote come from localization files beside them. English and 正體中文 to
  begin with.

### Changed

- The caution under per-process confinement now appears when the box is **clear**, which
  is the state worth warning about, and says what that state means rather than repeating
  the label above it. The two warnings also have different marks — a globe for where
  traffic goes, an open padlock for who may use it.

## [0.5.12] - 2026-09-12

### Fixed

- **The install folder held eighty-eight folders of translations for languages this
  application does not offer** — af-ZA, sl-SI, fil-PH and the rest. They are the Windows
  App SDK's own interface strings, shipped as Win32 resources rather than as .NET
  satellite assemblies, so the usual setting for trimming them does not reach them. Only
  the ones matching a language the interface speaks are kept now: fifteen instead of
  eighty-eight.

### Changed

- The per-process option is called *Only the target application may talk to the VPN*,
  which is what it does. It never governed the tunnel as a whole.

### Added

- **Ten more checks that drive the real window**, covering the dialogs and the choices
  inside them: settings offers every control it should, the About box shows the version
  actually running, switching language leaves no dropdown blank, the appearance choice
  sticks, the custom port appears only for the custom protocol, the target mode decides
  which picker is offered, refreshing the process list keeps it populated, and turning
  confinement on does not move the controls above it.

  Writing them found two things on its own. A dialog here is not a window and not called
  what it looked like it was called, so four tests were looking for something that has
  never existed; and Start is unavailable without a profile rather than pressing and
  doing nothing, which the test asserted the wrong way round until the button said so.

## [0.5.11] - 2026-09-12

### Fixed

- **Dark mode was white text on a white page.** The last attempt painted the background on
  one element and applied the theme to the one inside it, so the text resolved in the dark
  theme and the surface behind it in the light one. The theme now goes on the element that
  paints the background, where both agree.
- **The target section's icon drew as an empty box.** A code point sitting inside a font's
  character map does not mean the font has a glyph for it. The three section marks are
  emoji now, which have no such doubt and match the diagram above them.
- **The controls inside each section sat centred** in a card that was already the right
  width. An expander stretches itself but not its contents unless told to.
- **Choosing to attach and pressing start without picking a program threw an exception.**
  The check added for a missing executable did not cover attaching, which needs a
  different kind of nothing. It now says which choice is missing, and the message no longer
  tells anyone to type a process id at a control that is a list.

### Added

- **Tests that drive the real window.** Every visual defect reported so far — a button
  laid out and clickable but never painted, controls centred in a full-width card, a
  warning that widened its column instead of wrapping, a process list offering this
  application to itself — could pass every unit test in the project, because none of them
  are about what a method returns. Nineteen checks now open the published build and look.

## [0.5.10] - 2026-09-12

### Fixed

- **The caution line widened the column instead of wrapping.** A horizontal stack measures
  its children with unlimited width, so a wrapping text block inside one never wraps — it
  pushes everything beside it wider, including the button above. Both notes now sit in a
  grid that gives the text a real width to wrap into.
- **Settings and application information appeared twice.** The pair beside the status cards
  stayed behind when they moved to the top right.
- **The process picker offered this application to itself.** Attaching the tunnel to the
  window configuring it is not something anyone means to do.

### Changed

- **The diagram fills the space it was given.** The routes stretch with the window rather
  than sitting at a fixed width in the corner of a large empty card, and the packets travel
  the whole way across whatever width that turns out to be.
- **The note under per-process confinement appears only while it is on**, with the same
  caution marker as the routing one, and says what it does: packets from every other
  program here are stopped.
- The target path shows in full on hover, however narrow the box is.

## [0.5.9] - 2026-09-12

### Fixed

- **Dark mode was unusable.** The page never painted a background of its own, so the text
  followed the theme while the ground behind it did not — light text on a light surface.
  Dialogs kept the system's appearance too, because a dialog is hosted by the window root
  rather than by the element the theme was set on, and had to be told separately.

### Added

- **The two confinement switches are now a picture.** A grid of who is sending against
  where to, with traffic moving along each route: through the tunnel, out the ordinary way,
  or stopped. The four cells are every combination of the two settings, which answers in a
  glance what two paragraphs of prose had been failing to.
- **Attaching picks from a list of running programs** instead of asking for a process id
  typed in from somewhere else, with a button to refresh it.

### Changed

- The routing warning appears only while that option is on, says one thing, and says it in
  the colour of a warning.
- The sections lost their numbering; they were never steps to follow in order.
- Settings and application information moved to the top right, with the status cards
  directly beneath them.

## [0.5.8] - 2026-09-12

### Fixed

- **Confining the tunnel to one application only worked in one direction.** It dropped
  traffic that other programs here sent to the VPN, and did nothing about traffic arriving
  from it — so every other program on this machine stayed reachable from the other end,
  which is the half that matters when you do not know who is there. It now applies to both
  directions, and the explanation says so instead of promising more than it did.

### Changed

- **The routing option is now the other way up.** Keeping the rest of the machine off the
  tunnel is the safe state and what almost everyone wants, so it should not be something
  you have to switch on. The box now reads *Send all traffic through the VPN*, is off by
  default, and says what turning it on means: everything this machine sends goes through
  the VPN server first, so whoever runs that server sees all of it. That matters most with
  a profile somebody else gave you.

### Added

- **Light and dark appearance**, or following the system, applied immediately and
  remembered. Set under Appearance in settings.
- **Icons through the interface** — on each section, on Start and Stop, and on the log
  actions — and a status light that is green while a session is running.

## [0.5.7] - 2026-09-12

### Fixed

- **A download could not be cancelled.** The button said *Cancel* and could not be pressed:
  the download was run while holding the dialog's click deferral, and a dialog with an
  outstanding deferral disables its own buttons — including the only one that could have
  stopped it. The transfer now runs alongside the dialog instead of inside its click
  handler, so the button is live for exactly as long as there is something to cancel.
- **A cancelled or failed download left its partial file behind**, one per attempt, for
  ever. The incomplete file is now discarded when the transfer does not finish, and a
  completed download clears the installers that came before it.
- **Pressing Start with nothing to start did nothing at all** — no message, no log line, no
  change. Without a profile, or without an application chosen, it now says which one is
  missing instead of looking broken.

### Changed

- **Installing an update while a session is running now warns first**, and the safe answer
  is the default. Installing stops the tunnel and disconnects the target application, which
  is not something to discover afterwards.

## [0.5.6] - 2026-09-12

### Fixed

- **A new release is now noticed within about a minute of being published**, instead of at
  the next scheduled check. Asking that often is affordable because the request is
  conditional: the validator from the previous answer is sent back, and while the release
  is unchanged the reply is "not modified" — no body, and not counted against the rate
  limit. Only an actual new release costs a request. This is still polling rather than
  being told, so it is a minute rather than an instant, but nothing has to be pressed and
  nothing has to be restarted.
- **Dismissing the update notice left no way back to it.** Closing it was permanent for the
  session, and the update could only be reached again by restarting. Both the application
  information dialog and settings now offer *Update now* while one is waiting, so
  dismissing the notice only dismisses the notice.
- **Stop said *Stop* while there was nothing left to stop.** When the target exits, the
  session waits up to twenty seconds to see whether a launcher hands over to another
  process — during which the thing the session exists for is already dead. The button says
  *Force stop* for that window, which is what pressing it does: end the session now rather
  than wait the handover out.

### Changed

- **Everything about updating is now in the application information dialog**, and its
  button carries a badge while an update is waiting. Automatic checking, checking now, when
  the last check ran and the update itself all live beside the version they are being
  compared against, instead of being split between there and settings.
- **A downloaded installer is kept when you choose *Later*.** Declining to install
  immediately used to throw the download away; now the same button offers *Install now*
  until the release it belongs to is superseded.

## [0.5.5] - 2026-09-12

### Fixed

- **Automatic update checks were too rare to feel automatic.** Four hours between checks
  meant that in practice only a restart seemed to find anything, which left a button in
  settings as the real mechanism — and nobody wants to press a button to be told there is
  nothing new. Checks now run every thirty minutes, and bringing the window forward checks
  as well when the last one is more than five minutes old. Settings shows when the last
  check ran, so it is visible that one happens.
- **The two confinement options read as duplicates.** Both were phrased as restricting the
  tunnel, without saying that they restrict different things. Each label now names its own
  axis — *Only VPN addresses go through the tunnel* against *Only the target application
  may use the tunnel* — and each explanation opens by saying which question it answers:
  which destinations, or which program.

### Changed

- **The banner button is called *Update now*** rather than *What's new*. It installs the
  update; showing the notes is what it does on the way.
- **Settings can start an update**, not only look for one.
- **The empty strip along the top of the window is gone.** Settings and application
  information moved down beside the status cards, which is the only thing that was up
  there.
- **The relayed-advertisement count is shown only for Warcraft III.** It is the one
  protocol whose game information has to be asked for and forwarded; for the others the
  counter would sit at zero for ever, which reads as a fault rather than as "not
  applicable".

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

[0.5.13]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.13
[0.5.12]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.12
[0.5.11]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.11
[0.5.10]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.10
[0.5.9]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.9
[0.5.8]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.8
[0.5.7]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.7
[0.5.6]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.6
[0.5.5]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.5
[0.5.4]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.4
[0.5.3]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.3
[0.5.2]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.2
[0.5.1]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.1
[0.5.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.0
[0.4.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.4.0
[0.3.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.3.0
[0.2.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.2.0
[0.1.0]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.1.0
