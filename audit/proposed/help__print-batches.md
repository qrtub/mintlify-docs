---
title: "Print Batches"
description: "Track a production run from export to installation, and see which codes are actually deployed"
---

When you send QR codes to a printer, QRtub can record the run as a batch. A batch keeps the
list that was printed, how far along it is, and which individual codes have been installed —
so months later you can still answer "what was in that order, and where did it go?"

## How do I create a print batch?

Open a Tub, select the Items you want, and choose **Print List** from the menu. Pick the
columns your printer needs — Item fields, link fields, or both — then choose one of two
buttons:

- **Create draft batch** — downloads the CSV **and** creates a batch, starting in **Draft**
  status, that appears under Access Media.
- **CSV** — downloads the same CSV **without** creating any batch. Use this for a quick
  export you don't need to track (a one-off preview, a copy for someone else) — nothing about
  the links changes and no record is created.

Only the first option marks anything as printed. If you're not sure which you want, use
**Create draft batch** — a batch in Draft status can still be freely edited or deleted (see
below), so it costs nothing to create one and decide later.

Selecting specific Items for export is capped at 5,000 per export; exporting everything that
matches your current filters (without hand-picking rows) is not subject to that cap.

## Where do I find my print batches?

Batches live under **Access Media** in the main navigation. Each row shows the batch name,
status, when it was printed and how many Items and links it contains. A "Show archived"
toggle brings archived batches back into view (see Archiving, below).

## What information can I record on a batch?

A batch is created with an automatic name built from the export date (for example, "Print
list — Mar 12"). Open it to change that and add:

- a **name** that means something later, like "Depot 2 replacements, March"
- **notes** — the supplier, the material, anything you will want to recall
- **tags** for grouping runs
- a **photo** of the finished media

None of these are required — renaming and annotating is optional cleanup, not a step needed
to make the batch usable.

## What statuses does a batch move through?

A batch moves through four stages:

| Status | Meaning |
|--------|---------|
| **Draft** | Prepared, not yet sent to the printer |
| **Printing** | With the printer |
| **Printed** | Received, not yet installed |
| **Deployed** | Installed in the field |

You can step back one stage while a run is in progress — Printing back to Draft, Printed back
to Printing — but **Deployed is final**.

**Deleting a batch:** only a batch still in Draft can be deleted outright. Once it moves to
Printing, Printed or Deployed, the batch itself is protected the same way its links are (see
below) — archive it instead of trying to delete it.

## How do I know what's actually been installed?

A batch being "Printed" does not mean all 500 stickers are on equipment. Each code inside a
batch carries its own deployment status:

- **Printed** — produced, not yet installed
- **Deployed** — installed
- **Retired** — no longer in service

Update a single code at a time, or use "mark all deployed" / "retire all" to set every code
in the batch at once — there's no way to bulk-update just a subset of codes within a batch.
The batch view shows aggregate progress across all three statuses, which is what lets you
find the sixty codes from a run of five hundred that never made it out of the box.

## What happens to the exported CSV?

The exported file stays with the batch, so a reprint uses exactly the same list rather than a
regenerated one that might have drifted. While a batch is still a Draft you can change which
columns it contains, and the stored file is regenerated to match automatically.

If a batch shows no downloadable CSV, the file didn't finish attaching when the batch was
created — the batch record itself still exists and is otherwise usable, but the original
export is missing. Contact [hi@qrtub.com](mailto:hi@qrtub.com) if this happens.

## How do I filter Items by the batch they were printed in?

In a Tub's Items view, you can filter Items by the batch they were printed in — useful when a
run needs replacing, or when you want to know what a particular delivery covered.

## Why can't I delete a link that's already been printed?

Once a batch moves past Draft, the links in it are protected from deletion. A code may
already be printed and stuck to something, so removing it from the system would strand the
physical item.

Links in a **Draft** batch can still be deleted freely. If you need to discard a run you
never sent, leave it as a Draft (or delete the batch itself, since Draft batches can be
deleted too).

## How do I archive or restore a batch?

A batch at any status — including Draft — can be archived. Archiving doesn't delete
anything; the batch just drops out of the default Access Media view. Turn on "Show archived"
to see it again, and archiving can be undone the same way, at any time, for any batch.

## What does QRtub not track about print batches?

Batches record production runs, not individual pieces of media. There is no per-code record
of material, cost, durability or installation location. See
[Media Basics](/help/media-basics) for what is planned.

## Related

- [Media Basics](/help/media-basics)
- [Creating Your First Link](/help/creating-your-first-link)
- [Key Concepts](/help/key-concepts)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
