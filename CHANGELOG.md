# Changelog

## [1.1.9] — 2026-10-15

- App details show installed Compose YAML in a separate read-only viewer, with administrator permissions and audited access. Editing and redeployment remain separate future work.
- App menus share the All Apps actions across desktop pins, Server, Task Manager and Custom apps. Setup details open separately with username, password and additional information fields, and an explicit empty state.
- App details put the icon and name in the header, group configuration actions together and align related sections. Publisher setup notes render readable Markdown across app details and Store install views.
- About presents ZerOS, the creator’s current public GitHub profile and useful guides, with version and licence in the sidebar. The layout adapts to the app window. Opening About contacts GitHub; release information and guides remain available if GitHub is unavailable.
- The default browser favicon uses a transparent 64px image, reducing its download from about 1 MB to 6.8 KB while keeping custom favicons.
- Returning to the current wallpaper while another image loads keeps one ready layer, preventing a duplicate flash and ignoring the cancelled image's late response.
- Disk-image builds use a faster compression preset. Local full-image measurements reduced compression time by 51–55%, with Pi downloads about 9% larger and x86 downloads about 5% larger; hosted build savings remain unmeasured.

## [1.1.8] — 2026-10-14

### Fixes

- Terminal text selection follows the pointer at every interface size, including after moving, resizing or changing scale, without reconnecting the session.
- Pinned app tiles have quieter borders on dark wallpapers. Files and Task Manager icons follow the selected theme accent regardless of their position.
- Player's edges and corners resize above video controls. Loaded portrait, landscape and square video get a bounded initial window shape and remain fully visible; manual sizing and fullscreen survive, and narrow controls wrap.
- App installation reports ownership failures without making app-data folders world-writable. Existing data permissions are preserved.

### Enhancements

- Task Manager history offers measured summaries and an expandable time/value table, with unavailable values and missing intervals shown explicitly.

### Maintenance

- Frontend pull requests check generated API types, typecheck, lint, production build and representative mocked browser flows. Release builds cache container layers and verified upstream installer downloads without removing final artifact gates.
- A compatible js-yaml security patch updates development tooling; two other development dependency advisories remain documented.

## [1.1.7] — 2026-10-14

### Fixes

- Terminal's Ctrl+Shift+C copies selected text without triggering the browser shortcut. Plain Ctrl+C remains the shell interrupt; blocked clipboard operations appear in the status bar.
- Publisher app icons keep their transparent artwork across Store, Applications and Task Manager. Only missing or failed icons get a tinted fallback tile; uploaded server images still fill the tile.
- Desktop CPU, memory and health widgets open Task Manager's Overview; Essential's Storage widget opens Storage, regardless of the last page visited.

### Enhancements

- Optional Terminal Mouse mode copies selections and pastes on right-click, preserving the shell's bracketed paste behavior. It starts off and remembers the choice on this device.
- Store spotlight starts on a random app, avoids repeating the last banner and keeps the same app through catalog reordering. Playback pauses and manual controls retain their behavior.

## [1.1.6] — 2026-10-13

- System apps load when opened, with shared loading and recovery states that keep window controls usable. Dock hover/focus warms the intended app; remembered pages and minimized app state survive. The complete initial JavaScript gzip graph is about 31% smaller; lab timing results vary by viewport.
- Unhandled file drops are refused before React renders, including signed-out and connecting screens once the entry module executes. Files keeps its existing accepted-drop behavior.
- Browser appearance migration checks retry when preference/media refresh cancels pending local reads. Accepted/declined choices remain authoritative; migration stays explicit and preserves local data until confirmed transfer succeeds.
- A Text Editor that finishes loading in the background keeps the user's chosen foreground window instead of taking focus.
- Browser regressions retain their original payload, security, destination and geometry assertions with deterministic migration fixtures and readiness checks.

## [1.1.5] — 2026-10-13

- Current host metrics retire immediately on agent loss and expire after the freshness interval, while measured history keeps real gaps. Reconnection refreshes the snapshot and late responses cannot replace newer live readings.
- Shared header controls fit narrow phones and tablets; Essential pins and health remain reachable on short canvases through internal scrolling. Exposed desktop background opens the shared menu while covered widgets remain inert. Settled dock-gutter checks retain their exact assertions.
- Minimized and phone-covered system apps pause disposable polling and Store rotation. Covered desktop subscriptions, clocks and decorative animation pause; restoration reads current truth. Durable jobs, live events and terminal connections remain active.
- ZerOS updates check host package prerequisites before replacing files. Incomplete package operations refuse with recovery guidance while the current version keeps running; host repair is never performed implicitly. Setup preserves configured package holds and identifies incomplete required packages before apt changes.

## [1.1.4] — 2026-10-13

- Arcane receives a separate stable encryption key. Recipe updates preserve installed keys and deployment mode; file-backed or conflicting keys refuse replacement safely.
- Custom Compose and uploaded apps accept explicit masked variables through review/install. Missing required substitutions refuse deployment; optional/defaulted expressions retain Compose semantics. Values remain private and are bound to the reviewed project.
- Declared or explicitly selected IPv4/IPv6 loopback web interfaces remain private, retain their port bindings, and show access guidance without a browser launch link. Database ports are never guessed to be HTTP.
- Coordinated ZerOStore fixes preserve Arcane 2.14.0 and its DinD recipe and correct PostgreSQL setup guidance to match fresh defaults, with an initialized-database warning.
- Compatible generator and development dependency patches address the selected advisories; unresolved PM2 transitive advisories remain documented.

## [1.1.3] — 2026-10-12

### Data and account safety

- A failed app install now cleans up containers and networks while preserving named
  volumes, including data kept after an earlier uninstall. Deleting app data remains
  an explicit removal choice. Failed cleanup keeps the project available for repair.
- Notification requests completing after logout or an account change can no longer
  restore the previous account's history, counts or errors. Newer live updates survive
  older snapshots, and dismissal, read actions and pagination reconcile safely.

### Validation

- Alert API tests use an isolated HTTP refusal fixture, with explicit failure-code
  and secret-redaction checks, instead of relying on a public ntfy hostname.
- No database migration, dependency change, encryption-key rotation or port-binding
  change. Existing UI geometry, metrics freshness and bundle work remain on the audit
  roadmap; implementation evidence and validation limits are in docs/audits/.

### Installer ISO

- Releases also build a guided x86-64 UEFI installer ISO beside the existing disk
  images and install script. Debian's installer handles network setup, disk selection
  and explicit partition/erase confirmation; ZerOS's existing first boot creates the
  account. Internet access is needed for host packages.
- The ISO gets its own signed `installer.json` index. Existing `images.json` readers
  and server updates keep their original contracts. Disposable UEFI/USB boot,
  blank-disk installation and installed-system startup gate ISO publication.

## [1.1.2] — 2026-10-04

Files works on folders and selections, and typing is never taken by a window.

### Files

- Upload a whole folder, by dropping it or with **Upload folder**: its folders and files arrive as
  they were. Before, a dropped folder became an empty file with its name. A name already there
  asks first: keep both, skip or cancel.
- **Download anything**: from the right-click menu, the selection bar or the toolbar. A file
  downloads as it is; a folder, or several items at once, download as one `.zip`, made on the
  server as it is sent, of any size. Files ZerOS may not read (an app's own files) are left out and
  listed in the zip.
- Select like on a computer: Ctrl/Cmd-click, Shift-click, Ctrl+A or **Select all**; Esc clears
  the selection. The selection bar, the right-click menu and Delete, F2 and Enter offer only what
  fits what you selected.
- Folders show how many items they hold without being opened, in the folder view and the sidebar.
- The sidebar marks only the place you are in, the path names each place once, and long names wrap
  and are cut in the middle so the end and the extension stay visible, shown whole when you point
  at them.
- Also: read-only drives offer nothing that would write; names sort 2 before 10; your view and sort
  stay as you move between folders; an unreadable folder says why; large folders stay smooth; a
  file dropped where nothing takes it is no longer opened by the browser.

### Windows

- Typing is never taken by an open window. With Photos or Player open, even minimized, keys such
  as `0`, `1`, `r`, `+` or the space bar typed in Text Editor, Terminal, search or a form zoomed,
  rotated or played instead of being typed. A window's shortcuts now act only while it is in front,
  and never on a key typed into a field.

### Licence

- ZerOS is free to install and use, and its source code is not public; Settings → About says so.
  Version 1.1.1 and every version before it stay under the MIT licence.

### Releases

- The flashable Raspberry Pi and x86-64 images go straight onto the release. For 1.1.0 and 1.1.1
  they were held in the build service's storage on the way, which ran out of space, so those two
  releases have none.

## [1.1.1] — 2026-10-04

The desktop's secure address has no port: `https://<server>.<tailnet>.ts.net`.

- The desktop now also answers on port 443, so its secure tailnet link no longer needs `:80`.
  Remote Access shows the shorter link, and signing in to an app over HTTPS returns to it.
- Port 80 works exactly as before, over plain HTTP and HTTPS, so every bookmark keeps working, and
  a sign-in on one port is a sign-in on the other.
- Port 443 is taken only if it is free. If an app (a reverse proxy, for example) or another program
  already uses it, it keeps it, the desktop stays on `:80`, and the server says why. ZerOS looks
  about 30 seconds after it starts, so apps starting at boot get their ports first. Installing an
  app that wants 443 while the desktop holds it is refused as "port in use".

## [1.1.0] — 2026-10-03

Photos, Player, Text Editor and Terminal become real windows on the desktop, kept for your
account, and nothing you type in an editor is lost.

### Windows

- Photos, Player, Text Editor and Terminal open as windows over the desktop. Drag the title bar to
  move one, drag its edges to resize it, drop it on the left or right edge to fill that half, and
  double-click the title bar to maximize it. Several stay open at once; the one you click comes to
  the front.
- Minimize sends a window to the dock, where every open window has its own button. A maximized
  window stops above the dock, which stays visible.
- A window closes only with its ✕. Esc and the browser's Back button never close or move one.
- Your windows are kept for your account: which are open, where, at what size and what each
  shows. A reload, or signing in on another device, brings them back, fitted to that screen.
  Terminal windows are the exception: a shell cannot come back after a reload.
- Up to 12 windows at once. Phones show one window at a time, full screen.
- A system app (Files, Store, Settings…) you open or click comes in front of the windows; clicking
  a window, or its button in the dock, puts it back on top.

### Photos, Player and Text Editor

- Files' previewers are apps of their own now, in All Apps, search and the desktop pins. Files
  opens each file in the right one, "Open with…" lets you choose, and each app has Open… to pick a
  file on the server.
- Text Editor keeps your unsaved text on the server a few seconds after you stop typing, until you
  save or discard it. A reload, or another device, brings it back as unsaved.
- If the file changed on the server since you started editing, saving offers to keep both: your
  text is saved beside it as "name (2)". Nothing is ever overwritten without asking.
- Closing a window with unsaved text asks you to save, discard or cancel.
- A new document can be saved to any folder you choose.
- Drafts are limited to 2 MiB each and 20 (8 MiB in all) per account. Past that, ZerOS asks you to
  save or discard one; it never drops any.
- Full screen in Photos stays on as you step to the next picture.
- Each app opens its next window at the size you last gave it on that device.

### Desktop

- Every desktop style keeps its shape with any number of pins up to 12: apps sit in even rows
  instead of stretching a card or cutting it off.
- Essential shows server health as three cards, CPU, Memory and Storage, beside the clock and
  pins, at the same height and with the same corners.
- On a desktop whose pins were never changed, Photos, Player and Text Editor are pinned too.

### Fixes

- The notifications, Wi-Fi and profile buttons in the header now open on the first press, at
  once; a press could be missed or answered late.
- Dashboard's "Server applications" now lists your installed apps, not only your saved links.
- Choosing a folder (for a backup, or with Save as) no longer dims the screen behind it.

## [1.0.5] — 2026-10-03

Docker Hub in the Store, the desktop over HTTPS on your tailnet, and the other machines on your
tailnet in Remote Access.

### Docker Hub in the Store

- A Docker Hub page in the Store lists every Docker Official Image, with Most popular, Recently
  updated and search. Each image shows what it is, its pulls, its last update and the
  architectures it runs on; one that does not run on this server is shown but cannot be installed.
- Installing one is a custom app: ZerOS writes the Compose file from the image's own settings, the
  usual review shows every risk, and nothing is installed without your confirmation. The port you
  choose as its web page is served behind ZerOS sign-in; its other ports are reachable from the
  server itself only, so a database is never open to your network by default.
- Images that need settings before they start (a database password, for example) will not run
  this way; use a custom app for those. Docker Hub is asked at most once an hour, and when it
  cannot be reached the Store shows the last list or says so.
- Only Docker Official Images: Docker Hub's public API cannot list or search Verified Publishers or
  community images.

### Remote access

- The ZerOS desktop is served over HTTPS on your tailnet, with the same certificate as your apps,
  on its usual port: Remote Access gives its `https://` address while secure addresses are on.
- Opening an app at its `https://` address while signed out goes to the ZerOS sign-in and back,
  instead of showing an error.
- Remote Access lists the other machines on your tailnet, online or when last seen, and updates by
  itself when one comes online, goes offline or is renamed.

### Desktop and Files

- Up to 12 pins on the desktop.
- Home no longer gets an "Apps" folder: nothing ever used it, since apps keep their data in App
  data. An empty one is removed; one with your files in it stays.

### Fixes

- Opening an app at `http://zeros.local:<port>` while signed out could show a "Sign in to ZerOS"
  error instead of the sign-in screen, when the browser reached the server over IPv6.
- The HTTPS setup guide in Remote Access could take up to five minutes to notice a step you had
  finished; it now checks every ten seconds from the moment it is open.
- Turning secure addresses on or off could take up to five minutes to reach an app that needs
  HTTPS, such as Vaultwarden, when it came right after another change; it now applies at once.
- Apps could briefly stop answering over HTTPS once a day, when the daily certificate check was
  slow: the certificate already held is now kept until it really expires.

## [1.0.4] — 2026-10-02

Wi-Fi, a Terminal, and secure (HTTPS) addresses for apps on your tailnet.

### Wi-Fi

- Settings → Network lists the Wi-Fi networks in range and joins, leaves and forgets them, on
  servers where NetworkManager runs the network. Other servers say Wi-Fi is managed outside ZerOS
  instead of pretending.
- Joining a network keeps Ethernet up until the desktop answers over Wi-Fi. If it does not within
  the time limit, ZerOS goes back to the previous connection by itself and says so, so a remote
  server is never cut off.
- Leaving or forgetting the only network the server is on is refused.
- The Wi-Fi password goes only to NetworkManager; ZerOS never stores or logs it.

### Terminal

- Administrators get a real shell on the server, as its login user (never root), after typing
  their password again. Closing the window ends the session; a session also ends after 15 minutes
  without typing. Every session start and end is in the audit log.

### Secure addresses for apps (tailnet only)

- Apps can be opened at `https://<server>.<tailnet>.ts.net:<port>` with Tailscale's certificate,
  which browsers trust. It works only from devices on the same Tailscale account; the home-network
  addresses stay `http://`. Remote Access guides each step (MagicDNS, HTTPS certificates in the
  Tailscale admin page) and moves on by itself.
- A switch turns secure addresses off; every `http://` address keeps working.
- Open uses an app's secure address when it has one. Apps that only work over HTTPS, Vaultwarden
  first, are marked in the Store and told their secure address by ZerOS.

## [1.0.3] — 2026-09-30

Signing in from your own devices over Tailscale now covers ZerOS itself, not only its apps.

### Remote access

- A tailnet login mapped to a ZerOS user now opens the ZerOS desktop without a password, as the
  mapping dialog says. Before, the mapping only let that person into apps that require ZerOS
  sign-in, and the desktop still asked for the password. Open ZerOS at its `http://` tailnet
  address for this; serving the desktop over HTTPS is planned for later.
- Remote Access leads with that address, `http://<server>.<tailnet>.ts.net`, with Copy and Open.
  It used to offer only an `https://` address, where nothing answers. The HTTPS section now says
  it covers apps, each at its own port.

## [1.0.2] — 2026-09-29

Fixes from finishing the field test on Windows (WSL2) with an NVIDIA GPU, where 243 of 270 Store
apps worked on the first try.

### Apps

- Apps whose every hardware device is missing on this server (Jellyfin, Emby and Plex with NVIDIA,
  FileFlows, Stremio and llama.cpp on WSL) can be reviewed and installed again: 1.0.1 refused to
  review them.
- An app with a one-time setup step (AFFiNE, ee-gateway) shows as running once that step has
  finished, instead of degraded, and the step is not run again at every start of the server.
- Linkstack installs: a folder its recipe declares in an unusual way is now created and kept in
  the app's own data.

### Updates

- Updating ZerOS waits until no app is being installed or changed, instead of cutting the install
  off.
- An update whose download the app registry or Docker briefly refuses is tried again before it is
  rolled back.

## [1.0.1] — 2026-09-29

Fixes from the first field test, on Windows (WSL2) with an NVIDIA GPU.

### Security

- App data is no longer readable by other users on the server. Apps keep keys and databases
  there (Vaultwarden's signing key, for one), often readable by everyone because of the app's own
  settings; the folder above them is now open to ZerOS only. Updating fixes existing servers.

### Apps

- Apps start again after the server restarts, unless you stopped them yourself. Many Store apps
  stayed down after a reboot before.
- Apps that ask for hardware the server does not have (an Intel GPU, a Raspberry Pi's video chip,
  USB audio) now install without it instead of failing. Jellyfin, Emby and Plex with NVIDIA now
  install on machines such as Windows with WSL.
- A failed install removes the data folder it created, and removing an app removes its downloaded
  images unless another app still uses them.
- A download the app registry briefly refuses is tried again before the install fails.
- An app on the server's own network that uses port 8080 (openHAB, for one) can no longer stop
  ZerOS from starting: ZerOS itself no longer uses that port.
- The app-failure alert no longer calls an app that stopped "running again".

### Server and desktop

- ZerOS installs and runs on Windows through WSL2 (Debian or Ubuntu), including NVIDIA GPUs.
- Backups run at the time you set in the server's time zone, not in UTC.
- Task Manager shows the process count on every page, the GPU's power draw, and a CPU figure that
  no longer counts virtual machines twice.
- Remote access over HTTPS works within seconds of a restart, not after ten minutes.
- Removing an app says that its images are removed too.

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
- Custom apps from an uploaded Compose file or a project archive the server builds
  (`POST /apps/sources`).
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
  same way once installed from a release (`zeros update`, `POST /updates/zeros`).

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
