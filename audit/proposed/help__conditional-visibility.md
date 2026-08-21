---
title: "Conditional Visibility"
description: "Show or hide Destinations on Pages based on Item, device, time, or session data"
---

Conditional visibility shows or hides a Destination on a Page depending on data available at the moment someone scans your QR code — an Item field, the visitor's device, the current time, or their login session. It's written as a CEL (Common Expression Language) expression, an industry-standard expression syntax. This is an advanced feature for complex routing scenarios — most Pages work fine showing every Destination and letting people tap what's relevant.

## Where to set a condition

Select the Destination block on your Page, open its **Visibility** group in the Properties panel, and enter your expression into the **Show When (CEL Expression)** field (placeholder: `item.status == "active"`). As you type, a preview badge shows **Visible** or **Hidden** — evaluated live against sample data — plus an inline error if the expression doesn't parse. No condition means the Destination always shows.

## Should you use conditional visibility?

Use it only when showing every Destination would genuinely confuse people. In most cases, showing everything and letting visitors self-select is simpler and has nothing to silently misfire.

**Don't use it for:**
- Different audiences seeing different content — just show every Destination; people tap what's relevant to them
- Building different URLs from Item data — use a URL Template (`{{item.fieldName}}`) instead of a condition; see [Using Fields](/help/using-fields)
- Any scenario where showing all the options is simpler than hiding some of them

**Do use it for:**
- Completely different Destinations for different equipment or item types (forklift vs. crane inspection buttons)
- A warning or alert Destination that should only appear when a condition is met (e.g., "RETEST REQUIRED")
- Device-specific routing (a mobile app deep link vs. a desktop web link)
- Complex routing where showing every Destination would genuinely confuse the visitor

## Common use cases

### 1. Equipment type-specific inspections

**Scenario:** You manage forklifts and cranes. Each type needs a different inspection form, and you don't want forklift operators seeing the crane inspection button.

**Solution:** Show "Forklift Inspection" only for forklifts, "Crane Inspection" only for cranes.

**Condition for "Forklift Inspection":**
```
item.type == "forklift"
```

**Condition for "Crane Inspection":**
```
item.type == "crane"
```

**Setup:**
1. Use the standard `type` field, or add a custom field (e.g., `equipmentType`) to your Tub
2. Set each Item's type value (`"forklift"`, `"crane"`, etc.)
3. Create a separate Destination for each inspection type
4. Add the matching condition to each Destination

### 2. Tag-based routing

**Scenario:** Some equipment is tagged `"heavy-equipment"` and needs a specialised inspection; the rest uses the standard one.

**Condition to show "Heavy Equipment Inspection":**
```
"heavy-equipment" in item.tags
```

**Setup:**
1. Tag the relevant Items with `"heavy-equipment"` (the standard tags field)
2. Create a "Heavy Equipment Inspection" Destination
3. Add the condition `"heavy-equipment" in item.tags`
4. Tagged Items show this Destination; untagged Items don't

### 3. Test status-based routing

**Scenario:** Electrical test-and-tag equipment. You want a "RETEST REQUIRED" Destination to appear only once a test has expired.

**Condition to show "RETEST REQUIRED":**
```
item.testStatus == "expired"
```

**Setup:**
1. Add a `testStatus` custom field to your Tub
2. Set its value yourself: `"current"`, `"expired"`, `"pending"`, etc.
3. Create the "RETEST REQUIRED" Destination
4. Add the condition `item.testStatus == "expired"`

**Important:** `"expired"` here is a value **you** set and update — nothing recalculates it automatically. Conditions can't compare against a stored date to work out whether something has "expired" (see [Fields you can reference](#fields-you-can-reference) below). Keep the status field current, typically by updating it as part of your test/inspection workflow, or by CSV re-import.

## Fields you can reference

A condition can use any of the following. Field names are case-sensitive.

| Prefix | What it gives you | Notes |
|--------|-------------------|-------|
| `item.*` | Standard fields (`item.status`, `item.type`, `item.tags`, …) and any custom field you've added to the Tub | See [Using Fields](/help/using-fields) for the full standard-field table |
| `tub.*` | Fields on the Tub containing the Item, e.g. `tub.name` | |
| `device.*` | The visitor's device: `device.isMobile`, `device.isDesktop`, `device.isIOS`, `device.browser`, etc. | See [Device Detection](/help/device-detection) for the full field table and worked routing examples, including the iOS Safari app-link workaround |
| `session.*` | The logged-in user, if any: `session.user.id`, `session.user.email` | `session.user` is `null` when nobody's logged in — check with `session.user != null` before reading a nested field |
| `time.*` | `time.hour` (0–23), `time.dayOfWeek` (0=Sunday…6=Saturday), `time.dayOfMonth`, `time.month`, `time.year`, `time.isWeekend` | **All in UTC**, not the visitor's local time. There's no combined date and no date arithmetic — you can't compare against a stored date field, only against these fixed parts of *right now* |
| `request.*` | `request.country`, `request.city`, `request.language` | Populated from hosting-infrastructure headers when available; treat as best-effort, not guaranteed present for every visitor |

**Example — only during business hours in UTC:**
```
time.hour >= 9 && time.hour < 17 && !time.isWeekend
```
Because `time.*` is UTC, adjust the hour range for your own timezone offset — this isn't "9–5 wherever the visitor is."

**Supported operators and functions:** `==`, `!=`, `<`, `>`, `<=`, `>=`, `&&`, `||`, `!` (negation), `in` (array membership), and `size()` (array/string length). Other CEL features (regex matching, string functions like `startsWith`) aren't supported here.

## Using AI to generate conditions

For anything more complex than a single comparison, describe what you want in plain language to ChatGPT, Claude, or a similar tool and have it write the CEL expression — this is usually faster and less error-prone than hand-writing nested boolean logic.

**Prompt template:**
```
I'm using QRtub Pages with conditional visibility (CEL expressions).
I want to show a Destination called "[DESTINATION NAME]" only when:
- [Describe your first condition]
- [Describe your second condition]
- [Additional conditions]

Generate a CEL expression for this condition.

Available Item fields: [list your field names and types]
Available operators: ==, !=, &&, ||, !, in, size()
```

**Example prompt:**
```
I'm using QRtub Pages with conditional visibility (CEL expressions).
I want to show a Destination called "Crane Safety Inspection" only when:
- The Item has tag "crane" OR tag "heavy-equipment"
- AND the status is "active"

Generate a CEL expression for this condition.

Available Item fields: item.tags (array), item.status (string), item.type (string)
Available operators: ==, !=, &&, ||, in
```

**Response:**
```
("crane" in item.tags || "heavy-equipment" in item.tags) && item.status == "active"
```

Paste the result into the Destination's **Show When (CEL Expression)** field and check the live Visible/Hidden preview before publishing.

## How to test a condition

1. **Use the live preview first.** The Condition field in the Page editor shows **Visible**, **Hidden**, or an inline error as you type, evaluated against sample data — no need to create real Items just to check the syntax.
2. **Then verify against real data.** Create or edit a few Items with different field values and view the live Page for each, to confirm the condition behaves the way you expect against your actual data — the live preview uses sample data that may not match your real Items' shape.
3. **Start with one field and one condition.** Get that working before combining multiple conditions with `&&`/`||`.
4. **Use descriptive field names.** `equipmentType` is clearer in a condition than `type`.
5. **Keep at least one Destination unconditioned**, or one whose condition is the logical complement of the others. If every Destination's condition evaluates to `false` for a visitor, the Page shows **no Destinations at all** — nothing renders in their place, with no error or fallback message.

## Limits and failure modes

- **A typo or unknown field silently hides the Destination — it does not show an error.** If a condition references a field that doesn't exist (a misspelled field name, a deleted custom field, or an invented value that isn't actually available), the condition evaluates to `false` with no error shown anywhere — not on the live Page, not to you as the Page owner. If a Destination you expect to see isn't appearing, double-check the exact field name first.
- **No current-date value, no date arithmetic.** `time.*` gives you the hour, day, month, and year as separate numbers — there's no single "today" value and no way to compare it against a stored date field like `item.created_at`. Use a manually-maintained status field instead (see use case 3 above).
- **`time.*` is UTC**, regardless of where the visitor is scanning from.
- **`request.*` fields depend on hosting infrastructure and aren't guaranteed** for every request — don't build a condition where the only path to a visible Destination depends on `request.country` or similar being present.
- **Expressions have a practical ceiling:** roughly 500 characters, 10 levels of nested parentheses, and 20 operators. Well-scoped conditions like the examples on this page are nowhere near these limits, but a large AI-generated expression combining many fields might approach them.
- **Available on every QRtub plan** — conditional visibility isn't gated behind a specific plan tier.

## Getting help

Conditional visibility uses CEL (Common Expression Language), an industry-standard expression syntax — not something QRtub invented. For anything beyond a simple comparison:

1. Use the [AI prompt template above](#using-ai-to-generate-conditions) with ChatGPT, Claude, or a similar tool
2. Describe what you want in plain language and let the AI translate it to CEL
3. Check the live Visible/Hidden preview, then test against real Items before relying on it

Need help? Email [hi@qrtub.com](mailto:hi@qrtub.com) with your use case and we'll help you construct the right condition.

## Related

- [Using Fields](/help/using-fields) — complete Item, Tub, session, and device field reference, plus URL Template syntax
- [Device Detection](/help/device-detection) — full device-field reference and routing examples, including the iOS Safari app-link workaround
- [Pages Overview](/help/pages-overview)
- [Key Concepts](/help/key-concepts)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
