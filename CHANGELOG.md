# Changelog

All notable changes to Vigil are documented here. Every GitHub release's
notes are the relevant slice of this file, so any release tag shows both
what's new in that version and everything that came before it.

## v1.1.0 (Windows) — 2026-08-11

> **v1.0.0 has been withdrawn.** That build accidentally bundled the
> developer's own local config-backup folder. It contained no data
> belonging to anyone who downloaded it, and nothing about your setup was
> ever collected or transmitted — Vigil remains fully local. If you have
> the v1.0.0 download, delete it and use this release instead.

### Added
- **Business Memory is the landing screen**, opening on **Business
  Vitals** — Cash Flow, Vendor Spend and Invoice gauges built from what
  Vigil has actually learned about your vendors, with a flag strip
  calling out anything unusual.
- **Summary band on the brief** — the same Urgent / invoices / watches
  counts the tray popup ends on now carry into the window, so opening
  the brief continues that screen instead of restarting on a raw list.
  Counts that are zero aren't shown at all.
- **Windows installer** — a proper Setup with a Start Menu entry, desktop
  shortcut, real uninstaller and background update support. The portable
  single-file `.exe` is still available.

### Changed
- **The Daily Brief was restyled.** Emoji icons replaced with line icons
  across the sidebar and filters, section headers rebuilt as quiet labels
  rather than headings that competed with your email subjects, and
  urgency now reads as a coloured dot on the subject line instead of a
  bar down the side of every card.
- **The brief animates in** — content settles into place one piece at a
  time and each vitals gauge draws its ring as it arrives. Honours your
  system's reduced-motion setting.
- The greeting is plainer, and the line beneath it now tells you how much
  actually came in.

### Fixed
- Builds no longer bundle developer config backups or internal
  coordination docs. The app archive is less than half its previous size.
- The uninstaller and update metadata are no longer lost when a new build
  is applied over an existing install.

> Linux AppImage / .deb for 1.1.0 aren't in this release yet — Tanner:
> pull gatordts0/vigil-src **main @ 1f4c109**, rebuild, and attach the
> Linux assets. **Any Linux build made before 1f4c109 carries the same
> bundled config backups — don't distribute those.**

## v1.0.0 (Windows) — 2026-08-08 — withdrawn

### Added
- **Dedicated Settings window** — Mail / AI / Help accordion in its own
  glass window (not cramped in the Daily Brief sidebar).
- **Settings in the tray popup** — **Settings…** sits under **+ Add
  another account** so you can open Settings without opening the brief.
- **Add note to CRM** on email cards (click-to-handoff Comment on a Lead).
- **Call Logs** tab in the Daily Brief (read-only; needs Call Log access
  on the Frappe site).
- **Glass UI polish** — dual pulsing green edge glows on popup and brief.

### Fixed
- **Daily Brief text/icons** no longer show garbled characters in the
  greeting, Ask panel, and section headers.

> Linux AppImage / .deb for 1.0.0 are not in this release yet — Tanner:
> pull gatordts0/vigil-src **main @ 4dbed82**, rebuild, and attach
> Linux assets (or ship a 1.0.0 Linux follow-up). This tag ships the
> Windows portable .exe only.

## v0.7.1 (Windows) — 2026-08-07

### Fixed
- **Windows no longer silently signs you out after a rebuild or relaunch.**
  Saved accounts and settings now live in a stable AppData folder (same
  idea Linux already used), with a one-time migration the first time you
  open this build. Closing or reinstalling the single-file Windows
  download shouldn't wipe your login anymore.
- **Run-at-login** won't re-point itself at a temporary extract path when
  you're running the portable Windows build from Temp — that was a
  common source of odd black console flashes at startup.

### Added
- **Glass look for the popup and Daily Brief** — frosted panel, soft blur,
  green edge glow, and a much less see-through main surface so the
  desktop doesn't show through while you work.

> Linux AppImage / `.deb` for 0.7.1 are not in this release yet — stay on
> v0.7.0 (Linux) until those land. This tag ships the Windows portable
> `.exe` only.

## v0.7.0 (Linux) — 2026-08-07

### Fixed
- **The four security issues from your audit are fixed** - thank you for
  the detailed writeup, it was genuinely useful:
  - Every internal message inside the app now checks it's actually
    coming from Vigil's own window before acting on it, instead of
    trusting anything that could send a message.
  - The specific actions you flagged (opening a file, opening a link,
    revealing a file in its folder, opening the results window) are now
    restricted to only the files/links Vigil itself would ever actually
    use - not arbitrary paths or URLs.
  - Both windows now have a proper Content-Security-Policy and can't be
    made to navigate anywhere or pop open a new window.
  - **The Chromium sandbox issue has a real fix now, not just a
    workaround**: Vigil is now also available as a `.deb` package (see
    below) - installing it that way lets the sandbox actually run
    instead of needing `--no-sandbox`. The AppImage still needs
    `--no-sandbox` (that's a hard limitation of the AppImage format
    itself, not something Vigil's code can fix), so if the sandbox
    matters to you, the `.deb` is the one to use going forward.
- **Updated to a current, supported version of Electron** (was 12
  versions/multiple years behind) plus a mail-sending library update
  that had a few real security fixes of its own - both checked
  carefully against everything Vigil actually does and tested end to
  end before shipping, nothing about how the app works should feel
  different.

### Added
- **A `.deb` package**, for anyone who'd rather `apt install` Vigil than
  run the AppImage directly - see below. The AppImage keeps working
  exactly as before if you prefer that.
  To install it: `sudo apt install ./vigil_0.7.0_amd64.deb` (downloaded
  from this release, same as the AppImage). Uninstall later with
  `sudo apt remove vigil` if you ever want to.


## v0.6.0 (Linux) — 2026-08-07

### Fixed
- **The taskbar/dock icon now actually shows up.** Since the icon first
  shipped, it only ever appeared in the system tray - the taskbar/dock
  kept showing a generic icon instead. Root cause was how the icon file
  was packaged for Linux, not the icon itself. Fixed and confirmed by
  extracting the real, rebuilt app and checking the icon files directly.

### Added
- **Vigil should notice things more accurately now**, especially two
  changes worth knowing about:
  - Classification (Urgent/Needs Reply/No Action) and invoice detection
    can now run on a separate, more accurate model than the interactive
    Ask panel/chat, if you've got a bigger local model available. Off by
    default - nothing changes unless you turn it on in Settings.
  - Asking about your brief now understands paraphrased questions
    better - e.g. "what did the guy about the roof say" can find the
    right email even if it never uses the word "roof."
- **Asking something totally unrelated to your inbox** (general trivia,
  "write me a poem") now gets a clear, quick "that's outside what I can
  help with" instead of a vague non-answer.
- Small reliability fix: the background watch-checker now tries to
  start Ollama itself if it's installed but not running yet (matching
  what the main app already did), instead of silently doing nothing
  until you open the app.

## v0.5.0 (Linux) — 2026-08-07

### Added
- **Vigil finally has a real icon and logo** instead of a generic
  placeholder — an owl mark (watching over your inbox) plus a proper
  wordmark, on the taskbar, the tray, and inside the app itself.
- **"Push to Frappe"** — an email-detected invoice can now be sent
  straight into your Accounting module as a draft Purchase Invoice, with
  one click from the Invoices tab. It's always created as a draft, never
  auto-submitted, so you review and finish it yourself before it's real.
  (Needs a new write-enabled connection under Settings first — ask Diego
  if you want this turned on.)
- **"Report a bug or suggestion"** — a new entry in Settings lets you
  send a bug report or idea straight over, with a screenshot attached if
  you want, no email client needed.
- **The "Ask about this brief" panel is bigger** and no longer needs
  double scrolling to read a normal answer.
- **Asking to see something now actually shows it in the brief.**
  "Show me the emails from David" or "show me 5 no action emails" used
  to just print a list back inside the little chat box — now it filters
  the real brief list on screen instead, with a quick note confirming
  what matched.

### Fixed
- **The activity log no longer misattributes a multi-account run to one
  account.** A brief covering two mailboxes used to log the whole total
  under whichever one it finished with — now it spells out the real
  per-account split.
- Several rounds of small follow-up fixes to the "show me" filtering
  above, including recognizing "urgent"/"no action"/"reply needed" as
  real categories instead of literal search words.

---

## v0.4.2 (Linux) — 2026-08-06

### Fixed
- **The "wrong date" bug from v0.4.1, for real this time.** That
  release's own fix accidentally broke itself — a leftover naming clash
  in the code meant the popup was quietly reading the picked time window
  (like "168") as a raw timestamp instead of a duration, which is where
  "Dec 30, 4:00 PM to Dec 31, 4:00 PM" came from. Fixed at the root and
  double-checked nothing else in the app has the same kind of clash.

## v0.4.1 (Linux) — 2026-08-06

### Fixed
- **The popup now says the right fetch window.** Switching to 7 days and
  reopening the app used to still say "the last 24 hours" everywhere in
  the chat popup — that text was never actually wired to the real
  setting. Fixed in the greet screen, the "working" message, and the
  "done" summary.
- **Smaller download** — this build is ~16% smaller than the last one
  (switched a compression setting in the build itself), so it downloads
  and installs faster.

### Added
- **Fetch-window picker added to the popup itself** — not just the
  results window's settings. Pick 24 hours / 3 days / 7 days right on
  the "want me to check your emails?" screen, before saying yes.

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
