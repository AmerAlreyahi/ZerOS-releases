<div align="center">
  <img src="gallery/zeros-banner.png" alt="ZerOS" width="100%" style="border-radius: 28px; border: 1px solid #23416c;" />

# ZerOS

Your own personal server, with a real desktop you open in any browser.<br />
Your photos, files, media and passwords on your hardware: private, backed up, and yours.

**Latest version: [see Releases](https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest)**
</div>

<p align="center">
  <a href="docs/why-zeros.md"><b>Why ZerOS</b></a> ·
  <a href="#install">Install</a> ·
  <a href="docs/features.md">Features</a> ·
  <a href="#new-in-11">New in 1.1</a> ·
  <a href="#screenshots">Screenshots</a> ·
  <a href="docs/trust.md">Why you can trust it</a> ·
  <a href="docs/privacy.md">Privacy</a> ·
  <a href="docs/faq.md">FAQ</a>
</p>

<p align="center">
  <img src="gallery/desktop-home.jpg" alt="ZerOS desktop showing the clock, pinned apps and live server use" width="100%" style="border-radius: 24px; border: 1px solid #23416c;" />
</p>

## What is ZerOS?

ZerOS turns a Linux machine (a mini PC, a Raspberry Pi or an old computer) into a personal server,
and gives it a desktop you open in any browser. Install Immich for your photos, Jellyfin for your
films, Vaultwarden for your passwords or Home Assistant for your home in one click each, and manage
all of it the way you use a computer: windows, apps, a file manager and a task manager, behind one
sign-in. No terminal, no YAML.

This repository holds the ZerOS releases: the installer, the program and ready-to-flash images.

## Why ZerOS

- **A real desktop, not a control panel.** Apps open in windows you move, resize, snap and keep
  open side by side, and they come back after a reload or on another device.
- **Safe by design.** The part you talk to runs without privileges and cannot touch Docker; only a
  small agent is root, and it accepts a short list of fixed operations, never a command.
  [How ZerOS protects you →](docs/trust.md)
- **Every app is reviewed before it runs**, and anything that reaches beyond the app needs your yes.
  Every app sits behind your ZerOS sign-in, and never sees your session.
- **Updates that undo themselves.** ZerOS, your system and your apps update from one place; a signed
  ZerOS release or an app that does not come back healthy is rolled back on its own.
- **Backups, alerts and monitoring built in**: encrypted scheduled backups, alerts for full disks and
  failing drives by email, ntfy, Gotify or Telegram, and live CPU, memory, disk and GPU use.
- **Reach it from anywhere** through Tailscale, with secure `https://` addresses and nothing opened
  on your router.
- **Private.** No telemetry, no analytics, no account with us. [What leaves your server →](docs/privacy.md)

[Read the full case for ZerOS →](docs/why-zeros.md)

## Install

### Choose an installation option

| Option | Use it for | What happens |
| --- | --- | --- |
| **Installer script (`install.sh`)** | An existing supported Linux host on x86-64 or ARM64 | Installs ZerOS and its dependencies on your current OS; finishes with a setup token |
| **x86-64 UEFI image** — `zeros-<version>-x86-64-uefi.img.xz` | An Intel/AMD 64-bit mini PC with UEFI | Replaces the selected USB stick or disk with a complete bootable system; requires wired Ethernet and Secure Boot off |
| **Raspberry Pi ARM64 image** — `zeros-<version>-raspberry-pi-arm64.img.xz` | Raspberry Pi 4 or 5 | Replaces the selected SD card or boot media with a complete bootable system; use Raspberry Pi Imager's **Use custom** option |

A `.img.xz` is a **compressed full disk image**, with the system's disk layout and boot files
already prepared. It is not an ISO installer: write it with an image writer, rather than copying
the file onto a drive. Booting a flashed USB runs ZerOS from that USB; it does not automatically
install ZerOS onto the PC's internal disk. Flashing erases the selected drive.

Choose an image that is actually attached to the release: 1.1.0 and 1.1.1 have no flashable images.
See the [installation guide](docs/installation.md) for architecture selection, download
verification, flashing and first boot.

### What you need for the installer script

- Debian 12 or 13, Ubuntu 22.04 or 24.04, or Raspberry Pi OS (64-bit), on a 64-bit PC or ARM board.
- At least 1 GB of memory and 10 GB of free disk space.
- Nothing else using port 80 (ZerOS serves its desktop there).

### One command

```bash
curl -fsSL https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest/download/install.sh | sudo sh
```

The installer checks that the download is genuine before it changes anything. When it finishes, it
prints a **setup token**: open `http://<your-server>` in a browser and use the token to create your
account.

To see what it would change without changing anything:

```bash
curl -fsSL https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest/download/install.sh | sudo sh -s -- --dry-run
```

### Or flash an image

For the image options above, follow the [flashing and first-boot steps](docs/installation.md#flash-an-image-raspberry-pi-45-x86-64-mini-pcs). A flashed device is claimed from the local network
within 30 minutes of boot, without the installer's setup token.

## What you get

<p align="center"><img src="gallery/zeros-feature-orbit.svg" alt="ZerOS features: apps, files, system insight and your settings" width="100%" style="border-radius: 24px;" /></p>

- **Apps:** browse a store of self-hosted apps, see what each one will be allowed to do before you
  install it, then start, stop, update or remove it in one click.
- **Files:** upload, download, preview, organise and search your files, with a trash you can
  restore from. Photos, music, video and text open in their own windows.
- **Task Manager:** live CPU, memory, storage, network and GPU use, per-app usage and history.
- **Backups:** scheduled backups of your apps and folders, which you can check and restore.
- **Remote access:** reach your server securely from anywhere through Tailscale, without opening
  ports on your router, at a secure `https://` address your browser trusts.
- **Terminal and Wi-Fi:** a terminal on your server and Wi-Fi setup, right from the desktop.
- **Your desktop, your way:** ten themes, twelve desktop styles, wallpapers and up to twelve pinned
  apps.

<p align="center"><img src="gallery/zeros-ecosystem.svg" alt="Apps, files and live system insight" width="100%" style="border-radius: 24px;" /></p>

## New in 1.1

<p align="center">
  <img src="gallery/windows-editor-photos.jpg" alt="Text Editor and Photos open side by side as windows" width="100%" style="border-radius: 24px; border: 1px solid #23416c;" />
</p>

- **Real windows.** Photos, Player, Text Editor and Terminal open as windows you can move, resize,
  snap to half the screen, minimize to the dock and keep open side by side. They are kept for your
  account: a reload, or another device, brings them back.
- **Nothing you type is lost.** Text Editor keeps unsaved text on your server until you save or
  discard it, and never overwrites a file that changed meanwhile without asking.
- **Docker Hub in the Store.** Install any Docker Official Image, reviewed like every other app.
- **A secure address for the desktop.** On your tailnet, ZerOS opens at
  `https://<your-server>.<tailnet>.ts.net`, with a certificate your browser trusts.

<table>
  <tr>
    <td width="50%" align="center"><img src="gallery/windows-player.jpg" alt="Music and a video playing in two Player windows" width="100%" /><br /><b>Player windows</b></td>
    <td width="50%" align="center"><img src="gallery/store-dockerhub.jpg" alt="The Store's Docker Hub tab with Docker Official Images" width="100%" /><br /><b>Docker Hub</b></td>
  </tr>
  <tr>
    <td width="50%" align="center"><img src="gallery/remote-access.jpg" alt="Remote Access: the desktop's and each app's secure address" width="100%" /><br /><b>Secure addresses</b></td>
    <td width="50%" align="center"><img src="gallery/remote-access-devices.jpg" alt="Remote Access: the machines on your tailnet and who signs in without a password" width="100%" /><br /><b>Your tailnet</b></td>
  </tr>
</table>

## Screenshots

<table>
  <tr>
    <td width="50%" align="center"><img src="gallery/app-store.jpg" alt="The ZerOStore app store, with Immich in the spotlight" width="100%" /><br /><b>Store</b></td>
    <td width="50%" align="center"><img src="gallery/applications.jpg" alt="Applications: system apps and running apps" width="100%" /><br /><b>Applications</b></td>
  </tr>
  <tr>
    <td width="50%" align="center"><img src="gallery/task-manager-overview.jpg" alt="Task Manager: live CPU, memory, network, disk and an NVIDIA RTX 3090" width="100%" /><br /><b>Task Manager</b></td>
    <td width="50%" align="center"><img src="gallery/files-home.jpg" alt="Files: the Home folder with its default folders" width="100%" /><br /><b>Files</b></td>
  </tr>
  <tr>
    <td width="50%" align="center"><img src="gallery/applications-server.jpg" alt="Applications, Server: installed apps with their containers, ports and ZerOS sign-in" width="100%" /><br /><b>Server apps</b></td>
    <td width="50%" align="center"><img src="gallery/task-manager-storage.jpg" alt="Task Manager, Storage: every disk and volume, with mount and format actions" width="100%" /><br /><b>Storage</b></td>
  </tr>
  <tr>
    <td width="50%" align="center"><img src="gallery/settings-appearance.jpg" alt="Settings, Appearance: ten themes and display options" width="100%" /><br /><b>Themes</b></td>
    <td width="50%" align="center"><img src="gallery/login.jpg" alt="Sign-in" width="100%" /><br /><b>Sign-in</b></td>
  </tr>
</table>

<p align="center">
  <img src="gallery/settings-styles.jpg" alt="Settings, Styles: twelve desktop styles to choose from" width="100%" style="border-radius: 24px;" />
</p>

<p align="center">
  <img src="gallery/poster.png" alt="ZerOS: your server, your apps, one beautiful web desktop" width="100%" style="border-radius: 24px;" />
</p>

## Updates and backups

**Updates:** Settings → Maintenance updates ZerOS, your system and your apps. Every update is
checked before it's installed, and if something goes wrong afterwards, ZerOS puts back the
previous version by itself.

**Backups:** Settings → Backups copies your app data or any folder to another drive or folder, on a
schedule, keeping as many versions as you choose.

**Is it running?** The [installation guide](docs/installation.md#checking-zeros) lists the
commands that show ZerOS's state, version and logs on the server.

## Built to be trusted

ZerOS assumes any part of it that faces the network could one day have a bug, and makes sure no
single bug can take over your machine.

| | |
| --- | --- |
| **Least privilege** | The API runs unprivileged in a read-only container with every Linux capability dropped and no Docker access. Only the host agent is root, and it runs fixed programs with fixed arguments, never a shell. |
| **Apps can't see your session** | Every app sits behind ZerOS's gateway, which strips ZerOS's cookies and identity headers before a request reaches it. |
| **Review before install** | Privileged containers, host access, devices and system mounts are spelled out and need your explicit yes; mounting ZerOS's own folders is refused outright. |
| **Confined files** | Every file operation stays inside your Home, your drives and app data, and cannot follow a link out. |
| **Signed releases** | ZerOS installs and updates only from releases signed with its key, and rolls back if a new version does not come up. |
| **Accounts done right** | A one-time setup token, Argon2id passwords, strict cookies, CSRF protection, slowed-down guessing and an audit log. |
| **No open ports** | Remote access goes through Tailscale's encrypted network; nothing is exposed to the internet. |

[Read how ZerOS protects you, and what it does not do yet →](docs/trust.md) · To report a security
problem, see [SECURITY.md](SECURITY.md).

## Learn more

| | |
| --- | --- |
| [Why ZerOS](docs/why-zeros.md) | What makes ZerOS different, and the questions to ask any web OS for your server |
| [Features](docs/features.md) | Everything ZerOS can do, area by area |
| [Why you can trust ZerOS](docs/trust.md) | Its security design, and its limits |
| [Privacy](docs/privacy.md) | Every connection ZerOS makes, and what stays on your server |
| [Installing ZerOS](docs/installation.md) | Install, flash an image, check, repair and remove |
| [FAQ](docs/faq.md) | Questions people ask |
| [Changelog](CHANGELOG.md) | What changed in each version |

## Good to know

ZerOS is young and moves fast, with a tested release for every step. It runs on its creator's own
server and is tested on Ubuntu 24.04 and Debian 12 on x86-64; the Raspberry Pi image has not yet
been booted on a real Pi. What changed in each version is in the [changelog](CHANGELOG.md).

## Licence and credits

ZerOS is free to install and use; its source code is not public. The terms are in
[LICENSE.md](LICENSE.md). The photographers and projects it builds on are credited in
[CREDITS.md](CREDITS.md).

## About the creator

ZerOS is created and maintained by [Amer Zuher Alreyahi](https://github.com/AmerAlreyahi).

<p>
  <a href="https://github.com/AmerAlreyahi">GitHub</a> ·
  <a href="https://amer-alreyahi.vercel.app">Portfolio</a> ·
  <a href="https://www.linkedin.com/in/amer-zuher-alriyahi">LinkedIn</a>
</p>
