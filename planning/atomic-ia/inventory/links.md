# Link domain — atomic concept inventory

Source of truth checked against the real code, not memory or the earlier draft brief:

- `qrtub/src/lib/database/server-access-urls.ts` (the whole lifecycle: generate/random/numbered/custom, bulk ops, delete/release, scan-hash lookup)
- `qrtub/src/lib/database/server-numbered-patterns.ts`, `qrtub/src/lib/utils/numbered-pattern-format.ts`
- `qrtub/src/lib/types/link-generation-config.ts`, `qrtub/src/lib/database/auto-link-for-thing.ts`
- `qrtub/src/lib/services/access-link-csv-import-service.ts`, `qrtub/src/app/api/access-links/import/route.ts`
- `qrtub/src/app/api/access-links/resolve/route.ts` (claim-on-scan)
- `qrtub/src/app/api/numbered-patterns/**` (claim/delete/check-range-conflicts/stats)
- `qrtub/src/lib/utils/qr-code.ts`, usage in `qrtub/src/app/app/access-link/page.tsx` (QR download, single + bulk zip)
- `qrtub/supabase/migrations/20260416000003_reserve_system_slugs.sql`, `20251222000000_allow_underscores_in_slugs.sql`, `20251223000000_allow_underscore_at_start_of_slug.sql` (custom-slug validation + reserved words)
- `qrtub/src/lib/stripe-plans.ts` (plan-tier numbered-pattern limits)
- `qrtub/GLOSSARY.md`, `qrtub/BRAND.md`
- `qrtub/CLAUDE.md` §"Link lifecycle (creation + deletion)"
- Existing docs checked for overlap: `mintlify-docs/creating-your-first-link.mdx`, `mintlify-docs/key-concepts.mdx`, `mintlify-docs/unallocated-links.mdx`

**Terminology check requested by the brief:** grepped the codebase for an internal short-URL
term ("sURL" or similar). None exists. The internal (non-user-facing) name is the
`access_urls` table / `AccessUrl` type — GLOSSARY.md explicitly retires "Access Link / Access
URL" as user-facing language in favor of "Link". So there is no secret canonical term to
adopt; "Link" is correct everywhere in the docs, and "slug" is the right word for the
identifier portion (used consistently in the code: `normalizeCustomSlug`, `is_valid_custom_slug`,
`CustomSlugMetadata.slug`, etc.).

**Correction to the brief's assumption:** bulk QR export in the app (`handleBulkDownloadQRCodes`
in `access-link/page.tsx`, backed by `generateQrZipBytes`/`QR_CODE_OPTIONS` in
`lib/utils/qr-code.ts`) is **PNG-only**, fixed at 1024px/margin 2/black-on-white, zipped for
more than one code. There is no SVG export path anywhere in `src/`, and no user-facing or
code-level error-correction-level setting — the `qrcode` package's own default is used
implicitly and is never exposed as an option. The inventory row below documents what actually
ships and calls out the absence explicitly (per `mintlify-docs/CLAUDE.md`: "state limitations
explicitly — silence reads as capability").

---

## Inventory

| # | Concept | One-line definition | Nav category | Proposed page title | Proposed slug | Atomic call |
|---|---------|---------------------|---------------|----------------------|----------------|-------------|
| 1 | What a Link is | A Link is a slug that resolves to a destination — the QRtub-managed URL a QR code (or NFC tag, or typed address) encodes. | Links | What a Link Is | `what-a-link-is` | **Standalone.** ~350–450 words of real content: the slug→destination model, that it's encodable on any medium, and the "it's fundamentally a URL" framing (see note below) folded in as the page's technical grounding. This is the domain's front-door orientation page, playing the role Linear's "Concepts" page plays for the whole product — short, but load-bearing. |
| 2 | A Link is fundamentally a URL | Resolving a Link is the same mechanism any URL shortener uses (slug → lookup → redirect); nothing QRtub-specific is required for that base case. | Links | *(folded into "What a Link Is")* | — | **Merge, not standalone.** This isn't a separate mechanism from #1 — it's the same fact told from the "why does this feel like bit.ly" angle. Splitting it out would force two pages to both open with "a Link is a slug that resolves to something," which is exactly the kind of duplication Linear avoids. `unallocated-links.mdx` already makes this point once ("A shortener, with nothing else attached, ever") — the new page should make it properly, and `unallocated-links.mdx` can point to it instead of repeating it. Verified against BRAND.md's "vs. Link Shorteners" section (line 297–299): that section is a *positioning* claim (QRtub does more than a shortener), not a technical one — it doesn't contradict the redirect mechanism itself being ordinary. |
| 3 | What a slug is | The identifier segment of a Link's URL — what varies by creation strategy, distinct from the full address. | Links | What a Slug Is | `what-a-slug-is` | **Standalone.** ~250–400 words: slug vs. full URL (`/r/{slug}` for random, `/{slug}` for numbered/custom — confirmed in `src/app/r/[id]/` vs `src/app/[id]/`), and the general case-sensitivity split (random is case-sensitive, numbered/custom are case-insensitive — confirmed in `getByUrl`'s dual-lookup and `_generateRandom`'s comment "Random links are case-sensitive"). Defers each strategy's actual charset rule to that strategy's own page rather than repeating it here — this page explains the *concept* of a slug, not each strategy's rules. |
| 4 | Random links | The default link type: a 5-character base62 string, generated automatically, never user-chosen. | Links | Random Links | `random-links` | **Standalone.** ~300–400 words: base62 alphabet (`A-Za-z0-9`, confirmed `BASE62_CHARS`), fixed 5-char length (`RANDOM_LINK_METADATA`, confirmed comment "card #262: client-supplied length/charset ... deliberately ignored"), uniqueness retry loop (100 attempts against `url_exists` RPC), case-sensitive matching, and that it's the default link-generation mode for new tubs (`NEW_TUB_LINK_GENERATION_DEFAULTS`). Comfortably clears Linear's bar — their own "Priority" page (243 words) is thinner than what this needs to say. |
| 5 | Numbered links / numbered patterns | A reserved `<prefix><N digits><suffix>` range a team claims once, then mints individual numbered links against — sequentially, at a specific number, or as a bulk range. | Links | Numbered Links | `numbered-links` | **Standalone**, and deliberately kept as ONE page rather than split into "pattern" vs "generation modes" — see Open Questions. Real content: prefix/suffix/digits mask and case-insensitive ownership matching (`_findNumberedPatternId`); claiming a pattern (`claimPattern`, first-team-to-claim owns it, `PATTERN_HAS_LINKS` guard on delete); the three generation modes — auto-increment, specific/fill-gap, and range (bulk); range conflict checking (`checkRangeConflicts` / `check_numbered_range_conflicts` RPC) reporting which requested numbers are already used before committing; the per-request cap (1000 numbers per range request — `check-range-conflicts/route.ts`: "Range too large. Maximum 1000 numbers per request", and the general `generate()` count cap of 1000); and the plan-tier limit on how many patterns a team can hold (Starter: none included, Professional: 1, Scale: up to 5 — confirmed in `stripe-plans.ts` feature lists; flagged below as pricing-copy-only, not found enforced in application code). This is comparable in density to Linear's own `sla.md` (994 words for one page), so one richly-detailed page is the right size, not a reason to split. |
| 6 | Custom links | A user-chosen, memorable slug, validated against a fixed charset and a reserved-word blacklist. | Links | Custom Links | `custom-links` | **Standalone.** ~500–650 words: charset rules from `is_valid_custom_slug` (3–50 chars, lowercase letters/digits/hyphen/underscore only, must start with a letter/digit/underscore and end with a letter/digit, no consecutive hyphens); the reserved-word blacklist (`is_reserved_slug` — app routes, auth paths, brand terms, dashboard/admin paths, product-tier words, platform words, confirmed in `20260416000003_reserve_system_slugs.sql`); and the reserved-by-pattern guard (a custom slug can't land inside a numbered pattern's own reserved number-space — confirmed in `_generateCustom`'s `reservedByPattern` check). Reserved words are kept as a section of this page, not their own page — they're a single list explained by one rule ("can't collide with app routes or brand terms"), with no independent mechanism to justify separating it from the charset rule it exists alongside. |
| 7 | Unallocated links | A Link that exists — a real, permanent, printable slug — with nothing attached to it yet: no destination, no Item, no Page. | Links | Unallocated Links | `unallocated-links` | **Already atomic — no split needed.** Read the existing draft at `mintlify-docs/unallocated-links.mdx` (~430 words): "Why this feels wrong at first," "What this actually enables" (print-before-link, spares, permanent shortener case), and "What happens if someone scans one" (resolves to a "not connected yet" page, confirmed by the `UnallocatedLinkPage`/`LinkNotReadyView` components in `src/components/blocks/`). This is exactly Linear's shape — one coherent narrow idea, well within the word bar. Recommend only a small edit: fold in a pointer to the new "What a Link Is" page's shortener framing instead of restating "a shortener, with nothing else attached, ever" from scratch, so the two pages agree on one canonical phrasing rather than two near-duplicate ones. |
| 8 | Claim-on-scan | Scanning a pre-existing, third-party QR code for the first time adopts it: QRtub mints a Link bound to that code's decoded text, so every later scan of the same physical code resolves to the same Link. | Links | Claim-on-Scan | `claim-on-scan` | **Standalone.** ~350–450 words: the two-step resolve flow (`GET /api/access-links/resolve` looks up by parsed slug OR by `metadata.scanHash`; `POST` is the actual claim, idempotent — a second scan of the same code returns the existing Link instead of minting a duplicate); the mechanism (SHA-256 hash of the trimmed decoded text, stored at `metadata.scanHash`, with `metadata.scanValue` kept alongside so the original code can be re-previewed since a hash can't be reversed); and that the minted Link is an ordinary random link underneath. GLOSSARY.md already gives this its canonical name ("Claim-on-scan: First scan claims ownership of a Link") and explicitly warns against "Activation" or "registration" as synonyms. |
| 9 | Deleting a Link vs. releasing it from an Item | Deleting permanently removes a Link row; releasing only detaches it from an Item and leaves the Link (and any printed code) alive. | Links | Deleting and Releasing Links | `deleting-and-releasing-links` | **Standalone**, covering both as one contrast rather than two separate pages — this mirrors Linear's own "Delete and archive issues" being a single page about the two related actions. Real content: deletion is a hard delete (`ServerAccessUrlsService.delete` / `bulkDelete`), blocked by `assertLinksNotInProtectedBatch` when the link belongs to a batch that's past `draft` status (printing/printed/deployed) — printed physical codes can't be orphaned by an accidental delete; releasing (`unassignFromThing` / `bulkUnassignFromThing` / `unassignByThingIds`) only sets `thing_id = NULL`, is not blocked by print status, and happens **automatically** — not as a user choice — whenever an Item or a whole Tub is deleted (confirmed in `CLAUDE.md` §2b and the `/api/things` and `/api/tubs/[id]` DELETE routes), specifically so a physical code stays resolvable after the thing behind it is gone. This "why didn't deleting the item delete the link too" question is exactly the kind of surprising, narrow, well-defined behavior worth its own page. |
| 10 | Bulk link CSV import (with dry-run) | Create or update many Links at once from a CSV, previewable with a dry-run pass before anything is written. | Bulk Link Operations | Bulk Link Import via CSV | `bulk-link-csv-import` | **Standalone.** ~550–700 words: the column schema (`link_type`, `url`, `destination_url`, `is_active`, `fallback_url`, `fallback_message` — confirmed in `access-link-csv-import-service.ts`); per-type create rules (random: `url` must be blank, custom: `url` required and slug-validated at commit, numbered: `url` must match an existing owned pattern); how a row becomes an update instead of a create (matched by `url`, case rules mirroring each strategy); in-file duplicate detection (two rows creating the same slug); and the dry-run flow (`?dryRun=1` on `POST /api/access-links/import`, runs the same classification and pattern/reserved checks without writing, so the preview's created/updated counts are honest). |
| 11 | Bulk assign / unassign / delete | Acting on many Links at once — either an explicit list of IDs or every Link matching a filter/search scope (including "select all pages"). | Bulk Link Operations | Bulk Assigning, Unassigning, and Deleting Links | `bulk-link-assign-unassign-delete` | **Standalone.** ~400–550 words: the two selection models — by explicit ID list (`bulkAssignToThing`/`bulkUnassignFromThing`/`bulkDelete`) vs. by scope (`bulkAssignToThingByScope`/`bulkUnassignFromThingByScope`/`bulkDeleteByScope`, which resolves team + strategy + assigned + filters + search server-side so a "select all 4,000 links matching this filter" action doesn't require shipping 4,000 IDs to the browser first); and that bulk delete carries the same protected-batch guard as a single delete (a bulk delete is rejected in full if any link in scope belongs to a non-draft print batch — no partial silent deletion). |
| 12 | Bulk QR code export | Downloading the QR code image for one or many Links at once. | Bulk Link Operations | Downloading QR Codes | `downloading-qr-codes` | **Standalone**, but must be written to the actual, narrower feature — see the correction note above. ~350–450 words: single-Link download is a direct PNG; selecting more than one automatically switches to a zipped bundle of PNGs with live progress (`generateQrZipBytes`, batched 50 at a time, a persistent toast during multi-thousand-link exports); fixed appearance (1024px, margin 2, black-on-white — `QR_CODE_OPTIONS`); and an explicit, stated limitation: **no SVG export and no configurable error-correction level exist today** — every QR is PNG at a fixed size, and error correction uses the `qrcode` package's implicit default rather than an exposed setting. Do not describe format choice or EC-level tuning as available. |

---

## Notes on nav structure

Current `docs.json` has no "Links" group at all — the existing pages that touch this domain
(`creating-your-first-link.mdx`, `key-concepts.mdx`) are both **cross-domain** tutorial/overview
pages that also cover Item, Media, and Tub, so they aren't wholly "owned" by this inventory and
will need to be dismantled by whoever owns those other domains too. This inventory proposes two
new sibling top-level groups, matching Linear's own pattern of splitting a broad area into
adjacent sidebar groups (their "Issues" vs. "Issue properties"):

- **Links** — rows 1, 3–9 (the entity itself and its lifecycle)
- **Bulk Link Operations** — rows 10–12 (doing any of the above at scale)

Row 2 has no page of its own (folded into row 1) and row 7 keeps its existing filename/slug
(`unallocated-links`) since it's already correctly scoped and shipped in draft form.

## Open questions for the drafting phase

1. **Numbered Links: one page or two?** I've called it one page (row 5) because the brief's own
   bullet groups "prefix/suffix/digits, claiming a range, conflict checking, the per-request cap,
   the plan-tier limit" as a single item, and because Linear's own `sla.md` (994 words) shows a
   single page can carry this much detail. But it is the densest page in this inventory, and if
   the drafted page runs meaningfully past ~1000 words, splitting "claiming a pattern" (ownership,
   ownership-matching, deletion, plan limits) from "generating numbered links" (auto/specific/range
   modes, conflict checking, per-request cap) is the natural seam. Flagging rather than deciding
   unilaterally since it cuts against the brief's own grouping.
2. **Is the plan-tier numbered-pattern limit actually enforced, or just advertised?** I found the
   numbers (Starter: 0, Professional: 1, Scale: up to 5) only in `stripe-plans.ts`'s marketing
   feature-list copy. I could not find server-side code that rejects claiming a 2nd pattern on a
   Professional plan (no quota/entitlement check in `claimPattern`, the `numbered-patterns` POST
   route, or anywhere else I searched). The docs should state the number (it's a real, published
   plan fact) but whoever drafts this page should re-confirm with engineering whether it's actually
   gated in code or is aspirational pricing copy — the current page should not claim an enforcement
   mechanism ("you'll be blocked at...") that may not exist.
3. **Ownership boundary with Direct Mode / Page Mode.** GLOSSARY.md defines Direct Mode and Page
   Mode as Link-level concepts (which destination_url/destination_config shape a Link has), but
   they're really about what a Link *points to*, which is likely to be owned by whoever's doing the
   Destinations/Pages domain inventory rather than this one. I deliberately excluded them from this
   inventory since the brief's bullet list for this domain doesn't mention them, but the two
   inventories should cross-link rather than each silently assuming the other covers it.
4. **Should "What a Link Is" or "Unallocated Links" be the one that states the shortener framing
   canonically?** I recommended "What a Link Is" (row 1/2) as the canonical home and having
   `unallocated-links.mdx` reference it, but since that page already exists and already makes the
   point once, the drafting phase could equally decide to keep it there and have "What a Link Is"
   defer to it. Either is fine; just pick one so it isn't stated twice in slightly different words.
