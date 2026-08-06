# Changelog

All notable changes to Vigil are documented here. Every GitHub release's
notes are the relevant slice of this file, so any release tag shows both
what's new in that version and everything that came before it.

## v0.4.0 (Linux) — 2026-08-06

### Added
- **Fetch window setting** — a new "Fetch window" pill selector in the
  settings popover, below the merged-inbox checkbox: 24 hours (default,
  same as always), 3 days, or 7 days. Picking one refetches immediately.
  Heads up: switching to a wider window for the first time takes a bit
  longer that one time — every email newly in range has to be classified
  locally before it can show up. After that first run, it's back to
  normal speed since already-classified mail is never reprocessed.

## v0.3.10 (Linux) — 2026-08-06

### Added
- **Greeting added to the results window** — "Good morning/afternoon/
  evening, {name} 👋" plus today's date now sits above the search bar,
  time-of-day aware (not hardcoded to "morning").
- **Search moved above the filter tabs** — was tabs-then-search, now
  search-then-tabs.

## v0.3.9 (Linux) — 2026-08-06

### Added
- **Merged Inbox setting**, requested directly by David: a new gear icon
  next to the version number opens a settings panel with one checkbox —
  "Show all accounts in Inbox." Off by default. Turn it on and every
  configured account's mail shows up together, grouped by account within
  Act Now / Reply Needed / No Action, the same restrained pattern already
  used for Invoices and iCloud aliases — no more switching accounts
  through the header dropdown just to see everything.

## v0.3.8 (Linux) — 2026-08-06

### Added
- **Version number now shown in the results window**, pinned to the
  bottom of the left sidebar — always visible at a glance which build
  you're on.

## v0.3.7 (Linux) — 2026-08-06

### Fixed
- **Promotional invoices, for real this time.** David got another
  Domino's promo flagged as an invoice — this one visibly broken too
  ("$0.00", "~NaN% lower than average"). The earlier fix's guard let a
  $0 detection slip through, which then poisoned that vendor's own
  rolling average to $0 too, causing the NaN. A $0 "invoice" is now
  always treated as not a real one, and the average-comparison math can
  no longer divide by zero even if a bad record ever gets in some other
  way.

### Added
- **"Ask about this brief" can now actually fix a wrong invoice**, not
  just explain it. Say something like *"that Domino's thing isn't a real
  invoice"* and Vigil removes it and remembers to ignore that sender
  going forward — in both the popup's assistant and the results window's
  own chat panel, where the fix now shows up immediately without a
  manual refresh.

## v0.3.6 (Linux) — 2026-08-06

### Fixed
- **Promotional emails no longer get flagged as invoices.** David
  reported a food-delivery chain's marketing emails ("Large pizza only
  $9.99") getting detected as real invoices. Fixed with a broader
  marketing-keyword list, an explicit instruction to the classifier that
  advertised prices aren't real charges, and a new deterministic backstop
  that double-checks every invoice detection against known marketing
  language before it's shown.
- Same marketing-language safety net that already downgraded
  wrongly-Urgent promotional email now also applies to "Needs Reply" —
  catches promo emails with a "reply to redeem"-style call to action.

### Added
- **New "always ignore" sender learning.** Correcting an email down to No
  Action now teaches Vigil to always treat future emails from that sender
  as No Action *and* never flag them as invoices — the FYI-direction
  counterpart to the existing VIP-sender learning.
- Alias sub-header (grouping by which iCloud address received an email)
  now shows whenever an item has one, not just when a section has 2+
  distinct addresses — more consistent, easier to spot at a glance.

## v0.3.4 (Linux) — 2026-08-06

### Fixed
- **Account switcher actually works now.** Switching accounts and
  refreshing previously left the brief showing the first account's
  emails, unchanged — the switch/refresh handlers were sending the
  in-memory result straight to the results window instead of reading the
  freshly-written brief back off disk, so the window never actually got
  the new account's items. Reported same day the switcher shipped, fixed
  same day.
- Alias sub-header text size increased (12px → 14px) — reported too hard
  to read.
- Re-verified and re-applied the `--no-sandbox` AppImage fix (see v0.2.0
  below) for this build.
- Excluded a leftover dev-only `PATCH-NOTES.md` from the shipped app
  bundle (recurring build-config gap, same class as the `config/` fix
  below).

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
