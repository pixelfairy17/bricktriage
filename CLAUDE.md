# CLAUDE.md — BrickTriage (CONSTELLATION)

Collaboration guide for **Cowork** (SPEC + HTML prototype + deploy) and **Claude Code**
(native Swift, later). `SPEC.md` is the source of truth; `SECRETS.md` governs all keys.

## What this is

- Personal bulk-LEGO sorting + **cross-purchase set-completion** tracker. Single user (e),
  Sydney. iPhone-first (used while sorting), iPad second. iCloud sync (native phase).
- **Phase 0** = single-file HTML prototype, in live use logging real bulk.
- **Phase 1+** = native SwiftUI + SwiftData/CloudKit.
- **Direction (decided 2026-06-15): HTML now, native soon.** Keep iterating the workflow on
  HTML until it stops changing, then move to native. The known "visual bugs" are
  PWA/standalone-mode quirks (safe-area, fixed bars, separate storage), not app logic.

## Source of truth / map

- `SPEC.md` — product, data model, APIs, phases. **Authoritative.** (root)
- `SECRETS.md` — key-handling rules. Real key values never in chat/code/git. (root)
- `CHANGELOG.md` — per-version history of the HTML prototype. (root)
- `docs/spec-edits/SPEC-edits-v0_28_0.md`, `docs/spec-edits/SPEC-edits-v0_20_0.md` — proposed
  SPEC changes **awaiting e's approval**. Not merged into SPEC.md.
- `docs/BrickTriage-Handoff-v0.28.0_1.md` — v0.21.1 → v0.28.0 build handoff.
- No `README.md` yet (the global map convention) — create on request.

## Folder layout

- **root** — `index.html` (the deploy), `CLAUDE.md`, `SPEC.md`, `SECRETS.md`, `CHANGELOG.md`.
- `docs/` — handoffs + `spec-edits/` (proposed, unmerged SPEC changes).
- `archive/` — superseded prototype snapshots (not deployed).
- `design/` — mockups + the CORS probe (design artifacts, not the app).
- `backups/` — data backups. **Keep OUT of any pushed/published path** (they contain photos
  and now the API key).

## Files

- `index.html` — **CURRENT prototype, v0.28.1. This is the deploy.**
- `archive/bricktriage-v0.28.0.html` — last snapshot before the key-in-backup fix.
- `archive/bricktriage-v0.26.0.html`, `archive/bricktriage-v21.1.html`,
  `archive/BrickTriage 2.html` — older snapshots.
- `design/*-mockups.html`, `design/bricktriage-moc-mockups.pdf`,
  `design/bricktriage-cors-probe.html` — design/probe artifacts, not the app.
- `backups/bricktriage-backup-*.json` — data backups (photos base64-inlined → multi-MB;
  now also include `rb_api_key`).

## How the prototype is built

- Single self-contained HTML. **Preact 10 + htm + idb-keyval via esm.sh, no build step.**
- Persistence: **IndexedDB** (store `bricktriage` / `kv`) via a cache + write-back layer
  (`load` / `save` / `del`). API key stored under `rb_api_key`.
- Direct browser `fetch` to **Rebrickable** + **Brickognize** — CORS confirmed `*` (2026-06-11),
  no proxy needed.
- **On every change: bump `APP_VERSION` and prepend a `CHANGELOG` entry** (in-file array + this repo's CHANGELOG.md).

## Deploy workflow (GitHub Pages)

- Moving Netlify Drop → **GitHub Pages, private repo.**
- Deploy = commit the current HTML; Pages serves it.
- **Code rollback ("if something breaks") = `git revert` / redeploy a previous commit.** This
  is the safety net — every deploy is a traceable commit.
- After each deploy on iPhone: **delete the Home-Screen icon and re-add it** (iOS caches the
  launch page against the icon).
- ⚠️ **GitHub Pages publishes the *site* publicly even from a private repo.** Never put data
  backups or key values in the published path.

## Secrets / key handling

- **Rebrickable key**: free, read-only, low sensitivity. Entered in app Settings → stored on
  device (`rb_api_key`) → sent only to rebrickable.com. **Never hardcoded or committed.**
- Decision (2026-06-15): **key stays device-only.** Backup will be made to *include* the key
  so a restore brings it back (fix pending — see below).
- BrickLink OAuth1 (Phase 2): native `Secrets.xcconfig` only, never in browser.

## Backups / data restore (Phase 0)

- Backup = Settings → **Download backup JSON** (buys / sets / photos / merch / mocs / cats).
  Restore = import that file (replaces data).
- Decision (2026-06-15): safest path — **private repo, no browser token.** Commit data
  backups to the repo for history/traceability; restore is a **manual one-file import**.
  Auto-restore-on-boot is deferred (it needs a public repo or a stored token).
- **Pending fix:** include `rb_api_key` in export/import so a restore also restores the key
  (kills the "re-enter the key every time" friction).
- Photos inflate backups to multi-MB; committing full backups repeatedly bloats git. Prefer
  committing a **photo-light data backup** for history; keep full photo backups in iCloud/Files.

## Working rules (carried from the project brief)

- Concise bullets + headers. No beginner programming explanations. No padding. No re-explaining
  the spec back.
- **Ask before executing** if goal/placement is ambiguous — give concrete options + a
  recommendation, never open-ended questions.
- **Never invent** file contents, names, or decisions. If a referenced file is missing, stop
  and say so.
- **Plan first, wait for OK** before moving/deleting anything.
- When e reports a friction point, **fix the workflow first; polish never.**
- Workflow learnings → write back as **proposed SPEC.md edits** (e approves) in a `SPEC-edits-*`
  file, then fold approved ones in.
- Never ask e to paste key values into chat (SECRETS.md).

## Cowork vs Claude Code split

- **Cowork** (this app): SPEC work, HTML prototype, GitHub Pages deploy, backups.
- **Claude Code** (Mac): native Swift (Phase 1+) built from `SPEC.md` + this file. Secrets via
  `Secrets.xcconfig` (see SECRETS.md).
