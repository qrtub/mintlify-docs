---
title: "The Print-First Workflow"
description: "Order your tags in bulk before the assets exist, apply them as gear arrives, and connect them when you are ready"
---

Most tools assume you create a record, generate a code for it, print that code, and go and
stick it on something. That order works for one asset. It falls apart at two hundred.

The print-first workflow inverts it: the codes exist before the records do. Elsewhere in these
docs (see [Key Concepts](/help/key-concepts)) this same process is called **print-before-link** —
same workflow, same steps below, just two names for it.

## Why can't a single tag be printed on demand?

Because durable tags are made in production runs, not one at a time, so **you physically cannot
produce one tag on demand at any sensible cost**. Photo anodised aluminium plates are laid up and
cut as a sheet. Engraved tags are set up once and produced as a batch. Even ordinary
UV-resistant vinyl comes with minimum quantities and a lead time.

Example: a run of photo anodised aluminium plates being cut, each one numbered before any of the
equipment it will be fixed to has been recorded anywhere — the sheet has to be machined as one
job, so the numbers get allocated first.

![A CNC machine cutting a sheet of photo anodised aluminium asset plates, each carrying a QR code, a large asset number and a readable link. The visible plates run in sequence: BOU027, BOU028, BOU029.](/images/print-first-plates.jpg)

That has a consequence people usually discover halfway through a rollout: if your process
requires the asset record to exist before the tag can be made, you are stuck with a choice
between delaying the order until every detail is final, or printing something disposable.

Meanwhile the gear itself arrives over weeks. Equipment turns up before anyone has decided what
it is called, who owns it, or which system tracks it. Print-first accepts both of those
realities — batch manufacturing on one side, gear arriving piecemeal on the other — instead of
fighting them.

## How it works

**1. Generate the Links first.** Create as many as you need before any Items exist — each one is
a real, resolvable URL from the moment it's created. See [Creating Your First
Link](/help/creating-your-first-link) for the actual steps. One generation request creates up to
**1,000 Links**; for a bigger run, repeat the request. Your plan also caps how many *active*
Links you can hold at once — Starter up to 100, Professional up to 1,000, Scale up to 10,000 (see
[current plans](https://qrtub.com/pricing)) — and an unassigned print-first Link still counts
against that cap, so size the order to your plan, not just to the print run.

**2. Send the batch to be produced.** Export the list and give it to whoever makes your tags —
plates, engraved labels, weatherproof vinyl, NFC inlays, the medium doesn't matter, because all
it has to carry is a URL. Exporting creates a tracked batch, the same mechanism described in
[Print Batches](/help/print-batches). Do this from the account-level **Links** list (where you
generated them in step 1), not from inside a Tub — a Tub's print-list export needs Items to
already exist to select from, and at this point none do yet.

**3. Apply tags as gear arrives.** Fix a tag to each asset as it lands, in whatever order things
turn up. No decisions required beyond "this tag is now on this machine."

**4. Connect when you are ready.** Create the Item and connect it to the tag already on it.
Connecting itself is one action — pick the tag, pick or create the Item it belongs to — though
filling in that Item's own fields still takes as long as your Tub's fields take to fill in.

## What happens if someone scans a tag early?

An unconnected code does not 404 — this is usually the detail that decides whether the workflow
is practical for a given rollout:

- Someone on your team who scans it gets an option to assign it there and then, from their
  phone (this is what QRtub calls **claim-on-scan**). The person applying tags can connect it on
  the spot without going back to a desk.
- Anyone else gets a QRtub page that says the link hasn't been configured yet and invites them to
  sign in (if they're the owner) or learn what QRtub is — not an error page, but not a neutral
  blank page either.

So a tag applied on Monday and connected on Friday is not a dead code in the meantime. It is
simply not allocated yet.

## What if a tag ends up on the wrong item, or the item is gone?

A Link can be reassigned to a different Item at any point, so putting a tag on the wrong machine
costs an edit, not a reprint. The same applies at the end of an asset's life: deleting an Item
does not delete its Link — the Link is released back to your unassigned pool, so a tag already
fixed to something in the field keeps resolving and can be reused on the next thing that arrives.
See [Key Concepts](/help/key-concepts) for how Items, Links and Tubs relate.

## Things worth doing when you order a print run

### Print the link in text as well as the code

Codes get scratched, painted over, caked in mud, or scanned in bad light on a cracked screen. A
short readable URL under the code means someone can still type it.

### Make the tag number and the link match

If the plate reads `HPP021` and the link is `qrtub.com/hpp021`, there is no second identifier for
anyone to remember, mistype, or map back to a spreadsheet. Crews already refer to gear by the
number on the tag.

### Match the medium to the environment, not the budget

Tags on a boat or exposed to salt and constant wash need a different specification from tags on
a generator in a yard. You can run several media grades against one numbering scheme, because the
tag only carries a URL — what it's made of is a separate decision from what it points to. See
[Physical Media Management Basics](/help/media-basics) for typical costs and durability by
material.

### Order more than you need

The marginal cost of extra tags in a run is small compared with setting up a second run three
weeks later. Unallocated Links sit in the pool (up to your plan's active-link cap, see "How it
works" above) until something turns up to attach them to.

## Where does a Link point?

A Link can go straight to a single Destination, or open a Page with several Destinations to
choose from. Either way you set it up once for the whole category rather than per tag, using a
URL template that fills in each Item's own data — for example:

```
https://example.com/assets/{{item.serial_number}}
```

Every tag then resolves to its own asset's record without being configured individually. That's
what makes the workflow viable at a few thousand tags rather than a few dozen — see [Using
Fields](/help/using-fields) for the full binding syntax and examples.

One caution: field values are inserted into the template **exactly as stored** — QRtub does not
URL-encode them. A field containing a space, `&`, `?` or `#` (a serial number with a slash in it,
for instance) will produce a broken link. Keep the fields you bind into a template free of those
characters, or store a pre-encoded version alongside the display value.

Because the destination is held by QRtub rather than baked into the code, you can change where
every tag points later — a new system, a new URL structure — without touching anything physical.

## Limits

- **Generation:** up to 1,000 Links per generation request. Need more? Repeat the request.
- **Active Links per plan:** Starter up to 100, Professional up to 1,000, Scale up to 10,000 —
  print-first Links still sitting unassigned in the pool count toward this. See [current
  plans](https://qrtub.com/pricing).
- **One Item per Link:** a Link connects to one Item at a time (Page mode) or one Destination
  (Direct mode) — it doesn't fan out to several Items. One Item can hold several Links.
- **No automatic URL encoding:** a bound field containing a space or `&`, `?`, `#` breaks the
  URL it's inserted into — see "Where does a Link point?" above.

## Related

- [Key Concepts](/help/key-concepts)
- [Creating Your First Link](/help/creating-your-first-link)
- [Print Batches](/help/print-batches)
- [Using Fields](/help/using-fields)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
