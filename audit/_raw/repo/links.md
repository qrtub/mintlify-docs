# Link Audit — help/, industries/, integrations/, index.mdx

Scope per instructions: every `.mdx` page under `help/`, `industries/`, `integrations/`, plus `index.mdx`.
Grep was run across the **whole repo** (`grep -rnoP` on all `*.mdx` files) so nothing was sampled; two more files
outside the required scope (`snippets/cta.mdx`, `snippets/snippet-intro.mdx`) turned up in the raw grep and are
called out separately in §6 — they are not imported/referenced by any in-scope page, so they don't affect the count
below.

**Method:**
- Link forms searched: markdown `[text](url)`, markdown image `![alt](url)`, and JSX `href="..."` / `src="..."` /
  `url=` / `to=` / `link=` attributes. Reference-style `[text][ref]` links: none found in the repo.
- Every internal target was resolved against `docs.json`'s `navigation.tabs[].groups[].pages` (and top-level
  `tabs[].pages`) to determine which paths are real, navigable pages, not just guessed from file existence.
- Every external target was checked with `curl -sI` (and `-L` to confirm final destination) on 2026-08-19.

## Summary counts

| Metric | Count |
|---|---|
| In-scope `.mdx` files scanned | 20 (12 `help/`, 5 `industries/`, 2 `integrations/`, 1 `index.mdx`) |
| Total links found in scope (markdown + JSX href) | 117 |
| — internal page links | 68 |
| — internal JSX `href=` links | 2 |
| — internal image link | 1 |
| — external links (`https://…`) | 25 |
| — `mailto:` links | 22 (excluded from OK/BROKEN internal/external classification, see §7) |
| **Broken internal links** | **2** |
| **Anchor-only cross-page links** | **2** |
| External links checked | 3 unique URLs, all resolve 2xx/3xx |

## 1. Full internal-link inventory

Target column strips nothing — anchors are shown exactly as written. Status: **OK**, **BROKEN**, or
**OK + ANCHOR-ONLY** (page exists, but the link points at a fragment on another page — see §3).

| Source page | Line | Link text | Target | Status |
|---|---|---|---|---|
| help/app-links.mdx | 105 | Mitti Integration guide | /integrations/safetyculture | OK |
| help/app-links.mdx | 121 | Device Detection | /help/device-detection | OK |
| help/app-links.mdx | 130 | Device Detection routing | /help/device-detection | OK |
| help/app-links.mdx | 131 | Device Detection routing | /help/device-detection | OK |
| help/app-links.mdx | 137 | Mitti Integration | /integrations/safetyculture | OK |
| help/app-links.mdx | 138 | Device Detection | /help/device-detection | OK |
| help/app-links.mdx | 139 | Conditional Visibility | /help/conditional-visibility | OK |
| help/app-links.mdx | 140 | Pages Overview | /help/pages-overview | OK |
| help/building-a-page.mdx | 14 | Pages Overview | /help/pages-overview | OK |
| help/building-a-page.mdx | 82 | Using Fields | /help/using-fields | OK |
| help/building-a-page.mdx | 98 | Conditional Visibility | /help/conditional-visibility | OK |
| help/building-a-page.mdx | 153 | Pages Overview | /help/pages-overview | OK |
| help/building-a-page.mdx | 154 | Using Fields | /help/using-fields | OK |
| help/building-a-page.mdx | 155 | Conditional Visibility | /help/conditional-visibility | OK |
| help/building-a-page.mdx | 156 | Custom Fields | /help/custom-fields | OK |
| help/conditional-visibility.mdx | 166 | Using Fields | /help/using-fields | OK |
| help/conditional-visibility.mdx | 192 | Device Detection | /help/device-detection | OK |
| help/conditional-visibility.mdx | 231 | Using Fields | /help/using-fields | OK |
| help/conditional-visibility.mdx | 232 | Device Detection | /help/device-detection | OK |
| help/conditional-visibility.mdx | 233 | Pages Overview | /help/pages-overview | OK |
| help/conditional-visibility.mdx | 234 | Key Concepts | /help/key-concepts | OK |
| help/creating-your-first-link.mdx | 38 | Pages | /help/pages-overview | OK |
| help/creating-your-first-link.mdx | 39 | Direct Mode vs Page Mode | /help/key-concepts#link-modes | **OK + ANCHOR-ONLY** |
| help/custom-fields.mdx | 106 | Using Fields | /help/using-fields | OK |
| help/custom-fields.mdx | 107 | Building a Page | /help/building-a-page | OK |
| help/custom-fields.mdx | 111 | Using Fields | /help/using-fields | OK |
| help/custom-fields.mdx | 112 | Building a Page | /help/building-a-page | OK |
| help/custom-fields.mdx | 113 | Key Concepts | /help/key-concepts | OK |
| help/device-detection.mdx | 52 | Fallback URL feature | /help/app-links | OK |
| help/device-detection.mdx | 250 | App Links & Fallback URLs | /help/app-links | OK |
| help/device-detection.mdx | 251 | Using Fields | /help/using-fields | OK |
| help/device-detection.mdx | 252 | Conditional Visibility | /help/conditional-visibility | OK |
| help/device-detection.mdx | 253 | Pages Overview | /help/pages-overview | OK |
| help/device-detection.mdx | 254 | Key Concepts | /help/key-concepts | OK |
| help/key-concepts.mdx | 277 | Creating Your First Link | /help/creating-your-first-link | OK |
| help/key-concepts.mdx | 278 | Pages Overview | /help/pages-overview | OK |
| help/key-concepts.mdx | 279 | Conditional Visibility | /help/conditional-visibility | OK |
| help/key-concepts.mdx | 280 | Physical Media Management Basics | /help/media-basics | OK |
| help/key-concepts.mdx | 283 | Mitti Integration | /integrations/safetyculture | OK |
| help/key-concepts.mdx | 284 | CMMS Systems Integration | /integrations/cmms-systems | OK |
| help/media-basics.mdx | 161 | Key Concepts | /help/key-concepts | OK |
| help/pages-overview.mdx | 57 | Key Concepts | /help/key-concepts | OK |
| help/pages-overview.mdx | 58 | Conditional Visibility (Advanced) | /help/conditional-visibility | OK |
| help/print-batches.mdx | 87 | Media Basics | /help/media-basics | OK |
| help/print-batches.mdx | 91 | Media Basics | /help/media-basics | OK |
| help/print-batches.mdx | 92 | Creating Your First Link | /help/creating-your-first-link | OK |
| help/print-batches.mdx | 93 | Key Concepts | /help/key-concepts | OK |
| help/print-first-workflow.mdx | 17 | (image alt text, see §6) | /images/print-first-plates.jpg | OK (asset file exists) |
| help/print-first-workflow.mdx | 67 | Key Concepts | /help/key-concepts | OK |
| help/print-first-workflow.mdx | 100 | Using Fields | /help/using-fields | OK |
| help/print-first-workflow.mdx | 108 | Key Concepts | /help/key-concepts | OK |
| help/print-first-workflow.mdx | 109 | Creating Your First Link | /help/creating-your-first-link | OK |
| help/print-first-workflow.mdx | 110 | Print Batches | /help/print-batches | OK |
| help/print-first-workflow.mdx | 111 | Using Fields | /help/using-fields | OK |
| help/using-fields.mdx | 123 | Device Detection | /help/device-detection | OK |
| help/using-fields.mdx | 251 | Conditional Visibility | /help/conditional-visibility | OK |
| help/using-fields.mdx | 329 | Conditional Visibility | /help/conditional-visibility#using-ai-to-generate-conditions | **OK + ANCHOR-ONLY** |
| help/using-fields.mdx | 336 | Conditional Visibility | /help/conditional-visibility | OK |
| help/using-fields.mdx | 337 | Device Detection | /help/device-detection | OK |
| help/using-fields.mdx | 338 | Mitti (formerly SafetyCulture) Integration | /integrations/safetyculture | OK |
| help/using-fields.mdx | 339 | CMMS Integration | /integrations/cmms-systems | OK |
| help/using-fields.mdx | 340 | Pages Overview | /help/pages-overview | OK |
| index.mdx | 42 (JSX `href`) | Card "Industries" | /industries | **BROKEN** |
| index.mdx | 49 (JSX `href`) | Card "Integrations" | /integrations | **BROKEN** |
| integrations/cmms-systems.mdx | 131 | Pages Overview | /help/pages-overview | OK |
| integrations/cmms-systems.mdx | 132 | Key Concepts | /help/key-concepts | OK |
| integrations/safetyculture.mdx | 152 | App Links & Fallback URLs | /help/app-links | OK |
| integrations/safetyculture.mdx | 229 | App Links & Fallback URLs | /help/app-links | OK |
| integrations/safetyculture.mdx | 230 | Pages Overview | /help/pages-overview | OK |
| integrations/safetyculture.mdx | 231 | Key Concepts | /help/key-concepts | OK |

All 68 markdown-syntax internal links + the 1 image link resolve to files that both **exist on disk** and are
**wired into `docs.json` navigation** — i.e. they're real, reachable pages, not orphaned files. The only two broken
internal links are the JSX `href="/industries"` and `href="/integrations"` Card links in `index.mdx`.

## 2. Broken links (2)

| Source | Target | Why it's broken |
|---|---|---|
| `index.mdx:42` | `href="/industries"` | `docs.json` gives the "Industries" tab **no top-level `pages` entry** — only a `groups` array (`industries/civil-construction`, etc.). Mintlify does not synthesize a page at the bare tab-root path; there is no `industries/index.mdx` or redirect rule for `/industries` either. This link 404s. |
| `index.mdx:49` | `href="/integrations"` | Same problem: the "Integrations" tab also has no top-level `pages` entry, only `groups`, and there is no `integrations/index.mdx` or redirect for `/integrations`. This link 404s. |

Fix: point these Cards at a real first page in each tab (e.g. `/industries/civil-construction` and
`/integrations/safetyculture`), or add an explicit landing page + `docs.json` entry for each tab.

## 3. Anchor-only cross-page links (2) — flagged as meaningless to a retrieval system

Both fragments verified to exist as real headings on the target page (so they aren't *dead* anchors), but a
fragment-qualified cross-page link is opaque to any retrieval/RAG system that indexes by page, not by
heading-fragment — the system has no way to know the anchor promises something more specific than "go read that
whole page."

| Source | Link text | Target | Target heading exists? |
|---|---|---|---|
| `help/creating-your-first-link.mdx:39` | "Direct Mode vs Page Mode" | `/help/key-concepts#link-modes` | Yes — `## Link Modes` at `help/key-concepts.mdx:100` |
| `help/using-fields.mdx:329` | "Conditional Visibility" | `/help/conditional-visibility#using-ai-to-generate-conditions` | Yes — `## Using AI to Generate Conditions` at `help/conditional-visibility.mdx:75` |

## 4. Links where important information is carried only by the link text/context (not stated inline)

Ranked by how completely the surrounding prose defers substance to the link, strongest first. These are the
classic "see here for details" pattern: the sentence asserts that information exists, but states none of it, so a
retrieval system (or a reader) gets nothing usable unless it also fetches the linked page.

**Strong (zero inline substance given — the linked page is the only place the fact lives):**

1. `help/app-links.mdx:105` — *"See the [Mitti Integration guide](/integrations/safetyculture) for full setup
   instructions."* The Mitti example directly above gives only the app-link/fallback-URL template pair; no setup
   steps appear on this page at all.
2. `integrations/safetyculture.mdx:127` — *"To get question item IDs, see [Mitti's entity ID guide]
   (https://help.mitti.com/en-US/000076/)"* — no instructions for finding a *question item ID* appear anywhere in
   this doc (contrast with the Template ID steps a few lines above, which are spelled out).
3. `help/using-fields.mdx:329` — *"**Use AI to generate expressions** - See [Conditional Visibility]
   (/help/conditional-visibility#using-ai-to-generate-conditions)"* — the numbered list item's entire content is
   the link; no prompt example or technique is given inline. (Also flagged in §3 as anchor-only.)

**Medium (some inline content precedes it, but the specific thing promised — "full reference," "how it works,"
"putting fields on the page" — is still only on the other page):**

4. `help/custom-fields.mdx:106-107` — *"See [Using Fields](/help/using-fields) for the full reference and
   [Building a Page](/help/building-a-page) for putting fields on the page."* A short bullet list of *where*
   fields get used precedes this, but *how* to bind a field into a page section is not explained on this page at
   all.
5. `help/conditional-visibility.mdx:166` — *"See [Using Fields](/help/using-fields) for complete field reference
   with all available fields, types, and examples."* — a partial field list is given just above, but "complete…
   reference" is explicitly deferred.
6. `integrations/safetyculture.mdx:52` — *"See [Mitti's guide on getting entity IDs]
   (https://help.mitti.com/en-US/000076/) for detailed instructions."* — a 4-step outline precedes it, but "detailed
   instructions" for finding the ID on Mitti's own UI live only at the external link.

**Weak / acceptable ("see also" pointers with the core point already stated inline)** — noted for completeness,
not flagged as problems: `help/conditional-visibility.mdx:192`, `help/using-fields.mdx:123`,
`help/using-fields.mdx:251`, `help/device-detection.mdx:52`, `help/building-a-page.mdx:14,82,98`,
`integrations/safetyculture.mdx:152`, `help/print-first-workflow.mdx:100`. In each of these the sentence already
states the actual fact (what the feature does, what it returns, why it matters) and the link is genuinely
supplementary/deeper-dive material rather than the sole carrier of the claim.

## 5. External link check (curl -sI, run 2026-08-19)

| URL | Status | Notes |
|---|---|---|
| `https://qrtub.com/pricing` | **200 OK** | Direct 200, no redirect. Linked 12 times (once per help/industries/integrations/index page + snippet). |
| `https://help.mitti.com/en-US/000076/` | **308 → 308 → 200** | `308 Permanent Redirect` to `/en-US/000076` (drops trailing slash), then a second `308` to `/000076` (drops locale prefix), final target `200 OK`. All hops are 3xx/2xx — not flagged as broken, but note it's a double-redirect chain; linking directly to `https://help.mitti.com/000076` would save two hops. Linked from `integrations/safetyculture.mdx:52,127,228`. |
| `https://help.mitti.com/en-US/000149/` | **308 → 308 → 200** | Same redirect chain/behavior as above. Linked from `integrations/safetyculture.mdx:227`. |

**No external links returned a non-2xx/3xx status.** No 4xx/5xx found.

## 6. Out of required scope, found during the whole-repo grep

- `snippets/cta.mdx` and `snippets/snippet-intro.mdx` contain the same `qrtub.com/pricing` and `mailto:hi@qrtub.com`
  links, but neither file is imported/referenced by any page (`grep -rn "snippets"` across all `.mdx`/`docs.json`
  returns nothing) and neither is in `docs.json` navigation. They appear to be unused Mintlify-starter-kit leftovers.
  Flagging for cleanup/removal consideration, not as a link defect.
- `help/print-first-workflow.mdx:17` is a markdown **image** link (`![alt](/images/print-first-plates.jpg)`), not a
  page link. Included in the inventory since the task said "check every one" — the asset file exists at
  `/workspace/mintlify-docs/images/print-first-plates.jpg`, so it's OK.

## 7. `mailto:hi@qrtub.com` links (22 total, not part of internal/external status check)

Every in-scope page (all 12 `help/`, all 5 `industries/`, both `integrations/` pages) ends with an identical
`mailto:hi@qrtub.com` support-contact link plus the same `https://qrtub.com/pricing` CTA. These aren't page routes
so they were excluded from BROKEN/OK classification and from the curl check (curl can't probe a `mailto:` target
meaningfully); listed here only so the "every link" grep is auditable. No anomalies in the address itself (spelled
identically all 22 times).
