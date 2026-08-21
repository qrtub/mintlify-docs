---
title: "Key Concepts"
description: "QRtub's three-entity model — Item, Link, and Media — plus Link Modes, Destinations, and Tubs"
---

QRtub separates three things most QR code tools treat as one: the **Item** you're tracking,
the **Link** the QR code resolves to, and the **Media** the code is printed on. Each has its
own lifecycle. This page defines all three, plus Link Modes, Destinations, and Tubs — the
vocabulary the rest of the docs assume you know.

## What are QRtub's three entities: Item, Link, and Media?

### Item

**What it is:** The thing being represented or tracked — an excavator, a fire extinguisher,
a meeting room, a product, a building system, a rental tool.

**Key point:** Items are what you're actually managing. They have details like serial
numbers, descriptions, status, and custom fields you define inside a Tub (see below).

---

### Link

**What it is:** The QRtub-managed URL that a QR code encodes.

**Examples:**
- `qrtub.com/r/x5fgd` (Random)
- `qrtub.com/exc001` (ID-based)
- `qrtub.com/boardroom` (Custom)

**Key point:** Links exist independently of Items — you can generate a Link and print its QR
code before the Item it will represent even exists in your system. That's the
print-before-link workflow (below).

**Why this matters:**
- Generate Links for professional printing before your Item records are finalised.
- Reassign a Link to a different Item if you make a mistake — no reprinting needed.
- Deleting an Item does not delete its Link. The Link is released back to your unassigned
  pool, so a Media piece already carrying that code in the field keeps working and can be
  reassigned.
- Change where a Link points at any time without touching the physical code — see
  [Update Without Reprinting](#can-i-change-where-a-qr-code-points-without-reprinting-it)
  below.

---

### Media

**What it is:** The physical material the QR code is displayed on — a vinyl sticker, a metal
plaque, a billboard, a real estate sign, a printed ad, an NFC chip.

**Key point:** The code has to be printed or displayed on something physical, and that
material has its own lifecycle, separate from the Link it encodes and the Item it
represents. A metal plaque might outlast the excavator it's bolted to.

**What's tracked today:** Exporting a print list from a Tub creates a **Media Batch** — a
tracked production run with a name, notes, tags, and a photo, moving through
**Draft → Printing → Printed → Deployed**. A batch keeps the original CSV sent to your
printer and a per-code deployment status, and can be archived once finished. Full detail:
[Physical Media Management Basics](/help/media-basics).

**Not tracked today:** QRtub doesn't record what an individual piece of Media is made of,
what it cost, how long it's expected to last, or where it's installed — there's no
per-piece material type, cost, durability rating, or install-location field, and no
inventory count. If you need those records, keep them in your own asset system for now; the
Batch reference above is what links a printed piece back to the run it came from.

---

## What are Direct Mode and Page Mode?

A Link operates in exactly one of two modes:

### Direct Mode

**How it works:** The Link redirects immediately to a single Destination.

**When to use:** A simple one-to-one redirect where no user choice is needed.

**Example:** A QR code on a poster → an event registration page.

---

### Page Mode

**How it works:** The Link opens a Page — a mobile-friendly landing page where users pick
from multiple Destinations.

**When to use:** One QR code needs to serve more than one purpose, or different audiences
need different information from the same code.

**Example:** A QR code on equipment offers:
- "Start Inspection" → an inspection tool like Mitti (formerly SafetyCulture)
- "Log Maintenance" → your CMMS
- "Operator Manual" → PDF documentation
- "Contact Support" → a support form

**Key benefit:** One physical QR code, multiple systems, no code proliferation.

---

## How do the three entities and two modes fit together?

Here's one deployment worked through, using every concept above:

```
ITEM: Excavator #203
- Serial: EXC-2024-203
- Location: Site A
- Status: Active
▼
is assigned to
▼
LINK: qrtub.com/exc203
- Type: ID-based
- Mode: Page
- Created: Jan 2025
▼
is encoded on
▼
MEDIA: a stainless steel plaque, part of Batch #47 (500 pieces, PrintCo),
       bolted to the excavator's left cab door
```

QRtub tracks the Item's fields, the Link's type and mode, and the Batch a piece of Media
came from — that's the real, stored data. The plaque's material and install location above
are just there to make the physical object concrete; QRtub itself doesn't store them (see
"Not tracked today" under Media).

Each entity has its own lifecycle: the excavator might be sold, the Link persists and can be
reassigned to whatever replaces it, and the plaque might outlast the excavator entirely and
get moved to new equipment.

---

## What is a Destination?

**What it is:** Where a user ends up when they interact with a Link.

- **In Direct Mode:** the Link has exactly one Destination and redirects to it immediately.
- **In Page Mode:** the Page displays every Destination as a button or card for the user to
  tap.

**Current Destination type:** External URLs — any web address, including your own systems,
documentation, or a third-party tool.

### URL Templates

**What they are:** Destination URLs with field placeholders — `{{item.fieldName}}` — that
QRtub fills in with the scanning Item's own data before the redirect happens.

**Example:** the template `app.com/inspect?id={{item.assetID}}&name={{item.name}}` resolves,
for a specific Item, to `app.com/inspect?id=EXC-203&name=Excavator 203`.

**Two gotchas worth knowing before you deploy at scale:**
- Values are inserted **exactly as stored** — QRtub does not URL-encode them. A field
  containing a space, `&`, `?`, or `#` produces a broken link, as the space in
  "Excavator 203" would above. Keep fields used inside URLs free of those characters, or
  store a pre-encoded version alongside the display name.
- A field that's empty or not set on a given Item inserts as an empty string, not an error —
  the URL still resolves, just with that parameter blank.

URL Templates are what make bulk deployment practical: configure the template once, apply it
to hundreds or thousands of Items, and each QR code personalises itself automatically. Full
reference, including conditional visibility and all available fields:
[Using Fields in Pages](/help/using-fields).

---

## What is a Tub?

**What it is:** A category-based workspace for organising Items — think folder or asset
type, with custom fields and a Page template attached.

**What a Tub provides:**
- **Custom fields** — define exactly what data each Item type needs.
- **Page templates** — set how Pages look for this category.
- **Organised management** — keep equipment separate from products, facilities separate
  from tools.

**Examples:**
- Tub "Heavy Equipment" → custom fields: Serial #, Make, Model, Service Hours, Site
- Tub "Meeting Rooms" → custom fields: Room #, Floor, Capacity, AV Equipment
- Tub "Fire Safety Equipment" → custom fields: Type, Location, Inspection Due, Certification #

**Key point:** Items live inside Tubs. Links live at the account level and can be assigned
to Items in any Tub.

---

## What is the print-before-link workflow?

Print-before-link means generating and printing a batch of Links before the Items they'll
represent exist in your system, then connecting each one to an Item once it's known — so a
bulk print run isn't blocked on every record being finalised first.

This page defines the entities the workflow relies on; for the full walkthrough — including
what happens when someone scans a code that hasn't been connected to an Item yet, and
practical advice for running it at scale — see
[The Print-First Workflow](/help/print-first-workflow).

---

## Can I change where a QR code points without reprinting it?

Yes. Because a Link is a separate entity from both the Item and the Media it's printed on,
updating a Destination changes what every physical copy of that code does — the code itself
never has to be reprinted. Typical cases: switching inspection vendors, adding a new
Destination to an existing Page, or fixing a Destination whose target URL moved. Update the
Destination once; every deployed code picks up the change immediately.

---

## Does QRtub replace my other software?

No. QRtub is a connection layer between your physical items and your digital systems, not a
replacement for any of them:

- **Not asset management software** — it connects to yours.
- **Not inspection software** — it links to tools like Mitti, it doesn't provide inspection
  features itself.
- **Not maintenance tracking** — it integrates with your CMMS rather than tracking
  maintenance itself.
- **Not a compliance platform** — it routes to your compliance tools.

What it does do: manage the QR code infrastructure — Links, Pages, and print batches — that
connects a physical item to whichever digital systems you already use.

---

## Common Questions

### Why can't I just use a regular QR code generator?

Regular generators hardcode a URL into the QR code itself. Once printed, it can't be
changed. QRtub decouples the physical code from its Destination, so you can update where a
code points without reprinting.

### What's the difference between a Link and a QR code?

A Link is the QRtub-managed URL (`qrtub.com/r/xxxxx`). A QR code is one way of encoding that
Link as a scannable pattern — you could also type the Link, embed it in NFC, or use RFID.

### Do I need an Item to use a Link?

No. A Link can exist without an Item — useful for a simple redirect. Connecting a Link to an
Item unlocks Pages, URL Templates, and Tub-based organisation.

### Can one Link connect to multiple Items?

No. A Link connects to either one Item (Page Mode) or one Destination (Direct Mode). An
Item, however, can have multiple Links.

### What happens if a Link hasn't been connected to an Item yet and someone scans it?

See [The Print-First Workflow](/help/print-first-workflow#what-happens-if-someone-scans-a-tag-early)
for the full answer — in short, an unconnected code does not error out.

### What happens to a Link if I delete its Item?

The Link isn't deleted. It's released back to your unassigned pool, so any Media already
printed with that code keeps resolving and can be reassigned to a replacement Item.

### What happens to my scan data if I replace damaged Media?

New Media encoding the same Link resolves exactly as the old one did — the Item connection
is preserved because it's the Link, not the physical piece, that's connected to the Item.

---

## Next Steps

**Related Help Pages:**
- [The Print-First Workflow](/help/print-first-workflow)
- [Physical Media Management Basics](/help/media-basics)
- [Using Fields in Pages](/help/using-fields)
- [Creating Your First Link](/help/creating-your-first-link)
- [Pages Overview](/help/pages-overview)
- [Conditional Visibility](/help/conditional-visibility)

**Integration Guides:**
- [Mitti Integration](/integrations/safetyculture)
- [CMMS Systems Integration](/integrations/cmms-systems)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
