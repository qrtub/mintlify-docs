# Editorial Audit: Custom Fields

**File:** `/workspace/mintlify-docs/help/custom-fields.mdx`
**Live:** https://help.qrtub.com/help/custom-fields
**Nav group:** Help → Items & Data (the *only* page in that group)
**Siblings skimmed for overlap:** `help/using-fields.mdx` (Pages group), `help/building-a-page.mdx` (Pages group)

---

## 1. Self-containment

Mostly yes, with two concrete holes that would strand a cold reader or send them to the wrong tab.

**Gap A — "the destination URL field" is used before it's defined, and the page's own instructions for where to configure it are wrong for that one field.**

The "Where fields are configured" section tells the reader, as the very first instruction on the page: "Open the Tub, go to its settings, and choose the **Fields** tab." The "Defaults" section then says:

> "The **destination URL** field's default behaves slightly differently: it is stamped onto the Item when the Item is created."

Two problems verified against `../qrtub/src`:

- The page never explains what "the destination URL field" *is*. Nothing earlier on the page mentions Destinations, Direct Mode, or Pages — a reader who landed on this page cold (not "Key Concepts") has no way to know this is the field that decides what a scan resolves to.
- It is **not configured on the Fields tab** at all. `FieldConfigurationManagerCompact.tsx` explicitly hides it: `const isManagedExternally = (fieldName: string) => fieldName === 'destination_url';`. The actual control is a "Default destination" box on a different part of Tub settings (`EditTubForm.tsx`, the pass-through/Destination configuration, shown when profile pages are off) — not the Fields tab the page just told the reader to open.

A reader following this page's own first instruction to find and set that default will not find it there. Fix: either drop the destination-URL aside from this page (it's arguably out of scope for "Custom Fields") or explicitly say which tab it lives on and link to it.

**Gap B — Field type table names an option that doesn't exist under that name in the product.**

The Field Types table lists:

```
| **object** | Structured data |
```

The actual field-type dropdown in `FieldConfigurationManagerCompact.tsx` (`FieldTypeSelect`) offers exactly six options, verbatim: `Text`, `Number`, `Date`, `Yes/No`, `List`, **`UUID`**. There is no "Structured data" label anywhere in that UI. A reader (or a support bot paraphrasing this page) who goes looking for "object / structured data" in the Fields tab will see "UUID" and reasonably conclude the docs are describing something else, or that the feature has changed. This is a naming mismatch a cold reader cannot resolve from the page alone.

**Gap C — two behaviors a reader would need mid-task are not stated anywhere on the page:**

- **What happens to an Item's existing value when a custom field is deleted?** Verified in `thing-serializer.ts`: deleting a field only removes it from `fieldConfig.fields` (`deleteFieldFromConfig`); the underlying value is **not purged** from `metadata.fields`. The serializer has dedicated "orphan data" handling — *"This preserves data even if config is out of sync"* — and will still surface the old value, now keyed by its raw nanoid (e.g. `Xk9mPq7z`) instead of a human label, since `idToKeyMap` no longer has an entry for the deleted field. A reader deleting a field to "clean up" has no way to know from this page that the data survives (and resurfaces under an opaque key) rather than being removed.
- **What happens to existing values when a field's type is changed after Items already have data?** No validation, migration, or warning was found in `FieldConfigurationManagerCompact.tsx` gating a type change on existing data — the type selector is freely editable regardless of whether the field is already populated. The page doesn't mention this at all, and a reader has no way to know whether QRtub will convert, ignore, or corrupt existing values.

Otherwise, the page is a reasonably complete standalone reference for the mechanics it does cover (where to configure fields, what core vs. custom means, key rules, defaults, references, required-field enforcement) — someone who landed here cold could complete "define a custom field with defaults and validation" without following a link, apart from the two gaps above.

---

## 2. Answer-first

Checked every H2's opening sentence(s) against the question implied by its heading.

| Heading | Opening sentence(s) | Verdict |
|---|---|---|
| Where fields are configured | "Open the Tub, go to its settings, and choose the **Fields** tab." | Direct instruction, no preamble. Good, though at 11 words it's terse rather than a 40–60 word answer — there simply isn't more to say for this heading, so this is fine as-is. |
| Core fields and custom fields | "Four fields are always present and cannot be removed or renamed: **name**, **item_id**, **description** and **tags**." | Direct answer to "what's the difference between core and custom fields," leads with the concrete list. Good. |
| Field keys | "Every field has a key, used in bindings like `{{item.serial_number}}` and in conditions." | Answers *why keys matter* but the heading implies "what are the rules for keys" — that's answered by the bullet list right after, not the opening sentence. Borderline: acceptable, but the first sentence is scene-setting for the *bullets*, not itself the answer. |
| Field types | *(no opening sentence — goes directly to the table)* | Not answer-first in the sense of a sentence, but not preamble either — the table itself is the direct answer. Missing is a one-line frame ("QRtub has six field types:") that would help a chunk-only retrieval fit the type table to the question "what field types does QRtub support." |
| Allowed values | "A field can carry a fixed list of allowed values, which turns it into a picker rather than a free-text box." | Direct, concrete. Good. |
| Defaults | "A field can have a Tub-level default, applied when an Item is created with that field left blank." | Direct. Good. |
| Reference fields | "A field can point at another record instead of holding a value of its own." | Direct. Good. |
| Required fields and validation | "Any field can be marked required. Required fields are enforced when Items are created or edited, including during CSV import — a row missing a required value is rejected while the rest of the file still imports." | Direct, ~45 words, answers the heading precisely. Good — but see §5, this claim is incomplete without the allowed-values caveat that lives two sections earlier. |
| How fields get used elsewhere | "Once a field exists, it is available in three places:" | Direct lead-in to the list that follows; functions as a correct answer-first opener. Good. |
| Related | *(link list, no prose)* | N/A — not a content section. |

No section opens with marketing preamble or throat-clearing. This page's answer-first discipline is good; the main defect is content-completeness (§1, §5), not structure.

---

## 3. One question per page

This page is a single coherent question — "how do I define and configure the fields on a Tub's Items?" — covering one task (schema authoring) end to end: where to configure, core vs. custom, keys, types, allowed values, defaults, references, required/validation, and where fields get consumed. I would **not** split it; the sections are tightly related sub-steps of one configuration task, not distinct tasks bolted together.

One borderline candidate for a future split: **Reference fields** (lines 72–87) is a self-contained sub-feature (linking an Item to a team member / another Item / a Tub) that could stand alone as "Linking Items to Other Records," especially since it's the one part of this page that models relationships rather than plain data. At its current length (~16 lines, one table) it's too thin to justify a standalone page today — recommend leaving it merged, but flagging it as the first thing to peel off if the page grows further (e.g. if reference-field behavior on deletion of the referenced record, or bulk-reference CSV import, gets documented in more depth later).

The page is not too thin to stand alone — no merge recommendation.

---

## 4. Headings as questions

Most headings are fine as noun phrases because they already read as the natural label for a reference table/section (e.g. "Field types," "Allowed values") rather than hiding an implicit question. Proposing rewrites only where a question form is genuinely clearer:

- **"Where fields are configured"** → keep as-is; already reads as an implicit question and a QA-style rewrite ("Where do I configure fields?") adds nothing.
- **"Core fields and custom fields"** → **"Which fields are built in, and which can I add?"** This heading currently reads as a topic label; the section is actually answering a comparison question (what's fixed vs. what's mine), and the question form signals that up front.
- **"Field keys"** → **"What are the rules for a field's key?"** The current heading doesn't signal that the section is a validation-rules reference; a reader scanning headings for "why did my field key get rejected" would more easily match a question form.
- **"Field types"** → keep as-is (a table of options, not really an implicit question).
- **"Allowed values"** → keep as-is (topic label matches a reference table).
- **"Defaults"** → **"What happens if an Item is created with a field left blank?"** — sharper than the generic noun, and matches exactly what the section answers.
- **"Reference fields"** → **"How do I link a field to another record?"** — the current noun phrase doesn't signal "this is how you model relationships," which the first sentence of the section actually says.
- **"Required fields and validation"** → keep as-is; already close to a question in spirit and the compound noun phrase reads fine.
- **"How fields get used elsewhere"** → already phrased as an implicit question ("how... get used"); keep as-is.

---

## 5. Edge cases / limits / failure modes

This is the weakest dimension of the page. Concrete gaps, each checked against source:

1. **Allowed-values import validation is stated as unconditional but is actually conditional — and the condition is the opposite of what's implied.** The page says: *"Fields with allowed values are also validated on import, so a typo in a status column is caught rather than silently creating a new status."* Verified in `csv-row-validation.ts`:
   ```
   // 3. Allowed values (enum) — only when the field forbids new values.
   for (const [key, def] of Object.entries(fieldConfig.fields)) {
     if (!def.enabled || def.allowNewValues === true) continue;
   ```
   This check is **skipped entirely** when "Allow new values" is on for that field — which the page itself documents two sections earlier as a normal, common setting. So for any field with "Allow new values" on, a typo in a CSV status column is *not* caught — it silently registers as a new allowed value, exactly the failure mode the sentence claims doesn't happen. This is the single highest-risk gap on the page for an AI support agent: asked "will a typo in my CSV get caught," the correct answer is "only if the field doesn't allow new values," and nothing on the page says that.

2. **No statement of what happens to an Item's data when a custom field is deleted.** See §1 Gap C. Verified: data is not purged, it becomes an "orphan" surfaced under its raw nanoid key. This is exactly the kind of question ("if I delete a field, do I lose the data?") a support bot would otherwise have to guess at, and either wrong answer (yes/no) is plausible-sounding without the source.

3. **No statement of what happens when a field's type is changed after Items already hold values in it.** No conversion/validation logic found gating this in `FieldConfigurationManagerCompact.tsx`; the page is silent on it entirely.

4. **No stated ceiling on the number of custom fields a Tub can have.** Searched `field-config.ts`, `server-field-config.ts`, and `stripe-plans.ts` — no cap, no plan-tier gating of field count found anywhere. Given the CLAUDE.md instruction to treat silent absence as a defect, the page should say explicitly that there's no limit, rather than leaving a reader (or an AI agent) to wonder or invent a number.

5. **No plan-tier gating mentioned, and none appears to exist.** Custom Fields is not referenced in `stripe-plans.ts` as a gated feature. Worth a one-line explicit confirmation ("available on every plan") so an AI agent doesn't infer a tier restriction from its absence, consistent with how other QRtub docs handle unlimited/ungated capabilities.

6. **Field type table names a type ("object" / "Structured data") whose actual UI label ("UUID") and apparent purpose (see §1 Gap B) diverge from the description given.** This is as much an edge-case/accuracy gap as a self-containment one: nothing on the page explains what "object" fields are actually used for, and the label mismatch means a reader can't map the docs to the product with confidence.

7. **Required-field enforcement on CSV *update* rows is narrower than the page implies.** The page says "Required fields are enforced when Items are created or edited, including during CSV import." Verified in `csv-row-validation.ts`: on an update row, a required field is only checked "when it's present in the row" (`isUpdate && !(key in item)` → skipped) — so a partial-update CSV that omits a required column is *not* rejected for that column. The current wording ("enforced... during CSV import") reads as unconditional and doesn't distinguish create-import from update-import.

---

## 6. Chunk integrity

Evaluated each H2 as if retrieved alone, no surrounding page.

- **Where fields are configured** — Stands alone fine. One self-contained instruction.
- **Core fields and custom fields** — Stands alone fine. "them" in the second sentence refers back to the four fields named in the same section's first sentence — no cross-section dependency.
- **Field keys** — Stands alone fine.
- **Field types** — Stands alone as a table; no prose to depend on anything else. (Its only defect is the object/UUID mismatch, not isolation.)
- **Allowed values** — Stands alone; "the list" and "this" both resolve within the section's own preceding sentence. Fine in isolation, **but** its accuracy is coupled to information that actually lives in the "Required fields and validation" section (the import-validation caveat, per §5.1) — a reader who retrieves *only* this section would come away believing typos are always caught, which is wrong once "Allow new values" is on. This is a genuine chunk-integrity failure: the two sections make claims that only cohere when read together, and neither one contains the caveat that would make it independently correct.
- **Defaults** — **Fails in isolation.** "The destination URL field's default behaves slightly differently" assumes the reader already knows what "the destination URL field" is and where it lives — neither is established anywhere on this page (see §1 Gap A). A reader retrieving just this section has no way to resolve what's being referred to, and would also be misled about where to configure it (this page's own "Where fields are configured" section, if also retrieved, points to the wrong tab for this specific field).
- **Reference fields** — Stands alone fine; the table is self-explanatory and doesn't depend on prior sections.
- **Required fields and validation** — Stands alone fine in the sense of no pronoun/"above" dependency, but see the Allowed-values note above: its own claim is subtly incomplete without content from the earlier "Allowed values" section.
- **How fields get used elsewhere** — Stands alone fine; the three bullets are self-explanatory and it correctly links out (rather than assuming) for depth.

---

## Cross-page note (context, not a defect of this page)

`help/using-fields.mdx` (Pages group) lists a "Standard Item Fields" table with 14 entries — `item.id`, `item.status`, `item.serial_number`, `item.location`, `item.owner`, `item.type`, `item.subtype`, `item.created_at`, `item.updated_at`, etc. — presented as always-present standard fields. This directly conflicts with `custom-fields.mdx`'s own, source-verified claim that only **four** fields (`name`, `item_id`, `description`, `tags`) are core/always-present (confirmed in `field-defaults.ts`: `CORE_FIELDS = ['name', 'item_id', 'description', 'tags']`). Everything else `using-fields.mdx` lists as "standard" (status, serial_number, location, owner, type, subtype, etc.) is actually a **disableable, renamable, optional** `DEFAULT_FIELD_DEFINITIONS` entry — several of them (`equipment_manager`, `notes`, `manufactured_date`, `parent_item`) even ship `enabled: false` by default. This is flagged separately for whoever audits `using-fields.mdx`, but it's relevant here because it directly undermines a reader's ability to trust `custom-fields.mdx`'s "four core fields" claim if they cross-reference the sibling page afterward — the two pages currently tell a materially different story about what's built-in vs. optional.

---

## Verdict

A proposed rewrite is warranted and has been drafted to `/workspace/mintlify-docs/audit/proposed/help__custom-fields.md`. Rationale: the gaps found are not cosmetic — they touch self-containment (destination-URL default sent to the wrong tab, undefined term), a factual contradiction with real support-ticket consequences (allowed-values-on-import caveat), and two "what happens when things go wrong" edge cases with verified-but-undocumented answers (field deletion, field type change) that are exactly the kind of thing an AI support agent would otherwise fabricate.
