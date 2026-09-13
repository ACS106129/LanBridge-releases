# Changelog

All notable changes to LanBridge are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.5.20] - 2026-09-13

### Added

- **Sites you can send through the VPN without sending the whole machine.** Until now a
  program that needed a site to *see* it arriving from the far end — rather than needing to
  reach a machine there — had only one option, which was handing the VPN everything.

  Name the sites and their addresses are looked up through the tunnel and routed through
  it. Looking them up through the tunnel is the part that matters: a content network
  answers according to where the question came from, and one of these names answered with
  a Taipei edge from here and with different addresses two hours later. Routing that
  address through a Japanese tunnel would be worse than not routing it at all.

  The log says what it found for every name, from both sides, whether they agree or not —
  so when it does not work it says where it stopped rather than only that it stopped.

  Two things worth knowing before switching it on. It changes the machine's routing table,
  so it is empty by default and asked for rather than assumed. And while a session runs
  those sites are reachable by the target application alone: a browser opening the same
  address will not work until the session stops, at which point every route added is taken
  back.

## [0.5.19] - 2026-09-13

### Added

- **Somewhere to put a user name and password.** Some servers ask to be signed in to and
  there was nowhere to say so. A profile whose config says `auth-user-pass` with nothing
  after it means "ask at the console", and openvpn is started here with its output
  redirected and no window — so it asks a question nobody can hear and reports a failed
  sign-in, which reads as a wrong password for a password that was never entered. Manage
  profiles now has a Sign-in button per profile.

  The password is stored unencrypted, and the dialog says so rather than implying
  otherwise: openvpn reads it from a file and there is no way to hand it a secret that is
  not a file it can open. It sits in that profile's own folder, which only you, SYSTEM and
  Administrators can open — the same place as the private keys imported alongside it. It is
  never read back for display: the user name is filled in, the password box is left empty,
  and empty means keep what is stored, so a typo in the name can be fixed without retyping
  the password.

### Fixed

- **The target reached the internet over IPv6, around the tunnel entirely.** Found by
  watching a real session rather than by reading anything: four minutes of traffic, and one
  of the four things it talked to was over IPv6 while every other conversation went where
  it was told. This machine has a global IPv6 address from its provider, the tunnel carries
  IPv4, and nothing in between was looking.

  That was a leak in every mode, including the one that hands the whole machine to the VPN
  — a tunnel that carries no IPv6 cannot carry what the machine sends over IPv6. The
  target's IPv6 is now dropped while a session runs, which makes it fall back to the family
  the tunnel actually carries. Everybody else's IPv6 is untouched.

- **"No update" when nothing had been asked.** Checking for updates while the hourly limit
  was spent reported the version you already had, as though it had looked. Every failing
  path returns the last good answer, which is the right thing to return and the wrong thing
  to present as current — so a release went out and the person who pressed the button was
  told there was nothing there. It now says it could not check, and when it will try again.

  The refusals were self-inflicted. The validator that makes a check cost nothing lived
  only as long as the process, so every start spent one of the sixty requests an hour
  allowed to an unauthenticated caller — and the allowance is per address, so anything else
  on the machine was refused too. It is kept between runs now, which makes the ordinary
  case, that there is no new release, free.

## [0.5.18] - 2026-09-13

### Fixed

- **The About box thanked WinDivert without saying under what terms it is used.** It is
  under the GNU LGPL v3, which asks a program using it to say so, name the licence, and
  point at the copy it ships — and a thank-you is none of those. About now says all three:
  the licence, that the library is unmodified and replaceable, where the full text sits
  beside the program, and where the source is. OpenVPN is named too, along with the fact
  that it is fetched from openvpn.net rather than shipped here.
- **A Warcraft III room's free places never changed on the other machine.** Open a slot
  the computer was sitting in, and the peer went on showing the room exactly as it had
  been until the player left the game list and came back.

  A peer that already has the room in its list does not re-read the full advertisement. It
  takes the counts from a small announce packet the host broadcasts whenever the lobby
  changes, and only rebuilds the entry from scratch when the list is reopened. That
  announce is broadcast, and broadcast is the one thing this cannot capture: the game
  already holds the port it would have to listen on. So the announce is now derived from
  the advertisement instead, and sent when the numbers move.

  The counts are checked before they are sent. They are read from a fixed position at the
  end of a packet whose layout was inferred, and a game has between one and twenty-four
  places and cannot have more free than it has. Anything else means the reading is wrong,
  and then nothing is sent at all.

- **The update download still held the window.** 0.5.16 said this was fixed. The
  background transfer, the progress bar in the main window and the cancel button were all
  written and none of them were ever reached — one search of the source finds the code and
  no caller. The download went on running inside the dialog exactly as before.

  It now runs in the background for real, and the dialog offers **Continue in background**
  once it starts: the window closes, the transfer carries on, and it reports into the bar
  in the main window where it can also be cancelled. Opening the dialog again attaches to
  the download already running rather than starting a second one. Cancelling and failing
  are told apart, too — both end with no file, and both used to open the release page in a
  browser, so stopping a download sent you to a web page.

## [0.5.17] - 2026-09-13

### Fixed

- **One player leaving a Warcraft III lobby closed it to everybody.** Reported as: set a
  player's slot to computer, open or closed and they can never get back in. Those are
  three ways of dropping that player's connection, and a player leaving on their own does
  it too.

  A TCP listener and every connection accepted on it share one local port. The record of
  which ports belong to the game was kept per port, so the listener and the connections
  shared one entry, and the first connection to close took it. Warcraft is still listening
  and still advertising, so the room stays in everyone's list — but every packet arriving
  for that port now belongs, as far as the filter is concerned, to nobody, and is dropped.
  Visible and unjoinable, for everyone, until the host makes a new game.

  Each socket is now tracked separately, and a port stops belonging to the game when its
  last socket closes rather than its first.

- **The target application was still being run as an administrator.** 0.5.16 said it had
  fixed this and had not. Handing a process the signed-in user's identity can be done two
  ways and they want different permissions: the one used needs a privilege an elevated
  administrator does not have and cannot obtain, so it failed every time and the old
  behaviour quietly took over. It now uses the one whose permission the helper actually
  holds, and says which it did in the log.

## [0.5.16] - 2026-09-13

### Added

- **An installer in every language the application speaks.** It spoke eleven and its
  installer spoke two. There are now eleven, one per language, each with the right ANSI
  codepage for its culture — 1252, 932, 949, 936, 950 and 1251 — because Windows Installer
  will not take UTF-8 in its summary information and a mismatched codepage turns the text
  into mojibake. The update offers the one matching the language the window is in.
- **The window opens where you left it.** Size, position and whether it was maximised, kept
  between runs. The restored size is what is saved rather than the size on screen: a
  maximised window that saved its on-screen size would come back filling the display and
  then collapse the moment it was un-maximised. A position that no longer lands on a
  display — a monitor unplugged, a dock left behind, a resolution changed — is dropped
  rather than trusted, because a window restored onto a screen that is not there is a
  window nobody can reach.
- **A new icon.** The old one was a bar with two dots and said nothing about what this
  does. It is now an arrow leaving through the opening of a ring — the tunnel, and the one
  application going through it. Drawn separately at each size rather than scaled down from
  one large image: a sixteen-pixel icon is not a large one made smaller, and the first
  attempt, a literal bridge with a deck and an arch and two piers, read below 48 pixels as
  a table with something spilt on it. The ring is open on the side the arrow leaves by,
  because a closed one with a line through it is the prohibition sign.

### Fixed

- **Start was below the fold.** The two buttons were the last thing in the column of
  configuration cards, and that column scrolls. Once the cards were tall enough to fill
  it — which they are at an ordinary window height — the primary action of the
  application was something you had to scroll down to find. The buttons are now pinned
  under the cards and the cards scroll behind them.
- **The target application was running as an administrator.** The helper that starts it
  has to be one: openvpn configures a virtual adapter and WinDivert loads a driver. A
  child process inherits its parent's token, so the application being launched inherited
  administrator rights it never asked for.

  An elevated program is fenced off from the unelevated desktop. Nothing can be dropped
  onto it from Explorer, and nothing unelevated can hand it anything — which is how a game
  that signs in through the browser never receives its authorization code. The browser is
  unelevated; it launches a second, unelevated copy of the game to deliver the code; that
  copy cannot reach the elevated one already running, and the handoff fails with nothing
  on screen to say so. Settings and saves were also being written with the wrong token,
  which can leave files that cannot be changed later without elevating again.

  It is now started with the shell's token — explorer.exe runs as the signed-in user and
  is never elevated — so it runs as you, with your environment. This is what the installer
  already does with Impersonate="yes" on its launch action, for the same reason.
- **Downloading an update held the whole window hostage.** A hundred megabytes is minutes,
  and a modal dialog for the duration made the application unusable for those minutes over
  something nothing else depends on. The download now runs in the background and reports
  into a bar in the main window, with everything else still reachable and a cancel button
  that works.

## [0.5.15] - 2026-09-13

### Fixed

- **One refusal from the server ended the attempt.** openvpn treats a refused sign-in as
  fatal and exits on the first one, which is right for a server of your own and wrong for
  a public relay: those refuse because they are full or because the volunteer running one
  has gone, and the same profile connects a minute later. It now retries, and stops after
  three tries so a password that is genuinely wrong still gets reported rather than
  retried forever. A profile that sets its own retry limits no longer overrides that —
  how long to keep trying before telling you is this application's decision.
- **"EXITING auth-failure" explained nothing.** It reads like a wrong password, and after
  a certificate has already been accepted it usually is not one. The message now says
  which step failed and what that means: on a public relay, that it is full or gone; on a
  server of your own, that it wants a username and password that were not supplied.

## [0.5.14] - 2026-09-12

### Added

- **An imported profile now belongs to the application.** Importing a .ovpn used to
  remember where the file was and read it again on every run, which works until the file
  moves, the stick comes out, or Downloads is cleared — and then the application is
  holding a path to nothing. It is now copied into a folder of its own, and so is every
  certificate and key it refers to, with those references rewritten to point at the
  copies. Delete the original and nothing changes.
- **Somewhere to see what is being kept.** A Manage button beside Import: what is stored,
  which one is in use, rename, delete, and a way into the folder. Renaming and deleting
  confirm inside the row rather than in a second dialog, because a confirmation that has
  to close the list to ask its question loses the row it is about.
- **OpenVPN, if you do not have it.** This application drives the OpenVPN community
  client; it does not contain one. A machine without it used to get a sentence telling
  the user to go and install it, which is a support instruction dressed up as an error
  message. It now says so before anything is started, and offers to fetch the current
  build from OpenVPN's own download host and install it — refusing to run anything
  Windows does not trust or that is not signed by OpenVPN. Or point it at the copy you
  already have.
- **Tests for the installer.** What is inside the package — product identity, upgrade
  code, both shortcuts as separate choices, the helper and the driver in the payload —
  and a walk through its pages in both languages: the welcome page, the licence gate that
  really gates, the feature tree, where it will install to, and the summary. It stops at
  the summary and cancels, so running the suite installs nothing.
- **A test that looks at the pixels.** Every explanatory line in Settings and About is
  photographed in both themes and measured against what is behind it. Text that cannot be
  read is now a failing test rather than a screenshot in a bug report.

### Fixed

- **Three lines in About were invisible.** The tagline, the copyright and the last-checked
  line were painted with a brush taken from the application's resources, which resolves
  against the application's own theme — and an unpackaged WinUI application cannot change
  that after it starts, while the dialogs are drawn in the theme you picked. Choose the
  theme the application did not start in and the two disagree: white on white, or grey on
  grey. Everything in the markup was fine throughout, which is why this survived so long.
- **The update check stopped asking politely.** Sixty checks an hour is exactly the
  unauthenticated limit, so anything else on the machine using the same API pushed it
  over, and every request after that was refused — once a minute, for an hour, into the
  log. It now reads when the allowance comes back and waits until then, says so once
  rather than sixty times, and the two checks that fired together at startup are one.

- **The installer was writing on top of its own artwork.** Those bitmaps are not pictures
  beside the text — they are the background the dialog writes on, in its own dark colour,
  and the dialog decides where. Filling all 493 pixels with a blue gradient put every page
  title over a dark ground and ran the wordmark through the middle of the welcome
  paragraph. The artwork is now a strip down the left of the welcome page and a block at
  the right of the banner, with the rest left as paper for the installer to write on. A
  test photographs each page and measures the contrast where the text is.
- **The update offered everybody the English installer.** A release carries one MSI per
  language and the updater took the first one in the list, which is whichever was uploaded
  first. Building an installer per language and then handing out the English one at the
  last step undoes the point of building them. It now asks for the one matching the
  language the window is in, and falls back to English when that language has no installer
  of its own.

### Changed

- Every setting has a line under it saying what it changes, and where the settings are
  kept. Five labelled controls and nothing else is a form, not an explanation.
- About says how often updates are looked for and what interrupts, and credits the two
  projects that do the hard parts: OpenVPN's community client and WinDivert.

## [0.5.13] - 2026-09-12

### Added

- **A grand piano.** WinUI has a sound system built into every control and it is silent
  unless an application asks for it; this one never had, so every press has been quiet by
  omission rather than by choice. Its own sounds cannot be replaced and wear thin within
  an afternoon, so the notes come from the General MIDI synthesiser Windows already has,
  on program 0 — Acoustic Grand Piano. No audio files to ship and no library to depend on.
  A pentatonic scale, because presses arrive in whatever order you click in and that is
  the one set where any two of them agree. There is a checkbox in settings for anyone who
  wants a utility to keep its mouth shut.
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
