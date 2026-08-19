---
title: "robots.txt and .md indexing — live findings for help.qrtub.com"
description: "Live curl results against help.qrtub.com's robots.txt and three page .md variants, assessed against Mintlify's documented default behavior in mintlify-guidance.md."
---

# help.qrtub.com — robots.txt and markdown-variant indexing (live findings)

**Compiled:** 2026-08-19
**Method:** Live `curl` against the production site, cross-checked against `/workspace/mintlify-docs/audit/_raw/mintlify-guidance.md` (§4 and the "SEO meta tag generation" section) and a repo grep for any local customization.

---

## 1. `robots.txt` — full content

```
User-agent: *
Content-Signal: ai-train=yes, search=yes, ai-input=yes
Disallow: /cdn-cgi/
Allow: /_next/image
Disallow: /_next/
Sitemap: https://help.qrtub.com/sitemap.xml
```

Fetched via `curl -s https://help.qrtub.com/robots.txt`.

---

## 2. Is this deliberately configured, or Mintlify's unconfigured default?

**Verdict: unconfigured Mintlify default. Nothing about it is QRtub-specific.**

Evidence:

- **No local override file exists.** The repo root (`/workspace/mintlify-docs/`) contains no `robots.txt`. Per the guidance file's "SEO meta tag generation" section: "A custom `robots.txt`/`sitemap.xml` placed at the project root fully overrides the generated one (and loses the default Content-Signal directives)." Since no such file is present, Mintlify is serving its own auto-generated version, not a project override.
- **No `docs.json` customization.** `grep -in 'seo\|robots\|indexing\|noindex' docs.json` returns zero matches. There is no `seo.indexing`, no `seo.metatags`, nothing in `docs.json` that touches indexing/robots behavior at all.
- **The content itself matches the documented default pattern.** The guidance file (§4 and "SEO meta tag generation") states Mintlify's auto-generated `robots.txt` includes `Content-Signal` directives (per contentsignals.org / Cloudflare's Content Signals Policy) that "opt your documentation in to AI training, search indexing, and AI answer generation" for all user agents by default. The live file's `Content-Signal: ai-train=yes, search=yes, ai-input=yes` under a blanket `User-agent: *` is exactly that default posture, not a hand-written policy.
- **The `Disallow: /cdn-cgi/`, `Allow: /_next/image`, `Disallow: /_next/` lines are platform boilerplate**, not QRtub-authored rules — `/_next/` and `/cdn-cgi/` are Next.js/Vercel/Cloudflare internal paths that appear in default Next.js-hosted robots.txt output generally, not something specific to QRtub's content strategy. No help/industries/integrations path is disallowed; nothing in the file singles out any QRtub page or section.
- **The `Sitemap:` line points to the standard Mintlify-generated `sitemap.xml`** referenced elsewhere in the guidance file, again consistent with the default template rather than a custom entry.

Conclusion: this is Mintlify's stock output for a project with no `seo`/`robots` configuration and no root-level `robots.txt` override. Nothing in this repository customizes it.

---

## 3. `X-Robots-Tag` header on `.md` variants (`Accept: text/markdown`)

Three pages checked with `curl -sI -H "Accept: text/markdown" <url>`:

| URL | HTTP status | `x-robots-tag` |
|---|---|---|
| `https://help.qrtub.com/help/key-concepts` | 200 | `noindex, nofollow` |
| `https://help.qrtub.com/help/app-links` | 200 | `noindex, nofollow` |
| `https://help.qrtub.com/integrations/safetyculture` | 200 | `noindex, nofollow` |

All three markdown responses are served with `content-type: text/markdown; charset=utf-8` (confirming content negotiation worked — Accept header was honored, not ignored) and **all three carry `x-robots-tag: noindex, nofollow`**.

Full relevant headers were identical in shape across all three (only `age`, `date`, `x-vercel-id`, and `content-length` differed):

```
HTTP/2 200
content-disposition: inline
content-security-policy: default-src 'none'
content-type: text/markdown; charset=utf-8
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
server: Vercel
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-llms-txt: /llms.txt
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
x-robots-tag: noindex, nofollow
x-vercel-cache: HIT
```

**Side finding (not asked for, but resolves an open item in the guidance file):** the `Link` header's `rel="api-catalog"` entry (`</.well-known/api-catalog>`) confirms Mintlify *does* now serve a `/.well-known/api-catalog` discovery endpoint on this live site. The guidance file (§3c) flagged this as "NOT FOUND / NOT DOCUMENTED" in Mintlify's public docs as of 2026-08-19 — the live header shows it exists in the product regardless of whether Mintlify's docs describe it. Not otherwise investigated here (content of that endpoint not fetched); flagging for whoever owns that section of the audit.

---

## 4. Definitive answer: are `.md` URLs indexable by search engines by default?

**No — and this matches what was observed live.**

The guidance file is explicit that Mintlify's own public documentation is **silent** on this exact point (§4: "NOT FOUND / NOT DOCUMENTED — this specific claim is not addressed anywhere in Mintlify's published docs, in either direction... None mention an `X-Robots-Tag` HTTP header at all"). So this cannot be answered by citing Mintlify's docs — it has to be answered from the live response, which is what this check did.

**Live result: all three `.md` responses carry `x-robots-tag: noindex, nofollow`.** That header is a direct, unambiguous instruction to compliant search-engine crawlers (Google, Bing, etc.) to exclude the response from their index and not follow its links. So on help.qrtub.com, **the Markdown variant of a page is not indexable — it is actively excluded via `X-Robots-Tag`, independent of whatever indexing status the HTML counterpart page has.**

This is a real, observed behavior that:
- **Is not documented** anywhere in Mintlify's public docs (per the guidance file) — so it should be described in any audit as an *observed live behavior*, not cited to a Mintlify doc page.
- **Sits in apparent tension with `robots.txt`'s permissive `Content-Signal` posture** (`search=yes`, `ai-train=yes`, `ai-input=yes`). That signal is a site-wide/per-user-agent policy about crawling permission; the `X-Robots-Tag` on `.md` responses is a narrower, per-resource directive specifically on the Markdown content-negotiated variant. The two aren't contradictory in a technical sense (a crawler can be "allowed" to fetch a resource under Content-Signal/robots.txt and still be told via `X-Robots-Tag` not to index that particular response) — but the practical upshot is that the **HTML page** is the indexable, search-facing artifact, and the **`.md` variant** exists purely for AI-agent/LLM consumption (as fetched by tools honoring `Accept: text/markdown`, MCP, `llms.txt`/`llms-full.txt` consumers, etc.) and is deliberately kept out of conventional search indexes. That reading is consistent with Mintlify's stated purpose for the `.md` export ("optimized for AI tools and external integrations," per the guidance file §2) as distinct from the HTML page's SEO-oriented meta tags.
- Was consistent across all three pages tested (a help page, an app-links help page, and an integrations page), so this reads as platform-default behavior applied uniformly, not a per-page or per-section setting — nothing in `docs.json` or frontmatter conventions in this repo sets per-page `noindex` differently for these three pages (they're ordinary pages, not flagged `hidden` or `noindex` in their own frontmatter as far as this check covered).

**Bottom line for the audit:** Do not assume — and do not need to assume — whether `.md` URLs are indexable. They are observably **not** indexable by default on this Mintlify deployment (`X-Robots-Tag: noindex, nofollow` on all three sampled pages), even though Mintlify's own documentation doesn't state this anywhere. Treat this finding as sourced to the live curl output above, not to Mintlify's docs.
