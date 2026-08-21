---
title: "Physical Media Management Basics"
description: "What QRtub tracks about the physical material your QR codes are printed on today, and what's still planned"
---

QRtub separates three things in a physical QR deployment: the **Item** you're tracking, the
**Link** that resolves when someone scans, and the **Media** — the sticker, plaque, sign, or
chip the code is printed on. Media itself isn't tracked as its own record yet. What QRtub
tracks today is the **print batch** each Link came from — the production run, not the
individual piece stuck to a piece of equipment.

## What are Item, Link, and Media?

QRtub recognises three distinct things in a physical deployment: the Item being represented,
the Link that's the QR code's digital pattern, and the Media it's printed on. Each has its own
lifecycle — full detail and a worked example in [Key Concepts](/help/key-concepts).

| Entity | What It Is | Example |
|--------|-----------|---------|
| **Item** | The thing being represented | An excavator, fire extinguisher, meeting room, product |
| **Link** | The QRtub-managed URL (digital pattern) | `qrtub.com/r/x5fgd` |
| **Media** | The physical material displaying the QR code | A vinyl sticker, metal plaque, billboard, real estate sign, printed ad, NFC chip |

## Why does QRtub track media separately from Links and Items?

Physical media is infrastructure with its own cost and lifespan, independent of the Link it
carries: a $50 plaque and a $5,000 billboard are different investments, a plaque can outlast
the equipment it's fixed to, and swapping damaged media for new media doesn't touch the Link
or the Item it's connected to. See [Key Concepts](/help/key-concepts) for the full rationale.

Replacing damaged media already works today, without any dedicated feature: print the same
Link again onto new media and swap it in. The Link — and the Item it's connected to — doesn't
change, so nothing needs reconfiguring in QRtub. What's planned (see below) is a *record* that
the swap happened, not the ability to do the swap itself.

## What does QRtub track about media today?

QRtub tracks print batches, not individual pieces of media. Exporting a print list from a Tub
(under **Access Media** in the main navigation) creates a batch that moves through
**Draft → Printing → Printed → Deployed** (you can step back one stage while it's in progress,
but Deployed is final). Each batch keeps the item and link counts, the CSV that went to your
printer, and a deployment status per link, so you can tell what's actually installed.

A few limits worth knowing before you rely on this:

- **Cost allocation per batch is not available** — a batch has notes and tags, but no cost
  field or reporting.
- **Links in a batch past Draft are protected from deletion**, since a code may already be
  printed and stuck to something. Links in a still-Draft batch can be deleted freely.
- **Deployed is final** — once a batch reaches Deployed you cannot step it back to Printed.
- If you want to record material or supplier per run today, use the batch's **notes** field —
  there's no dedicated field for it, but notes are free text kept with the batch permanently.

For the full walkthrough — creating a batch, adding notes/tags/a photo, updating deployment
status one at a time or in bulk, filtering Items by batch, and archiving — see
[Print Batches](/help/print-batches).

## What isn't tracked yet?

There's no record of what an individual QR code is printed *on* — no material type, cost,
durability, or installation location per piece, and no inventory count of media in stock or
in the field. These remain planned:

- Media as a distinct entity, with type and material per item
- Media Templates (reusable design templates)
- Media inventory tracking
- A recorded replacement history (the swap itself already works today — see above)
- Cost tracking and reporting

## Are Media Templates available?

No — not yet. Planned functionality includes reusable design templates:

- **System-provided templates** — standard sizes (4x6" sticker, 24x36" sign, etc.)
- **Custom templates** — save your own designs for consistent production
- **Multi-format** — one design, multiple Media types (sticker, sign, billboard)
- **Partner integration** — send directly to Media Partners for production

## Are Media Partners available?

No — there's no partner programme yet. Media Partners would be the print shops, signage
companies, and engravers who produce physical Media. What's planned:

- **Vetted producers** who understand QRtub workflows
- **Template compatibility** with QRtub Media Templates
- **Direct ordering** of Media through QRtub

Today, you produce Media through whichever supplier you already use.

## Does any of this depend on my plan?

No. Print batch tracking is available on every QRtub plan, and there's no Media-specific
gating. Starter, Professional, and Scale differ only in active-link ceiling (100 / 1,000 /
10,000), editor count, and numbered-sequence patterns — since a batch is built from the links
in a Tub, a plan's link ceiling is the only thing that indirectly limits batch size. Business
vs Personal pricing differs only in GST display, not features. See
[plans and pricing](https://qrtub.com/pricing).

## Media types and typical costs (general reference)

The figures below are general market ranges, not QRtub data — QRtub doesn't track material,
cost, or durability per piece yet (see above). Use them to plan a print run, not to look up
what's recorded against any of your Items.

### Vinyl Stickers
- **Cost:** $0.50 - $5 each
- **Durability:** Indoor 1-3 years, weatherproof outdoor 3-5 years
- **Use cases:** General equipment, short-term installations
- **Sizes:** 2x2" to 6x6" common

### Metal Plaques
- **Cost:** $20 - $100 each
- **Durability:** 10+ years
- **Use cases:** Permanent installations, high-value equipment
- **Materials:** Stainless steel, aluminium, engraved or printed

### Signs (Rigid)
- **Cost:** $50 - $500 each
- **Durability:** 5-15 years
- **Use cases:** Facility signage, wayfinding, permanent locations
- **Materials:** Aluminium, acrylic, PVC, coroplast

### Billboards & Large Format
- **Cost:** $500 - $5,000+ each
- **Durability:** 6 months - 3 years (depending on material and weather)
- **Use cases:** Marketing campaigns, real estate, public information
- **Sizes:** 4x8 ft to 48x14 ft

### Real Estate Signs
- **Cost:** $50 - $300 each
- **Durability:** 6 months - 2 years
- **Use cases:** Property listings, yard signs, directional signs
- **Sizes:** 18x24" to 24x36" common

### Printed Ads (Newspaper, Magazines, Postcards)
- **Cost:** $0.10 - $2 per piece
- **Durability:** Single use
- **Use cases:** Marketing campaigns, direct mail, publications
- **Production:** Offset printing, digital printing

### NFC Chips
- **Cost:** $1 - $10 each
- **Durability:** 5-10 years
- **Use cases:** Embedded in products, contactless tap-to-scan
- **Technology:** Can encode same Link as QR code

## How do I choose the right media?

Match media to the Item's environment, expected lifespan, budget, and use case — this is a
planning checklist, not a QRtub feature, since QRtub doesn't track media type per item yet.

**Environment:**
- Indoor → Standard vinyl stickers
- Outdoor → Weatherproof vinyl or metal plaques
- Harsh conditions → Engraved metal or ceramic

**Duration:**
- Temporary (< 1 year) → Vinyl stickers, printed materials
- Medium-term (1-5 years) → Weatherproof vinyl, rigid signs
- Permanent (10+ years) → Metal plaques, engraved signs

**Budget:**
- Low-cost deployments → Vinyl stickers, printed materials
- Infrastructure investments → Metal plaques, signs
- Marketing campaigns → Billboards, large format prints

**Application:**
- Equipment → Stickers, metal plaques
- Facilities → Signs, plaques
- Marketing → Billboards, printed ads, postcards
- Products → Stickers, NFC chips

## Related pages

- [Key Concepts](/help/key-concepts) — the full three-entity model, Link modes, and the
  print-before-link workflow
- [Print Batches](/help/print-batches) — create and manage a batch, add notes and photos,
  track deployment status, and filter Items by batch
- [The Print-First Workflow](/help/print-first-workflow) — order media in bulk before your
  Items exist

Guides for Media Templates, per-piece Media tracking, and Media Partners will follow once
those features exist. Batch management already has its own guide — see
[Print Batches](/help/print-batches) above.

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
