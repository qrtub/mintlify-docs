---
title: "docs.json audit — cross-referenced against verified Mintlify guidance"
description: "Field-by-field audit of QRtub's docs.json against Mintlify's documented docs.json schema and AI/SEO behavior, as verified in audit/_raw/mintlify-guidance.md."
---

# docs.json audit (QRtub help.qrtub.com)

**Audited file:** `/workspace/mintlify-docs/docs.json` (131 lines)
**Cross-referenced against:** `/workspace/mintlify-docs/audit/_raw/mintlify-guidance.md` (compiled 2026-08-19 from live Mintlify docs fetches) and the raw source pages it cites in `audit/_raw/raw_*.md`.
**Method:** every recommendation below cites (a) the exact docs.json field path — present, absent, or misconfigured — and (b) the specific guidance-file section or raw-file line backing the claim. Where I pulled a more specific quote directly from a raw file than the guidance file's own summary, I cite the raw file and line number so this can be checked without re-fetching Mintlify.

I also read every `.mdx` page's frontmatter to confirm nav completeness (§4) and enumerated the repo's page files against `docs.json` navigation.

---

## 1. Top-level `description` — MISSING (high priority)

**Not present anywhere in docs.json.** The file has `$schema`, `theme`, `name`, `redirects`, `colors`, `favicon`, `navigation`, `logo`, `navbar`, `contextual`, `footer` — no `description` key at the top level (confirm by reading the full 131-line file; there is no sibling key to `"name": "QRtub documentation"` for `description`).

Guidance file `mintlify-guidance.md` §1 (**VERIFIED, verbatim**, citing `/docs/ai/llmstxt`):
> "An `llms.txt` file is a plain Markdown file that contains: **Site title** as an H1 heading. **Site description** as a blockquote summary below the title, sourced from the `description` field in your `docs.json` configuration."

Corroborated in the schema reference itself, `audit/_raw/raw_organize_settings-reference.md:263-268`:
```
### `description`
Site description for SEO and AI indexing.
**Type:** string
```
and `audit/_raw/raw_organize_settings-seo.md:13-21`: "A description of your documentation site for SEO and AI indexing. It appears in search engine results. AI tools use it to understand your site's purpose."

**Effect of the gap, concretely:** the site's auto-generated `/llms.txt` currently ships as:
```
# QRtub documentation

## Docs
...
```
with no blockquote line under the H1 — exactly the "currently llms.txt has none" symptom the task description flagged, and it is a direct, mechanical consequence of the missing field, not a rendering bug. Any AI tool or agent that fetches `/llms.txt` to decide whether QRtub's docs are relevant to a query is reading a title with zero disambiguating context ("QRtub documentation" alone doesn't say what QRtub *is*).

**Recommendation:** add a top-level `description`, e.g. something in the register of the homepage's own framing (`index.mdx` frontmatter: `"Print Once. Update Forever."`) but written as a full sentence for this exact purpose — e.g. `"Help docs for QRtub, a QR code platform that lets you change what a printed code does after it's deployed."` Keep it evergreen (per this repo's own `CLAUDE.md` content-strategy rule) and accurate to what's actually documented — do not describe capabilities (API, analytics, teams) that the retrieval audit (`audit/_raw/retrieval-grid-summary.md`) confirms aren't in the docs, since this string is also read by AI tools deciding what QRtub's docs cover.

---

## 2. SEO/metadata options — which are set, which aren't

Full inventory of the schema's SEO-adjacent top-level objects (`audit/_raw/raw_organize_settings-reference.md:263-269, 923-964`), checked against docs.json:

| Field | In docs.json? | Guidance citation |
|---|---|---|
| `description` | **Absent** | §1 above |
| `seo.indexing` | Absent (defaults to `"navigable"`) | `raw_organize_settings-reference.md:929-934`. **Not a live gap**: I confirmed every `.mdx` file in `help/`, `industries/`, and `integrations/` is already listed in `navigation` (§4 below) — there are no un-navigated pages this default would exclude, so leaving it unset is currently harmless. Worth a one-line note only if orphan pages get added later without a nav entry. |
| `seo.metatags` | Absent | `raw_organize_settings-reference.md:936-940`. Low priority — Mintlify auto-generates `og:*`/`twitter:*`/OG images from `name`, `logo`, and `colors.primary` per guidance's "Additional relevant findings" section, so this is a refinement, not a gap. |
| `seo.organization` | **Absent** | `raw_organize_settings-reference.md:942-946`: "Organization used as the publisher entity in structured data (JSON-LD) on every page. It accepts `id`, `name`, `legalName`, `url`, `logo`, and `sameAs`." Worth setting deliberately: QRtub is legally Teralis Pty Ltd (per this repo's `CLAUDE.md`, first line), and without `seo.organization` Mintlify falls back to deriving the publisher entity from site name/logo/URL alone — meaning the JSON-LD `Organization` entity that AI/search engines read as "who publishes this" says "QRtub documentation," not the actual legal entity, and has no `sameAs` links to corroborate it. |
| `search.prompt` | Absent (default placeholder) | `raw_organize_settings-reference.md:956-960`. Cosmetic-only; low priority. |
| `metadata.timestamp` | **Absent** (default `false`) | `raw_organize_settings-seo.md:110-114` / guidance §6: "Display a last-modified date on all pages... this timestamp uses the date of the last Git commit that modified the page's source file." This repo is git-backed (`/workspace/mintlify-docs/.git` exists), so enabling this is zero-maintenance and directly relevant to a docs corpus the retrieval audit already flagged as having currency-sensitive gaps (billing, plans, permissions) — a visible last-updated date is a cheap trust/freshness signal for both human readers and AI tools deciding whether content is current enough to cite. |
| `banner` | Absent | Not flagged as a gap — this is an intentional, situational field (site-wide announcement bar), not a baseline SEO/AI element. No recommendation. |

**Priority order if only doing a few:** `description` (§1, direct llms.txt effect) > `seo.organization` (structured-data correctness) > `metadata.timestamp` (freshness signal) > the rest (cosmetic/refinement).

---

## 3. `contextual.options` — complete/correct?

Current value, `docs.json:114-125`:
```json
"contextual": {
 "options": [
   "copy",
   "view",
   "chatgpt",
   "claude",
   "perplexity",
   "mcp",
   "cursor",
   "vscode"
 ]
}
```

**All eight are real, current, documented identifiers** — confirmed against both the schema reference (`raw_organize_settings-reference.md:660-666`, the enum type for `contextual.options`) and the dedicated feature page (`raw_ai_contextual-menu.md:25-44`, the full options table with descriptions). None are deprecated or inert. This matches guidance §3d's verdict exactly.

**What's supported but not enabled** — full identifier list from `raw_ai_contextual-menu.md:25-44` and `raw_organize_settings-reference.md:666`, compared to the current set:

| Identifier | In use? | Notes |
|---|---|---|
| `copy`, `view`, `chatgpt`, `claude`, `perplexity`, `mcp`, `cursor`, `vscode` | ✅ | current set |
| `grok` | ❌ | "Creates a Grok conversation with the current page as context" — same free-to-add pattern as `chatgpt`/`claude`/`perplexity`, no product/plan dependency documented. Given the other three general-purpose AI assistants are already included, omitting Grok specifically (vs. omitting all four, or including all four) looks like an oversight rather than a deliberate choice. |
| `aistudio` | ❌ | "Creates a Google AI Studio conversation with the current page as context" — same category as above, same no-dependency status. |
| `add-mcp` | ❌ | "Copies the `npx add-mcp` install command" — complements the already-enabled `mcp` (copies server URL) and would round out the MCP install path alongside the already-enabled `cursor`/`vscode` installers, for AI tools that consume the npx command instead of a raw URL. |
| `assistant` | ❌ | "Opens the assistant with the current page as context" (`raw_ai_contextual-menu.md:29`, linking to `/docs/assistant/index`) — this is Mintlify's own hosted AI assistant product, a **separate feature that must itself be enabled/configured**, not just an options-array entry. Flagging as a dependency, not a plain omission: don't add this identifier without first confirming (out of scope for this docs.json-only audit) whether the Mintlify AI Assistant add-on is actually provisioned for this deployment. |
| `download-pdf` | ❌ | Enterprise-plan-gated (`raw_ai_contextual-menu.md:30`) — do not recommend without confirming plan tier; out of scope here. |
| `devin`, `devin-desktop`, `devin-mcp` | ❌ | Devin-specific (an AI coding agent product). Given QRtub's audience (facilities/asset-management operators per the industries pages, not software developers), these are low-value additions — reasonable to leave out; not flagging as a gap. |
| `download-spec` | n/a | Only appears on API reference pages; QRtub's docs.json has no `api`/`openapi` configuration, so this option has no surface to attach to. Not applicable. |
| custom option objects | ❌ | Not in use — e.g. a "Contact support" or "Get started" custom option is possible (`raw_ai_contextual-menu.md:97-186`) but this is a content/marketing decision, not a technical gap. No recommendation either way. |

**Recommendation:** the clearest, lowest-risk additions are `grok`, `aistudio`, and `add-mcp` — all documented, all free of plan/product prerequisites, all consistent with the general-assistant/MCP-installer categories already represented in the current list.

---

## 4. Navigation grouping — structural issues

Full nav, `docs.json:17-92`, three tabs: **Help** (index page + 6 groups, 12 pages), **Industries** (1 group, 5 pages), **Integrations** (1 group, 2 pages).

**Nav-vs-repo completeness check (confirmed clean):** every `.mdx` file under `help/`, `industries/`, and `integrations/` (12 + 5 + 2 = 19 files) has a corresponding entry in `navigation`, and every nav entry resolves to a real file — no orphan pages, no broken nav references. Every page also has both `title` and `description` frontmatter set, satisfying this repo's own `CLAUDE.md` requirement.

### Issue A — the lone `"index"` page has no group, and is mis-scoped under the "Help" tab

`docs.json:19-23`:
```json
{
  "tab": "Help",
  "pages": [
    "index"
  ],
  "groups": [ ... ]
}
```
This mixes a tab-level `pages` array with a tab-level `groups` array — a pattern the schema does allow (confirmed valid in `audit/_raw/raw_organize_navigation.md:37-49` for `navigation.pages` generally, and tabs support the same `pages`/`groups` shapes per `raw_organize_settings-reference.md` §navigation.tabs), so this is not a validation error. But it is a content-modeling problem, not just a cosmetic one:

- `index.mdx` (read directly) is the site's marketing homepage — "Welcome to QRtub," "Print Once. Update Forever.," a "Why Choose QRtub?" section with three benefit blurbs ("Print Before You're Ready," "One Code, Multiple Systems," "Future Proof"). This is landing-page copy, not a help/how-to article, by this repo's own content-type taxonomy (`CLAUDE.md`, "Site structure" table: Help = "Enable successful usage," tone "Technical, clear, utility").
- Placing it as the sole ungrouped page inside the **Help** tab means a retrieval or browsing agent walking the nav tree sees "Help > Welcome to QRtub" with no group label to signal that this one entry is categorically different from the 12 genuine how-to pages beside it.
- It also affects the schema.org `BreadcrumbList` JSON-LD Mintlify generates from `navigation` on every indexable page (guidance file, "Additional relevant findings" → SEO section, citing `raw_optimize_seo.md`) — an ungrouped page produces a shallower/inconsistent breadcrumb depth than its grouped siblings, which is a small but real structured-data inconsistency an AI answer engine parsing breadcrumbs would notice.

**Recommendation:** either give `index` its own explicit group (e.g. a single-page "Overview" or "Welcome" group, consistent with how every other page in the tab is grouped), or — better, given it's actually the marketing homepage rather than a help article — reconsider whether it belongs inside the **Help** tab's navigation at all, versus being the un-tabbed site root that a visitor lands on before choosing a tab.

### Issue B — "Industries" is a full peer tab of marketing pages next to "Help" and "Integrations"

`docs.json:64-78`: the **Industries** tab contains only landing-page content — by this repo's own `CLAUDE.md` taxonomy, "Industry" pages are "Vertical landing pages," tone "Industry-aware, practical," explicitly a different content type from "Help" ("Enable successful usage," "Technical, clear, utility"). Confirmed by reading the pages themselves: `industries/civil-construction.mdx` etc. are framed as "QRtub for Civil Construction" pitches, not procedural documentation.

This is not a Mintlify schema violation — tabs can hold anything — but it is a structural smell worth flagging for a docs *and* GEO audit specifically:
- A user or an AI agent navigating by tab label has no signal that **Help** and **Industries** are different kinds of content (support docs vs. marketing verticals) sitting at the same navigational level as **Integrations** (which is itself genuine how-to content — "Guides for connecting QRtub to third-party tools," per `CLAUDE.md`).
- This matters more than usual here because the retrieval audit (`audit/_raw/retrieval-grid-summary.md`) already found the docs corpus prone to confident hallucination on gaps — mixing marketing copy into the same tab-level namespace as procedural help increases the chance an AI answer engine cites an industry pitch page (thin on mechanics, written to persuade) as if it were authoritative product documentation for a how-to question.
- **Compounding, separate issue:** `docs.json:66-77` nests a group literally named `"Industries"` *inside* the `"Industries"` tab (`"tab": "Industries"` → `"group": "Industries"` → pages). This redundant tab/group naming produces a repeated breadcrumb segment ("Industries > Industries > Civil Construction") in the JSON-LD `BreadcrumbList` and in any on-page breadcrumb UI — a small, easily-fixed redundancy. Compare to **Integrations**, which correctly uses a distinct group name (`"Operations"`) under its tab.

**Recommendation:** at minimum, rename the inner group (e.g. `"By Industry"` or split into sub-groups if the list grows) to remove the duplicate breadcrumb segment. The larger question — whether marketing vertical pages belong in the same tab-level structure as support docs at all — is a product/IA decision beyond a docs.json field fix, but worth surfacing given this audit's retrieval-quality angle.

### Minor, non-blocking observations
- No tab (`Help`, `Industries`, `Integrations`) has an `icon` set (`navigation.tabs[].icon` is supported per `raw_organize_settings-reference.md:33-39`) — purely cosmetic, not flagged as a defect.
- Within the **Help** tab, printing-related content is split across two different groups — `help/print-first-workflow` sits in **Concepts** (`docs.json:34-36`) while `help/print-batches` sits in its own single-page **Printing** group (`docs.json:57-60`). This may be intentional (concept vs. feature), but it's worth a second look given both are printing-lifecycle pages a reader might expect to find together.
- **Items & Data** (`docs.json:39-43`) and **Printing** (`docs.json:56-60`) are each single-page groups. Not wrong, just worth noting if the nav grows — a single-page group carries a heading for no grouping benefit yet.

---

## 5. Other unset fields the guidance file flagged as valuable

Beyond the SEO/metadata fields in §2, cross-referencing the full schema map (`audit/_raw/raw_organize_settings-reference.md`, full section list at lines 19-1000) against docs.json's actual keys (`$schema`, `theme`, `name`, `redirects`, `colors`, `favicon`, `navigation`, `logo`, `navbar`, `contextual`, `footer` — 11 top-level keys total):

- **`markdown.instructions`** (absent) — `mintlify-guidance.md` §2: "custom 'Agent Instructions' block appended to every page's Markdown export **and** to `llms.txt` and `llms-full.txt`, after the site title/description." This is a direct, zero-infrastructure way to steer how AI tools use QRtub's docs (e.g. an instruction not to describe unreleased/Planned features, mirroring this repo's own `CLAUDE.md` rule against promising Planned features) — currently unused.
- **`markdown.schema`** (absent, defaults `true`) — not applicable/no action: docs.json has no `api`/`openapi` config, so this OpenAPI-export toggle has nothing to act on. Not a gap.
- **`errors.404`** (absent) — schema reference confirms this section exists (`raw_organize_settings-reference.md:733-739` onward) for custom error-page copy/links; cosmetic, not flagged as a priority gap.
- **`integrations.*`** (entirely absent — no analytics/chat-widget integration configured; `raw_organize_settings-reference.md:964-997` lists ~20 supported providers, e.g. `ga4`, `plausible`, `posthog`, `intercom`, `frontchat`) — out of scope for an AI/SEO-focused docs.json audit (these are analytics/support-widget integrations, not retrieval/indexing features), but worth a one-line mention since the field exists and is entirely unused; no specific recommendation without knowing what analytics stack, if any, QRtub already runs elsewhere.
- **`navigation.tabs[].icon`** — see §4 minor observations.

Nothing else in the schema (`appearance`, `fonts`, `icons`, `background`, `styling`, `thumbnails`, `banner`, `interaction`, `variables`) looks like a meaningful gap for this site's current scale — these are visual/behavioral refinements with no direct AI-retrieval or SEO effect documented in the guidance file.

---

## 6. `footer.branding.enabled: false` and other footer flags

`docs.json:126-130`:
```json
"footer": {
  "branding": {
    "enabled": false
  }
}
```

**This field does not appear in Mintlify's current published schema reference.** I checked both places the schema is documented:
- `audit/_raw/raw_organize_settings-reference.md:581-601` (`### footer`) — documents exactly two sub-fields: `footer.socials` (object, platform→URL) and `footer.links` (array of link columns, max 4). No `footer.branding` anywhere in this section or searchable anywhere in the file.
- `audit/_raw/raw_organize_settings-structure.md:420-478` (`### footer`) — same two sub-fields, same absence, with a full worked example (`socials` + `links`) that does not include `branding`.
- A repo-wide grep of every fetched raw Mintlify page for the string "branding" turns up zero hits describing a `footer.branding` field (the only "branding" hit anywhere in `_raw/` is an unrelated sidebar link title, "Appearance and branding," in the settings-reference quick-reference list at line 13 — a page about themes/colors/logo, not this field).

**This is a genuine "NOT FOUND / NOT DOCUMENTED" case in the guidance file's own terminology (see its §-verdict conventions)** — not a refutation that the field does anything (Mintlify historically has offered a "remove branding" capability tied to paid plans, and unknown/legacy keys are commonly just silently ignored by config parsers rather than erroring), but the currently-published schema reference does not describe `footer.branding` as a supported field, its type, or what plan tier it requires. Two concrete possibilities, neither confirmable from Mintlify's docs alone:
1. It's a legacy/renamed field from an earlier docs.json schema version that no longer does anything (in which case `"enabled": false` is a no-op, and the actual branding-removal control — if one still exists — lives elsewhere, e.g. a dashboard plan setting rather than docs.json).
2. It's a real, currently-functioning field that Mintlify's public schema page simply hasn't caught up to documenting.

**Recommendation:** this needs a live check against the actual deployed site (inspect the rendered footer HTML on help.qrtub.com / a preview deployment for a "Powered by Mintlify" mark, or check the Mintlify dashboard's plan/branding settings) rather than further doc-searching — the guidance file's own closing note makes exactly this point about NOT-FOUND items generally: "the only reliable next step is inspecting live HTTP responses from an actual Mintlify-hosted site... not further searching Mintlify's docs." Do not assume this setting is currently doing what its name implies.

**Other footer observations:**
- `footer.socials` — **unset**. Supported, zero-cost, and QRtub is a real company with (presumably) at least a website/LinkedIn presence; omission isn't wrong but is a missed low-effort addition, and pairs with the `seo.organization.sameAs` gap in §2 (both are "tell search/AI engines who else vouches for this brand" signals).
- `footer.links` — **unset**. Supported (up to 4 columns); no recommendation either way, this is a content/IA decision (e.g. whether qrtub.com's marketing footer should be mirrored here) rather than a technical gap.

---

## Summary — highest-value fixes

1. **Add top-level `description`** (§1) — directly fixes the missing llms.txt blockquote; single string, no other dependency.
2. **Add `seo.organization`** (§2, §6) — corrects the JSON-LD publisher entity to the actual legal name (Teralis Pty Ltd, per this repo's `CLAUDE.md`) and lets `sameAs` corroborate it.
3. **Verify what `footer.branding.enabled` actually does** (§6) — it's set to a value but isn't a documented field; confirm live before trusting it.
4. **Fix the redundant `Industries` tab/group naming** (§4 Issue B) — one-line docs.json change, removes a duplicate breadcrumb segment.
5. **Give `index` its own group, or reconsider its placement in the Help tab** (§4 Issue A) — structural clarity for both breadcrumbs and content-type signaling.
6. **Add `grok`, `aistudio`, `add-mcp` to `contextual.options`** (§3) — documented, free, consistent with the categories already enabled.
7. **Enable `metadata.timestamp`** (§2) — zero-maintenance freshness signal, git-backed repo already supports it.
