# Changelog

All notable changes to LanBridge are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.32] - 2026-09-24

### Added

- DMM Game Player installs and starts in one step.
- Sort by country, speed, latency or sessions.
- Profiles keep the speed they were picked at.
- Downloads show speed and time left.

### Changed

- Tunnel sites sit with the VPN profile.
- Updates download much faster.
- Every help text is one sentence.

### Fixed

- A dropped tunnel no longer shows as ready.
- The same profile is stored only once.
- The remove button clears the scrollbar.

## [0.5.31] - 2026-09-23

### Added

- A ready-made DMM site list.

### Changed

- Only the sign-in goes through the tunnel; the game is not slowed.
- The session no longer follows the app into another process.
- An app already running is used instead of starting a second copy.

### Fixed

- The tunnel no longer dies quietly after a reconnect.
- A sign-in address is no longer dropped between two lookups.
- A brief drop no longer restarts the tunnel.

## [0.5.30] - 2026-09-20

### Fixed

- A refused check no longer reads as up to date.
- Profiles not in use can be deleted while connected.
- Wait-for mode no longer says it is starting the app.
- Unlisted sites the app reaches now stay in the tunnel.
- Lines that repeat on a timer are in the log again.

## [0.5.29] - 2026-09-19

### Added

- Pick and download a VPN from VPN Gate.
- Change the site list while connected.

### Changed

- Only the target's own sites stay tunnelled.
- The update carries one runtime, not two.

### Fixed

- Name following survives an openvpn reconnect.
- Update checks are no longer refused.

## [0.5.28] - 2026-09-19

### Fixed

- **The update is 38.8 MB smaller** — the Windows App SDK's machine-learning stack, onnxruntime and DirectML, was being shipped in a per-application VPN that never calls it.
- **A release is no longer visible until all of its installers are attached**, which is why 0.5.27 offered a German installer to a Chinese interface: it was published with one file uploaded and the rest still going up, and the application looks for a new release within about a minute.

## [0.5.27] - 2026-09-19

### Fixed

- **The target can no longer reach anything except through the tunnel** — the opening packet of a connection to an address no route covers is dropped rather than sent from this machine's own address, which is what caused the repeated 403s and the stuck loading screen.
- **Refusing that packet is what adds the route**, so the connection succeeds on its first retransmission instead of failing.
- **A blocked IPv6 packet is no longer counted or logged as a leak** — not in the line as it happens and not in the verdict at the end, which called one measured run "22 of 73 destinations did not go through the tunnel" when all 51 IPv4 ones did and the 22 were the guard working as designed.
- **A tunnel that reconnects onto a different address is followed**, and if the new one falls outside the subnet the packet filter was built around, refusing stops and says so rather than turning every packet the target sends into a refusal.

### Changed

- **Routes come back out of the tunnel again** — an address is released once no followed name answers with it and the target has no connection open to it, instead of the set growing for the whole session.
- **An adopted route expires after five idle minutes** and gives back its place against the session's limit.

## [0.5.26] - 2026-09-18

### Changed

- **The list of sites no longer has to be right** — it is only a warm start now, and anywhere the target reaches without the tunnel gets a route of its own, except the VPN server, this machine's own networks, broadcast and IPv6.
- **The end-of-run summary no longer calls a destination missed after it has been routed**, because it asks the system again rather than reading back the routes that were added.

## [0.5.25] - 2026-09-17

### Fixed

- **Sites only went through the tunnel when both sides happened to agree on their address** — eighteen of twenty-seven names answered differently, only the tunnel's answer was routed, and the application used the local one, so both are routed now.
- **The sites are edited one to a row, in a window of their own**, instead of a single box holding twenty-seven names.
- **A download the server cuts short is picked up where it stopped** rather than thrown away, asking each part again from the byte it reached, up to five times.
- **0.5.24 blamed that on a timeout and was wrong** — the failure had been running for thirty minutes, so a fifteen-minute limit cannot have been what ended it.

## [0.5.24] - 2026-09-17

### Fixed

- **A slow update download was thrown away just before it finished**, because the fifteen-minute limit covered reading the file and not only reaching the server; there is no overall limit now.
- **Updates download about three times faster**, because the release host limits each connection rather than the line, so the installer is fetched in four parts at once.
- **Progress is reported every 512 KB instead of every 80 KB**, which is a pace the window can use.

## [0.5.23] - 2026-09-16

### Added

- **The session writes down where the target actually went, and which of it missed the tunnel**, so one run names exactly what is missing instead of leaving the list of names a guess.
- **Addresses are reported with the name they answer to**, because a content network's edge name carries the location that is the whole question.

### Fixed

- **The named hosts are followed while the session runs** rather than pinned once at the start, since they answer with a sixty-second TTL and a session lasts hours.
- **The search for a working resolver stops once one has answered**, instead of timing out per name and costing minutes before the session started.

## [0.5.22] - 2026-09-16

### Added

- **The exit address is measured before and after the routes go in, and both answers go in the log**, because every earlier check was a step towards that rather than that — and "could not tell" is written as itself.

### Fixed

- **The log now holds the half of a session you would need it for** — the window's own steps, every message from the packet filter and everything OpenVPN said all go to the file.
- **A route is no longer rejected a moment before it starts working**; the routing table is given a few hundred milliseconds to settle first.
- **The packet filter records the filter it opened with**, so a clause that is missing can be told from one that never matched.

## [0.5.21] - 2026-09-14

### Fixed

- **Sites sent through the VPN were not going through it while everything said they were** — the next hop was guessed as the network's .1, which does not exist on a /30, so it is taken from the address the tunnel actually got and every route is now verified as the one the system would really use.
- **A name that could not be resolved through the tunnel was reported as agreeing with the local answer**; no answer is not the same answer, and the log now says which resolver and transport produced it.
- **Asking through the tunnel now falls back to TCP**, because a relay carrying one and not the other is a common thing for a volunteer server to do.

## [0.5.20] - 2026-09-13

### Added

- **Sites you can send through the VPN without sending the whole machine** — named sites are looked up through the tunnel and routed through it, reachable by the target application alone while the session runs.

## [0.5.19] - 2026-09-13

### Added

- **Somewhere to put a user name and password**, for a server that asks to be signed in to; the dialog says plainly that openvpn can only read it from a file, so it is stored unencrypted in that profile's own folder.

### Fixed

- **The target reached the internet over IPv6, around the tunnel entirely** — a leak in every mode, since a tunnel carrying IPv4 cannot carry what the machine sends over IPv6, so the target's IPv6 is dropped and it falls back.
- **"No update" when nothing had been asked** — a spent rate limit reported the version you already had, and the validator that makes a check free is now kept between runs.

## [0.5.18] - 2026-09-13

### Fixed

- **The About box thanked WinDivert without saying under what terms it is used**, and now names the LGPL v3, the copy shipped beside the program and where the source is.
- **A Warcraft III room's free places never changed on the other machine**, because the announce a peer reads is broadcast and cannot be captured, so it is derived from the advertisement instead and checked before it is sent.
- **The update download still held the window** — 0.5.16 said this was fixed while nothing ever called the code, so it now really runs in the background, with **Continue in background** in the dialog and a cancel in the main window.

## [0.5.17] - 2026-09-13

### Fixed

- **One player leaving a Warcraft III lobby closed it to everybody**, because a listener and every connection accepted on it shared one port record and the first close took it; each socket is tracked separately now.
- **The target application was still being run as an administrator** — 0.5.16's fix needed a privilege an elevated helper cannot hold, so it now uses the one it does and says which in the log.

## [0.5.16] - 2026-09-13

### Added

- **An installer in every language the application speaks**, each with the right ANSI codepage for its culture, and the update offers the one matching the window's language.
- **The window opens where you left it**, unless the position no longer lands on a display.
- **A new icon** — an arrow leaving through the opening of a ring, drawn separately at each size rather than scaled down from one large image.

### Fixed

- **Start was below the fold**, and the buttons are now pinned under the cards that scroll behind them.
- **The target application was running as an administrator**, so a browser signing in could never hand it the authorization code; it is started with the shell's token now.
- **Downloading an update held the whole window hostage**, and now runs in the background with a progress bar and a cancel button.

## [0.5.15] - 2026-09-13

### Fixed

- **One refusal from the server ended the attempt**, which is right for a server of your own and wrong for a public relay, so it retries three times before reporting.
- **"EXITING auth-failure" explained nothing** and now says which step failed and what that means for the kind of server you are using.

## [0.5.14] - 2026-09-12

### Added

- **An imported profile now belongs to the application** — the .ovpn and every certificate and key it refers to are copied into a folder of their own, so deleting the original changes nothing.
- **Somewhere to see what is being kept**, with rename, delete and a way into the folder, confirming inside the row rather than in a second dialog.
- **OpenVPN, if you do not have it**, fetched from OpenVPN's own download host and refusing anything Windows does not trust or that OpenVPN did not sign.
- **Tests for the installer**, walking its pages in both languages and cancelling at the summary so running the suite installs nothing.
- **A test that looks at the pixels**, measuring every explanatory line in Settings and About against what is behind it in both themes.

### Fixed

- **Three lines in About were invisible**, because a brush from the application's resources resolves against a theme an unpackaged WinUI application cannot change after it starts.
- **The update check stopped asking politely** — it now reads when the allowance comes back and waits until then, and says so once rather than sixty times.
- **The installer was writing on top of its own artwork**, which is the background the dialog writes on rather than a picture beside it.
- **The update offered everybody the English installer**, and now asks for the one matching the window's language.

### Changed

- Every setting has a line under it saying what it changes, and where the settings are kept.
- About says how often updates are looked for, and credits OpenVPN's community client and WinDivert.

## [0.5.13] - 2026-09-12

### Added

- **A grand piano** — control sounds played through the General MIDI synthesiser Windows already has, on a pentatonic scale so any two agree, with a checkbox to silence them.
- **Motion where something happened**, rather than everywhere.
- **The diagram reports rather than mimes** — still while there is no session, and the blocked cell shows the packets the guard actually dropped.
- **An installer that looks like this product**, with generated artwork instead of the WiX placeholders.
- **An installer in your language**, one MSI per language, English and 正體中文 to begin with.

### Changed

- The caution under per-process confinement now appears when the box is clear, which is the state worth warning about.

## [0.5.12] - 2026-09-12

### Fixed

- **The install folder held eighty-eight folders of translations for languages this application does not offer**, shipped as Win32 resources the usual trimming setting does not reach; fifteen are kept now.

### Changed

- The per-process option is called *Only the target application may talk to the VPN*, which is what it does; it never governed the tunnel as a whole.

### Added

- **Ten more checks that drive the real window**, covering the dialogs and the choices inside them — and writing them found four tests looking for something that has never existed.

## [0.5.11] - 2026-09-12

### Fixed

- **Dark mode was white text on a white page**, because the theme was applied to an element inside the one painting the background.
- **The target section's icon drew as an empty box**, since a code point in a font's character map does not mean there is a glyph for it.
- **The controls inside each section sat centred** in a card that was already the right width.
- **Choosing to attach and pressing start without picking a program threw an exception**, and now says which choice is missing.

### Added

- **Tests that drive the real window** — nineteen checks that open the published build and look, because every visual defect reported so far could pass every unit test in the project.

## [0.5.10] - 2026-09-12

### Fixed

- **The caution line widened the column instead of wrapping**, because a horizontal stack measures its children with unlimited width.
- **Settings and application information appeared twice**, the old pair staying behind when they moved to the top right.
- **The process picker offered this application to itself.**

### Changed

- **The diagram fills the space it was given**, rather than sitting at a fixed width in the corner of a large empty card.
- **The note under per-process confinement appears only while it is on**, and says what it does.
- The target path shows in full on hover, however narrow the box is.

## [0.5.9] - 2026-09-12

### Fixed

- **Dark mode was unusable**, because the page never painted a background of its own and dialogs had to be told the theme separately.

### Added

- **The two confinement switches are now a picture**, a grid of who is sending against where to, which answers in a glance what two paragraphs had been failing to.
- **Attaching picks from a list of running programs**, instead of asking for a process id typed in from somewhere else.

### Changed

- The routing warning appears only while that option is on, says one thing, and says it in the colour of a warning.
- The sections lost their numbering; they were never steps to follow in order.
- Settings and application information moved to the top right, with the status cards beneath them.

## [0.5.8] - 2026-09-12

### Fixed

- **Confining the tunnel to one application only worked in one direction**, leaving every other program here reachable from the far end, which is the half that matters when you do not know who is there.

### Changed

- **The routing option is now the other way up** — *Send all traffic through the VPN*, off by default, saying what turning it on means for whoever runs that server.

### Added

- **Light and dark appearance**, or following the system, applied immediately and remembered.
- **Icons through the interface**, and a status light that is green while a session is running.

## [0.5.7] - 2026-09-12

### Fixed

- **A download could not be cancelled**, because it ran while holding the dialog's click deferral, which disables the dialog's own buttons.
- **A cancelled or failed download left its partial file behind**, one per attempt, for ever.
- **Pressing Start with nothing to start did nothing at all**, and now says which choice is missing instead of looking broken.

### Changed

- **Installing an update while a session is running now warns first**, with the safe answer as the default, since installing disconnects the target application.

## [0.5.6] - 2026-09-12

### Fixed

- **A new release is now noticed within about a minute of being published**, using a conditional request that costs nothing while the release is unchanged.
- **Dismissing the update notice left no way back to it**, so both settings and the information dialog now offer *Update now* while one is waiting.
- **Stop said *Stop* while there was nothing left to stop**, and says *Force stop* during the window where the session is waiting out a handover.

### Changed

- **Everything about updating is now in the application information dialog**, beside the version it is being compared against.
- **A downloaded installer is kept when you choose *Later***, until the release it belongs to is superseded.

## [0.5.5] - 2026-09-12

### Fixed

- **Automatic update checks were too rare to feel automatic** — every thirty minutes now, and again when the window comes forward with the last check more than five minutes old.
- **The two confinement options read as duplicates**, and each label now names its own axis: which destinations, or which program.

### Changed

- **The banner button is called *Update now*** rather than *What's new*, because installing is what it does.
- **Settings can start an update**, not only look for one.
- **The empty strip along the top of the window is gone.**
- **The relayed-advertisement count is shown only for Warcraft III**, the one protocol it applies to.

## [0.5.4] - 2026-09-12

### Fixed

- **The update window showed the release notes as their Markdown source** rather than rendering them, which made the thing written to be read painful to read.
- **Updating did not close the application first**, so the installer was replacing files the tunnel and the helper still held open.
- **The window and taskbar kept a generic placeholder icon**, because an unpackaged window does not take the icon from the executable on its own.
- **The Chinese label for "Keep the rest of the machine off the tunnel" described the wrong setting**, which made two orthogonal options look like duplicates.

### Changed

- **The name and tagline no longer occupy the top of the window**; the title bar already says what this is.
- **The application information dialog no longer repeats the name it is titled with**, and shows the version large enough to read at a glance.

## [0.5.3] - 2026-09-12

### Fixed

- **A game hosted on one machine could be seen but not joined from the other**, because only the discovery port was opened while Warcraft III walks up to 6119 when 6112 is taken; the whole hosting range is open now, still to the VPN subnet alone.
- **A room stayed in the other player's list after the host left it**, because the closing announcement is broadcast and never arrives, so the relay notices the host has stopped answering and retracts it.
- **Switching language emptied the target-application and LAN-discovery dropdowns**, because replacing a selected entry reads as that entry being gone; entries now keep their identity and only their text changes.
- **The settings window kept the old language in its own title and button**, which are not part of the content that was relabelled.
- **The activity log did not reliably follow new lines**, because it scrolled before the new line had been laid out.
- **Upgrading rewrote every file, changed or not**, because the old version was removed in full before a single new file was written.
- **One of the log buttons was laid out and clickable but never painted.**

### Added

- **An application information button** beside the settings one, with the version, the copyright and a link to that version's notes.
- **A *Launch LanBridge* checkbox on the installer's last page**, which starts it unelevated, as it is meant to run.

### Changed

- **Closing the target application now ends the session only when the tunnel is bound to it**, since otherwise the tunnel may still be carrying traffic for something else.

## [0.5.2] - 2026-09-11

### Fixed

- **Release notes in the update window showed only their first heading**, because the control breaks lines on a carriage return that the normalized notes no longer carried.

## [0.5.1] - 2026-09-11

### Fixed

- **Games were visible but could not be joined, or did not appear at all** — everything that makes a game joinable arrives inbound and Windows blocks it by default, so a session now opens the discovery port for the VPN subnet alone and puts everything back when it ends.
- **The interface fell apart at any text size above the default, and restarting never brought it back**, because the scaled layer was centred first and then grown from its own top left corner.
- **Downloading an update showed no progress at all**, which is indistinguishable from a download that never started.
- **Switching automatic update checks back on did nothing for up to four hours.**
- **The close-window choice looked as though it had been discarded** when the language was changed during the same visit to Settings.

### Added

- **Update checks now run while the application sits in the notification area**, announced with a balloon and kept in the icon's tooltip.
- **A *Check now* button in Settings**, for when waiting for the next scheduled check is not the point.

## [0.5.0] - 2026-09-11

### Fixed

- **The VPN dropped the moment a game room was opened**, because the search for the process a launcher hands off to only matched executables with the same name.
- **Stop did nothing once a session had already ended**, because closing the pipe to a helper that was gone threw out of the cleanup path.
- **Switching language emptied every dropdown** instead of translating it.
- **The activity log did not follow new lines**, and now stops following the moment you scroll up to read something.

### Added

- **Release notes in your language**, published alongside the builds and shown in the update dialog.
- **Export error report**, bundling the on-screen activity with the log files from both processes.
- The text size setting now scales the whole interface rather than only the activity log.

## [0.4.0] - 2026-09-10

### Added

- **Automatic update checks**, showing what changed before you install; on by default.
- **Settings dialog** behind the gear button, holding language, activity font size, close behaviour and update checking.
- **Ten more interface languages**, alongside English and Traditional Chinese.
- **Activity log is now selectable text**, with *Copy all* and *Export…* buttons.
- **Adjustable activity font size** (10–22pt), remembered between runs.
- **Application icon**, used by the window, the taskbar, the notification area and Add/Remove Programs.
- **Installer now asks where to install**, and offers the two shortcuts as separate choices.

### Fixed

- **The VPN dropped the moment the game finished loading**, because a launcher's first process exiting was read as the target closing; the session follows the handoff now.
- **The tray icon never appeared**, because the icon handle was destroyed before Windows used it.
- **Reopening from the tray started a second copy** instead of restoring the running one.
- **`WinDivert64.sys` stayed locked after closing the app**, because opening a driver handle registers a kernel service that keeps running until it is stopped.
- **Switching the language back to "System default" did nothing**, because a single "everything changed" notification is not acted on reliably.
- **Uninstalling asked for a restart**, since the application and its helper were still holding files.
- Activity log rows had list-item padding that left half a blank line between entries.

### Changed

- Log actions moved to the activity panel on the right, instead of the bottom of the configuration column.

## [0.3.0] - 2026-09-09

### Added

- Error logging to `%LOCALAPPDATA%\LanBridge\logs\`, one file per process per run, with unhandled exceptions recorded rather than ending the application silently.
- Settings persistence on every change, so choices survive a crash or a forced termination.
- Notification-area support with a prompt on close, offering exit or minimise.
- Traditional Chinese interface alongside English.
- MSI installer with shortcuts, version metadata and an Add/Remove Programs entry.

### Fixed

- The published build launched and then died inside the XAML runtime, because an unpackaged WinUI application does not carry its compiled markup.

## [0.2.0] - 2026-09-09

### Fixed

- **The window never appeared after the elevation prompt**, because WinUI 3 cannot run elevated; the interface runs unelevated and hands privileged work to a helper that asks for consent once per session.

## [0.1.0] - 2026-09-09

### Added

- Per-application VPN that refuses the pushed default route and DNS, so only the VPN subnet crosses the tunnel and the rest of the machine keeps its normal path.
- Optional per-process confinement using WinDivert, dropping VPN-subnet traffic from every process except the target's.
- LAN discovery relay for tunnels that cannot carry broadcast, including Warcraft III's W3GS protocol, whose game information is only ever sent as a unicast reply.
- Generic UDP broadcast relay for other games, configured by port.
- Headless command-line driver for the same engine.

[0.5.32]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.32
[0.5.31]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.31
[0.5.30]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.30
[0.5.29]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.29
[0.5.28]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.28
[0.5.27]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.27
[0.5.26]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.26
[0.5.25]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.25
[0.5.24]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.24
[0.5.23]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.23
[0.5.22]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.22
[0.5.21]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.21
[0.5.20]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.20
[0.5.19]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.19
[0.5.18]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.18
[0.5.17]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.17
[0.5.16]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.16
[0.5.15]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.15
[0.5.14]: https://github.com/ACS106129/LanBridge-releases/releases/tag/v0.5.14
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
