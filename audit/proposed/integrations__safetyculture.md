---
title: "Mitti (formerly SafetyCulture / iAuditor)"
description: "Open Mitti inspections straight from a scan, pre-filled with the item's data. Mitti was called SafetyCulture, and iAuditor before that."
platform: "Mitti"
featured: true
---

Connect a QRtub [Destination](/help/pages-overview) to Mitti so staff can scan an item and land straight on the right inspection template, with that item's own data already filled in. This is available on every QRtub plan — nothing about deep links, URL Templates, or fallbacks is plan-gated.

## Is Mitti the same as SafetyCulture or iAuditor?

Yes — same product, three names. It launched as **iAuditor**, was renamed **SafetyCulture**, and became **Mitti** in August 2026. SafetyCulture remains the company name; Mitti is the platform name. Nothing you already set up stops working:

- The mobile app kept its identifiers, so the **`iauditor://` deep link scheme still works** — you'll see it throughout this page, and it's correct, not a leftover.
- **`app.safetyculture.com` URLs still resolve.** Examples below use `app.mitti.com`; you don't need to go back and change anything you already built.

## What can I do by connecting QRtub to Mitti?

Used with QRtub, a Mitti Destination lets you open specific inspection templates by scanning equipment, carry each [Item's](/help/key-concepts) own data into the inspection link so Mitti pre-fills answers, route different users (operators vs. managers vs. maintenance) to different inspection actions from the same QR code, and keep inspection access working even if you later switch to a different inspection platform.

## How does QRtub connect to Mitti?

QRtub connects to Mitti using **deep links** — URLs that open Mitti directly at a specific inspection, template, or report. No data is exchanged between the two systems: QRtub builds the URL, and Mitti reads it and handles the rest. Nothing is written back into QRtub from Mitti.

Mitti supports two deep link formats:
- **Mobile app** (`iauditor://`) — opens the Mitti app directly. The scheme still carries the old iAuditor name.
- **Web app** (`https://app.mitti.com/`) — opens in a browser. The older `app.safetyculture.com` domain also still works.

## How do I find my Mitti Template ID?

You need your Mitti Template ID before you can build any deep link in this guide. Get it from inside Mitti itself, not from QRtub:

1. Open the Mitti web app.
2. Navigate to your inspection template.
3. Copy the Template ID from the browser URL.

**Example:** `template_fcbc86fd41a74180921347e4be53bdf2`

If that URL doesn't make the ID obvious, or you need IDs for individual inspections, assets, or specific questions inside a template (needed further down this page), see [Mitti's guide on getting entity IDs](https://help.mitti.com/en-US/000076/) — QRtub doesn't surface these IDs anywhere in its own UI, so this lookup always happens inside Mitti.

## How do I set up a Destination that starts a new Mitti inspection?

Add a [Destination](/help/pages-overview) on the Item's Page whose URL is a Mitti deep link built from the Template ID you just copied. Mobile and web versions point to the same template using different URL formats.

### Mobile app deep link

**Destination Name:** Start Inspection
**Destination URL:** `iauditor://template/new_audit/<template_id>`

**Example:**
```
iauditor://template/new_audit/template_fcbc86fd41a74180921347e4be53bdf2
```

### Web app deep link

Use a web app deep link instead for users who prefer desktop or browser access — same template, opened in a browser rather than the mobile app.

**Destination Name:** Start Inspection (Web)
**Destination URL:** `https://app.mitti.com/inspection/new?templateId=<template_id>`

**Example:**
```
https://app.mitti.com/inspection/new?templateId=template_fcbc86fd41a74180921347e4be53bdf2
```

### Using QRtub URL Templates for bulk deployment

Use QRtub's [URL Template](/help/key-concepts) feature to insert each Item's own Template ID automatically, so one Destination configuration serves every piece of equipment in the Tub:

**Destination URL:** `iauditor://template/new_audit/{{item.templateID}}`

Store a different `templateID` value on each Item — forklifts point at the forklift template, excavators at the excavator template — and configure the Destination once. Every scan resolves `{{item.templateID}}` to that specific Item's value before the link opens, so 500 pieces of equipment need zero per-item Destination setup.

**If an Item's `templateID` field is empty**, QRtub does not fall back to a broken or blank URL — it treats the entire Destination rule as unresolved and skips it, moving on to the next Destination rule if one exists, or falling through to the default "no destination configured" state if this was the only rule. A scan on an Item with a missing `templateID` will not open a Mitti link at all, silently, until you fill in that field. This is worth testing for explicitly before a bulk rollout: check every Item you're deploying actually has the bound field populated.

## How do I pre-fill Mitti inspection answers with Item data?

Mitti lets you pre-fill specific inspection questions by adding the question's own **item ID** (not an arbitrary parameter name) as a query parameter on the deep link, with a QRtub field binding supplying the value.

### Mobile app pre-fill format

```
iauditor://template/new_audit/<template_id>?<item_id>=<response>
```

**Example with a QRtub URL Template:**
```
iauditor://template/new_audit/template_fcbc86fd41a74180921347e4be53bdf2?8f2f287e-be6e-470c-a2e2-a0fd8ab966ae={{item.assetID}}
```

Where:
- `template_fcbc86fd41a74180921347e4be53bdf2` is your Mitti template ID
- `8f2f287e-be6e-470c-a2e2-a0fd8ab966ae` is the question item ID inside your Mitti template — get this from [Mitti's entity ID guide](https://help.mitti.com/en-US/000076/), QRtub has no way to show it to you
- `{{item.assetID}}` is the QRtub field binding — replaced with that Item's own asset ID before the link opens

Deploy this one URL to 500 pieces of equipment and each scan substitutes that Item's own asset ID — no per-item setup. The same empty-field behavior described above applies here too: if `{{item.assetID}}` is blank for a given Item, the whole Destination is skipped rather than opening Mitti with a blank answer pre-filled.

### Pre-fill multiple questions

Chain additional `question_item_id=value` pairs with `&`:

```
iauditor://template/new_audit/<template_id>?<item_id_1>=<response_1>&<item_id_2>=<response_2>
```

**Example:**
```
iauditor://template/new_audit/template_fcbc86fd41a74180921347e4be53bdf2?8f2f287e={{item.assetID}}&a2e2a0fd={{item.location}}
```

**Important:** QRtub inserts field values exactly as stored and never URL-encodes them. A value containing a space, `&`, or `#` will break the deep link — store the value pre-encoded in the Item field if it needs one of those characters. Very long deep links (built from many pre-fill parameters, or long field values) may also stop working reliably — this is a limit of mobile browsers and OSes handling long custom-scheme URLs, not something QRtub imposes or checks for; as a rough guide, keep the whole URL under ~2000 characters.

## What happens if someone scans without Mitti installed?

`iauditor://` deep links only work if Mitti is installed on the device. Without a fallback, a scan on a device that doesn't have the app produces a blank screen or nothing at all — QRtub handles this automatically once you configure a Fallback URL.

When you enter an `iauditor://` URL as a Destination, an amber notice appears in the editor prompting you to add one.

**Recommended setup:**

| Field | Value |
|-------|-------|
| App link (Destination URL) | `iauditor://template/new_audit/{{item.templateID}}` |
| Fallback URL | `https://app.mitti.com/inspection/new?templateId={{item.templateID}}` |

**What happens at scan time:**
- Mitti installed → the app opens directly to the inspection template.
- Mitti not installed → after 2.5 seconds with no app response, QRtub navigates to your Fallback URL instead.

The Fallback URL supports the same `{{item.field}}` bindings as the app link, so it adapts per Item automatically. You can set a **Fallback Message** instead of a URL if there's no web equivalent (e.g. "Please install Mitti to access this inspection template") — how that message is displayed depends on where the Destination is used (a full styled page for a single-Destination Item vs. a plain browser alert for a Destination used as a button on a multi-Destination Page), so see [App Links & Fallback URLs](/help/app-links) for the exact behavior in each case.

**One more Mitti-specific gotcha:** iOS Safari opens `iauditor://` links correctly, but Chrome on iOS blocks custom-scheme deep links outright. If your team scans with a mix of browsers on iPhone, combine this Fallback URL with [Device Detection](/help/device-detection) so iOS Chrome users are routed straight to the web link instead of hitting a link that silently fails to open.

## What other Mitti destinations can a QR code link to?

Beyond starting a new inspection, you can point a Destination at an existing inspection, an asset profile, or a document.

**View or edit an existing inspection report** — the mobile deep link is the same for both viewing and editing, since Mitti's mobile app opens to the same interactive report screen either way; only the web link differs:
- **Mobile:** `iauditor://audit/<inspection_id>`
- **Web (view report):** `https://app.mitti.com/report/audit/<inspection_id>`
- **Web (edit inspection):** `https://app.mitti.com/inspection/<inspection_id>`

**With a QRtub URL Template**, store the latest inspection ID in each Item's `inspectionID` field so every scan routes to that Item's most recent inspection automatically:
```
https://app.mitti.com/report/audit/{{item.inspectionID}}
```

**Open an asset profile** — link a QRtub Item straight to its Mitti asset record:
- **Mobile:** `iauditor://asset/profile/<asset_id>`
- **Web:** `https://app.mitti.com/assets/<asset_id>`

**View a document or file** (web only):
- **Web:** `https://app.mitti.com/documents/<document_id>`

## Use Cases

**Equipment Inspections**
- Excavators, forklifts, vehicles
- Pre-start checklists
- Daily safety inspections

**Facility Inspections**
- Fire extinguishers
- Emergency equipment
- Building systems

**Multi-Audience Routing**
Create a Page with multiple Destinations so different people see different actions from the same scan:
- Operators see "Start Inspection" → starts a new inspection
- Managers see "View Report" → opens the latest report
- Maintenance sees "Asset History" → opens the Mitti asset profile

## Troubleshooting

**Mobile app doesn't open:**
- Confirm Mitti is actually installed on the device you're testing with.
- Verify the Template ID format is correct — it should match what you copied from the Mitti web app URL.
- Check that the signed-in Mitti user has access to that template.
- If you're testing on an iPhone, confirm you're using Safari, not Chrome — Chrome on iOS blocks `iauditor://` links (see the Fallback section above).

**Web link doesn't work:**
- Confirm the user is logged into the Mitti web app.
- Check that the user has permission to access the entity (template, inspection, or asset) the link points to.
- Confirm the ID in the URL is correct and complete.

**Data isn't pre-filling:**
- You need the specific **question item ID** from your Mitti template — not an arbitrary field name of your choosing. Get it from [Mitti's entity ID guide](https://help.mitti.com/en-US/000076/).
- Check the source Item field actually has a value. A missing or empty bound field doesn't produce a link with a blank answer — QRtub skips the whole Destination rule, so the scan won't open Mitti at all (see the empty-field note under URL Templates above).
- If the value contains a space, `&`, or `#`, it will break the URL — QRtub does not URL-encode field values automatically.
- If the deep link is very long (many pre-filled questions, or long field values), try shortening it — there's no exact QRtub-enforced limit, but mobile browsers and OSes can behave unreliably above roughly 2000 characters.

**Users can't access what the link opens:**
Deep links only work if the signed-in Mitti user has permission to the specific template, inspection, or asset the link points to. Confirm access inside Mitti before assuming the QRtub side is misconfigured.

## Resources

- [Mitti deep link documentation](https://help.mitti.com/en-US/000149/)
- [Get Mitti entity IDs](https://help.mitti.com/en-US/000076/)
- [App Links & Fallback URLs](/help/app-links)
- [Pages Overview](/help/pages-overview)
- [Key Concepts](/help/key-concepts)
- [Device Detection](/help/device-detection)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
