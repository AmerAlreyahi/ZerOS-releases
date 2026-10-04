# Installing ZerOS

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Features](features.md) ·
> [Why you can trust it](trust.md) · [Privacy](privacy.md) · [FAQ](faq.md)

There are two ways to install ZerOS: run the installer on a Linux machine you already have, or
flash a ready-made image onto a Raspberry Pi or a mini PC.

## On a Linux machine you already have

### What you need

- Debian 12 or 13, Ubuntu 22.04 or 24.04, or Raspberry Pi OS (64-bit), on a 64-bit PC or ARM board.
- At least 1 GB of memory and 10 GB of free disk space.
- Nothing else using port 80. If something is, the installer tells you what it is.
- CasaOS must not be installed; ZerOS stops rather than overwrite it.

### Install

```bash
curl -fsSL https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest/download/install.sh | sudo sh
```

The installer downloads ZerOS for your machine, checks that it's genuine, installs Docker if you
don't have it, and starts ZerOS. It finishes by printing a **setup token**.

Then open `http://<your-server>` in a browser (its name or IP address) and create your account with
that token.

### What the installer does

Before it changes anything, it checks that this is a supported system on x86-64 or ARM64, that
systemd runs it, that there is at least 1 GB of memory and about 10 GB free in `/var/lib`, that port
80 is free and that CasaOS is not installed. If one check fails, it stops and says which, and
nothing has been changed.

Then it:

- **checks the release's signature** against the key built into ZerOS, and refuses one that does not
  match;
- **installs what ZerOS uses, from your system's or the maker's own signed repositories**: Docker
  (from Docker's repository, if you don't have it), Avahi (so `zeros.local` works), restic (for
  backups) and smartmontools (for disk health), and NVIDIA's container toolkit if an NVIDIA driver is
  present;
- **creates two system users**, `zeros` and `zeros-gateway`, for the parts of ZerOS that run without
  root;
- **starts ZerOS**: the `zeros-agent` service, and the API and gateway containers.

It does not install Tailscale (Remote Access offers that when you want it), does not change your
network setup, and does not touch apps or containers it did not install. Every step compares what is
there with what should be and changes only the difference, which is why running it again is safe.

### Ports ZerOS uses

| Port | What |
| --- | --- |
| 80 | The desktop, over HTTP on your network and HTTPS on your tailnet |
| 443 | The desktop's secure address too, but only if nothing else uses 443 when ZerOS starts. An app such as a reverse proxy that already holds 443 keeps it |
| Each app's own port | The apps you install, behind ZerOS sign-in |

The API and the host agent listen only on the server itself, never on your network.

### Options

To see what the installer would change, without changing anything:

```bash
curl -fsSL https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest/download/install.sh | sudo sh -s -- --dry-run
```

To install a specific version instead of the newest one:

```bash
curl -fsSL https://github.com/AmerAlreyahi/ZerOS-releases/releases/latest/download/install.sh | sudo sh -s -- --version 1.0.0
```

Running the installer again on a working server changes nothing, so it's safe to repeat.

## Flash an image (Raspberry Pi 4/5, x86-64 mini PCs)

Each release has two images:

- `zeros-<version>-raspberry-pi-arm64.img.xz`: for the **Raspberry Pi 4 and 5**. Write it with
  Raspberry Pi Imager (choose "Use custom"). Imager's settings for a user, an SSH key and Wi-Fi
  still apply.
- `zeros-<version>-x86-64-uefi.img.xz`: for **x86-64 mini PCs with UEFI**. Write it to the machine's
  disk or a USB stick with any image writer. It needs wired Ethernet, and Secure Boot turned off.

Then:

1. Connect the device to your network with a cable and power it on. The first boot takes a few
   minutes, and needs no internet connection.
2. From a computer on the same network, open `http://zeros.local`. **Within 30 minutes of the
   boot**, you can create the administrator account there, with no setup token. Missed it? Turn the
   device off and on again: until someone has claimed it, every boot opens another 30 minutes.
3. The account you create is also the device's **SSH login**: `ssh <username>@zeros.local`, with the
   same password. Changing your ZerOS password later doesn't change the SSH one; use `passwd` over
   SSH for that.

> [!WARNING]
> Anyone on your network during those 30 minutes could claim the device before you. Do the first
> boot on a network you trust, not an open guest Wi-Fi.

## Graphics cards

ZerOS shows your graphics card in Task Manager, and apps that can use one (media servers, AI tools)
ask for your permission first and tell you which card they would get.

- **AMD, Intel and Raspberry Pi:** nothing to do.
- **NVIDIA:** install NVIDIA's driver yourself first (your system's `nvidia-driver` package, or
  NVIDIA's own), restart, then run `sudo zeros repair`. ZerOS then sets up everything apps need to
  use the card. ZerOS never installs the driver itself, because a failed driver install can leave a
  server unable to start.
- **x86-64 image:** NVIDIA's driver is already included, so NVIDIA cards work from the first boot.

## Updating

Settings → Maintenance, or:

```bash
sudo zeros update
```

## Checking ZerOS

Run these on the server.

Is everything running? `zeros-agent` is the host agent; `zeros` runs the API and the app gateway
as two Docker containers, `zeros-api-1` and `zeros-gateway-1`, which should both be "Up":

```bash
systemctl status zeros-agent zeros
sudo docker ps --filter name=zeros
```

Which version is installed, and is a newer one out?

```bash
zeros version
sudo zeros update --check
```

Does the desktop's server answer?

```bash
curl -s http://localhost/api/v1/health
```

The logs, when something is wrong:

```bash
journalctl -u zeros-agent -f
sudo docker logs -f zeros-api-1
sudo docker logs -f zeros-gateway-1
```

Is the installation intact? This lists what `sudo zeros repair` would put back, without changing
anything:

```bash
sudo zeros repair --dry-run
```

In the desktop, Settings → About shows each part's version and whether it is connected, and Task
Manager shows the server's live state.

## If something doesn't work

- **`http://zeros.local` does not open.** Use the server's IP address instead (`hostname -I` on the
  server prints it). Some networks and older Windows versions do not resolve `.local` names.
- **Lost the setup token?** Run `sudo zeros repair`: until the first account exists, it prints the
  token again.
- **The installer says port 80 is in use.** Something else serves a website on this machine. The
  message names the command that shows what it is; stop or move it, then run the installer again.
- **An app does not open.** Open Applications → Server: each app shows its state, and its logs are
  one click away. Task Manager shows whether the server is short of memory or disk.
- **Anything else.** [Checking ZerOS](#checking-zeros) lists the commands that show what ZerOS is
  doing, and `sudo zeros repair` puts back whatever an installation has lost.

## Fixing or removing ZerOS

To put back anything ZerOS set up that has since been changed or removed:

```bash
sudo zeros repair
```

To remove ZerOS but keep your data:

```bash
sudo zeros uninstall
```

To remove ZerOS and everything it stored:

```bash
sudo zeros uninstall --purge --yes
```
