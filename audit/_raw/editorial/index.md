# Editorial Audit — `index.mdx` (Homepage)

**File:** `/workspace/mintlify-docs/index.mdx`
**Live:** https://help.qrtub.com/index
**Nav placement:** `docs.json` — Help tab, top-level page (`"pages": ["index"]`), sitting above every group (Getting Started, Concepts, Items & Data, Pages, Printing). It is the only page in the Help tab with no group, i.e. the tab's landing page.

**Siblings skimmed for overlap:** `help/creating-your-first-link.mdx`, `help/key-concepts.mdx`, `industries/civil-construction.mdx`, `integrations/safetyculture.mdx`.

---

## 1. SELF-CONTAINMENT

A cold reader landing on this page, unable to follow any link, **cannot complete the implied task** ("start using QRtub") because:

- **No path to the actual first action.** The page never links to `/help/creating-your-first-link` — the page that literally contains Step 1–4 for generating a Link. The homepage's own "Get Started" section (line 26) lists features but gives no instructions and no link to the how-to page. A reader stuck on this chunk alone has no next click that leads to actually doing anything in the product.
- **No signup or login URL.** The page mentions "Deploy QR codes with professional management from day one" but the only outbound link is `https://qrtub.com/pricing`. There's no `app.qrtub.com/login` or signup URL in the body content itself (those only exist in `docs.json` navbar, which isn't part of the Markdown chunk an AI agent would retrieve).
- **Core vocabulary is used but never defined.** "Page" appears once, uncapitalized-context-dependent ("One QR code with a **Page** serves them all," line 18) with no definition. "Destination" — the term for where a Page's buttons route to — is never used at all on this page, even though the whole value proposition ("update Destinations whenever requirements change," conceptually) depends on it. A reader (or an AI agent synthesizing an answer from just this page) has no way to know what a Link, Page, Item, Tub, or Destination actually is — those definitions live only in `help/key-concepts.mdx`, which this page never links to.
- **No plan/pricing specifics.** "See plans and pricing →" is a bare link with zero inline numbers. The actual tiers (`Starter`/`Professional`/`Scale`) and their ceilings (see §5) are invisible to a reader who can't follow the link.

**Concrete missing pieces:** a link to `/help/creating-your-first-link`, a one-line definition of Link/Page/Destination (or a link to `/help/key-concepts`), and at least the plan-tier ceiling numbers or a pointer to them.

---

## 2. ANSWER-FIRST

Every H2's opening sentence(s), quoted verbatim, judged against the question implied by its heading:

### H2: "Why Choose QRtub?"
No paragraph directly under the H2 — it drops straight into an H3 ("Print Before You're Ready"). Judging by that first H3's opening sentence:
> "Professional printing requires bulk orders and a lead time. Don't wait until everything is finalised."
This is **pain/scene-setting, not an answer**. The actual answer — "Generate Links, print QR codes, and connect them when convenient" — is the *third* sentence. A retrieval system truncating to the first sentence gets the problem statement, not the capability.

### H3: "One Code, Multiple Systems"
> "Normally QR Codes only do one thing for one audience. One QRtub code can do the job of many."
Same pattern: opens with the competitor/default-behavior framing before the QRtub-specific answer. Two sentences of setup before the point lands.

### H3: "Future Proof"
> "Switch software, add new systems, fix broken links—all without touching your physical QR codes."
This one **is** answer-first — it states the capability immediately, no preamble. Good example on the page.

### H3: "Built for Physical Deployments"
> "Marine operations, construction equipment, rental fleets, lifesaving stations — QRtub handles real-world deployments where QR codes need to last months or years."
Answer-first — leads with the concrete industries, then the claim. Good.

### H2: "Get Started"
> "Core features available today:" (immediately followed by a bullet list, no sentence answer at all)
This is a **label, not an answer**. The heading implies "how do I get started?" but the section is a feature inventory with no steps and no first action. It answers "what exists" not "how do I begin."

### H2: "Find out more"
No text at all — the H2 is followed immediately by two `<Card>` components with one-line descriptions inside them ("How your industry uses QRtub." / "How to use QRtub with your favourite tools."). There is no sentence-level answer under the H2 itself; the section *is* the two card blurbs. In isolation (see §6) this section conveys almost nothing.

**Summary:** 2 of 6 headings (Future Proof, Built for Physical Deployments) are genuinely answer-first. The rest open with preamble, an unanswered label, or nothing at all.

---

## 3. ONE QUESTION PER PAGE

This page mixes three distinct jobs in one chunk:
1. **Marketing pitch** ("Why Choose QRtub?") — why the product exists, competitive framing.
2. **Feature inventory** ("Get Started") — a bullet list of what's available, with no procedure.
3. **Navigation hub** ("Find out more") — two cards pointing to Industries and Integrations tabs.

For a homepage, mixing pitch + feature list + hub navigation is the expected shape (it's the tab's landing page, not a task page), so **no structural split is needed** — this isn't the same failure mode as a how-to page answering two unrelated procedures.

The real problem is **redundancy with sibling pages**, not multiple questions on one page:
- "Print Before You're Ready" (index.mdx) restates, almost beat-for-beat, `help/key-concepts.mdx`'s "Print-Before-Link Workflow" section (*"You don't have all your Item details finalised yet... Generate Links... Professional printing... Field deployment"*) and `industries/civil-construction.mdx`'s "Print Before Deployment" section (*"Professional weatherproof QR codes require bulk printing and lead time..."*).
- "Future Proof" (index.mdx) is a shorter restatement of `key-concepts.mdx`'s "Update Without Reprinting" section and `civil-construction.mdx`'s "Update Without Reprinting" section — three pages now carry the same "switch vendors, don't reprint" pitch in near-identical language.

**Recommendation:** not a split, but a **trim**. The homepage's "Why Choose QRtub?" H3s should stay short (one sentence each, as a teaser) and defer full explanation to `/help/key-concepts`, rather than re-arguing the case at similar length. Currently the homepage argument is *nearly as long* as the concept page's argument, which means an AI agent asked "why print before linking?" has three near-duplicate sources to reconcile instead of one canonical one.

**Is the page too thin to stand alone?** No — as the tab landing page it earns its place structurally. But its actual informational payload (once you strip preamble and duplication) is thin relative to its heading count; see the proposed rewrite for a denser version.

---

## 4. HEADINGS AS QUESTIONS

| Current heading | Genuinely clearer as a question? | Proposed rewrite |
|---|---|---|
| "Why Choose QRtub?" | Already a question — fine as-is. | — |
| "Print Before You're Ready" (H3) | Yes — this is exactly the FAQ a prospect searches for. | "Can I print QR codes before my systems are ready?" |
| "One Code, Multiple Systems" (H3) | Yes — matches a real query shape. | "Can one QR code link to multiple systems?" |
| "Future Proof" (H3) | Yes — "Future Proof" is vague marketing-speak; the question form is more specific and matches what someone actually searches. | "What happens if I switch vendors or systems later?" |
| "Built for Physical Deployments" (H3) | Marginal — it's a positioning statement more than an answerable question. Leave as a noun phrase, but consider: | "Which industries is QRtub built for?" (optional, weaker gain than the others) |
| "Get Started" | No — "Get Started" is a standard, well-understood section label; converting it to a question ("How do I get started?") doesn't add clarity, and per house style (matches pattern in `creating-your-first-link.mdx`, `key-concepts.mdx`) numbered "Step N" headings are the convention for procedures, not this summary list. | — |
| "Find out more" | No — this is a navigation label, not a question a reader is asking. | — |

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

This page states **zero** limits, ceilings, or plan-tier gating. Given the house rule in `CLAUDE.md` ("State limitations explicitly; silence reads as capability" and the audit brief's own framing — this is exactly where an AI support agent invents a wrong answer), these are defects:

- **No plan tiers or ceilings are named**, even though they exist and are concrete (`../qrtub/src/lib/stripe-plans.ts`):
  - Starter ($5/mo): up to 100 active links, 1 editor
  - Professional ($25/mo): up to 1,000 active links, 5 editors, 1 numbered sequence pattern
  - Scale ($90/mo): up to 10,000 active links, 20 editors, 5 numbered sequence patterns, private-or-public scanning
  An AI agent asked "how many QR codes can I deploy on the Starter plan?" has nothing on this page (or anywhere else in the skimmed set) to answer from, and would either say "unlimited" (wrong) or fabricate a number.
- **No statement of what's NOT available.** The bullet list ("Bulk Link generation," "Pages with multiple destinations," "Print-before-link workflow," "URL Templates for bulk deployment") is presented as the complete feature set with no "not yet" callout. Per `../qrtub/BRAND.md` §1.4, API access, cross-account transfer/sharing, granular permissions, and payment Destinations are Planned, not available. A support bot retrieving only this page has no signal that these don't exist yet, and nothing stops it from answering "yes" to "does QRtub have an API?"
- **No mention of what happens when a URL Template field is empty or contains special characters.** This is a real, documented failure mode (`key-concepts.mdx`: *"QRtub does not URL-encode them, so a field containing a space, `&`, `?` or `#` will produce a broken link"*) but the homepage's bullet "URL Templates for bulk deployment" gives no hint this footgun exists. Since the homepage doesn't link to `key-concepts.mdx` at all, a reader stopping here has no way to discover it.
- **No mention of Direct Mode vs. Page Mode.** "One QR code can do more, linking to multiple systems" (line 8) is true only in Page Mode — Direct Mode Links go to exactly one Destination. The page states the multi-destination benefit as if it's the default/only behavior, with no caveat that a Link must be in Page Mode and carry a Page for this to work.
- **No mention of `qrtub.com/r/...` vs custom/ID-based Link formats**, unlike `creating-your-first-link.mdx` which explains the three types. A reader can't tell from this page alone what a "Link" looks like as a URL.

---

## 6. CHUNK INTEGRITY

Testing each H2 (and its H3 children) as if retrieved in isolation, no surrounding page:

### Intro paragraphs (before any H2)
Reads fine standalone as a value-prop paragraph, though it assumes the reader already knows what a "Destination" is conceptually ("Update Destinations whenever requirements change") without ever using that word — it says "Connect them during installation... Update Destinations whenever requirements change" (line 8), which is the *only* place anything Destination-like is implied, and even there the word "Destinations" is used **without ever having been introduced or defined** anywhere on this page. Isolated, a reader has no idea what a "Destination" is.

### H2: "Why Choose QRtub?"
- Depends on undefined terms: "Link" (never defined, only exemplified via "Generate Links"), "Page" (used once, undefined: *"One QR code with a Page serves them all"*, line 18).
- The four H3s are otherwise self-contained sentences that don't refer back to "the above example" or "this" — no anaphoric dependency problems within the section itself. This is a genuine strength: each H3 could be lifted individually without breaking grammatically.
- However, "Built for Physical Deployments" (*"QRtub handles real-world deployments where QR codes need to last months or years"*) implicitly assumes the reader already knows *why* longevity matters (durability of physical media, no reprinting) — that context lives in `key-concepts.mdx`'s "Media" section, not here.

### H2: "Get Started"
Standalone, the bullet list ("Bulk Link generation," "Pages with multiple destinations," "Print-before-link workflow," "URL Templates for bulk deployment") is a list of product-internal nouns with zero explanation. Someone who has never seen "URL Templates" or "Print-before-link workflow" gets a term, not an answer. This section's usefulness in isolation is close to zero for anyone but an existing user — it functions as a recap, not a stand-alone explainer.

### H2: "Find out more"
Worst case for chunk integrity. Retrieved in isolation, this section is:
```
<Columns cols={2}>
  <Card title="Industries" icon="building" href="/industries/civil-construction">
    How your industry uses QRtub.
  </Card>
  <Card title="Integrations" icon="plug" href="/integrations/safetyculture">
    How to use QRtub with your favourite tools.
  </Card>
</Columns>
```
No prose at all — if an AI agent chunks by H2 and returns just this section's text, the retrievable content is two six-word blurbs pointing at single example pages (civil-construction, safetyculture) that don't represent the full Industries/Integrations tabs (the nav has 5 industry pages and 2 integration pages; the cards only surface one each, so the section is also **under-representative of its own target tabs**, not just underspecified).

---

## Overall assessment

The page does not make any false capability claims (a real positive — every bullet under "Get Started" matches `BRAND.md`'s "Available" feature list and the app's actual behaviour). Its defects are structural: undefined vocabulary the rest of the docs rely on, no link to the actual first-action page, no plan/limit numbers, no "not yet available" signal, and one section ("Find out more") that carries no retrievable information on its own. These are exactly the failure modes the audit brief calls out, and they justify a rewrite — see `/workspace/mintlify-docs/audit/proposed/index.md`.
