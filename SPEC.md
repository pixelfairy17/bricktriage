# BrickTriage — Bulk LEGO Sorting & Set Completion Tracker

## Purpose

Personal-use app for triaging bulk LEGO purchases. Core differentiator vs existing apps
(Brick Collector, Rebrickable, Brickset): **cross-purchase set completion** — parts found
across multiple separate bulk buys accumulate against the same set, with completion %
tracked over time. Secondary goals: replace camera-roll-as-database with structured
photo storage, and inform keep / sell / part-out decisions with market value.

## User & context

- Single user (e), Sydney AU. iPhone primary (used while physically sorting), iPad secondary.
- Always on latest iOS. iCloud sync between devices.
- May ship to App Store later — code quality and architecture should not preclude this,
  but no multi-user/auth/analytics work in v1.

## Core workflow (the loop the app must make fast)

1. While sorting, user identifies a set (via instructions found, or in-app Brickognize scan
   of box / instructions / built model).
1. Enter set number → app fetches set metadata + full parts inventory from Rebrickable.
1. Set is added with: instructions status (none / paper / digital), source bulk-buy,
   photos, notes. **Quick-add path (validated in Phase 0): pre-selected source buy +
   auto-save on fetch + stay-on-page bulk mode** for rapid container logging.
1. Parts checklist: as user finds parts, tick them off (with qty). Completion % updates.
1. Triage decision per set: **Keep / Sell / Part-out (MOC donor)** — changeable anytime.
1. Later bulk buys: when entering/finding parts, app surfaces “this part is needed by
   incomplete set X” — the treasure-hunt feature.
1. Sell decision support: market value lookup (v1: deep-link to BrickLink price guide +
   avg-guide heuristic; v2: BrickLink API).

## Data model

### Buy (renamed from BulkBuy; may later rename to Haul — label-only)

- id, label (e.g. “Container 3 — Gumtree March”), **kind**: bulk | regular,
  **channel** (was “category” — user-managed list: FB Marketplace, eBay, Gumtree, LEGO
  Store, Kmart, etc.), datePurchased (user-editable, defaults today), pricePaid, notes, photos[]
- Terminology: kind = purchase type (Bulk/Regular); channel = platform/place
- Spend summary per kind: this-calendar-month + all-time totals
- Channel list: add/rename (propagates)/delete (clears)
- “No Buy Configured” = virtual view: sets with empty buyIds (haul unknown or deleted)
- **“Last bulk buy” concept (Phase 0): the BULK-kind buy most recently added to**
  (newest linked set’s dateAdded; never a Regular buy; falls back to newest-created bulk
  buy). Used as the Add-set picker default. Carry into native v1.

### TrackedSet

- id, setNumber (Rebrickable format, e.g. “75281-1”), name, year, theme, imageURL (cached)
- instructionsStatus: none | paper | digital (paper shown as 📖 on set cards)
- triage: undecided | keep | sell | partOut
- sourceBulkBuyIDs[] (a set can span multiple buys — this is the key feature)
- photos[], notes, dateAdded
- valueSet, valuePartOut (manual — entered from BrickLink price guide). Live pricing is
  Phase 2 (OAuth1 API).
- derived: completionPercent (foundQty / totalQty across PartEntry); avg-guide valuations
  foundVal / fullVal (see Part value heuristic) — precomputed into the list index, not
  recomputed per render.

### PartEntry (one row per part+color in a set’s inventory)

- id, trackedSetID, partNumber, colorID, colorName, partName, imageURL
- qtyNeeded (from Rebrickable inventory), qtyFound (user increments)
- isSpare flag (Rebrickable marks spares)

### LoosePart (v2 — parts pool not tied to a set)

- partNumber, colorID, qty, sourceBulkBuyID

### Part value heuristic (avg price guide — Phase 0, carry to v1)

Keyword match on part name + colour, highest rate wins:
5c basic bricks/plates/tiles (default) · 20c clips/hinges/brackets/Technic connectors ·
30c plants/flowers/trans/Friends colours · 50c animals/printed/big moulded · $2 minifigs/dolls.
Keyword map is a first-guess — refine from real use. Minifigs don’t appear in Rebrickable
set-parts inventories; $2 rate dormant until a minifig fetch is added (native v1 candidate).
Surfaced as: rate label per part row; Found ≈ / Full ≈ on set cards; Found parts /
All-parts-if-complete lines on buy detail.

### Currency

Phase 0: symbol-only setting (AUD/USD/NZD/CAD/EUR/GBP), defaults AUD ($), no conversion.
Native v1: proper currency handling; conversion remains out of scope until pre-shipping.

## Views / screens

1. **Sets list** — filterable by triage status; sort by newest / completion / value (cycle).
   Badge for “near complete” (≥90%). Views: Flat + By buy (collapsible buy headers; No Buy
   last; a set in multiple buys appears under each; accordion state persists). Cards show:
   name (full width), setNum · year · 📖 (paper instructions), triage + completion badges,
   two-line price block (Set/Parts manual values; Found ≈ / Full ≈ heuristic), buy labels
   with right-aligned small channel tags. All filter/sort/view state persists.
1. **Set detail** — metadata, photos, instructions toggle, triage picker, completion ring,
   parts checklist (Group:Colour / Group:Exact, search/filter, per-part rate label),
   BrickLink price link, manual est. value fields (set used + part-out), source buy picker.
1. **Sorting mode** (the killer screen) — fast part search across ALL incomplete sets:
   type/select a part (or scan it) → see which tracked sets need it → one-tap increment
   qtyFound. Optimised for one-handed phone use with bricks in the other hand.
1. **Add set** — set number input + scan; **source-buy picker permanently on the page**,
   pre-selected to the last bulk buy added to; ⚡ Quick add (auto-save on fetch, skip
   confirm) and ↻ Bulk mode (stay on page after save) toggles, both persisted.
1. **Bulk buys** — list of purchases, what came out of each, cost vs estimated value
   recovered (manual values + heuristic found/full parts lines).
1. **Parts pool** (v2) — aggregate of part-out sets + loose parts; export for Rebrickable
   MOC matching.

### UX rules learned in Phase 0

- **Inline creation from pickers**: any picker referencing another entity (e.g. source
  bulk-buy dropdown in Add Set) must allow creating that entity inline — in practice the
  set is discovered before the buy is catalogued. Applies to native v1.
- **Rapid-entry add flow**: pre-select the most likely target (last-added-to bulk buy),
  show it on the entry page (not the confirm page), make confirm skippable (quick add),
  and offer a stay-on-page loop (bulk mode). The user logs whole containers in one sitting.
- **Parts colour grouping**: generic colour-bucket mode (Blue, Green, Gray…) is the right
  default; Group:Exact toggle retained for precision. Keyword-based in Phase 0 (first-guess
  — refine from real use). In native v1 use Rebrickable colour family field. Exact colour
  always shown in part row regardless of mode.
- **Set values are manual in Phase 0**: user reads BrickLink price guide and types values.
  Est. recovery on buy detail auto-sums linked set values; heuristic avg-guide adds
  found/full parts estimates. Phase 2 automates via BrickLink API.
- **Persist ALL list/filter/sort/view state per tab**: state loss was reported as a bug on
  both Sets and Buys tabs. Systematic fix: storage keys per tab (`bt:sv:*`, `bt:bv:*`).
- **Navigation returns to origin**: set opened from buy detail → back to that buy. SwiftUI
  navigation stack handles this naturally.
- **Wipe and destructive actions behind a collapse**: Settings sections all collapsible;
  wipe requires expand → tap → confirm.
- **Floating dropdowns clip on iOS**: use in-flow expanding menus (chevron inside the chip
  label; options expand as a column below the bar). Never position:absolute in a scroll
  container.
- **Standalone (Add to Home Screen) is a different layout environment** (empirical,
  2026-06-11/12). Safari’s chrome masks missing safe-area handling: with
  black-translucent status bar, content renders under the clock unless
  `env(safe-area-inset-top)` is applied. And `position:fixed` bottom bars drift when the
  *body* is the scroll surface — fix is body `overflow:hidden` with scrolling inside an
  inner container. **Always test layout in standalone mode, not just Safari.** iOS also
  caches the launch page against the icon — delete + re-add the icon after deploys.
- **Standalone storage is separate from Safari** — IndexedDB/localStorage containers
  differ between the Safari tab and the Home-Screen app. Whichever is opened first gets
  the migration; the user must keep using the same one. (Moot in native v1.)
- **Visible boot diagnostics**: a static pre-rendered fallback DOM looks alive but is dead
  if the JS fails to load — replace with explicit Loading state + on-screen error banner.

## APIs

### Rebrickable v3 (primary catalog) — free API key

- GET /api/v3/lego/sets/{set_num}/ — set metadata
- GET /api/v3/lego/sets/{set_num}/parts/?page_size=1000 — full inventory (paginated;
  follow `next` until null — prototype does this automatically)
- GET /api/v3/lego/parts/{part_num}/ — part detail
- Auth: `Authorization: key {APIKEY}` header. Cache aggressively; catalog data is static.
- **CORS: CONFIRMED `Access-Control-Allow-Origin: *`** (empirical, 2026-06-11) — direct
  browser fetch works from any https origin.
- Note: BrickLink set IDs and Rebrickable set numbers usually match (“12345-1”) but part
  numbering differs between systems — store Rebrickable part numbers, keep BrickLink ID
  as optional alias field.

### Brickognize (in-app scan identification) — VERIFIED viable, free public API

- Identifies parts, sets (box/built model/instructions photo), minifigs from a single photo;
  returns ranked candidates with confidence scores + BrickLink/Rebrickable IDs.
- UX: camera/photo → candidate list → user confirms match → feeds set search or part lookup.
- Native iOS: proven (existing open-source iPhone apps use it).
- **Web CORS: RESOLVED — CONFIRMED `Access-Control-Allow-Origin: *`** (empirical,
  2026-06-11). Scan ships in the Phase 0 prototype (set scan in Add Set, part scan in
  Sorting mode).
- Watch item: set-scan candidates return BrickLink IDs, fed directly into Rebrickable
  fetch — usually identical; mismatches to be logged if hit.
- Constraints: one item per photo; needs good lighting/contrast; downscale to ≤1024px
  JPEG before upload (full-res mobile photos fail).
- Risk: free single-maintainer service, no SLA/ToS published. Scan is an OPTIONAL enhancer —
  manual entry must always work. If shipping to App Store: contact author for permission
  (Phase 3 gate).

### BrickLink (pricing)

- v1: deep-link only: <https://www.bricklink.com/v2/catalog/catalogitem.page?S={setNumber}#T=P>
- v2: official API (OAuth 1.0a, requires consumer registration + token; register own keys).
  Endpoint: GET /items/SET/{no}/price (guide_type=sold for 6-month sold average).
- Automated pricing confirmed impossible in the web prototype (OAuth1 secrets can’t live
  in-browser; scraping blocked by CORS) — Phase 2 native only.

## Phase 0 findings (2026-06-10/12) — environment

1. **Claude artifact sandbox blocks ALL external fetches** (CSP; TypeError in <5ms for both
   APIs). Not a CORS issue. n8n proxy cannot help inside the artifact either. Artifacts are
   unsuitable for any API-calling prototype.
1. **WebKit blocks network fetch from `file://` pages entirely** (iOS Safari/Edge, and any
   iOS browser — all are WebKit). Local HTML from iCloud Drive/Files is a dead end on iPhone.
   Chromium on desktop allows it.
1. **Both APIs serve `Access-Control-Allow-Origin: *`** (verified Mac/Chromium from file://,
   then iPhone Safari from Netlify origin). CORS permanently resolved for web.
1. Consequence: **Plan C** — prototype is a single self-contained HTML file (preact + htm
   via esm.sh, no build) hosted on any static https host (currently Netlify Drop), opened
   in iOS Safari, **Add to Home Screen** for app-like use (see standalone UX rules above).
1. n8n proxy fallback: NOT NEEDED — removed from plan.
1. **Storage: IndexedDB via idb-keyval (v0.10+), localStorage retired** (auto-migrated;
   old copy kept as frozen snapshot). Quota effectively unlimited for this data size;
   real usage/quota read from `navigator.storage.estimate()`. Safari ~7-day eviction
   still applies — backup after big sessions. Netlify Drop is static-only: no writable
   backend storage exists there; cloud auto-backup deferred (manual JSON export → Files →
   iCloud covers Phase 0).

## Phases

### Phase 0 — Prototype (REVISED: hosted single-file HTML, not artifact)

Goal: validate the sorting-mode workflow on real bulk for ~1 week. **Now in live use
logging real bulk (started 2026-06-12).**
Delivered through v0.14.0 (fourteen iterations over three days): sets list (persisted
state, By-buy view, channel tags, 📖 indicator, value block), set detail
(Group:Colour/Exact parts checklist with per-part rates, manual est. values), triage,
Rebrickable fetch with auto-pagination, Brickognize scan (≤1024px downscaled), rapid add
flow (permanent buy picker defaulted to last-added-to bulk buy, ⚡ Quick add, ↻ Bulk
mode), buys (kind/channel/date/edit/spend/recovery + heuristic lines/mass-select/swipe/
drag/No Buy/persisted filters), BrickLink link, currency symbol setting, IndexedDB + v2
backup, collapsible Settings, in-app changelog, boot diagnostics, standalone-mode layout
fixes (safe-area, pinned tab bar).

### Phase 1 — Native v1 (SwiftUI, SwiftData + CloudKit sync)

Everything in “Core workflow” except BrickLink API. Photos stored in app (CloudKit assets).
Brickognize scan included (proven in Phase 0 web; native path already proven by existing apps).
Carry over: quick/bulk add flow, last-added-to-bulk-buy default, part value heuristic
(+ minifig fetch so the $2 rate activates), colour-family grouping via Rebrickable data.
Target: latest iOS, iPhone + iPad layouts.

### Phase 2 — Native v2

Sorting-mode part search across sets, parts pool, loose parts, BrickLink price API,
bulk-buy cost-recovery stats, CSV/Rebrickable export.

### Phase 3 (only if shipping)

App Store prep: onboarding, empty states, error polish, privacy manifest, screenshots,
Brickognize author permission.

## Open questions (resolve during Phase 0)

- ~Part identification in-app~ RESOLVED: Brickognize, shipped in prototype.
- ~Brickognize CORS~ RESOLVED: ACAO * confirmed; n8n proxy dropped.
- ~Storage quota~ RESOLVED: IndexedDB (v0.10).
- Parts checklist granularity: both modes live in prototype (tap row = toggle all,
  steppers = per-piece) — pick winner from real use.
- Does “near complete” threshold + missing-parts list (to buy the last few parts on
  BrickLink) belong in v1? Likely cheap to add. Prototype shows ≥90% badge + nudge.
- BrickLink-vs-Rebrickable set ID mismatch from scan: log occurrences if any.
- Part rate keyword map + colour bucket map: refine both from real-bulk use.
- Quick-add edge: “last added to” ranks by set add date, not link time — does relinking
  old sets ever need to promote a buy? Watch in practice.

## Non-goals (v1)

- Multi-user, auth, social features
- Selling/listing integration (only valuation)
- Full MOC designer — export to Rebrickable covers this
- Currency conversion (symbol-only until pre-shipping)