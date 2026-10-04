# Why ZerOS

> [Home](../README.md) · [Features](features.md) · [Why you can trust it](trust.md) ·
> [Privacy](privacy.md) · [Install](installation.md) · [FAQ](faq.md)

ZerOS turns a Linux machine you own into a personal server, and gives it a desktop you open in any
browser. Your photos, files, media, passwords and home automation run on your hardware, under your
control, and you manage all of it the way you use a computer: with windows, apps, a file manager and
a task manager. No terminal, no YAML, no dashboard of a hundred settings.

## Built like an operating system, not a control panel

Most self-hosting tools give you a page of tiles. ZerOS gives you a desktop.

- **Real windows.** Photos, Player, Text Editor and Terminal open as windows you move, resize, snap
  to half the screen, minimize to the dock and keep open side by side. ZerOS keeps them for your
  account: reload the page, or sign in on another computer, and your windows come back where you
  left them.
- **Nothing you type is lost.** Text Editor keeps your unsaved text on the server seconds after you
  stop typing. A crash, a closed tab or a dead battery does not cost you a word. If the file changed
  while you were editing, ZerOS offers to keep both; it never overwrites without asking.
- **One sign-in for everything.** Apps you install sit behind the same ZerOS sign-in as the
  desktop. On your own devices, connected through Tailscale, you can be signed in without typing a
  password at all.
- **It feels like yours.** Ten themes, twelve desktop styles, your own wallpapers, up to twelve
  pinned apps, and a search box (Ctrl K) that finds apps, settings and file names.

## Apps without the homework

- **Hundreds of apps, one click each.** The Store holds about 350 self-hosted apps (Immich,
  Jellyfin, Nextcloud, Vaultwarden, Home Assistant, Pi-hole and many more), plus every Docker
  Official Image. Apps marked *Verified on ZerOS* were installed, opened and signed in to on a real
  ZerOS server.
- **You see what an app may do before it runs.** Every app, from the Store or your own Compose
  file, is reviewed before it is installed. Anything that reaches beyond the app itself (your
  hardware, your network, system folders, the Docker socket) is spelled out, and needs your yes.
  Some things are never allowed at all.
- **Updates that undo themselves.** ZerOS checks for app updates without downloading anything,
  updates one click at a time (or automatically, if you choose), and if an updated app does not come
  back healthy, it puts the previous version back on its own.
- **Your own projects too.** Paste a Compose file, or upload a project with its Dockerfile, and
  ZerOS builds and runs it with the same review and the same sign-in as a Store app.

## A server that looks after itself

- **Backups you can restore.** Schedule encrypted backups of your apps' data or any folder to
  another drive, keep as many versions as you like, check them, and restore with a click.
- **Updates for everything, in one place.** ZerOS itself, your system's packages and your apps,
  from Settings → Maintenance. ZerOS updates only to releases signed with its key, and rolls itself
  back if a new version does not come up.
- **It tells you before it hurts.** Alerts for disks filling up, failing drives (SMART), apps that
  stop and an agent that stops answering, in the desktop and, if you like, by email, ntfy, Gotify or
  Telegram.
- **You can see everything.** Task Manager shows live CPU, memory, network, disks and NVIDIA GPU
  use, per-app usage, every process, and its history.

## Reach it from anywhere, safely

Remote Access sets up [Tailscale](https://tailscale.com/) for you: a private, encrypted network
between your own devices. Nothing is opened on your router and nothing is exposed to the internet.
Your desktop and your apps then have secure `https://` addresses with certificates your browser
trusts, which is what apps like Vaultwarden need to work at all.

## What to ask of any web OS for your server

Before you trust a system with your photos and passwords, ask it these questions. These are ZerOS's
answers; [Why you can trust ZerOS](trust.md) explains each one in detail.

| Question | ZerOS |
| --- | --- |
| If the web interface has a bug, what can an attacker reach? | Only a short list of fixed operations. The web-facing part runs unprivileged, in a read-only container, with no Docker access; only a small host agent is root, and it accepts a fixed set of typed requests, never a command. |
| Can an app I install see my ZerOS session? | No. Every app sits behind ZerOS's gateway, which strips ZerOS's cookies and identity headers before a request reaches the app. |
| Can I install an app that quietly takes over the machine? | Not quietly. Risky permissions must be accepted one by one and are recorded; mounting ZerOS's own folders or its control socket is refused outright. |
| What stops someone on my network claiming a new server? | The installer prints a one-time setup token; without it, nobody can create the first account. |
| How do I know an update is genuine? | Every release is signed. ZerOS checks the signature before it installs or updates anything, and refuses a release that does not match. |
| What happens if an update breaks something? | Apps and ZerOS itself roll back to the previous version on their own. |
| Does it phone home? | No. No telemetry, no analytics, no account with us. [Privacy](privacy.md) lists every connection ZerOS makes and why. |
| Do I have to open ports on my router to use it away from home? | No. Remote access goes through Tailscale's encrypted network. |
| Can it read files outside the folders it shows me? | No. Every file operation is confined to your Home, your drives and app data, and cannot follow a link out of them. |
| Can I leave? | Yes. Your apps are plain Docker Compose projects and your files are plain files on your disks. `zeros uninstall` removes ZerOS and keeps your data. |

## Who ZerOS is for

- **People who want their own cloud** — photos, files, media, passwords — without renting it.
- **Home-lab tinkerers** who want a calm front end for the Docker apps they already run.
- **Families** sharing one machine that should just work, and update and back itself up.
- **Anyone with a spare PC, mini PC or Raspberry Pi**, from a Raspberry Pi 4 to a GPU workstation.

## Free to use

ZerOS is free to install and use on as many of your own machines as you like, at home or at work.
Its source code is not public; the terms are in [LICENSE.md](../LICENSE.md).

[Install ZerOS →](installation.md)
