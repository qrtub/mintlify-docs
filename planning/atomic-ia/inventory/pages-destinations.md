# Concept Inventory — Pages & Destinations

Domain: the **Page** entity (Page Editor, section types, theming, overrides, templates) and
**Destinations** as its own split-out topical category (routing, bindings, conditions, app
links, device routing, generic-platform guidance).

Grounded in `../qrtub/src/lib/page/`, `../qrtub/src/lib/types/destination-config.ts`,
`../qrtub/src/lib/types/landing-page-config.ts`, `../qrtub/src/lib/utils/device-detection.ts`,
`../qrtub/src/lib/utils/app-link-open.ts`, `../qrtub/src/lib/utils/url-helpers.ts`,
`../qrtub/src/lib/utils/destination-pattern-fields.ts`, `../qrtub/src/lib/database/field-defaults.ts`,
`../qrtub/src/lib/database/server-page-templates.ts`, `../qrtub/src/lib/templates/`,
`../qrtub/src/components/page/*`, `../qrtub/src/components/blocks/Destination*`,
`../qrtub/src/components/blocks/AppLinkOpener/`, `../qrtub/src/components/blocks/LinkNotReadyView/`,
`../qrtub/GLOSSARY.md`, `../qrtub/BRAND.md`, and existing pages under `/workspace/mintlify-docs/`
(`pages-overview.mdx`, `building-a-page.mdx`, `using-fields.mdx`, `conditional-visibility.mdx`,
`device-detection.mdx`, `app-links.mdx`, `integrations/cmms-systems.mdx`,
`integrations/safetyculture.mdx`).

Legend: **Cat.** = proposed top-level nav group. **S/M** = standalone page / merge into a named sibling.

---

## A. Pages group

| # | Concept | Definition | Cat. | Page title | Slug | S/M |
|---|---|---|---|---|---|---|
| A1 | Page (entity orientation) | What a Page is: the multi-Destination experience a Link can open, built once per Collection in the Page Editor. | Pages | Pages Overview | `pages-overview` | **Standalone** — short orientation page, Linear-"Concepts"-style. Keep tight now that mode detail and creation-steps split out (A2, A3). |
| A2 | Direct Mode vs Page Mode | The per-Collection (default) / per-Item (override) choice between an immediate single-Destination redirect and a multi-Destination Page; switchable anytime without reprinting. | Pages | Direct Mode vs Page Mode | `direct-mode-vs-page-mode` | **Standalone** — real distinct mechanics (tub default via `landingPageDefaults.destinationType`, item override via `ItemLandingPageConfig`, `getEffectiveDestinationType` inheritance rule, "Landing Page" vs "Destination Link" radio in the UI). Currently folded into `pages-overview.mdx`; extracting is itself the atomicity fix. |
| A3 | Turning Pages on for a Collection / creating one | The 4-step flow: enable pages on the Collection, create an Item, assign a Link, add Destinations — and the trap that assigning a Link alone does not produce a page. | Pages | *(fold into A1)* | — | **Merge** into Pages Overview — it's the "how to get started" tail of the orientation page, not a mechanism of its own. |
| A4 | Page Privacy (public vs private) | Collection-level `is_public` and Item-level `privacy` setting gate whether a Page requires the viewer to be a signed-in team member; also drives `robots: noindex` and minimal metadata. | Pages | Page Privacy: Public vs Private | `page-privacy` | **Standalone** — real, non-trivial (`checkPagePrivacy`, two independent gates, distinct from Destination-level access). Note: the UI checkbox literally reads "Private Landing page" (glossary mismatch — flag for drafting). |
| A5 | The Page Editor layout | The three-tab left panel (Components / Data / Structure), the Properties panel on the right, undo/redo, and the "switching Item clears undo history" gotcha. | Pages | The Page Editor Layout | `page-editor-layout` | **Standalone** — extracted from `building-a-page.mdx`. |
| A6 | Section types (catalog) | The 17 available sections, grouped into 5 categories (Data, Content, Layout, Interactive, Media) in the component palette. | Pages | Section Types | `section-types` | **Standalone reference/catalog page.** Confirmed exactly 17 manifests in `lib/registry/index.ts` (ActionLink, ItemHeader, Banner, Spacer, Button, SpecGrid, Card, Tags, AdminToolbar, KeyValue, Container, Link, ContactInfo, Text, TubInfo, Hero, ImageSection). `Initials` and `ExpandableText` are internal sub-components, not section types — do not list them as an 18th/19th. This one page should NOT try to explain every field of every section (that's what makes Linear's model work); it's a map, with 3 sections pulled out below because they have real distinct mechanics. |
| A7 | ActionLink | The section that IS a Destination button: auto-hides itself when its `href` binding can't resolve, and visually groups with adjacent ActionLinks into one button block. | Pages | ActionLink: The Destination Button | `action-link` | **Standalone** — genuinely distinct behavior (`shouldRenderActionLink`, `addActionLinkGroupAttributes`), not just a schema like most sections. |
| A8 | AdminToolbar | A configurable bar of custom buttons/icons (curated Lucide set) pinned to the page, defaulting to signed-in-only visibility so owners can add internal shortcuts to a page the public also sees. | Pages | AdminToolbar | `admin-toolbar` | **Standalone** — only section with this "dual-audience" behavior. |
| A9 | Banner | Full-width status/message strip. | Pages | — | — | **Merge** into A6 (Section Types). |
| A10 | Button | Simple button (non-card); docs should note "use ActionLink for card-based buttons." | Pages | — | — | **Merge** into A6. |
| A11 | Card | Layout container with linktree-style card styling; can hold other sections. | Pages | — | — | **Merge** into A6. |
| A12 | Container | Layout wrapper with max-width/padding controls; can hold other sections. | Pages | — | — | **Merge** into A6. |
| A13 | ContactInfo | Card for contact details, bindable fields. | Pages | — | — | **Merge** into A6. |
| A14 | Hero | Compact hero section with equipment details/description. | Pages | — | — | **Merge** into A6. |
| A15 | ItemHeader | Card-style header with image, title/type/subtype/status, chips, note. | Pages | — | — | **Merge** into A6 (its image controls are pulled into A16 instead). |
| A16 | Image display controls (Frame / Fit / Align) | Shared image-shape system — Frame Shape (square/portrait/landscape/natural), Image Fit (contain/cover), Image Align — reused identically by ImageSection and ItemHeader. | Pages | Image Display Controls | `image-display-controls` | **Standalone** — cross-cutting mechanic (`lib/page/image-fit.ts`), not owned by one section, so it doesn't fit cleanly inside either section's catalog entry. |
| A17 | ImageSection | Standalone image card. | Pages | — | — | **Merge** into A6 (control details live in A16). |
| A18 | KeyValue | Key-value pairs in a card. | Pages | — | — | **Merge** into A6. |
| A19 | Link | Simple text link (non-card); "use ActionLink for card-based links." | Pages | — | — | **Merge** into A6. |
| A20 | Spacer | Adds vertical spacing between sections. | Pages | — | — | **Merge** into A6. |
| A21 | SpecGrid | Grid of specifications in a card. | Pages | — | — | **Merge** into A6. |
| A22 | Tags | Inline tag pills. | Pages | — | — | **Merge** into A6. |
| A23 | Text | Text content block. | Pages | — | — | **Merge** into A6. |
| A24 | TubInfo | Card showing Collection name/description. | Pages | — | — | **Merge** into A6. |
| A25 | Putting Item data into a section (bindings in the editor) | Drag-a-field-from-the-Data-tab UX; `{{item.field}}` syntax; unresolved binding renders as empty, and most sections hide themselves when empty. | Pages | *(fold)* | — | **Merge** into A5 (Page Editor Layout) for the editor-UX half, and cross-link to the Destinations-group "Field Bindings & URL Templates" page for the syntax itself — don't duplicate the mechanic in both places. |
| A26 | Theming a page | Theme Presets (7 named presets), Accent Color, Border Radius, Shadows, Spacing, Typography Scale, Appearance (light/dark), Max Width, Padding; picking a preset **replaces** the whole theme rather than merging. | Pages | Theming a Page | `page-theming` | **Standalone.** Also the right home for the thin `theme.accent` / `theme.radius` bindings (A27 merges here). |
| A27 | Theme fields as bindings | `theme.accent`, `theme.radius` are readable as `{{theme.*}}` bindings inside a page. | Pages | — | — | **Merge** into A26 (Theming a Page) — two fields, not enough for its own page. |
| A28 | Previewing a page against real Item data | The item selector (Base Template vs a real Item, searchable), 5 responsive widths, and the Preview toggle that hides editing chrome. | Pages | Previewing a Page | `previewing-a-page` | **Standalone.** |
| A29 | Per-item page overrides (save semantics) | The Override toggle: ON saves to that Item only (stored as a sparse diff — `$remove`/`$order`/`$upsert` — via `item_overrides`), OFF updates the base template for every Item in the Collection; selecting a real Item turns Override on automatically; revert one section or clear all overrides. | Pages | Per-Item Page Overrides | `page-overrides` | **Standalone** — this is exactly the kind of save-semantics page that needs to exist on its own; the current wording in `building-a-page.mdx` is accurate and should carry over almost verbatim. |
| A30 | Page Template versioning | Every save creates a new version row (`page_templates`, versioned, one active at a time per Collection); version history, restoring an old version, cloning a template to another Collection. | Pages | Page Template Versions | `page-template-versions` | **Standalone — NEW, not covered anywhere today.** `ServerPageTemplatesService` confirms upsert-bumps-version, `restoreVersion`, `clone`. Boundary question: is this "Pages" domain or "Items & Data / Collection settings" domain? See open questions. |
| A31 | Starter page templates (gallery) | Pre-built starting layouts offered when setting up a Collection: Inventory, IT Assets, Medical Equipment, Equipment Inspections, Audience Routing, Branded Handoff, Clone a QR Code, plus a SafetyCulture-asset template and a bare Default. | Pages | Starter Page Templates | `starter-templates` | **Standalone, but boundary case** — arguably belongs to a Collection-setup/Getting-Started domain rather than this one. Flagged in open questions. |
| A32 | Page metadata & social previews | Auto-generated title/description/OG+Twitter image from the Item's name/description/image; pages are `noindex` but `follow` (thin per-item pages shouldn't dilute search, but link equity should still flow); private pages get minimal "Private Item / Sign in to view" metadata instead. | Pages | Page Metadata & Social Previews | `page-metadata` | **Standalone, but thin** — flagged as a merge-into-Pages-Overview candidate if it can't clear ~150 words on its own once drafted. |

## B. Destinations group (new — split out of Pages)

| # | Concept | Definition | Cat. | Page title | Slug | S/M |
|---|---|---|---|---|---|---|
| B1 | Destination (entity orientation) | Where a scan is routed — an external URL, an app deep link, a form, a payment flow (future) — set at Item, Link, or Collection level; also the UI element on a Page. | Destinations | What Is a Destination? | `what-is-a-destination` | **Standalone** — the anchor page for the whole group. Should state the real resolution order: an Item's `destination_config` (conditional rules) is checked before its plain `destination_url`; if neither resolves, nothing is synthesized at scan time (no collection-level fallback happens live — the tub default is only ever stamped in at Item-create time, see B9). |
| B2 | Destination resolution safety (dangerous-scheme filtering) | Any Destination URL is checked against a blocklist of dangerous schemes (`javascript:`, `data:`, `vbscript:` etc.) before being rendered as a link or used as a redirect target; an unsafe URL renders as `#` (page context) or triggers a 404 (public redirect context) instead of executing. | Destinations | — | — | **Merge** into B1 (What Is a Destination) as a short safety callout — real (`isSafeHref`/`sanitizeHref`/`safeFallbackUrl`), but not enough on its own for a page. |
| B3 | "This link isn't ready yet" state | The public page shown when a Direct-Mode Item has no resolvable Destination — shows the link's own slug/URL (never internal IDs) and an "Open in QRtub" link that routes owners to the authenticated fix-it view. | Destinations | — | — | **Merge** into B1 — it's the empty-state companion to the resolution-order explanation, not a separate mechanism. |
| B4 | Field bindings / URL Templates (the `{{ }}` mechanic) | Double-curly-brace syntax for inserting field values into a Destination URL; values are inserted exactly as stored (no auto-encoding); a missing/empty field inserts an empty string; single braces do nothing; field names are case-sensitive. | Destinations | Field Bindings & URL Templates | `field-bindings` | **Standalone** — the mechanic itself, separate from any one field catalog. Should also introduce the *set* of available namespaces (`item`, `tub`, `device`, `time`, `request`, `session`, `theme`) with one line each and links out to B5–B8/A26/device-detection, rather than re-listing every field inline as `using-fields.mdx` currently does. |
| B5 | Item Fields reference | The catalog of `item.*` bindings: standard fields (id, name, description, item_number, type, status, tags, serial_number, etc.) plus any custom fields a Collection defines. | Destinations | Item Fields Reference | `item-fields` | **Standalone** reference page. |
| B6 | Collection Fields reference | The catalog of `tub.*` bindings: id, name, description, items_name, created_at, `metadata.page.is_public`, `metadata.organizationName`, etc. | Destinations | Collection Fields Reference | `tub-fields` | **Standalone** reference page. |
| B7 | Time Fields reference | `time.hour`, `time.dayOfWeek`, `time.dayOfMonth`, `time.month`, `time.year`, `time.isWeekend` — all UTC, computed fresh per scan; explicitly **no absolute date and no date arithmetic**, so "is this overdue" can't be expressed with `time` alone. | Destinations | Time Fields Reference | `time-fields` | **Standalone — NEW, currently entirely absent from the docs** despite being real and already referenced by `mintlify-docs/CLAUDE.md`'s own technical notes. |
| B8 | Request Fields reference | `request.timestamp`, `.path`, `.referrer`, `.language`, `.country`, `.city`, `.ip` — sourced from the request/CDN headers (Vercel/Cloudflare geo headers), null where unavailable. | Destinations | Request Fields Reference | `request-fields` | **Standalone — NEW, currently entirely absent from the docs.** Should note the privacy angle (IP/geo become available to whatever CEL condition or URL template you write) since that's a real, non-obvious consequence of using this data. |
| B9 | Session fields | `session.user` / `.id` / `.email` / `.name` — only populated for a signed-in team member viewing their own page (e.g. gates AdminToolbar's default visibility); null for anonymous scans. | Destinations | — | — | **Merge** into B4 (Field Bindings & URL Templates) as one short namespace entry — too thin (2-3 sentences) to justify its own page. |
| B10 | Field renames keep destination bindings alive | Renaming a custom field rewrites any `destination_url` / `destination_config` (URL, fallback URL, CEL condition) that referenced its old name, in place, at save time — because destinations are stored with human-readable keys while Page-template bindings are stored keyed by a stable nanoid ID (and so never break on rename in the first place). | Destinations | Field Renames & Destination Stability | `field-renames-and-destinations` | **Standalone** — narrow, real, and exactly the kind of "gotcha behavior" page Linear itself would give its own slot (`buildFieldRenameMap`, `renameDestinationBindings`, `addFieldIdAliases`). |
| B11 | Conditional Visibility (show/hide a section) | Any section (not only Destination buttons) carries an optional CEL `condition`; false or unresolved ⇒ the section doesn't render. An undefined identifier (e.g. a typo, or an invented field like `today`) makes the whole condition evaluate to `false` **silently** — no error, just a rule that never fires. | Destinations | Conditional Visibility | `conditional-visibility` | **Standalone — scope tightened** from the current doc, which conflates this with B12 below. This page is about hide/show; it is not about choosing between URLs. |
| B12 | Conditional Destinations & rule priority | The `destination_config.rules` list on a single Destination: an ordered array, first-matching-condition wins, each rule can carry its own label + fallback; a `defaultLink` catches anything unmatched; the editor shows live Match / No Match / Error badges and lets you drag/reorder rules. | Destinations | Conditional Destinations & Rule Priority | `conditional-destinations` | **Standalone — NEW split**, currently entangled with B11 in `conditional-visibility.mdx`. This is a *routing* mechanism (which URL fires), mechanically distinct from B11 (whether a section shows at all), even though both use CEL and the same evaluator. |
| B13 | CEL expression limits | Hard validation limits on any CEL expression: 500-character max, max nesting depth 10, max 20 operators — enforced before evaluation. | Destinations | — | — | **Merge** — split the detail across B11 and B12 (each gets a one-line "limits" callout) rather than a standalone page; too thin alone. |
| B14 | Device Detection & Routing | Server/client User-Agent parsing into `device.type` / `.os` / `.browser` and boolean convenience flags; routing patterns (app vs web, app-store-by-platform, tablet-specific, browser-specific); explicitly not for security. | Destinations | Device Detection & Routing | `device-detection` | **Standalone — keep as-is.** Confirmed accurate against `lib/utils/device-detection.ts`. Already correctly scoped as its own page — no split needed. |
| B15 | App Links & Fallback URLs | Deep-link URLs (any non-`http(s)` scheme, including `tel:`); on click, QRtub tries to open the app and falls back to a Fallback URL or Fallback Message if it doesn't; three levels of fallback (Destination > Link > Item) with the most specific winning. | Destinations | App Links & Fallback URLs | `app-links` | **Standalone — keep, but factually update.** The real mechanism is **not** a naive blind timer: `openAppLink()` also listens for `visibilitychange`/`pagehide` and cancels the fallback the instant the tab is hidden (the app took over), and treats a timer that fires *very* late as evidence the OS suspended it while the app was foregrounded — this is explicitly the "flaky scan" fix and the current doc's "a 2.5-second timer starts... if the page is still visible" wording undersells/misdescribes it. Also worth stating plainly that `tel:` and `mailto:` count as app links under the same mechanism, not just custom `myapp://` schemes. |
| B16 | Default Destination Pattern (Direct-Mode tubs) | A Collection-level URL template (`fieldConfig.fields.destination_url.defaultValue`, plus a paired default fallback URL/message) that gets **stamped onto every new Item at create time** — frozen from then on, so editing the tub default later never changes existing Items; the settings UI warns how many existing Items are missing a value for each field the pattern references. | Destinations | Default Destination Pattern | `default-destination-pattern` | **Standalone — NEW, not documented anywhere today.** Only shown/relevant when a Collection is in pass-through (Direct Mode) mode — cross-link heavily to A2. |
| B17 | Connecting a generic system | Fallback methodology page for any platform without its own integration guide: check for a normal web URL you can template with `{{item.field}}` first, only reach for a deep link if the platform has no web equivalent, and test on one real Item before deploying in bulk. | Destinations | Connecting a Generic System | `connecting-a-generic-system` | **Standalone — NEW, explicitly requested.** Should generalize the pattern already used ad hoc in `integrations/cmms-systems.mdx`'s "Integration Approaches" section rather than duplicate it per-platform. |
| B18 | How to reverse-engineer a platform's deep links | General method for finding an undocumented app's deep-link scheme: check the vendor's own developer docs first, inspect a link the app itself generates (share sheet, "copy link"), watch for Android/iOS scheme differences, and always pair whatever you find with a Fallback URL since deep-link schemes are undocumented-and-unstable by nature. | Destinations | How to Reverse-Engineer a Platform's Deep Links | `reverse-engineering-deep-links` | **Standalone — NEW, explicitly requested.** Distinct from B17: B17 is "there's no guide, here's how to connect anyway using what's documented"; B18 is "there's no documentation at all, here's how to go find the scheme yourself." |

---

## What should move from "Pages" into "Destinations"

The task flagged App Links as the obvious mover and asked me to check Conditional Visibility
and Device Detection too. Verdict, checked against the current `docs.json` groups:

- **App Links & Fallback URLs (B15)** — moves. It is entirely about Destination routing
  behavior; it has no dependency on the Page Editor at all (a Direct-Mode Link with no Page
  uses the identical mechanism via `AppLinkOpener`).
- **Device Detection & Routing (B14)** — moves. Same reasoning: it's a `device.*` binding
  reference plus Destination-routing patterns. It currently sits in the "Pages" group only
  because Destinations didn't have a group of their own yet.
- **Conditional Visibility (B11) and its split-off sibling Conditional Destinations (B12)** —
  both move. Both are CEL-based Destination/section behavior, not Page-layout behavior — the
  Page Editor is just where the `condition` field happens to live in the UI.
- **Using Fields (the current single mega-page)** — retired as one page; it splits into B4
  (mechanic) + B5/B6/B7/B8 (per-namespace references) + B9 (merged) + a cross-link into
  device-detection.mdx for B14's own field table (do not duplicate the device field table in
  two places).

Net effect: **every one of the six current "Pages" group pages that isn't about the Page
Editor itself (`using-fields`, `conditional-visibility`, `device-detection`, `app-links`) moves
to Destinations.** Only `pages-overview` and `building-a-page` (now split across A1–A32) stay
in Pages.

---

## Notes for the drafting phase

- **17 is confirmed, not assumed.** Counted directly from `src/lib/registry/index.ts`'s
  `REGISTRY` map: 17 entries, matching `building-a-page.mdx`'s existing claim. `Initials.tsx`
  and `ExpandableText.tsx` in `src/components/page/` are internal helpers (avatar-fallback and
  truncate-with-more/less), not separately registered section types — don't inflate the count.
- **Two different "priority" concepts exist in this domain and must not be conflated:**
  fallback-source priority (Destination > Link > Item, inside App Links, B15) and rule-order
  priority (first-matching CEL rule wins, inside Conditional Destinations, B12). They use the
  word "priority" for genuinely different things.
- **Two different CEL-based mechanisms currently share one doc page** (`conditional-visibility.mdx`)
  and should not going forward: hiding a *section* (B11) vs choosing a *URL* via ordered rules
  (B12). The existing page's "Advanced: Device-Specific Destinations" section is really about
  B12 dressed up as B11 — the iOS-Safari-workaround example there picks between two whole
  Destinations by condition, which is rule routing, not show/hide of one thing.
- **Glossary/UI wording tension to flag, not silently fix:** the app's actual checkbox reads
  "Private Landing page" (component: `DestinationConfiguration.tsx`), but `GLOSSARY.md` wants
  the noun "Page" and disallows "Landing Page" as a capitalized product term. Screenshots/UI
  callouts in the new Page Privacy page (A4) will have to describe the real checkbox label
  while using "Page" in the surrounding prose — same tension the glossary's own "Not Yet
  Aligned" section anticipates for "profile page."
- **`landing_page` / `LandingPageEditor` naming in code is intentionally not what the docs
  should call anything** — glossary confirms this is old internal naming being migrated. Don't
  let variable names leak into page titles or slugs.
- Every "Merge" row above still deserves a short subsection in its target page, not a silent
  drop — several (image controls, CEL limits, session fields) are real behavior a reader could
  otherwise miss entirely.
