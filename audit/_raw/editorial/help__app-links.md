# Editorial Audit: App Links & Fallback URLs

**File:** `/workspace/mintlify-docs/help/app-links.mdx`
**Live:** https://help.qrtub.com/help/app-links
**Nav group:** Pages (`help/pages-overview`, `help/building-a-page`, `help/using-fields`, `help/conditional-visibility`, `help/device-detection`, `help/app-links`)
**Siblings skimmed:** `help/device-detection.mdx`, `help/conditional-visibility.mdx`, `help/key-concepts.mdx`, `integrations/safetyculture.mdx`

Source verified against: `qrtub/src/lib/hooks/useAppLinkHandler.ts`, `qrtub/src/lib/utils/app-link-open.ts`, `qrtub/src/lib/utils/url-helpers.ts`, `qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`, `qrtub/src/components/blocks/AppLinkFallbackWarning/AppLinkFallbackWarning.tsx`, `qrtub/src/components/page/AppLink.tsx`, `qrtub/src/components/page/{Button,Link,ActionLink}.tsx`, `qrtub/src/lib/page/destination-resolver.ts`, `qrtub/src/lib/page/public-link-resolution.tsx`, `qrtub/src/lib/page/bindings.ts`.

---

## 1. Self-containment

A cold reader landing on this page (no link-following) can mostly configure a fallback URL, but three things would leave them either stuck or wrong:

- **Undefined vocabulary.** The page uses "Destination editor," "Link settings," "Item editor," and "Item" throughout (e.g. "In the Destination editor:" line 43; "In the Link settings" in the table at line 92) without ever defining what a Destination, Link, Item, or Page is. `help/key-concepts.mdx` defines all four (`## The Three-Entity Model`, `## Destinations`) but is only reachable via the "Related" list at the very bottom (line 138), after the reader has already been asked to act on these terms. A reader who has never opened `key-concepts` cannot locate "the Destination editor" from this page alone.
- **The two different failure experiences are never distinguished — this is the biggest gap.** App links render through two different code paths depending on how the Destination is used, and they produce visibly different results for the exact same Fallback Message setting:
  - **Single-Destination Item (Direct Mode)**: the scan server-redirects to `AppLinkOpener` (`qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`), a full-page component. It shows a spinner ("Opening app..."), and on timeout a branded card with the fallback message (or a default "App not available" heading + explanatory copy) plus **"Try Again" and "Go Back" buttons**.
  - **A Destination used as a button/link on a Page** (`ActionLink`, `Button`, `Link` in `qrtub/src/components/page/`, all wrapping `AppLink.tsx` → `useAppLinkHandler.ts`): on timeout, `useAppLinkHandler.ts` line 36 runs `if (fallbackMessage) alert(fallbackMessage);` — a plain **native browser `alert()` popup**, not a styled panel, and there is no "Try Again"/"Go Back" affordance at all.

  The page's own framing — "When someone without the app scans, they see your message rather than a confusing blank screen" (line 63) — is true for the Direct-Mode/single-Destination case and materially misleading for the Page/multi-Destination case, where "your message" is a bare OS `alert()` dialog. A support agent answering "what will my customer see if they don't have the app" cannot give a correct answer from this page alone without knowing which mode the reader is using.
- **Binding-failure behaviour is unstated and would produce a wrong answer if asked.** Section 6 ("Using Bindings in Fallback URLs") promises "Configure both once. Deploy to 500 items. Each scan routes to the right app screen (or web equivalent) for that specific item" (line 83). Verified in `qrtub/src/lib/page/bindings.ts` (`resolveBindingsForUrl`, lines 404-429): if a bound field evaluates to `undefined`, `null`, or `""` for a given item, the **entire URL** (not just that segment) is marked unresolved. For the primary Destination URL this means the rule is skipped entirely (`destination-resolver.ts` lines 58-61); for a Fallback URL, `resolveFallbackUrl` (`destination-resolver.ts` lines 107-111) discards the whole fallback and falls through to the next level (Link → Item) or the default "App not available" message. So an item missing the bound field does **not** get a URL with a blank placeholder — it silently gets no fallback (or no destination) at all. The page's "no per-item configuration" claim is true only when every item has that field populated; it does not say what happens when one doesn't.

## 2. Answer-first (every H2, quoted verbatim)

| H2 | Opening sentence(s) as written | Verdict |
|---|---|---|
| What Are App Links? | "App links are URLs that use a custom scheme instead of `https://`. When a device recognises the scheme, it opens the corresponding app." | **Answer-first.** Direct, ~27 words, no preamble. |
| The Problem: App Not Installed | "When someone scans a QR code and taps a Destination with an app link:" *(followed immediately by a two-item bullet list)* | **Preamble.** The heading poses a problem; the opening line is a colon-terminated fragment that sets up a list rather than stating in prose what the problem actually is. A retrieval snippet that stops at the first sentence learns nothing. |
| How QRtub Handles App Links | "QRtub detects when a Destination URL is an app link (any non-`https://` scheme). At scan time:" *(then a 4-step numbered list)* | **Partial.** First sentence is a real, direct answer (detection mechanism). The second sentence is again a colon fragment leading into a list rather than a self-contained 40-60 word answer — a snippet retrieved as "first sentence only" undersells the mechanism (misses the timer/fallback behaviour entirely). |
| Setting a Fallback URL | "When you add or edit a Destination with an app link URL, QRtub automatically shows a fallback configuration panel." | **Mostly answer-first** but thin (19 words) — describes when the UI appears, not yet how to set the value; the actual "how" is in the numbered list that follows. |
| Setting a Custom Fallback Message | "If there's no web equivalent to fall back to, set a **Fallback Message** instead of a URL." | **Answer-first.** Direct, 17 words. |
| Using Bindings in Fallback URLs | "Fallback URLs support `{{item.field}}` bindings — the same way Destination URLs can include Item data automatically." | **Answer-first.** Direct. |
| Where Fallback Settings Live | "Fallback URL and Fallback Message can be set at three levels:" *(then a 3-row table)* | **Borderline.** Correctly answers "where" but as a colon fragment into a table rather than a sentence; acceptable because the table itself is the answer, but there's no prose fallback if a retrieval system strips tables. |
| Common Examples | *(no lead sentence — jumps straight to `### Mitti (iAuditor)`)* | **N/A / preamble by omission.** This H2 has zero prose of its own; it's a pure grouping header for three H3s. Not wrong, but it means the H2 itself answers nothing if retrieved alone. |
| App Links vs Device Detection | "These two features are complementary, not alternatives:" *(then a 2-row comparison table)* | **Answer-first**, if terse (8 words) — it directly resolves the "vs" framing in the heading before the table elaborates. |

## 3. One question per page

This page is scoped to a single coherent task — "how do I make an app-link Destination survive the app-not-installed case" — and covers it end to end (URL fallback, message fallback, bindings, the three-level hierarchy, and how it interacts with device detection). That's an appropriate single-page scope; **no split is recommended**.

It is not too thin to stand alone either — at ~140 lines with multiple worked examples it's a legitimate standalone chunk, not a stub that should be folded into `key-concepts.mdx` or `device-detection.mdx`.

The one internal redundancy worth naming: `integrations/safetyculture.mdx` has its own section "## App Not Installed? Set a Fallback URL" (lines 129-150) that re-explains the same 2.5-second-timeout mechanism and links back here with "See [App Links & Fallback URLs](/help/app-links) for full details." That's a defensible product-specific worked example, not true duplication — it stays.

## 4. Headings as questions

| Current heading | Proposed question form | Why (or why not) |
|---|---|---|
| What Are App Links? | *(keep as-is)* | Already effectively a question. |
| The Problem: App Not Installed | **What happens when the app isn't installed?** | Converts a labelled-problem heading into the actual implicit question, and matches how a support agent or searcher would phrase it. |
| How QRtub Handles App Links | **What happens, step by step, when someone taps an app link?** | Slightly clearer that this section is the mechanism/timeline, not a settings guide. |
| Setting a Fallback URL | **How do I set a Fallback URL?** | Matches "how do I..." phrasing readers/agents actually search. |
| Setting a Custom Fallback Message | **How do I set a custom Fallback Message?** | Same reasoning. |
| Using Bindings in Fallback URLs | **Can Fallback URLs use field bindings like `{{item.field}}`?** | The current noun phrase reads as a feature list; the question form matches how someone would ask before finding this section. |
| Where Fallback Settings Live | **Where can I set Fallback URL and Fallback Message?** | Minimal change, clarifies it answers "where," not "what are the settings." |
| Common Examples | *(leave as noun phrase)* | This is a worked-examples index, not an implicit question — forcing a question form here would be artificial. |
| App Links vs Device Detection | **When should I use App Links instead of Device Detection?** | The "vs" framing reads as a comparison-table heading; the question form signals it also tells you which to reach for. |

## 5. Edge cases / limits / failure modes

Explicit gaps found (each verified against source, not asserted from behavior guesses):

1. **No mention that a Fallback Message renders as a native browser `alert()`, not a styled page, when the Destination is used as a Page button/link** (`useAppLinkHandler.ts` line 36: `if (fallbackMessage) alert(fallbackMessage);`). This is a real UX detail a reader configuring a Page full of Destinations would want to know before writing message copy (an `alert()` has no styling, no line breaks beyond `\n`, and blocks the page until dismissed).
2. **No mention of the "Try Again" / "Go Back" recovery UI** shown on the branded failure screen for the single-Destination/Direct-Mode path (`AppLinkOpener.tsx` lines 43-58). The page describes only the message/URL outcome, not what the screen looks like or that it offers retry.
3. **Binding-resolution failure is undocumented** (see §1 above, `bindings.ts` `resolveBindingsForUrl`): an empty/missing bound field doesn't degrade to a URL with a blank segment — it silently drops the entire fallback (or destination rule) and falls through to the next level or the default message. Nothing on the page — or in "Using Bindings in Fallback URLs" specifically, where this matters most — says what happens when the field is empty for some items.
4. **The "flaky scan" timer-suspension nuance is unstated.** `app-link-open.ts` deliberately does *not* fire the fallback if the tab was hidden or the timer was suspended for far longer than the timeout (mobile browsers pause JS timers while the native app is foregrounded) — this is exactly the mechanism that prevents a returning user from being wrongly bounced to the fallback. The page's step 4 ("If the page is still visible after 2.5 seconds — the app wasn't installed") is *directionally* correct but omits the actual safeguard, which matters if a reader is troubleshooting "the fallback fired even though the app opened."
5. **No plan/tier statement.** The page never says whether App Links / Fallback URLs require a specific QRtub plan. Checked `qrtub/src/lib/stripe-plans.ts` and found no gating tied to app-link or fallback fields — this appears to be a base capability of Destinations, not a premium feature — but the page should say so explicitly rather than leaving the reader to infer it, since silence on tier is exactly where an AI agent guesses.
6. **`tel:` links are also "app links" by this mechanism** (`isAppLink()` in `url-helpers.ts` classifies anything that isn't `http:`/`https:`, which includes `tel:`) but the page's three examples (lines 13-15) are all custom-scheme mobile apps. A reader with a `tel:` Destination wondering "does the 2.5-second fallback timer apply to phone-call links too?" gets no answer either way.
7. **No stated ceiling on URL length**, even though the sibling `integrations/safetyculture.mdx` page states "Very long deep links may not work consistently" and "keep it under ~2000 characters" in its own troubleshooting section. This page, which is the canonical fallback/app-link reference, states no such limit.
8. **No mention of what a desktop scan/click does.** Device-detection.mdx explicitly recommends combining App Links with Device Detection to route app-store downloads by OS, implying a plain app-link Destination with no device condition will also fire (and presumably fail) on desktop — but app-links.mdx never states what a desktop visitor sees when they hit a bare app-link Destination with no Device Detection layered on.

## 6. Chunk integrity (each H2 read in isolation)

- **What Are App Links?** — Self-contained. Fine on its own.
- **The Problem: App Not Installed** — Self-contained; doesn't depend on prior prose, though it does presuppose the reader already knows what a "Destination" is (see §1).
- **How QRtub Handles App Links** — Self-contained mechanism description; fine alone.
- **Setting a Fallback URL** — Self-contained procedurally, but assumes "the Destination editor" is a known, locatable UI surface (see §1 gap).
- **Setting a Custom Fallback Message** — Self-contained.
- **Using Bindings in Fallback URLs** — Self-contained; the example is fully inline (app link + fallback URL both shown), doesn't lean on the earlier Mitti example.
- **Where Fallback Settings Live** — Self-contained table; reads fine alone.
- **Common Examples** — The parent H2 has no prose, so isolating it alone yields *nothing* — only the H3 children below it carry content. If a retrieval system chunks by H2 and swallows H3s underneath, this is fine; if it treats "Common Examples" as its own chunk boundary excluding children, the H2 in isolation is empty.
- **App Links vs Device Detection** — Mostly self-contained, but the comparison table's first row reads **"App not installed | Fallback URL (this page)"** (line 128) and the closing paragraph says "For Mitti specifically..." — the phrase **"(this page)"** is a deictic reference that breaks if this section is retrieved in isolation from the page title/URL (a chatbot quoting just this table would show "(this page)" with no antecedent). Should read "Fallback URL — see above" or name the mechanism directly instead of pointing at the container document.
- **Related** — Standard footer link list; not a content section, no isolation issue.

---

### Summary

The page is accurate everywhere it commits to a specific, checkable claim (the 2.5-second timeout, the binding syntax, the three-level fallback priority order, the Mitti example) — no factual errors found in what it *does* say. The defects are almost entirely omissions: two different failure UIs treated as one, a binding-failure mode that silently produces "no fallback" rather than "url with blank," an undefined-terms self-containment gap shared with the rest of the Pages group, and a couple of preamble/colon-fragment H2 openers. These are substantive enough (particularly #2 in self-containment and #1/#3 in edge cases) to warrant a rewrite rather than a line edit — see `audit/proposed/help__app-links.md`.
