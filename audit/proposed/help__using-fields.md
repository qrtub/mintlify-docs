---
title: "Using Fields in Pages"
description: "Reference Item, Tub, session, device, time, and request data in Destination URLs and conditions, with field availability, limits, and failure modes"
---

A **Destination** is a routing target on a Page — a button or rule that sends someone
somewhere (an inspection app, a CMMS, a manual) when they scan an Item's QR code. Fields let a
Destination's URL and visibility depend on that Item's own data instead of being fixed text.
There are two places you use a field: inside a Destination's **URL**, and inside a
Destination's **condition** (what makes it show or hide). Both use the same field names; only
the syntax around them differs.

## How do I reference a field?

Use double curly braces — `{{item.fieldName}}` — inside a URL, and a bare reference — like
`item.status == "operational"` — inside a condition. Single braces (`{fieldName}`) are not a
binding and are sent to the destination literally.

### In a URL: `{{field.name}}`

```
https://app.example.com/inspection/new?assetId={{item.serial_number}}
```

When someone scans the code for Item "EXC-203" (serial number `SN-2024-203`), this resolves
to:

```
https://app.example.com/inspection/new?assetId=SN-2024-203
```

### In a condition: direct reference, no braces

```
item.status == "operational"
```

A Destination with this condition only appears when the Item's `status` field equals
`"operational"`. Conditions use [CEL](https://github.com/google/cel-spec) (Common Expression
Language) — `==`, `!=`, `<`, `>`, `<=`, `>=`, `&&`, `||`, `in`, and `size()` are supported. See
[Conditional Visibility](/help/conditional-visibility) for worked routing scenarios (equipment
type, tags, status, device) — this page is the field reference; that page is the cookbook for
deciding when and how to combine conditions.

## What fields are available?

Six categories of data are available in both URL templates and conditions: Item, Tub, Session,
Device, Time, and Request. Custom fields you add to a Tub are also available under `item.*`.

### Item fields

Every Item has a mix of **always-present** fields and **optional** fields that depend on your
Tub's configuration.

**Always present** — these exist on every Item regardless of Tub setup:

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `item.id` | string | Internal database ID (a UUID) | `"3f8a91c2-4b77-4e1a-9c30-eb15b7a2f001"` |
| `item.item_id` | string | The Item ID you assign — the identifier you lead with | `"EXC-203"` |
| `item.name` | string | Display name | `"Excavator #203"` |
| `item.description` | string | Description text | `"CAT 320 Hydraulic Excavator"` |
| `item.tags` | array | Tags array | `["construction", "rental"]` |
| `item.image` | string | Image URL | `"/uploads/exc203.jpg"` |
| `item.created_at` | date | Creation date | `"2024-01-15T00:00:00Z"` |
| `item.updated_at` | date | Last update date | `"2024-01-20T00:00:00Z"` |

`item.id` is rarely what you want in an outbound URL — it's the raw database key, not a
friendly identifier. `item.item_id` (or `item.item_number`, below, if your Tub uses it) is
almost always the better choice for a field like `?assetId=`.

**Optional — enabled per Tub:** these are common fields offered from a shared library when you
set up a Tub, but a Tub can be built without them, or with them later disabled. A field that
isn't enabled on a Tub resolves as empty (see **Missing or empty fields**, below) — check the
Tub's Field Configuration before relying on one.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `item.item_number` | string | Item number/SKU | `"EXC-203"` |
| `item.type` | string | Type/category | `"Heavy Equipment"` |
| `item.subtype` | string | Subtype | `"Excavator"` |
| `item.status` | string | Current status | `"operational"` |
| `item.serial_number` | string | Serial number | `"SN-2024-203"` |
| `item.location` | string | Physical location | `"Site A"` |
| `item.owner` | string | Owner/assignee | `"John Smith"` |

**Important:** the field is `item.item_number`, not `item.number`.

#### Custom Item fields

Any custom field you add to your Tub is accessible as `item.{fieldName}`, using the name you
gave it when you created the field.

**Example:** a custom field called `inspectionDue`:
- URL: `{{item.inspectionDue}}`
- Condition: `item.inspectionDue != ""`

### Tub fields

Information about the Tub (category/workspace) containing the Item.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `tub.id` | string | Tub unique identifier | `"tub-123"` |
| `tub.name` | string | Tub display name | `"Heavy Equipment"` |
| `tub.description` | string | Tub description | `"Construction equipment fleet"` |
| `tub.image_url` | string | Tub image URL | `"/images/equipment-icon.png"` |
| `tub.items_name` | string | Items collection name | `"Machines"` |
| `tub.created_at` | date | Tub creation date | `"2024-01-01T00:00:00Z"` |
| `tub.metadata.page.is_public` | boolean | Public accessibility | `true` |
| `tub.metadata.organizationName` | string | Organisation name | `"BuildCo Inc."` |

**Nested metadata example:**
```
tub.metadata.page.is_public == true
```

### Session fields

Information about whoever is currently viewing the page, if they happen to be signed in.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `session.user` | object | User object (`null` if not signed in) | `{...}` |
| `session.user.id` | string | User ID | `"user-123"` |
| `session.user.email` | string | User email | `"john@example.com"` |
| `session.user.name` | string | User name | `"John Smith"` |

For a typical public QR-code scan, nobody is signed in, so `session.user` is `null` and any
session-based condition or binding will not fire. Session fields are only useful for the
subset of views where the viewer is signed in to QRtub at the time — for example, a team
member checking their own item, not the general public scanning it. Don't build a
customer-facing routing rule around `session.user`; use it only for the "signed-in team
member" case.

**Check if someone is signed in:**
```
session.user != null
```

### Device fields

Automatically detected from the scanning device.

| Field | Type | Description | Example Values |
|-------|------|-------------|----------------|
| `device.type` | string | Device type | `'mobile'`, `'tablet'`, `'desktop'` |
| `device.os` | string | Operating system | `'ios'`, `'android'`, `'windows'`, `'macos'`, `'linux'`, `'unknown'` |
| `device.browser` | string | Browser | `'chrome'`, `'safari'`, `'firefox'`, `'edge'`, `'opera'`, `'unknown'` |
| `device.isMobile` | boolean | True if mobile phone | `true` / `false` |
| `device.isTablet` | boolean | True if tablet | `true` / `false` |
| `device.isDesktop` | boolean | True if desktop | `true` / `false` |
| `device.isIOS` | boolean | True if iOS device | `true` / `false` |
| `device.isAndroid` | boolean | True if Android | `true` / `false` |

See [Device Detection](/help/device-detection) for device-based routing patterns.

### Time fields

The current time, at the moment the code is scanned — always UTC, not the viewer's local
time zone.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `time.hour` | number | Current hour, UTC (0–23) | `14` |
| `time.dayOfWeek` | number | Day of week, UTC (0 = Sunday, 6 = Saturday) | `1` |
| `time.dayOfMonth` | number | Day of month, UTC (1–31) | `15` |
| `time.month` | number | Month, UTC (1–12) | `6` |
| `time.year` | number | Year, UTC | `2025` |
| `time.isWeekend` | boolean | True if Saturday or Sunday, UTC | `false` |

**Restrict a Destination to business hours (UTC):**
```
time.hour >= 9 && time.hour < 17 && !time.isWeekend
```

There is no date value and no date arithmetic — `time` exposes the parts above, not a full
date, so "show this only when the inspection is overdue" can't be expressed from time alone.
Use a status field you maintain instead (`item.status == "expired"`).

### Request fields

Information about the scan request itself.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `request.timestamp` | string (date) | Request timestamp, ISO 8601 | `"2025-01-15T14:30:00Z"` |
| `request.path` | string | Request URL path | `"/item-123"` |
| `request.referrer` | string | Referring URL, if available | `"https://google.com"` |
| `request.language` | string | Browser's Accept-Language preference | `"en-US"` |
| `request.country` | string | Country code from CDN geo headers, if available | `"US"` |
| `request.city` | string | City from CDN geo headers, if available | `"San Francisco"` |
| `request.ip` | string | Client IP address, if available | `"192.168.1.1"` |

`request.country` and `request.city` depend on geo headers from the hosting CDN and are `null`
when those headers aren't present — treat country-based routing as "available when we can
detect it," not guaranteed on every scan.

**Route by country:**
```
request.country == "AU"
```

### Theme fields

The Page's own theme configuration.

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `theme.accent` | string | Accent colour | `"sky"` |
| `theme.radius` | string | Border radius | `"xl"` |

## What happens with missing, empty, or invalid fields?

**In a condition:** a missing, `null`, or undefined field makes the whole condition evaluate
to `false` — silently, with no error. `item.owner != null` is the pattern for checking a field
exists before relying on it.

**In a Destination URL:** this is where the behaviour is easy to misread. If every field in
the URL resolves, the URL is used as-is. But if *any* field in that URL is missing, empty, or
`null`, QRtub does not deliver a URL with a blank spot where the value should be — it skips
that entire Destination rule and falls through to the next matching rule, or to the
Destination's default URL if one is set. If nothing else matches, the Destination doesn't
fire at all. When you're troubleshooting "why did the wrong Destination show up" or "why
didn't this one fire," the first thing to check is whether one of its URL's fields is empty
on that Item — not whether the URL looks broken.

**Arrays (like Tags) cannot be inserted into a URL template directly.** `"tag-name" in
item.tags` and `size(item.tags) > 0` work fine in conditions, but `{{item.tags}}` in a URL is
treated the same as a missing field — the rule is skipped, not filled in with a joined
string.

**Case sensitivity:** field names are case-sensitive. `item.Status` will not match
`item.status`.

**URL encoding:** field values are inserted into URL templates exactly as stored — QRtub does
not URL-encode them. If a value may contain spaces or characters such as `&`, `?`, or `/`,
confirm the receiving system accepts them unencoded, or avoid putting that field in a URL.

**Condition length and complexity limits:** conditions are capped at 500 characters, 10 levels
of nested parentheses, and 20 operators. A condition over any of these limits is rejected when
you save it, with an error naming which limit was hit. This is rarely a practical ceiling, but
if you're chaining many `||`/`&&` clauses together, you can hit it.

## Is this available on every plan?

Yes — field binding in URL templates and conditional visibility are part of the Page editor,
which is included on every QRtub plan, including the entry Starter plan. Neither is a
higher-tier add-on.

## Examples

A couple of complete examples per mechanism — for a library of routing scenarios (by
equipment type, tag, status, and device), see
[Conditional Visibility](/help/conditional-visibility).

**Pre-fill an inspection app with the Item's serial number and location:**
```
https://app.inspectionapp.com/new?assetId={{item.serial_number}}&location={{item.location}}
```

**Open the matching record in a CMMS:**
```
https://app.yourcmms.com/workorders/new?asset={{item.item_number}}&site={{item.location}}
```

**Show a Destination only for operational, tagged equipment:**
```
item.status == "operational" && "heavy-equipment" in item.tags
```

**Show a mobile-only Destination:**
```
device.isMobile
```

## Getting help

For complex field usage:

1. **Use AI to generate expressions** — See [Conditional Visibility](/help/conditional-visibility#using-ai-to-generate-conditions)
2. **Test with sample Items** — Create test Items with different field values
3. **Check field names** — Verify exact field names in your Tub's Field Configuration
4. **Contact support** — Email [hi@qrtub.com](mailto:hi@qrtub.com) with your use case

## Related

- [Conditional Visibility](/help/conditional-visibility) - Routing scenarios and worked examples
- [Device Detection](/help/device-detection) - Device-based routing
- [Building a Page](/help/building-a-page) - Where to enter URLs and conditions in the Page Editor
- [Mitti (formerly SafetyCulture) Integration](/integrations/safetyculture) - Using fields with Mitti
- [CMMS Integration](/integrations/cmms-systems) - Using fields with maintenance systems
- [Pages Overview](/help/pages-overview) - What a Page and a Destination are

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
