# Why you can trust ZerOS

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Features](features.md) ·
> [Privacy](privacy.md) · [Install](installation.md) · [FAQ](faq.md)

A home server holds the most personal things you own. ZerOS is designed on one assumption: **any
part of it that faces the network could one day have a bug**, so no single bug should be able to
take over your machine, your apps or your files. This page explains how, and, just as plainly, what
ZerOS does not do yet.

## Three parts, each with only the power it needs

```
  Your browser
       │
       ▼
  ┌───────────────────┐   unprivileged, its own container, host network,
  │  ZerOS gateway    │   no Docker access, no database
  └───────────────────┘
       │ desktop               │ your apps (behind ZerOS sign-in)
       ▼                       ▼
  ┌───────────────────┐   unprivileged, read-only container,
  │  ZerOS API        │   every Linux capability dropped, no Docker socket
  └───────────────────┘
       │ a fixed list of typed requests, over a local socket
       ▼
  ┌───────────────────┐   the only part that is root:
  │  ZerOS host agent │   runs fixed programs with fixed arguments, never a shell
  └───────────────────┘
```

- **The API**, which everything you click talks to, runs as an ordinary user in a read-only
  container with every Linux capability dropped and `no-new-privileges`. It has **no access to
  Docker** and cannot run programs on the host.
- **The host agent** is the only part that runs as root. It does not accept commands: it accepts a
  fixed list of operations ("start this app", "mount this confirmed disk", "list updates"), each a
  typed, size-limited request that it validates again itself. It runs programs from a fixed list of
  paths with fixed argument lists, **never through a shell**, so there is nothing to inject into.
  Only root and the ZerOS API may talk to it, which it checks on every connection, and every call is
  recorded.
- **The gateway**, which answers on your network, runs unprivileged in its own container. Its one
  extra permission is opening ports below 1024. It has no database and no Docker access.

So a bug in the web-facing code can, at worst, ask the agent for one of its fixed operations — and
the agent applies its own checks to every one.

## Your apps cannot see your ZerOS session

Every app's web port is published only on the server itself (`127.0.0.1`), and the ZerOS gateway is
the only way in from your network.

- **Sign-in in front of every app**, on the LAN too: an app that requires ZerOS sign-in cannot be
  opened without a valid ZerOS session, whatever address is used.
- **Your session never reaches an app.** The gateway removes ZerOS's session cookies and any
  identity headers a client sent before forwarding a request, so a malicious or compromised app
  cannot steal your session or pretend to be you.
- **Access is checked again while you use it.** Open connections are re-checked every 10 seconds,
  so signing out, or losing access to an app, ends what is already open.

## Apps are reviewed before they run

Every app, from the Store, Docker Hub or your own Compose file, goes through the same review on the
server before anything is installed.

- **Never allowed:** mounting ZerOS's own folders or its control socket (or anything above them,
  such as `/`), reading files from the server through the Compose file, and secrets pulled from the
  host.
- **Allowed only with your explicit yes, recorded in the audit log:** privileged containers, host
  networking or process namespace, added capabilities, device access, and mounts of system folders,
  home directories or the Docker socket.
- **Every app gets** `no-new-privileges` and a restart policy, and its data in its own folder.
- **The review reads only the file you gave it**: nothing it names on the server is opened, and no
  server setting leaks into it.

## Your files stay inside their folders

- ZerOS shows three kinds of place: your **Home**, **drives** you attach, and **app data**. Every
  file operation is confined to them using the operating system's own confinement (Go's `os.Root`),
  which walks every path one folder at a time and cannot be led out by a link, a `..`, or a folder
  swapped for a link at the wrong moment.
- Drives ZerOS mounts are mounted `nosuid,nodev`.
- Formatting a disk needs your explicit confirmation and a fresh check, just before it starts, that
  it is still the same disk.
  Nothing is mounted, formatted or moved automatically.
- Pictures you upload as wallpapers or avatars are re-encoded, which strips their hidden metadata
  (camera, location). They are served only to you; the one exception is your avatar on the sign-in
  screen of a single-account server.

## Your account

- **A one-time setup token**, printed by the installer, is the only way to create the first
  account, so nobody else on your network can claim a fresh server.
- **Passwords** are hashed with Argon2id, the current recommendation for password storage.
- **Sessions** use `HttpOnly`, `SameSite=Strict` cookies, `Secure` and `__Host-` over HTTPS; every
  change needs a CSRF token; and repeated wrong passwords slow sign-in down.
- **Sensitive actions ask for your password again**, such as opening a Terminal.
- **Permissions are checked by the server on every request**, not just hidden in the interface.
- **An audit log** records app installs and removals, updates, power actions, disk operations,
  deleted files, approved risks and every host operation.
- **Secrets are never written to logs.** Alert channel passwords and tokens are write-only: once
  saved, ZerOS never shows or returns them again.

## Updates you can trust

- **Every release is signed** with a key that exists only in ZerOS's release pipeline. The installer
  and every server check the signature before installing anything, and refuse a release that does
  not match the key built into ZerOS. A release made any other way is refused.
- **ZerOS rolls itself back.** It keeps a copy of everything it replaces, and if the new version
  does not come up healthy, it puts the old one back by itself.
- **Apps roll back too.** Before an app update, ZerOS records exactly which image versions were
  running; if the updated app fails its health check, it returns to them.
- **System updates are conservative.** OS packages are upgraded with a fixed, safe command (never a
  distribution upgrade, never removing packages), and ZerOS never reboots your server on its own.

## Away from home, without opening your router

Remote access uses [Tailscale](https://tailscale.com/), a private network between your own devices,
encrypted end to end. ZerOS opens no port on your router and puts nothing on the public internet. On
your tailnet, the desktop and your apps get `https://` addresses with real certificates for your
machine's own `*.ts.net` name. ZerOS never uses a certificate of its own making, and never exposes
your server publicly through Tailscale Funnel.

## Honest about its limits

Trust also means saying what is not there yet:

- **No two-factor sign-in yet.** It is planned; until then, use a strong password and keep remote
  access on Tailscale.
- **On your home network the desktop uses plain `http://`.** HTTPS is available on your tailnet,
  where certificates can be issued for your machine's name.
- **Only an app's web port is behind ZerOS sign-in.** If an app publishes other ports (a game
  server, a database), ZerOS lists them as not protected, so you can see them.
- **Apps keep the Linux capabilities their images need**, because most store images do not work
  without them; the review shows anything beyond the defaults.
- **Backups go to drives and folders on the server**, not yet to the cloud, and do not yet include
  ZerOS's own settings database.
- **ZerOS's source code is not public.** Its design is documented here, its releases are signed, and
  security reports are welcome: see [SECURITY.md](../SECURITY.md).

If you find a security problem, please report it privately as [SECURITY.md](../SECURITY.md) explains.
