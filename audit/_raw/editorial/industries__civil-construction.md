# Editorial Audit — /industries/civil-construction (marketing/citability bar)

**File:** `/workspace/mintlify-docs/industries/civil-construction.mdx`
**Live:** https://help.qrtub.com/industries/civil-construction
**Nav group:** Industries tab → "Industries" group (siblings: `industries/contract-cleaning`,
`industries/arboriculture-tree-management`, `industries/electrical-test-and-tag`,
`industries/local-government-councils`)

**Scope note:** per instructions, this page is audited against a *different* bar than the
how-to pages in this repo's editorial audit — not SELF-CONTAINMENT/ANSWER-FIRST/CHUNK-INTEGRITY
criteria, but **citability and accuracy of marketing claims**: is every capability claim true,
against `/workspace/qrtub/BRAND.md` §1.6 ("Claims That Are FALSE") and the app source, and is
the page safe for an AI agent to cite verbatim out of its original marketing context.

---

## 1. CAPABILITY CLAIM LEDGER

Every substantive capability claim on the page, checked against `BRAND.md` §1.4/1.5/1.6, the
sibling pages, and app source where needed.

| Claim (location) | Verdict | Basis |
|---|---|---|
| "Every excavator, loader, or crane gets a single QR code linking to all required systems" (L25) | **True** | Matches BRAND §1.5 ("One QR code can connect to multiple Destinations via a Page") |
| "Mitti (formerly SafetyCulture) inspections for daily pre-start checks" (L26) | **True** | Matches `integrations/safetyculture.mdx` |
| "Maintenance logs / Operator manuals / Service history" as Destinations (L27–29) | **True, correctly scoped** | Framed as "linking to," never "tracking" — matches CLAUDE.md's rule that QRtub does not track maintenance itself |
| "Order stickers before equipment arrives... No dependency on perfect timing" (L34–38) | **True** | Matches UC-004 and `help/print-batches.mdx` (Draft status exists before printing) |
| "Switch inspection apps without reprinting... Connect to different systems on different sites" (L44–47) | **True** | Matches UC-003 |
| "they update 150 destinations in QRtub. Zero reprinting. Zero downtime." (L82) | **True** | Matches UC-003 mechanism — Destinations are what changes, not physical codes |
| "URL Templates auto-populate equipment IDs from QRtub Item fields" (L106) | **True** | Matches CLAUDE.md field-binding section and `integrations/safetyculture.mdx`'s own worked examples |
| "Route operators directly to the right asset in your maintenance system" (L110) | **True** | A Destination URL/deep link with a `{{item.field}}` binding, same mechanism as Mitti section |
| Final feature list: bulk Links, Pages, Mitti/CMMS integration, URL Templates (L174–177) | **True — all "Available"** | Cross-checked against BRAND §1.4; nothing here is drawn from a "Planned" row |
| "Different Information for Different People" section (L51–71) | **Overstated — see §4** | Implies role-based targeting; the actual mechanism is self-selection from one shared Page |
| "Weatherproof codes lasting years" under "Designed for Physical Deployments" (L143) | **Ambiguous — see §4** | Durability is a property of customer-sourced Media, not something QRtub the software "handles" |
| "Order Professional Codes... Order in bulk for cost efficiency" (Step 2, L127–128) | **Ambiguous, not false** | QRtub generates the print-ready export (`help/print-batches.mdx`); it does not manufacture or ship physical stickers/plaques itself. The page never says QRtub does, but it also never clarifies the reader sources printing from their own vendor |

**Net:** no claim on this page states or implies a feature from BRAND.md's "Planned" list
(API access, analytics dashboards, cross-account transfer, granular permissions, payment
Destinations, full Media inventory/Batch/Template management) as available today. That is the
single most important thing this page gets right, and it is worth stating plainly: **this page
does not commit the specific errors CLAUDE.md calls out industry pages for historically.**

## 2. BRAND.MD §1.6 "CLAIMS THAT ARE FALSE" — DIRECT CHECK

Checked each of the twelve listed false claims against this page's text:

| False claim (BRAND.md §1.6) | Present on this page? |
|---|---|
| QRtub is asset management software | No — never stated; "asset" is used only as the customer's own domain word for equipment (explicitly sanctioned by CLAUDE.md's industry-page guidance), never as a claim about what QRtub *is* |
| QRtub replaces SafetyCulture / maintenance systems / any software | No — every mention frames Mitti/CMMS as something the code "links to" or "opens," never "replaces" |
| QRtub provides inspection capabilities | No |
| QRtub tracks maintenance history | No — "Maintenance logs in your CMMS platform" (L27) correctly attributes the tracking to the external CMMS |
| QRtub is a payment platform | No — Payment Destinations aren't mentioned at all |
| QRtub has an API | No |
| QRtub provides analytics dashboards | No — "Utilisation tracking" and "Cost allocation" (L68–69) are listed as what *external* fleet-management tools provide, under "Site managers see," not attributed to QRtub itself (though see §4 for the chunk-isolation risk this creates) |
| QRtub supports cross-account transfer | No |
| QRtub has granular permissions/sharing between orgs | No |
| Full Media inventory management is available | No — page never uses "Media" as a tracked entity; it only talks about physical stickers/plaques in generic terms |
| Media Batch management is available | No |
| Media Templates are available | No |

**Verdict: clean.** This page does not make any of the twelve specifically-flagged false claims.
That distinguishes it from at least one sibling page in this repo's own editorial audit
(`help__key-concepts.md`, which found active Media-tracking overclaims). The defects on this
page are of a different, subtler kind — see §4.

## 3. TERMINOLOGY CONSISTENCY

Checked against `/workspace/qrtub/GLOSSARY.md` and the two integration pages this page draws
its named tools from.

- **Product-name and domain casing**: "QRtub" and "qrtub.com" are used correctly throughout
  (checked every occurrence at L2, L8, L20, L106, L125, L181) — no `QRTub`/`QR Tub` typos, no
  uppercase domain.
- **No deprecated "Profile Page"** — the term never appears; "Pages" is used correctly as the
  generic capitalised entity (L134, L173, L175).
- **"Asset" vs "Item"** — GLOSSARY.md §"What NOT to Call Things" maps `Asset → Item` for
  QRtub's own data model, but CLAUDE.md's industry-page guidance explicitly allows "a
  council's 'assets'" as legitimate customer-domain vocabulary. This page's uses ("One Code
  Per Asset," L24; "the right asset in your maintenance system," L110) are all customer-domain
  usage, not a claim about QRtub's own entity model — **not a violation**.
- **Mitti naming is inconsistent within this single page.** L26 calls it "Mitti (formerly
  SafetyCulture)"; L105, forty lines later, calls the same product "Mitti (iAuditor)" with no
  mention of SafetyCulture at all. The canonical source, `integrations/safetyculture.mdx`,
  titles it "Mitti (formerly SafetyCulture / iAuditor)" — the full three-name chain, every
  time. Splitting the qualifier across two different partial forms in one page is a minor but
  real citability defect: a chunked retrieval that surfaces only the L105 sentence ("Mitti
  (iAuditor) - Pre-start inspections...") gives an agent no path back to the far more
  commonly-searched name "SafetyCulture," since that word never appears near it. Recommend
  using the full "Mitti (formerly SafetyCulture)" form consistently, matching the L26 usage.

## 4. AI-RETRIEVAL CITABILITY RISK

The core question for this bar: if a support bot or search agent retrieves a chunk of this
page **in isolation**, without the rest of the article's framing, does it produce a false or
misleading answer? Three findings, ranked by severity.

### 4.1 "Different Information for Different People" implies capability the app doesn't have (most severe)

The section (L51–71) reads:

> "**Operators see:** Pre-start inspection checklist / Daily check-in procedures / Emergency contacts / Operating instructions
>
> **Maintenance teams see:** Service history and logs / Maintenance schedules / Parts documentation / Technical specifications
>
> **Site managers see:** Compliance documentation / Certification status / Utilisation tracking / Cost allocation
>
> Same QR code. Relevant information for each role."

This phrasing describes automatic, role-differentiated content delivery — as if scanning the
same code as "an operator" surfaces different information than scanning it as "a site
manager." Checked against the actual mechanism (`../qrtub/src/lib/page/bindings.ts`,
`../qrtub/src/lib/page/context.ts`, and CLAUDE.md's own Conditional Visibility section):
conditional visibility (CEL) can branch on Item fields, Tub fields, device fields, and time
fields — **and** (confirmed in `bindings.ts`, not documented in CLAUDE.md's own conditional
visibility section) a `session.user` binding exists for a specific logged-in QRtub team
member's ID. None of these constitute a "role" abstraction. There is no way for QRtub to know
that the person currently scanning is "an operator" versus "a site manager" without either (a)
the visitor self-selecting from a shared list of Destinations, or (b) someone hand-writing a
CEL condition against one specific user's account ID — which does not scale to an unbounded
population of operators.

The honest version of this exact feature is stated correctly by three of this page's own
siblings. `contract-cleaning.mdx` (L27): *"Everyone who scans sees the SAME branded Page with
ALL the buttons. Each person taps what's relevant to them."* `local-government-councils.mdx`
(L33) uses identical framing. Those pages are explicit that this is self-selection from one
shared list, not targeted delivery.

**Why this matters for chunked retrieval specifically:** a support bot asked "does QRtub show
operators and managers different information automatically?" that retrieves only this H3 in
isolation has strong textual grounds to answer "yes" — which is false. Retrieved alongside the
rest of the page it is more likely (though not certain, since nothing later in the page
explicitly corrects the framing) to be read as a "different people tap different buttons"
story. The fact that this page is the *one* sibling in the group that doesn't state the
self-selection mechanism plainly is the defect — not that the underlying feature (multi-
Destination Pages) is unavailable.

**Recommended fix:** rewrite to match the accurate framing already used by the sibling pages
— e.g., "Every code opens one Page listing everything: pre-start checklist, service logs,
compliance status. Operators tap what they need; maintenance and management do the same" —
and drop the "X see: / Y see:" heading structure, which is what invites the role-targeting
misread.

### 4.2 "Utilisation tracking" / "Cost allocation" bullets are one chunk-boundary away from a false capability claim

Still inside §4.1's section, the "Site managers see:" bullet list includes "Utilisation
tracking" and "Cost allocation" with no qualifier that these live in external fleet-management
software. The qualifying context — "QRtub connects your physical equipment to the digital
tools you already use" — appears in a different H2 ("Integration with Construction Software,"
L100) roughly 50 lines later, under its own "Fleet Management" H3 (L117–120), which *does*
correctly attribute "Utilisation tracking systems" and "Cost allocation platforms" to external
tools. A retrieval system that returns only the L51–71 chunk for a query like "does QRtub do
cost allocation for equipment" has no attribution info in front of it, and BRAND.md §1.3 is
explicit that QRtub "IS NOT" asset management software. Recommend adding the words "in your
fleet system" or similar directly inside the L51–71 bullets themselves, rather than relying on
a separate section to carry the qualifier — the qualifier needs to travel with the claim, not
sit in a heading 50 lines away.

### 4.3 "Real-World Construction Use Cases" heading over-promises for unattributed hypothetical scenarios

Section (L73–98) is titled "Real-World Construction Use Cases" and opens each subsection with
specific, concrete numbers presented as fact: "A civil contractor manages 150+ pieces of heavy
machinery..." (L76), "You order 200 equipment QR codes for a new project. Machinery arrives
over 3 months..." (L40). These are unattributed (no named customer, no quote), so they don't
violate BRAND.md §3.2's literal rule against inventing "customer quotes or case studies" — but
the heading itself asserts these are "real-world," when they read as illustrative composites,
not documented customer stories. An AI agent that quotes "QRtub has a civil contractor customer
managing 150+ pieces of machinery across multiple sites" as a verified fact, sourced to a
heading that says "Real-World," would be overstating what the page actually supports.
Recommend renaming to something like "Example Scenarios" or "How This Plays Out," which invites
the same content without implying documented provenance.

## 5. REDUNDANCY WITH SIBLING INDUSTRY PAGES

Per `audit/01-external.md` (§1, §2), all five `/industries/*` pages — including this one —
share a byte-identical `## Ready to Deploy?` + `**Core features available:**` template
skeleton, with only the bullet content varying. This page contributes 7,746 MD bytes (~1,933
estimated tokens) to the 19-page `llms-full.txt` corpus (`audit/01-external.md` §1).

Beyond that confirmed byte-identical skeleton, this page's core explanatory sections — "One
Code Per Asset" (UC-001), "Print Before Deployment" (UC-004), "Update Without Reprinting"
(UC-003) — restate, in different words but the same substance, exactly the use-case library
entries CLAUDE.md documents once for reuse across every industry page. `contract-cleaning.mdx`
restates the identical three points ("Bulk Deployment Across Facilities," "Real-World Example:
Public Restroom QR Code" mirrors "Update without reprinting"). `local-government-councils.mdx`
does the same. None of the three industry pages point back to a single canonical explanation
of *how* the underlying mechanism works (that lives in `help/print-first-workflow.mdx` and
`help/key-concepts.mdx`); each re-derives it independently for its own vertical.

For a retrieval system, this means a generic question like "can I switch software without
reprinting my QR codes" can surface three-to-five near-duplicate industry-flavored answers,
none more authoritative than another, none linking to the dedicated `help/*` page that actually
owns the mechanism in depth. This is a corpus-dilution problem more than a per-page defect —
noted here because it directly bears on §6's llms.txt recommendation.

Distinguishing note: unlike `contract-cleaning.mdx` and `local-government-councils.mdx`, this
page does **not** use the "public scans the code and treats visible operational buttons as a
marketing signal" narrative device (their "Note on security" / "Restaurant Kitchen Effect" /
"Professional Council Effect" framing). That specific redundancy is confined to those two
pages and does not extend to this one — worth noting so it isn't over-generalized.

## 6. SHOULD THIS PAGE BE IN LLMS.TXT?

**Verdict: yes, keep it in — but only once §4.1 and §4.2 are fixed. As currently worded, the
page should not stay in an index an AI agent treats as ground truth.**

Reasoning:

- The premise that a technical-docs `llms.txt`/`llms-full.txt` should exclude marketing content
  *by genre* doesn't hold up on its own — per `audit/01-external.md`, `llms-full.txt` is
  "97.89% unique, substantive content," and industry-fit questions ("does this work for
  construction equipment," "what does this connect to for civil contractors") are real
  questions a prospect or support bot legitimately asks that no `/help/*` page answers. Genre
  alone isn't disqualifying; **accuracy is the correct bar**, which matches how this audit was
  scoped.
- Measured against that bar, this page is mostly clean: §1 and §2 above find no BRAND.md
  "FALSE" claims, no Planned feature presented as available, and every "Available" feature
  claimed in the closing checklist is genuinely available. That is a materially better result
  than at least one `/help/*` page already audited in this project (`help__key-concepts.md`,
  which found an active Media-tracking overclaim).
- However, §4.1 is a real, specific defect: the "Different Information for Different People"
  section describes automatic role-based targeting that the app does not do, in a page whose
  own sibling pages get the identical underlying feature right. A support bot citing this
  section alone would give a wrong answer about a core mechanism (Pages/Destinations) that
  this same corpus documents correctly elsewhere. That inconsistency — right answer in three
  sibling pages, wrong framing in this one — is exactly the kind of drift an `llms.txt`
  corpus should not be shipping.
- §4.2's un-anchored "utilisation tracking / cost allocation" bullets compound the same risk:
  they are one chunk-boundary away from contradicting BRAND.md's explicit "QRtub IS NOT asset
  management software" line.

**Concrete recommendation:** fix §4.1 (rewrite to the self-selection framing already used
correctly by `contract-cleaning.mdx` and `local-government-councils.mdx`) and §4.2 (move the
"in your fleet system" qualifier into the bullet list itself) before this page's next
regeneration into `llms-full.txt`. Once fixed, there is no accuracy-based reason to exclude it.
The redundancy noted in §5 is a lower-priority, corpus-wide cleanup (deduplicating the
"Ready to Deploy?" skeleton and pointing industry pages back to the canonical `help/*`
explanations) that applies to all five industry pages equally, not a reason to single out this
one for removal.

---

## Summary of findings, ranked by severity

1. **(Moderate-high, citability)** "Different Information for Different People" (L51–71)
   implies role-based automatic content targeting the app does not have; the honest
   self-selection framing is stated correctly by two sibling pages this page fails to match.
2. **(Moderate, citability)** "Utilisation tracking" / "Cost allocation" bullets (L68–69) carry
   no in-line qualifier that these are external-tool destinations, one chunk-boundary away from
   contradicting BRAND.md's "QRtub is NOT asset management software."
3. **(Minor, terminology)** "Mitti (formerly SafetyCulture)" (L26) vs. "Mitti (iAuditor)"
   (L105) — same product, two different partial names within one page.
4. **(Minor, wording)** "Weatherproof codes lasting years" (L143) attributes a physical-media
   durability property to the software platform rather than to customer-sourced Media.
5. **(Minor, framing)** "Real-World Construction Use Cases" (L73) heading asserts documented
   provenance for what read as unattributed illustrative scenarios.
6. **(Corpus-level, not page-specific)** Structural redundancy with sibling industry pages —
   confirmed byte-identical "Ready to Deploy?" skeleton (`audit/01-external.md`) plus
   independently-restated explanations of the same three use-cases (UC-001/003/004) across
   at least three of the five industry pages.

No BRAND.md §1.6 "Claims That Are FALSE" violation was found on this page (§2) — this page's
problems are about *how* true claims are framed for isolated, chunked consumption, not about
stating untrue ones.
