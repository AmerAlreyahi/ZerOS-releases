# What ZerOS can do

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Why you can trust it](trust.md) ·
> [Privacy](privacy.md) · [Install](installation.md) · [FAQ](faq.md)

A tour of ZerOS, area by area. Everything here is in the current release.

## The desktop

![The ZerOS desktop](../gallery/desktop-home.jpg)

- **A desktop in any browser**, on a computer, a tablet or a phone. Nothing to install on the
  device you use.
- **Twelve desktop styles**, from a quiet overview to a clock in orbit, and **ten themes**, light and
  dark, with your own wallpapers and an adjustable interface size.
- **Up to twelve pinned apps**, the system apps, your server apps and the websites you add.
- **Search everything with Ctrl K**: apps, settings, the server's file names, or the web.
- **Notifications and activity**: what finished, what failed and what needs you, in one place, with
  a link straight to the thing it is about.
- **Live updates**: what changes on the server appears on every open screen at once, without
  reloading.

## Windows

![Text Editor and Photos side by side](../gallery/windows-editor-photos.jpg)

- **Photos, Player, Text Editor and Terminal open as real windows**: drag to move, drag an edge to
  resize, drop on the left or right edge to fill that half, double-click to maximize.
- **The dock** shows every open window; minimize one and bring it back with a click. Up to twelve
  windows at once.
- **Your windows follow you.** Their places, sizes and files are kept for your account, so a reload
  or another device brings them back, fitted to that screen.
- **Text Editor never loses your work.** Unsaved text is kept on the server a few seconds after you
  stop typing, until you save or discard it. If the file changed meanwhile, you choose: keep both,
  discard yours, or carry on editing.
- **A window closes only with its ✕**, and asks first if something is unsaved.

## Apps and the Store

![The Store](../gallery/app-store.jpg)

- **About 350 self-hosted apps** in the Store: photos, media, files and sync, passwords, home
  automation, ad blocking, monitoring, AI, finance, developer tools and more. *Verified on ZerOS*
  marks the apps that were installed, opened and signed in to on a real ZerOS server.
- **Every Docker Official Image** from Docker Hub (nginx, postgres, redis, node, python and more than
  180 others), with a Compose file ZerOS writes for you.
- **Your own apps:** paste a Docker Compose file, or upload a project with its Dockerfile and ZerOS
  builds it on the server.
- **A review before every install** that names what the app may do on your server: your hardware,
  your network, system folders. You approve each risk, or nothing is installed.
- **Start, stop, restart, update, open logs and remove** from Applications, with each app's
  containers, ports, CPU and memory in view.
- **Sign-in in front of every app** you install, with the same ZerOS account, switchable per app.
- **Apps that need HTTPS get it**: ZerOS gives apps like Vaultwarden a secure address on your
  tailnet automatically.
- **Generated passwords** for apps that come with one: ZerOS shows the username and password when
  you ask, and records that you looked.
- **Recipe updates**: when the Store improves an app's recipe, ZerOS shows what would change and lets
  you apply it, keeping your data.

## Files

![Files](../gallery/files-home.jpg)

- **Your Home, your drives and your apps' data**, in one file manager.
- **Upload anything, of any size**: uploads resume where they stopped if the connection drops.
- **Previews** for photos, music, video and text, in their own windows, and **thumbnails** in the
  folder view.
- **Trash** you can restore from, **stars** for the places you use most, **search** by file name, and copy, move and delete that run in the background with progress you can follow.

## Task Manager

![Task Manager](../gallery/task-manager-overview.jpg)

- **Live CPU, memory, network, disk and GPU** (NVIDIA, AMD and Intel cards: use, memory, temperature
  and power, as far as each card reports them), updated every five seconds, with history kept as
  per-minute and per-hour summaries.
- **Every process**, with the option to reload or stop one.
- **Every app's** CPU and memory use, state and sign-in.
- **Every disk and volume** with its health (SMART) and space, and actions to mount, unmount and
  format.

## Storage

![Storage](../gallery/task-manager-storage.jpg)

- **Attach a drive and use it**: mount it for your files or for backups, or format it, with your
  confirmation for every step. ZerOS never mounts, formats or moves anything on its own.
- **Move your apps' data, or Docker's own data, to a bigger drive**, with a verified copy.
- **Disk health alerts** before a drive fails.

## Backups

- **Scheduled, encrypted backups** (restic) of your apps' data, their settings, or any folder, to
  another drive or folder.
- **Keep as many versions as you like**, run a backup now, **check** a backup, and **restore** with
  one click, with your confirmation before anything is overwritten.
- **Stop apps during a backup**, if you choose, so their databases are copied consistently.
- **Warnings** when a backup goes to the same disk as its source, or runs late.

## Updates

- **ZerOS, your system and your apps**, from Settings → Maintenance.
- **ZerOS updates itself** from signed releases only, and rolls back on its own if the new version
  does not come up.
- **App updates roll back** on their own if the updated app is not healthy.
- **A daily check** tells you what is waiting; app updates can install themselves if you turn that
  on.

## Alerts

- **Disks filling up**, **failing drives**, **apps that stop** and **a host agent that stops
  answering**, with thresholds you set, cleared only when the problem is really gone.
- **In the desktop always**, and also by **email, ntfy, Gotify or Telegram** if you set them up.

## Remote access

![Remote Access](../gallery/remote-access.jpg)

- **Set up Tailscale from the desktop**: a private, encrypted network between your own devices, with
  nothing opened on your router.
- **Secure `https://` addresses** for the desktop and for every app, with certificates your browser
  trusts.
- **Sign in without a password** from your own devices: say which Tailscale login is which ZerOS
  account.
- **See every machine on your tailnet**, online or not, and copy its address.

## Network and system

- **`zeros.local`**: reach the server by name on your home network.
- **Wi-Fi from Settings**: see the networks around the server and switch to one. If the desktop does
  not answer over Wi-Fi, ZerOS switches back by itself.
- **Terminal**: a shell on your server, in a window, after confirming your password.
- **Restart and shut down** the server from the desktop.
- **Settings → About** shows the version of every part of ZerOS and whether it is connected.

## Installation

- **One command** on Debian 12 or 13, Ubuntu 22.04 or 24.04, or Raspberry Pi OS (64-bit), on x86-64
  or ARM64. It checks everything first, changes only what is missing, and can show you what it would
  do without doing it.
- **Ready-to-flash images** for the Raspberry Pi 4 and 5 and for x86-64 mini PCs.
- **Graphics cards** for your apps: AMD, Intel and Raspberry Pi work as they are; for NVIDIA, install
  the driver and ZerOS sets up the rest (the x86-64 image already includes it).
- **Repair and uninstall** commands put back what is missing, or remove ZerOS while keeping your
  data.

See the [installation guide](installation.md).
