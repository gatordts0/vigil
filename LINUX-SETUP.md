# Vigil — Linux Setup

Vigil is a local-first Gmail/iCloud/Yahoo triage assistant. It reads your
inbox, sorts mail into Urgent / Needs Reply / FYI using a locally-run AI
model (via Ollama), flags invoices, and supports standing "watches" and
local file search. Nothing about your email leaves your machine — there's
no cloud AI service involved.

## Before you install: Ollama

Vigil needs Ollama (a local AI model runner) installed and at least one
model pulled. Vigil will try to start Ollama itself if it's installed but
not already running — it can't install Ollama or pull a model for you.

1. Install Ollama:
   ```
   curl -fsSL https://ollama.com/install.sh | sh
   ```
   (or see https://ollama.com/download for other install methods)

2. Pull Vigil's default model:
   ```
   ollama pull qwen2.5:7b
   ```
   A smaller/faster model works too if your machine is limited, but
   qwen2.5:7b is what Vigil expects by default.

3. Confirm it's running:
   ```
   ollama list
   ```
   Should show the model you just pulled. Ollama serves a local API at
   `http://localhost:11434` — no account, no API key.

## Download Vigil

Latest Linux release: **v0.7.0**, on the Releases page —
https://github.com/gatordts0/vigil/releases/tag/v0.7.0

(Linux and Windows ship on separate tags and aren't always released
together — the newest tag overall may be Windows-only. Check the
Releases page for anything newer tagged "(Linux)" before installing.)

Two files ship with every Linux release — **use the `.deb`, not the
AppImage, if the option exists** (see why below):

- `vigil_0.7.0_amd64.deb` — **recommended**
- `Vigil-0.7.0.AppImage` — portable fallback, real limitation below

### Option A — `.deb` (recommended)

This is the one to use. As of v0.7.0, installing this way is what
actually lets Chromium's real sandbox run.

```
sudo apt install ./vigil_0.7.0_amd64.deb
```

Launch from your application menu, or from a terminal: `vigil`

Uninstall later: `sudo apt remove vigil`

### Option B — AppImage (portable, but no real sandbox)

**Why the `.deb` is preferred:** the AppImage format itself can't ship a
setuid-root `chrome-sandbox` binary (it mounts as your own user, not
root), so it structurally cannot use Chromium's real sandbox no matter
what version of Vigil you run — that's an AppImage limitation, not a bug
in Vigil's code. Any AppImage build of Vigil needs `--no-sandbox` to
launch at all. On Ubuntu 24.04+ specifically, this isn't optional: those
releases set `kernel.apparmor_restrict_unprivileged_userns=1`, and
without `--no-sandbox` the AppImage aborts on launch with:

```
FATAL:setuid_sandbox_host.cc(158) The SUID sandbox helper binary was
found, but is not configured correctly. Rather than run without
sandboxing I'm aborting now.
```

If you still want the AppImage (no `apt`/no root, or just prefer
portable):

```
chmod +x Vigil-0.7.0.AppImage
./Vigil-0.7.0.AppImage --no-sandbox
```

If it won't launch at all even with that flag, your distro may need
`libfuse2` (AppImages require FUSE to mount themselves):
```
sudo apt install libfuse2
```

## First launch

A chat popup walks you through setup:

1. Checks whether Ollama is installed and running (tries to start it if
   installed but idle).
2. Asks which email provider to connect — Gmail, iCloud, or Yahoo — and
   walks you through generating an app password for that account.
3. Once connected, Vigil builds your first daily brief.

A second launch attempt just refocuses the existing window rather than
opening twice — that's expected, not a bug.

## Where Vigil stores its data

`~/.config/Vigil/` — standard Electron `userData` location for an app
built with `productName: "Vigil"`. Check there first if you need to find
or reset saved accounts/settings; the app's own Settings window is the
more reliable way to check or change them day-to-day.

## If something goes wrong

- **AppImage crashes with a `setuid_sandbox_host.cc` / SUID sandbox
  error:** this is the Ubuntu 24.04+ issue above — use `--no-sandbox`,
  or better, switch to the `.deb` package, which doesn't need it.
- **AppImage won't launch / silently does nothing:** try installing
  `libfuse2` (above).
- **Can't connect an email account:** see the troubleshooting section at
  the end of `Vigil-Gmail-Setup-Guide.md`.
- **Vigil can't reach the AI model:** confirm Ollama is actually running
  (`ollama list`) and the model from "Before you install" is pulled.
- **Two windows / duplicate tray icons:** close both and relaunch — only
  one instance is meant to run at a time.

For anything not covered here, check
[CHANGELOG.md](https://github.com/gatordts0/vigil/blob/main/CHANGELOG.md)
in this repo for known issues in your specific version.
