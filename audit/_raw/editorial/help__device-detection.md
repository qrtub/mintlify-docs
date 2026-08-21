# Editorial Audit: Device Detection & Routing

**File:** `/workspace/mintlify-docs/help/device-detection.mdx`
**Live:** https://help.qrtub.com/help/device-detection
**Nav group:** Pages (`help/pages-overview`, `help/building-a-page`, `help/using-fields`,
`help/conditional-visibility`, `help/device-detection`, `help/app-links`)
**Siblings skimmed:** `help/conditional-visibility.mdx`, `help/using-fields.mdx`,
`help/building-a-page.mdx`, `help/pages-overview.mdx`, `help/app-links.mdx`
**Source verified against:** `qrtub/src/lib/utils/device-detection.ts`,
`qrtub/src/lib/page/destination-resolver.ts`, `qrtub/src/lib/page/bindings.ts`,
`qrtub/src/components/blocks/DestinationConfiguration/DestinationConfiguration.tsx`,
`qrtub/src/components/blocks/DestinationRulesEditor/DestinationRulesEditor.tsx`,
`qrtub/src/components/blocks/AddEditItemForm/AddEditItemForm.tsx`,
`qrtub/src/lib/types/landing-page-config.ts`, `qrtub/src/lib/stripe-plans.ts`

## Headline finding

The page documents only one of the two shipped mechanisms for routing by device, and it is
arguably the more complicated one for most of its own use cases. There is a second, simpler,
fully shipped mechanism — **per-Item "Conditional Rules" on a single Destination** (a checkbox
labelled "Enable conditional routing", a "When… Then…" rule list evaluated first-match-wins,
an "Else" connector between rules, and a "Default URL" fallback, all inside the Item's
"Destination Link" radio option, on the Item's **Destination** tab, in
`AddEditItemForm`) — that the page never mentions. It requires no
Page at all (works in Direct Mode) and is a closer match to nearly every scenario the page
walks through (mobile app vs. web app, app-store routing, the Mitti iOS Safari workaround),
which are all "one silent redirect that differs by device," not "show the visitor two buttons
and let them pick." This is detailed under Self-Containment and Edge Cases below, and is why a
rewrite is proposed.

---

## 1. SELF-CONTAINMENT

A reader landing here cold, unable to follow a link, could correctly write CEL device
conditions (the field reference is accurate and complete) but **could not actually complete
either of the two real setup paths**, for these concrete reasons:

- **The page never names the UI control it tells you to use.** Every use case says "Set
  condition: `device.isMobile`" or "Condition: `device.isIOS`" as if "condition" were a
  labelled field. The actual control, per `help/building-a-page.mdx`, is: *"Every section has
  a **Visibility** setting that takes a condition"* (line 89). A cold reader has no way to know
  they're looking for a field called **Visibility** in the page editor's Properties panel — the
  word "Visibility" does not appear anywhere in `device-detection.mdx`.

- **Missing prerequisite: Pages must be switched on for the Tub.** Every "Setup" block in this
  page assumes multiple Destinations exist on a Page, but per `help/building-a-page.mdx`:
  *"Pages must be switched on for the Tub. Open the Tub's settings and turn on the page option
  (labelled **Show a profile page** in the current app). If it is off, scans go straight to a
  single Destination and there is no page to edit."* This page never says that, so a reader
  starting from "Create a 'Mobile App' Destination" has nowhere to click — there is no page
  to add a second Destination to yet.

- **An entire, simpler, shipped mechanism is missing.** Verified in
  `DestinationConfiguration.tsx` and `DestinationRulesEditor.tsx`: on an Item's own
  **Destination** tab (verified label in `AddEditItemForm.tsx`'s `tabs` array: `{ id:
  'destination', label: 'Destination' }`), with the **Destination Link** radio option selected
  (Direct Mode — no Page required), there is a **"Conditional Rules"** editor:
  a checkbox "Enable conditional routing," an "Add Rule" button that adds a `When [condition]
  → Then [redirect to URL]` pair (each rule can also carry its own app-link Fallback URL/Message
  and an optional Label), rules connected by an "Else" divider and badged "First match wins,"
  and a "Default URL (fallback when no rules match)" field below. This is the mechanism a
  single QR code would actually use to *silently* redirect a mobile visitor to the app and a
  desktop visitor to the web app — no visible second button, no Page needed. The current page's
  own use cases ("Mobile App vs Web App," "Platform-Specific App Downloads," the Mitti Safari
  workaround) all describe exactly this shape of problem, yet the page only ever tells the
  reader to build it with two visible Page Destinations plus per-Destination Visibility. That's
  a valid alternative (see next point) but presenting it as the *only* path is a real gap.

- **No mention of the practical trade-off between the two mechanisms.** Page-mode Destinations
  are Tub-wide — per `help/building-a-page.mdx`, "You build one layout for a Tub, and every Item
  in that Tub renders it with its own data" — so one Visibility condition covers every Item.
  The Conditional Rules editor, by contrast, is **per-Item** (it only appears in
  `AddEditItemForm`, not in any Tub-level template or bulk editor) — verified by grepping
  `DestinationConfiguration`/`DestinationRulesEditor` usage across the whole `src/components`
  tree; the only import site is `AddEditItemForm.tsx`. A reader with hundreds of items who wants
  every one of them to silently redirect by device would need to configure Conditional Rules on
  each Item individually, whereas the same outcome via a Page Visibility condition is set once.
  This is exactly the kind of ceiling an AI support agent needs and currently has no way to
  state correctly.

- **No plan-tier statement.** Checked `qrtub/src/lib/stripe-plans.ts`: device fields,
  conditional visibility, and the landing-page editor are not gated by plan at all — Starter
  already includes "Drag and drop landing page editor," and neither `DestinationConfiguration`
  nor `DestinationRulesEditor` reference plan/feature-flag checks anywhere. The page never
  says this is available on every plan, so an AI agent has nothing to cite when a user asks "do
  I need to upgrade for this?" and may guess wrong in either direction.

**Verdict:** Not self-contained. The field reference alone (device.type/os/browser + flags) is
accurate and complete, but the actual "how do I make this happen" instructions are incomplete
in a way that would leave a cold reader stuck at the first step.

---

## 2. ANSWER-FIRST

Quoting the literal opening text of every H2, in document order:

- **"## Why Device Detection Matters"** opens: *"Different devices need different
  experiences:"* — a one-line lead-in to a bullet list, not itself an answer to "why does this
  matter." No standalone 40-60 word answer exists; you must read the four bullets plus the
  closing line to get the actual point. **Preamble, not answer-first.**

- **"## Common Use Cases"** has **zero lead sentence** — the H2 goes directly to `### 1. Mobile
  App vs Web App`. There is nothing here that answers any question at the H2 level at all.
  **Empty opener** — worse than preamble.

- **"## Available Device Information"** also has **zero lead sentence** — goes straight to
  `### Device Type` / `**Field:** \`device.type\``. Same defect as above.

- **"## How to Set Up Device Routing"** opens directly with the numbered list: *"1. **Create
  your Destinations** - One for each device scenario"*. This is reasonably answer-first for a
  procedural heading (the reader gets the actionable step immediately) but it is three short
  fragments, not a 40-60 word framing of what the procedure assumes (a Page must already exist,
  Destinations means Page sections) — so it answers "what do I do" but not "what does this
  presuppose."

- **"## Combining Device Detection with Item Data"** opens: *"You can combine device detection
  with Item fields for powerful routing:"* — this is the one H2 on the page that genuinely
  answers its own implied question ("can I combine these?") in a single direct sentence, though
  at 11 words it's well under the 40-60 word target and gives no mechanism detail before the
  example.

- **"## Important Notes"** opens directly into a bolded sub-label: *"**User-Agent
  Detection:**\nDevice detection uses browser User-Agent strings. These are generally reliable
  but:"* — no topic sentence for the H2 itself; the heading is a grab-bag (see Heading and Chunk
  Integrity sections below) so there is no single question it could answer-first.

- **"## Testing Your Setup"** opens: *"Before deploying QR codes with device routing:"* —
  preamble before the numbered list, not an answer.

- **"## When NOT to Use Device Detection"** opens: *"**Don't use device detection for:**"* —
  functionally this IS the answer (a direct bulleted list of the "don't" cases immediately),
  so this is the second-best-performing H2 on the page for answer-first structure, just not
  written as flowing prose.

- **"## Related"** — a links list, not subject to answer-first (as expected for a Related
  section).

**Verdict:** 1 of 8 content H2s (arguably 2, counting "When NOT to Use") opens with something
that functions as a direct answer; the rest open with scene-setting fragments or, in two cases,
nothing at all before dropping into subheadings. This will read fine to a human scanning
top-to-bottom, but a chunk-retrieval system handing an LLM just the H2 heading + its first
sentence gets little to nothing to work with for "Common Use Cases" and "Available Device
Information" specifically.

---

## 3. ONE QUESTION PER PAGE

This page is not thin — if anything it's carrying more than one distinct kind of content:

1. **A reference table** (device.type / device.os / device.browser, values, convenience flags,
   examples) under "## Available Device Information."
2. **A procedural task** ("how do I make different destinations show for different devices").
3. **A judgment/decision question** ("should I even use this feature").
4. **A testing checklist.**

The reference table (#1) is a genuine, near-verbatim triplicate across three sibling pages:

- `help/using-fields.mdx` has a complete **Device Fields** table (lines 107-122) with the exact
  same fields, types, and example values, plus its own pointer: *"See Device Detection for
  detailed device routing examples."*
- `help/conditional-visibility.mdx` has its own **"### Quick Device Field Reference"**
  (lines 167-191) — restating `device.type`/`device.os`/`device.browser` and every convenience
  flag again, with nearly identical example snippets (`device.type == "mobile"`,
  `device.os == "ios"`, `device.browser == "safari"`), immediately after telling the reader
  *"See [Using Fields](/help/using-fields) for complete field reference..."* — i.e., it points
  to the canonical source and then restates the table anyway.
- `device-detection.mdx` restates it a third time under "## Available Device Information,"
  including the same three example blocks per field.

This directly contradicts the project's own stated content strategy (`mintlify-docs/CLAUDE.md`):
*"Search for existing information before adding new content. Avoid duplication unless
strategic."* `using-fields.mdx` is clearly positioned as the canonical field reference (it also
covers Item/Tub/Session/Theme fields in the same table format) — the other two pages copying
its Device Fields section is unstrategic duplication, not intentional redundancy.

**Proposed split:**
- Keep `using-fields.mdx` as the single canonical source for field names/types/values (it
  already is, and already covers every field family, not just device).
- Shrink `device-detection.mdx`'s "## Available Device Information" to a two-or-three line
  summary (just enough to make the page's own examples legible without a click) plus a link to
  `using-fields.mdx#device-fields` for the full table — matching the "one source, referenced —
  not duplicated" principle `CLAUDE.md` states for code facts, applied here to field references.
  Do not delete it outright: the answer-first and chunk-integrity checks above depend on some
  device-field context surviving in this page for standalone retrieval, so trim rather than
  cut.
- Shrink `conditional-visibility.mdx`'s "### Quick Device Field Reference" the same way (out of
  scope for this single-page audit, but noted since it's the same defect against the same
  canonical source).

The procedural, judgment, and testing content (#2-#4) are legitimately one task ("route by
device") and its natural sub-questions (how, when to, how to verify) — they don't need
splitting into separate pages. The page is not too thin to stand alone.

**Verdict:** Split the reference table out (de-duplicate against `using-fields.mdx`); keep the
procedural/decision/testing content together as currently scoped.

---

## 4. HEADINGS AS QUESTIONS

Only proposing rewrites where the question form is a genuine retrieval/clarity improvement —
several headings are already effectively questions and are left alone.

| Current heading | Verdict |
|---|---|
| Why Device Detection Matters | Keep — already frames a rationale; a question form ("Why does device detection matter?") is not clearer. |
| **Common Use Cases** | Rewrite → **"When would I use device-based routing?"** — the section is entirely decision-support (five scenarios), and a question heading signals that intent for retrieval better than a generic label. |
| **Available Device Information** | Rewrite → **"What device information can I use in conditions?"** — directly matches the content, and pairs naturally with the `using-fields.mdx` pointer proposed above. |
| **Device Type** / **Operating System** / **Browser** (H3s) | Rewrite → **"What values does `device.type` return?"** / **"What values does `device.os` return?"** / **"What values does `device.browser` return?"** — each section is answering exactly this question and nothing else; the question form is unambiguous where "Device Type" alone could be mistaken for a settings/config topic. |
| How to Set Up Device Routing | Minor tighten → **"How do I set up device-based routing?"** — already question-shaped ("How to..."), low-priority polish only. |
| Combining Device Detection with Item Data | Rewrite → **"Can I combine device detection with Item fields?"** — matches the section's own opening sentence ("You can combine...") and is a genuinely common query shape. |
| **Important Notes** | This is the worst-performing heading on the page for retrieval — it is a grab-bag covering four unrelated points (UA reliability, security posture, fallback defaults, "always have an escape hatch"). Split into named questions instead of one vague H2: **"How reliable is User-Agent detection?"**, **"Can I use device detection to restrict access?"**, **"What happens if QRtub can't detect the device?"**, **"Should every Page always have a fallback Destination?"** A vague heading here is precisely where an AI agent retrieving "Important Notes" alone has no idea what's inside without reading the whole section — the heading itself carries none of the section's four topics. |
| Testing Your Setup | Rewrite → **"How do I test device routing before deploying?"** — the section is a checklist answering exactly this. |
| When NOT to Use Device Detection | Keep — already effectively a question in imperative form; no clearer question-form rewrite available. |
| Related | N/A — not subject to this heuristic. |

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

**What the page already states correctly** (verified against `device-detection.ts`):
- User-Agent can be spoofed or masked by privacy browsers (page: *"Users can modify their
  User-Agent (rare)"* / *"Some privacy browsers mask device info"*) — accurate framing.
- "New devices/browsers might be detected as `'unknown'`" — accurate; `parseUserAgent` defaults
  `os`/`browser` to `'unknown'` when no regex matches.
- Fallback behaviour when detection fails — *"Type: `desktop`, OS: `unknown`, Browser:
  `unknown`"* — verified exactly against both `getDeviceInfoFromNavigator`'s no-`window` branch
  and `parseUserAgent`'s default `type = 'desktop'`.
- "Not for Security" warning — correct and appropriately blunt; nothing in
  `destination-resolver.ts` or `bindings.ts` treats device fields as an auth mechanism.
- "Always Provide Options" (a fallback Destination with no condition) — good, matches the
  `defaultLink`/"Default URL" fallback pattern that actually exists in the resolver.

**Gaps — stated with no evidence, or missing outright:**

1. **No plan-tier statement**, as detailed in Self-Containment §1. Verified not gated in
   `stripe-plans.ts` — worth stating explicitly ("available on every QRtub plan") precisely
   because *silence* here is what causes an AI support agent to invent a tier requirement.

2. **The entire per-Item "Conditional Rules" mechanism is absent** — see Self-Containment §1.
   This is the single biggest capability gap on the page.

3. **Silent-false condition typos are not mentioned.** `bindings.ts`'s `evaluateCondition` is
   the same evaluator behind every condition on this page (Page Visibility and Conditional
   Rules alike), and per this repo's own `qrtub/CLAUDE.md`: *"An undefined identifier makes the
   whole condition evaluate to `false` silently... An invented value ... produces a rule that
   never fires and no error message."* A typo like `device.isMoblie` will not error anywhere in
   the UI the reader is likely to notice quickly — the Destination just never matches. The page
   never warns that a bad device condition fails **silently**, which is exactly the class of bug
   an AI support agent would otherwise have to guess at ("did you spell the field right?" is not
   something it can suggest confidently without this being documented).

4. **Tablet misdetection on iPad is a real, silent gotcha the regex itself predicts** — verified
   directly in `device-detection.ts`: `isTabletUA = /ipad|android(?!.*mobile)|tablet/` depends on
   the literal substring `"ipad"` appearing in the User-Agent. iPads running Safari with
   "Request Desktop Website" (the platform default in recent iPadOS versions) send a
   Mac-style User-Agent with no `"ipad"` token, so such a device is classified `type: 'desktop'`,
   `os: 'macos'` — not `tablet`/`ios`. The page's "Tablet-Optimised Experience" use case and its
   "Fallback Behaviour" note both talk about *undetectable* devices, but never about a device
   that *is* detected, just detected as the wrong bucket. This is worth a one-line caveat.

5. **Bots, crawlers, and link-preview unfurlers are not mentioned as a source of `'unknown'`/
   `'desktop'` results.** When Slack, iMessage, or a search-engine crawler fetches the link
   server-side to build a preview, its User-Agent won't match any of the mobile/tablet/OS/
   browser regexes, so it resolves to the same "desktop, unknown OS, unknown browser" bucket as
   genuine undetectable traffic. This is a common real-world support question ("why did the
   Slack preview show the desktop version?") that the current "Fallback Behaviour" note doesn't
   anticipate.

6. **No mention that device context is re-evaluated on every scan, not cached per QR code or
   per visitor.** Minor, but worth one sentence so a reader doesn't wonder whether switching
   phones changes the outcome (it does, immediately — verified: `getDeviceInfoFromHeaders` reads
   the request's own `user-agent` header fresh on every hit in `destination-resolver.ts`).

---

## 6. CHUNK INTEGRITY

Testing each H2 (and, where a chunker would split at H3, each H3) in isolation, no surrounding
page:

- **"## Why Device Detection Matters"** — self-contained. No dangling references.
- **"## Common Use Cases"** (H2 alone, no children) — **broken in isolation**: there is no text
  at all between the heading and the first `### 1.` subheading, so a chunk containing only this
  H2's "own" text would be empty. If a chunker instead retrieves individual `###` items:
  - `### 1. Mobile App vs Web App` — self-contained.
  - `### 2. Platform-Specific App Downloads` — self-contained.
  - `### 3. iOS Safari Deep Link Workaround (Mitti Example)` — self-contained; the one internal
    cross-reference ("the [Fallback URL feature](/help/app-links) handles that automatically")
    is an explicit link, not a bare "see above," so it survives isolation fine.
  - `### 4. Tablet-Optimised Experience` — self-contained.
  - `### 5. Browser-Specific Features` — self-contained.
- **"## Available Device Information"** (H2 alone) — same defect as "Common Use Cases": zero
  lead text, straight into `### Device Type`. Each child H3 (`Device Type`, `Operating System`,
  `Browser`) is, individually, fully self-contained (Field / Values / Convenience flags /
  Examples, no forward or backward references).
- **"## How to Set Up Device Routing"** — self-contained; the ASCII "Example Page" tree doesn't
  reference anything defined earlier in the document.
- **"## Combining Device Detection with Item Data"** — self-contained.
- **"## Important Notes"** — the *content* is self-contained sentence-by-sentence (no "as
  discussed above" or "this" with an unclear antecedent), but the *heading* gives zero signal
  about what's inside (see Heading section) — a retrieval system matching on heading text alone
  would never surface this section for "what happens if a phone can't be detected" or "is this
  secure," even though both answers live here.
- **"## Testing Your Setup"** — self-contained; closing line ("Scan the same QR code from each
  device...") doesn't depend on anything earlier.
- **"## When NOT to Use Device Detection"** — self-contained.
- **"## Related"** — a links list; not applicable to this check.

**Verdict:** No pronoun/"as above"-style breakage anywhere on the page — that's a genuine
strength. The only chunk-integrity defects are the two H2s ("Common Use Cases," "Available
Device Information") that carry no text of their own before dropping into subheadings, which
matters only if a chunker ever retrieves an H2 span without its children.

---

## Summary of required changes

1. Add the missing per-Item **Conditional Rules** mechanism (checkbox, rule list, Else/first-
   match-wins, Default URL) as a documented path — verified in
   `DestinationRulesEditor.tsx`/`DestinationConfiguration.tsx`.
2. Name the actual UI control ("Visibility") wherever the page currently says "condition:".
3. State the Page-mode prerequisite ("Pages must be switched on for the Tub").
4. State the Tub-wide vs. per-Item scope difference between the two mechanisms.
5. State plan availability explicitly (not gated, verified against `stripe-plans.ts`).
6. Add the four missing edge cases: silent-false condition typos, iPad-as-desktop
   misclassification, bot/crawler unfurler traffic, per-scan (not cached) evaluation.
7. Trim "## Available Device Information" against the `using-fields.mdx` canonical table.
8. Rewrite the flagged headings, especially splitting "Important Notes" into named questions.
9. Give every H2 (especially "Common Use Cases" and "Available Device Information") a real
   lead sentence before its children.

Given the scope of #1-#4 in particular (a missing shipped mechanism, not a wording nit), a full
replacement draft has been written to
`/workspace/mintlify-docs/audit/proposed/help__device-detection.md`.
