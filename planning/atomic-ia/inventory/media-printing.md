# Concept Inventory — Media and Printing

Domain owner scope: what Media is (the physical material), Print Batches (creation,
lifecycle, per-code status, archiving), the Print-First Workflow (ordering before assets
exist, in-app scanner, Claim-on-scan), and Working with a Print Shop (VDP vs. gang-sheet
production, job prep).

**Sources verified against, not memory:**
- App source: `src/lib/database/server-print-batches.ts`, `src/lib/constants/batch-status.ts`,
  `src/lib/hooks/useBatches.ts`, `useActiveBatchOps.ts`, `useTubBatchOperations.tsx`,
  `usePrintBatchStatus.ts`, `src/app/app/media/page.tsx` + `batch-columns.ts`,
  `src/components/blocks/PrintHistoryPanel/PrintHistoryPanel.tsx` (1751 lines — the batch
  detail panel, deployment tracker, links/columns editors),
  `src/components/blocks/PrintListModal/PrintListModal.tsx`,
  `src/components/blocks/BatchPickerModal/BatchPickerModal.tsx`,
  `src/components/blocks/ScanQrModal/ScanQrModal.tsx`, `src/lib/utils/scan-code.ts`,
  `src/lib/utils/qr-code.ts`, `src/app/api/access-links/resolve/route.ts`,
  `src/lib/page/public-link-page.tsx`, `src/app/app/access-link/page.tsx` (bulk QR
  download), `src/lib/database/server-templates.ts` (confirmed `media_batches` /
  `templates` table relation is dead code behind the unrelated Collection-creation "templates"
  feature — not a real Media feature), `dev-docs/print-tracking.md` (status header only —
  the body is a pre-build pitch, not current behaviour).
- `GLOSSARY.md`, `BRAND.md` (entity model, feature-status table, "Claims That Are FALSE").
- Existing docs read in full: `media-basics.mdx`, `print-batches.mdx`,
  `print-first-workflow.mdx`, `print-shop-basics.mdx`, `preparing-print-job.mdx`,
  `unallocated-links.mdx` (adjacent, primarily a Links-domain page).

---

## Critical finding: two drafted pages describe an unshipped feature as if it exists

`print-shop-basics.mdx` and `preparing-print-job.mdx` both carry a `DRAFT` HTML comment
saying they were "written ahead of the SVG export feature" and must not go into nav until
it ships. I verified this against the source: there is **no SVG export and no
error-correction-level control anywhere in the codebase**
(`grep` for `errorCorrection`/`error_correction`/svg export in `src/` returns nothing; the
only match is the planning brief itself, `../qrtub-ops/briefs/qr-format-and-error-correction.md`).

What actually ships today (`src/lib/utils/qr-code.ts`, `src/app/app/access-link/page.tsx`):
- QR download is **PNG only**, fixed 1024px width / 2-module margin, default (library)
  error correction — no format choice, no level choice.
- Single-link download filename: `qr-code-{slug}.png`. Bulk download is a ZIP of the same
  pattern, one PNG per link, named `qr-code-{slug}.png` — **not** `{slug}.svg` as
  `preparing-print-job.mdx` currently states, and not a bare-slug filename either.
- The CSV print list ("Print List" export from a Collection or the Access Links page) **does**
  exist today and **does** carry a Full URL / Short URL column that contains the same slug
  — so "match the QR filename's slug against the CSV's URL column" is directionally right,
  it's just the filename pattern in the current draft that's wrong.

This means the drafted "QR Code Export Formats & Error Correction Level" content (row #29
below) describes a **planned** feature and must stay out of nav / be rewritten around the
PNG-only reality until that brief ships — mirrored by a **new**, accurate atomic page for
what genuinely exists today (row #12b). Flagging this prominently because it's exactly the
kind of drift `mintlify-docs/CLAUDE.md` calls out as the recurring failure mode for this
site.

---

## Category: Media

| # | Concept | Definition | Nav category | Page title | Slug | Atomic? |
|---|---|---|---|---|---|---|
| 1 | What Media is | The physical material a QR code is displayed on — sticker, plaque, sign, billboard, NFC chip — as a third entity distinct from the Link (digital pattern) and the Item (thing represented). | Media | What Is Media? | `media/what-is-media` | **Standalone.** Short orientation page, like Linear's "Concepts" — the three-entity distinction plus a one-line pointer to what's actually tracked (batches) vs. not (per-item Media). ~200-250 words of real, non-duplicated content. |
| 2 | Media types & materials | Reference table of common physical Media types (vinyl stickers, metal plaques, rigid signs, billboards, real-estate signs, printed ads, NFC chips) with typical cost, durability and use case, plus how to choose by environment/duration/budget. | Media | Choosing Media for Your Deployment | `media/media-types` | **Standalone as one merged page.** Each individual type (e.g. "Vinyl Stickers") is only 20-30 words alone — too thin to stand on its own — but the full comparison + selection guidance is real, distinct content worth one page. Note: these cost/durability figures are generic industry reference, not app behaviour, so they don't need source verification the way a feature claim would — but they should be clearly framed as guidance, not a QRtub-tracked fact. |
| 3 | What's not tracked yet (Media roadmap) | Per-item Media (material, cost, durability, install location), Media Templates, Media Partners, and cost/inventory reporting are all Planned, not built — only production-run (batch) tracking exists today. | Media | *(section, not a page)* | — | **Merge into #1.** A bullet list of "not yet" items reads as one short callout inside the overview, not 150+ words of distinct mechanism — matches BRAND.md's feature-status table, doesn't need its own URL. |

## Category: Print Batches

| # | Concept | Definition | Nav category | Page title | Slug | Atomic? |
|---|---|---|---|---|---|---|
| 4 | Print Batches overview | A batch is the record of one CSV print-list export: which links were in it, how far along it is, and what's actually been installed. Exists so months later you can answer "what was in that order, and where did it go?" | Print Batches | Print Batches | `print-batches/overview` | **Standalone**, short — orientation for the category, the way Linear's own "Concepts" sits above "Priority", "Due dates", etc. |
| 5 | Creating a batch | Select Items in a Collection (or links on the Access Links page) → **Print List** → pick columns via the column picker → export. The export both downloads a CSV and creates a batch in Draft, unless you use the "download CSV only, don't track" toggle. | Print Batches | Creating a Print Batch | `print-batches/creating-a-batch` | **Standalone.** Real, distinct procedure (`PrintListModal`, `createBatchWithCsv`), ~250-350 words. |
| 6 | Batch status lifecycle | Draft → Printing → Printed → Deployed. Each forward step has a named action ("Finalise and print", "Mark as printed", "Move to deployment"); you can step back one stage except out of Deployed, which is terminal. | Print Batches | Batch Status: Draft to Deployed | `print-batches/batch-status` | **Standalone.** Rich enough on its own (`BATCH_STATUS_TRANSITIONS`/`_REVERSE`, per-status UI actions) — directly analogous to Linear's "Issue status" page. |
| 7 | Archiving & deleting a batch | Archive/unarchive hides a completed batch from the default view without deleting it. Hard delete only works on a **Draft** batch (and removes its stored CSV/image); anything past Draft can't be deleted — archive it instead — because the links may already be printed and stuck to something. | Print Batches | Archiving and Deleting Batches | `print-batches/archiving-and-deleting` | **Standalone, merged.** Mirrors Linear's own "Delete and archive issues" — one page, not two-plus-a-caveat-page. Folds in *why* deletion is blocked past Draft (`ProtectedBatchError`) rather than splitting that into its own page. |
| 8 | Batch details: name, notes, tags, photo | Inline rename; a notes textarea; a tag-chip input (Enter or trailing comma commits a tag, Backspace on an empty input removes the last one); a cover photo of the finished media via file upload. | Print Batches | Naming, Tagging and Photographing a Batch | `print-batches/batch-details` | **Standalone.** Concrete, distinct UI mechanism (`BatchDetailView`'s tag/notes/image handlers) — comparable in scope to Linear's "Priority" (243 words). |
| 9 | Editing included columns / regenerating the CSV | While a batch is still Draft, its item/link column selection can be changed (reusing the same column picker used at export); saving regenerates the stored CSV to match. Locked once the batch leaves Draft. | Print Batches | Editing a Batch's Columns | `print-batches/editing-columns` | **Standalone.** Distinct, narrow mechanism (`updateColumnConfig` + `regenerateCsv`), draft-only constraint worth stating explicitly. |
| 10 | Adding & removing links in a batch | From inside a batch (current/add tabs) or from the Items/Access Links grid ("Add to batch"), links can be added to or removed from a **Draft** batch one at a time, in bulk by selection, or in bulk by scope (e.g. "every link in this Collection"). Locked once past Draft — the UI shows "Batch is printed — links are locked". | Print Batches | Adding and Removing Links in a Batch | `print-batches/editing-links` | **Standalone.** One coherent mechanism with two entry points (`BatchLinksEditor` + `BatchPickerModal`/`useActiveBatchOps`) — worth unifying into one page rather than two, since it's the same rule (draft-only) told from two doors. |
| 11 | Per-code deployment status | Independent of the batch's own status, each link inside a batch carries its own **Printed / Deployed / Retired** state. A segmented bar shows the mix, items are filterable by slug or item name, and status can be bulk-set ("mark all deployed", "retire all") or changed one at a time. The tracker itself only appears once the batch reaches Deployed. | Print Batches | Tracking Deployment Status Per Code | `print-batches/deployment-status` | **Standalone.** A full, narrow mechanism (`DeploymentTracker`, `getDeploymentStatusCounts`, `bulkUpdateDeploymentStatus`) — this is the page that answers "which of the 500 stickers never left the box," directly comparable to Linear's "SLAs" (994 words) in richness. |
| 12 | The stored CSV: downloading & reprinting | The exact CSV that was exported is kept with the batch (not regenerated on the fly, except when columns are edited in Draft), so a reprint uses precisely the same list. Download streams server-side rather than via a signed URL (correcting the original pre-build pitch in `dev-docs/print-tracking.md`, which claimed 1-hour signed links). A "Share with print partner" shortcut appears while a batch is in Printing. | Print Batches | The Batch CSV: Downloading and Reprinting | `print-batches/csv-download` | **Standalone.** |
| 12b | Downloading QR code images (PNG) | Each link's QR code can be downloaded individually (PNG, `qr-code-{slug}.png`) or in bulk as a ZIP of the same pattern, from the Access Links page. Fixed 1024px / 2-module margin, default error correction — no format or level choice today. | Print Batches *(or Working with a Print Shop — see open question)* | Downloading QR Code Images | `print-batches/downloading-qr-images` | **Standalone — and net-new.** Nothing in the current docs accurately describes this shipped capability; `preparing-print-job.mdx` describes a different, unshipped SVG/error-correction version of it. This page should state today's PNG-only reality plainly. |
| 13 | Finding & filtering the batch list | The **Access Media** page: four summary cards (Total Batches, Total Links, Allocated, Unallocated), a sortable/filterable/searchable table, a "show archived" toggle, and per-user column visibility preferences (persisted to `localStorage`). | Print Batches | Finding and Filtering Your Batches | `print-batches/finding-batches` | **Standalone.** Also absorbs the smaller "filter a Collection's Items by the batch they were printed in" behaviour (the `batch_id` filter badge linking out to the Items/Access Links grids) — that's a one-sentence fact, too thin to be its own page. |
| 14 | Print history for a link | From a specific access link/QR code, see every batch it has ever appeared in (`getBatchesForAccessUrl`), with a re-download of each historic CSV. Surfaces on the Access Links grid too, as a compact "Printed 2× · Last: Mar 9" summary per row. | Print Batches | Viewing a Link's Print History | `print-batches/link-print-history` | **Standalone.** Different audience/entry point than the batch-centric pages above (per-code forensics vs. per-batch management) — worth its own page rather than a subsection. |

## Category: Print-First Workflow

| # | Concept | Definition | Nav category | Page title | Slug | Atomic? |
|---|---|---|---|---|---|---|
| 15 | The print-first workflow (overview) | Durable Media is made in runs with lead times and minimums, so codes get generated and printed before the Items they'll represent exist. Four steps: generate Links, send the batch to be produced, apply tags as gear arrives, connect when ready. | Print-First Workflow | The Print-First Workflow | `print-first/overview` | **Standalone**, kept as the one broader narrative/orientation page in this category — not asked to split further, and its ~960 words earn their keep as connective framing rather than one flat concept. |
| 16 | The in-app QR scanner | Opened from the top navbar's scanner icon: a camera view (via `@yudiel/react-qr-scanner`) that decodes a QR live, plus a text field that doubles as a manual-paste box and as the target for a USB "gun" (HID) barcode scanner. A resolved code offers "Go to destination" or "Open access link." | Print-First Workflow | The In-App QR Scanner | `print-first/scanner` | **Standalone — and net-new.** Not documented anywhere in the current site. Distinct mechanism (camera + manual + HID input converging on one resolver) worth its own page. |
| 17 | Claim-on-scan: adopting an unknown code | When a scanned code doesn't resolve to anything QRtub already knows, the scanner offers "Create & open": it mints a new random Link bound to a hash of the scanned text (idempotent — scanning the same physical code again returns the same Link, never a duplicate), optionally attaching a low-res reference photo captured from the live camera frame at scan time. | Print-First Workflow | Claim-on-Scan: Adopting an Unknown Code | `print-first/claim-on-scan` | **Standalone.** Named directly in `GLOSSARY.md` ("Claim-on-scan") and has real, specific mechanics (`scanHash`, `scanValue`, `scanPhotoUrl`, idempotency) — a clear atomic page, not a subsection of the scanner page, because the *naming/ownership* mechanic is conceptually distinct from the *scanning* mechanic. |
| 18 | Tips for print-first deployments | Print the Link as readable text alongside the code (for scratched/muddy/low-light scans); make the tag number match the Link slug; pick media grade by environment, not budget, since several grades can share one numbering scheme; order more than you need since a second run is costlier than spare capacity. | Print-First Workflow | Tips for Print-First Deployments | `print-first/tips` | **Standalone as one merged page.** Each individual tip is a single sentence — too thin alone — but the set together is real, practical, and doesn't belong inside the overview page. |
| 19 | What happens if a tag is scanned before it's connected | An unconnected code doesn't 404: a team member scanning it gets an on-the-spot "assign it" option (via the scanner's resolve/claim flow, or the public unallocated-link page's assign prompt), while a member of the public sees a neutral branded page. | *(cross-domain — see open question)* | — | **Likely merge, into the existing `unallocated-links.mdx`.** That page already exists and covers this fact (`renderPublicLinkPage`'s `UnallocatedLinkPage` branch) from the Link's-eye view. Duplicating it inside Print-First would fragment one behaviour across two domains. Flagged for the drafting phase to confirm ownership rather than assigning a slug here. |
| 20 | Fixing print-first mistakes (reassignment, release-on-delete) | A Link can be reassigned to a different Item at any time (a mistake costs an edit, not a reprint); deleting an Item releases its Link back to the unassigned pool instead of deleting the Link, so an already-applied tag keeps working. | *(cross-domain — Links)* | — | **Out of this domain's scope to slug.** This is core Link-lifecycle behaviour (`unassignByThingIds`, the release-not-delete policy in `CLAUDE.md`'s "Link lifecycle" section) that almost certainly belongs to a Links/Items domain inventory, not Media and Printing, even though `print-first-workflow.mdx` currently narrates it. Listed here only so the drafting phase doesn't lose the fact when the current page gets split. |
| 21 | URL templates for print-first tags | One destination template (`{{item.serial_number}}`) configured once resolves every tag in a run to its own Item's data. | *(out of scope)* | — | **Already covered — do not duplicate.** This is `using-fields.mdx` / field-binding territory (`src/lib/page/bindings.ts`), already an existing page per the repo listing. |

## Category: Working with a Print Shop

| # | Concept | Definition | Nav category | Page title | Slug | Atomic? |
|---|---|---|---|---|---|---|
| 22 | Choosing your print production method | A one-question orientation: does the shop's equipment merge live per piece, or does it need one flattened file laid out in advance? Routes to the two flow pages below. | Working with a Print Shop | Choosing Your Print Production Method | `print-shop/choosing-a-method` | **Standalone, deliberately thin.** This *replaces* the current `print-shop-basics.mdx` once VDP and gang-sheet content move out — an orientation page in the Linear "Concepts" mould, not a place for the real depth. |
| 23 | Variable data printing (VDP) | Digital presses image each piece individually, pulling a different value from a data file live as the run goes. A template (fixed design + marked variable region) merges with a data file (one row per piece) at print time. Suits stickers, labels, vinyl at volume. | Working with a Print Shop | Variable Data Printing (VDP) | `print-shop/vdp` | **Standalone.** Split out of `print-shop-basics.mdx` (978 words total, at the very top of Linear's observed range, covering two genuinely separate mechanisms plus a glossary — this is the first of the two). |
| 24 | Gang sheets & composite-sheet production | Materials that can only reproduce one static image per pass (photo-anodized aluminium, laser-engraved metal, UV-printed rigid panels, screen-printed boards) need every unique code already laid out in its final position in one file *before* that single pass runs — a "gang sheet" or "nesting." Cutting/routing/engraving happens afterward, using registration marks. Built in ordinary design tools (InDesign Data Merge's "Multiple Records" mode, or a VDP shop's "N-up"/imposition step) — the merge still happens, just once, ahead of time. Bounded by physical sheet/bed size. Laser engraving is a documented exception that can run either way. | Working with a Print Shop | Gang Sheets and Composite-Sheet Production | `print-shop/gang-sheets` | **Standalone.** The second flow split out of `print-shop-basics.mdx` — meaty enough alone (worked example: photo-anodized aluminium's "imaged first" convention) to clear the bar without the VDP content propping it up. |
| 25 | Print-shop vocabulary | Variable data printing, static element, variable field, data source/file, merge, gang sheet/nesting, fabrication/route-and-cut — currently a standalone glossary list at the bottom of `print-shop-basics.mdx`. | *(no separate page)* | — | **Merge — fold each term into whichever of #23/#24 it belongs to.** Every term is already explained in-line where it's actually used; a separate glossary page would just repeat definitions that live better next to their mechanism, and the list alone doesn't reach 150 words of *distinct* content once the duplicated definitions are removed. |
| 26 | Preparing your print job (overview) | What a shop needs from you, at a glance: a spec, a data file, the codes themselves, and a way to match one to the other — with a proof before the full run. | Working with a Print Shop | Preparing Your Print Job | `print-shop/preparing-your-job` | **Standalone, deliberately thin.** Replaces the current 949-word `preparing-print-job.mdx` as a short checklist/router once the five concepts below move out — same "orientation, not depth" pattern as #22. |
| 27 | QR code print spec: quiet zone & minimum size | A QR code needs a quiet zone — blank space at least 4 modules wide on all sides, nothing (logo, border, mounting hole, laminate edge) crossing into it — plus a practical minimum printed size (~20-25mm / 1") to stay reliably scannable by a phone camera. This is what to mark on a design spec for a shop redrawing your artwork. | Working with a Print Shop | QR Code Print Spec: Quiet Zone and Minimum Size | `print-shop/quiet-zone-and-size` | **Standalone.** Narrow, verifiable technical fact — comparable to Linear's "Priority" in scope. |
| 28 | Matching QR codes to data rows | Today, matching a downloaded QR image to its CSV row is manual, by slug. **Needs correction before drafting**: the current draft claims a code at `qrtub.com/r/aBc12` downloads as `aBc12.svg`; the actual shipped filename is `qr-code-aBc12.png` (see the critical finding above). If the shop's software can render the code itself from the Full URL column, the image files aren't needed at all. | Working with a Print Shop | Matching QR Codes to Data Rows | `print-shop/matching-codes-to-rows` | **Standalone**, once the filename/format claim is corrected against `qr-code.ts` and `access-link/page.tsx`. |
| 29 | QR code export formats & error correction level (PLANNED) | SVG vs. PNG choice, and a selectable M/Q/H error-correction level, so a code holds up under lamination/scratching. **Not shipped** — see the critical finding above; this describes `../qrtub-ops/briefs/qr-format-and-error-correction.md`, not current behaviour. | *(hold out of nav)* | QR Code Export Formats and Error Correction *(draft only, do not publish)* | `print-shop/export-formats-and-error-correction` | **Standalone once built, but not before.** Keep the row so the concept isn't lost, but this must not reach `docs.json` until the feature ships — exactly the guard the existing `DRAFT` comment already asks for. Today's real, shippable equivalent is row #12b (PNG-only download) — write and publish that one now instead. |
| 30 | What to expect when a shop redraws your file | Shops typically rebuild your design in their own software rather than use your file as-is — converting colours to their press and regenerating the QR itself from your data rather than trusting a pasted-in image. Not a sign of a problem. Illustrated with a worked customer example (plaque design → shop rebuild → proof → scan-test). | Working with a Print Shop | What to Expect When a Shop Redraws Your File | `print-shop/shop-redraws-your-file` | **Standalone.** Real, distinct expectation-setting content (~200 words) that's easy to lose if merged into the spec or data-file pages. |
| 31 | Getting and scanning a proof | Ask for a proof of a handful of real records (not just the blank template) before the full run; confirm the right destination lands on the right piece; actually scan the codes at real size/quiet-zone, not just look at them. Cheap to catch before printing, not after. | Working with a Print Shop | Getting and Scanning a Proof | `print-shop/getting-a-proof` | **Standalone.** Narrow, actionable, and general enough to apply to both VDP and gang-sheet jobs — belongs on its own rather than duplicated inside each flow page. |
| 32 | Exporting the data file for a shop | Export a print list (CSV) with at minimum the Full URL column, plus whatever Item fields the shop should print alongside each code; "one row = one physical piece" is the governing convention. | *(merge — see below)* | — | **Merge into "Creating a Print Batch" (#5).** This is the same export mechanic already covered there; the only shop-specific addition is "make sure Full URL is included" and the one-row-per-piece framing, which reads as a sentence inside #5 rather than a duplicate page about the same CSV export. |

---

## Notes for the drafting phase

**On `print-shop-basics.mdx` and `preparing-print-job.mdx` specifically** (the direct
question asked of this domain): **no, neither is atomic enough as currently drafted.**
Both sit at 949-978 words — the very top of the Linear-observed 200-1000 word range — and
each visibly bundles multiple independent concepts under one title:
- `print-shop-basics.mdx` = VDP (its own mechanism) + gang-sheet/composite production (a
  materially different mechanism, with its own worked example) + a decision heuristic +
  a term glossary. That's at least three atomic pages' worth of content wearing one page's
  title.
- `preparing-print-job.mdx` = a print spec (quiet zone/size) + a data-file export procedure
  (which duplicates content that belongs with batch creation) + an image-export procedure
  (currently describing an unshipped feature) + a matching convention (currently containing
  a factual error about the filename format) + an expectation-setting narrative about shop
  redraws + proofing advice. Six distinct things.

Recommended shape: turn both into short orientation/router pages (rows #22 and #26) — the
same role Linear's "Concepts" page plays above "Priority", "Due dates", etc. — and let the
real depth live in the split-out atomic pages (#23-25, #27-32). This also happens to be the
only way to correct the unshipped-feature and filename-format problems without leaving them
buried inside a page that's also trying to do five other things.

**On the `templates` / `media_batches` database relation**: `server-templates.ts` exposes
`getWithMediaBatches`, and a `templates` table joins to `media_batches` /
`media_batch_access_urls` in the schema. I traced every caller: the only UI touching
`ServerTemplatesService` is `/app/app/templates` and `/app/app/tub/create`, which are the
**Collection-creation template gallery** (`FIELD_CONFIG_TEMPLATES`) — nothing to do with Media.
`getWithMediaBatches` itself has zero callers anywhere in `src/`. Treat "Media Templates"
as genuinely unbuilt (matching BRAND.md's "Planned" status), not as a feature with a
half-finished UI waiting to be documented.

**On the `access_link_print_batches` vs. `media_batches` naming collision**: `CLAUDE.md`'s
own architecture section names both tables in the same breath ("Tables: `templates` →
`media_batches` → `media_batch_access_urls` for batch generation; `access_link_print_batches`
/ `access_link_print_events` for what was actually printed and deployed"), which reads as if
two live systems exist. Only the second is real and reachable from the UI. Worth a one-line
callout wherever this domain's overview page is drafted, so a future reader of the source
doesn't get misled the way I initially was.

## Open questions for drafting

1. **Where does "what happens if a tag is scanned before it's connected" (#19) live?**
   `unallocated-links.mdx` already exists and covers it from the Link's point of view; the
   Print-First Workflow's own narrative wants to state it too, from the deployment point of
   view. Recommend the Print-First overview page (#15) link to the existing page rather than
   restate it, but the Links-domain inventory owner should confirm they're treating it as
   canonically theirs.
2. **Link reassignment and release-on-delete (#20)** currently reads as part of the
   print-first story but is core Link-entity behaviour. Confirm with whoever owns the
   Links/Items domain inventory that it's captured there, so it isn't silently dropped when
   `print-first-workflow.mdx` gets split.
3. **Does "Downloading QR Code Images" (#12b) belong under Print Batches or under Working
   with a Print Shop?** It's reachable from the Access Links page (not the Media/batch UI),
   but its audience is almost always "getting codes to a print shop." Placed it under Print
   Batches provisionally since the mechanism lives next to the CSV export it's usually
   paired with, but a case exists either way.
4. **Should "Archiving & Deleting Batches" (#7) instead split into two** if the drafted page
   turns out to run long once both halves are written out in full? Flagged as one page now
   on the strength of the Linear "Delete and archive issues" precedent, but that precedent
   is for *issues*, not batches with a hard-delete/soft-archive split this material — worth
   rechecking word count once drafted.
5. **Media types & materials (#2)**: the cost/durability figures in the current
   `media-basics.mdx` are generic industry reference, not sourced from QRtub's own code or
   from any customer-verified numbers. Confirm before publishing whether this content is in
   scope for a *product* help site at all, or whether it reads better as marketing/industry
   content — it doesn't fail the "document the code" rule (it isn't a capability claim), but
   it's a different register from the rest of this domain.
