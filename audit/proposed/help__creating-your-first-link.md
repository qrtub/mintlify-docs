---
title: "Creating Your First Link"
description: "Generate your first Link, choose a type, and either redirect it, connect it to an Item, or leave it unassigned"
---

A Link is a QRtub-managed URL (`qrtub.com/r/x5fgd`) that you can put on a QR code, NFC tag, or
anywhere else. This page covers creating one, in the app, right now.

## What do I need before I start?

A QRtub account with at least one team. Go to
[app.qrtub.com](https://app.qrtub.com/login) and sign in — everything below happens inside the
dashboard, not on the marketing site. You don't need an Item, a Tub, or a Page set up first;
Links exist independently and can be created before any of those do.

## Where do I create a Link?

In the left sidebar, click **Access Link**. This opens the Links table for your team, showing
every Link you've created so far — empty, the first time. Click **Create Link** — this opens
the Create Access Link panel, which is where the rest of this page happens.

## What Link types are available?

Three, chosen with the **Strategy** selector at the top of the panel:

| Strategy (in the app) | Example | What it is |
|---|---|---|
| **Random** | `qrtub.com/r/aB3xk` | A 5-character, case-sensitive code, auto-generated, always under a `/r/` prefix. The default — pick this if you don't care what the code reads. |
| **Numbered** | `qrtub.com/cra001` | An optional prefix and suffix around a zero-padded sequence number, 1–10 digits (`CRA` + 3 digits + none → `CRA001`, `CRA002`, …). Some docs and the marketing site call this "ID-based" — it's the same thing as "Numbered" in the app. |
| **Custom** | `qrtub.com/mylink` | A slug you type yourself: 3–50 characters, lowercase letters, numbers, hyphens, or underscores only. Anything else is rejected. |

**Number of Links:** for Random and Numbered (in its default "Auto" mode), a **Number of
Links** field lets you create more than one at a time — up to **100 per request**. Numbered
also has a **Range** mode for filling a specific span of numbers (e.g. 1–500), capped at
**1,000 per request**. Custom links are always created **one at a time** — there's no bulk
option for a strategy where every slug has to be typed individually.

Numbered also has a **Specific** mode, for minting one exact number out of sequence (useful for
filling a gap left by a deleted or skipped Link).

If a custom slug or a specific number is already taken, the app tells you at creation time and
won't create the duplicate — nothing is silently overwritten.

## What can a new Link do right away, before I connect anything?

Two options, both available in the same Create Access Link panel:

- **Leave it unassigned.** This is a fully valid state, not a broken one. Print it, apply it to
  something in the field, and connect it later — see
  [The Print-First Workflow](/help/print-first-workflow) for what scanning an unconnected Link
  does (it does not show an error page).
- **Set a Destination URL.** The panel has an optional **Destination URL** field. Fill it in and
  the Link redirects straight there on every scan — no Item, no Tub, no Page required. This is
  the fastest path to a working, scannable Link if all you need is a simple redirect.

Connecting the Link to an Item (next section) or building a Page are further options once an
Item exists — they aren't required to get a working Link today.

## How do I connect a Link to an Item?

1. In the Links table, select one or more Links (checkboxes on the left of each row). Selecting
   at least one reveals a bulk-actions menu.
2. Choose **Assign to Item** from that menu. This opens a picker searching across every Item in
   every Tub on your team.
3. Pick the Item. The Link is now attached to it.

An Item can have more than one Link. A Link can only ever point at one Item (or one Destination
URL) at a time — assigning it elsewhere moves it, it doesn't add a second attachment.

Connecting a Link to an Item is also what unlocks a Page: Pages are switched on per Tub (in
that Tub's settings), and only Items whose Tub has that setting on show a Page when their Link
is scanned. See [Pages Overview](/help/pages-overview) for that setup.

## How do I get the QR code onto something physical?

Select the Link (or Links) you want in the table, then choose **Download QR Codes** from the
bulk-actions menu — one Link downloads a single PNG, several download as a zip. What you print
it on, and how you track that print run, is covered in
[Print Batches](/help/print-batches) and [The Print-First Workflow](/help/print-first-workflow).

## Next Steps

- [Pages Overview](/help/pages-overview) — turn a Link into a multi-Destination landing page
- [Key Concepts](/help/key-concepts#link-modes) — Direct Mode vs Page Mode in more depth
- [The Print-First Workflow](/help/print-first-workflow) — generate and print Links before your
  Items exist

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
