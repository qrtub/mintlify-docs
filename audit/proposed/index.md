---
title: "Welcome to QRtub"
description: "Print Once. Update Forever."
---

Physical QR codes are permanent once printed — but your systems aren't. QRtub lets you change what your codes do without reprinting.

A QRtub QR code encodes a **Link** — a QRtub-managed URL (like `qrtub.com/r/x5fgd`). A Link either redirects straight to one **Destination** (Direct Mode), or opens a **Page** listing several Destinations for the user to choose from (Page Mode). Because the Link is what's printed — not the Destination itself — you can generate Links and print codes before you know what they'll point to, then connect or change the Destinations at any time. See [Key Concepts](/help/key-concepts) for the full entity model (Item, Link, Media, Tub).

If you switch vendors in two years, update the Destination. Don't reprint 500 stickers.

## Why Choose QRtub?

### Can I print QR codes before my systems are ready?
Yes. Generate Links and print QR codes first; connect them to a Destination whenever you're ready. Professional printing needs bulk orders and lead time, so this lets you order codes without waiting for every system to be finalised. See [Print-Before-Link Workflow](/help/print-first-workflow) for the full walkthrough.

### Can one QR code link to multiple systems?
Yes, in Page Mode. Instead of one code doing one thing for one audience, a Link can open a Page listing several Destinations — inspections, maintenance, manuals, support — and the person scanning picks the one they need. One code replaces the multiple stickers a piece of equipment would otherwise carry.

### What happens if I switch vendors or systems later?
You update the Destination, not the physical code. Switch software, add a new system, or fix a broken link, all without touching the printed QR code. Two years from now, if 500 codes need to point somewhere new, you update 500 Destinations — you don't reprint 500 stickers.

### Which industries is QRtub built for?
Marine operations, construction equipment, rental fleets, lifesaving stations — QRtub is built for physical deployments where a QR code needs to keep working for months or years, not just for a single campaign.

## What can I do with QRtub today?

Available now:

- **Bulk Link generation** — create many Links at once for professional printing
- **Pages with multiple Destinations** — one Link, several routes, chosen by the person scanning
- **Print-before-link workflow** — print first, connect later, reassign a Link if you make a mistake (no reprinting)
- **URL Templates** — a Destination URL can use `{{item.fieldName}}` placeholders so each Item's own data is inserted automatically; configure the Destination once, deploy to every Item in a Tub. Values are inserted exactly as stored — QRtub doesn't URL-encode them, so keep fields used in URLs free of spaces, `&`, `?`, or `#`

Start here: [Creating Your First Link](/help/creating-your-first-link) walks through generating your first Link, downloading the QR code, and connecting it to an Item.

## Plans and limits

Every plan includes unlimited, free public scanning. The tiers differ on how many active Links and editors you get:

| Plan | Price | Active Links | Editors | Notable extras |
|---|---|---|---|---|
| Starter | $5/mo (or $50/yr) | Up to 100 | 1 | Random and custom link types, data export |
| Professional | $25/mo (or $250/yr) | Up to 1,000 | Up to 5 | 1 numbered sequence pattern, team management |
| Scale | $90/mo (or $900/yr) | Up to 10,000 | Up to 20 | Up to 5 numbered sequence patterns, private-or-public scanning |

[See full plan details and pricing →](https://qrtub.com/pricing)

**Not yet available:** API access, cross-account transfer of Links/Items between accounts, granular per-user permissions, and payment Destinations. If your workflow depends on one of these, ask before assuming it exists — [contact us](https://qrtub.com/contact).

## Find out more

<Columns cols={2}>
  <Card
    title="Industries"
    icon="building"
    href="/industries/civil-construction"
  >
    How construction, marine, cleaning, arboriculture, electrical test-and-tag, and council teams use QRtub — five industry guides, all built on the same Link/Page/Destination model above.
  </Card>
  <Card
    title="Integrations"
    icon="plug"
    href="/integrations/safetyculture"
  >
    QRtub doesn't sync data with other software — it builds URLs (including app deep links) that open your existing tools pre-filled with an Item's data. Guides cover Mitti (formerly SafetyCulture) and CMMS platforms.
  </Card>
</Columns>

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
