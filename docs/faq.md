# Questions and answers

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Features](features.md) ·
> [Why you can trust it](trust.md) · [Privacy](privacy.md) · [Install](installation.md)

## The basics

**Is ZerOS free?**
Yes. Install and use it on as many of your own machines as you like, at home or at work. The terms
are in [LICENSE.md](../LICENSE.md).

**Is it open source?**
No; its source code is not public. ZerOS 1.1.1 and every version before it were published under the
MIT licence and stay under it. How ZerOS protects you is documented in
[Why you can trust ZerOS](trust.md), and every release is signed.

**What do I need?**
A 64-bit machine with at least 1 GB of memory and 10 GB of free space, running Debian 12 or 13,
Ubuntu 22.04 or 24.04, or Raspberry Pi OS (64-bit). A Raspberry Pi 4 or 5, a mini PC or an old
desktop all work; more memory means more apps at once. Or flash a ready-made image: see
[Install](installation.md).

**Does it need the internet?**
Your desktop, files and running apps work on your home network without it. ZerOS needs the internet
to install and update itself and your apps, to load the Store, and for remote access.
[Privacy](privacy.md) lists every connection it makes.

**Can I use it from my phone?**
Yes. The desktop works in a phone's browser, with one window at a time on small screens. Away from
home, install the Tailscale app on the phone and open your server's secure address.

## Apps

**Which apps can I install?**
About 350 from the Store, every Docker Official Image from Docker Hub, and any app you have as a
Docker Compose file or as a project with a Dockerfile.

**Will it touch the Docker containers I already run?**
No. ZerOS manages the apps it installed and leaves everything else alone.

**Can I run ZerOS next to CasaOS?**
No. Both want the same ports and the same apps, so the installer stops rather than overwrite CasaOS.

**What happens if an app update breaks the app?**
ZerOS notices that the app did not come back healthy and puts the previous version back on its own,
then tells you.

## Your data

**Where are my files?**
On your server's disk: your Home in `/var/lib/zeros/home`, your apps' data in
`/var/lib/zeros/appdata`, and drives you attach under `/mnt/zeros`. They are ordinary files and
folders.

**How do I back up?**
Settings → Backups. Schedule encrypted backups of your apps' data or any folder to another drive or
folder, keep as many versions as you want, and restore with a click.

**Does ZerOS send my data anywhere?**
No. There is no telemetry and no account with us. See [Privacy](privacy.md).

**Can I leave ZerOS later?**
Yes. `sudo zeros uninstall` removes ZerOS and keeps your files, your apps and their data. Your apps
are plain Docker Compose projects.

## Remote access and security

**How do I reach my server away from home?**
Remote Access sets up Tailscale, a private encrypted network between your own devices. Nothing is
opened on your router.

**Can I put ZerOS on a server with a public IP address (a VPS)?**
ZerOS is designed for a machine on your own network. On a machine with a public address, the
desktop and your apps would be reachable from the internet, on your network's plain HTTP. If you do
it anyway, let a firewall allow only your tailnet.

**Is there two-factor sign-in?**
Not yet; it is planned. Use a strong password, and keep remote access on Tailscale.

**Can several people have accounts?**
Managing accounts from the desktop is planned. Today, the person who sets the server up is its
administrator.

**I found a security problem.**
Please report it privately: see [SECURITY.md](../SECURITY.md).

## Updates and problems

**How do I update?**
Settings → Maintenance, or `sudo zeros update` on the server. ZerOS only installs releases signed
with its key, and rolls back on its own if a new version does not come up.

**Something is not working.**
[If something doesn't work](installation.md#if-something-doesnt-work) and
[Checking ZerOS](installation.md#checking-zeros) cover the usual cases.
