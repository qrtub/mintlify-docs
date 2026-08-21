# Editorial Audit — /help/using-fields

**File:** `/workspace/mintlify-docs/help/using-fields.mdx`
**Live:** https://help.qrtub.com/help/using-fields
**Nav group:** Help → Pages (siblings: `help/pages-overview`, `help/building-a-page`,
`help/conditional-visibility`, `help/device-detection`)

Verified against `/workspace/qrtub/src/lib/page/bindings.ts`, `context.ts`,
`destination-resolver.ts`, `public-link-page.tsx`, `/workspace/qrtub/src/lib/editor/property-schemas.ts`,
`/workspace/qrtub/src/lib/types/field-config.ts`, `/workspace/qrtub/src/lib/types/destination-config.ts`,
`/workspace/qrtub/src/lib/stripe-plans.ts`, and the `things` table migration
(`supabase/migrations/20250718000002_complete_schema.sql`).

---

## 1. SELF-CONTAINMENT

**Verdict: Mostly — a reader can write a working `{{item.field}}` URL template or a
condition from this page alone — but several real, verifiable gaps would produce wrong
answers or broken Destinations, and one core term is used throughout without ever being
defined.**

**"Destination" is never defined on this page.** The very first H3 says "Use `{{field.name}}`
syntax to insert field values into **Destination URLs**" (line 12), and the word
"Destination(s)" recurs in nearly every example and pattern (`## Common Patterns` alone uses
it 12+ times: "Destination: 'Forklift Inspection'", etc.) — but the page never states what a
Destination *is*. A cold reader who hasn't already read `/help/pages-overview` has no way to
know whether "Destination" means a button, a redirect rule, or something else. This is a
page-wide dependency, not a one-off.

**Missing field categories that are real and in production use.** The `## Available Fields`
section documents Item, Tub, Session, Device, and Theme fields — but omits two entire
categories that exist in the same context object and are resolved the same way:

- **Time fields** (`time.hour`, `time.dayOfWeek`, `time.dayOfMonth`, `time.month`,
  `time.year`, `time.isWeekend`) — defined in `TIME_PROPERTIES`
  (`qrtub/src/lib/editor/property-schemas.ts:233-264`), exposed in the visual editor's field
  picker, and *always* added to the destination-resolution context: `buildDestinationContext`
  in `destination-resolver.ts` unconditionally sets `context.time = buildTimeInfo()` (line 284
  — "Always add time info").
- **Request fields** (`request.country`, `request.city`, `request.language`, `request.ip`,
  `request.path`, `request.referrer`, `request.timestamp`) — defined in `REQUEST_PROPERTIES`
  (`property-schemas.ts:267-303`) and added to the same context whenever headers are available
  (`destination-resolver.ts:280`).

This isn't a hypothetical omission — the sibling page `/help/building-a-page.mdx` explicitly
sends readers here for it: *"Beyond Item and Tub fields, you can bind device and time
information — see [Using Fields](/help/using-fields) for the full reference."* (line 81-82).
That promise is broken; time information is not in this "full reference" at all, and an AI
agent asked "can I route by country" or "can I restrict a Destination to business hours"
would, working from this page alone, wrongly say no.

**The "Standard Item Fields" table doesn't distinguish guaranteed fields from optional ones.**
Per `qrtub/src/lib/types/field-config.ts`:
- `CORE_FIELDS = ['name', 'item_id', 'description', 'tags']` (line 337) — fixed DB columns,
  always present, cannot be disabled.
- `SYSTEM_FIELDS = ['id', 'tub_id', 'created_at', 'updated_at', 'image', 'destination_config']`
  (line 314) — also always present.
- Everything else in the page's table — `item_number`, `type`, `subtype`, `status`,
  `serial_number`, `location`, `owner` — comes from `DEFAULT_FIELD_DEFINITIONS`, a *library* of
  optional fields. `getStandardFieldConfig()` (line 427), the function backing a
  "start from scratch" Tub, enables **only the four core fields**: *"A 'start from scratch'
  tub holds ONLY the four core fields — nothing else... they're offered on demand from the
  global library instead"* (lines 428-432). A Tub that hasn't added the Status field has no
  `item.status` to reference — the page never says this, so a reader has no reason to check
  before building a condition on `item.status == "operational"` (the page's own leading
  example).

**The page also omits `item.item_id`, a field it implies doesn't exist.** `item_id` is one of
the four `CORE_FIELDS` — always present, cannot be disabled (`field-config.ts:337`) — and
`binding-translator.ts`'s `isCoreField()` check means `{{item.item_id}}` passes straight
through untranslated, so it resolves like any other core-field binding. It is the "identifier
users lead with" per the field's own default ordering (`DEFAULT_FIELD_ORDER`, line 271-272,
puts `item_id` right after `name`). Neither this page's table nor the app's own field picker
(`ITEM_PROPERTIES` in `property-schemas.ts`) list it — a real, always-available field is
invisible to a reader relying on either surface.

**`item.id` is documented with a misleading example.** The table gives `item.id` → `"item-456"`
as the example value. In the schema, `things.id` is `UUID PRIMARY KEY DEFAULT gen_random_uuid()`
(`supabase/migrations/20250718000002_complete_schema.sql:22`), and
`ThingSerializer.deserialize` sets `id: dbThing.id` directly (`thing-serializer.ts:111`) — so
the real value looks like `"3f8a91c2-4b77-4e1a-9c30-eb15b7a2f001"`, not a short friendly string.
A reader building `https://yourapp.com/equipment/{{item.id}}` (the page's own "Custom
Application" example, line 175) will get a UUID in that URL, not `item-456`.

## 2. ANSWER-FIRST

Quoting the first line of body content under every H2 (the page nests almost all content one
level down in H3s, so where the H2 itself carries no text, the immediate next line is quoted
and marked as such):

**"## Two Ways to Use Fields"** (line 8) — no text of its own; the next line is H3
`### 1. URL Templates (Double Curly Braces)` (line 10), which opens: *"Use `{{field.name}}`
syntax to insert field values into Destination URLs."*
Judgment: **The H2 itself fails** — zero framing sentence before the reader is inside
subsection #1. The H3 opener is fine on its own (11 words, direct), but a reader who wants
"what are the two ways" gets no single answer at the H2 level; they must read both H3 headers
to construct it themselves.

**"## Available Fields"** (line 35) — no text of its own; next line is H3 `### Item Fields`
(line 37): *"Items have **standard fields** (built-in) and **custom fields** (you define
them)."*
Judgment: **Same pattern** — H2 has no lead sentence stating what categories exist or how many
(five documented, two more real-but-missing per §1). The reader has to scroll the whole
section to discover the field-category list.

**"## URL Template Examples"** (line 133) — no text of its own; next line is H3
`### Basic Field Insertion` (line 135), followed immediately by bold label + fenced code, no
prose at all in this H2's span.
Judgment: **Fails** — pure example gallery, no answer sentence anywhere.

**"## Conditional Visibility Examples"** (line 178) — same structure: H3 `### Item Status`
(line 180) straight into bold label + code, no prose.
Judgment: **Fails** — same as above.

**"## Common Patterns"** (line 252) — no text of its own; H3
`### Pattern 1: Equipment-Specific Inspections` (line 254) opens: *"Different equipment types
route to different inspection templates."*
Judgment: **Partial** — the H3 opener is a reasonable 8-word direct statement, but again the
H2 itself has no summary sentence, and the pattern relies on the still-undefined term
"Destination" (see §1, §6).

**"## Important Notes"** (line 303) — opens directly into a bold subhead, `**Field Names:**`,
then a bullet list starting *"Use exact field names: `item.item_number` NOT `item.number`"*.
Judgment: **Good enough** — this section is reference-style (a list of gotchas), and each
bullet is self-contained and direct; no scene-setting preamble to complain about.

**"## Getting Help"** (line 324) — opens: *"For complex field usage:"* followed by a numbered
list.
Judgment: **Good** — five words of framing, then direct action items. No preamble problem.

**"## Related"** (line 333) — a link list, not a content section; answer-first doesn't apply.

**Summary:** the page has essentially no prose at the H2 level anywhere except the single
opening paragraph before all headings. Every content H2 immediately drops into an H3 or a
labeled code block. For a retrieval system chunking on H2, four of six content sections
("Two Ways to Use Fields," "Available Fields," "URL Template Examples," "Conditional
Visibility Examples") return zero direct-answer text before the reader hits either a
sub-heading or a bare code block.

## 3. ONE QUESTION PER PAGE

**This page is doing two jobs that should be separated: (1) the field reference (what fields
exist, their syntax, their failure modes) and (2) a routing-recipes cookbook — and the second
job substantially duplicates the sibling page `/help/conditional-visibility.mdx`, which has
its own, near-identical worked examples.**

Concretely:
- This page's `## Common Patterns` → `### Pattern 1: Equipment-Specific Inspections`
  ("Different equipment types route to different inspection templates... Destination:
  'Forklift Inspection'... Condition: `item.type == "forklift"`") is the same scenario as
  `conditional-visibility.mdx`'s `## Common Use Cases` → `### 1. Equipment Type-Specific
  Inspections` ("You manage forklifts and cranes... Condition for 'Forklift Inspection'
  Destination: `item.type == "forklift"`").
- This page's `### Pattern 3: Status-Dependent Actions` overlaps
  `conditional-visibility.mdx`'s `### 3. Test Status-Based Routing` — both teach
  status-field-based show/hide with the same `item.status`/`item.testStatus` shape.
- This page's entire `## Conditional Visibility Examples` gallery (Item Status, Tags,
  Equipment Type, Combining Multiple Conditions, Device-Based Conditions) covers the same
  ground `conditional-visibility.mdx` already owns end-to-end, including its own device
  section (`## Advanced: Device-Specific Destinations`) and its own "Using AI to Generate
  Conditions" workflow.

CLAUDE.md is explicit about this: *"Search for existing information before adding new
content. Avoid duplication unless strategic."* Two Pages-group siblings independently
teaching "show the forklift inspection only for forklifts" is unstrategic duplication, and
`conditional-visibility.mdx` itself already points back here calling this page *"the complete
field reference"* (line 165, 230) — meaning the two pages *should* be dividing labor
(reference vs. decision-guidance/cookbook), but both currently carry the same cookbook.

**Proposed split:**
- **Keep on this page**, because nothing else owns it: the binding syntax (`## Two Ways to
  Use Fields`, tightened), the complete `## Available Fields` reference (with Time and
  Request added — see §1/§5), and the failure-mode/limits material from `## Important Notes`.
- **Cut `## URL Template Examples`, `## Conditional Visibility Examples`, and
  `## Common Patterns` down to a small, non-overlapping set** — one or two examples per field
  category is enough to show the syntax; the "which pattern for which business problem"
  cookbook role belongs to `conditional-visibility.mdx`, which already has it (and already has
  the "when you need this / when you don't" decision framing this page lacks entirely).

The page is not too thin to stand alone — the opposite risk applies here: it should shed the
duplicated cookbook weight and keep its unique reference content.

## 4. HEADINGS AS QUESTIONS

Proposed only where the question form is a genuinely clearer retrieval target than the
current noun phrase:

| Current heading | Proposed | Rationale |
|---|---|---|
| "Two Ways to Use Fields" | "How do I reference a field?" | Matches how a user or support bot would actually phrase the question; "two ways" requires already knowing there are two. |
| "Available Fields" | "What fields are available?" | Direct question match for the page's core reference purpose. |
| "Item Fields" (H3) | "What Item fields can I use?" | A chunker splitting on H3 benefits from the question form when this table is retrieved alone. |
| "Tub Fields" (H3) | "What Tub fields can I use?" | Same reasoning. |
| "Session Fields" (H3) | "What session/user fields are available?" | Same reasoning, and see §5 — this heading should also carry the "usually null" caveat. |
| "Device Fields" (H3) | *(leave as-is)* | Already cross-referenced in depth by `/help/device-detection`; noun form is fine here since the H3 body opens with a direct definition. |
| "Theme Fields" (H3) | *(leave as-is)* | Same — short, unambiguous category label. |
| "URL Template Examples" / "Conditional Visibility Examples" / "Common Patterns" | *(leave as labels, but see §3 — recommend trimming instead of renaming)* | These are example galleries, not single answerable questions; converting to question form doesn't fix the actual problem, which is duplicated content. |
| "Important Notes" | "What should I watch out for when using fields?" | More specific than a generic label; matches what a reader is actually trying to find (gotchas). |
| "Getting Help" / "Related" | *(leave as-is)* | Standard navigational sections. |

## 5. EDGE CASES / LIMITS / FAILURE MODES

Treating absence as a defect:

1. **Two entire field categories are missing** (Time, Request) — covered fully in §1. An AI
   agent would incorrectly deny that business-hours or country-based routing is possible.
2. **No CEL expression limits are documented anywhere on this page or on
   `conditional-visibility.mdx`.** `qrtub/src/lib/page/bindings.ts` enforces real ceilings via
   `CEL_VALIDATION_LIMITS` (lines 223-227): max expression length **500 characters**, max
   nesting depth **10**, max operator count **20** — checked by `validateBindingExpression`,
   which is wired into the actual condition-editing UI
   (`DestinationRulesEditor.tsx:78`, `PropertyForm.tsx:288`). The page's own
   `## Combining Multiple Conditions` section teaches readers to build compound expressions
   like `(item.type == "crane" || item.type == "forklift") && item.status == "operational"`
   with no indication that stacking several more of these will eventually be rejected, or what
   the error looks like.
3. **The "Missing Fields" guidance is incomplete for the case that matters most.** The page
   states: *"If a field is empty/null, URL templates insert empty string."* (line 315). That
   describes the low-level per-token substitution in `resolveBindings`, but it is not what a
   reader observes for an actual Destination. `destination-resolver.ts`'s `resolveDestination()`
   calls `resolveBindingsForUrl`, and when any binding in a rule's URL is unresolved
   (missing/null/empty, or an object/array value), the code does:
   ```
   if (result.unresolvedPaths.length > 0) {
     bindingErrors.push(`Rule ${i}: Skipped - unresolved: ...`);
     continue;   // the ENTIRE rule is skipped
   }
   ```
   (lines 58-61) — falling through to the next rule or `defaultLink`, not delivering a URL
   with a blank segment. A reader troubleshooting "why didn't my Destination fire, or why did
   the wrong one fire" would be misled by the current wording into looking for a broken link
   with an empty query param, when the actual symptom is a *different rule (or none) taking
   over entirely*.
4. **The array-handling advice doesn't say how, and may not be possible at all.**
   `## Important Notes` says: *"Cannot directly insert arrays in URL templates (convert to
   string first)."* (line 322) — but no mechanism for "converting to string first" is
   documented anywhere in the docs, and the source suggests there isn't one exposed to users:
   `resolveBindingsForUrl`'s own comment states *"An object/array binding would stringify to
   '[object Object]' / 'a,b' — a broken URL that would otherwise count as 'resolved'. Treat it
   as unresolved instead."* (bindings.ts:416-419) — i.e. the code deliberately treats any
   array/object binding as a failed (skipped) binding rather than emitting a joined string.
   "Convert to string first" implies a capability that doesn't appear to exist; this should
   either be corrected to "arrays cannot be inserted into URL templates" or the actual
   conversion mechanism (if one exists elsewhere, e.g. a template helper) should be named.
5. **No plan-tier statement.** Silent on whether field binding, URL Templates, or conditional
   visibility require a paid plan. Verified against `qrtub/src/lib/stripe-plans.ts`: the
   $5/month Starter plan already includes *"Drag and drop landing page editor"* with no
   separate mention of field binding or conditional visibility as a gated add-on anywhere in
   the plan feature lists — so as far as the source shows, this is available on every plan.
   That should be stated explicitly (a one-line "not gated by plan"), rather than left for an
   AI agent to guess.
6. **Session fields never state when `session.user` is actually populated.** The page's own
   hedge — *"Access information about the logged-in user (if authenticated)"* — is present but
   thin. Verified in `public-link-page.tsx:236-250`: the session passed into the page context
   comes from `supabase.auth.getUser()` on the **anonymous public scan request** — for the
   overwhelming majority of real-world scans (a member of the public scanning an item's QR
   code), there is no signed-in session, so `session.user` is `null` and every session-based
   condition or binding is dead weight. It's only non-null when the person viewing the page at
   that moment happens to be signed in (e.g., a team member checking their own item). This is
   exactly the kind of caveat a support agent needs and currently has to infer.

## 6. CHUNK INTEGRITY

Each H2 evaluated as if it were the only thing retrieved:

- **"Two Ways to Use Fields"** — **Mostly self-contained**: both numbered H3s carry their own
  syntax and example. Weak point: uses "Destinations" and "Destination URLs" without defining
  the term (see §1) — a reader with zero prior context knows the mechanics but not what
  they're mechanics *for*.
- **"Available Fields"** — **Self-contained** for the categories it covers (each H3 carries a
  full table + example); the defect here is completeness (§1), not isolation.
- **"URL Template Examples"** — **Fails in isolation.** Retrieved alone, this section is nothing
  but bare code blocks (`https://api.example.com/assets/{{item.serial_number}}`, etc.) with no
  reminder of what the `{{ }}` syntax means or that it only works inside a Destination URL —
  that explanation lives entirely in the earlier "Two Ways to Use Fields" section. A chunk
  retrieved here alone teaches nothing about the mechanism, only shows it in use.
- **"Conditional Visibility Examples"** — **Fails in isolation** for the same reason: no
  reminder of what a "condition" is, where it's entered, or what evaluating `true` vs. `false`
  does to a Destination. Pure boolean-expression snippets with a category label.
- **"Common Patterns"** — **Fails in isolation**, and more severely: each pattern is written as
  `**Destination: "Forklift Inspection"**` / `- URL: ...` / `- Condition: ...` bullet fragments
  that assume the reader already has both (a) the `{{ }}`/CEL vocabulary from earlier sections
  and (b) the concept of a "Destination" as a named, orderable thing on a Page — neither of
  which is established inside this section, and (b) is never established anywhere on this
  page (§1).
- **"Important Notes"** — **Self-contained.** Each bullet (field-name case sensitivity, URL
  encoding, missing fields, arrays) reads fine as an independent fact, though see §5 for
  factual completeness issues within it.
- **"Getting Help"** — **Self-contained**; item 1 points to an anchor on
  `conditional-visibility.mdx` as a "for more" pointer, which is fine (external enrichment, not
  a dependency).
- **"Related"** — Pure link list; no content to assess for isolation.

**Net: 3 of 6 content H2s ("URL Template Examples," "Conditional Visibility Examples,"
"Common Patterns") depend on vocabulary and concepts established only in earlier sections of
the same page, and all three inherit the page-wide undefined-"Destination" problem from §1.**

---

## Recommendation

A substantive rewrite is warranted, not a tweak. Two categories of real, verified fields
(Time, Request) are missing from what a sibling page calls "the full reference"; the
"Standard Item Fields" table doesn't distinguish the four always-present core fields from
optional library fields that a given Tub may not have enabled, and omits the one field
(`item.item_id`) that actually is always present; the stated failure mode for a missing field
in a URL template describes the wrong layer of the system (per-token substitution rather than
the rule-skipping behavior a reader will actually observe); no CEL complexity limits are
documented anywhere; "Destination" is used dozens of times and defined zero times; and roughly
half the page duplicates worked examples that `/help/conditional-visibility.mdx` already owns
in more depth. A proposed replacement has been written to
`/workspace/mintlify-docs/audit/proposed/help__using-fields.md`.
