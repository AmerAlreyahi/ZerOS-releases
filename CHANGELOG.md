# Changelog

## [1.0.0] — 2026-09-28

The first release. Everything below is implemented and wired end to end (frontend and backend)
unless noted otherwise.

### Desktop

- A responsive web desktop: persistent header and navigation, one app surface at a time, Ctrl+K/
  Cmd+K search, configurable navigation position and auto-hide, and phone/tablet layouts down to
  narrow screens.
- Twelve desktop styles (Essential, Orbit, Horizon, Eclipse, Constellation, Monolith, Pulse,
  Dashboard, Nominal, Nominal Tiles, Terminal, Clear), a health ring driven by live server state,
  and per-user pins, wallpapers and theme.
- Ten themes (Midnight, Daylight, Solstice, Sandstone, Nebula, Citrine, Tidal, Clover, Crimson,
  Orchid), interface scaling, glass intensity, reduced-motion and reduced-transparency support.
- Sign-in is the server's own: first-run setup with the installer's setup token (or, on a flashed
  image, the LAN claim window — see Installation), session restoration, sign-out, and sign-in
  redirects from the app gateway.
- Settings: Account, Alerts, Maintenance (Updates and Backups), Appearance, Wallpaper, Styles and
  About, all server-backed.
- One exception, stated in the UI: the Dashboard style's scratchpad note is saved in the browser
  only, not on the server.

### Apps and Store

- ZerOStore: a single catalog (`AmerAlreyahi/ZerOStore`) built from the CasaOS App Store and Umbrel's
  app store, with compatibility notes and generated per-app passwords. Browse, review a Compose
  change, install, and manage installed apps' lifecycle (start, stop, restart, logs, remove).
- Custom apps from an uploaded Compose file or a project archive the server builds.
- The app gateway: reach installed apps through ZerOS with an optional per-app ZerOS sign-in
  requirement. The gateway never forwards the ZerOS session cookie to an app, and Tailscale device
  identity lets a mapped tailnet user in without a redirect.
- Risky app configuration (privileged mode, host networking, sensitive mounts, GPU/device access)
  is flagged before install and needs explicit confirmation.
- GPU support: every NVIDIA, AMD and Intel GPU and the Raspberry Pi's VideoCore is reported in Task
  Manager; apps that request a GPU name which one they would get, or that the server has none.
- Task Manager (formerly Live Statistics / Statistics): installed apps, host metrics, processes and
  storage from the server, with reload/kill/force-kill for host processes by PID and a search that
  reaches past the busiest 200.

### Files

- Server-backed Files: the confined roots Home, secondary drives and app data, with folders,
  uploads (resumable), downloads, previews, rename, trash/restore and starred locations.
- Default Home folders (`Pictures`, `Videos`, `Music`, `Downloads`, `Apps`) created on first use.
- Server-wide file search from the Files search box and the header search.
- Preferences, pins, uploaded wallpapers and the account avatar are stored per user on the server.

### Monitoring

- Live host metrics every 5 seconds (CPU, memory, per-volume storage, network, uptime, every GPU),
  with unmeasured figures reported as unavailable, never as zero.
- History (`hour`/`day`/`week`/`month`) from per-minute and per-hour summaries, not raw samples.
- Per-app resource usage, and a server health score driving the desktop's health ring.

### Backups and updates

- Backups (restic): plans back up app data, app config or a Files folder to a drive or folder on
  the server, on a schedule, with configurable retention. Run, verify and restore on demand; edit a
  plan in place or pause its schedule.
- Updates: OS package updates and per-app updates from Settings → Maintenance, both with a
  backup-before, health-check-after, automatic-rollback-on-failure policy. ZerOS itself updates the
  same way once installed from a release (`sudo zeros update`).

### Remote access

- Tailscale, installed natively (not in Docker) so it keeps working when Docker doesn't: one-click
  install and enable from the UI, `https://zeros.<tailnet>.ts.net` for the desktop and for apps.
  ZerOS never removes the client or logs an account out.
- Plain HTTP on the local network (`http://zeros.local`) is a deliberate decision, not an
  oversight — remote access is meant to go through Tailscale instead.

### Installation and images

- One-command install: `curl -fsSL <release>/install.sh | sudo sh` downloads, checksums and runs
  `zeros setup`, which verifies the release's signature before changing anything on the host.
  Debian 12/13, Ubuntu 22.04/24.04 LTS or Raspberry Pi OS (64-bit), on x86-64 or ARM64.
- `zeros setup` is idempotent; `zeros repair` puts back anything it placed; `zeros uninstall`
  (with `--purge` to also remove data) reverses an install.
- Signed release channel: GitHub Releases plus GHCR, an ed25519-signed manifest, and
  `THIRD_PARTY_LICENSES.txt` shipped with every release.
- Flashable images for Raspberry Pi 4/5 and x86-64 UEFI mini PCs (the x86-64 image includes
  NVIDIA's driver), claimed by the first account created from the local network within 30 minutes
  of boot — no setup token needed. That account is also the server's SSH login.

### Tested and not yet tested

- Installs were tested on Ubuntu 24.04 and Debian 12, x86-64, in LXD system containers.
- The x86-64 image was booted only in QEMU; the Raspberry Pi image was built but never booted.
- The distribution install matrix (CI across every supported system) is not built yet.
- No real hardware on a real network has been tested.
