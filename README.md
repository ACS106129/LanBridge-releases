# LanBridge — downloads

Builds of **LanBridge**, published here so they can be downloaded without a GitHub
account. The source lives in a private repository; this one holds nothing but releases.

> GitHub does not serve release assets from a private repository to anyone without a
> token, and shipping a token inside the application would hand every downloader read
> access to the source. Hence the split.

## Get it

Take the latest from the [releases page](https://github.com/ACS106129/LanBridge-releases/releases).

| File | Use |
| --- | --- |
| `LanBridge-<version>-x64.msi` | Installer. Asks where to install and whether you want Start menu and desktop shortcuts. |
| `LanBridge-<version>-win-x64.zip` | The same build, no installer. Unpack and run `LanBridge.exe`. |

Both are self-contained: no .NET runtime needs to be installed first.

The application itself runs without administrator rights. Consent is requested once per
session, when you press Start, because the OpenVPN client has to configure a virtual
network adapter and — if per-process confinement is enabled — the WinDivert driver has to
be loaded.

## What it does

Runs one application over a VPN without routing the rest of the machine through it, and
bridges LAN game discovery across tunnels that cannot carry broadcast.

Two problems, specifically:

**A VPN takes over the whole machine.** Servers commonly push a default route and DNS
settings that redirect everything. LanBridge refuses those on the client side, so only
the VPN subnet goes through the tunnel. Optionally it goes further and drops VPN-subnet
traffic from every process except the one you launched.

**LAN discovery does not cross a layer-3 tunnel.** Game discovery relies on UDP broadcast,
which is never routed. LanBridge forwards those announcements to peers as unicast
instead. Warcraft III needs more than that — the packet carrying the game name, map and
port is never broadcast at all, only sent as a unicast reply — so LanBridge asks the
local game for it and forwards the answer.

## Requirements

- Windows 10 version 1903 or later, 64-bit
- The OpenVPN community client

## Changes

See [changelog/CHANGELOG.en.md](changelog/CHANGELOG.en.md), and the same file in ten other languages beside it. Each release's notes are generated from it, so the
release page, the changelog and the in-app update dialog always say the same thing.

## Third-party components

Includes [WinDivert](https://reqrypt.org/windivert.html) under its own licence, a copy of
which is installed alongside the application. WinDivert loads a kernel-mode driver while
per-process confinement is active; the driver is unloaded when the session ends.
