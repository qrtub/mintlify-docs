---
title: "Pages Overview"
description: "What a Page is, how Page Mode differs from Direct Mode, and how to turn one on for a Tub"
---

A Page is a mobile-friendly landing page that a Link opens instead of redirecting straight to
one URL. It shows one or more **Destinations** — buttons that each route to a URL you set, such
as an inspection form, a maintenance system, or a manual — and the person who scanned taps the
one they need. This replaces one-QR-code-per-system: instead of three stickers on one machine,
one Link carries all three options.

## What is a Page?

When someone scans a QR code linked to a Page, they land on that page and see every
Destination you've added, each as its own button. On a piece of equipment, that might look
like:

- "Start Inspection" → Opens Mitti (formerly SafetyCulture)
- "Log Maintenance" → Opens CMMS system
- "View Manual" → Opens documentation

Everyone who scans sees the same full list and picks the one relevant to them — a Page doesn't
filter which Destinations different people see unless you add
[Conditional Visibility](/help/conditional-visibility) rules to it.

## How is Page Mode different from Direct Mode?

A Link (and, by default, every Item in a Tub) runs in one of two modes:

| Mode | Behaviour | Best for |
|------|-----------|----------|
| **Page Mode** | Opens the landing page described above, with multiple Destinations | Multi-system integration, audience self-selection |
| **Direct Mode** | Redirects immediately to a single Destination, no page in between | Simple redirects, single-purpose codes |

You can switch between modes at any time — it changes how the *next* scan behaves, and never
requires reprinting the QR code. Switching Page Mode off doesn't delete the page you built:
the layout and Destinations stay saved, and reappear if you switch back on. See
[Key Concepts](/help/key-concepts#link-modes) for how these two modes fit into the wider
Item / Link / Media model.

## Why use Page Mode instead of Direct Mode?

Page Mode is worth the extra tap when one code needs to do more than redirect to a single
place. Three concrete reasons to choose it:

**One code, every system** — Connect one QR code to multiple software systems (an inspection
tool, a CMMS, documentation) instead of printing a separate code per system. Configure it once
per Tub; every Item routes to its own pre-filled Destinations.

**Audience routing (self-select, not automatic)** — Show every Destination on the same page
and let different people tap what's relevant: staff tap operational tools, customers tap
support info, technicians tap maintenance access. Everyone sees the same list — Page Mode
doesn't hide Destinations from specific viewers on its own. If you need to actually restrict
which Destinations appear per equipment type, device, or item status, that's what
[Conditional Visibility](/help/conditional-visibility) is for.

**Branded presentation** — Every scan opens a page you control: customise colours, logos, and
descriptions in the [Page Editor](/help/building-a-page).

If none of that applies — the code only ever needs to go to one place — Direct Mode is
simpler and skips the extra tap for the person scanning.

## How do I create a Page?

Pages are switched on **per Tub, not per Link**:

1. Open the Tub's settings and turn on the page option (labelled **Show a profile page** in
   the current app). Every Item in that Tub then opens a page when scanned; left off, scans
   pass straight through to a single Destination. An individual Item can override its Tub's
   setting if that one item needs to behave differently.
2. [Create an Item and generate or assign a Link](/help/creating-your-first-link) in the Tub.
3. [Add sections and Destinations](/help/building-a-page) to the page in the Page Editor.

Assigning a Link to an Item only attaches a URL pattern — on its own it does not produce a
page. If a Tub is set to pass-through (Direct Mode), its Items keep redirecting no matter how
many Links you assign; the page toggle in step 1, not the Link itself, is what turns the page
on.

Page Mode is available on every plan, including Starter — it isn't a higher-tier feature.

## Related

- [Creating Your First Link](/help/creating-your-first-link) — generate a Link and connect it
  to an Item
- [Building a Page](/help/building-a-page) — lay out sections, add Destinations, and bind Item
  data in the Page Editor
- [Key Concepts](/help/key-concepts) — the Item / Link / Media model and Link Modes in full
- [Conditional Visibility](/help/conditional-visibility) — show or hide Destinations based on
  Item data or device

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
