# Proposed Information Architecture — help.qrtub.com (Draft v1)

Calibrated against Linear's real docs (linear.app/docs): many narrow, single-topic pages
(200–1,000 words each) grouped under topical categories, with one short orientation page per
category playing the role Linear's "Concepts" page plays — never the place the real depth
lives. This draft applies that calibration across every domain inventory in
`planning/atomic-ia/inventory/`, reconciling the overlaps and open questions those inventories
flagged rather than concatenating them as-is.

Three tabs, matching the genre boundaries already present in the live site (a technical help
corpus vs. vertical landing pages vs. partner-connection guides) rather than Linear's single
flat sidebar — QRtub's marketing/landing content doesn't belong in the same register as its
reference docs, so the tab split is preserved, but *within* the Help tab this draft goes flat
and wide (17 groups) exactly the way Linear's own single sidebar does, instead of inventing
more tabs to hide the size.

Total: 115 Help pages across 17 groups, 6 Use Cases pages, 7 Integrations pages = **128 pages.**

---

## Decisions

### 1. Tubs vs. Items separability

**Resolved: they split cleanly, and custom-field schema goes with neither — it's a third
sibling, "Fields."** Adopting `tubs.md`'s verdict, which is well-evidenced rather than
asserted: the existing `custom-fields.mdx` already achieves the split unassisted (119 lines
that fully explain defining a field and never once explain filling one in), and the code
backs it structurally — `FieldConfig` lives exclusively in `tub.metadata.fieldConfig`, is
read/written only through Tub API routes, and no code path for defining a field touches an
Item, or vice versa. The one connection point (`defaultValue`, a Tub-level fallback used when
an Item is created blank) is a one-sentence cross-reference, not a shared page — the same
shape as Linear referencing sort order from `priority.md` without merging the pages.

Because the field-schema domain is large enough to be its own group (10 pages, see below),
it becomes a **third sibling top-level group, not a subsection of Tubs or Items** — mirroring
Linear's own precedent of "Issues" and "Issue properties" as adjacent sidebar groups even
though issue properties are edited from inside an issue. Final shape: **Tubs** (12 pages,
tub-level settings), **Fields** (10 pages, the custom-field schema mechanism), **Items** (5
pages, the Item entity itself) — three siblings, none nested inside another.

### 2. App Links / Conditional Visibility / Device Detection: Pages vs. Destinations

**Resolved: all three move to the new Destinations group, along with a fourth mechanism
(`destination_config` rule-priority) that the current single `conditional-visibility.mdx`
was silently conflating with Conditional Visibility proper.** Adopting `pages-destinations.md`'s
verdict:

- **App Links & Fallback URLs** moves — it has no Page Editor dependency at all; a Direct-Mode
  Link with no Page uses the identical `AppLinkOpener` mechanism.
- **Device Detection & Routing** moves — it's a `device.*` binding reference plus
  Destination-routing patterns that only sat under "Pages" because Destinations didn't have a
  group yet.
- **Conditional Visibility** moves, and splits into two pages on the way: hiding a *section*
  (`destinations/conditional-visibility`) is mechanically distinct from choosing a *URL* via an
  ordered `destination_config.rules` list (`destinations/conditional-destinations`), even
  though both evaluate CEL. The current page's "Advanced: Device-Specific Destinations"
  example is actually the second mechanism dressed up as the first — this split fixes that,
  it doesn't just relocate the confusion.
- Net effect: only `pages-overview` and `building-a-page` (now split across the Pages group's
  13 pages) stay in Pages. Everything else that touches routing, not layout, is Destinations.

### 3. Industries tab naming/reframing

**Resolved: rename the tab to "Use Cases," nest the five current pages under a "By Industry"
group, and add one short `use-cases/overview` orientation page.** Adopting
`industries-reframe.md`'s recommendation over "Applications" (collides in meaning with the
adjacent Integrations tab) and "Industry Guides" (safest option, but doesn't touch the actual
tension — BRAND.md states "QRtub is industry-agnostic" while the tab groups by five named
verticals regardless of what it's called). "Use Cases" reuses vocabulary the brand docs
already treat as canonical (UC-001…UC-008) and matches the content's real shape — every page
already organizes itself as "Two Use Cases, Same Solution" narratives wearing an
industry-vertical label, so this reframes the tab to match the content rather than the
content to match the tab.

The one real risk the inventory flagged — "Use Cases" colliding with the horizontal,
cross-industry UC-numbered library if that ever gets its own docs — is mitigated by nesting
today's five pages under a **"By Industry"** group (leaving room for a future "By Workflow"
group) and by adding the one orientation page the inventory didn't get to propose:
`use-cases/overview`, which states the By Industry / By Workflow split explicitly from day
one so a second group later doesn't read as a retrofit.

**Ship the rename together with the fabrication/tone fixes already found on 4 of 5 pages**,
per the inventory's own reasoning: the sentences needing factual correction (the fabricated
auto-updating register, the phantom analytics/alerts upsell, the unhedged "auto-route" claim)
are the same sentences carrying the worst of the "weaponize"/"guerrilla marketing" tone.
Fixing tone and fixing fact are the same edit on the same lines.

### 4. Concepts flagged "too thin, should merge"

Every flagged candidate, confirmed or overridden with reasoning:

| Flagged concept | Inventory | Recommendation | Call |
|---|---|---|---|
| Item ID Mask Conflicts | tubs.md row 7 | fold into row 6 if <150 words | **Confirmed merge** → subsection of `tubs/item-id-mask` ("Mask conflicts"). The free-form-vs-numbered-pattern asymmetry is real and worth a heading, not a URL. |
| The Tub's Page Template | tubs.md row 10 | fold into row 2 if too thin | **Confirmed merge** → subsection of `tubs/creating-a-tub` ("Where the page template comes from"). It's a one-`page_templates`-row-per-Tub ownership fact plus a hand-off link to the Page Editor — real, but not enough distinct mechanism to justify its own URL once the starter-template gallery (which is where a reader actually encounters this) already exists as a sibling page. |
| Item image | items.md row 5 | fold into row 1 if too thin once drafted | **Confirmed merge** → subsection of `items/overview` ("The Item image"). It's a fixed system field with one real cross-feature fact (feeds reference-field previews) — a paragraph, not a page; Linear has no per-entity "avatar" page either. |
| Multiple values (List fields) | items.md row 12 | fold into row 9 (Custom field types) if <150 words | **Confirmed merge** → subsection of `fields/field-types` ("Multiple values"). It's one checkbox with one gating rule (enabled only when type is List) — genuinely thinner than Linear's own 243-word floor once isolated. |
| Page metadata & social previews | pages-destinations.md A32 | fold into Pages Overview if <150 words | **Overridden — kept standalone** (`pages/page-metadata`). Once drafted this has real, specific content: auto-generated OG/Twitter tags from Item data, the deliberate `noindex, follow` split (thin per-item pages shouldn't dilute search, but link equity should still flow), and a distinct minimal-metadata path for private Pages. That's three non-obvious, verifiable facts — comfortably past Linear's 243-word floor, not a footnote on an orientation page. |
| Item ID Mask Conflicts / Tub's Page Template / Item image / Multiple values | — | — | Each merge target above gets a **named heading** for the merged concept (not a silent paragraph), so it's still individually findable in search/retrieval even without its own URL — per the brief's own instruction. |

No other flagged-thin concept from the inventories survived independent review as
standalone; the ones above are the complete set.

### 5. Additional resolution: duplicates across sibling inventories

Six domain inventories were written independently and, predictably, a few features got
inventoried twice from two different angles. Resolving each to one canonical page rather than
publishing both:

1. **Starter templates** (tub-field-schema + page-sections + sample-items, seeded together at
   Tub creation) — inventoried in both `tubs.md` (row 3) and `pages-destinations.md` (A31),
   and *also* orphaned once in `account-team-billing.md`'s "Workspace" section. **Canonical
   home: `tubs/starter-templates`.** It's a Tub-creation-time feature before it's a Pages
   feature; the Pages and Account/Team/Billing inventories both cross-link to it instead of
   duplicating.
2. **Per-item Page overrides** (the Override toggle: sparse-diff save vs. base-template save,
   auto-enable, revert) — inventoried in both `items.md` (row 7, "Item Page overrides") and
   `pages-destinations.md` (A29, "Per-Item Page Overrides"), describing the identical
   mechanism from two angles. **Canonical home: `pages/page-overrides`**, since the mechanism
   is fundamentally about what the Page Editor decides to save and where — `items/overview`
   cross-links out to it rather than Items getting its own copy.
3. **Default Destination Pattern for Direct-Mode tubs** (a Tub-level URL template stamped onto
   every new Item at creation, frozen thereafter) — inventoried in both `tubs.md` (row 9) and
   `pages-destinations.md` (B16), again describing the same Scan-tab feature. **Canonical
   home: `tubs/default-destination`**, since it's authored from Tub Settings alongside the
   other Tub-level scan defaults; Destinations cross-links to it instead of repeating it as a
   thirteenth Destinations page.
4. **Downloading QR code images (PNG, single + bulk zip)** — inventoried in both `links.md`
   (row 12) and `media-printing.md` (row 12b), verbatim the same feature reached from the same
   Access Links page. **Canonical home: `bulk-links/downloading-qr-codes`** — it's an action on
   Links, not on a print batch; Print Batches cross-links to it.
5. **Claim-on-scan** — inventoried in `links.md` (row 8, the general resolve/claim mechanism
   used by any scan) and `media-printing.md` (row 17, the in-app scanner's "Create & open"
   action, which adds a scanner-specific reference-photo capture not present in the general
   case). **Kept as two pages, each scoped and cross-referenced rather than merged**:
   `links/claim-on-scan` owns the mechanism (hash, idempotency, minted-link fact);
   `print-first/claim-on-scan` owns only what's genuinely new about the scanner's UI action
   (the "Create & open" button, the optional photo capture) and defers to the first page for
   the mechanism instead of re-deriving it.
6. **Page Template Versioning** (`pages-destinations.md` A30) — flagged with an open boundary
   question (Pages domain or Tub-settings domain?). **Resolved to Pages**
   (`pages/page-template-versions`): version history, restore, and clone are Page Editor
   actions reached from inside that tool, not a Tub Settings control — the same reasoning that
   keeps "Fields" a sibling of "Tubs" rather than a subsection of it also argues for keeping
   version history with the tool that produces the versions.

### Excluded from this draft (deliberately, not by oversight)

- **QR code export formats & error correction level** (`media-printing.md` row 29) — describes
  a planned brief (`qrtub-ops/briefs/qr-format-and-error-correction.md`), not shipped
  behavior. Per the existing `DRAFT` guard on the current unpublished pages, this stays out of
  `docs.json` until the feature ships. `bulk-links/downloading-qr-codes` documents today's
  real, PNG-only equivalent instead.
- **Jotform, Google Maps, Google Sheets integrations** (`integrations.md`) — three reserved
  folders with no shipped feature to inventory yet, and in Google Sheets' case, an
  undecided integration *direction* (deep-link-in vs. data-export-out) that changes which tab
  it belongs to entirely. Held out of the tree until a real feature exists to document,
  matching how the rest of this redesign treats unshipped work.

---

## Proposed tree

### Tab: Help

- **QRtub Documentation** — `index` — Homepage: what QRtub is, in one screen, with links into
  Getting Started and the rest of the tab. *(sits outside any group, same as today.)*

#### Group: Getting Started

- **Creating Your First Link** — `creating-your-first-link` — The fastest path end to end:
  create a Tub, get a Link, connect it to an Item — kept as the one tutorial-shaped page in
  this group, distinct from the conceptual orientation below it.
- **Key Concepts** — `key-concepts` — A short, cross-entity orientation tying Tub, Item, Link,
  Page, and Media together in one sentence each — the same role Linear's own "Concepts" page
  plays above "Priority," "Due dates," etc. Deliberately trimmed once the atomic pages below
  exist; it points outward rather than re-explaining any of them.

#### Group: Tubs

- **What Is a Tub?** — `tubs/overview` — Orientation: the entity that groups Items under one
  schema, link-generation rule, scan behavior, and page template.
- **Creating a Tub** — `tubs/creating-a-tub` — The single-step creation fork (one destination
  vs. a page of several, or a starter template), the auto-generated `Tub<N>` name, and where
  the page template a new Tub gets actually comes from.
- **Starter Templates** — `tubs/starter-templates` — The 8-template gallery that seeds a new
  Tub's field schema, page sections, and sample Items in one step — canonical home for this
  feature (see Decision 5.1).
- **Tub Details** — `tubs/tub-details` — The four display-only general settings: name,
  description, custom Items label, and cover image.
- **Link Generation for New Items** — `tubs/link-generation-modes` — The Tub-level setting
  deciding whether a Link is minted automatically when a new Item is created, and which of the
  three modes (Random / ID-based / None) it uses.
- **Building an Item ID Mask** — `tubs/item-id-mask` — The prefix/suffix/digit-count editor for
  ID-based link generation, including the free-form-vs.-numbered-pattern conflict rules (see
  Decision 4).
- **Scan Behavior for New Items** — `tubs/scan-behavior-default` — The Tub-level default
  deciding whether a brand-new Item starts as Direct Mode or Page Mode.
- **Default Destination for New Items** — `tubs/default-destination` — The Scan-tab
  CEL-expression builder for a default destination URL template stamped onto new Items at
  creation, plus the live "missing field value" warning (canonical home for this feature, see
  Decision 5.3).
- **Exporting a Tub Backup** — `tubs/export-backup` — Downloading a JSON snapshot of a Tub's
  settings, fields, and page template — Items are explicitly excluded.
- **Importing a Tub Backup** — `tubs/import-backup` — Restoring a backup into an *existing*
  Tub in Merge mode (additive) or Replace mode (destructive, typed confirmation required).
- **Creating a New Tub From a Backup** — `tubs/new-tub-from-backup` — Using a backup file to
  spin up a brand-new Tub instead of an existing one, QRtub's closest equivalent to "duplicate
  a Tub."
- **Deleting a Tub** — `tubs/deleting-a-tub` — What's actually hard-deleted (Items, page
  template) vs. what survives by design (Links are released, not deleted, so printed codes
  keep resolving).

#### Group: Fields

- **Core Fields vs. Custom Fields** — `fields/core-vs-custom` — The load-bearing split: 4 fixed
  core-field columns vs. everything else stored by a stable ID in `metadata.fields`.
- **Custom Field Types** — `fields/field-types` — The six field types and what each gates
  (allowed values, multiple values, reference config), including the Multiple Values toggle
  (see Decision 4).
- **Allowed Values** — `fields/allowed-values` — The `{value, label, color}` picker list that
  also drives chip color everywhere the field renders, including the scanned Page.
- **Allow New Values** — `fields/allow-new-values` — The per-field toggle controlling whether a
  value outside the Allowed Values list can be typed in or imported via CSV.
- **Reference Fields** — `fields/reference-fields` — The field type that points at another
  Item, Tub, or team member instead of holding its own value.
- **Field Defaults** — `fields/field-defaults` — The Tub-level fallback applied when an Item is
  created with a field left blank, including the destination-URL field's "stamped at
  creation, never retroactive" exception.
- **Required Fields** — `fields/required-fields` — The "Required" checkbox and its one
  carve-out: a partial CSV update is only checked against columns that row actually touches.
- **Creating a Custom Field** — `fields/creating-a-field` — Adding a field: label, auto-slugged
  key, type, and the field-key validation rules.
- **Renaming a Field** — `fields/renaming-a-field` — Why renaming is safe (the stable ID never
  changes) and what auto-updates on save (page-editor bindings referencing the old key).
- **Deleting and Disabling Fields** — `fields/deleting-and-disabling` — The core-vs-custom
  asymmetry: a core field can only be disabled, a custom field can be permanently deleted.

#### Group: Items

- **What Is an Item?** — `items/overview` — Orientation: the individual record a Tub tracks,
  including the Item image system field (see Decision 4).
- **Name and Description** — `items/name-and-description` — The two free-text core fields
  every Item ships with.
- **Item ID** — `items/item-id` — The per-Tub-unique identifier field, its blank/whitespace
  normalization, and its role in ID-based Link generation.
- **Tags** — `items/tags` — The core `tags` array field, its default free-form-addition
  behavior, and its colored-chip rendering.
- **Duplicating an Item** — `items/duplicating` — What the "Copy" action carries over (fields)
  vs. what it deliberately drops (Item ID, the source's page override).

#### Group: Import & Export

- **Importing Items from CSV** — `import-export/importing-items` — Bulk create/update by
  matching `id`, per-row validation against the Tub's field config, dry-run preview, and the
  10 MB / 10,000-row limits.
- **Exporting Items to CSV** — `import-export/exporting-items` — Downloading visible-columns
  vs. full-field CSV, scoped by the current search/filter/sort.

#### Group: Links

- **What a Link Is** — `links/what-a-link-is` — Orientation: a slug that resolves to a
  destination, encodable on any medium, and why it behaves like an ordinary URL shortener at
  its core.
- **What a Slug Is** — `links/what-a-slug-is` — The identifier segment of a Link's URL, and the
  case-sensitivity split between random links and numbered/custom links.
- **Random Links** — `links/random-links` — The default 5-character base62 link type, minted
  automatically, never user-chosen.
- **Numbered Links** — `links/numbered-links` — Claiming a `<prefix><N digits><suffix>` range
  as a team and minting against it — sequentially, at a specific number, or as a bulk range.
- **Custom Links** — `links/custom-links` — User-chosen slugs, their charset rules, and the
  reserved-word / reserved-pattern blacklist.
- **Unallocated Links** — `links/unallocated-links` — A real, printable Link with nothing
  attached yet, why that's a deliberate feature (print-before-link, spares), and what a scan
  of one shows.
- **Claim-on-Scan** — `links/claim-on-scan` — The general mechanism: scanning an unrecognized
  third-party code mints a Link bound to a hash of its decoded text, idempotently.
- **Deleting and Releasing Links** — `links/deleting-and-releasing-links` — Hard delete
  (blocked once a Link is in a non-draft print batch) vs. release (automatic, happens whenever
  an Item or Tub is deleted, keeps printed codes resolving).

#### Group: Bulk Link Operations

- **Bulk Link Import via CSV** — `bulk-links/csv-import` — Column schema, per-type create
  rules, and the dry-run preview for creating/updating many Links from one file.
- **Bulk Assigning, Unassigning, and Deleting Links** — `bulk-links/assign-unassign-delete` —
  Acting on an explicit ID list vs. an entire filtered/searched scope, including "select all."
- **Downloading QR Codes** — `bulk-links/downloading-qr-codes` — Single PNG download vs. a
  zipped bundle for multiple Links; states plainly that there's no SVG export and no
  configurable error-correction level today (canonical home, see Decision 5.4).

#### Group: Pages

- **Pages Overview** — `pages/pages-overview` — Orientation: what a Page is, and the 4-step
  flow to turn one on for a Tub (enable, create an Item, assign a Link, add Destinations).
- **Direct Mode vs. Page Mode** — `pages/direct-mode-vs-page-mode` — The per-Tub default /
  per-Item override choice between an immediate redirect and a multi-destination Page.
- **Page Privacy: Public vs. Private** — `pages/page-privacy` — The Tub-level and Item-level
  gates that require a signed-in team member to view a Page, and their effect on `noindex`.
- **The Page Editor Layout** — `pages/page-editor-layout` — The three-tab component/data/
  structure panel, the Properties panel, and the "switching Item clears undo history" gotcha.
- **Section Types** — `pages/section-types` — The catalog of all 17 section types grouped by
  category — a map, not a page-per-section (ActionLink, AdminToolbar, and image controls are
  the three exceptions with real distinct mechanics, covered on their own pages below).
- **ActionLink: The Destination Button** — `pages/action-link` — The one section that *is* a
  Destination button: auto-hides when its binding can't resolve, groups visually with
  adjacent ActionLinks.
- **AdminToolbar** — `pages/admin-toolbar` — The configurable button bar that defaults to
  signed-in-only visibility, letting owners add internal shortcuts to a public page.
- **Image Display Controls** — `pages/image-display-controls` — The shared Frame Shape / Fit /
  Align system reused identically by ImageSection and ItemHeader.
- **Theming a Page** — `pages/page-theming` — The 7 theme presets, accent color, radius,
  typography scale, and why picking a preset replaces the whole theme rather than merging.
- **Previewing a Page** — `pages/previewing-a-page` — The item selector (base template vs. a
  real Item), 5 responsive widths, and the chrome-hiding Preview toggle.
- **Per-Item Page Overrides** — `pages/page-overrides` — The Override toggle: on saves to one
  Item only (a sparse diff), off updates every Item sharing the base template; canonical home
  for this mechanism (see Decision 5.2).
- **Page Template Versions** — `pages/page-template-versions` — Every save creates a new
  version; viewing history, restoring an old version, cloning a template to another Tub
  (placed here, not under Tubs — see Decision 5.6).
- **Page Metadata & Social Previews** — `pages/page-metadata` — Auto-generated OG/Twitter tags
  from Item data, the `noindex, follow` split, and the reduced metadata a private Page shows
  instead (kept standalone — see Decision 4).

#### Group: Destinations

- **What Is a Destination?** — `destinations/what-is-a-destination` — Orientation: where a
  scan is routed, the real resolution order (Item's `destination_config` checked before its
  plain `destination_url`), the dangerous-scheme safety filter, and the "not ready yet" empty
  state.
- **Field Bindings & URL Templates** — `destinations/field-bindings` — The `{{ }}` syntax
  itself, the full set of namespaces (`item`, `tub`, `device`, `time`, `request`, `session`,
  `theme`) with one line each, and the no-auto-encoding rule.
- **Item Fields Reference** — `destinations/item-fields` — The catalog of `item.*` bindings:
  standard fields plus whatever custom fields a Tub defines.
- **Tub Fields Reference** — `destinations/tub-fields` — The catalog of `tub.*` bindings.
- **Time Fields Reference** — `destinations/time-fields` — `time.hour`/`dayOfWeek`/etc.,
  computed fresh per scan in UTC, and the explicit absence of an absolute date or date math.
- **Request Fields Reference** — `destinations/request-fields` — `request.ip`/`country`/`city`/
  etc. sourced from CDN geo headers, with the privacy consequence of using them stated plainly.
- **Field Renames & Destination Stability** — `destinations/field-renames-and-destinations` —
  Why renaming a custom field doesn't break a Destination that references it.
- **Conditional Visibility** — `destinations/conditional-visibility` — Hiding a section with a
  CEL `condition`; the silent-`false` behavior of an undefined identifier (a typo, or an
  invented value like `today`).
- **Conditional Destinations & Rule Priority** — `destinations/conditional-destinations` — The
  ordered `destination_config.rules` list on a single Destination: first match wins, plus a
  `defaultLink` catch-all — a routing mechanism, distinct from Conditional Visibility's
  show/hide (see Decision 2).
- **Device Detection & Routing** — `destinations/device-detection` — `device.*` bindings and
  routing patterns (app vs. web, per-platform app store, tablet/browser-specific) — explicitly
  not a security mechanism.
- **App Links & Fallback URLs** — `destinations/app-links` — Deep-link URLs (any non-`http(s)`
  scheme, including `tel:`), the three-level fallback (Destination > Link > Item), and the
  real visibility-based cancel-on-app-open mechanism.
- **Connecting a Generic System** — `destinations/connecting-a-generic-system` — Methodology
  for any platform with no dedicated guide: try a plain web URL Template first, reach for a
  deep link only if there's no web equivalent.
- **How to Reverse-Engineer a Platform's Deep Links** — `destinations/reverse-engineering-deep-links`
  — Method for finding an undocumented app's own deep-link scheme, and why to always pair it
  with a Fallback URL.

#### Group: Media

- **What Is Media?** — `media/what-is-media` — Orientation: the physical material a QR code is
  displayed on, as a third entity distinct from the Link and the Item, and what's tracked
  today (production runs) vs. not yet (per-item Media).
- **Choosing Media for Your Deployment** — `media/media-types` — Comparison guide across
  common Media types by cost, durability, and use case — general industry reference, framed
  as guidance rather than an app-tracked fact.

#### Group: Print Batches

- **Print Batches** — `print-batches/overview` — Orientation: a batch is the record of one CSV
  print-list export — what was in it, how far along it is, what's been installed.
- **Creating a Print Batch** — `print-batches/creating-a-batch` — Select Items or Links, pick
  columns, export — and, for shop-bound exports, why the Full URL column matters and the
  one-row-per-piece convention.
- **Batch Status: Draft to Deployed** — `print-batches/batch-status` — The Draft → Printing →
  Printed → Deployed lifecycle and the named forward/backward actions at each step.
- **Archiving and Deleting Batches** — `print-batches/archiving-and-deleting` — Archive/
  unarchive (reversible, any status) vs. hard delete (Draft-only, since past-Draft links may
  already be printed and stuck to something).
- **Naming, Tagging and Photographing a Batch** — `print-batches/batch-details` — Inline
  rename, notes, tag chips, and a cover photo of the finished media.
- **Editing a Batch's Columns** — `print-batches/editing-columns` — Changing which columns a
  still-Draft batch's CSV includes, and why it locks once the batch leaves Draft.
- **Adding and Removing Links in a Batch** — `print-batches/editing-links` — The two entry
  points (inside the batch, or from the Items/Links grid) into the same Draft-only rule.
- **Tracking Deployment Status Per Code** — `print-batches/deployment-status` — The
  independent Printed/Deployed/Retired state per link inside a Deployed batch.
- **The Batch CSV: Downloading and Reprinting** — `print-batches/csv-download` — The exact
  stored CSV a reprint uses, server-streamed rather than a signed URL; QR image downloads live
  under Bulk Link Operations, not here (see Decision 5.4).
- **Finding and Filtering Your Batches** — `print-batches/finding-batches` — The Access Media
  page's summary cards, filters, and the "show archived" toggle.
- **Viewing a Link's Print History** — `print-batches/link-print-history` — Every batch a
  specific Link has ever appeared in, from the Link's side rather than the batch's.

#### Group: Print-First Workflow

- **The Print-First Workflow** — `print-first/overview` — Why codes get generated and printed
  before the Items they'll represent exist, and the four-step shape of that workflow.
- **The In-App QR Scanner** — `print-first/scanner` — The camera-view / manual-paste / USB-gun
  scanner reachable from the top navbar, and what a resolved code offers.
- **Adopting an Unknown Code From the Scanner** — `print-first/claim-on-scan` — The scanner's
  "Create & open" action and its optional reference-photo capture — scoped to what's genuinely
  new here; see `links/claim-on-scan` for the underlying mechanism (Decision 5.5).
- **Tips for Print-First Deployments** — `print-first/tips` — Practical advice (print the Link
  as readable text, match tag number to slug, order more than you need) too short individually
  to be their own pages, real together.

#### Group: Working with a Print Shop

- **Choosing Your Print Production Method** — `print-shop/choosing-a-method` — One-question
  orientation: does the shop's equipment merge live per piece, or need one flattened file laid
  out in advance?
- **Variable Data Printing (VDP)** — `print-shop/vdp` — Digital presses imaging each piece
  individually from a data file, live, as the run goes.
- **Gang Sheets and Composite-Sheet Production** — `print-shop/gang-sheets` — Materials that
  can only reproduce one static image per pass, and why every unique code has to be laid out
  in its final position before that single pass runs.
- **Preparing Your Print Job** — `print-shop/preparing-your-job` — Orientation checklist: spec,
  data file, the codes themselves, a way to match one to the other, a proof before the full
  run.
- **QR Code Print Spec: Quiet Zone and Minimum Size** — `print-shop/quiet-zone-and-size` — The
  4-module quiet zone and ~20–25mm minimum size to mark on a design spec.
- **Matching QR Codes to Data Rows** — `print-shop/matching-codes-to-rows` — Matching a
  downloaded QR image's filename to its CSV row by slug, and when the image files aren't
  needed at all.
- **What to Expect When a Shop Redraws Your File** — `print-shop/shop-redraws-your-file` — Why
  a shop rebuilding your design in their own software (rather than using your file as-is)
  isn't a sign of a problem.
- **Getting and Scanning a Proof** — `print-shop/getting-a-proof` — Requesting a proof of real
  records (not just the blank template) and actually scanning it at real size before the full
  run.

#### Group: Account

- **Account Overview** — `account/overview` — The read-only identity panel: email, account-
  created date, verification status, auth provider, avatar sourcing, and the list of teams you
  belong to.
- **Change Your Password** — `account/change-password` — The password-change form, its
  validation rules, and why the request goes through a server route rather than the client
  SDK.

#### Group: Team

- **Team Overview** — `team/overview` — Orientation: the shared workspace that owns Tubs,
  holds members, and optionally carries one subscription.
- **Switching Between Teams** — `team/switching-teams` — The Team Switcher, its `localStorage`
  persistence, and the dashboard-reset side effect of switching.
- **Creating a Team** — `team/create-a-team` — The Create Team modal: name, auto-generated
  slug, optional logo, and immediate ownership.
- **Team Settings** — `team/team-settings` — Renaming a team, changing its URL slug, and
  uploading a logo, owner-only.
- **Team Roles** — `team/roles` — Owner vs. Editor, and the non-obvious fact that the "Owner"
  role label alone does not grant owner authority — only Transfer Ownership does.
- **Inviting Team Members** — `team/invite-members` — The two invite paths (search an existing
  user, or invite by email), the 50/day rate limit, and the active-subscription requirement.
- **Managing Pending Invitations** — `team/pending-invitations` — The separate list of
  email-only pending invites, with Resend and Revoke.
- **Accepting a Team Invitation** — `team/accept-invitation` — The invitee's side: explicit
  Accept/Reject for an existing user vs. auto-join on first login for a brand-new one.
- **Removing Team Members** — `team/remove-members` — Bulk-selecting and removing members,
  owner-only, with confirmation.
- **Leaving a Team** — `team/leave-a-team` — Why a non-owner can leave immediately but an owner
  must transfer ownership first (or can't leave at all, if they're the last active member).
- **Transferring Ownership** — `team/transfer-ownership` — The single-transaction ownership
  reassignment, and the real gap between what the confirmation dialog implies (a picker) and
  what the code does (always transfers to the first listed active member).

#### Group: Billing

- **Billing Is Per-Team** — `billing/per-team-billing` — Why subscriptions attach to a team,
  never to a user account, and what that means for someone who owns several teams.
- **Plans Overview** — `billing/plans-overview` — The three self-serve tiers plus an
  offline-only Enterprise tier — structural facts that don't drift, deliberately not restating
  current prices (those live on qrtub.com/pricing).
- **Subscribing to a Plan** — `billing/subscribe` — The public checkout flow: pick a plan,
  enter an email, and how account + team + billing account get created together.
- **Upgrading a Team's Plan** — `billing/upgrade-a-team` — The in-app upgrade flow for an
  already-signed-in owner, distinct from checkout because no new account or team is created.
- **Managing Your Subscription** — `billing/customer-portal` — The Stripe Customer Portal
  hand-off for payment method, invoices, and cancellation — QRtub has no in-app equivalent for
  any of the three.
- **Subscription Status** — `billing/subscription-status` — What `active`/`trialing`/
  `past_due`/`canceled` each mean for feature access.
- **Plan Limits & Quotas** — `billing/plan-limits` — What varies by plan (link allowance, seats,
  reserved ID sequences) as categories, without hardcoding numbers that go stale.

#### Group: Workspace

- **Search Everything** — `workspace/search-everything` — The global search panel across
  Tubs, Items, Links, and Pages at once, its category filter, and its local recent-searches
  history.

---

### Tab: Use Cases *(renamed from "Industries" — see Decision 3)*

#### Group: (root)

- **Use Cases Overview** — `use-cases/overview` — Orientation: QRtub is industry-agnostic —
  same capabilities, different nouns — and why this tab is organized "By Industry" today with
  room for a "By Workflow" group later, so the two senses of "use case" never collide.

#### Group: By Industry

- **Construction Equipment** — `use-cases/construction` — Equipment fleets, one code per
  machine, Mitti/CMMS/manual destination scenarios, mixed-ownership fleets.
- **Contract Cleaning** — `use-cases/contract-cleaning` — Facility QR codes serving staff,
  clients, and the public simultaneously from one code.
- **Arboriculture & Tree Management** — `use-cases/arboriculture` — Tree-population inspection
  and works-history codes, with a public-education angle.
- **Electrical Test and Tag** — `use-cases/electrical-test-and-tag` — Compliance-register
  pattern shared by two audiences: in-house teams and contract test-and-tag providers.
- **Local Government & Councils** — `use-cases/local-councils` — Multi-department council
  assets, contractor coordination, and public issue reporting.

---

### Tab: Integrations

- **How Integrations Work** — `integrations/overview` — Orientation: every integration here is
  a URL/deep-link recipe QRtub builds and hands off to, not a native API connection — states
  the shared mechanism once before the platform-specific guides below.

#### Group: Mitti

- **Setting Up the Mitti Integration** — `integrations/mitti/setup` — The rename history
  (iAuditor → SafetyCulture → Mitti, old URLs still resolve), the two deep-link formats, and
  the core "start a new inspection" Destination recipe.
- **Mitti Entities You Can Link To** — `integrations/mitti/entities` — Reference table: view/
  edit an inspection, open an asset profile, view a document — each a different deep-link
  target.
- **Pre-Filling Mitti Inspections** — `integrations/mitti/prefilling` — Answering inspection
  questions automatically via URL parameters, the question-ID vs. field-name distinction, and
  the practical link-length ceiling.
- **Mitti Worked Examples** — `integrations/mitti/worked-examples` — Equipment/facility
  inspection scenarios, a multi-audience Page example, and a troubleshooting checklist.

#### Group: CMMS Systems

- **Setting Up a CMMS Integration** — `integrations/cmms-systems/setup` — The three-way
  decision tree (deep link / web URL / URL Template) for connecting to any maintenance
  platform, plus the vendor-lock-in-avoidance framing.
- **CMMS Worked Examples** — `integrations/cmms-systems/worked-examples` — A three-Destination
  UpKeep setup built from one field template, a multi-system Page example, and a common
  platforms reference table.

---

## Implementation notes (not part of the IA decision, but relevant to shipping it)

- Every retired page (`custom-fields.mdx`, `using-fields.mdx`, `conditional-visibility.mdx`,
  `device-detection.mdx`, `app-links.mdx`, `pages-overview.mdx`, `building-a-page.mdx`,
  `print-batches.mdx`, `media-basics.mdx`, `print-first-workflow.mdx`, and the never-published
  `print-shop-basics.mdx` / `preparing-print-job.mdx` drafts) needs a `docs.json` redirect to
  its replacement's new slug, following the existing `/help/*` → root redirect pattern.
- `key-concepts.mdx` needs an actual content trim, not just a nav move — at its current length
  it's playing the "Concepts" role in name only; the real depth needs to already live in the
  atomic pages above before this page can safely shrink.
- `industry:` frontmatter on the five Use Cases pages is dead (Mintlify navigates from
  `docs.json`, not frontmatter) and should be removed rather than carried forward, per the
  existing `sidebar_position`/`category`/`slug` precedent already flagged as inert elsewhere.
