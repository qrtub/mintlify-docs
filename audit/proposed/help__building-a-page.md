---
title: "Building a Page"
description: "Use the Page Editor to lay out sections, bind Item data, and theme the page people see when they scan"
---

The Page Editor is where you decide what someone sees when they scan a QR code. You build
one layout — the **base template** — for a Tub, and every Item in that Tub renders that
template with its own data. (New to Tubs, Items, and Destinations? See
[Key Concepts](/help/key-concepts) first.)

## Before you start

Pages must be switched on for the Tub. Open the Tub's settings and turn on the page option
(labelled **Show a profile page** in the current app). If it is off, scans go straight to a
single Destination and there is no page to edit. See
[Pages Overview](/help/pages-overview).

## Opening the editor

From the Tub's settings, open the **Profile page** tab and choose **Edit profile page**. The
editor opens in a new tab.

## The layout

The page sits in the middle. A panel on each side does the work.

The **left panel** has three tabs, and only one is visible at a time:

| Tab | What it does |
|-----|--------------|
| **Components** | The palette of sections you can add, searchable and grouped by category |
| **Data** | The fields you can bind. Drag one onto a setting to bind it |
| **Structure** | The page as a tree — reorder, nest, and see which sections carry conditions |

The **Properties** panel on the right shows settings for whatever is selected, or settings
for the whole page when nothing is selected.

## Adding and arranging sections

1. On the **Components** tab, find a section and add it
2. Select it on the page to open its settings in **Properties**
3. Switch to the **Structure** tab to reorder, or use the up and down controls

Container and Card can hold other sections, so you can group things together and style the
group as a unit.

Undo and redo are in the top bar, holding up to 50 steps back. Switching to a different Item
clears the undo history — finish an edit before you go looking at another Item.

## Section types

Seventeen sections are available, grouped by category in the palette.

| Category | Sections |
|----------|----------|
| **Data Display** | ItemHeader, ActionLink, KeyValue, SpecGrid, Tags, ContactInfo, TubInfo |
| **Content** | Hero, Text, Banner |
| **Layout** | Container, Card, Spacer, AdminToolbar |
| **Interactive** | Button, Link |
| **Media** | ImageSection |

Two are worth calling out:

- **ActionLink** is the button that sends someone to a Destination. If its URL depends on a
  field the Item has not filled in, the section hides itself rather than showing a broken
  link, and the editor tells you which field is missing.
- **AdminToolbar** is a bar of your own buttons pinned to the bottom of the page. It defaults
  to showing only when someone is signed in, so you can put internal shortcuts on a page the
  public also sees.

## Putting Item data into a section

Most section settings accept a binding instead of fixed text. Bindings use double curly
braces and a namespace:

```
{{item.name}}
{{item.serial_number}}
{{tub.name}}
```

Rather than typing them, open the **Data** tab in the left panel and drag a field straight
onto the setting you want it in. Beyond Item and Tub fields, you can bind device and time
information — see [Using Fields](/help/using-fields) for the full reference.

If a binding cannot be resolved, it renders as empty rather than showing an error, and most
sections hide themselves when their content is empty.

## Showing a section only sometimes

Every section has a **Visibility** setting that takes a condition. While you type, the editor
evaluates it against the current preview data and shows whether the section is currently
visible, hidden, or the expression is invalid.

```
item.status == "operational"
```

Conditions use the same expression language as Destination visibility. See
[Conditional Visibility](/help/conditional-visibility).

## Previewing with real data

The item selector in the editor's top bar switches what the canvas renders against:

- **Base Template** uses placeholder data
- Any real Item in the Tub uses that Item's actual field values

This is the fastest way to catch a layout that works for a short name and breaks for a long
one. Search the selector by name, Item ID or description, or step through Items with the
arrows.

Five widths are available for checking the layout: Responsive, Mobile (375px), Tablet
(768px), Desktop (1024px) and Wide (1440px). The **Preview** toggle in the same top bar hides
the editing outlines and drag handles so you see close to the finished page.

## Theming and layout

With no section selected, **Properties** shows **Page Settings**, split into two groups:

- **Page Info** — the page's name, and a read-only version number that increments as you save
- **Theme** — the settings that control how the page looks:
  - **Theme Preset** — Classic Light, Professional Dark, Warm Minimal, High Contrast, Soft
    Pastel, Vibrant Modern, Monochrome
  - **Accent Colour**, **Border Radius**, **Shadows**, **Spacing**, **Typography Scale**
  - **Appearance** — light or dark
  - **Max Width** and **Padding** for the page container

Choosing a preset replaces the whole theme rather than merging with your current settings, so
pick the preset first and adjust afterwards.

## Does saving change one Item or the whole Tub?

It depends on the **Override** toggle in the top bar, and on what you're previewing when you
save. Get this wrong and an edit meant for one machine can reshape the page for every Item in
the Tub — or the reverse, an edit meant for everyone can silently land on only one Item.

When you are previewing a real Item, the top bar shows the **Override** toggle:

- **Override: ON** — saving stores changes for **that Item only**, leaving the base template
  — and every other Item that uses it — untouched
- **Override: OFF** — saving updates the **base template** itself, changing the page for
  *every* Item in the Tub — except any individual section a given Item has already
  overridden, which keeps its own version regardless of what the base template does

**Selecting a real Item turns Override on automatically.** That is the safe default — it
means an experiment on one machine cannot silently reshape the other four hundred. To change
the page for everyone, either switch back to **Base Template**, or turn Override off
deliberately.

If an Item already has overrides and you save with the toggle off, the editor warns you
first. You can revert a single section back to the base template, or remove all of an Item's
overrides at once.

Use overrides sparingly: one machine that needs an extra button is a good reason, and
restyling forty Items individually is a sign the base template should change instead.

## Related

- [Key Concepts](/help/key-concepts)
- [Pages Overview](/help/pages-overview)
- [Using Fields](/help/using-fields)
- [Conditional Visibility](/help/conditional-visibility)
- [Custom Fields](/help/custom-fields)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
