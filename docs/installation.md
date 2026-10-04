# Installing ZerOS

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Features](features.md) ·
> [Why you can trust it](trust.md) · [Privacy](privacy.md) · [FAQ](faq.md)

There are two ways to install ZerOS: run the installer on a Linux machine you already have, or
flash a ready-made image onto a Raspberry Pi or a mini PC.

## Choose your installation path

| Path | Hardware / OS | Disk and account setup |
| --- | --- | --- |
| `install.sh` on an existing host | x86-64 or ARM64 running a supported Linux distribution | Keeps the host OS and disk layout; installs services and dependencies; prints a setup token |
| x86-64 UEFI image | Intel/AMD 64-bit mini PC with UEFI | Writes a complete system onto the selected drive; claim the first account on the LAN |
| Raspberry Pi ARM64 image | Raspberry Pi 4 or 5 | Writes a complete system onto the selected boot media; claim the first account on the LAN |

The script uses the Linux distribution already installed on your host. The image supplies its own
Linux system and ZerOS together; the public release docs do not identify its exact base distribution
or version. Do not assume the image is the same distribution as your existing host. Once running,
`cat /etc/os-release` shows the host distribution.

### Select the right architecture

On an existing Linux host, `uname -m` reports `x86_64` for Intel/AMD 64-bit PCs or `aarch64` for
ARM64. The corresponding release programs are `zeros-linux-amd64` and `zeros-linux-arm64`; the
installer selects the appropriate one. A 32-bit OS is not supported, even on a 64-bit CPU.

For flashing, select by device as well as CPU: the Raspberry Pi image is for Pi 4/5, not every
ARM64 board, and the x86-64 image needs UEFI rather than legacy BIOS boot. An ARM64 host running a
supported distribution can use the script without using the Raspberry Pi image.

The Docker image used by the API and gateway is a component of the installation, not a bootable
OS image or a replacement for the documented host installation.

### Before you start

Back up anything important. For the script, have terminal or SSH access to the target server,
root access through `sudo`, and internet access to download ZerOS and dependencies. For flashing,
use another computer with an image writer and media large enough for the **uncompressed** image,
plus space on the writing computer if you need to decompress it. The 1 GB memory / 10 GB free-space
requirements below describe the running host; compressed download size is not the required media
capacity. Allow extra space and memory for the apps and data you plan to keep.

## Verify downloads

Download files from the [official release](https://github.com/AmerAlreyahi/ZerOS-releases/releases)
and keep the program/image and verification files from the **same version** together. Check the
release's actual assets before following a command: images are available in 1.1.2, but absent from
1.1.0 and 1.1.1.

### SHA-256 checks

Download `SHA256SUMS` from the same release. Only compare files that it actually lists. On Linux,
for example, with `zeros-linux-amd64` and `SHA256SUMS` in the current folder:

```bash
sha256sum zeros-linux-amd64
```

On macOS, use `shasum -a 256 zeros-linux-amd64`. On Windows PowerShell:

```powershell
Get-FileHash .\zeros-linux-amd64 -Algorithm SHA256
```

Compare the complete hash with the entry for that exact filename in `SHA256SUMS`; letter case does
not matter. Substitute your downloaded image's filename to calculate its hash **before
extracting or flashing**. If all files listed in `SHA256SUMS` are present, Linux can check them with
`sha256sum -c SHA256SUMS`. Missing files are not successful checks.

Do not assume `SHA256SUMS` covers images added after the main release. If an image is not listed,
compare against its SHA-256 digest in GitHub's release asset metadata when available. The metadata
can be viewed at
`https://api.github.com/repos/AmerAlreyahi/ZerOS-releases/releases/tags/v1.1.2`
(replace the tag for another version); match the asset's `name` and `digest` (`sha256:...`). If
neither source supplies a hash, a local hash alone cannot verify the download. Stop if a published
hash does not match and download again from the official release.

### Signatures and their scope

SHA-256 detects a mismatch with the published file; it is not, by itself, proof of the publisher's
identity. Releases ship `release.json` and `release.json.sig`; the release manifest uses Ed25519,
and `zeros setup` and `zeros update` verify it against the public key built into ZerOS. The
installer downloads and checksums the architecture-specific program, then runs `zeros setup`.
This is distinct from manually checking an image before writing it.

The 1.1.2 asset list does not include `images.json` or an image signature. Do not treat
`release.json.sig` as a detached signature for an arbitrary `.img.xz`, and do not assume a
signature is GPG-compatible from its filename or asset type. This public repository supplies no
standalone key/verifier procedure for authenticating image downloads. Building an image from a
verified ZerOS release does not replace verification of the downloaded image itself.

The one-command installation below executes the downloaded `install.sh` bootstrap script. If you
prefer to inspect it first, download it to a file from the chosen official release, read it, and
compare its hash with `SHA256SUMS` if listed, before running `sudo sh ./install.sh`. That checksum
comparison has the same authenticity limits described above.

## On a Linux machine you already have

### What you need

- Debian 12 or 13, Ubuntu 22.04 or 24.04, or Raspberry Pi OS (64-bit), on a 64-bit PC or ARM board.
- At least 1 GB of memory and 10 GB of free disk space.
- Nothing else using port 80. If something is, the installer tells you what it is.
- CasaOS must not be installed; ZerOS stops rather than overwrite it.
- The host must run systemd; use root access through `sudo` and an internet connection.

### Install

Run this on the **target Linux server**, not the computer you only use to open its desktop.
The script installs on the running OS; it does not flash a disk or install a replacement OS.


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

### Full disk image, not an ISO installer

A `.img.xz` is an XZ-compressed raw disk image containing the prepared disk layout, filesystems,
boot files and Linux system with ZerOS. Writing it replaces the selected drive's contents,
including its partition table. It is not an ISO with a wizard that later copies ZerOS to another
disk, and copying the `.img.xz` file onto a formatted USB does not make that USB bootable.

Available releases offer these two device-specific images:

- `zeros-<version>-raspberry-pi-arm64.img.xz`: for the **Raspberry Pi 4 and 5**. Write it with
  Raspberry Pi Imager (choose "Use custom"). Imager's settings for a user, an SSH key and Wi-Fi
  still apply.
- `zeros-<version>-x86-64-uefi.img.xz`: for **x86-64 mini PCs with UEFI**. Write it to the machine's
  disk or a USB stick with any image writer. It needs wired Ethernet, and Secure Boot turned off.

### Download and write the image

1. Open the release's **Assets** and download the correct `.img.xz`. Use the same version's
   verification files and [verify the download](#verify-downloads).
2. Back up the selected SD card, USB stick or disk. Flashing **erases that whole drive**.
   Confirm the drive's identity and capacity before starting.
3. For Raspberry Pi 4/5, open Raspberry Pi Imager, choose **Use custom**, select the image, and
   choose the target media. Imager's user, SSH-key and Wi-Fi settings apply as described above.
   Wired Ethernet is the simplest first-boot path.
4. For x86-64, use an image writer that supports raw disk images and select the target USB or
   disk. If the writer supports XZ-compressed images, select the `.img.xz` directly; otherwise
   extract it to `.img` first and write that file. Do not use ISO-only creation or file-copy mode.
5. Let writing and the writer's verification finish, then safely eject the media.

### Boot a Raspberry Pi 4 or 5

Insert the written SD card (or use boot media your Pi is already configured to boot), connect
Ethernet and a suitable power supply, then power on. The image is ARM64 and specific to Pi 4/5.
The repository notes that it has not yet been booted on a real Pi, so the published image is not a
claim of complete hardware validation.

### Boot an x86-64 UEFI mini PC

Connect the written USB or disk and wired Ethernet. In the machine's firmware settings, use
**UEFI boot** and turn **Secure Boot off**. Open its boot menu (the key depends on the manufacturer)
and select the UEFI entry for the written drive.

**Yes, you can write the image to a USB and boot from it.** ZerOS then runs from that USB, which
holds the system and its data. It does not copy itself onto the internal SSD. For a permanent
installation on an SSD, write the image directly to that target SSD with an image writer, then
boot it; back it up first and do not overwrite a disk that is running your current OS. Alternatively,
install a supported Linux distribution on the internal disk and use `install.sh`.

The changelog records an x86-64 image boot in QEMU; it does not provide a general VM installation
procedure or guarantee compatibility with every PC.

### First boot and claiming the server

1. Boot on a **trusted local network**. The first boot takes a few minutes and needs no internet
   connection. The desktop is a web interface, opened from another device's browser.
2. From a computer on that network, open `http://zeros.local`. If the name does not resolve, find
   the server's IP address in your router's device list and open `http://<server-ip>`.
3. **Within 30 minutes of boot**, create the administrator account, with no setup token.
   If the window expires before anyone claims it, restart the device: each boot opens another
   30-minute window until it has been claimed.
4. The account you create is also the device's **SSH login**:
   `ssh <username>@zeros.local`, with the same password. Changing your ZerOS password later
   does not change the SSH password; use `passwd` over SSH for that.
5. After sign-in, check Settings → About and Task Manager. Internet access is needed later for
   installing apps, updates and remote access; it is not needed for this image's initial boot.

> [!WARNING]
> Anyone on your network during those 30 minutes could claim the device before you. Do the first
> boot on a network you trust, not an open guest Wi-Fi.

For the **script installation**, account creation uses the printed setup token instead of this
image claim window, and the guide does not promise that the web account creates a host SSH login.
See [Checking ZerOS](#checking-zeros) for services, version, health and logs after either path.

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
