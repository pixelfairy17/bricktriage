# BrickTriage — Phase 0 Prototype Changelog

All changes to the single-file HTML prototype (deployed as `index.html`).
Hosted on GitHub Pages (was Netlify Drop). Stack: Preact 10 + htm via esm.sh, no build step, IndexedDB persistence (v0.10+; localStorage before that).

> Note: this file currently jumps from v0.28.1 to v0.14.0 — v0.15.0–v0.28.0 live in the in-file
> `CHANGELOG` array and `BrickTriage-Handoff-v0.28.0_1.md`. Back-fill here on request.

-----

## v0.42.0 — 2026-06-19

- **MOC Parts List — group by type.** Added a **Group: Colour ↔ Group: Type** toggle on the parts-list action row, using the same ~12 emoji buckets as the set parts list (🧱 Bricks, ▭ Plates, ⬛ Tiles, ◣ Slopes, ⬤ Round, ⚙️ Technic, …). Part type is read from the part name; the choice persists.

## v0.41.0 — 2026-06-19

- **Set parts list — Group: Type.** The Group chip now cycles **Colour → Type → Exact**. *Group: Type* buckets parts into ~12 easily-distinguished groups with emoji labels — 🧱 Bricks, ▭ Plates, ⬛ Tiles, ◣ Slopes & wedges, ⬤ Round/cones, 🪝 Brackets/clips/hinges, 🪜 Bars/rods, 🪟 Windows/doors/panels, ⚙️ Technic, 🛞 Wheels & tyres, 🌿 Plants/food/animals, 🧑 Minifig & gear, ✨ Other. Heuristic match on the part name (refinable from real use, like the colour groups / part-rate maps).

## v0.40.0 — 2026-06-19

- **Set parts list — tick = have.** Ticking a part's checkbox now **auto-marks it have** (qty → max). Unticking only deselects (un-mark via the row tap or the batch **Mark as not have**).
- **MOC Parts List — swipe-delete glitch fixed.** The red **Delete** panel now only renders while a row is actually being swiped, so it no longer bleeds through behind the qty/＋ controls at rest.
- **MOC Parts List — donor/found sources.** The donor line is now **"Found X in donor • Y in sets"** — `X` = free pieces in **♻️ donor** sets, `Y` = free pieces in other tracked sets **not marked built**. Each count is a **link** that lists the sets holding the part, each with a **purple →** to pull it straight into the MOC.

## v0.39.0 — 2026-06-19

- **Set parts list — checkbox always on.** Every part row now shows the select checkbox at all times (the **☑ Select** mode toggle is gone). Tick any part and the sticky batch bar appears with **Mark as have** (75%) and **→ MOC** (25%) **side by side on one line** (was two stacked full-width buttons). **All shown / None** kept.
- **Unified checkbox look.** The **MOC Parts List** now uses the same round **✓** checkbox as the set parts list (was a ☐/☑ glyph).

## v0.38.0 — 2026-06-19

- **MOC Parts List rows now read like a set's parts list.** The **have/need** count sits in the middle between the **− / +** steppers (which edit the *needed* qty). The have-count now also counts pieces sitting in sets you flag **♻️ Donor**, so it reflects what you can actually pull — surfaced on the row as **"N in donor sets"**.
- **Swipe-to-delete.** Swipe a parts-list row left to reveal **Delete** (replaces the easy-to-mis-tap ✕). **Clear parts list** moved to a small right-aligned button on the *Select all missing* row (was a big full-width button).
- **Part pictures in the MOC Parts List.** Thumbnails now also reuse images from any tracked set, are **tap-to-enlarge** (with a BrickLink lookup), and a **🖼 Get part images** button pulls real images + names from Rebrickable for imported / hand-added lines.
- **Add parts by name.** Search Rebrickable by **part name** (not just number) when adding to a MOC — results show a thumbnail + name to add in one tap.
- **Bug — duplicate minifigs.** A set with **2× of the same minifig** now doubles that fig's parts (qty needed). Re-fetch minifigs on an already-tracked set to apply (**↻ Re-fetch** in the Minifigs accordion). *(e.g. fig-006547 in 21159-1.)*

## v0.37.0 — 2026-06-18

- **MOC detail redesign — Parts List accordion.** The old "Wanted parts (target build)" card is now a collapsible **Parts List** block: the full bill of materials for the build (you may or may not own each part — this is distinct from a *want* list, which is only what you still need to buy). Colour groups + thumbnails as before, with a `have/total · %` summary on the accordion header so it reads while collapsed.
- **Mark parts → want list (bulk or individual).** Tick parts with a per-row checkbox (or tap the row), or **Select all missing**, then **🛒 Add N ticked parts to want list** pushes just those onto the MOC-linked want list — shortfall when partly had, else full qty. Replaces the old all-or-nothing "Add missing to want list" button.
- **Minimal Add / import.** A collapsible **➕ Add / import parts** inside the Parts List (hidden until tapped) holds the by-hand part-add field and the **XML / CSV import** — keeps the list clean while sorting.
- **Bug — qty steppers.** Each parts-list row now has **− / +** steppers to adjust the needed quantity (✕ still removes the line).
- **Bug — part thumbnails.** A row with no BrickLink colour id (hand-added or imported without one) now falls back to a **real allocated part's image**, so the picture shows once you've gathered the brick.

## v0.36.0 — 2026-06-17

- **Want lists & MOC wanted parts now read like a set's parts list.** Collapsible **colour groups** (tap a colour to expand/collapse, with have/need counts and a chevron) and **part thumbnails**. Thumbnails come from the BrickLink colour id captured at import (`img.bricklink.com/ItemImage/PN/{colorId}/{part}.png`), or the set part's own image when pushed from a set. *(Re-import an existing MOC list to pick up the colour ids for thumbnails.)*
- **Want-list parts gain − / have / + steppers.** Count how many you've actually got; the row marks itself off (struck through, greyed) when have = qty. Tapping the part name toggles it full/none. Completing a part on a **set-linked** list still syncs to the set per Settings → Want to buy (manual / ask / auto). Progress (partial have) now feeds the list's % and the WTB tab count.

-----

## v0.35.0 — 2026-06-17

- **WTB rework — named want lists.** The Want-to-buy tab is now a collection of **named lists** (like naming a buy) instead of one flat pile. Each list is either **standalone** (you name it) or **linked to a Set or MOC**. Open a list to see its parts **grouped by colour**, check them off, rename, or delete it. The old flat list migrates into one "General" named list automatically.
- **Two-way linking (push).** A Set (set detail) or MOC (wanted card) with missing parts shows a **"🛒 Add missing to want list"** button that pushes its shortfall (need − have) into a linked list — creating it the first time and **topping it up** on re-run. From the list you can jump back to the linked Set/MOC.
- **"Got" sync (Settings → Want to buy).** For a **set-linked** list, ticking a part as bought can also update the set, your choice: **Manual** (just check off), **Ask** (prompt each time to also mark it found in the set), or **Auto** (always mark it found, base-matched and capped). **MOC-linked** lists always count "got" parts toward the MOC's **% of plan**, so buying shows progress.
- **Backup format v7** includes want lists.

-----

## v0.34.0 — 2026-06-17

- **MOC import fixed and made readable.** The wanted-list CSV parser is now quote-aware (part names containing commas, e.g. `"Brick, Modified 1 x 2…"`, no longer shift the columns) and reads the colour **name** column instead of the colour-id number, and it captures the part name. Imported wanted lists now render **like a set's parts list** — grouped by colour with part name · colour · qty and a have/need count per row and per colour group. Import shows an **"Imported N parts (M pcs)"** confirmation.
- **MOCs are now set-like.** Added a **Planning** status (for builds you're still designing or haven't gathered parts for) and a **"% of plan"** readout in the header — the wanted list is the MOC's plan, matched against the real parts you allocate from sets. Have-counts base-match (a BrickLink import number like `3023` finds the Rebrickable part `3023b`).
- **Note 1:** opening a freshly added set auto-collapses Instructions, Box and Buys & notes (only Triage stays open).
- **Note 2:** the "just changed" part hint is its own one-line, right-aligned row under the count (no longer wraps into the part name).
- **Note 3:** opening a set scrolls to the top (was inheriting the list's scroll position).
- **Note 4:** returning from a set/MOC restores your place in the sets list (was jumping to the top).
- **Note 5:** sets with no Rebrickable part inventory (multipacks like 66786) show a clear explanation instead of an empty list.
- **Note 6:** "Yellowish Green" groups under Green, not Yellow.
- **Note 7:** in a set's parts **Select** mode you can bulk-move every selected found part into a MOC at once; the MOC picker defaults to the last MOC you used.
- **Note 8:** the sets list sort gains an **↑ Asc / ↓ Desc** toggle for Newest / Completion / Value.

-----

## v0.33.0 — 2026-06-17

- **Hunt mode "Find sets containing" now lists sets in-app for printed/sticker parts.** When you find a part that isn't in any set you track, **🔎 Find LEGO sets containing** returns a selectable list of sets right in the app — tap one to track it and log the part. Previously, printed/sticker parts (Brickognize returns the BrickLink id, e.g. `65474pb04`, which Rebrickable's direct part lookup 404s on) dead-ended at a BrickLink link, forcing you to copy the set number by hand and add it from the Sets tab. The finder now resolves the BrickLink id via Rebrickable's `bricklink_id` filter first (guarded against a server that ignores the param), so the whole pick-a-set flow stays in-app. The BrickLink "find in sets" link only appears if the part is genuinely unindexed by Rebrickable.

-----

## v0.32.0 — 2026-06-17

- **#1 Delete set — Cancel button.** The confirm step now shows **Cancel** alongside **Delete permanently** (was a single "Tap again to permanently delete" with no way back), so an accidental tap on "Delete set…" no longer risks a second reflex tap deleting the set.
- **#1 Reset part counts.** A **↺ Reset** chip in a set's parts filter row wipes every part count back to 0 (or the MOC-allocated floor, so allocations stay valid) after a confirm — for recounting from scratch. Also clears the Built flag.
- **#1 Tap confirmation.** After you +/− a part (or tap a row to mark it have/clear), a tiny green/red **"just changed → x/y"** line appears under that row. Fixes re-counting because you couldn't tell whether a tap had registered. *(A full app-wide action history is noted as a possible follow-up.)*
- **#2 Minifigs moved.** The Minifigs section now sits **directly under the parts filter**, above the colour groups — it was easy to miss below the list, especially in sets like Belle's Storybook (43177) where minifigs carry "normal" parts.
- **#3 Instructions & Box moved.** The 📖 Instructions & Box group now sits at the **bottom** of the parts list, below the colour groups.
- **#4 Est. value — market price.** The value accordion is renamed from "Est. value — BrickLink" to **"Est. value"** and gains **Market price** + **Last checked** fields above the manual BrickLink (As set / Parted out) fields. Entering a market price auto-stamps today's date.

-----

## v0.31.0 — 2026-06-17

- **#1 BrickLink price-guide link fixed.** The set-detail "BrickLink price guide ↗" link now opens `catalogPG.asp?S=<num>` — the price-guide table directly — instead of the v2 `catalogitem.page#T=P`, whose tab anchor often failed to switch on mobile (landing on the catalog tab).
- **#2 ♻️ Donor / spare set flag.** A new flag, separate from triage, in a set's Triage section. Mark a set kept **intact** in your donor box — available to part out or sell later if the price is right, but not broken down into a loose-parts list. Shows a `♻️ Donor` badge on set cards. Kept **out of Sorting mode's part-hunt by default** (a donor set is a parts *source*, not one you're filling); Settings → Sorting → "Hunt donor sets" includes them. A 🎯 target set is always hunted regardless.
- **#3 Scan → set for printed parts.** Scanning a printed/sticker part Rebrickable can't resolve (e.g. `65474pb04`) no longer dead-ends: Sorting mode offers a BrickLink **"find which sets have this part"** link (`catalogItemIn.asp?P=…&in=S`), and Add-set's scan does the same when Brickognize returns a part instead of a set. The part-image viewer gains an **"In which sets ↗"** link too.
- **Scan match — stickered/printed parts.** Brickognize returns the *applied* variant (`30350bpb187`), but a set's Rebrickable inventory lists the plain mould (`30350b`). Scans now **base-match** (strip the trailing print/pattern suffix + mould letter), so the sticker-applied tile in hand logs against the tile the set needs. Applies to 🎯 target auto-log and the cross-set search.
- **Sticker sheets hidden.** The loose "sticker sheet" inventory row is dropped from parts lists and completion % by default — you buy bulk with stickers already applied, so it was noise. Toggle in Settings → Completion %. The stickered tile itself still counts normally.
- **Parts list ordering.** Colour groups now sort **alphabetically** (Other last) instead of the old fixed Transparent→Pink order — no more hunting for where White landed.
- **Scan UI.** Result thumbnails are larger and **tap-to-enlarge**; added a note that BrickLink renders parts as grey line-art (colour isn't shown).

-----

## v0.30.0 — 2026-06-16

- **#1 Link existing set / merch to a buy.** A buy's detail page now lets you attach an *existing* set or merch item that currently belongs to no buy. `+ Link existing set` / `+ Link existing merch` (each with a count) appear only when there's something unlinked; tapping opens a picker showing just those orphan items, and the list shrinks live as you link them.
- **#2 Minifig parts no longer duplicated in the parts list.** The set-parts fetch now passes `inc_minifig_parts=0`, so minifig pieces (hair, torsos, accessories) appear only in the Minifigs accordion — not the main parts list. Existing sets keep their already-imported minifig rows until re-tracked.
- **#3 Found spares stay visible with Spares off.** Hiding spares now keeps any spare you've actually logged (`qtyFound > 0`) on screen, so found spares don't vanish when the Spares chip is toggled off.
- **#4 Built is reversible.** Marking a set Built still auto-fills every unticked main part, but it now records what it filled — un-ticking Built undoes exactly that fill and leaves hand-counted parts untouched.
- **#5 Smarter Select mode.** Entering Select mode auto-expands all part groups. The sticky batch button now reads the selection: when every picked row is already *have*, it becomes `Mark N as not have` (clearing down to any MOC-allocated floor); otherwise it stays `Mark N as have (full qty)`.

-----

## v0.29.7 — 2026-06-15

- **Pop-up fix (back button untappable / sheet covers whole screen).** v0.29.4 set sheets to `max-height:100%`, which made them fill the entire screen — no backdrop to tap-dismiss, and the back button sat in the iOS status-bar tap zone at the very top (which the system swallows, so taps didn't register). Sheets now leave a top gap (`max-height: calc(100% - 28px - env(safe-area-inset-top))`): a tappable backdrop strip + the back button clears the status bar. The keyboard cap is handled by this CSS against the visible-viewport-sized modal, so focused fields still scroll into view above the keyboard. Removed the JS that was forcing the sheet to full visible height.

-----

## v0.29.6 — 2026-06-15

- **Reverted the sticky/blurred sheet header from v0.29.5** — it broke the pop-up layout and made the back button untappable. Kept only the safe-area top whitespace (title clears the status bar/clock); sheets are back to normal scrolling.

-----

## v0.29.5 — 2026-06-15

- **Sheet / pop-up top spacing + sticky blurred nav.** v0.29.4's `max-height:100%` let tall sheets (e.g. merch edit) run to the very top, colliding the title with the status bar/clock. Added safe-area-aware top whitespace (`padding-top: 22px + env(safe-area-inset-top)`), and made the sheet header a **sticky, blurred top nav** (iOS-style) — it stays pinned while the form scrolls underneath. Applies to every modal sheet (each starts with a Header row).

-----

## v0.29.4 — 2026-06-15

- **Keyboard / focused-field visibility (the actual fix).** The sheet's `max-height` was `85vh` of the **full** screen, so when the keyboard was up the sheet was taller than the visible strip and `scrollIntoView` centred the focused field *inside that oversized sheet* — behind the keyboard. The sheet is now **capped to the visible viewport** (`fitModals` sets `maxHeight = visualViewport.height`), so focusing any field scrolls **it** into view above the keyboard and the rest of the form scrolls underneath. The scroll runs a few times (60/250/450 ms) to land correctly as the keyboard animates in.

-----

## v0.29.3 — 2026-06-15

- **Root-cause modal fix (the merch-edit bug, and the whole "can't see my screen" class).** Every popup sheet (merch edit, new/edit buy, scan, move-to-MOC, photo/gallery/part-image viewers, MOC editor, import) was rendered **inside** the scrollable `.content` container. On iOS, `position:fixed` is trapped by a scrolling ancestor instead of the viewport — so sheets slid off-screen, the tab bar showed through behind them, and the keyboard hid the field. All modals now render through a **portal to `<body>`** (Preact `createPortal`), escaping `.content` entirely. Combined with the v0.29.2 `visualViewport.offsetTop` tracking, sheets stay put and the focused field stays above the keyboard.

-----

## v0.29.2 — 2026-06-15

- **Merch edit / all modal sheets — keyboard pushes the sheet off-screen.** When iOS opens the keyboard it shifts the visual viewport down (`offsetTop` > 0), but a `position:fixed` bottom sheet stayed anchored to the layout-viewport top — so the sheet slid up off-screen and the app/tab-bar showed through behind it, with the field you were editing hidden. Modals now track `visualViewport.offsetTop/offsetLeft` (not just height), keeping the sheet aligned to what's actually visible and the focused field above the keyboard.

-----

## v0.29.1 — 2026-06-15

- **#8 follow-up — keyboard fix vs. viewport sizer conflict.** The 0.29.0 keyboard fix added a second `visualViewport` handler that fought the existing standalone viewport sizer (`fitApp`) — both set `#app` height to different values on every keyboard event, thrashing the layout and breaking field editing inside sheets (e.g. **merch → Qty**). Consolidated into a single set of handlers: `#app` shrinks to the visible viewport **only while the keyboard is open** (otherwise sized from `window.innerHeight` for the standalone-stale-units case), modal sheets stay pinned above the keyboard, and the focused field scrolls into view app-wide.

-----

## v0.29.0 — 2026-06-15

UI-bug sweep + backlog from the sorting session.

- **#1 Sorting — 🎯 Target set.** Pick the set you're filling; every scan/search then matches **only** that set. A single unambiguous match auto-logs +1; otherwise the matching colour rows are listed for a tap. The target persists (`bt:sortTarget`); tap ✕ to clear and hunt across all sets again.
- **#6 Minifigs.** Sets now fetch their minifigs from Rebrickable (`/sets/{set}/minifigs/` + `/minifigs/{fig}/parts/`). New **Minifigs** accordion in set detail: each fig is its own collapsible parts inventory (legs/torso/head/hair/accessories) with per-fig have/need + **Mark all parts present**. Tracked **separately** from the main parts count (minifig parts aren't in the base set inventory). Sets tracked before this build get a one-tap **Fetch minifigs** button. Minifig data rides along inside the set record (backup unchanged, still `v: 6`).
- **#3 Parts — tap to view.** Tapping any part image (set parts or minifig parts) opens it full-size with its number/colour and a BrickLink part link — disambiguates lookalike parts.
- **#4 BUG FIX — Select mode corruption.** Leaving ☑ Select mode no longer mangles the part rows (qty vanishing, a dead checkbox left behind). Rows are now keyed per mode so Preact remounts them cleanly — same positional-diffing landmine as v0.20–v0.22.
- **#5 Select mode upgraded.** Rows keep their − / qty / + steppers in select mode, and tapping a row marks it all-present (greys it out, tap again to undo). Batch **Mark N as have** still works. Everything autosaves.
- **#7 Spares excluded from group totals.** Colour-group have/need counters no longer add spares unless **Settings → count spares** is on (Black reads 10/10, not 10/15) — matching how completion % already worked.
- **#8 BUG FIX — keyboard hides fields.** The iOS soft keyboard no longer covers the field being edited (notes, prices, etc.). The app tracks the keyboard via `visualViewport`, shrinks to the visible area and scrolls the focused field into view — app-wide, not just modal sheets.

-----

## v0.28.1 — 2026-06-15

- **Backup now includes the Rebrickable API key** — restoring a backup also restores the key, so it no longer has to be re-entered after re-adding the app to the Home Screen. Backup format `v: 6`; import is backward-compatible with `v: 5`. Keep backup files private (the deployed app never contains the key).
- **Deploy cleanup** — removed the local-path "saved from url" artifact from the file head for GitHub Pages.

-----

## v0.14.0 — 2026-06-11

### Add set

- **Source-buy picker permanently on the Add page** — shown regardless of Quick/Bulk toggle state, below the set-number card; pre-selected to the last bulk buy added to; supports multiple buys, removal, and inline quick-create
- **Quick add links the picker selection** — saved confirmation names the linked buy(s); selection applies to every set saved from the page until changed
- **Confirm screen simplified** — duplicate buy picker removed; non-quick flow is now fetched preview + Save/Back only

-----

## v0.13.1 — 2026-06-11

### Quick add target corrected

- Links to the **BULK-kind buy most recently added to** (judged by newest linked set’s dateAdded), never a Regular buy; falls back to the newest-created bulk buy if none has sets yet
- Known edge: linking an *old* set to a buy later doesn’t promote that buy (ranked by set add date, not link time)

-----

## v0.13.0 — 2026-06-11

### Standalone layout (Home-Screen web app)

- **Tab bar pinned for real** — root cause: the page *body* was the scroll surface, and iOS standalone mode lets `position:fixed` elements drift while the body scrolls/rubber-bands. Body is now `overflow:hidden`; all scrolling moved inside the `#app` container. Also stabilises the Buys select-mode action bar

### Add set

- **⚡ Quick add toggle** — fetching by number or scan auto-links the default buy and saves immediately, skipping the confirm screen (scan candidate list still shown as normal)
- **↻ Bulk mode toggle** (visible with Quick add on) — stays on the Add page after each save with a green “✓ Saved …” confirmation and cleared input; off returns to the sets list
- Both toggles persist (`bt:quickadd`, `bt:bulkadd`)

-----

## v0.12.0 — 2026-06-11

### Set cards

- **Channel tag moved to the bottom buy-label line**, right-aligned, smaller (10px) — set name gets full width back
- **Price block tightened** — Set/Parts and Found ≈/Full ≈ are two tight lines (line-height 1.45) reading as one block; no more wrap-spill
- **📖 icon** next to `setNum · year` when the set has a paper instruction book

### Standalone layout

- **Safe-area top padding** — content no longer slides under the iOS status bar in Home-Screen mode (title was overlapping the clock; Safari’s chrome had been masking the missing inset)

-----

## v0.11.0 — 2026-06-11

### Buy detail

- **“All parts if complete” line** under Found parts — full-inventory qty × avg-guide rate across linked sets
- Both valuation lines now read from precomputed index fields (no per-render record loads)

### Set cards

- Value row adds **Found ≈** (green) and **Full ≈** (dim) avg-guide estimates next to Set / Parts
- Index entries carry `foundQty/foundVal/fullQty/fullVal`; old entries auto-backfill on first load

### Currency

- **Settings → Currency** — symbol-only dropdown (AUD/USD/NZD/CAD/EUR/GBP), defaults AUD ($), no conversion; all hardcoded `A$` removed; minifig rate label now `$2`

### Boot diagnostics

- Static pre-rendered DOM snapshot replaced with a “Loading BrickTriage…” placeholder + **visible red error banner** if startup fails (module/esm.sh load errors were previously a silent dead UI — the Home-Screen “not working” symptom)

-----

## v0.10.0 — 2026-06-11

### Storage

- **Moved from localStorage to IndexedDB** (idb-keyval via esm.sh) behind the existing synchronous `load/save/del` API: in-memory cache hydrated before first render, writes persist in the background. Quota goes from ~5 MB to hundreds of MB
- **Automatic one-time migration** from localStorage on first load; the old localStorage copy is left untouched as a frozen snapshot / emergency fallback
- Settings gauge now shows real browser-reported usage/quota (`navigator.storage.estimate()`)
- Note: home-screen web apps and Safari tabs have **separate storage** on iOS — migration runs in whichever one is opened first; keep using the same one. Safari ~7-day eviction still applies to IndexedDB — backup after big sessions

-----

## v0.9.0 — 2026-06-11

### Sets list

- **View chip moved to front** — ≡ Flat / ⊟ By-buy is now the first chip in the filter bar
- **By-buy accordion state persists** — open/closed buy groups survive navigation (`bt:sv:groups`); also cleared on wipe

### Set cards

- **Channel as coloured tag** — yellow channel badge(s) next to `setNum · year` (unique channels across linked buys); buy-label line below no longer repeats the channel

### Part valuation (avg price guide)

- **Per-part rate heuristic** — keyword match on part name + colour, highest rate wins: 5c basic bricks/plates/tiles (default) · 20c clips/hinges/brackets/Technic connectors · 30c plants/flowers/trans/Friends colours · 50c animals/printed/big moulded · $2 minifigs/dolls
- **Rate shown in part row** — yellow label after `colour · partNumber` in the set parts checklist
- **Buy detail: Found parts line** — `n pcs ≈ $x` under Est. recovery: found qty × rate summed across all linked sets (found parts only, spares included)
- Notes: keyword map is a first-guess (same class as colour buckets) — refine from real use. Minifigs don’t appear in Rebrickable set-parts inventories, so the $2 rate is dormant until a minifig fetch is added (native v1 candidate).

-----

## v0.8.0 — 2026-06-11

### Sets list

- **Persisted view state** — triage filter, sort mode (Newest/Completion/Value), and view mode now survive tab switches and back-navigation (localStorage `bt:sv:*`)
- **By-buy view** — new ⊟ By-buy chip groups sets under collapsible buy headers (label, kind · channel, set count); No Buy Configured is the last group; a set spanning multiple buys appears under each; flat sort/filter still applies within groups
- **Buy channel on set cards** — dim line shows linked buy labels + channels (e.g. `Container 3 · FB Marketplace + Container 4`); hidden inside the By-buy view where it would be redundant

### Parts checklist (Set detail)

- **Group: Colour / Group: Exact toggle** — new chip; persists globally (`bt:partgroup`)
- **Colour mode**: keyword-match bucketing into Transparent → Metallic → Glow/Special → Black → Gray → White → Brown/Tan → Red → Orange → Yellow → Green → Blue → Purple → Pink → Other; spectrum-ordered; similar shades sort adjacent within bucket
- **Exact mode**: original alphabetical-by-exact-colour-name behaviour
- **Exact colour always shown in row** — `colorName · partNumber` dim line visible in both modes

### Terminology

- “Buy categories” renamed to **Buy channels** everywhere in the UI (data keys unchanged; backups forward/backward compatible)
- Kind (Bulk / Regular / No Buy) is now clearly distinguished from Channel (purchase platform) in Settings hint

-----

## v0.7.0 — 2026-06-11

### Set values

- **Manual est. value fields** in set detail — “As set (used)” and “Parted out” (AUD), entered manually from the BrickLink price guide
- **Value badges on set cards** — yellow for set value, purple for part-out; hidden when both empty
- **↓ Value sort** added to sets list sort cycle (Newest → Completion → Value); sorts by the higher of the two values

### Buy detail

- **Est. recovery** — sum of set + part-out values across linked sets shown under Price paid in buy detail

### Notes

- Automated BrickLink pricing confirmed impossible in the web prototype: OAuth1 secrets cannot live in-browser; page scraping blocked by CORS. Phase 2 native only.

-----

## v0.6.1 — 2026-06-11

### Bug fixes

- **Filter dropdown clipping on iOS** — root cause: absolutely-positioned floating panels were clipped by the app container’s overflow context. Replaced `DropNav` floating panels with in-flow expanding menus (tap chip → options expand as a card below the filter bar, pushing content down). Nothing floats; cannot clip.
- Removed `overflow-x:hidden` on `#app` (was a band-aid that became a clipping source)

### Scan

- **Photo downscaling** — client-side resize to ≤1024px JPEG q0.85 before Brickognize upload; full-res iPhone photos (multi-MB) were failing/timing out on mobile data
- **Richer error messages** — HTTP status + API response detail surfaced; error state now includes actionable tips (one item per photo, plain background, fill the frame, parts individually not assembled clusters)

-----

## v0.6.0 — 2026-06-11

### Filter nav redesign

- **Vertical dropdown menus** — chevron rendered inside the chip label as one text node (`All ▾`); tapping opens an in-flow card with vertically-stacked options; picking collapses. Eliminated the separate arrow-button sibling that was being multiplied by preact’s positional diff.

### Select mode

- Icon-only `☑` chip moved to the far left of the filter bar; becomes `✓` when active

### Settings

- All Settings sections now collapsible via a shared `Section` component: Buy channels, Rebrickable API key, Backup/transfer, Danger zone (wipe), Version & changelog
- Wipe now requires: expand section → tap once → confirm tap (three deliberate actions)

-----

## v0.5.0 — 2026-06-11

### Mass-edit select mode (Buys)

- `☑ Select` chip enters checklist mode on the buys list; works across all filter views
- Fixed action bar above tab bar: selected count, All shown / None buttons, → Bulk, → Regular, Channel dropdown (all configured channels + clear); already-matching buys skipped

### Filter persistence fix (Buys) — v0.4 bug recurrence

- Root cause identified: kind/category/sort were component-local state wiped on unmount; buy-detail open state was also local. Fixes: filters persist to localStorage; buy-detail `openBuyId` lifted to App; `key="buy-list"` / `key="buy-detail"` force clean remounts on tree swap
- “Add buy” moved to a modal sheet (button always rendered — eliminates conditional button/form swap that contributed to DOM corruption)

### Filter nav (Buys) — v0.4 design

- No Buy chip: counter removed, B capitalised
- Sort labels capitalised: ↓ Date / ↑ Date / ☰ Manual
- **✕ Clear** chip appears when kind or category filter is active; resets both without touching sort

### Settings

- Buy channels collapsible, moved to top of Settings
- Version & changelog collapsible card added

### Scroll

- `overscroll-behavior-y: none` added (runaway scroll; confirmed fixed in v0.6.1 key-swap cleanup)

-----

## v0.4.0 — 2026-06-11

### Buy editing

- **Edit modal** (label, kind, channel, price, purchase date) accessible from buy detail and from swipe-right `•••`
- **Explicit purchase date field** in `BuyForm` (date picker, defaults today); applied to quick-create and full Buys form

### Navigation

- Set opened from a buy detail returns to that buy on back (not to sets list); `setOrigin` tracked in App; tab bar back resets origin

### Buy detail sort

- ↓ Newest / ↓ Completion toggle chip in buy detail sets list

### Swipe actions

- Swipe-right on buy card: `•••` (edit) + `✕` (delete with confirm); delete unlinks buy from sets → sets move to No Buy Configured

### Manual reorder

- Long-press (~350ms) then drag in Manual sort mode; order persists (`bt:buyorder`)
- Sort chip cycles: ↓ Date → ↑ Date → ☰ Manual; choice persists (`bt:bv:sort`)

### No Buy Configured

- “No buy (n)” chip in kind filter; lists sets with no buy linked; tap → set detail

### Category manager (Settings)

- Per-category rename (propagates to all buys) and delete; add field

### Backup v2

- Exports include categories + manual buy order (`bt:buyorder`)

-----

## v0.3.0 — 2026-06-11

### Buys redesigned as first-class entities

- `kind`: bulk | regular (regular = LEGO Store / Kmart / single eBay purchase etc.)
- `category` (now: channel): free string from a user-managed list (FB Marketplace, eBay, Gumtree, LEGO Store, Kmart; + Other free input)
- Explicit `date` field
- **All Buys** page (renamed from Buys): All / Bulk / Regular kind chips; category chip row filtered to channels actually in use
- **Spend summary card** at top: BULK and REGULAR rows each with “$x this month · $y total” (calendar-month boundary)
- **Buy detail** view: price paid, linked sets with completion rings; tapping a set navigates to set detail (back returns to buy)

### Buy cards

- Kind badge + channel badge (or “no channel” placeholder) + set count + date

### BuyForm shared component

- Used by Buys tab add + quick-create inside BuySelect picker

-----

## v0.2.0 — 2026-06-11

### Bulk-buy picker (Set detail + Add set)

- `BuySelect` component: selected buys as removable chips; dropdown to add more; “＋ New bulk buy…” creates inline (label + price)
- Replaced the single `<select>` in Add set and the chip-row in Set detail

### Parts checklist — collapsible colour groups

- Each colour name is a tappable header row: `COLOUR · count` + `found/needed` tally + ▸/▾
- Default: all collapsed; typing in the parts filter auto-expands all groups
- Header found/total counts go green when complete

### Scan — camera or library

- Removed `capture="environment"` attribute from scan file input; iOS now offers Photo Library / Take Photo sheet

-----

## v0.1.0 — 2026-06-10

### Initial Phase 0 prototype

**Sets**

- Sets list with triage filter chips (All / Undecided / Keep / Sell / Part-out) and sort (↓ Newest / ↓ Completion)
- Add set: set number input → Rebrickable fetch (set metadata + full parts inventory, auto-paginated)
- Brickognize scan on Add set (box / instructions / model photo → candidate list → confirm → prefill number)
- Set detail: metadata card, BrickLink price guide link, triage picker, instructions status, source bulk buy chips, notes, completion ring
- Parts checklist: grouped by exact colour, each part row shows image, name, part number, qty stepper (±1), tap-row toggle (0↔needed); “near complete” (≥90%) badge + nudge
- Delete set with double-confirm

**Bulk buys**

- Simple list: label, price paid (AUD), linked set names
- Add buy inline form on Buys tab
- Bulk buy dropdown on Add set and Set detail with inline creation

**Settings**

- Rebrickable API key field (persisted to localStorage; never leaves device)
- Download backup JSON (v1: sets + buys)
- Restore from backup (replaces all data)
- Wipe all data (double-confirm)
- localStorage usage gauge (~MB, Safari cap note)

**Infrastructure**

- Single HTML file; no build; preact 10 + htm + hooks via esm.sh CDN
- localStorage persistence: `bt:index` (set summaries), `bt:set:{num}` (full records), `bt:buys`
- `overscroll-behavior-y: none` (body)
- iOS meta tags: mobile-web-app-capable, status-bar black-translucent, viewport-fit=cover
- Dark warm-LEGO palette: `--bg:#1C1A17`, `--yellow:#F5C518`, `--green:#5FBF6E`, `--red:#E05A4E`, `--blue:#4E9BE0`, `--purple:#A98BD6`