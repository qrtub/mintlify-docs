# Editorial Audit — `/industries/local-government-councils.mdx`

**Page:** QRtub for Local Councils
**Live:** https://help.qrtub.com/industries/local-government-councils
**File:** `/workspace/mintlify-docs/industries/local-government-councils.mdx`
**Bar applied:** citability/accuracy for a marketing/industry landing page (per task brief), not the how-to bar used for `/help/*` pages.
**Cross-checked against:** `../qrtub/BRAND.md` (Feature Status, Claims That Are TRUE/FALSE), `../qrtub/GLOSSARY.md`, `../qrtub/src` (bindings, database schema, team/tub permission model), sibling pages in the same `docs.json` nav group (`industries/civil-construction.mdx`, `industries/electrical-test-and-tag.mdx`, `industries/contract-cleaning.mdx`, `industries/arboriculture-tree-management.mdx`).

---

## 1. Page Overview

Frontmatter is complete (`title`, `description`, plus non-standard `industry`/`featured` fields used by the industries nav, which is fine — CLAUDE.md only mandates `title`/`description`). No internal links anywhere in the body (only the standard footer CTA to `qrtub.com/pricing` and `mailto:hi@qrtub.com`) — the page never links to any `/help/*` page even though it describes URL Templates, contractor access, and Tub/department structure in depth. This is a missed cross-link opportunity for a chunk-based retriever, though it matches the pattern on every other industries page (none of the five link internally either).

The page sits in `docs.json` under `tab: "Industries" → group: "Industries"`, alongside `civil-construction`, `contract-cleaning`, `arboriculture-tree-management`, and `electrical-test-and-tag`. Per the external retrieval audit (`audit/_raw/external/llms-full.md`), this page is one of 19 pages Mintlify auto-concatenates into `llms-full.txt` and auto-lists in `llms.txt` — there is no custom `llms.txt` in this repo curating that list down, so every page in `docs.json`'s nav, marketing or technical, is exposed identically to AI retrieval.

## 2. Sibling Page Overlap & Redundancy Context

All five industry pages (confirmed by diffing headings against `civil-construction.mdx`, `electrical-test-and-tag.mdx`, `contract-cleaning.mdx`, `arboriculture-tree-management.mdx`) share one template almost verbatim, with only the nouns swapped:

```
## The Challenge
## One Code[...], All Audiences See Everything
## The [Industry Noun] Effect          <!-- "Restaurant Kitchen", "Urban Forest Dashboard", "Professional Council" -->
## Bulk Deployment Across [Population]
## Real-World Example: [Scenario]
## Integration with [Industry] Software/Systems
## Getting Started
## Why [Industry] Choose QRtub
## Ready to Deploy?
**Core features available:**
```

This is confirmed structurally (not byte-identical) by the external audit's finding that `## Ready to Deploy?` + `**Core features available:**` appears on exactly 5 pages, all industries pages, and that the "Note on security" paragraph (buttons visible but still gated behind login) appears on exactly 3 pages including this one (arboriculture, contract-cleaning, local-government-councils).

Practical implication for retrieval: an AI agent that has already ingested one industry page has effectively already seen this page's rhetorical shell — the "public sees operational buttons and infers professionalism" argument, the "print before systems are ready" argument, the "update destinations without retagging" argument. The only genuinely new information this specific page contributes is the concrete list of council-specific nouns (asset types, department names, named software: TechnologyOne, Assetic, Conquest, Molo, Pathway, Mitti, Proludic, Arborcheck) and the two claims flagged in §3.4/§3.5 below. That is a thin unique-information yield for a page of this length (258 lines) — most of the page is the shared rhetorical template restated in council vocabulary.

## 3. Claim-by-Claim Accuracy Audit

### 3.1 Verified accurate

- **URL Templates syntax** (line 119): `yourinspectionapp.com/asset?id={{item.assetID}}&type={{item.assetType}}&location={{item.location}}&department={{item.department}}` — correct double-brace, `item.`-namespaced syntax per `../qrtub/src/lib/page/bindings.ts` and CLAUDE.md. No claim of automatic URL-encoding is made, which is correct (there is none).
- **"No per-asset configuration needed"** for URL Templates — accurate; this is exactly what URL Templates do (bind once, resolve per Item).
- **"Update without retagging"** claims throughout — accurate; Links/Destinations are decoupled from physical Media per the entity model, and this is a BRAND.md "Claims That Are TRUE" item.
- **"No contractor logins to provision in QRtub"** — accurate. Destinations are plain URLs; QRtub has no per-viewer authentication layer of its own, so this claim holds.
- **"Findings are saved in the inspection app you use"** (line 134) and **"Record work performed in your maintenance system"** (line 139) — these correctly attribute storage to the third-party system, not QRtub. This is exactly the phrasing CLAUDE.md asks for ("opens the inspection... pre-filled", not "syncs").
- **"Basic Media tracking"**-adjacent language: the page only ever describes ordering/tagging physical Media in general terms (materials, weatherproofing) and never claims batch tracking, cost tracking, or Media inventory — so it does not trip the BRAND.md false claim "Full Media inventory management is available."
- **No BETA language** — the page never describes QRtub as in beta, consistent with the August 2026 retirement of that word (per CLAUDE.md), even though `../qrtub/BRAND.md` itself still says "QRtub is currently in BETA" under Claims That Are TRUE (1.5) and titles its feature table "1.4 Feature Status (BETA)". That is a drift between the two source-of-truth files, not a defect in this page — flagged here only so it isn't mistaken for something this page got right by accident.

### 3.2 "Each sees only their assets" — unverified / likely overclaim

Line 111: *"Same platform. Different departments. Each sees only their assets. Unified access in the field."* This, plus the "Ready to Deploy" bullet *"Department-isolated Tubs with cross-visibility options"* (line 246), reads as a claim that QRtub enforces per-department access control — i.e., a Parks Tub is actually walled off from a Facilities Tub for a given staff member, with an option to grant cross-Tub visibility where wanted.

Checked against the app source:
- Team membership (`../qrtub/src/lib/database/generated-types.ts`, `teams`/team-user tables) carries a `role` field, but that role is scoped to the **team/account**, not to an individual Tub. `../qrtub/src/app/app/team/page.tsx` and `../qrtub/src/app/features/page.tsx` ("Invite team members to collaborate on your Tubs. Control access with role-based permissions.") describe account-level roles, not per-Tub restriction.
- A `shared_tubs` table exists in the database schema (`permission_level`, `share_team_id`, `tub_id`) that would be the natural mechanism for "cross-visibility options" — but it is referenced **nowhere else in `src/`** outside the generated Supabase types. No route, hook, or component reads or writes it. It is schema scaffolding with no shipped feature behind it.
- `../qrtub/BRAND.md` §1.4 lists "Granular permissions" and "Cross-account transfer & sharing" as **Planned**, and §1.6 "Claims That Are FALSE" explicitly lists "QRtub has granular permissions/sharing between organizations (planned)."

**Conclusion:** "Each sees only their assets" and "cross-visibility options" describe a per-Tub access-control feature that does not appear to be built. Separate departments can get separate Tubs (true — Tubs are just workspaces), but nothing in the code enforces that a Parks officer's login can't also see the Facilities Tub, and no cross-visibility toggle exists to grant it deliberately either. This is the page's clearest instance of the BRAND.md-forbidden claim category ("granular permissions... planned"), phrased in industry-specific language rather than QRtub's own terms, which is likely why it wasn't caught by a literal glossary/terminology scan.

### 3.3 "View Inspection History" / "Access complete inspection records" — ambiguous attribution

The page is generally careful to say a Destination "opens" or "saves in" the third-party app (see §3.1). But the "View Inspection History" button (appears 4 times: lines 39, 53, 146, 152) and "Access complete inspection records" (line 153, Auditor persona) are never explicitly attributed to an external system the way "Log Maintenance" and "Playground Safety Inspection" are. A reader — human or AI agent — could reasonably conclude QRtub itself holds an inspection history log that Destinations merely surface, rather than that button being one more link-out to whatever inspection platform the council already uses.

That distinction matters because `../qrtub/BRAND.md` §1.6 explicitly lists as FALSE: *"QRtub tracks maintenance history (it links to systems that do)."* The page never says this outright, but it also never says the opposite for this specific button, unlike every sibling button. Recommend making the external attribution explicit here too (e.g., "opens your inspection app's history view for this asset") so an AI agent answering "does QRtub keep an audit trail?" can't reasonably cite this page for "yes."

### 3.4 Named third-party systems — real companies, unverifiable partnership claims

Line 190: *"Asset management: TechnologyOne, Assetic, Conquest, Molo, Pathway"*; line 191: *"Inspections: Mitti (formerly SafetyCulture), Proludic (playgrounds), Arborcheck (trees)"*. These are real, named enterprise vendors used by Australian/NZ councils. Per CLAUDE.md's own guidance ("What 'integration' means here" — QRtub has no API integrations; it builds a URL that opens them) naming a specific vendor is only accurate if the intent is "you can build a URL Template to this vendor's product," which is true of any URL-accepting system and isn't unique to these five names. The page doesn't claim sync or write-back for any of them (good), but presenting them under a heading literally titled "Integration with Council Systems" — with the lead sentence "QRtub connects to the systems councils already use" — invites an AI agent or reader to treat this as a verified, tested partner list rather than a set of illustrative examples. Given TechnologyOne, Assetic, Conquest, Molo and Pathway are real, identifiable companies, an inaccurate implied-partnership claim carries more real-world risk than a generic "CMMS platforms" reference (which the sibling `civil-construction.mdx` page uses instead: "UpKeep, Fiix, Maintenance Connection" under the same pattern — so this is a corpus-wide pattern, not unique to this page, but worth flagging here since the task is a per-page audit).

Recommend either: (a) softening the heading/lead sentence to make explicit these are examples of systems a URL Template can point to, not tested integrations, or (b) confirming with product/BRAND owners that these five names are deliberately chosen because customers have actually configured URL Templates against them, in which case the current wording is fine and this is a non-issue.

### 3.5 Everything else in "Why Councils Choose QRtub" / "Use Cases" / "Getting Started"

These sections restate claims already covered above (public accountability narrative, contractor coordination, field accessibility, audit access) without introducing new capability claims. No additional BRAND.md violations found in these sections beyond the ones already listed in 3.2–3.4.

## 4. Internal Redundancy (within-page duplication)

Beyond the cross-page template overlap in §2, this page repeats its own central scenario almost verbatim **twice within itself**. Compare:

> Lines 49–56 ("Public accountability in action"):
> *"A parent at a playground scans the QR code on the equipment, sees: Playground equipment details and age rating / 'Playground Safety Inspection' button (council uses professional inspection systems!) / 'Log Maintenance' button (they track maintenance properly!) / 'View Inspection History' (transparency!) / 'Report Issue' button ← They tap this to report a damaged swing"*

> Lines 141–148 ("Real-World Example" → Parent/ratepayer):
> *"Scan out of curiosity or to report issue / See 'Installed: 2018, Last inspected: 15/02/2025' / See 'Playground Safety Inspection' (council has professional systems!) / See 'Log Maintenance' (they track upkeep!) / See 'View Inspection History' (transparency!) / Trust council's playground management / Tap 'Report Issue' if swing is broken"*

This is the same persona, same asset type (playground/swing), same five bullet beats, restated with cosmetic wording changes roughly 90 lines apart. Combined with the "Professional Council Effect" section (lines 58–76) making the identical "operational visibility signals professionalism to ratepayers" argument a third time in different words, the same rhetorical point is made three separate times in one 258-line page.

For human readability this reads as padding — a reader skimming for "what does this actually do" has to wade through the same anecdote twice. For AI-agent retrieval specifically, this is worse than ordinary repetition: a chunk-based retriever (splitting on `##`/`###`) will very likely return both the "One Code Per Asset" chunk and the "Real-World Example" chunk for the same query (e.g., "how does a resident report a broken playground swing"), consuming two chunks' worth of context budget to deliver one idea, and increasing the odds a summarizing agent presents the same anecdote twice in one answer as if they were two different examples.

**Recommendation:** cut the "Public accountability in action" bullet list (lines 47–56) entirely and let the later "Real-World Example" section carry the one full worked scenario — or vice versa — rather than running both.

## 5. AI-Retrieval / Chunking Assessment

- Fifteen `##` H2 sections, reasonably sized (mostly under 30 lines each) — good chunk granularity if a Mintlify/naive chunker splits on H2.
- Several structural sub-groupings rely on bold text as pseudo-headers (`**Parks & Reserves Tub:**`, `**Contractor** (during maintenance):`) rather than real headings. A chunker that only recognizes `#`/`##` will fold five distinct department blocks (Parks & Reserves, Buildings & Facilities, Fleet & Equipment, Urban Forest, Infrastructure) into one "Multi-Department, Unified Access" chunk — acceptable size-wise, but means a query about one specific department (e.g., "does this work for street trees") retrieves the whole multi-department block rather than a tight, tree-specific chunk.
- No internal links to `/help/*` pages that explain the mechanics referenced here (URL Templates, Tubs, Destinations) in more procedural depth — an agent that retrieves this page for "how do I set up department Tubs" has nowhere to route the user for the step-by-step version.
- The repeated three-times narrative (§4) and the five-page shared template (§2) both dilute the signal-to-noise ratio of any retrieval corpus this page is part of.

## 6. Human Readability Notes

- Voice mostly matches BRAND.md's "quietly confident" register in the mechanical sections (Bulk Deployment, Getting Started, Integration), but the "Professional Council Effect" / "public accountability in action" sections lean into a more oversold, exclamation-mark-heavy style ("(council uses professional inspection systems!)", "(they track upkeep!)", "(transparency!)") that reads closer to BRAND.md's own "too casual" example than its "just right" calibration. Recommend trimming the parentheticals and exclamation marks — the underlying point (visible operational buttons read as professionalism) can be made once, plainly, without needing three repetitions and inline cheerleading.
- Terminology: "asset"/"asset management" is used throughout in place of QRtub's canonical "Item" — this is explicitly sanctioned by CLAUDE.md's industry-page guidance ("a council's 'assets'" is named as the correct customer-domain term to use), so this is **not** a glossary violation, just noting it was checked.
- Tub/Page/Destination/Link capitalization is consistent with GLOSSARY.md throughout (no "Profile Page," no "Asset" as the QRtub entity name, no "Access Link").

## 7. llms.txt Inclusion — Verdict

**No — this page should not be in the `llms.txt` / `llms-full.txt` index as currently written**, though not for the blanket reason "it's marketing, marketing doesn't belong in docs." The reasoning is narrower and more actionable:

1. **It contains at least one claim (§3.2) that matches BRAND.md's own "Claims That Are FALSE" category** — a specific, checkable capability claim (per-department Tub access control with a cross-visibility option) that the source code does not support. An AI support agent or ChatGPT/Perplexity answering "can we restrict a department's Tub from other departments?" by citing this page would give a council prospect a wrong, sales-relevant answer. That is exactly the failure mode a technical citability index (as opposed to an SEO landing page) needs to avoid, and it's a materially different risk than the same wording sitting on the human-browsed marketing site, where a salesperson can correct it in conversation.
2. **One ambiguous attribution (§3.3)** compounds this: "View Inspection History" reads as if QRtub itself holds the record, contradicting another explicit BRAND.md false-claim entry ("QRtub tracks maintenance history").
3. **Low unique-information density relative to its length** (§2, §4): three internal restatements of the same anecdote plus a five-page shared rhetorical template mean an agent spends a disproportionate share of retrieved context on repeated persuasion copy rather than new facts, compared to the `/help/*` pages in the same index, which are dense, single-purpose, and mechanically verified line-by-line per CLAUDE.md's own checklist.
4. Practically, Mintlify's default `llms.txt`/`llms-full.txt` generation (confirmed via `audit/_raw/external/llms-full.md` and the Mintlify guidance doc) lists every page in `docs.json`'s nav with no way to exclude one page short of a custom `llms.txt` file, which this repo does not yet have. So the real decision isn't "add a noindex flag to this one file" — it's whether to introduce a custom `llms.txt` that curates `/help/*` and `/integrations/*` in (mechanically verified, narrow-claim content) while leaving `/industries/*` to be discovered the normal way (site nav, sitemap, Google/GEO) rather than through the same index a support bot uses to answer "can QRtub do X."

**Bottom line:** fix §3.2 and §3.3 (they're the actual accuracy defects), trim the internal repetition (§4), and only then is this page fit to sit in the same index as `/help/*` content. As currently written, it should not be treated as an equally-citable source in `llms.txt`.

## 8. Summary of Recommended Fixes

1. **(High)** Rewrite or remove "Each sees only their assets" (line 111) and "Department-isolated Tubs with cross-visibility options" (line 246) — no per-Tub access-control or cross-visibility feature exists in the app. If department separation is only ever "put them in different Tubs, anyone on the team can still see every Tub," say that.
2. **(Medium)** Clarify that "View Inspection History" / "Access complete inspection records" open the council's own inspection platform, not a QRtub-native log — match the explicit-attribution treatment already given to "Log Maintenance" and "Findings are saved in the inspection app you use."
3. **(Medium)** Soften "Integration with Council Systems" heading/lead sentence, or confirm the five named asset-management vendors (TechnologyOne, Assetic, Conquest, Molo, Pathway) are deliberately chosen as verified customer configurations rather than illustrative examples.
4. **(Medium)** Cut the duplicate playground/parent anecdote — keep either the "Public accountability in action" bullets (lines 47–56) or the "Real-World Example" persona table (lines 126–157), not both.
5. **(Low)** Add at least one or two internal links to relevant `/help/*` pages (e.g., URL Templates, Tubs) so an agent that lands here for the pitch has somewhere to send the reader for mechanics.
6. **(Process)** Once 1–4 are fixed, this page is reasonable to keep in `llms.txt`; until then, recommend a custom `llms.txt` that excludes `/industries/*` from the AI-citability index while leaving it fully live and indexable on the normal site.
