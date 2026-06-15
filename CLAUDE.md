# CLAUDE.md — BrickTriage (CONSTELLATION)

Collaboration guide for **Cowork** (SPEC + HTML prototype + deploy) and **Claude Code**
(native Swift, later). `SPEC.md` is the source of truth; `SECRETS.md` governs all keys.

## Deploy/repo session log (2026-06-15)

- Repo `pixelfairy17/bricktriage` is now **PUBLIC** (free GitHub Pages requires public, or a
  paid plan). **Live at https://pixelfairy17.github.io/bricktriage/**, serving `main` / root.
- Verified before going public: no key, token, backup, or photo is committed anywhere in
  history — repo holds only `index.html` + core docs.
- Added `.gitignore` blocking `backups/`, `*backup*.json`, `SECRETS.md`, `Secrets.xcconfig`,
  `.env*` — guardrail so secrets/data can't be committed by accident.
- v0.28.1 **shipped** the "include `rb_api_key` in backup" fix (was pending).
- ⚠️ **Repo is public now** → this **reverses** the old "commit data backups for history"
  plan. Backups contain the key + photos; **never commit them.** History/rollback covers the
  *app code* (`index.html`), not data.
- Deploy loop: branch → PR → e taps **Merge** → Pages auto-rebuilds → reload on phone.

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
  SPEC changes **awaiting e's approval**. Not merged into SPEC.md. **(local-only — not in the repo.)**
- `docs/BrickTriage-Handoff-v0.28.0_1.md` — v0.21.1 → v0.28.0 build handoff. **(local-only.)**
- `README.md` — stub (`# bricktriage`). Expand on request.

## What's actually in the GitHub repo (public)

Only 6 paths are tracked: `index.html`, `CLAUDE.md`, `SPEC.md`, `CHANGELOG.md`, `README.md`,
`.gitignore`. The `docs/`, `archive/`, `design/`, and `backups/` folders below describe the
**full local project on e's device** — they are **not pushed** (backups/secrets are gitignored;
docs/design/archive simply haven't been added). Don't assume they exist in a fresh clone.

## Folder layout (local project; repo subset noted above)

- **root** — `index.html` (the deploy), `CLAUDE.md`, `SPEC.md`, `CHANGELOG.md`, `README.md`,
  `.gitignore`. `SECRETS.md` is local-only (gitignored).
- `docs/` — handoffs + `spec-edits/` (proposed, unmerged SPEC changes). Local-only.
- `archive/` — superseded prototype snapshots (not deployed). Local-only.
- `design/` — mockups + the CORS probe (design artifacts, not the app). Local-only.
- `backups/` — data backups. **gitignored; never commit** (they contain photos and the API key).

## Files

- `index.html` — **CURRENT prototype, v0.28.1. This is the deploy** (the only app file in the repo).
- `archive/bricktriage-v0.28.0.html` — last snapshot before the key-in-backup fix. (local-only)
- `archive/bricktriage-v0.26.0.html`, `archive/bricktriage-v21.1.html`,
  `archive/BrickTriage 2.html` — older snapshots. (local-only)
- `design/*-mockups.html`, `design/bricktriage-moc-mockups.pdf`,
  `design/bricktriage-cors-probe.html` — design/probe artifacts, not the app. (local-only)
- `backups/bricktriage-backup-*.json` — data backups (photos base64-inlined → multi-MB;
  include `rb_api_key` as of format v6). **gitignored; never commit.**

## How the prototype is built

- Single self-contained HTML. **Preact 10 + htm + idb-keyval via esm.sh, no build step.**
- Persistence: **IndexedDB** (store `bricktriage` / `kv`) via a cache + write-back layer
  (`load` / `save` / `del`). API key stored under `rb_api_key`.
- Direct browser `fetch` to **Rebrickable** + **Brickognize** — CORS confirmed `*` (2026-06-11),
  no proxy needed.
- **On every change: bump `APP_VERSION` and prepend a `CHANGELOG` entry** (in-file array + this repo's CHANGELOG.md).

## Deploy workflow (GitHub Pages)

- **Live: https://pixelfairy17.github.io/bricktriage/** — GitHub Pages, **public repo**, serving
  `main` / root. (Was Netlify Drop; went public 2026-06-15 because free Pages requires it.)
- Deploy = land the current HTML on `main`; Pages auto-rebuilds (the "pages build and deployment"
  workflow runs itself, ~1 min). Normal loop: branch → PR → e taps **Merge**.
- **Code rollback ("if something breaks") = `git revert` / redeploy a previous commit.** This
  is the safety net — every deploy is a traceable commit.
- After each deploy on iPhone: **delete the Home-Screen icon and re-add it** (iOS caches the
  launch page against the icon).
- ⚠️ **Repo is public + Pages is public.** Never put data backups or key values anywhere in the
  repo (`.gitignore` enforces backups/secrets). The deployed `index.html` carries no key.

## Secrets / key handling

- **Rebrickable key**: free, read-only, low sensitivity. Entered in app Settings → stored on
  device (`rb_api_key`) → sent only to rebrickable.com. **Never hardcoded or committed.**
- Decision (2026-06-15): **key stays device-only.** Backup now *includes* the key so a restore
  brings it back — **shipped in v0.28.1** (backup format v6). This means backup files are now
  secret-bearing: **never commit them** (repo is public; `.gitignore` blocks them).
- BrickLink OAuth1 (Phase 2): native `Secrets.xcconfig` only, never in browser.

## Backups / data restore (Phase 0)

- Backup = Settings → **Download backup JSON** (buys / sets / photos / merch / mocs / cats).
  Restore = import that file (replaces data).
- Decision (2026-06-15, updated): repo went **public**, so **do NOT commit backups** (they now
  carry the key + photos). Restore is a **manual one-file import** of a backup kept in
  iCloud/Files. `.gitignore` blocks `backups/`/`*backup*.json` as a guardrail.
  Auto-restore-on-boot is deferred (it needs a stored token, which a public repo can't hold safely).
- **Shipped (v0.28.1):** `rb_api_key` is included in export/import, so a restore also restores
  the key (kills the "re-enter the key every time" friction). This is *why* backups are now secret-bearing.
- Photos inflate backups to multi-MB. Keep full photo backups in iCloud/Files; nothing backup-
  related belongs in the public repo.

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
