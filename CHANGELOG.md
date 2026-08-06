# Changelog

All notable changes to Vigil are documented here. Every GitHub release's
notes are the relevant slice of this file, so any release tag shows both
what's new in that version and everything that came before it.

## v0.3.2 (Linux) — 2026-08-06

### Changed
- **Classification labels renamed** and a **header account switcher +
  refresh control** added (from the Windows source update).
- **iCloud alias tag now always shown** on an email card when it differs
  from the account address, not just when a section has 2+ distinct
  aliases — makes single-off-address mail visible too, not only when
  there's a full group to split.
- **Alias matching is now case-insensitive** — the same address arriving
  in different casings (a real, confirmed case) no longer shows up as two
  separate groups.
- **Transparent window backgrounds** on both the popup and results
  windows, fixing visible white corners around the rounded window shape.

### Fixed
- Re-verified and re-applied the `--no-sandbox` AppImage fix (originally
  introduced in v0.2.0, see below) after it briefly regressed back to a
  launcher-only workaround during this build's assembly — confirmed
  baked into `AppRun` again, works regardless of how the AppImage is
  launched.
- Excluded a leftover dev-only `PATCH-NOTES.md` from the shipped app
  bundle.

## v0.3.1 (Linux) — 2026-08-05

### Changed
- **New build synced from the Windows source.** The single-instance lock
  and iCloud alias-grouping fixes from v0.2.1 (which were only ever a
  Linux-side patch on top of the built app) are now part of the real app
  source itself. Alias grouping was also extended further: an alias tag
  now shows directly on individual email cards (not just the section
  sub-headers) whenever it differs from the account tag.

### Fixed
- **`--no-sandbox` re-baked into this build's AppImage launcher script.**
  This is a Linux-packaging-only fix (Chromium's sandbox can't work
  inside an AppImage's own mount regardless of the app itself) that has
  to be reapplied to every new AppImage build until it's handled at the
  build-tool level — it isn't something that lives in the app source, so
  a fresh build from Windows doesn't carry it over automatically.

## v0.2.1 (Linux) — 2026-08-05

### Added
- **iCloud multi-alias grouping.** One iCloud/Apple ID login can receive
  mail addressed to several different addresses (aliases, custom-domain
  addresses, "Hide My Email"). The brief now groups emails by which
  address actually received them whenever an account has 2+ in use —
  Urgent/Needs Reply/FYI stay the main structure, with a light sub-label
  underneath when there's something to distinguish. Single-alias and
  non-iCloud accounts are unaffected.

### Fixed
- **Removed personal data accidentally bundled into the app itself.** The
  shipped `app.asar` contained a leftover `config/` folder with real
  email/invoice history and a stale duplicate `win-unpacked/` build — both
  build artifacts that should never have been packaged, never actually
  read by the running app (confirmed dead weight), but present in the
  distributed file. Excluded from this build.

## v0.2.0 (Linux) — 2026-08-05

### Fixed
- **Single-instance lock.** Previously nothing stopped two copies of Vigil
  running at once (e.g. an old build + a new one, or a second launch
  attempt) — both would independently poll mail and race to write the
  same config data. A second launch now just refocuses the existing
  window.
- **Removed a stale duplicate 0.1.0-era build** (`linux-unpacked/`) that
  had been bundled inside `app.asar` alongside the real 0.2.0 code.
- **Sandbox flag baked into the AppImage itself** (`--no-sandbox`, applied
  in `AppRun`) — Chromium's SUID sandbox can't function inside an
  AppImage's own `nosuid` FUSE mount regardless of file permissions, so
  the app previously aborted on launch unless a custom `.desktop` launcher
  supplied the flag manually. Now works out of the box for anyone running
  the AppImage directly.
