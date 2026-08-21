# Atomic IA Inventory — Integrations

Domain: the **Integrations** tab (`help.qrtub.com`, `docs.json` tab `"Integrations"`).
Scope: everything currently in `/workspace/mintlify-docs/integrations/*.mdx`
(`safetyculture.mdx`, `cmms-systems.mdx`), plus the four reserved platform folders named in
the brief (`mitti`, `jotform`, `google-maps`, `google-sheets`).

Sources checked:
- `/workspace/mintlify-docs/integrations/safetyculture.mdx` (1,175 words) — to be split into `mitti/`
- `/workspace/mintlify-docs/integrations/cmms-systems.mdx` (522 words) — not in the reserved-folder list; sanity-checked separately
- `/workspace/mintlify-docs/app-links.mdx`, `using-fields.mdx`, `key-concepts.mdx`, `pages-overview.mdx` — the general mechanisms Integrations pages build on top of (deep links, `{{item.field}}` bindings, fallback URLs) — **not re-documented here**, only referenced
- `/workspace/qrtub/src/lib/templates/safetyculture-asset.ts`, `equipment-inspections.ts`, `all.ts` — confirms Mitti is a pass-through Destination pattern (`destination_url` with a `{{item.safetyculture_asset_id}}` token), not an API integration
- `/workspace/qrtub/src/lib/types/destination-config.ts` and everything under `src/lib/types/`, `src/app/api/` — confirms QRtub has **no native third-party API integrations at all**. Every "integration" in this domain is the same underlying mechanism (a URL, optionally templated with item fields, optionally a deep link with a fallback) pointed at a specific third-party platform's own URL scheme. There is no Jotform, Google Maps, or Google Sheets code anywhere in `src/` — those three folders are 100% reserved/未-built, confirmed by `grep -ril` across `src/`
- `/workspace/qrtub/GLOSSARY.md`, `/workspace/qrtub/BRAND.md` — terminology (Link/Destination/Item/Page, never "asset"), and the explicit claim in BRAND.md 1.3 that QRtub "links to" tools like SafetyCulture rather than replacing or exchanging data with them

## Top-line verdict on the decided structure

**The `mitti/{setup,entities,prefilling,worked-examples}.mdx` shape is well-calibrated and should ship as-is.** The existing `safetyculture.mdx` is 1,175 words — split four ways that's ~250–350 words/file after removing shared boilerplate (CTA footer, resources list), squarely inside Linear's observed 200–1,000-word band, and each of the four names maps onto genuinely distinct real content already present in the source file (see table). No change recommended for Mitti.

**Two things need a decision before the reserved folders get built** — flagged in Open Questions below, not resolved here:
1. `cmms-systems` is *not* one of the four reserved-folder platforms, and it shouldn't be forced into the same shape — it has no "entities" taxonomy (every CMMS vendor has a different one) and no "prefilling" feature (no CMMS deep link here supports field-level answer prefill the way Mitti's audit questions do). Recommend a **2-file** split instead: `cmms-systems/setup.mdx` + `cmms-systems/worked-examples.mdx`.
2. The three reserved platforms (`jotform`, `google-maps`, `google-sheets`) don't have built features yet, so nobody can confirm today whether each one actually has four distinct atomic concepts' worth of content. Best guess per platform below — Google Maps in particular looks like it may only support 1–2 files, and Google Sheets is ambiguous about which *direction* the integration even goes (see Open Questions).

---

## Inventory

### Mitti — `integrations/mitti/setup.mdx`

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| Platform rename (iAuditor → SafetyCulture → Mitti) | One product, three names; old `iauditor://` scheme and `app.safetyculture.com` URLs both still resolve, so nothing already configured breaks | Integrations > Mitti | Setting Up the Mitti Integration | `integrations/mitti/setup` | Merge — opening context for setup.mdx, ~150 words alone, not a page on its own |
| Deep-link integration model | QRtub builds a URL and hands off; no data is exchanged between QRtub and Mitti | Integrations > Mitti | (same) | (same) | Merge — one framing paragraph in setup.mdx |
| Two deep link formats (mobile scheme vs web) | `iauditor://` opens the app; `https://app.mitti.com/...` opens in a browser | Integrations > Mitti | (same) | (same) | Merge — short comparison, part of setup.mdx |
| Finding your Mitti Template ID | Where in the Mitti web app to copy the template ID you'll need before building any Destination | Integrations > Mitti | (same) | (same) | Merge — a numbered prerequisite step inside setup.mdx |
| Starting a new inspection (basic Destination) | The core recipe: `iauditor://template/new_audit/<template_id>` as a Destination URL, plus its web equivalent | Integrations > Mitti | (same) | (same) | **Standalone-caliber** — this is the anchor content of setup.mdx |
| Templating the template ID with `{{item.templateID}}` | Applying QRtub's general field-binding mechanism so different Items open different Mitti templates from one Destination config | Integrations > Mitti | (same) | (same) | Merge — a short applied example; the mechanism itself is documented in `using-fields.mdx` |
| Fallback URL / Fallback Message for Mitti | The specific recommended pairing (app link + `app.mitti.com` web fallback) for when Mitti isn't installed | Integrations > Mitti | (same) | (same) | Merge — general fallback mechanism has its own full page (`app-links.mdx`); this is just the Mitti-specific pairing |

### Mitti — `integrations/mitti/entities.mdx`

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| Entity ID lookup (beyond template ID) | Pointer to Mitti's own guide for getting inspection/asset/document IDs, the shared prerequisite for every row below | Integrations > Mitti | Mitti Entities You Can Link To | `integrations/mitti/entities` | Merge — one shared note at the top of entities.mdx |
| View inspection report deep link | `iauditor://audit/<inspection_id>` (mobile) / `https://app.mitti.com/report/audit/<inspection_id>` (web) | Integrations > Mitti | (same) | (same) | Part of the entities catalog — one row of a reference table |
| Edit existing inspection deep link | Also `iauditor://audit/<inspection_id>` on mobile, but a different web URL (`.../inspection/<id>`) than "view report" — **source currently lists the same mobile URL for both view and edit; worth confirming with Mitti's docs before publishing, it may be a copy-paste duplication in the current draft** | Integrations > Mitti | (same) | (same) | Part of the entities catalog |
| Open asset profile deep link | `iauditor://asset/profile/<asset_id>` / `https://app.mitti.com/assets/<asset_id>` — links an Item straight to its Mitti asset record | Integrations > Mitti | (same) | (same) | Part of the entities catalog |
| View document/file deep link | `https://app.mitti.com/documents/<document_id>` — web only, no mobile scheme documented | Integrations > Mitti | (same) | (same) | Part of the entities catalog |

Four reference rows plus the shared lookup note comfortably clears the atomicity bar as one page; none of the four is independently worth 150+ words on its own, which is exactly why they belong together as one "here's everything else you can deep-link to" page rather than one page per entity type.

### Mitti — `integrations/mitti/prefilling.mdx`

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| Pre-filling a single inspection question | Appending `?<question_item_id>=<response>` to the new-audit deep link answers that question automatically when the inspection opens | Integrations > Mitti | Pre-Filling Mitti Inspections | `integrations/mitti/prefilling` | **Standalone-caliber** — the anchor content of this page |
| Pre-filling multiple questions | Chaining additional `&<question_item_id>=<response>` pairs | Integrations > Mitti | (same) | (same) | Merge — direct extension of the row above, same page |
| Question item ID vs an arbitrary field name | You need Mitti's own internal question ID from the template, not a name you invent — with a pointer to Mitti's entity ID guide | Integrations > Mitti | (same) | (same) | Merge — a clarifying note inside prefilling.mdx |
| No URL-encoding of field values | QRtub inserts `{{item.field}}` values raw, unencoded; a value containing a space, `&`, or `#` breaks the deep link, so it must be pre-encoded in the Item field itself | Integrations > Mitti | (same) | (same) | Merge here, but **see Open Questions** — this is a QRtub-wide binding limitation, not Mitti-specific, and repeating it on every integration page risks drift if the platform behavior ever changes |
| Deep link length ceiling (~2,000 characters) | Practical limit on how many questions can be pre-filled before very long links stop working reliably | Integrations > Mitti | (same) | (same) | Merge — a troubleshooting-flavored footnote on prefilling.mdx |

### Mitti — `integrations/mitti/worked-examples.mdx`

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| Use case: equipment inspections | Excavators, forklifts, vehicles — pre-start checklists and daily safety inspections | Integrations > Mitti | Mitti Worked Examples | `integrations/mitti/worked-examples` | Merge — one short example among several |
| Use case: facility inspections | Fire extinguishers, emergency equipment, building systems | Integrations > Mitti | (same) | (same) | Merge — pairs with the row above |
| Worked example: multi-audience Page | One Page, three Destinations, each a different Mitti deep link for a different role (operator/manager/maintenance) | Integrations > Mitti | (same) | (same) | **Standalone-caliber** — the flagship worked example on this page |
| Troubleshooting: mobile app doesn't open | Checklist: app installed, template ID correct, user has template access | Integrations > Mitti | (same) | (same) | Merge — troubleshooting subsection |
| Troubleshooting: web link doesn't work | Checklist: logged into Mitti web app, has permission on the entity, entity ID correct | Integrations > Mitti | (same) | (same) | Merge |
| Troubleshooting: data not pre-filling | Checklist: correct question item ID, Item field actually populated, special characters encoded, link under the length ceiling — cross-links to prefilling.mdx | Integrations > Mitti | (same) | (same) | Merge |
| Troubleshooting: users lack Mitti permissions | Deep links only work if the scanning user has access to that entity inside Mitti itself | Integrations > Mitti | (same) | (same) | Merge |

### CMMS Systems — sanity-checked, **not** one of the four reserved platforms

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| CMMS overview (what these platforms are, why connect) | UpKeep/Fiix/etc. track maintenance schedules, work orders, and asset history; QRtub connects Items to their records | Integrations > CMMS Systems | Setting Up a CMMS Integration | `integrations/cmms-systems/setup` | Merge — short framing paragraph |
| Three integration approaches (deep link / web URL / URL Template) | The decision tree: does the vendor support a deep link, is there just a web URL, and how a QRtub URL Template automates picking the right one per Item | Integrations > CMMS Systems | (same) | (same) | **Standalone-caliber** — the anchor content of setup.mdx |
| Field mapping best practice | Keep the Item's asset-ID field in the same format as the CMMS's own asset identifiers | Integrations > CMMS Systems | (same) | (same) | Merge — a short tip |
| Vendor lock-in protection (value prop) | If you switch CMMS vendors, update the Destination in QRtub instead of reprinting codes | Integrations > CMMS Systems | (same) | (same) | Merge — one line in the overview, not technical content |
| Worked example: UpKeep three-destination setup | View Asset Record / Create Work Order / View Maintenance History, all built from one `{{item.assetID}}` template | Integrations > CMMS Systems | CMMS Worked Examples | `integrations/cmms-systems/worked-examples` | **Standalone-caliber** — the anchor content of worked-examples.mdx |
| Worked example: multi-system Page (CMMS + Mitti + docs) | One Page combining "Start Inspection" (Mitti), "View Maintenance" (CMMS), "Log Issue", "Equipment Manual" | Integrations > CMMS Systems | (same) | (same) | Merge — one example among several |
| Common CMMS platforms reference table | UpKeep / Fiix / Maintenance Connection / Hippo CMMS / eMaint, with web-access and deep-link-support notes | Integrations > CMMS Systems | (same) | (same) | Merge — a reference table, not its own page |
| Troubleshooting: "page not found" / asset ID mismatch | Checklist for the most common CMMS setup failure | Integrations > CMMS Systems | (same) | (same) | Merge |
| Troubleshooting: authentication / session / SSO | Checklist: user logged in, session tokens, SSO if supported | Integrations > CMMS Systems | (same) | (same) | Merge |

### Reserved platforms — content-shape sanity check only (no shipped feature to inventory yet)

| Concept | Definition | Nav category | Page title | Slug | Atomic or merge |
|---|---|---|---|---|---|
| Jotform: pre-fill form fields via URL parameters | Jotform natively supports `?fieldName=value` prefill — the same shape as Mitti's question-prefill, likely the meatiest of Jotform's four files | Integrations > Jotform (reserved) | Pre-Filling Jotform Forms | `integrations/jotform/prefilling` | Likely **standalone-caliber** once built — mirrors Mitti's prefilling.mdx |
| Jotform: "entities" (new submission vs view/edit existing) | Unclear whether Jotform gives QRtub anything resembling Mitti's template/inspection/asset/document taxonomy, or just "the form" | Integrations > Jotform (reserved) | Jotform Entities | `integrations/jotform/entities` | **Flag** — may be too thin to justify its own file; could need to merge into setup.mdx until the real feature is scoped |
| Google Maps: open a location from Item coordinates | Building a Maps URL (`?q=lat,lng` or place ID) from an Item's location field(s) | Integrations > Google Maps (reserved) | Setting Up Google Maps Links | `integrations/google-maps/setup` | Likely **the entire integration** — Google Maps has no "entities" beyond a pin and no field-level "prefilling" concept at all |
| Google Sheets: direction of the integration is undecided | Could mean "deep-link a scan into a specific row/view of a Sheet" (matches the other three platforms' shape) *or* "export/sync Item data into a Sheet" (a completely different, data-flow feature, arguably belonging under Items & Data, not Integrations) | Integrations > Google Sheets (reserved) | *(undetermined)* | `integrations/google-sheets/*` | **Flag — do not build until scoped.** The four-file shape assumes the deep-link-recipe pattern; if it's actually the export/sync direction, this platform doesn't belong in this nested-folder shape at all |
| Integrations tab orientation page | A short page stating the shared mechanism up front — every integration here is a URL/deep-link recipe, not a native API connection — and pointing to `using-fields.mdx` / `app-links.mdx` for the mechanism itself | Integrations (tab root) | How Integrations Work | `integrations/index` (or `integrations/overview`) | **Recommend adding** — not in the current file tree at all. Matches Linear's own small "Concepts" orientation page sitting alongside the deep-dive groups; right now a first-time reader lands directly in `mitti/setup.mdx` with no signpost that Jotform/Maps/Sheets exist or that the underlying mechanism is documented elsewhere |

---

## Notes

- **Redirect needed.** `docs.json` currently has no redirect for `/integrations/safetyculture`, and the file is being replaced by four new URLs under `integrations/mitti/`. Add a redirect (`/integrations/safetyculture` → `/integrations/mitti/setup`) in the same PR that removes the old file, following the existing pattern already used for the `/help/*` → root moves.
- **Possible source inconsistency to verify before publishing `entities.mdx`:** the current `safetyculture.mdx` lists the identical mobile URL (`iauditor://audit/<inspection_id>`) for both "View Inspection Report" and "Edit Existing Inspection." That may be correct (Mitti's mobile app might not distinguish view/edit at the URL level) or may be a copy-paste artifact — worth a quick check against Mitti's own deep-link docs (linked at the bottom of the current page) during drafting.
- **Duplication to trim, not carry forward:** both `safetyculture.mdx` and `cmms-systems.mdx` currently re-explain the general `{{item.field}}` URL Template mechanism from scratch. `using-fields.mdx` (Help tab) already owns that content in full; the Integrations pages should show a one-line applied example and link out, not re-teach the mechanism. Same applies to the "no URL-encoding" caveat (see Open Questions).
- **Terminology check:** all Mitti content correctly uses "Link," "Destination," "Item," and "Page" per `GLOSSARY.md`, and never "asset" for the QRtub side (the word "asset" only appears correctly, referring to the *third-party* platform's own object). CMMS content is consistent too. No glossary violations found in either source file.

## Open Questions

1. **CMMS folder shape.** The brief names four reserved folders (`mitti`, `jotform`, `google-maps`, `google-sheets`) but says nothing about `cmms-systems`. Should it get the same nested-folder treatment (and if so, 2 files as recommended above, or something else), or does it deliberately stay a single flat page because it's a multi-vendor generic pattern rather than one named platform? This needs an explicit decision, not an inference.
2. **Jotform's real shape is unknown.** Nobody has scoped what a QRtub↔Jotform integration actually supports yet. If it turns out Jotform only really offers "open this form, pre-filled" with no separate entity types, `entities.mdx` may need to fold into `setup.mdx`, leaving Jotform with 3 files instead of 4. Should the four-file shape be treated as a firm target to design the feature toward, or as a consequence that falls out of whatever the feature ends up supporting?
3. **Google Maps likely doesn't need 4 files.** There's no field-level "prefilling" concept and no meaningful "entities" catalog beyond a single pin/location — the entire integration may be one `setup.mdx` plus one `worked-examples.mdx`. Confirm whether the reserved-folder shape is meant literally (always exactly 4 files per platform) or as a ceiling/template to trim from.
4. **Google Sheets — biggest open item.** "Google Sheets integration" could mean two unrelated things: (a) a scan deep-links into a specific Sheet/row, matching the pattern of the other three platforms, or (b) Item data exports/syncs into a Sheet, which is a data-flow feature with nothing to do with deep links and would more naturally sit under an "Items & Data" or "Import/Export" category rather than Integrations. This needs a product decision before any IA work on that folder is meaningful.
5. **Where does the system-wide "no URL-encoding" caveat live?** It currently only appears inside Mitti's prefilling content but is a property of every `{{item.field}}` binding QRtub has, not a Mitti-specific behavior. Should it move to `using-fields.mdx` as the canonical explanation, with every integration page (Mitti prefilling, and eventually Jotform prefilling) reduced to a one-line cross-reference?
6. **Is the proposed `integrations/index.mdx` orientation page wanted at all?** It's not in the currently decided structure. Given Linear's own comparably tiny "Concepts" page sitting next to its deep-dive groups, it seems like a natural fit, but this domain's brief was explicitly "confirm and finalize," so this is being flagged rather than assumed.
