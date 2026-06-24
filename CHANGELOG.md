# BrickTriage — Phase 0 Prototype Changelog

All changes to the single-file HTML prototype (deployed as `index.html`).
Hosted on GitHub Pages (was Netlify Drop). Stack: Preact 10 + htm via esm.sh, no build step, IndexedDB persistence (v0.10+; localStorage before that).

> Note: v0.15.0–v0.28.0 were back-filled here (2026-06-20) from the in-file `CHANGELOG` array; they
> are condensed from the per-release notes. The full original wording also lives in that in-file array
> and `BrickTriage-Handoff-v0.28.0_1.md`.

-----

## v0.50.0 — 2026-06-24

- **WTB part rows now match a set's parts list exactly.** Tappable thumbnail to enlarge, tap the name to toggle got, and a horizontal **− / got/needed / +** stepper — replacing the typeable got box and the "N needed / M got" text. Complete rows go **green-bordered + dimmed**, same as a set.
- **WTB images fetch automatically on open** (matching sets, whose images come free with the set inventory) — no need to tap **🖼 Get part images**. Runs once per list when a Rebrickable key is set; the manual button remains as a fallback.
- **Fixed: parts pushed from a MOC's parts list (and a set's "Add missing") now carry their image + Rebrickable colour id.** They arrive **with** a picture instead of a blank thumbnail. This was the real reason "Get part images" looked broken on MOC-linked lists — those rows had no Rebrickable colour id to fetch a colour-correct image from.

-----

## v0.49.0 — 2026-06-24

- **WTB parts list now works like a set's parts list.** Replaced the **To Get / Got It** sections with a single grouped list plus a **filter bar**:
  - a **text filter** (name / number / colour),
  - a **Hide purchased** toggle (hides parts where have ≥ qty),
  - a **Group: Colour ↔ Group: Part** toggle (persists, `bt:wtbpartgroup`),
  - an **⌄/⌃ All** expand-all / collapse-all chip,
  - a **↺ Reset filter** chip (clears the text filter + Hide purchased).
- Typing in the filter **auto-expands** every group. **Clear purchased** replaces the old **Clear Got It** button.

-----

## v0.48.0 — 2026-06-22

- **Loose / PAB buys can itemise parts.** Open a **Loose** buy → **Loose parts** → **+ Add part**: find a part by **📷 scan**, **part number**, or **Rebrickable description search** (the same finder as the want list), optionally pick its **colour**, then record **qty** (a plain text box — no stepper), **price paid**, and any **per-part shipping**.
- **Live unit price** per part — **(price paid + part shipping) ÷ qty** — so per-piece cost is comparable across different AliExpress sellers / PAB.
- **Whole-buy shipping.** Loose buys gain an order-level **Shipping** field (Add / Edit buy) for postage on top of per-part shipping. The buy detail shows **Price paid + Shipping + Parts subtotal + Total**, and the **Loose** spend stat now includes shipping and itemised parts.
- **Fix:** a Loose / PAB buy's detail header no longer mislabels itself as **Bulk** — it now reads **Loose / PAB**.

## v0.47.0 — 2026-06-20

- **Instructions — Leaflet.** Added a **Leaflet** option alongside *None / Book / Reprint* for single-sheet instructions.
- **Building status (set).** A set can be marked **🔨 Building** — this is a **status/filter only**: unlike **Built** it does **not** mark any parts as found. It shows on the set card and gets its own **🔨 Building** filter chip in the Sets list.
- **Recategorise a part's type.** A new **Part category** selector — in a part's **enlarged image** and in the **swipe-left Edit** sheet on MOC / WTB parts — overrides the auto *Group: Type* bucket for that part **everywhere it appears**. Fix a mis-grouped part yourself instead of reporting it. (Overrides are included in backups.)
- **Sets → by-buy: narrow to one buy.** The by-buy view gains a **buy picker** that filters the list to a **single specific buy** (by name), so you can see exactly which sets came from that purchase — not just grouped by channel.

## v0.46.0 — 2026-06-20

- **Tags (new, cross-app).** Create your own **coloured tags** and attach them to **sets, MOCs, want-list parts, and any individual part**. A part's tags follow it **everywhere it appears** (set inventory, MOC parts list, want lists). Manage tags in **Settings → Tags**: rename (✎), recolour (🎨), or delete (✕ — deleting strips the tag from everything using it). Tag a set or MOC from its **🏷 Add/Edit tags** button; tag a part from its **enlarged image**. The Sets list gains a **🏷 Tags filter** (shows sets matching **all** ticked tags), and tags appear on **set & MOC cards**. Tags are included in **backups** (export format v8).
- **Add set — scan a part now finds the SET.** When a box/instructions scan comes back as a **part** rather than a set, tap **🔎 Find the set**: the app looks up (by colour where needed) which **sets contain that part** on Rebrickable and lists them — tap one to **fetch + add** it. Replaces the old dead-end that only linked out to BrickLink. (The BrickLink lookup is still there as a fallback.)
- **MOC Parts List — import a set's exact parts.** Under **➕ Add / import parts**, enter a **set number** to pull that set's full Rebrickable inventory (main **+** minifig parts; spares / separators / sticker sheets excluded) straight into the parts list — for building a set from your own bulk instead of buying it boxed.
- **Part-type grouping fix.** A part whose **name starts with** *Plant / Tile / Plate / Brick* is now classified by that **leading noun first**. Fixes **10884** *"Plant, Leaves 6 x 5 Swordleaf with Clip"* landing in **🧑 Minifig & gear** (its name contains "**sword**") — it now correctly reads as **🌿 Plants**.

## v0.45.0 — 2026-06-19

- **Part-type grouping fixes.** *Plant…* parts now always group under **🌿 Plants** — even *Plant, Plate Round 1x1 with 3 Leaves* (was landing in **▭ Plates** because of the "plate" in its name). Minifig **Equipment / Utensil** parts (e.g. *Equipment, Dish*) now group under **🧑 Minifig & gear** instead of **🧱 Bricks**.

## v0.44.0 — 2026-06-19

- **MOC Parts List — Edit in the swipe menu.** Swiping a parts-list row left now reveals **Edit** as well as **Delete**. Edit opens a sheet to change the part's **colour** — picked from the real colours that part comes in on Rebrickable, each shown as the part *rendered in that colour* — and its **quantity** (a typeable number field).
- **MOC Parts List — colours now match.** Picking/editing a colour stores the **colour-correct Rebrickable image** (the generic part image is only the default colour, which is why thumbnails didn't match the listed colour). **🖼 Get part images** is colour-aware too — re-run it, or edit a line's colour, to correct existing rows.
- **Part-type grouping fix.** Removed the catch-all **⬤ Round/cones/cylinders** bucket. *Plate, Round / Special* now group under **▭ Plates**, *Plant, Round* under **🌿 Plants**, *Brick, Round* under **🧱 Bricks** (the leading noun wins). Standalone cones/cylinders/domes/dishes fold into Bricks; cockpits into **🪟 Windows**. **Tiles / Plates / Bricks** also rank **above Slopes & wedges**, so *Tile, Curved / Macaroni* reads as a **⬛ Tile** (was landing in Slopes via "curved"); plain slopes/wedges/cheese still group as **◣ Slopes & wedges**.
- **Set parts list — reverted tick-to-auto-mark.** The always-on select checkbox now just **selects** a part (it no longer auto-marks it have). Apply with the bottom **"Mark N as have / not have"** batch bar — as it worked the iteration before.
- **WTB — add a part by scan / number / description.** The add-a-part flow now lets you **scan** a part, type a **part number**, or **search Rebrickable by description**, then pick the **colour** (with thumbnails) and **qty** before adding — replacing the blind type-it-in form.
- **WTB — pictures + enlarge.** Part pictures are fetched from Rebrickable (**🖼 Get part images**); tap a thumbnail to **enlarge** it.
- **WTB — clearer qty + quick edit.** Each row reads **"N needed / M got"**; the got-count is a **typeable number field** (plus − / +). The old ✕ delete moved into the **swipe-left menu** (Edit colour/qty/For + Delete).
- **WTB — To Get / Got It sections.** A list now splits into collapsible **To Get** and **Got It** sections. Mark a part's got-count up to its needed qty (tap the name, type, or step) and it moves to **Got It**; knock it back down and it returns to **To Get**. **Clear Got It** in one tap.
- **WTB — per-part "For" link.** A wanted part can optionally be tagged **For: 🧩 MOC / 🧱 Set** so you know which build it's destined for while shopping (shown as a chip on the row).

## v0.43.0 — 2026-06-19

- **MOC Parts List — have/need now counts only what's recorded.** Donor / "in sets" availability no longer auto-counts toward have/need, and no longer greys/dims the row, until you actually **transfer** it in with **→**. The count is now just what you've transferred from sets + bought. Each row shows a small **♻️ N** under the qty = pieces transferred into this MOC, so a transfer is clearly indicated.

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

## v0.28.0 — 2026-06-13

- **NF2 — MOC parts import.** Every MOC now has a *Wanted parts (target build)* list. Import the build from BrickLink Studio (XML) or Rebrickable (CSV/TSV) by paste, or add part numbers by hand. A have/need counter + % matches your real allocated parts against the target (by part number). Add-by-scan and BrickLink colour-name mapping still to come — colours import as-is for now.

## v0.27.0 — 2026-06-13

- **NF1 — Loose / PAB buys.** New buy kind with fill-a-box presets (Small $17.95 / Large $32.99 one-tap the price), its own Loose tab and spend line in Buys.
- **NF4 — Want-to-buy list.** New WTB tab (Sets → WTB). Flag parts to grab at the PAB wall or a second-hand store with qty / colour / note, check them off as you find them, and clear checked-off in one tap.

## v0.26.0 — 2026-06-13

- **#2 Sets — Condition filter** (✨ New / ♻️ Used) + an Incomplete-only toggle in the filter bar; stack with Triage and Theme, ✕ Clear resets everything.
- **#11 New-set cards declutter.** % and triage hidden by default on new sets (Settings → "Set cards (new sets)" switches each back on). Cards show "♻️ N → MOC · M in set" when pieces are out to MOCs.
- **#12 New view.** ✨ By condition (New purchases vs Bulk/used) accordion, added to the view chip cycle.
- **#14 Add merch from a buy's detail** screen, pre-linked to that buy (bulk + regular).
- **#17 Merch** gains quantity + current market rate (each); detail shows qty, market rate, market total.
- **#18 Buy assessment** now folds in merch market value ("Merch — market now") alongside paid + RRP.
- **#1 Add-set autosaves a draft** (set number, linked buys, fetched inventory) — tab away and back without losing your place; clears on save or cancel.

## v0.25.0 — 2026-06-13

- **Built auto-marks parts.** The Built button now auto-marks every main part (spares + separators excluded) as have when you flip a set to Built — fills only the unticked ones.
- **#9 Set cards.** Dropped the 🛍️ NEW tag; condition is now a corner emoji on the set image — ✨ new · ♻️ used · 📤 part-out (part-out wins).
- **#3 Instructions.** Set how many books a set HAS (1–N), then tick the ones you own — records "has 2 books, I have neither / book 1 / both", each with its own condition. Summary reads have/total.
- **#15 Buys cards.** Merch count shown next to the set count; date moved to its own line.

## v0.24.0 — 2026-06-13

- **Per-set logging mode** (🔍 Find / ↩ Remaining) in the parts bar. *Find* = tick parts up as you find them. *Remaining* = the set is marked full, then you tick off what you pull for a build — the rest is inferred parted out, canonical count kept.
- **Remaining mode header** — segmented donut (green intact → yellow left → purple parted → red missing) + an "X/Y pcs in set · N left · N parted" counter that reads "✓ Full" when nothing's been pulled.
- **Remaining mode** — a "pieces missing from this set" stepper drives the red missing segment for incomplete donors. Switching in marks all parts present (with confirm); switching back keeps your counts.
- **MOC → Add parts** — ✓ Take all free (grab every available piece from the source set in one tap) and Clear taken (return everything pulled from that set).
- Renamed the set-detail **Accessories** group to **Instructions & Box** — it's not parts.

## v0.23.0 — 2026-06-13

- **Parts list — spare tag.** Spare parts now carry a blue SPARE tag on the colour/number line (was a faint "(spare)") — the rows that looked like duplicates are the spares. Still hidden behind the Spares chip by default.
- **Brick separators removed entirely** — they're tools, not set parts: gone from the parts list, completion %, and the value heuristic (matches 96874 / 630 / 6007 and anything named "separator").
- **Settings → Completion %** — new toggle to count spares toward % (off by default); flipping it instantly recomputes every set. Separators always excluded.
- Existing sets **reindex once on load** so the separator/spare changes apply to sets already tracked.

## v0.22.0 — 2026-06-13

- **FREEZE FIX (the MOC one).** MocDetail's add / return / main segments are now keyed — the last unkeyed segment swap (same Preact positional-diffing landmine as v0.20/v0.21). Pressing Done after adding parts left a stale invisible layer that ate every tap except the PDF link; reload was the only escape. Fixed at the root.
- **Modal sheets track the iOS keyboard** (visualViewport): the sheet stays pinned above the keyboard and the focused field scrolls into view — fixes fields scrolling out of reach when the number pad opens mid-add.
- **New MOC / New buy text fields** no longer trigger iOS "AutoFill Contact" / saved addresses (autocomplete/autocorrect off, non-contact field names).
- **Tab behaviour** — tapping the current tab scrolls it to top; tapping any tab returns to that tab's top-level list (exits an open set / buy / MOC / merch detail).

## v0.21.1 — 2026-06-12

- **FREEZE FIX.** Toggling Compact | Detailed corrupted the DOM and froze the app (same positional-diffing landmine as v0.20): set cards now carry per-density keys (full remount on toggle), the three view blocks (flat / by-buy / by-theme) got keyed wrappers, and every filter chip is keyed.
- **Set detail** — switching Used → New auto-collapses all accordions (values pre-filled); New → Used auto-expands them and offers to clear the auto-filled data.
- **Parts list** — Book & Box renamed to 📖 Accessories and is now collapsible like the colour groups (included in ⌄ All).

## v0.21.0 — 2026-06-12

- **Sets gain Condition** (🛍️ New / 🔄 Used) — auto-defaulted from the linked buy's kind (bulk → Used, receipt → New), editable in the header; switching to New offers a one-tap autofill (all parts + spares have, instructions → Book ×1 Excellent, box → Yes Excellent, triage → Keep).
- **Sets gain Built ✓**, price paid + RRP (shown when New), and a Box section (None | Yes + condition + note); status line reads parts-in-MOC → Built ✓ → complete → x/y found, in that priority.
- **Instructions relabelled** — Paper → Book, Digital → Reprint; 📖 Instructions ↗ deep link (LEGO.com) added under the set name.
- **Receipts** no longer ask for price/RRP — totals derived from linked sets' paid/RRP plus merch; buy detail $ overview includes a merch paid/RRP line.
- **Set detail reworked into accordions** (est. value, triage, instructions, box, buys & notes) — collapsed by default when New, plus a ⌄/⌃ all toggle; parts list gains a 📖 Book & Box group on top, expand/collapse-all, and a ☑ select mode.
- **Set photos** — gallery of catalog image + BrickLink box image — tap to enlarge, swipe to loop, ★ pick which shows on cards (BrickLink box image is an empirical hotlink; quietly hidden on 404).
- **Add set reordered** — mode segment → Link to buy (single collapsed row) → set number + scan + fetch.
- **Sets overview** — 🛍️ NEW tag; New sets show paid · RRP; Triage + Theme as multi-select checkboxes (Keep unticked by default); By-buy view groups by CHANNEL with a Bulk | Regular | All segment.
- **Sets overview — Compact | Detailed toggle.** Compact rows show thumb, number, name, %, triage, NEW/📖/📦 badges, with a right-edge chevron zone expanding details in place; Expand all / Collapse all chip.

## v0.20.0 — 2026-06-12

- **FREEZE FIX.** All Sets-tab segment views (Sets/Merch/MOCs) + every top-level tab view now carry explicit keys — unkeyed sibling swaps let Preact's positional diffing corrupt the DOM, leaving a stale invisible overlay that swallowed every tap after editing merch.
- **Photo viewer** — tall images (full-page screenshots) capped at 70% of viewport height so ✕, ★ and Delete stay reachable.
- **Merch gets a read-only detail page** (buy-detail pattern): condition, price, RRP + saved %, source buys, notes, photo gallery with tap-to-enlarge, ★ display-pic, multi-photo add. Merch now supports MULTIPLE photos.
- **Add set** — Quick/Bulk chips replaced by a Normal | ⚡ Quick | ↻ Bulk segmented control with a per-mode hint; fetch button label follows the mode.
- **Buy detail** — + Add set to this buy opens Add set with that buy pre-linked; saving/cancelling returns to the buy detail; Bulk mode stays for the next set.
- **All Buys** — adding a buy opens its detail page straight away.
- iOS standalone bottom gap — parked for native (noted in SPEC for SwiftUI verification).

## v0.19.0 — 2026-06-12

- **Regular buys are now receipts.** The Regular segment groups by channel (collapsible headers with receipt count + total spend); each receipt shows date, price, RRP · −% markdown, and linked-set thumbnails.
- **New-receipt form** drops the label (auto-named "Kmart — 12 Jun 2026", editable later) and gains an RRP field; buy detail shows RRP + saved %.
- **Add set** — ⚡ same-day receipt suggestion chips above the buy picker; ＋ New receipt creates one inline (kind preset to Regular).
- Bulk segment unchanged (labels, channel filter, manual drag, mass-select).

## v0.18.0 — 2026-06-12

- **Buy photos** — pick MULTIPLE photos in one go; first photo = card display pic (yellow border in the gallery); expanded view gets ✕ plus ★ Set as display pic and Delete.
- **Standalone bottom gap — final web attempt.** App height measured at runtime from window.innerHeight (CSS viewport units can be stale in standalone) and re-measured on resize/rotation; area below the app paints tab-bar colour. If a gap persists it's a WebKit standalone bug → parked for native.
- **Buys** — kind dropdown replaced by a Bulk | Regular | No Buy segment under the spend card + Add buy; channel + sort chips per segment; manual drag-reorder within a segment.

## v0.17.0 — 2026-06-12

- **MOCs — third segment on the Sets tab** (Sets | Merch | MOCs). A MOC has name, photo, notes, status (Building/Built/Disassembled) and a list of REAL parts allocated from tracked sets — grouped by source set so rebuild/teardown knows where every genuine piece goes home.
- **Transfers both ways** — + Add parts inside a MOC (pick source set → take with steppers, capped at unallocated found parts) and a → button on every set part row (pick MOC + qty, inline new-MOC create).
- **Completion untouched by transfers** — the ring stays; set cards show a ♻️ n badge, set detail shows ♻️ pcs-in-MOCs, part rows show "♻️ n → MOC name".
- **⇠ Return parts** — tick pieces back per source set as you re-bag them; when everything's home the MOC auto-flips to Disassembled.
- **PDF instructions** attachable per MOC (stored on-device; tap to save/open via iOS sheet). NOT in JSON backups — keep originals in Files.
- **Backup v5** includes MOCs (allocations + photos; PDFs excluded).

## v0.16.0 — 2026-06-12

- **Themes** — every fetched set stores its root theme (Friends, City, …) — purple badge on cards; Triage ▾ + Theme ▾ dropdowns (only themes you own, with counts); view chip cycles ≡ → ⊟ By buy → ⊞ By theme; ✕ Clear when filters active; one-tap banner backfills themes for tracked sets.
- **Instructions** — Paper now expands per-booklet rows (book #, condition, note); duplicates = two rows; set cards show 📖×n.
- **Merch** — Sets tab gains a Sets | Merch segment: track LEGO-branded boxes/storage/etc with photo, release year, RRP-then, condition, price paid, linked buy(s), notes.
- **Backup v4** includes merch.

## v0.15.0 — 2026-06-12

- **Tab-bar gap bug killed at the root.** `position:fixed` abandoned entirely (standalone iOS resolves fixed elements against a stale viewport). App is now a 100dvh flex column — scrollable content above, tab bar as a normal in-flow element below. Delete + re-add the Home-Screen icon after this deploy.
- **Add set** — entering an already-tracked number offers Open set / Dismiss instead of an error.
- **Sorting** — when no tracked set needs a part, 🔎 Find LEGO sets containing it (Rebrickable part lookup → pick colour → set list); tapping a set tracks it (linked to last bulk buy) and logs +1, or logs straight into an already-tracked set.
- **Buys** — photo gallery per buy (camera/library, downscaled ≤1024px, on-device); card thumbnails (bulk → first photo or 📦, regular → linked set image, stacked with a count badge).
- **Backup v3** includes buy photos.

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