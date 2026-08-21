---
title: "CMMS Systems Integration"
description: "Connect QR codes to your maintenance system using deep links, web URLs, or QRtub's URL Templates"
platform: "CMMS"
featured: false
---

Connect a QRtub Destination to your Computerised Maintenance Management System (CMMS) — UpKeep, Fiix, Maintenance Connection, or any other platform — so scanning equipment opens its asset record, work order, or maintenance history. QRtub builds the URL; your CMMS handles everything after that. No data is exchanged between the two systems, and nothing writes back into QRtub.

## Overview

Scanning equipment with a CMMS Destination configured can:

- Open that asset's maintenance record
- Start a new work order pre-filled with the asset's ID
- Show service history for that specific piece of equipment
- Route different people (operators vs. technicians) to different CMMS screens from the same code

Each of these is just a URL your CMMS already understands — QRtub's job is constructing that URL correctly and routing a scan to it. The rest of this page covers the three ways to build that URL, then testing, troubleshooting, and known platform support.

## How do I connect QRtub to my CMMS?

There are three ways to point a Destination at your CMMS, in order of setup effort. All three are entered the same way: in the Item's Page, add a Destination, and set its Destination URL.

### Option 1: Direct deep links (if your CMMS supports them)

Some CMMS platforms provide a deep link scheme that opens their mobile app directly to an asset or work order:

```
yourCMMS://asset/[ASSET_ID]
yourCMMS://workorder/create?asset=[ASSET_ID]
```

Check your CMMS's own developer or integration documentation for whether it supports this and what the scheme looks like — QRtub has no way to confirm a third-party app's deep link format.

**Deep links only work if the app is installed.** If someone scans without the CMMS app on their device, a bare deep link does nothing. Always pair a deep link Destination with a **Fallback URL** (the CMMS's web equivalent) or a **Fallback Message** — QRtub detects any non-`https://` Destination URL, shows an amber prompt to configure a fallback, and automatically falls back after a 2.5-second timer if the app doesn't open. See [App Links & Fallback URLs](/help/app-links) for the full mechanism and setup steps.

### Option 2: Web-based access

Most CMMS platforms have a web interface reachable by a standard `https://` URL — no deep link scheme needed:

```
https://app.upkeep.com/assets/[ASSET_ID]
https://app.fiix.io/web/wo/create?assetId=[ASSET_ID]
```

This is the simplest option: it's a plain Destination URL, works in any browser, and needs no fallback configuration. Confirm the exact URL structure in your CMMS's documentation — the examples above illustrate the shape, not a verified endpoint for either product.

### Option 3: URL Templates (for bulk deployment)

If you're deploying more than a handful of codes, use QRtub's URL Templates to build each asset's CMMS URL automatically from Item data, instead of typing one URL per Item.

**In QRtub:**
- Item field `cmmsAssetID`: `PUMP-042`
- Destination URL template: `https://app.yourcmms.com/assets/{{item.cmmsAssetID}}`

**Resolved URL when scanned:**
`https://app.yourcmms.com/assets/PUMP-042`

Configure the template once, on one Destination. Import or create any number of Items with their own `cmmsAssetID` values, and every code routes to the correct asset automatically — no per-Item URL configuration. The field name `cmmsAssetID` is just an example; use whatever custom field name holds the asset identifier in your Tub, and reference it consistently across every Destination that needs it.

**Two things to know before you rely on this:**

- **QRtub does not URL-encode field values.** They're inserted into the URL exactly as stored. A value containing a space, `&`, or `#` — for example `PUMP 042` instead of `PUMP-042` — will break the generated link. Keep asset ID fields free of those characters, or store a pre-encoded version.
- **A blank field doesn't produce a broken link with an empty placeholder — it produces no destination at all.** If `cmmsAssetID` is empty for a given Item, that Destination fails to resolve, and scanning shows a "link not ready" page instead of opening a malformed CMMS URL. Check the Destination editor's preview panel before deploying — it flags any field that won't resolve, so you can catch empty asset IDs before printing rather than after.

## Testing before mass deployment

Ensure your QRtub Item fields match your CMMS's own asset identifier format — same format, same casing, all required parameters included — before generating a batch of codes.

Before deploying a large batch (500 codes, for example):

1. Test on mobile devices — both iOS and Android, since deep link behavior differs by platform and browser.
2. Verify CMMS authentication works from a fresh, logged-out session, not just your own already-authenticated device.
3. Confirm any deep links actually open the app, and that the fallback triggers correctly when the app isn't installed.
4. Test from different network conditions — a slow connection can make the 2.5-second fallback timer feel like the app "isn't working" when it's actually about to fall back correctly.
5. Use the Destination editor's preview panel to check for binding errors across a sample of Items, not just the one you built the template against.

**Vendor lock-in protection:** this testing effort pays off beyond the initial rollout. If you switch CMMS vendors later, you update the Destination URLs in QRtub — the physical QR codes never change and never need reprinting.

## Which CMMS platforms support deep links?

Deep link availability is set entirely by the CMMS vendor, not QRtub, and changes as vendors update their apps. As of this writing:

| Platform | Web access | Deep links | Notes |
|----------|------------|------------|-------|
| UpKeep | Yes | Check UpKeep's docs | Mobile-first platform |
| Fiix | Yes | Check Fiix's docs | Web and mobile apps |
| Maintenance Connection | Yes | Not documented | Web-based |
| Hippo CMMS | Yes | Check Hippo's docs | Cloud-based |
| eMaint | Yes | Not documented | Enterprise platform |

Every platform in this table supports Option 2 (web-based access) regardless of deep link status, so web URLs are always a safe fallback while you confirm deep link support directly with your vendor.

## Troubleshooting

**"Page not found" or asset-not-found errors:**
- Check that asset IDs match exactly between QRtub's Item field and the CMMS — including case and any leading zeros or prefixes.
- Verify the URL structure against your CMMS's current documentation; vendors change URL paths without notice.
- Test with a static, hand-typed URL first (no `{{item.field}}` placeholder), then add the URL Template once the static version works.

**Authentication issues:**
- Confirm the user is logged into the CMMS app or web session — a Destination URL can't authenticate on the user's behalf.
- Check whether your CMMS requires a session token or app-specific login that a bare URL can't carry.
- If your CMMS supports SSO, consider it to reduce login friction on scan.

**Scan shows "link not ready" instead of the CMMS page:**
- This means the Destination's URL Template referenced an Item field that was empty or missing for that specific Item — not a CMMS-side error. Check the Item's `cmmsAssetID` (or equivalent) field has a value, and check the Destination preview panel for binding errors.

## Combining CMMS with other systems

A single QR code isn't limited to one destination. With Page Mode turned on for the Tub (see [Pages Overview](/help/pages-overview)), one Item can offer a CMMS Destination alongside inspection, documentation, or support links — for example "Start Inspection" (Mitti), "View Maintenance" (CMMS), "Equipment Manual" (a PDF link). See [Key Concepts](/help/key-concepts) for the full multi-Destination pattern and [Mitti Integration](/integrations/safetyculture) if inspections are one of the systems you're combining.

## Resources

- [App Links & Fallback URLs](/help/app-links) — deep link fallback mechanics, required reading for Option 1
- [Pages Overview](/help/pages-overview) — turning on Page Mode for multi-Destination codes
- [Key Concepts](/help/key-concepts) — Items, Tubs, Destinations, and URL Templates explained
- Your CMMS vendor's own documentation — for deep link support and current URL structures

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
