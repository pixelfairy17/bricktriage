# BrickTriage — Native (Phase 1) Build Plan

Source of truth for product/data is `SPEC.md`. This file is the **engineering plan** for porting the
Phase 0 HTML prototype (`index.html`) to a native iOS app. Built by **Claude Code on the Mac**
(not the web session). Decision (2026-06-15): **go native now** — the workflow is settled; the
remaining HTML pain is WebKit/PWA presentation jank, not app logic. Account tier: **paid Apple
Developer ($99/yr)** so iCloud sync is in from day one.

## Why native (what it fixes for free)

These are all *standalone-PWA* quirks the HTML keeps fighting — native removes them by default:

- Soft-keyboard covering fields → SwiftUI keyboard avoidance + `ScrollViewReader`.
- Bottom safe-area gap / fixed-bar drift → real `TabView` + safe-area handling.
- Separate PWA vs Safari storage, fragile IndexedDB → SwiftData on disk.
- Preact positional-diff freezes → no manual DOM diffing.
- Home-screen icon cache / "delete & re-add to deploy" → normal app install.

## Prerequisites

- Mac + Xcode (latest), iOS deployment target = latest − 1.
- **Paid Apple Developer account** (chosen) → enables CloudKit + 1-year signing + TestFlight.
- App ID with **iCloud (CloudKit)** + **CloudKit container** capability.
- Rebrickable API key handling via `Secrets.xcconfig` (see `SECRETS.md`) — never committed.
- No App Store submission for now: install via Xcode → device, or TestFlight for iPhone+iPad.

## Architecture

- **SwiftUI** app, `@main` App + `TabView` (Sets / Sorting / Buys / Settings — mirror the HTML tabs).
- **SwiftData** (`@Model`) for persistence; **CloudKit** mirroring via
  `ModelConfiguration(cloudKitDatabase: .private)` for iPhone↔iPad sync (the SPEC goal).
  - CloudKit constraints to design around: every non-optional must have a default or be optional;
    no unique constraints; relationships must be optional. Plan models accordingly.
- **Networking**: `URLSession` async/await. Two services:
  - `RebrickableClient` — `Authorization: key <k>` header; sets, set parts (paged), themes,
    set minifigs, minifig parts. Mirror the endpoints already proven in `index.html`.
  - `BrickognizeClient` — multipart `POST /predict/` with a downscaled JPEG.
- **Images**: store part/set catalog images as remote URLs (load + cache via `AsyncImage` or a small
  cache). Store **user photos** (buys/merch/MOCs) as files in the app container, path referenced from
  SwiftData — do NOT inline base64 in the DB (the HTML backup does; that's why backups are multi-MB).
- **Secrets**: Rebrickable key in Keychain (entered in Settings), or `Secrets.xcconfig` for a baked
  default during dev. BrickLink OAuth1 (Phase 2) stays native-only.

## Data model (mapped from the current JS records)

Translate the existing shapes 1:1 so the HTML backup JSON imports cleanly (see Migration).

- **Buy**: id, label, kind (bulk | regular | loose), channel, datePurchased, pricePaid, rrp, notes,
  photos[] (file refs). Receipt totals derived from linked sets+merch.
- **TrackedSet**: setNum, name, year, theme, img, imgDefault, condition (new|used), built,
  pricePaid, rrp, box{has,cond,note}, instructions(none|paper|digital) + booklets[] + bookCount,
  triage(undecided|keep|sell|partOut), valueSet, valuePartOut, logMode(find|remaining), missingCount,
  notes, buyIds[], dateAdded, **parts[]**, **minifigs[]**.
  - **Part**: rowId, partNumber, partName, colorID, colorName, img, qtyNeeded, qtyFound, isSpare.
  - **Minifig**: figNum, name, img, quantity, parts[] (same Part shape). Tracked separately from set %.
- **Merch**: id, name, year, rrp, pricePaid, condition, qty, marketRate, buyIds[], notes, photos[],
  dateAdded.
- **Moc**: id, name, notes, status(building|built|disassembled), photos[], pdfs[] (file refs),
  allocations[] (setNum, partRowId, qty, snapshot of part meta), wanted[] (target build import).
- **WTB** (want-to-buy): partNum, qty, colour, note, got.
- **Settings/KV**: currency, countSpares, card toggles, channels, sortTarget, partgroup.

Keep the **completion rules** identical: exclude separators always; exclude spares unless
`countSpares`; minifig parts tracked separately (not in set %).

## Screen inventory (parity targets)

1. **Sets list** — filters (triage/theme/condition/incomplete), views (flat/by-buy/by-theme),
   compact/detailed cards, segments Sets|Merch|MOCs|WTB.
2. **Set detail** — header (condition/built/price/photos gallery), accordions (value/triage/instr/
   box/buys+notes/**minifigs**), parts list (colour groups, find/remaining, select mode w/ steppers,
   tap-image-to-view), MOC move sheet.
3. **Add set** — Rebrickable fetch (normal/quick/bulk), scan, link-to-buy, drafts.
4. **Sorting** — search + scan, **target set**, cross-set "needs this part" hits, find-sets fallback.
5. **Buys** — bulk/regular/loose segments, receipts, spend summary, photos, detail.
6. **MOCs** — allocations from sets, return parts, wanted-parts import, PDF attach.
7. **Settings** — API key, currency, completion/card toggles, channels, backup import/export, wipe.

## Migration / interop

- Implement **import of the HTML backup JSON** (current format `v: 6`) → seed SwiftData on first run.
  This is the bridge: export from the live HTML app, import into native, done. Keep the importer even
  after launch (it's the rollback path while both exist).
- Native export should produce the same JSON shape (minus base64 photos → optional) for symmetry.

## Milestones & effort (Claude Code on Mac)

1. **M1 — Skeleton + data + import (≈1–1.5 days):** project, SwiftData models + CloudKit config,
   backup JSON importer, TabView shell. Verifiable: import a real backup, browse sets.
2. **M2 — Sets + parts checklist (≈1–1.5 days):** set list/cards/filters, set detail parts list with
   steppers/select mode, completion %, minifigs accordion. The core sorting loop.
3. **M3 — Add set + Rebrickable + scan (≈1 day):** fetch set/parts/minifigs, Brickognize scan,
   link-to-buy, drafts.
4. **M4 — Sorting mode (≈0.5–1 day):** search, target set, cross-set hits, scan-to-log.
5. **M5 — Buys / Merch / MOCs / WTB (≈1.5 days):** remaining segments + detail screens + spend.
6. **M6 — Settings, export, iCloud verify, polish (≈1 day):** key in Keychain, export, CloudKit
   iPhone↔iPad sync test, safe-area/keyboard polish.

**Single-device MVP (M1–M4): ~4–5 focused days. Full parity + iCloud (M1–M6): ~7–8 days.**

## Open decisions for `e`

- CloudKit container name + whether to share any data type (no — single user, private DB only).
- Photo storage cap / downscale target on native (HTML uses ≤1024px JPEG — keep).
- Whether to keep the HTML app live in parallel during the native ramp (recommended: yes, until M5).
