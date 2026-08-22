# Concept inventory — Collections

Domain scope (per brief): the Collection entity itself and every Collection-*level* setting — link-generation
mode, the item-ID mask builder, page-template defaults, the creation-time starter-template
gallery, custom-field SCHEMA ownership, whole-Collection JSON backup/restore, and Collection deletion.

## Sources checked

- `qrtub/src/lib/database/server-tubs.ts`, `types.ts` (the `tubs` table service)
- `qrtub/src/lib/types/tub-metadata-defaults.ts`, `link-generation-config.ts`,
  `landing-page-config.ts`, `field-config.ts` (only the tub-ownership parts — see note below)
- `qrtub/src/lib/database/item-id-mask-reservation.ts`
- `qrtub/src/lib/hooks/useCreateTub.ts`, `create-tub-payload.ts`, `useTubForm.ts`,
  `useTubOperations.ts`, `tub-name-generator.ts`
- `qrtub/src/components/blocks/EditTubForm/EditTubForm.tsx` (1,028 lines — the actual Settings
  tabs: General, Fields, Links, Scan, Page, Admin — read in full)
- `qrtub/src/components/blocks/TubExportImportModal/TubExportImportModal.tsx`,
  `ImportTubFromBackup/ImportTubFromBackup.tsx`
- `qrtub/src/lib/services/tub-export-import-service.ts`, `lib/types/export-import.ts`,
  `lib/hooks/useTubExportImport.ts`
- `qrtub/src/app/api/collections/route.ts`, `[id]/route.ts`, `import-new/route.ts`
- `qrtub/src/app/app/tub/create/page.tsx` (the creation fork), `lib/templates/all.ts` +
  `README.md` (the starter-template registry)
- `qrtub/supabase/migrations/20250718000002_complete_schema.sql`,
  `20250912000001_create_page_templates_table.sql`, `20260318000001_add_print_batch_tracking.sql`,
  `20251008000000_add_team_and_strategies_to_access_urls.sql`,
  `20260223000001_unassign_access_urls_for_tub.sql` — verified the actual FK/cascade behavior at
  the DB level rather than trusting code comments
- `qrtub/GLOSSARY.md`, `qrtub/BRAND.md`
- Existing docs checked for overlap: `mintlify-docs/custom-fields.mdx`, `using-fields.mdx`,
  `key-concepts.mdx`

## Overlap with sibling domain inventories — read this before drafting

Three sibling inventories already exist in this directory and each one reaches into Collection-owned
territory. Reconciling with them, not duplicating them, is itself part of this domain's task
(the brief specifically asks whether Collection settings — especially custom-field schema — split
cleanly from Items):

1. **`items.md` already owns the entire field-*schema* mechanism** under its own top-level
   "Fields" category (rows 8–18: core vs custom fields, the six field types, allowed values,
   allow-new-values, multiple-values, reference fields, field defaults, required fields,
   creating/renaming/deleting a field). That inventory's own notes flag this as an open
   question ("should 'Fields' actually be Collections?") and explicitly ask whoever does the Collections pass
   not to re-derive the same table. Having now read the same source (`field-config.ts`) end to
   end myself, **I'm resolving that open question**: see "Verdict on the explicit task
   question" below. Short version — `items.md`'s 11 Fields rows should stay exactly as
   scoped; I am not re-listing them here.
2. **`links.md` already owns numbered patterns as a Links-domain resource** (row 5: claiming a
   `<prefix><N digits><suffix>` range, generation modes, conflict-checking, plan-tier limits).
   What's still mine is the Collection-*setting* that decides *whether* a link gets minted at all when
   an Item is created, and the mask editor as it appears in Collection Settings — not the numbered-
   pattern mechanism itself, which I cross-reference rather than re-explain. `links.md` row 9
   also already states the fact that Collection deletion releases (not deletes) Links; my "Deleting a
   Collection" page confirms the same fact from the Collection side, with the DB-level mechanics
   (`ON DELETE CASCADE` vs the app's `unassign_access_urls_for_tub` step) that `links.md` didn't
   need to go into.
3. **`account-team-billing.md` picked up "Collection template gallery" as an unowned orphan** (its
   "Workspace" section, row: `workspace/collection-templates`), explicitly flagging in its own open
   questions that this isn't really an Account/Team/Billing concept and asking whoever owns
   Collections to reconcile. **I'm claiming it** — see row 3 below (`collections/starter-templates`). That
   inventory's `workspace/collection-templates` row should be retired in favor of this one.

## Verdict on the explicit task question

**Collection-level settings form a clean, standalone nav category ("Collections"), fully separable from
Items — including custom-field schema.** Reasoning:

- The litmus test the brief poses — can "define a custom field" be written as a complete,
  standalone concept without needing "fill in a custom field" in the same breath? — is already
  answered empirically by the current `custom-fields.mdx`. It is 119 lines of real, specific
  content (field keys, types, allowed values, defaults, reference fields, required-field
  validation) and it **never once explains how a person types a value into an Item's field**.
  It only points outward to `using-fields.mdx` for *binding* syntax (`{{item.field}}`), which is
  a third, unrelated concept (referencing a field in a URL/condition, not filling it in). That
  the existing doc already achieves a clean split, unassisted, is strong evidence the split is
  real and not just theoretically possible.
- The code backs this up structurally: `FieldConfig`/`FieldDefinition` (the schema) live
  exclusively in `tub.metadata.fieldConfig`, are read/written only through Collection API routes
  (`/api/collections/[id]`) and only editable from the Collection Settings "Fields" tab
  (`FieldConfigurationManagerCompact`, rendered by `EditTubForm`). An Item never carries its own
  schema — `ThingSerializer` only ever *applies* the Collection's schema to translate an Item's raw
  JSONB into semantic key/value pairs. There is no code path where defining a field requires
  knowing anything about a specific Item, and no code path where filling in an Item's field
  value touches the schema-definition functions (`addCustomFieldToConfig`,
  `ensureFieldIds`, `validateFieldKey`, etc.) — the two are read by different services for
  different reasons.
- The one place the two legitimately touch is `defaultValue` (a Collection-level fallback used *when*
  an Item is created blank) — but that's a one-sentence cross-reference ("an explicit Item value
  always wins"), not a shared page. Exactly the same shape as Linear's own `priority.md`
  referencing sort order elsewhere without merging the pages.
- Because the schema domain is large enough on its own (11 pages in `items.md`'s accounting),
  it's better served as its **own sibling top-level category ("Fields") rather than nested under
  "Collections"** — matching Linear's own precedent of "Issues" and "Issue properties" as separate
  sidebar groups even though issue properties are edited from inside an issue. So the final
  shape is three siblings: **Collections** (this inventory), **Fields** (`items.md`, schema
  definition), **Items** (`items.md`, everything else about the Item entity) — not "Collections" and
  "Items" with Fields buried in either one.
- Every other Collection setting in this inventory (link generation, the item-ID mask, scan-behavior
  default, backup/restore, deletion) is even more clearly Collection-only than the schema case — none
  of them require explaining anything about a specific Item's data to describe completely, only
  a one-line note where an Item can override the Collection default (scan behavior, field defaults).

## Inventory

| # | Concept | One-line definition | Nav category | Proposed page title | Proposed slug | Atomic call |
|---|---|---|---|---|---|---|
| 1 | What a Collection is | The entity that groups a set of Items under one shared data schema, link-generation rule, scan behavior, and page template; owned by a team. | Collections | What Is a Collection? | `collections/overview` | **Standalone.** Short orientation page (~200–300 words), the same role Linear's own "Concepts" page plays — ties Item, Link, custom-field schema, and page template together in one sentence each and points out to their owning pages/domains, without re-explaining any of them. |
| 2 | Creating a Collection | The single-step "fork": pick whether new Items' links connect to one destination or to a page of several, or skip straight to a starter template — the Collection is created immediately (no multi-field form), gets an auto-generated `Collection<N>` name, and everything is editable inline afterward. | Collections | Creating a Collection | `collections/creating-a-collection` | **Standalone.** ~300–400 words: the two fork cards (confirmed `FORK_CHOICES` in `tub/create/page.tsx`), the "print first, connect later" escape hatch to Access Links, the sequential default name (`nextSequentialTubName` — next-past-highest-existing `Collection<N>`, not a running count, so a deleted `Tub2` doesn't collide with a new one), and the fact this seeds `linkGeneration.mode: 'random'` by default for new tubs (vs. `'none'` for tubs that predate the feature). |
| 3 | Starter templates | The gallery of pre-built starter kits (SafetyCulture-style asset, Clone QR Code, Equipment + Inspections, Audience Routing, Branded Handoff, Inventory, IT Assets, Medical Equipment) that seed a new Collection's field schema, page sections, and 3–5 sample Items in one step. | Collections | Starter Templates | `collections/starter-templates` | **Standalone.** ~350–500 words: confirmed exactly 8 user-facing templates in `templates/all.ts` (a 9th, `default`, is registered but deliberately hidden — `INTERNAL_TEMPLATE_IDS`); a template bundles three things at once (`fieldConfig`, `generatePageSections`, `generateSampleItems`); the fork page shows the first 5 as examples with a "Browse all" link to the full grid at `/app/templates`; picking one enables page mode when the template's `destinationType` is `landing_page`. **Supersedes** `account-team-billing.md`'s orphaned `workspace/collection-templates` row — same feature, this is the right home for it. |
| 4 | Collection details (name, description, items label, image) | The four general-purpose settings on a Collection: its name, a free-text description, an optional override for what to call its Items (e.g. "Excavators" instead of "Items"), and a cover image. | Collections | Collection Details | `collections/collection-details` | **Standalone**, though the thinnest of the bunch — flag for a word-count check at draft time. Real content: confirmed in `EditTubForm`'s "General" tab (`GeneralTab`, `PanelSection` titled "Collection details") and the `items_name` column read/written by `useTubOperations`/`useTubForm`; worth stating plainly that none of these four are used in any binding or validation — purely display. |
| 5 | Link-generation modes (Random / ID-based / None) | The Collection-level setting deciding whether — and how — a Link gets minted automatically the moment a new Item is created in this Collection. | Collections | Link Generation for New Items | `collections/link-generation-modes` | **Standalone.** ~350–450 words: the three modes verified in `link-generation-config.ts` and the exact UI copy in `EditTubForm`'s `LINK_MODE_OPTIONS` — `random` (mint a fresh `/r/x5fgd`-style link, today's default for new Collections), `item_id` (build the slug from the Item's own Item ID — hands off to row 6 for the mask), `none` (no link at all, the "print first, attach later" case, and the default for Collections that predate this feature). Explicitly state that this only governs *creation*-time minting — deletion behavior is fixed and not configurable (cross-link to row 14 and to `links.md` row 9, which already documents the release-not-delete mechanic from the Link side). Does not re-explain what a Link *is* — that's `links.md` row 1. |
| 6 | The Item ID mask builder | The inline editor shown when link generation mode is "ID-based": define the prefix/suffix affixes wrapped around an Item's own Item ID, optionally reserving an exact digit count that turns it into a numbered pattern the whole team can adopt. | Collections | Building an Item ID Mask | `collections/item-id-mask` | **Standalone.** ~400–550 words: the two mask shapes confirmed in `TubItemIdMask`/`isPatternMask` — a free-form mask (`digits` absent, slug = `<prefix><item_id><suffix>` verbatim, legacy behavior) vs. a numbered-pattern mask (`digits` set, e.g. `CRA`+4+`TL` → items must match `CRA0042TL` exactly, template shown as `CRA####TL`); the editor lets you adopt one of the team's existing reserved patterns (with live next/used/gap stats, confirmed `getPatternStats`) or define a brand-new one. Cross-references `links.md` row 5 for what a numbered pattern *is* and how claiming/generating one works generally — this page is specifically about the Collection-settings entry point into that mechanism, and the format-validation gate it imposes on every Item ID typed into this Collection (`validateItemIdForTub`, enforced server-side on create/edit, not just in the form). |
| 7 | Item ID mask conflicts (team-scoped exclusivity vs. shared numbered patterns) | The rule that a *free-form* mask's prefix/suffix can only belong to one Collection per team, while a *numbered-pattern* mask is a shared team resource multiple Collections can adopt at once. | Collections | Item ID Mask Conflicts | `collections/item-id-mask-conflicts` | **Standalone, but the thinnest merge-candidate in this inventory** — flag for drafting to fold into row 6 if it can't clear ~150 words alone. Real, verified, and genuinely surprising content: `findItemIdMaskConflict` blocks saving a free-form mask already used by another Collection on the team (409, "pick different affixes"), specifically *because* a free-form mask has no shared sequence to coordinate collisions — while `isPatternMask` candidates are explicitly exempted from this check because numbered patterns already have their own shared-ownership model (owned by team, not Collection — see `links.md` row 5). This asymmetry is the kind of one-clause, non-obvious rule Linear gives its own short page to. |
| 8 | Scan behavior default (Page Mode vs. Direct Mode for new Items) | The Collection-level toggle that decides what a brand-new Item starts as when scanned: pass-through to one destination, or open a page listing several — each Item can still be changed individually afterward. | Collections | Scan Behavior for New Items | `collections/scan-behavior-default` | **Standalone**, but scope carefully — see note. ~200–300 words: confirmed as the "What a scan does" panel in `EditTubForm`'s Scan tab, backed by `landingPageDefaults.profilePagesEnabled`/`destinationType`, with `getEffectiveDestinationType` falling back to this Collection default only when the Item itself hasn't made an explicit choice. **Ownership boundary:** this page is about the *Collection-level default*, not about what Direct Mode / Page Mode fundamentally *are* — GLOSSARY.md defines those as product-wide concepts, and `links.md`'s own open questions flag that the general concept likely belongs to a Pages/Destinations domain pass that doesn't exist yet. Cross-reference outward for that; don't re-derive it here. |
| 9 | Default destination for new Items | The Collection-level destination URL template (with optional app-link fallback) applied to an Item at creation time when the Collection defaults to pass-through mode, plus a live warning when the template references a field some Items don't have a value for yet. | Collections | Default Destination for New Items | `collections/default-destination` | **Standalone, with a scoping note.** ~300–400 words of content genuinely not covered elsewhere: the CEL-expression builder UI (`CelExpressionInput`, live preview against a real sample Item), the app-link fallback URL/message pairing specific to this default, and `DestinationFieldHints` — a warning that names exactly which Items are missing a field the template references and how many. **Overlap check:** `items.md` row 14 ("Field defaults") already states the underlying fact that a destination-URL default is "stamped onto the Item at creation, not retroactive" — don't restate that derivation here, just link to it; this page is about the Scan-tab authoring experience around that one special field, not the general default-value mechanism. |
| 10 | The Collection's page template | Every Item in a Collection renders the same one shared page template (unless a specific Item has its own override) — created from the starter template at Collection creation, previewed live from Collection Settings, and edited in the Page Editor. | Collections | The Collection's Page Template | `collections/page-template` | **Standalone, but the other real merge-candidate** — flag for drafting. Confirmed content: one `page_templates` row per Collection (`tub_id` unique-ish relationship, `ON DELETE CASCADE` confirmed in the migration), the "Profile page" Settings tab's live `MobilePagePreview` (rendered against sample field data, not a mock), and the explicit "Edit profile page" hand-off to `/app/page-editor/{tubId}` in a new tab. Deliberately does **not** cover how to actually build a page (drag blocks, bind fields, Item-level overrides) — that's the Page Editor / Pages domain's territory; this page states only the Collection-level ownership fact and hands off. If that's too little to clear 150 words on its own, fold it into row 2 (Creating a Collection) instead of forcing a standalone page. |
| 11 | Exporting a Collection backup | Downloading a JSON snapshot of a Collection's settings, field schema, and page template — deliberately excluding its Items. | Collections | Exporting a Collection Backup | `collections/export-backup` | **Standalone.** ~200–300 words: confirmed by reading both the service (`TubExportImportService.exportTubData` — the code comment "Note: Items are excluded from tub export") and the actual modal copy ("Note: Items are not included in this backup. Use CSV export/import from the table for items."). State this limitation explicitly and plainly — it's the one fact a reader would otherwise assume is included. Item overrides and the active page template *are* included. |
| 12 | Importing a backup — Merge vs. Replace | Restoring a downloaded backup into an *existing* Collection, in one of two modes: Merge (add new fields only, nothing existing is touched) or Replace (overwrite the Collection's name/description/fields/page template outright, gated behind typing "REPLACE"). | Collections | Importing a Collection Backup | `collections/import-backup` | **Standalone**, deliberately covering both modes as one contrast page rather than two, mirroring how `links.md` handles "Deleting vs. releasing" as a single page. ~350–450 words: Merge matches incoming fields by their human-readable key and only *adds* ones the target doesn't already have (`mergeFieldConfigs` — never overwrites an existing field's definition); Replace deletes the target's existing Items and item overrides outright before restoring from the backup (confirmed in `importTubData`'s replace branch — a real, destructive detail worth stating even though the UI copy undersells it as just "settings, fields, and page template"); both modes explicitly state Items themselves are unaffected unless the (rare, hand-edited) backup file happens to carry an `items` array. |
| 13 | Creating a new Collection from a backup | Using a backup file to spin up a brand-new Collection (rather than importing into an existing one) — the new Collection's name gets " (Copy)" appended automatically. | Collections | Creating a New Collection From a Backup | `collections/new-collection-from-backup` | **Standalone.** ~200–300 words: confirmed the `" (Copy)"` suffix in `createTubFromBackup`, the preview dialog shown before committing (name, field count, template name, description), and that this is QRtub's closest equivalent to "duplicate a Collection" even though it's framed as a restore-from-file flow rather than a same-account clone button — worth stating plainly since a reader will look for a "Duplicate" action and not find one. |
| 14 | Deleting a Collection | Permanently removing a Collection: its Items and page template are hard-deleted, but every Link that was assigned to one of those Items is released to the unassigned pool first, not deleted — so printed physical codes keep resolving. | Collections | Deleting a Collection | `collections/deleting-a-collection` | **Standalone.** ~300–400 words, and load-bearing (this is the behavior most likely to be assumed wrong). Verified at the DB level, not just from code comments: `things.tub_id` and `page_templates.tub_id` are both `REFERENCES tubs(id) ON DELETE CASCADE`, so Items and the page template really are gone; `access_urls.thing_id` is *also* `ON DELETE CASCADE` against `things(id)`, which is exactly why the delete route calls the `unassign_access_urls_for_tub` RPC (sets `thing_id = NULL` for every affected Link) *before* deleting the Collection — skip that step and the cascade would silently delete the Links too. Also worth one line on print batches/Media templates referencing the Collection: their `tub_id` FK is `ON DELETE SET NULL` (confirmed in the print-batch-tracking migration), so they survive deletion, just lose the "made from this Collection" association — flagged here but owned by a Media domain pass, not re-explained. Matches the app's own confirmation copy verbatim (`EditTubForm`'s `handleDelete`). |

## Notes for the drafting phase

- **Nav shape recommendation:** one top-level "Collections" category holding all 14 rows above,
  sibling to "Fields" and "Items" (both `items.md`) and to "Links" (`links.md`) — not nested
  under any of them, and not swallowing any of them either. Within "Collections," a natural sidebar
  sub-grouping (mirroring the app's own Settings tabs) is: **Getting started** (1–4), **Links**
  (5–7), **Scan behavior** (8–9), **Page template** (10), **Backup & restore** (11–13),
  **Deleting a Collection** (14) — six small groups rather than one long flat list, the same instinct
  Linear applies to "Issues" vs. "Issue properties."
- **Two explicit merge-candidates flagged inline** (row 7 "Item ID mask conflicts" and row 10
  "The Collection's page template") — both are real, verified, non-invented content, but both are
  short enough that a word-count check at draft time might land them under the ~150-word floor.
  If so, fold row 7 into row 6 and row 10 into row 2, per the notes on those rows.
- **`privacy` (public/private) exists in the data model but has no live Collection-level UI control
  today** — `TubLandingPageDefaults.privacy` is set once at creation (always `'public'`) and
  read back, but `EditTubForm`'s Scan tab has no toggle for it; the only place privacy is
  actually *set* by a user is per-Item (`AddEditItemForm`). Deliberately **not** given a Collection-
  settings page here — documenting a control that doesn't exist would violate the "state
  limitations explicitly" rule the other way around (implying a setting exists). If a Collection-level
  privacy toggle ships later, it's a small standalone addition to row 8's territory.
- **`tub.metadata.templateId` and template-change regeneration** (the `PUT /api/collections/[id]`
  route's `templateChanged` branch, which would regenerate the page template if `templateId`
  changed) exists in the API but has **no corresponding UI** — nothing in `EditTubForm` lets a
  user change a Collection's template after creation today. Not given a page for the same reason as
  privacy above; noted here so drafting doesn't stumble on the dead code path and assume a
  missing feature.
