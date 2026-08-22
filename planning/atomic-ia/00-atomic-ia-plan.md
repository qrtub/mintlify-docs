# The Atomic IA Plan — help.qrtub.com

One document, six parts. Part 1 is the tree itself — read that first if you only read one
thing. Everything after it is the reasoning, so you can defend any branch of the tree without
re-deriving it.

**Bottom line up front:** 127 pages, 3 tabs, 19 groups (corrected from the workflow's original
128 — see Part 6 for why "Transferring Ownership" was merged into "Leaving a Team" after
verification). Nothing here is invented — every page
below traces to a line of app code or an existing draft, cited in the inventory files this
plan was built from (`planning/atomic-ia/inventory/*.md`). Where something is a genuine
judgment call rather than a fact, it's flagged as such in Part 6, not smoothed over.

---

## 1. The Tree

Read as: **Tab → Group → Page** — `slug` → one-line description. Every URL is
`help.qrtub.com/<slug>` except where noted (Use Cases and Integrations tabs use their own
path prefixes, shown per tab).

### Tab: Help — `help.qrtub.com/`

- **QRtub Documentation** — `help.qrtub.com/` — Homepage: what QRtub is, one screen, links into everything below.

**Getting Started**
- **Creating Your First Link** — `help.qrtub.com/creating-your-first-link` — End-to-end tutorial: create a Collection, get a Link, connect it to an Item.
- **Key Concepts** — `help.qrtub.com/key-concepts` — One sentence each on Collection, Item, Link, Page, Media — points outward, doesn't re-explain.

**Collections**
- **What Is a Collection?** — `help.qrtub.com/collections/overview` — The entity grouping Items under one schema, link rule, scan behavior, page template.
- **Creating a Collection** — `help.qrtub.com/collections/creating-a-collection` — The creation fork, auto-generated name, where the page template comes from.
- **Starter Templates** — `help.qrtub.com/collections/starter-templates` — The 8-template gallery seeding a new Collection's fields, page, and sample Items at creation only.
- **Collection Details** — `help.qrtub.com/collections/collection-details` — Name, description, custom Items label, cover image — display-only.
- **Link Generation for New Items** — `help.qrtub.com/collections/link-generation-modes` — Collection-level setting: auto-mint a Link on Item creation, and which of 3 modes.
- **Building an Item ID Mask** — `help.qrtub.com/collections/item-id-mask` — Prefix/suffix/digit-count editor for ID-based links, plus mask-conflict rules.
- **Scan Behavior for New Items** — `help.qrtub.com/collections/scan-behavior-default` — Collection-level default: new Item starts as Direct Mode or Page Mode.
- **Default Destination for New Items** — `help.qrtub.com/collections/default-destination` — CEL-expression builder for the URL template stamped onto new Items at creation.
- **Exporting a Collection Backup** — `help.qrtub.com/collections/export-backup` — JSON snapshot of settings, fields, page template — Items excluded.
- **Importing a Collection Backup** — `help.qrtub.com/collections/import-backup` — Restoring into an existing Collection: Merge (additive) vs. Replace (destructive).
- **Creating a New Collection From a Backup** — `help.qrtub.com/collections/new-collection-from-backup` — Spin up a brand-new Collection from a backup file — the closest thing to "duplicate."
- **Deleting a Collection** — `help.qrtub.com/collections/deleting-a-collection` — What's hard-deleted at the Collection level; defers to Links for what survives.

**Fields**
- **Core Fields vs. Custom Fields** — `help.qrtub.com/fields/core-vs-custom` — 4 fixed core columns vs. everything else stored by stable ID.
- **Custom Field Types** — `help.qrtub.com/fields/field-types` — The six types side by side, what each gates, and the "which type do I need" decision.
- **Allowed Values** — `help.qrtub.com/fields/allowed-values` — The `{value, label, color}` list authored on the field itself; drives chip color everywhere.
- **Allow New Values** — `help.qrtub.com/fields/allow-new-values` — Toggle: can a value outside the Allowed Values list be typed or CSV-imported.
- **Reference Fields** — `help.qrtub.com/fields/reference-fields` — Field type holding no value of its own — a live pointer to another Item, Collection, or member.
- **Field Defaults** — `help.qrtub.com/fields/field-defaults` — Collection-level fallback for a blank field on Item creation; destination-URL exception noted.
- **Required Fields** — `help.qrtub.com/fields/required-fields` — The "Required" checkbox and the partial-CSV-update carve-out.
- **Creating a Custom Field** — `help.qrtub.com/fields/creating-a-field` — Label, auto-slugged key, type, validation rules.
- **Renaming a Field** — `help.qrtub.com/fields/renaming-a-field` — Why it's safe (stable ID never changes), and what auto-updates: page bindings, Destinations, CSV/filters.
- **Deleting and Disabling Fields** — `help.qrtub.com/fields/deleting-and-disabling` — Core fields can only be disabled; custom fields can be permanently deleted.

**Items**
- **What Is an Item?** — `help.qrtub.com/items/overview` — The individual record a Collection tracks, including the Item image system field.
- **Name and Description** — `help.qrtub.com/items/name-and-description` — The two free-text core fields every Item ships with.
- **Item ID** — `help.qrtub.com/items/item-id` — The per-Collection-unique identifier field, blank/whitespace normalization, role in ID-based links.
- **Tags** — `help.qrtub.com/items/tags` — The core `tags` array field, default free-form additions, colored-chip rendering.
- **Duplicating an Item** — `help.qrtub.com/items/duplicating` — What "Copy" carries over vs. drops (Item ID, the source's page override).

**Import & Export**
- **Importing Items from CSV** — `help.qrtub.com/import-export/importing-items` — Bulk create/update by `id`, per-row validation, dry-run, 10 MB / 10,000-row limits.
- **Exporting Items to CSV** — `help.qrtub.com/import-export/exporting-items` — Visible-columns vs. full-field CSV, scoped by current search/filter/sort.

**Links**
- **What a Link Is** — `help.qrtub.com/links/what-a-link-is` — A slug that resolves to a destination; why it behaves like an ordinary URL shortener.
- **What a Slug Is** — `help.qrtub.com/links/what-a-slug-is` — The identifier segment of a Link's URL, and the case-sensitivity split by type.
- **Choosing a Link Type** — `help.qrtub.com/links/choosing-a-link-type` — Side-by-side comparison: Random vs. Numbered vs. Custom, and when to reach for each.
- **Random Links** — `help.qrtub.com/links/random-links` — The default 5-char base62 link, minted automatically, never user-chosen.
- **Numbered Links** — `help.qrtub.com/links/numbered-links` — Claiming a `<prefix><N digits><suffix>` range and minting against it.
- **Custom Links** — `help.qrtub.com/links/custom-links` — User-chosen slugs, charset rules, reserved-word blacklist.
- **Unallocated Links** — `help.qrtub.com/links/unallocated-links` — A real, printable Link with nothing attached yet — why that's a deliberate feature.
- **Claim-on-Scan** — `help.qrtub.com/links/claim-on-scan` — Scanning an unrecognized third-party code mints a Link bound to a hash, idempotently.
- **Deleting, Unassigning, and Releasing Links** — `help.qrtub.com/links/deleting-and-releasing-links` — Hard delete vs. manual unassign vs. automatic release (Item or Collection deletion) — the single canonical page for all three.

**Bulk Link Operations**
- **Bulk Link Import via CSV** — `help.qrtub.com/bulk-links/csv-import` — Column schema, per-type create rules, dry-run preview.
- **Bulk Assigning, Unassigning, and Deleting Links** — `help.qrtub.com/bulk-links/assign-unassign-delete` — Bulk-operation mechanics: explicit ID list vs. filtered/searched scope, "select all."
- **Downloading QR Codes** — `help.qrtub.com/bulk-links/downloading-qr-codes` — Single PNG vs. zipped bundle; states plainly there's no SVG export or configurable error correction today.

**Pages**
- **Pages Overview** — `help.qrtub.com/pages/pages-overview` — What a Page is, the 4-step flow to turn one on for a Collection.
- **Direct Mode vs. Page Mode** — `help.qrtub.com/pages/direct-mode-vs-page-mode` — Per-Collection default / per-Item override between an immediate redirect and a multi-destination Page.
- **Page Privacy: Public vs. Private** — `help.qrtub.com/pages/page-privacy` — Collection- and Item-level gates requiring sign-in to view; effect on `noindex`.
- **The Page Editor Layout** — `help.qrtub.com/pages/page-editor-layout` — The three-tab panel, the Properties panel, the undo-history gotcha.
- **Section Types** — `help.qrtub.com/pages/section-types` — The catalog of all 17 section types by category — a map, not a page-per-section.
- **ActionLink: The Destination Button** — `help.qrtub.com/pages/action-link` — The one section that *is* a Destination button; auto-hides, groups visually.
- **AdminToolbar** — `help.qrtub.com/pages/admin-toolbar` — Configurable button bar, defaults to signed-in-only visibility.
- **Image Display Controls** — `help.qrtub.com/pages/image-display-controls` — Shared Frame Shape / Fit / Align system, reused by ImageSection and ItemHeader.
- **Theming a Page** — `help.qrtub.com/pages/page-theming` — 7 theme presets, accent color, radius, typography scale.
- **Previewing a Page** — `help.qrtub.com/pages/previewing-a-page` — Item selector, 5 responsive widths, chrome-hiding Preview toggle.
- **Per-Item Page Overrides** — `help.qrtub.com/pages/page-overrides` — Override toggle: sparse diff on one Item vs. updating every Item on the base template.
- **Page Template Versions** — `help.qrtub.com/pages/page-template-versions` — Every save creates a version; history, restore, clone to another Collection.
- **Page Metadata & Social Previews** — `help.qrtub.com/pages/page-metadata` — Auto-generated OG/Twitter tags, the `noindex, follow` split, private-Page minimal metadata.

**Destinations**
- **What Is a Destination?** — `help.qrtub.com/destinations/what-is-a-destination` — Where a scan is routed, real resolution order, safety filter, "not ready yet" state.
- **Field Bindings & URL Templates** — `help.qrtub.com/destinations/field-bindings` — The `{{ }}` syntax itself, the namespace directory, the no-auto-encoding rule.
- **Item Fields Reference** — `help.qrtub.com/destinations/item-fields` — Catalog of `item.*` bindings: standard fields plus any Collection's custom fields.
- **Collection Fields Reference** — `help.qrtub.com/destinations/collection-fields` — Catalog of `tub.*` bindings.
- **Time Fields Reference** — `help.qrtub.com/destinations/time-fields` — `time.hour`/`dayOfWeek`/etc., computed per scan in UTC — and why "overdue" can't be expressed.
- **Request Fields Reference** — `help.qrtub.com/destinations/request-fields` — `request.ip`/`country`/`city`/etc. from CDN geo headers; the privacy consequence stated plainly.
- **Conditional Visibility** — `help.qrtub.com/destinations/conditional-visibility` — Hiding a section with a CEL `condition`; undefined identifiers silently evaluate false.
- **Conditional Destinations & Rule Priority** — `help.qrtub.com/destinations/conditional-destinations` — Ordered `destination_config.rules`: first match wins, plus a `defaultLink` catch-all.
- **Device Detection & Routing** — `help.qrtub.com/destinations/device-detection` — `device.*` bindings and routing by device class — not a security mechanism.
- **App Links & Fallback URLs** — `help.qrtub.com/destinations/app-links` — Deep-link URLs, the 3-level fallback, detecting whether one specific app actually opened.
- **Connecting a Generic System** — `help.qrtub.com/destinations/connecting-a-generic-system` — Methodology for any platform with no dedicated guide.
- **How to Reverse-Engineer a Platform's Deep Links** — `help.qrtub.com/destinations/reverse-engineering-deep-links` — Finding an undocumented app's deep-link scheme.

**Media**
- **What Is Media?** — `help.qrtub.com/media/what-is-media` — The physical material a QR code sits on — a third entity, distinct from Link and Item.
- **Choosing Media for Your Deployment** — `help.qrtub.com/media/media-types` — Comparison guide by cost, durability, use case — general reference, not app-tracked.

**Print Batches**
- **Print Batches** — `help.qrtub.com/print-batches/overview` — A batch is the record of one CSV print-list export.
- **Creating a Print Batch** — `help.qrtub.com/print-batches/creating-a-batch` — Select Items or Links, pick columns, export.
- **Batch Status: Draft to Deployed** — `help.qrtub.com/print-batches/batch-status` — The Draft → Printing → Printed → Deployed lifecycle — one value for the whole batch.
- **Archiving and Deleting Batches** — `help.qrtub.com/print-batches/archiving-and-deleting` — Archive/unarchive (reversible) vs. hard delete (Draft-only).
- **Naming, Tagging and Photographing a Batch** — `help.qrtub.com/print-batches/batch-details` — Inline rename, notes, tag chips, cover photo.
- **Editing a Batch's Columns** — `help.qrtub.com/print-batches/editing-columns` — Changing CSV columns while still Draft; locks once past Draft.
- **Adding and Removing Links in a Batch** — `help.qrtub.com/print-batches/editing-links` — Two entry points into the same Draft-only rule.
- **Tracking Deployment Status Per Code** — `help.qrtub.com/print-batches/deployment-status` — Independent Printed/Deployed/Retired state per code — N values, not one.
- **The Batch CSV: Downloading and Reprinting** — `help.qrtub.com/print-batches/csv-download` — The exact stored CSV a reprint uses, server-streamed.
- **Finding and Filtering Your Batches** — `help.qrtub.com/print-batches/finding-batches` — Summary cards, filters, the "show archived" toggle.
- **Viewing a Link's Print History** — `help.qrtub.com/print-batches/link-print-history` — Every batch a specific Link has appeared in, from the Link's side.

**Print-First Workflow**
- **The Print-First Workflow** — `help.qrtub.com/print-first/overview` — Why codes get printed before the Items they'll represent exist.
- **The In-App QR Scanner** — `help.qrtub.com/print-first/scanner` — Camera / manual-paste / USB-gun scanner from the top navbar.
- **Adopting an Unknown Code From the Scanner** — `help.qrtub.com/print-first/claim-on-scan` — The scanner's "Create & open" action and its optional photo capture.
- **Tips for Print-First Deployments** — `help.qrtub.com/print-first/tips` — Practical advice too short individually, real together.

**Working with a Print Shop**
- **Choosing Your Print Production Method** — `help.qrtub.com/print-shop/choosing-a-method` — VDP vs. Gang Sheets, named head-to-head.
- **Variable Data Printing (VDP)** — `help.qrtub.com/print-shop/vdp` — Digital presses imaging each piece individually, live.
- **Gang Sheets and Composite-Sheet Production** — `help.qrtub.com/print-shop/gang-sheets` — Materials needing one flattened, pre-laid-out file per pass.
- **Preparing Your Print Job** — `help.qrtub.com/print-shop/preparing-your-job` — Orientation checklist: spec, data file, codes, matching, proof.
- **QR Code Print Spec: Quiet Zone and Minimum Size** — `help.qrtub.com/print-shop/quiet-zone-and-size` — 4-module quiet zone, ~20–25mm minimum size.
- **Matching QR Codes to Data Rows** — `help.qrtub.com/print-shop/matching-codes-to-rows` — Matching a downloaded image's filename to its CSV row by slug.
- **What to Expect When a Shop Redraws Your File** — `help.qrtub.com/print-shop/shop-redraws-your-file` — Why a shop rebuilding your design isn't a problem sign.
- **Getting and Scanning a Proof** — `help.qrtub.com/print-shop/getting-a-proof` — Requesting and scanning a real-data proof before the full run.

**Account**
- **Account Overview** — `help.qrtub.com/account/overview` — Read-only identity panel: email, created date, verification, auth provider, teams.
- **Change Your Password** — `help.qrtub.com/account/change-password` — The password-change form and why it's server-routed.

**Team**
- **Team Overview** — `help.qrtub.com/team/overview` — The shared workspace that owns Collections, holds members, optionally carries one subscription.
- **Switching Between Teams** — `help.qrtub.com/team/switching-teams` — The Team Switcher, its `localStorage` persistence, the dashboard-reset effect.
- **Creating a Team** — `help.qrtub.com/team/create-a-team` — Name, auto-generated slug, optional logo, immediate ownership.
- **Team Settings** — `help.qrtub.com/team/team-settings` — Renaming, URL slug, logo — owner-only.
- **Team Roles** — `help.qrtub.com/team/roles` — Owner vs. Editor; the "Owner" role label alone grants nothing — only Transfer Ownership does.
- **Inviting Team Members** — `help.qrtub.com/team/invite-members` — Two invite paths, the 50/day rate limit, active-subscription requirement.
- **Managing Pending Invitations** — `help.qrtub.com/team/pending-invitations` — Email-only pending invites, Resend and Revoke.
- **Accepting a Team Invitation** — `help.qrtub.com/team/accept-invitation` — Explicit Accept/Reject vs. auto-join on first login.
- **Removing Team Members** — `help.qrtub.com/team/remove-members` — Bulk-selecting and removing members, owner-only.
- **Leaving a Team** — `help.qrtub.com/team/leave-a-team` — Why a non-owner can leave immediately, and why an owner leaving auto-transfers to the first active member first — verified: there is no standalone "Transfer Ownership" feature anywhere in the app (one call site, `handleLeaveTeam`), so this isn't a separate page — see the correction note in Part 6.

**Billing**
- **Billing Is Per-Team** — `help.qrtub.com/billing/per-team-billing` — Why subscriptions attach to a team, never a user account.
- **Plans Overview** — `help.qrtub.com/billing/plans-overview` — Three self-serve tiers plus an offline-only Enterprise tier — structure, not prices.
- **Subscribing to a Plan** — `help.qrtub.com/billing/subscribe` — Public checkout: pick a plan, enter email, account+team+billing created together.
- **Upgrading a Team's Plan** — `help.qrtub.com/billing/upgrade-a-team` — In-app upgrade for an already-signed-in owner.
- **Managing Your Subscription** — `help.qrtub.com/billing/customer-portal` — Stripe Customer Portal hand-off for payment method, invoices, cancellation.
- **Subscription Status** — `help.qrtub.com/billing/subscription-status` — What `active`/`trialing`/`past_due`/`canceled` each mean for feature access.
- **Plan Limits & Quotas** — `help.qrtub.com/billing/plan-limits` — What varies by plan, as categories — no hardcoded numbers.

**Workspace**
- **Search Everything** — `help.qrtub.com/workspace/search-everything` — Global search across Collections, Items, Links, Pages; category filter; recent-searches history.

### Tab: Use Cases — `help.qrtub.com/use-cases/`

- **Use Cases Overview** — `help.qrtub.com/use-cases/overview` — QRtub is industry-agnostic; why this tab is "By Industry" today with room for "By Workflow" later.

**By Industry**
- **Construction Equipment** — `help.qrtub.com/use-cases/construction` — Equipment fleets, one code per machine, mixed-ownership fleets.
- **Contract Cleaning** — `help.qrtub.com/use-cases/contract-cleaning` — Facility codes serving staff, clients, and the public from one code.
- **Arboriculture & Tree Management** — `help.qrtub.com/use-cases/arboriculture` — Tree-population inspection and works-history codes, public-education angle.
- **Electrical Test and Tag** — `help.qrtub.com/use-cases/electrical-test-and-tag` — Compliance-register pattern for in-house teams and contract providers.
- **Local Government & Councils** — `help.qrtub.com/use-cases/local-councils` — Multi-department assets, contractor coordination, public issue reporting.

### Tab: Integrations — `help.qrtub.com/integrations/`

- **How Integrations Work** — `help.qrtub.com/integrations/overview` — Every integration is a URL/deep-link recipe, not a native API connection.

**Mitti**
- **Setting Up the Mitti Integration** — `help.qrtub.com/integrations/mitti/setup` — Rename history (iAuditor → SafetyCulture → Mitti), deep-link formats, the core recipe.
- **Mitti Entities You Can Link To** — `help.qrtub.com/integrations/mitti/entities` — Reference table: inspection, asset profile, document — each a different target.
- **Pre-Filling Mitti Inspections** — `help.qrtub.com/integrations/mitti/prefilling` — Answering questions via URL parameters, question-ID vs. field-name, length ceiling.
- **Mitti Worked Examples** — `help.qrtub.com/integrations/mitti/worked-examples` — Scenarios, multi-audience Page example, troubleshooting checklist.

**CMMS Systems**
- **Setting Up a CMMS Integration** — `help.qrtub.com/integrations/cmms-systems/setup` — The three-way decision tree (deep link / web URL / URL Template) for any platform.
- **CMMS Worked Examples** — `help.qrtub.com/integrations/cmms-systems/worked-examples` — A three-Destination UpKeep setup, multi-system Page, platforms reference table.

**Totals:** Help 114 pages (17 groups) + Use Cases 6 pages + Integrations 7 pages = **127 pages**
(one merged out of Help's Team group post-verification — see Part 6).

**Deliberately excluded from this draft** (not an oversight): QR SVG export / error-correction
level (unshipped — planned brief only); Jotform, Google Maps, and Google Sheets integrations
(no shipped feature yet to document, and Google Sheets has an undecided integration direction
that would change which tab it belongs to). All four stay out of `docs.json` until something
real exists to write about.

---

## 2. The three decisions you asked for

### Can Collection settings stand alone from Items, or are they conflated?

**Decision: they split cleanly into three siblings — Collections, Fields, Items — none nested
inside another.**

The instinct to worry about this was right — a Collection's custom-field *schema* (what fields
exist, what type, what's required) sits right next to Item *data* (what's actually typed
into those fields) in most people's mental model, and it would have been easy to conflate
them into one bloated "Collection settings" group.

They don't conflate, for a reason stronger than "it felt cleaner": the code enforces the
separation. `FieldConfig` (the schema) lives exclusively in `tub.metadata.fieldConfig`, is
read and written only through Collection API routes, and no code path for *defining* a field ever
touches an Item, and vice versa. The existing (unpublished) `custom-fields.mdx` proves this
in practice too — 119 lines that fully explain defining a field and never once explain
filling one in.

So the schema domain — which turned out to be big enough on its own (10 pages: types,
allowed values, allow-new-values, reference fields, defaults, required, create/rename/delete)
— became its own sibling, "Fields," rather than a subsection of either Collections or Items. That
mirrors how Linear itself keeps "Issues" and "Issue properties" as separate, adjacent sidebar
groups even though you edit an issue's properties from inside the issue.

The one real connection point — a Collection-level default value that fills in a blank field when
an Item is created — is handled as a one-sentence cross-reference between `fields/field-defaults`
and `collections/default-destination`, not a shared page. Everything else Collection-level (link
generation, the item-ID mask, scan-behavior default, backup/restore, deletion) is even more
clearly Collection-only — none of it requires explaining a specific Item's data to describe
completely.

**Final shape:** Collections (12 pages) · Fields (10 pages) · Items (5 pages).

### Which pages moved into the new "Destinations" category, and which stayed in "Pages"?

**Decision: App Links, Device Detection, and Conditional Visibility all move to the new
Destinations group. Only `pages-overview` and the page-building/editor content stay in
Pages.**

The test for each was: does this feature depend on the Page Editor, or does it work
identically for a plain Direct-Mode Link with no Page at all?

- **App Links & Fallback URLs moves.** A Direct-Mode Link with no Page uses the exact same
  `AppLinkOpener` mechanism. Zero Page Editor dependency.
- **Device Detection & Routing moves.** It's a `device.*` binding reference plus routing
  patterns. It only ever lived under "Pages" because Destinations didn't have a group yet —
  there was never a real reason for it to be there.
- **Conditional Visibility moves — and splits in two on the way.** The existing single
  `conditional-visibility.mdx` was quietly doing two jobs: hiding a *section* (a genuine
  show/hide decision) and choosing a *URL* via an ordered `destination_config.rules` list (a
  routing decision). Both use CEL, which is exactly why they'd been merged in the first place,
  but they're mechanically different questions. The page's own "Advanced: Device-Specific
  Destinations" example turned out to be the second mechanism dressed up as the first. This
  wasn't just a relocation — the split fixes a real conflation, producing
  `destinations/conditional-visibility` and `destinations/conditional-destinations` as two
  pages.

Net effect: of the six pages that used to live under "Pages," only two survive there
(`pages-overview`, and the page-building content, now split across the Pages group's 13
pages). Everything else that's about *routing* a scan rather than *laying out* a Page moved
to Destinations, which ends up with 12 pages of its own.

### The Industries tab — naming and reframing

**Decision: rename the tab from "Industries" to "Use Cases," nest the current five pages
under a new "By Industry" group, and add one orientation page (`use-cases/overview`) that
states the split explicitly.**

The actual tension, not just the surface complaint: BRAND.md states plainly that "QRtub is
industry-agnostic," but the tab groups content by five named verticals no matter what it's
called. Two other names were seriously considered and rejected:

- **"Applications"** — rejected. It collides in meaning with the adjacent Integrations tab,
  which is literally about connecting to other applications (Mitti, CMMS). A reader scanning
  the top nav could reasonably expect "Applications" to mean "connect an app," not "which
  industry am I in."
- **"Industry Guides"** — rejected as the safe-but-empty option. It's the lowest-effort
  choice and breaks nothing, but it doesn't touch the actual tension at all — it just makes
  the pages read a bit less like a sales pitch once you're inside one, while still grouping
  by named vertical under a name that says "industry."

**"Use Cases" won** because it reuses vocabulary the brand docs already treat as canonical
(BRAND.md/CLAUDE.md's own UC-001…UC-008 library) rather than inventing a new term, and it
matches the content's actual shape — every one of the five pages already organizes itself as
a "Two Use Cases, Same Solution" narrative wearing an industry-vertical label. This reframes
the tab to match the content, instead of forcing the content to match the tab.

The one real risk — "Use Cases" colliding with the horizontal, cross-industry UC-numbered
library if that library ever gets its own docs pages — is handled by nesting today's five
pages under a "By Industry" group now, explicitly leaving room for a future "By Workflow"
group, and stating that split on day one in the new `use-cases/overview` page so a second
group later doesn't read as a retrofit.

**Ship this together with the fabrication/tone fixes already found on 4 of 5 pages** — the
fabricated auto-updating register, the phantom analytics/alerts upsell, and the unhedged
"auto-route" claim are the same sentences carrying the worst of the "weaponize" /
"guerrilla marketing" tone. Fixing the facts and fixing the tone is the same edit on the same
lines, so there's no reason to sequence them as two separate passes.

---

## 3. How Links got broken down

You asked for real granularity here specifically, so here is every atomic page that resulted,
plus what each one actually owns:

| Page | Slug | Owns |
|---|---|---|
| What a Link Is | `links/what-a-link-is` | The slug→destination model; why it behaves like an ordinary URL shortener underneath everything else QRtub adds. |
| What a Slug Is | `links/what-a-slug-is` | The identifier segment itself, and the one cross-type rule: random links are case-sensitive, numbered/custom are not. |
| Choosing a Link Type | `links/choosing-a-link-type` | The head-to-head comparison of the three types below — added specifically because no such page existed (see Part 4, finding #6). |
| Random Links | `links/random-links` | 5-char base62, auto-minted, never user-chosen — the default. |
| Numbered Links | `links/numbered-links` | Claiming a `<prefix><N digits><suffix>` range as a team; auto-increment / specific / bulk-range minting; conflict checking. |
| Custom Links | `links/custom-links` | User-chosen slugs; charset rules; the reserved-word and reserved-pattern blacklist. |
| Unallocated Links | `links/unallocated-links` | A real, printable Link with nothing attached — the deliberate print-before-link and spares use case. |
| Claim-on-Scan | `links/claim-on-scan` | The general mechanism: an unrecognized third-party code mints a Link bound to a hash of its decoded text, idempotently. |
| Deleting, Unassigning, and Releasing Links | `links/deleting-and-releasing-links` | The full single-Link picture: hard delete, manual unassign, and both automatic release triggers (Item deletion, Collection deletion). |
| Bulk Link Import via CSV | `bulk-links/csv-import` | Column schema, per-type create rules, dry-run preview, at scale. |
| Bulk Assigning, Unassigning, and Deleting Links | `bulk-links/assign-unassign-delete` | Bulk-operation mechanics only (explicit ID list vs. filtered scope) — defers to the page above for what unassign *means*. |
| Downloading QR Codes | `bulk-links/downloading-qr-codes` | Single PNG vs. zipped bundle; states plainly there's no SVG export and no configurable error-correction level today. |

That's 9 pages in the Links group plus 3 in Bulk Link Operations — 12 atomic pages total for
what used to be one or two undifferentiated pages, plus a companion page in
`print-first/claim-on-scan` (scanner-specific UI action only, deferring to
`links/claim-on-scan` for the underlying mechanism).

**On "sURL":** you mentioned this term, so it was specifically checked against the codebase
rather than assumed. **It isn't a real term in QRtub's code or docs — there is no "sURL"
anywhere in the source.** The inventory (`inventory/links.md`, top of file) grepped the
codebase for an internal short-URL term and found none. The actual internal (non-user-facing)
name is the `access_urls` database table and the `AccessUrl` type — and even that is
explicitly retired as user-facing language: `GLOSSARY.md` states "Access Link / Access URL"
should never appear in the docs, in favor of the plain word **"Link."** The identifier
portion of a Link's URL is consistently called a **"slug"** throughout the actual code
(`normalizeCustomSlug`, `is_valid_custom_slug`, `CustomSlugMetadata.slug`). So there's no
hidden canonical term to adopt instead of what the tree already uses — "Link" and "slug" are
already the correct, code-verified words, and the tree above uses them consistently.

---

## 4. The test

Before any content existed, 45 realistic questions (a small-business, non-developer user
typing into search or asking support — see `planning/atomic-ia/test/questions.md`) were
graded against **titles and one-line descriptions only.** The question: if you read nothing
but the sidebar and the description under each link, could you tell which single page
answers this, with no tie?

**v1 result:** of 45 questions, **10 came back ambiguous or a gap** — meaning either two or
more sibling pages' descriptions were an equally plausible match (ambiguous), or no page's
description covered the question at all (gap). The other 35 resolved cleanly to one page.

**The 10 findings, and what each one exposed:**

1. Collection deletion vs. Link survival — `collections/deleting-a-collection` and `links/deleting-and-releasing-links` both half-claimed the "printed codes keep working" fact.
2. Delete vs. unassign a Link — `links/deleting-and-releasing-links` never stated the manual single-Link unassign action existed at all; `bulk-links/assign-unassign-delete` competed on the same word.
3. Field rename safety — `fields/renaming-a-field` and `destinations/field-renames-and-destinations` were fully duplicate content from two angles.
4. Reference field vs. Allowed Values — three pages (`fields/reference-fields`, `fields/allowed-values`, `fields/field-types`) each answered part of "which type do I need" with no page naming the actual axis.
5. Page Template Version vs. Starter Template — three pages, no page stating the timeline difference (seeded once vs. runs forever after).
6. Random vs. Numbered vs. Custom Link — a genuine **gap**: no comparison page existed at all, despite three atomic type pages.
7. App Links vs. Device Detection — both plausibly "decide where a scan goes based on the phone," with neither description naming the real distinguishing axis (app-open detection vs. device-class routing).
8. `{{time.today}}` — three pages (`time-fields`, `conditional-visibility`, `field-bindings`) each partially touched the "no dates" limitation, none owned it.
9. Batch status vs. per-code deployment status — genuine vocabulary overlap (both use "Printed"/"Deployed") at two different scopes (one value vs. N values).
10. VDP vs. Gang Sheets — the comparison page already existed (`print-shop/choosing-a-method`) but its description didn't name either term, so it lost to its own siblings on a lexical match.

**Fixes applied (v2):** 7 were pure description rewrites — the underlying page split was
already correct, the descriptions just didn't name the axis that actually separated the
siblings. 3 needed structural changes:

- **Merge** — `destinations/field-renames-and-destinations` folded into `fields/renaming-a-field`
  as a named subsection (it was a duplicate, not a genuine split).
- **Rescope and rename** — `links/deleting-and-releasing-links` retitled "Deleting,
  Unassigning, and Releasing Links" and widened to be the single canonical home for every
  trigger of Link release, manual included.
- **New page** — `links/choosing-a-link-type` added, closing the comparison-page gap, on the
  same pattern `print-shop/choosing-a-method` already uses for VDP vs. Gang Sheets.

**Retest result: all 10 findings resolved. Nothing came back ambiguous or a gap a second
time.** The full one-to-one mapping from each of the 10 original findings to its specific fix
is in draft-tree-v2.md's "What changed and why" section, so any fix can be traced back to the
exact question that forced it.

**Residual problems: none.** This is worth stating plainly rather than hedging: the retest
came back clean at 45/45. That doesn't mean the tree is perfect — it means these particular
45 questions, which were deliberately weighted toward the narrowest and most confusable
splits in the tree, no longer trip on titles and descriptions alone. Part 5 below is how you
keep it that way as new pages get added.

---

## 5. A reusable testing method

Run this every time someone proposes a new page — a new feature is documented, an existing
page is split, or a page is renamed. It takes a few minutes and it is the entire reason the
v1 → v2 fix above worked.

**The recipe:**

1. **Write 2–3 questions the new (or changed) page should uniquely answer.** Phrase them the
   way a real user would type them — not the page's own title reworded. If you're adding
   "Choosing a Link Type," a real question is "what's the actual difference between a Random
   Link, a Numbered Link, and a Custom Link" — not "what are the three link types."

2. **For each question, read only the title + one-line description of every sibling page in
   that group (and any page one group over that touches the same territory).** Don't open the
   pages. Don't use what you know about the feature. Pretend you're a user scanning a sidebar.

3. **Ask: does exactly one page's description answer this, with no plausible second
   candidate?** If yes, move on. If two or more descriptions are each a reasonable match — or
   if none of them clearly is — that's a finding.

4. **Treat every tie as a sign the *IA* needs work, not the questions.** This is the part
   that's easy to get backwards. The fix is never "ask a more specific question so it stops
   tying" — a real user doesn't know to phrase around your page structure. The fix is one of:
   - **Rewrite the competing descriptions** to each name the actual axis that separates them
     (this fixed 7 of the last 10 findings — usually the split was already right, the
     description just didn't say what made it different from its neighbor).
   - **Merge** if two pages turn out to say the same one fact from two angles — that's a
     duplicate, not a split.
   - **Add a comparison page** if several atomic siblings exist but nothing puts them side by
     side — the "which one do I want" question needs its own answer, and Linear's own docs
     never leave that gap (see `print-shop/choosing-a-method` for the existing pattern to
     copy).
   - **Rescope one page** if the honest answer to the question is currently split across two
     pages that each half-state it.

5. **When you fix something, add the reciprocal pointer.** Every fix in v2 that survived as
   two separate pages (rather than a merge) got a "see the other page for X" line in *both*
   directions, not just one. A one-directional pointer means half the readers still land on
   the wrong page first.

6. **Re-run the exact question after the fix**, cold, without remembering your own edit. If
   it still ties, you haven't fixed it — you've usually just moved the ambiguity to a
   different pair of sentences.

This costs nothing but attention and catches the exact failure mode that made v1 wrong:
structurally correct pages that nonetheless send a real reader to the wrong door, because the
description competed with a sibling's instead of naming what made it different.

---

## 6. What is still open

These are genuine judgment calls the inventory or drafting phases flagged — not gaps in the
process, decisions that need a person, not another pass of this exercise.

1. **Is the numbered-pattern plan-tier limit (Starter: 0, Professional: 1, Scale: up to 5)
   actually enforced, or just advertised?** The numbers exist only in `stripe-plans.ts`'s
   marketing feature-list copy. No server-side quota check was found anywhere in
   `claimPattern`, the `numbered-patterns` POST route, or elsewhere. `links/numbered-links`
   should state the number as a real plan fact, but whoever drafts it needs to confirm with
   engineering whether it's gated in code before writing "you'll be blocked at..." — the code
   may have changed since this check.

2. **Same question, same shape, for the general plan limits (active-link caps, editor-seat
   caps).** Direct code inspection found **no server-side enforcement** anywhere in `src/lib`
   or `src/app/api` for either cap — they exist only inside `STRIPE_PLANS[].features`.
   Recommend `billing/plan-limits` phrase these as "what's included in your plan," not "the
   ceiling the system enforces," until someone re-verifies enforcement at draft time.

3. **"Editor" the team role vs. "editor" the per-plan seat count use the same bare word for
   two related-but-distinct things.** Nobody has confirmed whether every active team member
   counts toward the seat limit regardless of their role label — there's no seat-count
   enforcement in the code at all to check against. Word `team/roles` and `billing/plan-limits`
   so they don't read as contradicting each other, once someone answers this.

4. **Corrected after this plan was drafted, not still open:** the workflow's original finding
   here mis-stated this one. Verified directly against `src/app/app/team/page.tsx:255-263` and
   a repo-wide grep for `transferOwnership` call sites: there is exactly one, inside
   `handleLeaveTeam`. There is no standalone transfer-ownership button or page anywhere in the
   app — transfer only happens as a side effect of an owner leaving. The dialog's own text
   also already discloses the auto-pick in the same message the workflow quoted only half of
   ("Select a new owner: [list]... **The first active member will be selected as the new
   owner.** Continue?") — not a hidden bug, just a two-part sentence. Net: "Transferring
   Ownership" is merged into "Leaving a Team" in Part 1 above, not a sibling page — one real
   feature, one page.

5. **Does a Collection-level Page privacy toggle exist, or not?** `TubLandingPageDefaults.privacy` is
   set once at creation (always `'public'`) and read back, but there's no UI control for it in
   Collection Settings — only per-Item privacy exists today. Deliberately left undocumented as a
   Collection-level control rather than describing a setting that isn't there. If a Collection-level toggle
   ships later, it's a small addition to `collections/scan-behavior-default`'s territory.

6. **The manual single-Link "unassign" action's exact UI surface needs verification before
   drafting `links/deleting-and-releasing-links`.** v2 widened this page to assert a manual
   unassign action exists outside the bulk-operations UI — confirm that's actually true in
   `../qrtub/src/` before the page states it as a feature, not just infer it from the bulk
   version existing.

7. **Jotform, Google Maps, Google Sheets integrations — no feature exists yet to write about,
   and Google Sheets has an unresolved *direction* question:** does a scan deep-link into a
   Sheet/row (matching the other three integrations), or does Item data export/sync into a
   Sheet (a completely different data-flow feature that would sit under Import & Export, not
   Integrations, and change which tab it belongs to)? This is a product decision, not an IA
   one — the tree can't be finalized for this integration until someone answers it.

8. **CMMS Systems currently gets 2 pages while Mitti gets 4 — is that asymmetry the right
   final shape, or a placeholder?** CMMS is deliberately generic (any maintenance platform,
   no single vendor's entity catalog to enumerate), which is why it doesn't get its own
   "entities" or "prefilling" page the way Mitti does. Worth a product sign-off that this is
   intentional rather than incomplete before treating it as final.

9. **BRAND.md's own industry list doesn't match the docs' industry list.** BRAND.md Part 6
   ("Early Industries") names Marine, Construction, Equipment hire, Lifesaving; the Use Cases
   tab covers Civil Construction, Contract Cleaning, Arboriculture, Electrical Test & Tag,
   Local Councils. Only "Construction" overlaps. This plan doesn't resolve that — it's a
   content-strategy question (which verticals QRtub is actually positioning toward) that sits
   above the IA and should go back to whoever owns BRAND.md.

10. **Word-count checks on two "confirmed merge" candidates were made from the *inventory's*
    estimate, not a drafted page.** `collections/item-id-mask`'s "Mask conflicts" subsection and
    `collections/creating-a-collection`'s "Where the page template comes from" subsection were folded in on
    the strength of a word-count prediction. If either turns out to have more real content
    than expected once actually written, it's fine to split it back out — the merge call was a
    prediction, not a permanent constraint.
