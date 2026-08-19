---
title: "Mintlify AI/SEO/docs.json guidance — verified reference"
description: "Findings from live-fetching Mintlify's own published documentation, gathered to ground a technical audit of help.qrtub.com. Every claim below is cited to an exact Mintlify URL fetched on 2026-08-19."
---

# Mintlify guidance reference (for QRtub docs.json / llms.txt audit)

**Compiled:** 2026-08-19
**Method:** All content below was fetched live from `www.mintlify.com/docs/*` on 2026-08-19, either via the WebFetch tool or via `curl` against the page's `.md` variant (Mintlify serves a Markdown version of every page at `<url>.md` — see §2). Raw fetched Markdown is saved alongside this file in this same `_raw/` directory (`raw_*.md`) for anyone who wants to check the primary source directly rather than trust this summary.

**How to read the verification tags:**
- **VERIFIED** — quoted or closely paraphrased from a specific Mintlify page, URL given.
- **NOT FOUND / NOT DOCUMENTED** — I looked in the pages most likely to cover it and it is not stated. This is not proof a behavior doesn't exist server-side, only that Mintlify's public docs don't claim it. Treat these as open questions for the audit, not confirmed absences.
- **REFUTED** — Mintlify's docs directly contradict the claim as worded.

Pages fetched (11 distinct pages, full list for traceability):
1. https://www.mintlify.com/docs/ai/llmstxt
2. https://www.mintlify.com/docs/ai/markdown-export
3. https://www.mintlify.com/docs/ai/contextual-menu
4. https://www.mintlify.com/docs/ai/model-context-protocol
5. https://www.mintlify.com/docs/ai/skillmd
6. https://www.mintlify.com/docs/ai/mintlify-mcp
7. https://www.mintlify.com/docs/organize/settings-reference
8. https://www.mintlify.com/docs/organize/settings-structure
9. https://www.mintlify.com/docs/organize/settings-seo
10. https://www.mintlify.com/docs/organize/pages
11. https://www.mintlify.com/docs/organize/navigation
12. https://www.mintlify.com/docs/optimize/seo
13. https://www.mintlify.com/docs/guides/geo
14. https://www.mintlify.com/docs/llms.txt (Mintlify's own site index — used to discover/confirm the URLs above)

---

## 1. llms.txt blockquote summary sourced from docs.json top-level `description`

**VERDICT: VERIFIED, verbatim.**

From https://www.mintlify.com/docs/ai/llmstxt (section "Structure of the llms.txt file"):

> "An `llms.txt` file is a plain Markdown file that contains: **Site title** as an H1 heading. **Site description** as a blockquote summary below the title, sourced from the `description` field in your `docs.json` configuration."

The example in the same page shows this shape literally:

```
# Site title

> A brief description of the documentation site.

## Docs
...
```

This is corroborated on https://www.mintlify.com/docs/organize/settings-seo (`description` field): "A description of your documentation site for SEO and AI indexing. It appears in search engine results. AI tools use it to understand your site's purpose." — this is the same top-level `docs.json` field, not a per-page field.

Do not confuse this with the *per-page* description line inside the `## Docs` link list further down llms.txt — that one comes from each page's own frontmatter `description` (truncated at 300 characters or the first line break), per the same page. Both are real and both are called "description," but they are two different fields at two different scopes (site-level `docs.json.description` vs. page-level frontmatter `description`).

---

## 2. Markdown variants: content negotiation vs. fixed `.md` URL suffix

**VERDICT: VERIFIED — both mechanisms exist, both automatic, no configuration required.**

Source: https://www.mintlify.com/docs/ai/markdown-export

- **Fixed URL suffix** (confirmed automatic, no config): "Add `.md` to any page's URL to view a Markdown version." Example given: `https://mintlify.com/docs/ai/markdown-export.md`.
- **HTTP content negotiation** (confirmed automatic, no config): "Send a request with `Accept: text/markdown` or `Accept: text/plain` to any page URL to receive the Markdown version instead of HTML." Example given: `curl -L -H "Accept: text/markdown" https://mintlify.com/docs/ai/markdown-export`.

The page states plainly: "Mintlify automatically generates Markdown versions of pages optimized for AI tools and external integrations." There is no docs.json flag mentioned to turn this on or off — it is default platform behavior, same authentication rules as the HTML page apply (see the Authentication table on that page: public sites → all `.md` URLs public; partial auth → public pages' `.md` public, protected pages' `.md` require auth; full auth → all `.md` require auth).

Additional related mechanisms on the same page (useful for the audit, not directly asked but relevant):
- `markdown.schema` (boolean, docs.json) — whether the Markdown export of API reference pages includes the full OpenAPI/AsyncAPI spec. Default `true`.
- `markdown.instructions` (string or array of strings, docs.json) — custom "Agent Instructions" block appended to every page's Markdown export **and** to `llms.txt` and `llms-full.txt`, after the site title/description.
- The `<Visibility for="humans">` / `<Visibility for="agents">` MDX component lets you show different content in the HTML page vs. the Markdown/agent export of the same page.

Also from https://www.mintlify.com/docs/ai/llmstxt: Mintlify adds discovery HTTP headers to every page response (not just llms.txt-related pages) — `Link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt"` and `X-Llms-Txt: /llms.txt` — so agents can discover the files without prior knowledge of the URL. This is a different, additional mechanism from the per-page `.md` suffix/negotiation.

---

## 3. `.well-known/*` agent-discovery endpoints and their relationship to `contextual.options`

**VERDICT: All four endpoint families are auto-generated with zero configuration required. `contextual.options` is an entirely separate, unrelated feature — it controls a user-facing UI menu of buttons, not server-side discovery-file generation.**

### 3a. MCP server-card / server-cards
Source: https://www.mintlify.com/docs/ai/model-context-protocol

- "Mintlify generates a search MCP server for your site and hosts it at the `/mcp` path of your site URL." — automatic, no config.
- "Mintlify serves all discovery endpoints automatically. They require no configuration."
- Endpoints confirmed: `/.well-known/mcp`, `/.well-known/mcp.json`, `/.well-known/mcp/server-card.json` (single server card), `/.well-known/mcp/server-cards.json` (array of all server cards).
- The **only** configuration trigger mentioned anywhere is for **authenticated sites**: you must configure the `/authed/mcp` endpoint via dashboard **Settings → Security & access → MCP** to enable authenticated access and manage OAuth redirect domains. This is an auth-gating configuration, not a generation trigger — the card/server exists either way.

### 3b. agent-card.json and agent-skills/index.json
Source: https://www.mintlify.com/docs/ai/skillmd

- "Mintlify automatically generates a `skill.md` file for your project by analyzing your documentation with an agentic loop." No manual configuration required; "requires no maintenance." Note: generation/update of `skill.md` "can take up to 24 hours" (i.e., not instantaneous after a docs change — worth flagging in the audit if freshness is being assumed).
- Confirmed auto-generated, no-config endpoints: `/.well-known/agent-skills/index.json`, `/.well-known/skills/index.json`, `/.well-known/agent-card.json` (an Agent-to-Agent/A2A agent card).
- You *may* optionally override the auto-generated `skill.md` with a custom file placed at the project root — same override pattern as `llms.txt`/`llms-full.txt`.

### 3c. `/.well-known/api-catalog`
**VERDICT: NOT FOUND / NOT DOCUMENTED.** I searched every fetched page (`grep -rn "api-catalog"` across all raw fetches) and found zero mentions anywhere in Mintlify's docs.json reference, SEO reference, MCP page, skill.md page, or API settings page (`/docs/organize/settings-api`). Mintlify's own documentation does not currently describe an `api-catalog` well-known endpoint. If help.qrtub.com is actually serving one, either (a) it is a Mintlify feature not yet documented publicly, (b) it's a generic web convention Mintlify happens to also emit but doesn't call out, or (c) something else is generating that path. This needs live-checking against the actual site rather than assumed from Mintlify docs — I could not confirm or refute it from Mintlify's published guidance.

### 3d. Relationship to `contextual.options`
Source: https://www.mintlify.com/docs/ai/contextual-menu and https://www.mintlify.com/docs/organize/settings-reference (`contextual` section)

`contextual` in `docs.json` is **purely a UI feature**: "The contextual menu provides quick access to AI-optimized content and direct integrations with popular AI tools." It renders a menu button in the page header (or ToC, via `contextual.display`) with clickable actions. It is unrelated to whether the `.well-known` discovery files exist — those are generated regardless of whether `contextual` is configured at all.

Confirmed valid `contextual.options` identifiers, with exact descriptions, straight from the docs.json schema reference (https://www.mintlify.com/docs/organize/settings-reference, `### contextual` section) and the contextual-menu page:

| Identifier | Description |
|---|---|
| `copy` | Copies the current page as Markdown for pasting as context into AI tools |
| `view` | Opens the current page as Markdown |
| `assistant` | Opens the assistant with the current page as context |
| `download-pdf` | Downloads the current page as a PDF (Enterprise plans) |
| `chatgpt` | Creates a ChatGPT conversation with the current page as context |
| `claude` | Creates a Claude conversation with the current page as context |
| `perplexity` | Creates a Perplexity conversation with the current page as context |
| `grok` | Creates a Grok conversation with the current page as context |
| `aistudio` | Creates a Google AI Studio conversation with the current page as context |
| `devin` | Creates a Devin session with the current page as context |
| `devin-desktop` | Opens Devin Desktop with the current page as context |
| `mcp` | Copies your MCP server URL to the clipboard |
| `add-mcp` | Copies the `npx add-mcp` install command |
| `cursor` | Installs your hosted MCP server in Cursor |
| `vscode` | Installs your hosted MCP server in VS Code |
| `devin-mcp` | Installs your hosted MCP server in Devin |
| `download-spec` | Downloads the deployment's OpenAPI spec (API reference pages only) |

**All of the values seen on help.qrtub.com — `copy`, `view`, `chatgpt`, `claude`, `perplexity`, `mcp`, `cursor`, `vscode` — are real, currently-documented identifiers.** None are inert or deprecated per current docs. `contextual.options` is a required array field (first item = default action); `contextual.display` is optional, `"header"` (default) or `"toc"`. Pages can override the site-wide config with a page-level `contextual` frontmatter field of identical shape.

---

## 4. `X-Robots-Tag: noindex` on markdown variants / default exclusion of `.md` URLs from search indexing

**VERDICT: NOT FOUND / NOT DOCUMENTED — this specific claim is not addressed anywhere in Mintlify's published docs, in either direction.**

I checked https://www.mintlify.com/docs/ai/markdown-export (the canonical page for `.md` export behavior), https://www.mintlify.com/docs/optimize/seo (the full SEO/indexing/robots reference), and https://www.mintlify.com/docs/organize/settings-seo. None mention an `X-Robots-Tag` HTTP header at all, and none state that `.md` URLs are (or aren't) excluded from search indexing by default, distinct from their HTML counterpart.

What Mintlify's docs *do* say about indexing control, which is adjacent but not the same claim:
- `noindex` (page frontmatter, boolean): "Set to `true` to exclude the page from site search, sitemaps, search engine indexing, and AI assistant context. The page remains visible in navigation." (https://www.mintlify.com/docs/organize/pages) — this is a per-page opt-out, not something applied automatically to `.md` variants specifically.
- `hidden: true` frontmatter automatically implies `noindex: true`. (Same page, and https://www.mintlify.com/docs/optimize/seo, "Disable indexing" section.)
- `seo.indexing` (docs.json, `"navigable"` default or `"all"`): controls whether *pages not in your nav* get indexed at all — again a page-inclusion setting, not a `.md`-vs-HTML distinction.
- A whole-project "Don't index project" dashboard add-on renders every page `noindex, nofollow`, blanks the sitemap, 404s `llms.txt`/`llms-full.txt`, and serves a site-wide disallow `robots.txt` (https://www.mintlify.com/docs/optimize/seo, "Disable indexing for the entire project"). Still nothing `.md`-specific.
- Separately, and arguably in tension with a hypothetical "noindex the .md by default" policy: Mintlify's auto-generated `robots.txt` explicitly **allows all crawlers by default**, including AI bots (`GPTBot`, `ClaudeBot`, `PerplexityBot`, `Google-Extended`, etc.), via Content-Signal directives that "opt your documentation in to AI training, search indexing, and AI answer generation" (https://www.mintlify.com/docs/guides/geo, "Allow AI agents in robots.txt"; also https://www.mintlify.com/docs/optimize/seo). This is a robots.txt-level allow, not a per-URL response header, but it's worth noting in the audit that Mintlify's default posture is "let AI/search crawlers in," which sits awkwardly next to an assumption that `.md` URLs are noindexed by default.

**Conclusion for the audit:** Do not assert Mintlify auto-noindexes `.md` variants — that is not a documented behavior. If help.qrtub.com's `.md` responses are observed (live, via curl) to carry `X-Robots-Tag: noindex`, that is either an internal Mintlify implementation detail not surfaced in public docs, or something else entirely, and should be verified by inspecting actual response headers from the live site rather than cited to Mintlify's docs.

---

## 5. Supported frontmatter fields — and are `sidebar_position`, `category`, `slug` real?

**VERDICT: The full supported field list is enumerated below, sourced from the canonical schema page. `sidebar_position`, `category`, and `slug` do NOT appear anywhere in Mintlify's frontmatter reference or its docs.json schema reference — REFUTED as supported fields; they are not part of Mintlify's schema.**

Source: https://www.mintlify.com/docs/organize/pages ("Page metadata" section — this is Mintlify's canonical, current frontmatter reference).

Confirmed frontmatter fields, with what each does:

| Field | Type | Purpose |
|---|---|---|
| `title` | string | Page title in nav/browser tab. If omitted, Mintlify auto-generates one from the file path. |
| `description` | string | Brief summary, displays under the title, feeds SEO **and** the per-page description used in `llms.txt` (see §1). |
| `sidebarTitle` | string | Short title shown in the sidebar (distinct from `title`). |
| `icon` | string | Font Awesome / Lucide / Tabler icon name, external URL, or project file path. |
| `iconType` | string | Font-Awesome-only style: `regular`, `solid`, `light`, `thin`, `sharp-solid`, `duotone`, `brands`. |
| `tag` | string | Small tag/badge shown next to the page title in the sidebar (e.g. "NEW"). |
| `hidden` | boolean | Removes page from sidebar nav; page stays reachable by direct URL; **implies `noindex: true` automatically**. Explicitly warns: do not set to `false`, "results in undefined behavior" — to unhide, remove the field entirely. |
| `noindex` | boolean | Excludes page from site search, sitemaps, search-engine indexing, and AI assistant context. Page stays in navigation. |
| `searchable` | boolean (default `true`) | Opt page out of in-product/docs search + AI assistant context (page remains externally indexable and in sitemap). |
| `boost` | number | Multiplies in-product search ranking (>1 boosts, 0–1 de-prioritizes). No effect if `searchable: false`. |
| `deprecated` | boolean | Shows a "deprecated" label next to the page title. |
| `hideFooterPagination` | boolean | Hides prev/next footer nav links. |
| `related` | array or boolean | Curated related-pages list, or `false` to hide the section; requires the Related pages add-on. |
| `hideApiMarker` | boolean | Hides the HTTP method badge (GET/POST/etc.) next to the title in the sidebar. |
| `contextual` | object | Per-page override of the site-wide `contextual` menu (`options` + `display`, same shape as docs.json — see §3d). |
| `groups` | string[] | Restricts page access to specific auth groups (requires authentication configured). |
| `mode` | string | Page layout: `default`, `wide`, `custom`, `frame`, `center`, or `assistant`. |
| `api` / `openapi` | string | Adds an interactive API playground for the given spec/operation. |
| `url` | string | For nav entries that are external links rather than local pages. |
| `keywords` | string[] | Internal-search-only discoverability keywords; not shown in page content. |
| `timestamp` | boolean | Per-page override of the global `metadata.timestamp` last-modified display (see §6). |
| `lastUpdatedDate` | string (date or ISO 8601) | Per-page override of the *displayed* last-modified date (see §6). |
| any custom key | any YAML | "Any valid YAML frontmatter" is accepted and passed through, e.g. `product: "API"` — Mintlify explicitly allows arbitrary custom fields, it just doesn't do anything with them unless a component/theme reads them. |
| `"og:title"`, `"twitter:image"`, etc. | string | Per-page SEO/social meta-tag overrides — must be quoted because of the colon in the key. Full list at https://www.mintlify.com/docs/optimize/seo (§ "Page-level meta tags" / "Common meta tags reference"). |

**On `sidebar_position` / `category` / `slug` specifically:** I grepped the full raw text of both https://www.mintlify.com/docs/organize/pages and https://www.mintlify.com/docs/organize/settings-reference for these three strings — zero matches in either. Mintlify's own docs state plainly that page order and hierarchy come entirely from the `navigation` object in `docs.json` (https://www.mintlify.com/docs/organize/navigation — pages are listed in an explicit array, not inferred from a numeric position field), and that page URLs are simply file paths in the repo, not a separate `slug`. This matches — and confirms — the project's own `CLAUDE.md`, which already flags these three as "inert leftovers from another system" that should not be added to new pages. Mintlify's docs give no reason to revise that guidance; if anything they reinforce it, since neither field is recognized or documented anywhere in the current schema.

---

## 6. "Last updated" timestamp — is there one, and how is it populated?

**VERDICT: VERIFIED — yes, it exists, disabled by default, and when enabled it is populated from git commit history by default, with two explicit override mechanisms.**

Source: https://www.mintlify.com/docs/organize/settings-seo (`metadata` section) and https://www.mintlify.com/docs/organize/pages ("Last modified timestamp" section — this second page has the more complete explanation of *how the date is sourced*).

- `metadata.timestamp` (docs.json, boolean, **default `false`**): "Display a last-modified date on all pages. When enabled, each page shows the date its content was last modified."
- Per-page override: `timestamp` frontmatter field (boolean) — forces the timestamp on or off for that one page regardless of the global setting.
- **Data source, exact quote** (https://www.mintlify.com/docs/organize/pages): "this timestamp uses the date of the last Git commit that modified the page's source file." So it is git-based automatically, not a field you fill in — confirming the automatic half of the claim.
- Manual override of the *displayed date* (for cases where git history doesn't reflect real content changes, e.g. content migrated from elsewhere): `lastUpdatedDate` frontmatter field, accepting a date-only value or ISO 8601 timestamp.
- Mintlify documents the exact precedence order for which date gets shown: (1) `lastUpdatedDate` frontmatter if set, (2) for git-backed deployments, the last commit date touching that file, (3) fallback to the most recent deployment timestamp (e.g., for non-git-backed content).

So: it's git-based by default, with a frontmatter escape hatch (`lastUpdatedDate`) for when git history is misleading, and a separate boolean (`timestamp`) purely to show/hide the feature per page. This is a fully documented, current feature — not a legacy or inert one.

---

## 7. Mintlify's own AI/LLM-retrieval writing best practices (GEO guide)

**VERDICT: VERIFIED for everything Mintlify actually publishes — but two specific claims commonly repeated as "Mintlify best practice" (one-question-per-page, and a recommended page-length range) are NOT FOUND anywhere in Mintlify's guide. Do not attribute those two to Mintlify.**

Source: https://www.mintlify.com/docs/guides/geo (the complete, and only, page Mintlify publishes specifically on this topic — "GEO guide: Optimize docs for AI search and answer engines").

What the guide actually recommends, verbatim/near-verbatim:

- **Lead with the answer.** "Structure each section so the most important information comes first... Avoid preambles, unnecessary context, and caveats before the point." Gives a worked "leads with the answer" vs. "buries the answer" example.
- **Use headings phrased as the questions users ask**, not topic labels — e.g. "How do I rotate my API keys?" over "API key management," because "AI systems match user queries to heading text when deciding what content to surface."
- **Be specific with numbers, limits, and examples.** "Vague descriptions don't get cited. Specific, accurate details do." For every parameter/behavior: state the exact value/range, describe boundary behavior, show a concrete code example.
- **Use consistent terminology** — one name per concept throughout a page (their example: don't alternate "API key" / "access token" / "API token").
- **Use sequential, non-skipping heading hierarchy** (don't jump H2 → H4).
- **Label all code blocks** with a language.
- **Write real alt text** for diagrams that conveys the actual content, not generic labels like "architecture diagram."
- **Use specific nouns instead of pronouns** ("the API key" instead of "it") because AI systems excerpt content and lose surrounding context.
- **Write descriptive page titles/descriptions** framed as "what does this page help users do?" — ties directly to the frontmatter `description` field (§5) which also feeds `llms.txt` (§1).
- **Control indexing settings** — reminder that `seo.indexing` defaults to `"navigable"` (nav-only); set to `"all"` to bring hidden pages into AI/search context.
- **Allow AI agents in robots.txt** — don't block `GPTBot`, `OAI-SearchBot`, `ChatGPT-User`, `ClaudeBot`, `Claude-User`, `PerplexityBot`, `Google-Extended`; Mintlify's default `robots.txt` already allows all of these, so this is really a "don't override the default with something more restrictive" warning. The CLI check `mint score` includes a `robotsTxtAllowsAI` check for this.
- **Test how AI tools represent your docs** by literally asking ChatGPT/Perplexity/Claude product questions and checking whether/how they cite you, treating wrong answers as a signal your docs are "ambiguous, missing, or contradictory rather than that the AI is broken."
- Explicit FAQ answer in the guide: "Should I write differently for AI versus human readers? No... Write for your users first; GEO follows naturally from good technical writing."

**What is NOT in this guide (and I found no other Mintlify page covering it):**
- No statement recommending "one question per page" or any single-topic-per-page rule as such (their closest analog is *heading*-level directness — "lead with the answer" per section/heading — not a page-scoping rule).
- No stated ideal or maximum page length / word count for GEO purposes anywhere in this guide or in the SEO guide. (The SEO — not GEO — guide at https://www.mintlify.com/docs/optimize/seo does give character-count guidance for **titles** (50–60 chars) and **meta descriptions** (150–160 chars) under "SEO best practices," but that's classic search-snippet-length guidance, not documentation-page-length guidance, and it's an SEO recommendation, not a GEO/AI-retrieval one.)

If the audit has been treating "one question per page" or a page-length ceiling as Mintlify-sourced guidance, that should be corrected to "not Mintlify's own published guidance" — it may be sound general documentation advice from elsewhere, but Mintlify does not publish it under this framing.

---

## Additional relevant findings (not directly asked, but load-bearing for a docs.json audit)

### SEO meta tag generation, structured data, and canonical URLs
Source: https://www.mintlify.com/docs/optimize/seo

- Mintlify auto-generates a large set of meta tags per page (charset, `og:*`, `twitter:*`, `canonical`, `robots`, `noindex`, `keywords`, favicons, `apple-mobile-web-app-title`, `msapplication-TileColor`, `generator: Mintlify`, sitemap link, etc.) — all overridable via `docs.json` `seo.metatags` or page frontmatter.
- `canonical` is auto-built from the page URL; overridable per-page via frontmatter `canonical`.
- Mintlify emits schema.org JSON-LD structured data on every indexable page as a connected `@graph`: `Organization`, `WebSite`, `WebPage` (incl. description + modification dates), `BreadcrumbList` (from `docs.json` navigation), and either `TechArticle` or `APIReference` as the main content entity depending on whether the page has `api`/`openapi`/`asyncapi` frontmatter. Pages with `noindex: true` get **no** structured data at all.
- `seo.organization` (docs.json object: `id`, `name`, `legalName`, `url`, `logo`, `sameAs`) sets the structured-data publisher entity; if omitted, Mintlify derives it from site name/logo/URL.
- OG images are auto-generated per page (1200×630, using site logo + page title/description + primary color); three levels of customization exist (custom background image, per-page override, full custom `og:image`).
- Mintlify auto-generates `sitemap.xml` and `robots.txt`; both viewable by appending the path to the site URL. The auto `robots.txt` includes Content-Signal directives (per contentsignals.org / Cloudflare's Content Signals Policy) that by default opt the site's content in to AI training, search indexing, and AI answer generation for all user agents. A custom `robots.txt`/`sitemap.xml` placed at the project root fully overrides the generated one (and loses the default Content-Signal directives).

### docs.json top-level structure (confirmed section map)
Source: https://www.mintlify.com/docs/organize/settings-reference (the complete schema reference) and https://www.mintlify.com/docs/llms.txt (site index), which together confirm docs.json settings are split by Mintlify's own docs into these reference pages:
- https://www.mintlify.com/docs/organize/settings.md — overview/required file
- https://www.mintlify.com/docs/organize/settings-appearance.md — theme, colors, logo, favicon, fonts, background
- https://www.mintlify.com/docs/organize/settings-structure.md — navbar, nav groups, footer links, banner, **contextual menu**, redirects
- https://www.mintlify.com/docs/organize/settings-api.md — OpenAPI/AsyncAPI specs, API playground, SDK examples, auth
- https://www.mintlify.com/docs/organize/settings-integrations.md — analytics, chat widgets, third-party integrations
- https://www.mintlify.com/docs/organize/settings-seo.md — top-level `description`, `seo` object, `search` object, `metadata.timestamp`

### Related-pages / navigation add-ons
Source: https://www.mintlify.com/docs/organize/related-pages (per the llms.txt index; not deep-fetched, but its description is: "Add a related topics section at the bottom of each page using automatic recommendations or manually curated links defined in page frontmatter") and the `related` frontmatter field documented in §5 above.

---

## Summary table: claim-by-claim verdicts

| # | Claim | Verdict | Primary citation |
|---|---|---|---|
| 1 | llms.txt blockquote sourced from docs.json top-level `description` | **VERIFIED** (verbatim) | /docs/ai/llmstxt |
| 2 | `.md` variants via both fixed suffix and Accept-header negotiation, automatic | **VERIFIED**, both mechanisms, zero config | /docs/ai/markdown-export |
| 3 | `.well-known` mcp/server-card, agent-card, agent-skills, api-catalog auto-generated; relation to `contextual.options` | **VERIFIED** for mcp/server-card, agent-card, agent-skills (all automatic, no config beyond auth-gating). **api-catalog: NOT FOUND** in Mintlify docs. `contextual.options` confirmed to be a separate, unrelated UI-menu feature; all 8 values seen on the QRtub site are real documented identifiers | /docs/ai/model-context-protocol, /docs/ai/skillmd, /docs/ai/contextual-menu |
| 4 | `X-Robots-Tag: noindex` auto-applied to `.md` variants; `.md` excluded from indexing by default | **NOT FOUND / NOT DOCUMENTED** either way | /docs/ai/markdown-export, /docs/optimize/seo |
| 5 | Frontmatter field list; `sidebar_position`/`category`/`slug` real or inert | Full field list **VERIFIED**; the three named fields **REFUTED** as supported — absent from schema, confirms CLAUDE.md's existing guidance | /docs/organize/pages, /docs/organize/settings-reference |
| 6 | "Last updated" timestamp concept and data source | **VERIFIED** — `metadata.timestamp` (docs.json) + `timestamp`/`lastUpdatedDate` (frontmatter), git-commit-date by default | /docs/organize/pages, /docs/organize/settings-seo |
| 7 | Mintlify's own AI/LLM retrieval writing guidance | **VERIFIED** for lead-with-answer, question-phrased headings, specificity, consistent terminology, heading hierarchy, code-block language tags, alt text, noun-not-pronoun, descriptive title/description, indexing config, robots.txt AI-crawler allowance. **"One question per page" and page-length guidance: NOT FOUND** — not Mintlify-sourced | /docs/guides/geo |

---

## Notes for downstream agents using this file

- Raw fetched Markdown for every page cited above is saved as `raw_*.md` in this same directory, named after the URL path (e.g. `raw_organize_settings-seo.md` = https://www.mintlify.com/docs/organize/settings-seo). Use these to pull an exact quote rather than re-fetching.
- Everything tagged **NOT FOUND / NOT DOCUMENTED** in this file is a gap in *Mintlify's public documentation*, not necessarily a gap in Mintlify's actual product behavior. If an audit needs to settle one of those (especially §3's `api-catalog` and §4's markdown-variant robots header), the only reliable next step is inspecting live HTTP responses from an actual Mintlify-hosted site (e.g. `curl -I` against help.qrtub.com's `.md` URLs and its `/.well-known/*` paths), not further searching Mintlify's docs — I was thorough across the docs.json reference, SEO reference, and every `/docs/ai/*` page and did not find it there.
- I did not find any Mintlify page contradicting an earlier/superseded version of any of these claims — nothing here reads as "this used to work differently." Where Mintlify's docs are simply silent (§4, and the api-catalog part of §3), that silence should not be read as either confirmation or denial.
