# Installing ZerOS

> [Back to the README](../README.md)

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
curl -fsSL https://github.com/AmerZuher/ZerOS-releases/releases/latest/download/install.sh | sudo sh
```

The installer downloads ZerOS for your machine, checks that it's genuine, installs Docker if you
don't have it, and starts ZerOS. It finishes by printing a **setup token**.

Then open `http://<your-server>` in a browser (its name or IP address) and create your account with
that token.

### Options

To see what the installer would change, without changing anything:

```bash
curl -fsSL https://github.com/AmerZuher/ZerOS-releases/releases/latest/download/install.sh | sudo sh -s -- --dry-run
```

To install a specific version instead of the newest one:

```bash
curl -fsSL https://github.com/AmerZuher/ZerOS-releases/releases/latest/download/install.sh | sudo sh -s -- --version 1.0.0
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
