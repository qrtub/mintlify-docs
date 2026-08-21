# Proposed Information Architecture — help.qrtub.com (Draft v2)

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
(Unchanged from v1's total — see Decision 6 below: one Help page was merged out, one new one
added, net zero, with two group counts shifting: Destinations 13 → 12, Links 8 → 9.)

**v2 changes what v1 got structurally wrong or left underspecified, found by running 45
realistic questions against titles + one-line descriptions only.** 10 came back ambiguous or
a gap. Every fix below traces to one of those 10 findings — see "What changed and why" at the
end for the full mapping. Decisions 1–5 are carried forward unchanged from v1; v2 adds
Decision 6.

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

### 6. v2: Fixing the 10 ambiguous/gap findings from the 45-question retrieval test

Two different failure shapes showed up, and they got two different kinds of fix:

**Description-only fixes (7 cases)** — the page split was structurally sound, but two or more
sibling descriptions each claimed the same fact or the same lexical territory without stating
the axis that actually separates them. Fix: rewrite the descriptions so each page's job is
narrowed to what only it covers, and add an explicit "see the other page for X" pointer in
both directions. No slugs changed, no pages moved.

**Structural fixes (3 cases)**:

- **Merge** (`destinations/field-renames-and-destinations` → folded into
  `fields/renaming-a-field` as a named subsection). This pair wasn't a mechanism/consumer
  split like Decision 2's Pages↔Destinations moves — the Destinations page's entire content
  was one fact ("renaming doesn't break a Destination that references the field") that just
  restated the Fields page's own claim from one consumer's point of view, with nothing else in
  it. That's a duplicate, not a sibling; Decision 4's own merge criteria apply. It becomes a
  named subsection so it's still independently findable.
- **Rescope + rename** (`links/deleting-and-releasing-links` → retitled "Deleting,
  Unassigning, and Releasing Links," widened to state the manual single-Link unassign action
  explicitly, and made the single canonical home for *every* trigger of Link release,
  including the Tub-deletion cascade previously half-claimed by `tubs/deleting-a-tub`).
  `tubs/deleting-a-tub` narrows to defer to it instead of restating the release mechanism.
- **New page** (`links/choosing-a-link-type`) — the tree had three atomic type pages and no
  comparison page, which is exactly the shape Linear avoids (it always pairs atomic pages with
  one page that puts them side by side, e.g. `print-shop/choosing-a-method` for VDP vs. Gang
  Sheets already does this correctly elsewhere in this same tree). Added as the missing
  sibling, following that existing precedent instead of inventing a new pattern.

The remaining two cases (Print Batch status vocabulary overlap; VDP/Gang Sheets already having
a comparison page whose description didn't say so loudly enough) turned out to need only the
description fix plus, in the Print Batch case, one named subsection making the contrast
explicit on the page most likely to be retrieved for a "what's the difference" query.

Full mapping of each of the 10 test findings to its specific fix is in "What changed and why"
at the end of this document.

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
  the page template a new Tub gets actually comes from — including a named subsection,
  "Starter template vs. page template versions," explaining why those are different
  timelines: a starter template is a one-time seed applied only at creation, while Page
  Template Versions (Pages group) is the ongoing save history that runs from then on
  regardless of how the Tub started. *(v2: strengthened per Decision 6 — resolves the
  Page-Template-Versions-vs-Starter-Templates ambiguity.)*
- **Starter Templates** — `tubs/starter-templates` — The 8-template gallery that seeds a new
  Tub's field schema, page sections, and sample Items in one step, applied once at creation
  only — canonical home for this feature (see Decision 5.1). What happens to the page template
  afterward is Page Template Versions' job, not this page's. *(v2: added the "once, at
  creation only" scoping.)*
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
- **Deleting a Tub** — `tubs/deleting-a-tub` — What's hard-deleted at the Tub level (Items, the
  page template, the Tub record itself) and the one thing that's deliberately not included:
  Links are never part of a Tub delete — they're released, a Links-domain mechanism this page
  defers to rather than restates; see "Deleting, Unassigning, and Releasing Links" in the
  Links group for the full mechanism and every trigger of it, Tub-deletion included. *(v2:
  narrowed per Decision 6 — resolves the ambiguity with `links/deleting-and-releasing-links`
  over which page owns the "printed codes keep working" fact.)*

#### Group: Fields

- **Core Fields vs. Custom Fields** — `fields/core-vs-custom` — The load-bearing split: 4 fixed
  core-field columns vs. everything else stored by a stable ID in `metadata.fields`.
- **Custom Field Types** — `fields/field-types` — The six field types side by side and what
  each gates — including the Multiple Values toggle (see Decision 4) and the two axes readers
  most often confuse with each other: a fixed picker list authored on the field itself
  (Allowed Values) vs. a live pointer to another record (Reference). This is the page to land
  on for "which type do I need," not either mechanism's own reference page. *(v2: added the
  explicit Allowed-Values-vs-Reference framing — resolves the three-way ambiguity with those
  two pages.)*
- **Allowed Values** — `fields/allowed-values` — The `{value, label, color}` picker list,
  authored directly on the field itself, that also drives chip color everywhere the field
  renders, including the scanned Page. Scoped to values stored on this field — a field that
  instead points at another record's own data is a Reference field, covered on its own page,
  not this one. *(v2: added the explicit boundary against Reference Fields.)*
- **Allow New Values** — `fields/allow-new-values` — The per-field toggle controlling whether a
  value outside the Allowed Values list can be typed in or imported via CSV.
- **Reference Fields** — `fields/reference-fields` — The field type that holds no value of its
  own — it's a live pointer to another Item, Tub, or team member, resolved at render/scan
  time, so no Allowed Values list ever applies to it. *(v2: added the explicit boundary
  against Allowed Values.)*
- **Field Defaults** — `fields/field-defaults` — The Tub-level fallback applied when an Item is
  created with a field left blank, including the destination-URL field's "stamped at
  creation, never retroactive" exception.
- **Required Fields** — `fields/required-fields` — The "Required" checkbox and its one
  carve-out: a partial CSV update is only checked against columns that row actually touches.
- **Creating a Custom Field** — `fields/creating-a-field` — Adding a field: label, auto-slugged
  key, type, and the field-key validation rules.
- **Renaming a Field** — `fields/renaming-a-field` — Why renaming is safe (the stable ID never
  changes), organized as one subsection per consumer that keeps working without you touching
  it: page-editor bindings (auto-update on save), Destinations that reference the field (a
  named subsection, "Destinations and renamed fields" — absorbed here from the former
  standalone `destinations/field-renames-and-destinations` page, which restated this exact
  page's own claim from one consumer's point of view and nothing else), and filters/CSV
  imports keyed by the field's slug. Single canonical answer to "does renaming break anything
  pointing at the old name," regardless of which consumer the reader has in mind. *(v2:
  absorbed the former Destinations page — resolves the ambiguity between the two by merging
  the duplicate rather than just re-describing it; see Decision 6.)*
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
- **What a Slug Is** — `links/what-a-slug-is` — The identifier segment of a Link's URL, and its
  one cross-type rule: random links are case-sensitive, numbered and custom links are not. For
  how the three link types actually differ in how a slug gets produced in the first place, see
  "Choosing a Link Type" below. *(v2: narrowed scope to the case-sensitivity fact only, with an
  explicit pointer — resolves the four-way ambiguity across the type pages.)*
- **Choosing a Link Type** — `links/choosing-a-link-type` — A side-by-side comparison of
  Random, Numbered, and Custom links: generation mechanism (auto-minted / claimed-range /
  user-chosen), who controls the slug, and the typical reason to reach for each. The
  orientation page to land on for "what's the real difference between these three," read
  before any one type's own page. *(v2: new page — closes the comparison-page gap flagged by
  the retrieval test; see Decision 6.)*
- **Random Links** — `links/random-links` — The default 5-character base62 link type, minted
  automatically, never user-chosen — full mechanism only; see "Choosing a Link Type" for how
  this compares to Numbered and Custom.
- **Numbered Links** — `links/numbered-links` — Claiming a `<prefix><N digits><suffix>` range
  as a team and minting against it — sequentially, at a specific number, or as a bulk range;
  see "Choosing a Link Type" for how this compares to Random and Custom.
- **Custom Links** — `links/custom-links` — User-chosen slugs, their charset rules, and the
  reserved-word / reserved-pattern blacklist; see "Choosing a Link Type" for how this compares
  to Random and Numbered.
- **Unallocated Links** — `links/unallocated-links` — A real, printable Link with nothing
  attached yet, why that's a deliberate feature (print-before-link, spares), and what a scan
  of one shows.
- **Claim-on-Scan** — `links/claim-on-scan` — The general mechanism: scanning an unrecognized
  third-party code mints a Link bound to a hash of its decoded text, idempotently.
- **Deleting, Unassigning, and Releasing Links** — `links/deleting-and-releasing-links` — The
  complete single-Link picture: hard delete (removes the Link outright; blocked once it's in
  a non-draft print batch) vs. release (the Link survives, becomes Unallocated, and keeps
  resolving). Covers both paths to release: the manual action (unassigning a Link from an Item
  that still exists, from the Links grid) and the two automatic triggers (deleting the Item a
  Link is attached to, or deleting its whole Tub). Canonical home for the Tub-deletion cascade
  fact as well as the Item-deletion one — `tubs/deleting-a-tub` defers here instead of
  restating it, and `bulk-links/assign-unassign-delete` defers here for what unassign actually
  does to the Link/Item relationship, keeping its own page scoped to bulk-operation mechanics.
  *(v2: retitled and widened per Decision 6 — resolves both the Tub-deletion-cascade ambiguity
  with `tubs/deleting-a-tub` and the delete-vs-unassign ambiguity with
  `bulk-links/assign-unassign-delete`.)*

#### Group: Bulk Link Operations

- **Bulk Link Import via CSV** — `bulk-links/csv-import` — Column schema, per-type create
  rules, and the dry-run preview for creating/updating many Links from one file.
- **Bulk Assigning, Unassigning, and Deleting Links** — `bulk-links/assign-unassign-delete` —
  Bulk-operation mechanics only: acting on an explicit ID list vs. an entire filtered/searched
  scope, including "select all." For what unassigning actually does to a single Link/Item
  relationship (delete vs. release vs. manual detach), see "Deleting, Unassigning, and
  Releasing Links" in the Links group — this page assumes that concept rather than
  re-explaining it. *(v2: narrowed scope with an explicit pointer — resolves the ambiguity with
  `links/deleting-and-releasing-links`; see Decision 6.)*
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
  version, regardless of whether the Tub started from a Starter Template or from blank;
  viewing history, restoring an old version, cloning a template to another Tub (placed here,
  not under Tubs — see Decision 5.6). Starter Templates seeds this history once at creation;
  this page is what runs from then on. *(v2: added the reciprocal framing against Starter
  Templates — resolves the ambiguity with `tubs/starter-templates`.)*
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
  `theme`) named as a directory in one line each, and the no-auto-encoding rule. Syntax and
  directory only — what values each namespace actually exposes, and where a namespace runs
  out (for example, no date field in `time`), lives on that namespace's own reference page.
  *(v2: narrowed to syntax-only with an explicit pointer, so this page stops competing with
  the namespace reference pages on a query about what a specific namespace can do.)*
- **Item Fields Reference** — `destinations/item-fields` — The catalog of `item.*` bindings:
  standard fields plus whatever custom fields a Tub defines.
- **Tub Fields Reference** — `destinations/tub-fields` — The catalog of `tub.*` bindings.
- **Time Fields Reference** — `destinations/time-fields` — `time.hour`/`dayOfWeek`/etc.,
  computed fresh per scan in UTC — and, as a named subsection, "Why you can't check if
  something is overdue": there is no `time.today` and no absolute date value or date math, so
  "is this inspection overdue" cannot be expressed with `time.*` at all; use a status field you
  maintain yourself instead (`item.testStatus == "expired"`). Canonical page for any question
  phrased as an invented `time.*` value or a date-comparison attempt. *(v2: promoted this from
  an implied absence to a named, explicitly-titled subsection — resolves the ambiguity with
  Conditional Visibility and Field Bindings over which page owns the "no dates" fact; matches
  the known documented failure mode in `CLAUDE.md`.)*
- **Request Fields Reference** — `destinations/request-fields` — `request.ip`/`country`/`city`/
  etc. sourced from CDN geo headers, with the privacy consequence of using them stated plainly.
- **Conditional Visibility** — `destinations/conditional-visibility` — Hiding a section with a
  CEL `condition`; the silent-`false` behavior of any undefined identifier (a typo, or a field
  that doesn't exist on this Tub). For the specific case of an invented date-like value such as
  `today`, see Time Fields Reference, which owns that fact and explains why no such value
  exists at all. *(v2: replaced the "today" example with a pointer to Time Fields Reference —
  resolves the ambiguity that example created; see Decision 6.)*
- **Conditional Destinations & Rule Priority** — `destinations/conditional-destinations` — The
  ordered `destination_config.rules` list on a single Destination: first match wins, plus a
  `defaultLink` catch-all — a routing mechanism, distinct from Conditional Visibility's
  show/hide (see Decision 2).
- **Device Detection & Routing** — `destinations/device-detection` — `device.*` bindings and
  routing patterns (app vs. web, per-platform app store, tablet/browser-specific) for choosing
  *which URL* to send a scan to, based on the phone's declared OS/browser/tablet type — a
  device-class routing decision, not app-open detection, and explicitly not a security
  mechanism. See App Links & Fallback URLs for whether one specific app actually opened. *(v2:
  named the distinguishing axis explicitly — resolves the ambiguity with App Links & Fallback
  URLs.)*
- **App Links & Fallback URLs** — `destinations/app-links` — Deep-link URLs (any non-`http(s)`
  scheme, including `tel:`), the three-level fallback (Destination > Link > Item), and the
  real visibility-based mechanism for detecting whether one specific app actually opened after
  being dispatched — independent of device type. See Device Detection & Routing for choosing
  between destinations by device class instead. *(v2: named the distinguishing axis explicitly
  — resolves the ambiguity with Device Detection & Routing.)*
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
  Printed → Deployed lifecycle and the named forward/backward actions at each step — one status
  value for the whole batch. See Tracking Deployment Status Per Code for the separate,
  per-code state that reuses two of these same words at a different scope. *(v2: added the
  explicit "one value for the whole batch" framing and pointer — resolves the ambiguity with
  Tracking Deployment Status Per Code.)*
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
  independent Printed/Deployed/Retired state tracked per individual link, once a batch as a
  whole reaches Deployed — plus a named subsection, "How this differs from the batch's own
  status," spelling out why the same two words (Printed, Deployed) mean something different
  here than at the batch level: batch status is one value for the whole export; this is N
  independent values, one per code, that only start existing once the batch itself reaches
  Deployed. *(v2: added the explicit contrast subsection — resolves the ambiguity with Batch
  Status: Draft to Deployed; see Decision 6.)*
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

- **Choosing Your Print Production Method** — `print-shop/choosing-a-method` — The direct,
  named comparison of Variable Data Printing (VDP) against Gang Sheets: one question — does
  the shop's equipment merge live per piece, or need one flattened file laid out in advance —
  decided before reading either method's own page in detail. *(v2: named both terms explicitly
  in the description so this page competes on the same lexical terms its two sibling pages do
  — resolves the ambiguity between all three.)*
- **Variable Data Printing (VDP)** — `print-shop/vdp` — Digital presses imaging each piece
  individually from a data file, live, as the run goes — mechanism only; see Choosing Your
  Print Production Method for how this compares to Gang Sheets.
- **Gang Sheets and Composite-Sheet Production** — `print-shop/gang-sheets` — Materials that
  can only reproduce one static image per pass, and why every unique code has to be laid out
  in its final position before that single pass runs — mechanism only; see Choosing Your Print
  Production Method for how this compares to VDP.
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
- v2 adds one more retired slug to that list: `destinations/field-renames-and-destinations`
  (merged into `fields/renaming-a-field` — see Decision 6) needs the same redirect treatment if
  it was ever published, even in draft form.
- `key-concepts.mdx` needs an actual content trim, not just a nav move — at its current length
  it's playing the "Concepts" role in name only; the real depth needs to already live in the
  atomic pages above before this page can safely shrink.
- `industry:` frontmatter on the five Use Cases pages is dead (Mintlify navigates from
  `docs.json`, not frontmatter) and should be removed rather than carried forward, per the
  existing `sidebar_position`/`category`/`slug` precedent already flagged as inert elsewhere.
- `links/deleting-and-releasing-links`'s widened scope (v2) needs its manual-unassign claim
  verified against `../qrtub/src/` before drafting — confirm a single-Link "unassign" action
  actually exists outside the bulk-operations UI before the page asserts it as a feature, per
  `CLAUDE.md`'s "document how the code actually works" rule.
- `destinations/time-fields`'s new "why you can't check if something is overdue" subsection
  should be written directly from the language already verified in `CLAUDE.md`'s "Conditional
  visibility" section, not re-derived — that section already states the exact limitation and
  the recommended workaround.

---

## What changed and why

Every fix below resolves one specific finding from the 45-question retrieval test (verdicts
quoted from that test's output).

1. **"If I delete a whole Tub, do the physical QR codes already stuck on my equipment stop
   working?" (ambiguous: `tubs/deleting-a-tub` vs. `links/deleting-and-releasing-links`)** —
   **Structural rescope.** `links/deleting-and-releasing-links` (retitled "Deleting,
   Unassigning, and Releasing Links") is now the sole canonical home for the release mechanism
   and *every* trigger of it, Tub-deletion included. `tubs/deleting-a-tub`'s description was
   narrowed to state only what's hard-deleted at the Tub level and explicitly defer to the
   Links page for what survives — it no longer makes its own competing claim about printed
   codes.

2. **"What's the actual difference between deleting a Link and just unassigning it from an
   Item?" (ambiguous: `links/deleting-and-releasing-links` vs.
   `bulk-links/assign-unassign-delete`)** — **Structural rescope, same page as #1.** The Links
   page's description now explicitly states the manual single-Link unassign action (not just
   the two automatic triggers), closing the gap the test found — that a user can manually
   detach a Link from a still-existing Item was previously unstated anywhere. The bulk page's
   description was narrowed to bulk-operation mechanics only, with an explicit pointer back to
   the Links page for the conceptual delete-vs-unassign question, so it stops competing on the
   word "unassigning" for a single-item question it was never scoped to answer.

3. **"If I rename a custom field, does it break anything that was already pointing at the old
   name?" (ambiguous: `fields/renaming-a-field` vs.
   `destinations/field-renames-and-destinations`)** — **Merge.** The Destinations page's entire
   content was one fact restating the Fields page's own claim from a single consumer's point
   of view — a duplicate per Decision 4's own criteria, not a genuine mechanism/consumer split
   like the ones Decision 2 established. Folded into `fields/renaming-a-field` as a named
   subsection ("Destinations and renamed fields") alongside the other consumers (page bindings,
   filters/CSV), so there's exactly one page that answers this question regardless of which
   consumer the reader has in mind.

4. **"What's a Reference field, and how is that different from just picking a value off a
   list?" (ambiguous: `fields/reference-fields` vs. `fields/allowed-values` vs.
   `fields/field-types`)** — **Description fix.** All three descriptions now state the actual
   distinguishing axis (value stored on the field itself vs. a live pointer elsewhere) in both
   directions, and `field-types` is explicitly reframed as the "which type do I need" decision
   page that names both mechanisms together, matching the comparative shape of the question.

5. **"What's the difference between a Page Template Version and a Starter Template?"
   (ambiguous: `pages/page-template-versions` vs. `tubs/starter-templates` vs.
   `tubs/creating-a-tub`)** — **Description fix + subsection.** Added a named subsection to
   `tubs/creating-a-tub` ("Starter template vs. page template versions") stating the timeline
   distinction directly, and added reciprocal one-line framing to the other two pages ("seeded
   once" vs. "runs from then on") so any one of the three pages, read alone, states where the
   other two fit.

6. **"What's the real difference between a Random Link, a Numbered Link, and a Custom Link?"
   (gap: no comparison page existed)** — **New page.** Added `links/choosing-a-link-type`,
   following the same pattern `print-shop/choosing-a-method` already uses for VDP vs. Gang
   Sheets elsewhere in this tree. The three type pages and `what-a-slug-is` now point to it
   instead of each answering a fragment of the comparison on its own.

7. **"Don't App Links/Fallback URLs and Device Detection both just decide where a scan goes
   based on the phone? What's actually different about them?" (ambiguous:
   `destinations/device-detection` vs. `destinations/app-links`)** — **Description fix.** Both
   descriptions now name the actual distinguishing axis explicitly — device-class URL routing
   vs. detecting whether one specific app opened — with a reciprocal pointer, so the axis no
   longer lives only in the planning prose (Decision 2) that a reader/retriever never sees.

8. **"Can I write something like {{time.today}} in a Destination to check if an inspection is
   overdue?" (ambiguous: `destinations/time-fields` vs. `destinations/conditional-visibility`
   vs. `destinations/field-bindings`)** — **Description fix + subsection.** Added a named,
   explicitly-titled subsection to `destinations/time-fields` ("Why you can't check if
   something is overdue") stating the limitation and the workaround in the same terms
   `CLAUDE.md` already documents. Removed the competing "today" example from
   `conditional-visibility`'s description (replaced with a generic typo example) and pointed it
   at Time Fields Reference instead, and narrowed `field-bindings` to syntax-only so it stops
   competing on namespace-specific capability questions.

9. **"What's the difference between a Print Batch's overall status and the 'deployment status'
   of one code inside it?" (ambiguous: `print-batches/batch-status` vs.
   `print-batches/deployment-status`)** — **Description fix + subsection.** Added a named
   subsection to `deployment-status` ("How this differs from the batch's own status")
   explaining the vocabulary overlap directly (both use "Printed"/"Deployed," but one is a
   single batch-wide value and the other is N independent per-code values), and added a
   reciprocal one-line pointer to `batch-status`.

10. **"What's the difference between Variable Data Printing and Gang Sheets at a print shop?"
    (ambiguous: `print-shop/choosing-a-method` vs. `print-shop/vdp` vs.
    `print-shop/gang-sheets`)** — **Description fix.** `choosing-a-method`'s description now
    names both terms verbatim and states outright that it's the head-to-head comparison, so it
    competes on the same lexical terms its two sibling pages do instead of only describing
    itself abstractly. The two entity pages now explicitly defer to it for the comparison,
    keeping their own descriptions to mechanism only.
