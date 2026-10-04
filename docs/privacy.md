# Privacy: what leaves your server

> [Home](../README.md) · [Why ZerOS](why-zeros.md) · [Features](features.md) ·
> [Why you can trust it](trust.md) · [Install](installation.md) · [FAQ](faq.md)

Your files, photos, passwords and settings stay on your server. ZerOS has **no telemetry, no
analytics, no crash reports, no advertising and no account with us**. Nothing about you, your files
or your apps is ever sent to the people who make ZerOS.

The desktop itself loads nothing from the internet either: its fonts, icons and wallpapers ship
with ZerOS, and pages such as Settings → About are read only from your server.

## Every connection ZerOS makes, and why

A server does need the internet for a few things: to find updates, to download the apps you choose,
and to reach you away from home. This is the complete list.

| When | Where to | What for |
| --- | --- | --- |
| Once a day, and when you press *Check for updates* | GitHub (`AmerAlreyahi/ZerOS-releases`) | Is there a newer, signed ZerOS release? |
| Once a day, and when you press *Check for updates* | Your system's package mirrors | Which OS packages have updates |
| Once a day, and when you press *Check for updates* | The registries your apps' images come from (such as Docker Hub) | Has an app's image changed? Only the image's fingerprint is compared; nothing is downloaded |
| When you open the Store or sync it | GitHub (`AmerAlreyahi/ZerOStore`) | The app catalog and its pictures |
| When you open the Store's Docker Hub tab | Docker Hub's public API | The list of Docker Official Images (kept an hour) |
| When you install or update an app | That app's image registry | Download the app |
| When you update ZerOS, and when you install it | GitHub and GitHub's container registry | Download the signed release |
| When you install ZerOS, and when you turn on Remote Access | Docker's, NVIDIA's and Tailscale's own package repositories | Install those components from their makers, signed by them |
| While Remote Access is on | Tailscale | Connect your devices and issue your `*.ts.net` certificate |
| When an alert fires, if you set up a channel | The email server, ntfy, Gotify or Telegram you chose | Send you the alert |

The daily check can be turned off in **Settings → Maintenance**; you can then check by hand when you
like.

## In your browser

- **Web search** in the search box opens the search engine you chose, in a new tab, only when you
  pick the web result.
- **Websites you add** to the desktop show the site's own icon, which your browser loads from that
  site. ZerOS never fetches it.
- **Apps you open** are the apps themselves; what they do with the internet is up to each app. The
  Store's review shows what an app may access on your server before you install it.

## What ZerOS keeps, and where

- **On your server only:** your account, sessions, preferences, window layout, unsaved editor text,
  notifications, the audit log, monitoring history and backup records.
- **Passwords** are stored only as Argon2id hashes. Backup repository passwords and alert channel
  secrets are readable only by ZerOS, never written to a log, and never shown again once saved.
- **Wi-Fi passwords** are kept only by your system's network manager, never by ZerOS.
- **Logs** never contain passwords, cookies, request bodies or secrets.

## Remote access

Remote access is off until you turn it on. It uses [Tailscale](https://tailscale.com/), whose
[privacy policy](https://tailscale.com/privacy-policy) covers the coordination of your devices; the
traffic between your devices is encrypted end to end and does not pass through ZerOS's makers.
