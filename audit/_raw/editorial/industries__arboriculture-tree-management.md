# Editorial Audit — /industries/arboriculture-tree-management

**File:** `/workspace/mintlify-docs/industries/arboriculture-tree-management.mdx`
**Live:** https://help.qrtub.com/industries/arboriculture-tree-management
**Nav group:** Industries → Industries (siblings: `industries/civil-construction`,
`industries/contract-cleaning`, `industries/electrical-test-and-tag`,
`industries/local-government-councils`)
**Page type:** Vertical marketing / landing page, not a how-to page. Assessed below against
citability-and-accuracy criteria (is every capability claim true, per `../qrtub/BRAND.md`
§1.5–1.6 and the app source), not the how-to rubric (self-containment, answer-first, chunk
integrity) used for `/help/*` pages in this same audit.

---

## 1. CLAIM-BY-CLAIM ACCURACY AUDIT

Every distinct capability claim on the page, checked against `../qrtub/BRAND.md` §1.4–1.6,
`CLAUDE.md`, and (where named) the app source.

| Claim (paraphrased, with line) | Verdict | Basis |
|---|---|---|
| "One professional QR code per tree" / bulk Link generation for tree populations (lines 8, 74–83, Getting Started #1) | **True** | Bulk Link generation is `Available` per BRAND.md §1.4. |
| URL Templates auto-fill `{{item.assetID}}`, `{{item.species}}`, `{{item.location}}` (lines 79–83) | **True, syntax correct** | Matches the documented double-brace `{{item.field}}` binding syntax (`CLAUDE.md` → `../qrtub/src/lib/page/bindings.ts`). URL Templates are `Available` per BRAND.md. |
| "Update the Destination... Physical tags stay unchanged" (line 85, and repeated in "50-year asset lifecycle") | **True** | Matches the core, verified "update without reprinting" mechanic. |
| Everyone scanning sees the same Page with all Destinations, public taps what's relevant (lines 26–41) | **True** | Matches Page Mode / Destinations model (`Available`). |
| "**Note on security:** ...they reach your inspection app which requires login credentials. The Page shows what's available; your existing systems control who can actually access them" (line 41) | **True and well-hedged** | Correctly attributes access control to the external system, not QRtub — the one place on this page that gets the mechanism right. |
| "Log Maintenance" / "View Inspection History" Destinations shown to council staff (lines 34–35, 96–99, 106–107) | **True as Destinations, but see §3** | A Destination is just a link/button (per `../qrtub/GLOSSARY.md`: "Destination... an external URL"). Fine as long as the reader understands the *history itself* lives in the external system QRtub links to, not in QRtub. This page never states that explicitly — see §3. |
| "Arborists... Log findings and risk rating" (line 94) | **Unverifiable / risky as worded** | See §3 — reads as QRtub doing the logging when read out of surrounding context. |
| "Report Tree Issue" as a "public reporting" capability (lines 36, 155) | **Technically true, imprecisely worded** | A Destination *can* be a link to a form (per GLOSSARY.md), so "public reporting" is achievable — but only as a link the council configures to their own intake system/email/form. The page never says this is "just a Destination you point somewhere," so it reads as a built-in reporting/ticketing feature. Worth one clarifying sentence. |
| "Order tree tags — Weatherproof aluminium tags, stainless steel plaques, or durable vinyl labels" (line 153) | **True but requires care** | This describes the customer's own procurement action (buying physical media from a supplier), not a QRtub fulfillment service. Matches "Basic Media tracking" (`Available`) but reads adjacent to Media Templates / Media Batch management, which are `Planned` per BRAND.md §1.4. The page doesn't claim QRtub supplies or designs the physical tag, so this clears the bar, but it sits one sentence away from overclaiming — the identical pattern appears verbatim across all four sibling industry pages. |
| "QRtub connects to the tools arborists and councils already use" + named platform list (lines 141–146) | **Overclaims the mechanism — see §2** | `CLAUDE.md` is explicit: *"QRtub does not have API integrations with third-party products. It builds a URL that opens them... Never write 'integrates with X' in a way that implies data exchange, sync, or write-back."* This page never states the actual mechanism (deep link / URL only, no data exchange) anywhere, unlike the dedicated `integrations/safetyculture.mdx` page, which states outright: *"No data is exchanged between the two systems; QRtub builds the URL and Mitti handles the rest."* A reader (or an AI agent) relying on this page alone has no way to know that "connects to" means "opens a URL," not "syncs with." |
| Named platforms: Arborcheck, TreePlotter, CONFIRM (Abacus), TreeWorks, Mitti (line 143) | **One fabricated attribution, one category error — see §2** | See §2 for the detailed check. |
| "QRtub is currently in BETA" | **N/A — correctly absent** | Per `CLAUDE.md`, the word "BETA" was retired site-wide in August 2026. This page correctly does not use it. |

## 2. THIRD-PARTY PRODUCT VERIFICATION

The brief calls for checking whether every capability claim is actually true. Because this
page names five specific external products as tree-management software QRtub "connects to,"
those names were checked (web search, since they're outside the app repo):

- **"CONFIRM (Abacus)"** — **The parenthetical vendor attribution appears to be wrong.**
  Confirm is a real asset-management platform used by UK councils for infrastructure
  including street trees — but it is not made by a company called Abacus. It was developed
  under Yotta and is now sold under Siemens (`assetmanagement.siemens.com/products/confirm`).
  No connection between "Confirm" and "Abacus" turned up in any source checked. This reads
  like an invented or confused vendor credit sitting inside a claim about what QRtub
  "connects to" — exactly the kind of detail an AI agent would repeat verbatim if it cited
  this page, and exactly the kind of detail a reader at the actual council using Confirm
  would immediately flag as wrong.
- **"Arborcheck"** — **Real, but not the kind of product this list implies.** Arborcheck is a
  chlorophyll-fluorescence—based tree health *diagnostic instrument* (a physiological stress
  measurement system built on research from Bartlett Tree Research Laboratories), not a
  records/inspection software platform with forms or a mobile app that a QR-code deep link
  would open. Listing it alongside TreePlotter, CONFIRM, TreeWorks and Mitti under "Tree
  inspection platforms" that URL Templates "auto-populate... pre-fill inspection forms" for
  is a category error: there is no indication Arborcheck has a URL-addressable
  form/app to pre-fill. The same "Arborcheck (trees)" credit is repeated in
  `industries/local-government-councils.mdx` (line 191), so the error is duplicated, not
  isolated to this page.
- **TreePlotter, TreeWorks** — Real products (PlanIT Geo's TreePlotter; The Kenerson Group's
  TreeWorks, an ArcGIS Pro extension for urban forestry). Plausibly real software with field
  data collection, though this audit did not confirm either supports URL query-parameter
  pre-fill specifically — the mechanism this page's central claim depends on.
- **Mitti (formerly SafetyCulture)** — **Correct**, and the naming matches the site's own
  canonical reference (`integrations/safetyculture.mdx`): "iAuditor → SafetyCulture → Mitti
  (August 2026)."

**Net:** of five named tree-software integrations, one carries a vendor attribution that
doesn't check out (CONFIRM/Abacus) and one is miscategorized as inspection-form software when
it's actually a hardware diagnostic system (Arborcheck). For a page whose entire "Integration"
section exists to be citable ("what does QRtub work with for tree inspections"), getting two
of five names wrong is a real accuracy problem, not a style nit.

## 3. INSPECTION-LOGGING WORDING — READS AS A CLAIM QRTUB DOESN'T MAKE ELSEWHERE

BRAND.md §1.6 lists as explicitly **FALSE**: *"QRtub provides inspection capabilities"* and
*"QRtub tracks maintenance history (it links to systems that do)."*

This page's "Real-World Example" section (lines 91–94) reads:

> **Arborists** (during scheduled inspection):
> - Tap "Start Tree Health Assessment"
> - Complete inspection using pre-filled tree data
> - Log findings and risk rating

Read after the earlier "Note on security" paragraph (line 41, which does establish that
tapping a button "reaches your inspection app"), a careful reader can infer that
"complete inspection" and "log findings" happen in the *external* app, not in QRtub. But nothing in this specific section restates that — there's no "...in your inspection app" or
equivalent qualifier attached to the action itself. Compare the sibling
`industries/electrical-test-and-tag.mdx`, which is careful every time this exact narrative
beat recurs: *"Complete test, apply new tag. Test record is saved in the testing app you
use."* That qualifying clause is missing from the parallel arboriculture beat and from the
"Park Tree" example (lines 91–99) alike.

This matters specifically for AI-agent retrieval: if a chunker or a support bot surfaces only
this bullet list (a very plausible retrieval unit — it's a tight, self-contained-looking
block), the natural paraphrase is "QRtub lets arborists log inspection findings and risk
ratings," which is exactly the false claim BRAND.md prohibits. The fix is a one-clause
addition, not a rewrite — add the same disclaimer sibling pages already use.

## 4. OVERLAP / REDUNDANCY WITH SIBLING PAGES — AND A DIRECT CONTRADICTION

Per `CLAUDE.md`: *"Search for existing information before adding new content. Avoid
duplication unless strategic."* Industry pages are explicitly meant to share a template
("change the nouns, not the verbs") — but this page's tree content specifically duplicates,
and in one place contradicts, `industries/local-government-councils.mdx`:

- **Structural duplication is total.** Every major heading here — "One Code, One Page, All
  Audiences See Everything," the "Note on security" paragraph (byte-identical apart from the
  quoted button name), "The [X] Effect," "Bulk Deployment," "Real-World Example," "Public
  Education," "Integration with [X] Software," "Getting Started," "Why [X] Choose QRtub,"
  "Use Cases," "Ready to Deploy?" — reappears in the same order in
  `local-government-councils.mdx`, which *also* covers street trees, heritage trees, and park
  trees as one of its explicit asset categories (its own text: *"parks, playgrounds,
  buildings, equipment, trees, signs, infrastructure"*; *"Street trees: Tag urban forest
  population. Arborists perform assessments. Public learns about species, reports
  concerns."*, line 234). A support bot asked "does QRtub work for council street trees" has
  two nearly-equally-relevant pages to choose from, with no cross-link between them.
- **The two pages give contradictory answers to the same question.** This page's
  Integration section lists five tree-inspection platforms: *"Arborcheck, TreePlotter, CONFIRM
  (Abacus), TreeWorks, Mitti (formerly SafetyCulture)"* (line 143). The council page's
  Integration section, addressing the identical use case (council-managed trees), lists only
  one: *"Arborcheck (trees)"* (line 191), alongside a totally different asset-management
  vendor list (TechnologyOne, Assetic, Conquest, Molo, Pathway) that doesn't appear on this
  page at all. An AI agent asked "what tree-inspection software does QRtub connect to" will
  give a different answer depending on which of these two pages it retrieves — a direct,
  checkable inconsistency between two live, currently-published pages.
- **Neither page links to the other.** Given the topical overlap, a reader landing on either
  page has no signal that a second, overlapping page exists.

## 5. VOICE / FRAMING RISK — NARRATED "THOUGHTS" READ CLOSE TO INVENTED TESTIMONIALS

BRAND.md §3.2 ("Never"): *"Invent customer quotes or case studies."* This page (and its
siblings) repeatedly narrates an invented persona's internal thought in quotation marks as if
recorded:

> "They think: 'The council actually manages these trees professionally. I can report issues
> directly. And I learned something about this tree.'" (line 68)

This isn't attributed to a named customer, so it doesn't cross into a fabricated case study
in the strict sense BRAND.md is guarding against — but structurally it's doing the same
rhetorical job (a quoted reaction presented as representative and specific), and an AI agent
summarizing the page could easily present it as "users report that..." if it doesn't parse
the hypothetical framing correctly. Low severity, but worth flagging since it recurs multiple
times across the page (lines 68, 108) and the pattern is shared with every sibling industry
page.

## 6. SHOULD THIS PAGE BE IN llms.txt / llms-full.txt?

**No — not as currently written.** Reasoning:

1. **It fails the one bar that matters most for an AI-consumed index: every claim must be
   citable and true.** This audit found one fabricated/unverifiable vendor attribution
   (CONFIRM/Abacus), one category error presenting a diagnostic instrument as inspection
   software (Arborcheck), an integration-mechanism claim ("connects to") that omits the
   "URL-only, no data exchange" caveat the site states clearly elsewhere, and inspection/
   logging language that reads as a capability claim BRAND.md explicitly lists as false when
   read out of surrounding context — exactly the retrieval mode `llms.txt` exists to serve.
   A support bot or research agent treats every sentence in an `llms.txt`-indexed page as
   equally authoritative; this page hands it two wrong facts and one exploitable ambiguity.
2. **It duplicates, and in one place contradicts, a sibling page that is also indexed.**
   `local-government-councils.mdx` covers council-managed trees directly and is also in
   `llms.txt`. Having both in the same AI-facing index means the index itself contains
   conflicting answers to "what tree software does QRtub connect to" — a self-inflicted
   consistency problem that costs tokens and invites a wrong citation regardless of which
   page the agent happens to retrieve.
3. **The page is marketing rhetoric, not documentation, and reads as such even mid-page.**
   Lines like "the operational buttons they won't use demonstrate professional tree
   stewardship" and "This builds community trust" are persuasion copy aimed at a prospective
   customer deciding whether to buy QRtub — not information a support agent would ever need
   to answer a user's product question, and not something a coding agent integrating QRtub
   needs either. Mixing this register into a technical index degrades signal-to-noise for
   every other page in the same file, and risks an agent citing sales narrative
   ("demonstrates professional urban forest management") as if it were a documented product
   behavior.

**Conditional path to "yes":** if the `/industries/*` pages stay in the index at all (a
separate, defensible decision — they do answer "does QRtub work for my industry," a real
question), this specific page should not go back in until: the two false/miscategorized
vendor names are fixed or removed, the integration section states the actual mechanism (URL
deep link, no sync — matching `integrations/safetyculture.mdx`'s own wording), the
inspection-logging bullets get the same disclaiming clause the electrical-test-and-tag page
already uses, and the overlap with `local-government-councils.mdx` is resolved (either merge
the tree content into one canonical page with a cross-link, or make the two pages' vendor
lists consistent). Until then, the safer default for an AI-facing index is to exclude it and
keep the marketing tab as a human-only surface, or ship a stripped verified-facts-only
version specifically for `llms.txt` that omits the persuasion framing and the unverified
vendor list.

---

## Recommendation

Do not add this page to `llms.txt` in its current form. Fix, in order of severity: (1) the
CONFIRM/Abacus attribution and the Arborcheck category error — both are checkable-wrong facts
sitting in a "what does QRtub connect to" claim, the single highest-risk sentence type for an
AI index; (2) state the integration mechanism explicitly (URL/deep-link only, no data
exchange) at least once on the page, matching `integrations/safetyculture.mdx`; (3) add the
"...in your inspection app" qualifier to the inspection-logging bullets, matching
`industries/electrical-test-and-tag.mdx`; (4) resolve the tree-content duplication and vendor-
list contradiction with `industries/local-government-councils.mdx`, either by cross-linking
or by consolidating. None of this requires a structural rewrite — the page's shape (problem →
one-code-all-audiences → bulk deployment → integration → getting started → use cases) is
sound and consistent with its siblings; the defects are specific, fixable factual claims, not
the page's approach.
