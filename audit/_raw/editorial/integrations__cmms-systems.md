# Editorial audit — CMMS Systems Integration

- File: `/workspace/mintlify-docs/integrations/cmms-systems.mdx`
- Live: https://help.qrtub.com/integrations/cmms-systems
- Nav group: Integrations → Operations (sibling: `integrations/safetyculture.mdx`)
- Verified against: `/workspace/qrtub/src/lib/page/bindings.ts`, `/workspace/qrtub/src/lib/page/destination-resolver.ts`, `/workspace/qrtub/src/lib/types/destination-config.ts`, `/workspace/qrtub/src/components/blocks/DestinationPreview/DestinationPreview.tsx`, and sibling docs `help/app-links.mdx`, `help/pages-overview.mdx`, `help/key-concepts.mdx`, `integrations/safetyculture.mdx`

---

## 1. Self-containment

A cold reader cannot complete this page's implied task ("connect QRtub to my CMMS") using only this page. Specific missing pieces:

- **No UI procedure at all.** The page never says *where* in the QRtub app to enter these URLs. Compare the sibling page, which is explicit: "In your Item's Page, add a new Destination: **Destination Name:** Start Inspection, **Destination URL:** `iauditor://...`" (`integrations/safetyculture.mdx`, "Basic Setup: Start Inspection"). This page's "Example Setup: UpKeep" section jumps straight to three bare URL blocks under a "Create Page Destinations" H3 with zero instruction on how to create one.
- **Page Mode is never mentioned as a prerequisite.** The "Multi-System Page" section and the whole premise of multiple Destinations depend on the Tub having its page option turned on (`help/pages-overview.mdx`: "Pages are switched on per Tub, not per Link... In the Tub's settings, turn on the page option"). A reader landing cold on this CMMS page, follows the "Multi-System Page" example, and gets nothing, because their Tub is still in Direct Mode — the page gives no indication this setting exists.
- **No mention of the app-link fallback mechanism.** "Option 1: Direct Deep Links" presents `yourCMMS://asset/[ASSET_ID]` with no warning about what happens if the app isn't installed. `help/app-links.mdx` documents QRtub's automatic 2.5-second fallback timer and the Fallback URL/Fallback Message configuration — none of that is referenced here, even though Option 1 is exactly the deep-link scenario that mechanism exists for. A reader who wires up a CMMS deep link per this page, without reading `app-links.mdx` (not linked from this page at all), will ship links that show a blank screen for anyone without the CMMS app installed.
- **No URL-encoding caveat**, despite the whole page being built around inserting Item field values into URLs. `help/key-concepts.mdx` and `integrations/safetyculture.mdx` both explicitly warn: "QRtub inserts field values exactly as stored and never URL-encodes them. A value containing a space, `&` or `#` will break the deep link." The CMMS page's own examples (`PUMP-042`, `{{item.assetID}}`) invite exactly this failure mode with zero warning.
- **Inconsistent field name between sections** — see §6 below. A reader can't tell whether the field should be called `cmmsAssetID` or `assetID`.
- **Resources section doesn't link `help/app-links`** even though deep links are Option 1 of this very page — a genuine gap versus the equivalent Mitti page, which links it prominently.

## 2. Answer-first

Quoting the actual opening text of every H2 (and the H3s where the H2 has no lead-in of its own):

- **## Overview** — opens: *"CMMS platforms track maintenance schedules, work orders, and asset history. Integrating with QRtub enables:"* then a bullet list. This is background-first, not answer-first — it defines what a CMMS is generically before saying what *this integration* does, and never states in one place what QRtub's role is (build a URL; no data exchange). Partial credit: the bullets are concrete, but there's no single 40–60 word direct answer.
- **## Integration Approaches** — has **no text at all** before its first H3. The H2 chunk, if retrieved alone, starts with `### Option 1: Direct Deep Links (If Supported)` and its lead sentence *"Some CMMS platforms provide deep link URLs that open the mobile app:"* — reasonable as an H3 opener, but the H2 itself never states the actual answer ("there are three ways to connect: deep links, web URLs, or URL Templates").
- **## Example Setup: UpKeep** — no H2-level lead sentence; falls straight into `### Create Page Destinations` and three bare code blocks with bold labels. No sentence tells the reader what field UpKeep expects, what `assetID` must contain, or that this is illustrative rather than a verified UpKeep API contract.
- **## Multi-System Page** — opens: *"Combine CMMS with other systems:"* — a four-word fragment, not a sentence, let alone a 40–60 word answer. It gives no mechanism (that this needs Page Mode, that each numbered item is a separate Destination).
- **## Common CMMS Platforms** — opens directly with the table, no lead sentence. The one piece of framing text — *"**Note:** Deep link availability changes. Check your CMMS documentation or contact support."* — appears **after** the table, so a reader (or an AI agent) skimming just the table sees unqualified ✅/❌ claims before the caveat that they may be wrong.
- **## Best Practices** — opens straight into `**Field Mapping**` bold-lead-in, no H2 sentence framing what "best practices" means here or why these three matter.
- **## Troubleshooting** — opens directly with `**"Page not found" errors:**` and a bullet list — no framing sentence describing the two failure modes it covers before listing them.
- **## Resources** — a bare link list, fine as-is; this heading doesn't imply a question that needs a direct-answer opener.

Verdict: **zero of the seven content H2s** open with a direct 40–60-word answer to their heading's implied question. Every one leads with either a generic-definition sentence, a sentence fragment, or no lead sentence before subheadings/lists/tables.

## 3. One question per page

This page is answering at least four distinct questions, bundled:

1. **"How do I connect QRtub to a generic/unknown CMMS?"** — Integration Approaches (deep link / web URL / URL Template), the real, vendor-agnostic core of the page.
2. **"How do I set this up for UpKeep specifically?"** — Example Setup: UpKeep. This is a single-vendor deep-dive sitting inside a page whose title and framing are vendor-agnostic ("CMMS Systems Integration," "Common CMMS Platforms" table lists five vendors). Nothing explains why UpKeep gets a worked example and Fiix/eMaint/Hippo/Maintenance Connection don't.
3. **"How do I combine CMMS with other systems on one code?"** — Multi-System Page. This duplicates content that already lives on `help/key-concepts.mdx` (UC-001, the "Excavator" example with "Start Inspection → Mitti / Log Maintenance → CMMS") almost verbatim, and on `integrations/safetyculture.mdx` ("Multi-Audience Routing" under Use Cases). It adds no CMMS-specific information.
4. **"Which CMMS platforms are known to work, and how do I test/troubleshoot a rollout?"** — Common CMMS Platforms table, Best Practices, Troubleshooting.

**Proposed split:**

- **Keep on this page** (rename conceptually to "Connecting QRtub to a CMMS"): the three integration approaches, the field-mapping/testing best practices, and troubleshooting — these are all one coherent task ("wire up my CMMS, whichever one it is, and get it working reliably").
- **Cut "Multi-System Page" entirely** and replace with a one-line pointer: "To show a CMMS Destination alongside other systems (inspections, manuals) on one code, see [Pages Overview](/help/pages-overview) and [Key Concepts](/help/key-concepts)." This section is pure duplication today and adds retrieval noise — an AI agent asked "how do I combine CMMS with SafetyCulture" would get a thinner, staler answer from this page than from key-concepts.mdx.
- **Cut "Example Setup: UpKeep"** as a dedicated H2, or fold its three URL patterns into the Option 3 explanation as one labeled example ("e.g., for UpKeep: ..."), clearly hedged as illustrative and not vendor-verified. A single-vendor deep dive without an equivalent for the other four vendors in the platform table reads as incomplete rather than as a deliberate choice.
- If UpKeep genuinely deserves its own worked example (parity with the Mitti/SafetyCulture page), it should be a **separate page** (`integrations/cmms-upkeep.mdx` or similar), following the same shape as `integrations/safetyculture.mdx` — named product, verified deep link scheme, its own troubleshooting. As written, it's too thin (3 URLs, no confirmation these are real UpKeep endpoints) to justify a standalone page today, and too vendor-specific to sit inside a vendor-agnostic guide.

This page is not too thin to stand alone once the above is trimmed — the three-approaches + best-practices + troubleshooting core is a legitimate, complete single-question chunk.

## 4. Headings as questions

| Current heading | Keep or rewrite? | Proposed question form |
|---|---|---|
| Overview | Keep | — (not a question-shaped section; fine as a noun heading) |
| Integration Approaches | Rewrite | **"How do I connect QRtub to my CMMS?"** — the section answers exactly this, and the noun phrase gives no hint that three concrete options follow |
| Option 1: Direct Deep Links (If Supported) | Keep | Already effectively a labeled option, not a question |
| Option 2: Web-Based Access | Keep | Same |
| Option 3: URL Templates | Keep | Same |
| Example Setup: UpKeep | Rewrite (if kept) | **"What does a UpKeep setup look like?"** — minor clarity gain, and signals "example," not "instructions" |
| Multi-System Page | Rewrite/cut | If kept as a pointer only: **"Can I show CMMS alongside other systems on one code?"** |
| Common CMMS Platforms | Rewrite | **"Which CMMS platforms support deep links?"** — the table is answering a support question ("does my CMMS work with this?"), and the current noun phrase reads as a marketing list rather than a direct capability answer |
| Best Practices | Rewrite | **"How do I test a CMMS rollout before deploying it at scale?"** — this is what the section actually contains (field mapping + a pre-deployment test checklist), and "Best Practices" is generic enough to be meaningless to a retrieval system matching a specific question |
| Troubleshooting | Keep | Already implicitly a question-answering heading; "Troubleshooting" is a well-understood convention, converting it to "What do I do if the CMMS link doesn't work?" would be a lateral move, not a clarity gain |
| Resources | Keep | Not a question-shaped section |

## 5. Edge cases / limits / failure modes

Treating every gap below as a defect per the audit brief — these are exactly the questions an AI support agent will be asked and, absent a stated answer, will guess at:

- **What happens if the mapped field is empty on some Items?** Not stated anywhere on this page. Verified in source (`/workspace/qrtub/src/lib/page/bindings.ts` `resolveBindingsForUrl`, and `/workspace/qrtub/src/lib/page/destination-resolver.ts` `resolveDestination`): a binding that evaluates to `undefined`, `null`, or `''` is recorded as **unresolved**, and the destination-resolution layer **discards that Destination/rule entirely** rather than inserting an empty string into the URL — for a plain (non-conditional) Destination, an empty `defaultLink` binding means `resolveDestination` returns no destination at all, which (`/workspace/qrtub/src/lib/page/public-link-resolution.tsx`) surfaces to the visitor as a **"link not ready" page**, not a broken CMMS URL. This is the single most support-relevant fact missing from the page: if `cmmsAssetID` is blank for some Items, those QR codes will show "link not ready," not open a malformed UpKeep URL. Worth noting the QRtub editor also surfaces this before deployment — `DestinationPreview.tsx` renders `bindingErrors` in the Destination preview panel — so the page could point readers to check the preview before printing.
- **No URL-encoding statement** (see §1) — a field value containing a space, `&`, or `#` breaks the generated CMMS URL silently. Sibling pages state this explicitly; this page doesn't, despite depending entirely on inserting field values into URLs.
- **No app-link fallback behavior stated** for Option 1 deep links — no mention of the 2.5-second timer, Fallback URL, or Fallback Message documented in `help/app-links.mdx`. A reader following only this page will ship a deep-link Destination with no fallback and no idea that's even configurable.
- **No plan-tier gating mentioned** — checked `/workspace/qrtub/BRAND.md`'s feature-status table for anything gating Destinations, URL Templates, or Page Mode by plan; found none. This appears to be a non-issue for this page (no tier gap exists to document), but if plan tiers do gate Page Mode or the number of Destinations, that isn't verifiable from the repo made available and should be confirmed with product before publishing, since its absence here currently reads as "available on every plan."
- **No character/length limit stated** for generated URLs. The Mitti/SafetyCulture page states "Very long deep links may not work consistently" and "keep it under ~2000 characters" in its troubleshooting section — this page's Troubleshooting section has no equivalent line, despite CMMS work-order URLs plausibly carrying several concatenated parameters (see the "Create Work Order" example, which already chains a query string).
- **Vendor deep-link support claims are asserted without a source or date.** The "Common CMMS Platforms" table states specific ✅/❌ facts about UpKeep, Fiix, Maintenance Connection, Hippo CMMS, and eMaint's deep-link support. These are claims about third-party products QRtub doesn't control and this repo can't verify either way; the only hedge is the single trailing note *after* the table ("Deep link availability changes. Check your CMMS documentation or contact support"). Given the docs style guide's own instruction to "avoid dated claims that quietly expire," a table of unlinked, unsourced vendor capability claims is a durability risk independent of QRtub's own product — worth downgrading the checkmarks to "check vendor docs" framing throughout, not just in a footnote.
- **No statement of what "Test URLs before mass deployment" (Best Practices) actually means procedurally** — no mention of the Destination preview panel (which does exist and does show binding errors, per `DestinationPreview.tsx`) as the concrete tool for this "best practice." The advice is stated with no way to act on it from the page.

## 6. Chunk integrity

Evaluating each H2 in isolation, as if only that section were retrieved:

- **Overview** — Self-contained. Fine alone.
- **Integration Approaches** — Self-contained (contains all three options with their own examples). No dangling references.
- **Example Setup: UpKeep** — **Breaks in isolation.** Uses `{{item.assetID}}` in all three code blocks, but the earlier "Option 3: URL Templates" section (a different H2 subsection) established the field name as `{{item.cmmsAssetID}}` ("Item field `cmmsAssetID`: 'PUMP-042'"). A reader (or AI agent) retrieving only "Example Setup: UpKeep" has no idea `assetID` is a fictional/example field name unrelated to the `cmmsAssetID` field named two sections earlier — and if both chunks are retrieved together, they contradict each other on what the field should be called. This is a genuine, fixable inconsistency, not just a completeness gap.
- **Multi-System Page** — Self-contained in the sense that it doesn't grammatically depend on prior text, but its content ("Combine CMMS with other systems:" + a 4-item list + "One QR code, all systems accessible") is too terse to be useful in isolation — it doesn't explain that this requires Page Mode (a Tub-level setting explained only in `help/pages-overview.mdx`, not linked from this section) or that each numbered line is a separate Destination the reader must configure individually.
- **Common CMMS Platforms** — Self-contained as a table, but isolated retrieval loses the caveat note if a chunker splits at the row boundary vs. keeping the trailing "**Note:**" line attached — a real risk since the note is a separate paragraph after the table rather than a table footnote or inline caveat.
- **Best Practices** — Self-contained but assumes the reader already knows what "Field Mapping," "your QRtub Item fields," and "your CMMS asset identifiers" mean in QRtub's data model (Item, Tub, custom fields) — reasonable for a reader of the whole site, but this section doesn't reintroduce "Item field" and would confuse someone who lands directly on this chunk via search without ever having seen `help/key-concepts.mdx`.
- **Troubleshooting** — Self-contained; the two failure categories and their bullets read fine with no external dependency. This is the best-isolated section on the page.
- **Resources** — Self-contained (a link list), though "Contact your CMMS vendor for deep link documentation" only makes sense with the rest of the page's context that deep-link support is CMMS-vendor-dependent — negligible risk since Resources sections aren't usually retrieved as an answer chunk on their own.

---

## Summary

The page's core content (three integration approaches, platform table, best practices, troubleshooting) is sound and largely non-redundant with its sibling. But it fails self-containment (no in-app procedure, no Page Mode prerequisite, no fallback mechanism), fails answer-first structure on every H2, bundles at least one clearly separable, duplicative section (Multi-System Page) and one underdeveloped vendor-specific detour (Example Setup: UpKeep), and is silent on the single most consequential edge case for this exact feature — what a scan shows when the mapped field is empty (a "link not ready" page, not a broken URL, per verified source behavior). A substantive rewrite is warranted; see `/workspace/mintlify-docs/audit/proposed/integrations__cmms-systems.md`.
