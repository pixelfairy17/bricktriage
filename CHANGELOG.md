# BrickTriage — Phase 0 Prototype Changelog

All changes to the single-file HTML prototype (deployed as `index.html`).
Hosted on GitHub Pages (was Netlify Drop). Stack: Preact 10 + htm via esm.sh, no build step, IndexedDB persistence (v0.10+; localStorage before that).

> Note: this file currently jumps from v0.28.1 to v0.14.0 — v0.15.0–v0.28.0 live in the in-file
> `CHANGELOG` array and `BrickTriage-Handoff-v0.28.0_1.md`. Back-fill here on request.

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