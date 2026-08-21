---
title: "Pre-Publish Checklist"
description: "Reusable checklist for any new page on help.qrtub.com, run once before it merges. Sourced from the editorial audit (audit/03-editorial.md) and Mintlify's own published guidance (audit/_raw/mintlify-guidance.md)."
---

# Pre-publish checklist

Run this before any new `/help/*`, `/industries/*`, or `/integrations/*` page goes into
`docs.json`. It exists because the editorial audit found the same handful of failures
repeating across almost every page — this list catches them before publish instead of
after.

Not a restatement of `CLAUDE.md` — that file covers *how to write*. This is the last-mile
check on the finished file.

---

## Frontmatter

- [ ] `title` and `description` are both present (Mintlify auto-generates a title from the
      file path if missing, and silently omits the page from `llms.txt`'s description line
      if `description` is missing — don't rely on the fallback)
- [ ] `description` actually describes what the page delivers, not just its topic — it's
      truncated into `llms.txt` at 300 chars and is often the *only* thing an AI agent sees
      before deciding whether to fetch the full page
- [ ] `description` doesn't promise something the body doesn't deliver (a real finding: a
      page promised "how Destinations fit together" and never explained the fitting-together)
- [ ] No `sidebar_position`, `category`, or `slug` fields — not part of Mintlify's schema,
      confirmed absent from both `settings-reference` and `organize/pages`; navigation comes
      only from `docs.json`
- [ ] If the page should be excluded from AI/search context but stay reachable, use
      `noindex: true` explicitly rather than leaving it ambiguous

## One question per page

- [ ] The page answers one implied question, not two or three stitched together (a
      procedure + a reference table + a pitch is three pages wearing a trenchcoat)
- [ ] If it must cover a marketing pitch *and* a hub/nav role (only acceptable for tab
      landing pages like `index`), confirm it isn't re-arguing a case another page already
      makes at similar length — that's the "three near-duplicate sources for one AI query"
      failure the audit found repeatedly
- [ ] Check sibling pages for overlap before adding a new section — the audit found the same
      "print before you're ready" and "update without reprinting" pitches independently
      re-written on three separate pages

## Answer-first structure

- [ ] Every H2/H3's opening sentence *is* the answer, not scene-setting, competitor framing,
      or a label followed by a bullet list — a retrieval system that truncates to sentence
      one should get the actual point
- [ ] Headings are phrased as the question a user would type, where that's a real gain
      ("Can I print QR codes before my systems are ready?" beats "Print Before You're Ready")
      — but don't force it onto a plain navigational label like "Related" or "Get Started"
- [ ] Heading hierarchy is sequential — no jumping H2 straight to H4

## Edge cases, limits, and failure modes stated

- [ ] Every capability claim has its boundary stated next to it: what happens on empty
      input, a mistyped value, a missing field, a plan-tier ceiling — silence reads as
      capability, and this was the single most common defect across the audit
- [ ] Anything that fails *silently* is called out explicitly (e.g. an undefined identifier
      in a condition evaluates to `false` with no error — a reader who doesn't know this
      cannot debug it)
- [ ] Plan-tier numbers or ceilings are named (or linked) wherever a capability is described,
      not left for the reader to discover by hitting a paywall
- [ ] Anything Planned-but-not-built is stated as such, in this page's own words — don't
      assume a reader arriving at this page will have also read `BRAND.md`

## Chunk integrity (every H2 stands alone)

- [ ] Read each H2 (and its children) as if it were the *only* text an agent retrieved — no
      surrounding page, no earlier section. Does it still make sense?
- [ ] No section resolves a term ("Destination," "Link," "Page," "the above example") only in
      an earlier section — either define it locally in one clause or repeat the definition
- [ ] No section is a two-line label plus bare `<Card>`/link block with zero standalone
      prose — that content is invisible to a chunked retriever
- [ ] Bullet-fragment sections (patterns, examples) re-establish their own vocabulary inline
      rather than assuming the reader arrived via the section above

## Terminology consistency

- [ ] Every entity name matches `../qrtub/GLOSSARY.md` exactly: **Link**, **Page**,
      **Destination**, **Item**, **Tub** — capitalized as the glossary capitalizes them
- [ ] No "Asset" where the glossary says Item, no "Access Link," no "Profile Page" — check
      the glossary's "What NOT to Call Things" section directly, don't rely on memory
- [ ] Industry pages may use the customer's own domain word (a council's "asset," an
      electrician's "test and tag") for *their* concepts — but QRtub's own concepts still use
      QRtub's terms. Don't let the industry noun leak onto a QRtub entity.
- [ ] One name per concept for the whole page — don't alternate between two names for the
      same thing (Mintlify's own GEO guidance calls this out explicitly)
- [ ] Concrete UI strings (button labels, nav labels, section names) are copy-checked against
      the current app source, not remembered from a previous version — the audit's most
      severe single-page finding was a step-by-step procedure using a nav label and a button
      name that no longer exist in the app

## Australian spelling

- [ ] "-ise" not "-ize" (organise, customise, recognise), "colour," "behaviour," "licence"
      (noun) — Teralis Pty Ltd is an Australian company and the whole site is en-AU
- [ ] Check anything drafted or pasted from a US-English source (docs generators, AI output,
      third-party copy) — this is the most common way US spelling slips in unnoticed

## No boilerplate-heavy footer bloat

- [ ] Closing CTA is the standard one — "See plans and pricing" → `qrtub.com/pricing`, plus
      the support address (`hi@qrtub.com`) — not a novel variant invented for this page
- [ ] A "Related"/"Next steps" block is a genuine, curated pointer list, not padding — if it
      only exists to look complete, cut it
- [ ] Footer links don't repeat content already stated in the body just to add length

## Internal links point at real, non-anchor-only targets

- [ ] Every internal link resolves to a page that actually exists in `docs.json` navigation
      — not a page that was renamed, merged, or never built
- [ ] Links use relative paths, per `CLAUDE.md`
- [ ] Prefer linking to a full page over a same-page or cross-page `#anchor` alone — an
      anchor-only link degrades badly (or breaks) once content is chunked for retrieval, and
      it's invisible to an agent that only sees the target page's top-level Markdown
- [ ] If this page describes a mechanic that has its own procedural page elsewhere (URL
      Templates, Tubs, Conditional Visibility), link to it — the audit found industry and
      integration pages that described a mechanic in depth with zero link to the page that
      actually teaches it
- [ ] If this page's own frontmatter `description` or body promises something ("the full
      reference," "everything you need") verify the page actually contains all of it — a
      sibling page's promise of completeness was found false against this page's own content

## Everything else the audit surfaced repeatedly

- [ ] No fabricated capability presented as fact — every claim traced to actual behaviour in
      `../qrtub/src/`, not to how the feature "should" work
- [ ] No invented customer quotes, testimonials, or "Real-World Example" that reads as a
      documented case study when it's actually a generic illustrative scenario — label it
      "Example scenario" or similar if it's hypothetical
- [ ] No implied per-role or per-department access control (e.g. "each team only sees their
      own assets") unless that access control is actually built — this exact claim shape
      recurred across multiple industry pages and is explicitly on `BRAND.md`'s false-claims
      list
- [ ] Named third-party products/integrations are named accurately, and phrased as "opens
      X, pre-filled with this item's data" — never "integrates with," "syncs with," or
      implies write-back, since QRtub does not exchange data with third-party systems
- [ ] Language tags on every code block; real, descriptive alt text on every image (not
      "diagram" or "screenshot")
- [ ] Page is actually added to `docs.json` navigation — an unlisted page is unpublished
      regardless of how finished the file is
