---
title: "Custom Fields"
description: "Define the fields your Items need, set allowed values and defaults, and link Items to other records"
---

A Tub defines what data its Items hold. Beyond the built-in fields, you add your own — a
serial number, an inspection date, a responsible person — and those fields then feed your
pages, your Destination URLs and your conditions.

## Where fields are configured

Open the Tub, go to its settings, and choose the **Fields** tab. This is where you add, edit,
relabel, reorder, enable/disable and delete fields. The one exception is the tub-level default
for the Destination URL itself — see **Defaults** below — which is set from the Tub's
Destination configuration, not the Fields tab.

## Core fields and custom fields

Four fields are always present and cannot be removed or renamed: **name**, **item_id**,
**description** and **tags**. You can relabel them, mark them required, or turn them off, but
their underlying keys are fixed.

Everything else you add is a custom field. Each custom field gets a hidden stable identifier
when it is created, which means **renaming a field is safe** — the label and key change, and
existing data and bindings follow the rename.

There's no limit on how many custom fields a Tub can have, and Custom Fields is available on
every plan.

## Field keys

Every field has a key, used in bindings like `{{item.serial_number}}` and in conditions. The
rules:

- lowercase letters, numbers and underscores only
- between 2 and 64 characters
- no leading, trailing or repeated underscores
- cannot be a reserved word, and cannot collide with an existing field

A key is suggested automatically from the label you type, so in most cases you do not write
one yourself.

## Field types

Every field has one of six types, chosen from a dropdown in the Fields tab:

| Type (in the app) | Stored as | Use it for |
|------|------|------------|
| **Text** | string | Names, serial numbers, locations, URLs |
| **Number** | number | Quantities, hours, readings |
| **Yes/No** | boolean | Yes/no flags |
| **Date** | date | Dates such as an installation date |
| **List** | array | Multiple values, such as tags |
| **UUID** | object | A raw UUID value — not the same as a [reference field](#reference-fields) below, which uses a **Text** field plus a link to another record |

The field type can be changed after the field already holds data on some Items. There's no
conversion step — an existing value keeps whatever it was stored as, and only newly entered
values follow the new type. Avoid changing a populated field's type unless you're prepared to
clean up or re-enter existing values by hand.

## Allowed values

A field can carry a fixed list of allowed values, which turns it into a picker rather than a
free-text box. Each value can be given a colour.

This does more than tidy up data entry. **Status and tag colours on the page come from these
values**, so a status of `overdue` shown in red on the Item grid appears red on the scanned
page too, without configuring it twice.

Two settings control how strict the list is:

- **Allow new values** — people can add a value that is not on the list, and it is registered
  for future use
- **Multiple values** — the field accepts several values at once rather than one

**This matters for CSV import.** A field is checked against its allowed-values list on import
*only* when **Allow new values** is off. If **Allow new values** is on — the more permissive,
and more commonly used, setting — a typo in that column is not rejected: it's registered as a
brand-new allowed value instead. Turn **Allow new values** off for any field where you want a
misspelled status or category caught rather than silently added to the list.

## Defaults

A field can have a Tub-level default, applied when an Item is created with that field left
blank. An explicit value on the Item always wins; the default never overwrites what someone
typed.

The **Destination URL** — the URL or app link an Item's QR code resolves to — has its own
tub-level default, but it isn't set on the Fields tab like other fields. It's configured from
the Tub's Destination settings (the "Default destination" box shown when the Tub is in
pass-through/Direct mode — see [Key Concepts](/help/key-concepts) for Direct Mode vs. Page
Mode). That default is stamped onto an Item only once, when the Item is created; changing the
Tub default later does not retroactively change Items that already exist.

## Reference fields

A field can point at another record instead of holding a value of its own. This is how you
model relationships — an Item that belongs to a site, or has an assigned owner.

Choose what the field references:

| References | Choices offered |
|------------|-----------------|
| **A team member** | Anyone on the team |
| **Another Item** | Items in the same Tub, or Items in any Tub on the team |
| **A Tub** | Any Tub on the team |

Reference fields render as a dropdown, which can be made searchable for long lists. When a
page or grid shows a reference, it displays the referenced record's name and image rather
than a raw identifier.

## Required fields and validation

Any field can be marked required. On item creation, and on a full item edit, a required field
left blank is rejected. During CSV import specifically:

- **On a new-item import**, a row missing a required value is rejected while the rest of the
  file still imports.
- **On an update-by-CSV import**, a required field is only checked when that column is present
  in the row — a partial-update row that simply doesn't mention the column is not rejected for
  it.

Fields with allowed values are validated on import too, but only when **Allow new values** is
off for that field — see the caveat under **Allowed values** above.

## Deleting a field

Deleting a field removes it from the Tub's configuration, the item editor and the page — but
it does not delete the values already stored on existing Items. That data isn't shown or
usable anywhere once the field is gone, and re-adding a field with the same name later creates
a new field rather than reconnecting to the old data. Treat deleting a field as hiding its data
rather than erasing it, and disable a field instead of deleting it if you might want it back.

## How fields get used elsewhere

Once a field exists, it is available in three places:

- **URL Templates** — `https://example.com/asset/{{item.serial_number}}`
- **Conditions** — `item.status == "operational"`
- **Page sections** — bind a field into any section's settings

See [Using Fields](/help/using-fields) for the full reference and
[Building a Page](/help/building-a-page) for putting fields on the page.

## Related

- [Using Fields](/help/using-fields)
- [Building a Page](/help/building-a-page)
- [Key Concepts](/help/key-concepts)

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
