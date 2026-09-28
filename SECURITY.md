# Security

## Reporting a vulnerability

Report privately through GitHub's [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing/privately-reporting-a-security-vulnerability):
open a draft security advisory on this repository rather than a public issue. Do not report
security problems through a public issue, pull request or discussion.

## Supported versions

Only the first release (`1.0.0` and its patch releases) is supported. There is no long-term
support branch yet.

## No security review has been done

The backend has not had a security review as of this writing. The points below describe what the
design intends to protect against, not what has been independently verified.

## What ZerOS protects against

- **Session hijacking and CSRF:** session cookies are HttpOnly and SameSite=Strict (and Secure over
  HTTPS); every unsafe, authenticated request also needs the session's CSRF token in
  `X-CSRF-Token`, checked server-side.
- **Unauthorized actions:** every state-changing operation declares a required capability (power,
  updates, file access, app installation, remote access, and so on) and is checked against the
  signed-in user's role on the server for every request. Hiding a control in the UI is never the
  enforcement.
- **Secrets at rest:** notification-channel passwords/tokens and backup repository passwords are
  sealed with AES-256-GCM before they reach the database, using a key file that only the API's
  process can read.
- **Apps seeing your ZerOS session:** apps are reached through the ZerOS app gateway, which can
  require a ZerOS sign-in before letting a request through and never forwards the ZerOS session
  cookie to the app itself.
- **A compromised release reaching your server:** `zeros setup`, `zeros update` and the flashable
  images all verify the release manifest's signature against the public key built into `zeros`
  before changing anything, and `zeros update` refuses to downgrade.
- **A broken update leaving the server down:** app and ZerOS updates back up first and roll back
  automatically if the post-update health check fails.

## What ZerOS does not protect against

- **Plain HTTP on the local network.** `http://zeros.local` and the LAN IP address are served over
  plain HTTP, by design — this is a deliberate decision for the local network, not an oversight.
  Anyone who can already see your LAN traffic can see that traffic. Remote access is meant to go
  through Tailscale instead, which gives it its own encrypted, authenticated connection
  (`https://zeros.<tailnet>.ts.net`).
- **A compromised host.** The host agent is the one process that runs as root and can be told to
  reconfigure Docker, mount drives, install packages and run backups. Nothing in ZerOS defends
  against an attacker who already has root, or physical access to the machine.
- **A malicious app you chose to install.** ZerOS reviews store and custom apps for risky
  configuration (privileged mode, host networking, sensitive mounts) before install and asks for
  explicit confirmation, but it does not sandbox or audit what an app does once you have accepted
  that risk.
- **The first 30 minutes after flashing an image.** A flashed device can be claimed by the first
  account created from the local network within 30 minutes of boot, with no setup token. Anyone
  else on that network during that window could claim it first — do the first boot on a network you
  trust.
- **Anything the underlying OS, Docker or a third-party app is separately vulnerable to.** ZerOS
  installs and updates its own dependencies (Docker, Tailscale, the NVIDIA container toolkit) from
  their own signed repositories, but does not patch or audit them beyond keeping them current.
