# Audit: The Print-First Workflow

- Page: **The Print-First Workflow**
- HTML URL: https://help.qrtub.com/help/print-first-workflow
- Markdown URL: https://help.qrtub.com/help/print-first-workflow.md
- llms.txt description: "Order your tags in bulk before the assets exist, apply them as gear arrives, and connect them when you are ready"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | `200` |
| Content-Type (`.md`) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (`.md`) | `noindex, nofollow` |
| Content-Length (`.md`) | 5,987 bytes |
| Content-Length (HTML, default `Accept`) | 240,223 bytes |
| HTML : Markdown byte ratio | **40.1×** (240223 / 5987) |
| Estimated tokens (chars/4) | **~1,497 tokens** |
| Headings (H1/H2/H3 count) | 1 × H1, 7 × H2, 0 × H3 |
| Boilerplate share of page tokens | **~5.6%** (top banner ~3.0% + footer CTA ~2.6%) |

## 1–2. Raw header findings

**Request with `Accept: text/markdown`:**
```
HTTP/2 200
content-type: text/markdown; charset=utf-8
content-disposition: inline
content-security-policy: default-src 'none'
x-robots-tag: noindex, nofollow
x-llms-txt: /llms.txt
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
content-length: 5987
```

**Default request (HTML):**
```
HTTP/2 200
content-type: text/html; charset=utf-8
content-length: 240223
etag: "z0o17bl2cn55ax"
```
No `x-robots-tag` header on the HTML response — `noindex, nofollow` is emitted only on the markdown variant, exactly the asymmetry seen on other pages of this site (e.g. `/help/creating-your-first-link.md`). The representation built for machine consumption is the one telling crawler-driven indexers not to index/follow it. Direct-fetch agents (a user pasting the URL into ChatGPT/Claude, a coding agent doing `curl`) are unaffected since they don't consult this header, but crawler-based answer engines (Perplexity, Google AI Overviews, Bing Copilot) that respect `X-Robots-Tag` for indexing could be prevented from ever surfacing this specific page's markdown content in an AI-generated answer.

The same rich `Link:` header set is present (`llms.txt`, `llms-full.txt`, API catalog, MCP server card, agent card, agent-skills index) — good machine-discoverability infrastructure, consistent site-wide.

## 3. HTML-to-Markdown ratio

240,223 bytes (HTML) / 5,987 bytes (Markdown) = **40.1×**. Markdown strips essentially all Next.js/Mintlify chrome (nav, sidebar, hydration payload, CSS/JS) down to the page's actual prose — a much leaner ratio than the ~154× seen on the shorter "Creating Your First Link" page, simply because this page has much more page-specific text to begin with (the fixed HTML shell overhead is roughly constant across pages, so the ratio shrinks as body content grows).

## 4–5. Full markdown body + token estimate

Full body (5,987 bytes/chars):

```markdown
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.

# The Print-First Workflow

> Order your tags in bulk before the assets exist, apply them as gear arrives, and connect them when you are ready

Most tools assume you create a record, generate a code for it, print that code, and go and
stick it on something. That order works for one asset. It falls apart at two hundred.

The print-first workflow inverts it: the codes exist before the records do.

## Why the usual order does not survive contact with a site

Durable identification is made in runs. Photo anodised aluminium plates are laid up and cut
as a sheet. Engraved tags are set up once and produced as a batch. Even ordinary
UV-resistant vinyl comes with minimum quantities and a lead time.

<img src="https://mintcdn.com/teralis/c1nSYeFBcz1X7r4A/images/print-first-plates.jpg?fit=max&auto=format&n=c1nSYeFBcz1X7r4A&q=85&s=5ff5307e094e73fd3f228e32eb40e651" alt="A CNC machine cutting a sheet of photo anodised aluminium asset plates, each carrying a QR code, a large asset number and a readable link. The visible plates run in sequence: BOU027, BOU028, BOU029." width="1600" height="1430" data-path="images/print-first-plates.jpg" />

Above: a run of photo anodised aluminium plates being cut. The numbers were allocated before
any of the equipment they will be fixed to was recorded, because the sheet has to be
machined as one job.

That has a consequence people usually discover halfway through a rollout: **you physically
cannot produce one tag on demand** at any sensible cost. So if your process requires the
asset record to exist before the tag can be made, you are stuck with a choice between
delaying the order until every detail is final, or printing something disposable.

Meanwhile the gear itself arrives over weeks. Equipment turns up before anyone has decided
what it is called, who owns it, or which system tracks it.

Print-first accepts both realities instead of fighting them.

## How it works

**1. Generate the Links first.** Create as many as you need — a hundred, a thousand — before
any Items exist. Each one is a real, resolvable URL from the moment it is created.

**2. Send the batch to be produced.** Export the list and give it to whoever makes your tags.
Plates, engraved labels, weatherproof vinyl, NFC inlays — the medium does not matter, because
all it has to carry is a URL.

**3. Apply tags as gear arrives.** Fix a tag to each asset as it lands, in whatever order
things turn up. No decisions required beyond "this tag is now on this machine".

**4. Connect when you are ready.** Create the Item and connect it to the tag already on it.
This is the only step that needs someone to think, and it is a single action.

## What happens if someone scans a tag early

This is the question that usually decides whether the workflow is practical, and the answer
matters: **an unconnected code does not 404.**

* Someone on your team who scans it gets an option to assign it there and then, from their
  phone. The person applying tags can connect it on the spot without going back to a desk.
* Anyone else gets a neutral branded page rather than an error.

So a tag applied on Monday and connected on Friday is not a dead code in the meantime. It is
simply not allocated yet.

## Getting the mistakes back

Tags get put on the wrong machine. Gear gets sold. A Link can be reassigned to a different
Item at any point, so a mistake costs an edit rather than a reprint.

The same applies at the end of an asset's life. Deleting an Item does not delete its Link —
the Link is released back to your unassigned pool, so a tag already fixed to something in the
field keeps working and can be reused. See [Key Concepts](/help/key-concepts).

## Things worth doing while you are at it

**Print the link in text as well as the code.** Codes get scratched, painted over, caked in
mud, or scanned in bad light on a cracked screen. A short readable URL under the code means
someone can still type it.

**Make the tag number and the link match.** If the plate reads `HPP021` and the link is
`qrtub.com/hpp021`, there is no second identifier for anyone to remember, mistype, or map
back to a spreadsheet. Crews already refer to gear by the number on the tag.

**Match the medium to the environment, not the budget.** Tags on a boat or exposed to salt
and constant wash need a different specification from tags on a generator in a yard. You can
run several media grades against one numbering scheme, because the tag is only carrying a
URL — what it is made of is a separate decision from what it points to.

**Order more than you need.** The marginal cost of extra tags in a run is small compared with
setting up a second run three weeks later. Unallocated Links sit in the pool until something
turns up to attach them to.

## Where the codes point

A Link can go straight to a single destination, or open a Page with several options. Either
way you set it up once for the whole category rather than per tag, using a template that
fills in each Item's own data:

```
https://example.com/assets/{{item.serial_number}}
```

Every tag then resolves to its own asset's record without being configured individually.
That is what makes the workflow viable at a few thousand tags rather than a few dozen. See
[Using Fields](/help/using-fields).

Because the destination is held by QRtub rather than baked into the code, you can change
where every tag points later — a new system, a new URL structure — without touching anything
physical.

## Related

* [Key Concepts](/help/key-concepts)
* [Creating Your First Link](/help/creating-your-first-link)
* [Print Batches](/help/print-batches)
* [Using Fields](/help/using-fields)

***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

Token estimate: 5,987 chars / 4 ≈ **1,497 tokens** for the entire page, boilerplate included. Word count is 989 words, so this is a genuinely substantial help page (roughly 4× the length of "Creating Your First Link").

## 6. Heading structure (in order)

| Level | Heading |
|---|---|
| H1 | The Print-First Workflow |
| H2 | Why the usual order does not survive contact with a site |
| H2 | How it works |
| H2 | What happens if someone scans a tag early |
| H2 | Getting the mistakes back |
| H2 | Things worth doing while you are at it |
| H2 | Where the codes point |
| H2 | Related |

No H3s. Flat, sequential H1→H2 structure with no skipped levels — good shape for heading-based chunking (7 clean H2 sections, each independently readable). As on other pages audited from this site, the very first line is the blockquoted pseudo-heading `> ## Documentation Index` — a `##`-level token wrapped in a blockquote, sitting before the real H1. A naive heading scanner that doesn't anchor to line-start, or that strips leading `>` before pattern-matching, risks mistaking this banner for the page's first heading/title ("Documentation Index" instead of "The Print-First Workflow"). Confirmed identical on `/help/key-concepts.md`, so this is a site-wide template artifact, not page content.

## 7. Boilerplate vs. page-specific content

Two blocks are confirmed site-wide boilerplate (byte-for-byte identical to `/help/key-concepts.md` and to the previously-audited `/help/creating-your-first-link.md`):

**A. Top "Documentation Index" banner** (177 bytes incl. trailing blank line):
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
177 / 5987 ≈ **3.0%** of the page's tokens (≈44 of ~1,497 tokens).

**B. Footer CTA + contact block** (157 bytes):
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
157 / 5987 ≈ **2.6%** of the page's tokens (≈39 tokens).

**Combined boilerplate ≈ 334 / 5,987 bytes ≈ 5.6% of the page's token budget.** Because this page is roughly 4× longer than the shortest audited page, the same fixed-size banner+footer occupies a much smaller *share* of the total — 5.6% here versus ~24% on "Creating Your First Link". This is a general property worth noting for the site: the fixed-cost boilerplate tax is much more punishing on short pages than on long ones.

The **"Related"** section (4 links, 187 bytes, ≈3.1% of tokens) is templated in *shape* (every page seems to end with a "## Related" list before the footer) but not in *content* — the four links (`Key Concepts`, `Creating Your First Link`, `Print Batches`, `Using Fields`) are specific to this page's topic and genuinely useful for follow-up retrieval, so it is not counted as wasted boilerplate.

## 8. Other retrieval-quality notes

- **Terminology drift on the product's own noun: "asset" used where the glossary requires "Item."** Cross-checked against `/workspace/qrtub/GLOSSARY.md`, whose "What NOT to Call Things" table explicitly maps `Asset → Item` with the note "qrtub doesn't position itself as asset management." This page uses the bare word **"asset"/"assets" nine times** (subtitle: "before the assets exist"; body: "one asset... falls apart at two hundred", "the asset record to exist", "Fix a tag to each asset", "the end of an asset's life", "its own asset's record"; even in the image alt text: "asset plates", "a large asset number"; and in the URL-template code sample: `https://example.com/assets/{{item.serial_number}}`) versus the canonical term **"Item" used correctly only 5 times**. `/workspace/qrtub/BRAND.md` §1.3 explicitly lists "QRtub is asset management software" under "Claims That Are FALSE (Never State)" and "Asset management software" under "What QRtub is NOT." No single sentence here makes that false claim outright, but the sheer density of "asset" language — including in the llms.txt description itself ("before the assets exist") — means an agent synthesizing an answer from this page (e.g. "what is QRtub?" or "does QRtub do asset management?") is being fed vocabulary that pulls toward exactly the positioning the brand file says to avoid. This is the single most actionable finding on this page.
- **Workflow name diverges from the product's own internal glossary term.** `GLOSSARY.md` defines the canonical term **"Print-before-link"** for "Workflow: print Links on QR codes first, connect to Items later," and this repo's own `CLAUDE.md` use-case library (UC-004) calls it **"Print-before-link deployment."** The published page is titled **"The Print-First Workflow"** instead — a different name for what appears to be the identical concept. This isn't a factual error, but it is a naming split between the internal reference vocabulary and the public-facing page title/H1, which weakens exact-term retrieval: an agent (or a human) searching the site or an index for "print-before-link" will not obviously match "print-first," and vice versa.
- **"Media" (the canonical term for physical material, per GLOSSARY.md) never appears** despite the entire page being about producing and applying physical media in bulk runs. The page instead uses the generic word "tag" throughout (22 occurrences) and "gear"/"equipment" for the tracked object. "Tag" isn't flagged as forbidden in the glossary, but it also isn't the canonical noun, so a page that is topically about Media production/batches doesn't surface that vocabulary at all — a minor consistency gap next to the "asset" issue above, but same shape of problem.
- **Self-contained, but leans on 4 outbound links for full context.** The page defines its own terms reasonably well in-line (Link, Item, Page, Destination are all used in explanatory sentences, not just as bare jargon), which is better than some other pages on this site. It does rely on `/help/key-concepts` for the deeper mechanics of link release-on-delete, and `/help/using-fields` for how the `{{item.serial_number}}` binding syntax actually works — an agent answering a narrow follow-up like "exactly what field syntax can I use in the URL template" would need a second fetch.
- **Relative internal links only resolve with the site's base URL.** All 6 internal links (`/help/key-concepts` ×2, `/help/using-fields` ×2, `/help/creating-your-first-link`, `/help/print-batches`) are root-relative. If the raw markdown is lifted out of context (pasted into a prompt, stored in a vector DB without the source URL as metadata), these are unresolvable. The footer CTA and contact link, by contrast, use fully-qualified URLs (`https://qrtub.com/pricing`, `mailto:hi@qrtub.com`), so the page is inconsistent about which links survive being copied out of their origin.
- **Inline HTML `<img>` tag risks silent content loss in naive markdown pipelines.** The single image is embedded as a raw HTML `<img>` tag (not markdown `![]()` syntax), with `src`, richly descriptive `alt` text, `width`/`height`, and a `data-path` attribute. Full markdown parsers/renderers pass this through fine, and the `alt` text is unusually good for retrieval — it fully describes the photo's content ("A CNC machine cutting a sheet of photo anodised aluminium asset plates... The visible plates run in sequence: BOU027, BOU028, BOU029."), so a text-only agent still gets the image's information. But any pipeline that treats the `.md` as pure Markdown and strips raw HTML tags via a blanket regex (common in cheap "clean the markdown" preprocessing steps for RAG ingestion) would silently delete the entire image and its alt text, leaving the following sentence ("Above: a run of photo anodised aluminium plates being cut...") dangling with no antecedent — a coherence break that wouldn't be visible in the rendered HTML page at all, only in the raw `.md`.
- **`x-robots-tag: noindex, nofollow` only on the `.md` response** (see header section) — repeating this here because it's a header-level, not content-level, retrieval risk and applies identically to every page checked on this site so far.
- **No other malformed markdown.** Lists, bold labels, the fenced code block for the URL template, inline code spans (`` `HPP021` ``, `` `qrtub.com/hpp021` ``), and the horizontal rule (`***`) all render cleanly and match the HTML page's visible content. No broken tables, no dangling reference-style links.
