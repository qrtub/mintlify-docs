---
title: "App Links & Fallback URLs"
description: "Use deep links to open mobile apps directly, with automatic fallback when the app isn't installed"
---

QRtub [Destinations](/help/key-concepts#destinations) support app deep links — URLs with a custom scheme (like `iauditor://` or `spotify://`) instead of `https://`, which open a mobile app directly to a specific screen instead of a web page. When someone scans and the app isn't installed, QRtub automatically falls back to a URL or message you specify, so the scan ends somewhere useful instead of a dead screen.

This page assumes you already have a [Destination](/help/key-concepts#destinations) — on an [Item](/help/key-concepts#item) (Direct Mode) or on a [Page](/help/pages-overview) — whose URL you want to set to an app link. If you haven't created one yet, start with [Creating Your First Link](/help/creating-your-first-link).

## What are app links?

App links are URLs that use a custom scheme instead of `https://`. When a device recognises the scheme, it opens the corresponding app rather than a browser tab.

**Examples:**
- `iauditor://template/new_audit/<template_id>` — opens Mitti (formerly SafetyCulture) to a specific template
- `myapp://asset/EXC-203` — opens a custom app to an asset record
- `spotify://track/3n3Ppam7vgaVa1iaRUIOKE` — opens Spotify to a track
- `tel:+61-2-5550-1234` — this also counts as an app link (any non-`https://` scheme does) and gets the same fallback handling described below

These work well when the app is installed. The problem is what happens when it isn't.

## What happens when the app isn't installed?

Nothing opens, or the user sees a bare OS error — a scan that ends on a blank or broken screen, with no way forward. This is common during app rollouts (not everyone has installed it yet), and whenever someone outside your own team scans — a contractor, visitor, or customer who was never going to have your internal app installed in the first place.

QRtub's fix is the Fallback URL and Fallback Message described below: configure one, and a scan without the app lands somewhere useful instead of nowhere.

## What happens, step by step, when someone taps an app link?

QRtub detects that a Destination's resolved URL is an app link (any scheme other than `http://`/`https://`) and runs a timed handoff to the native app, falling back automatically if it doesn't open:

1. QRtub attempts to open the app link.
2. A 2.5-second timer starts.
3. **If the app opens** — the browser tab is backgrounded, QRtub detects this and cancels the timer. Nothing else happens; the user is in their app.
4. **If nothing has taken over after 2.5 seconds** — QRtub treats the app as not installed and shows the fallback.

Mobile browsers suspend JavaScript timers while another app is in the foreground, so if the user returns to the tab well after 2.5 seconds have actually elapsed, QRtub treats that as a sign the app *did* open (the timer was suspended, not just slow) and skips the fallback — it doesn't wrongly bounce a returning user.

**What "showing the fallback" looks like depends on how the Destination is used** — this is the part most setups get wrong, because the same Fallback Message setting looks completely different in each case:

- **A single-Destination Item (Direct Mode)** — the scan redirects to a full-page "Opening app..." screen. On timeout it shows either your Fallback Message or a default "App not available" notice, styled as a card, with **Try Again** and **Go Back** buttons.
- **A Destination shown as a button/link on a Page** (alongside other Destinations) — tapping it intercepts the click in place. On timeout, a Fallback URL navigates the browser there; a Fallback Message pops up as a **plain native browser `alert()`** — no styling, no Try Again button, just the OS's own dialog with an OK button.

If you're writing Fallback Message copy for a Page with multiple Destinations, keep it short and plain — it will render as a bare alert, not a branded page.

## How do I set a Fallback URL?

When you add or edit a Destination and its URL is an app link, QRtub automatically shows a fallback configuration panel in the editor.

**In the Destination editor:**

1. Enter your app link URL (e.g. `iauditor://template/new_audit/template_abc123`).
2. An amber notice appears: "This is an app link — set a fallback for users without the app installed."
3. Enter a **Fallback URL** — the web page to open if the app isn't available.

**Example (Mitti):**
- App link: `iauditor://template/new_audit/template_fcbc86fd41a74180921347e4be53bdf2`
- Fallback URL: `https://app.mitti.com/inspection/new?templateId=template_fcbc86fd41a74180921347e4be53bdf2`

Users with Mitti installed go straight to the app. Users without it land on the web version — same form, different delivery.

## How do I set a custom Fallback Message?

If there's no web equivalent to fall back to, set a **Fallback Message** instead of (or as well as) a URL. QRtub uses the URL if one is set; the message is shown only when there's no URL to send the user to.

**Example:**
- App link: `myapp://asset/{{item.assetID}}`
- Fallback Message: `"Please install the Asset Manager app from the App Store to access this equipment record."`

Remember the display differs by context (see the step-by-step section above): on a single-Destination Item this appears as a styled card with a retry button; on a Page Destination it appears as a plain browser `alert()`.

If neither Fallback URL nor Fallback Message is set, QRtub shows a default "App not available" notice.

## Can Fallback URLs use field bindings like `{{item.field}}`?

Yes. Fallback URLs support `{{item.field}}` bindings the same way Destination URLs do, so one Fallback URL configuration adapts per item instead of needing to be set up per item.

**Example:**

App link:
```
iauditor://template/new_audit/template_abc123?8f2f287e={{item.assetID}}
```

Fallback URL:
```
https://app.mitti.com/inspection/new?templateId=template_abc123&asset={{item.assetID}}
```

Configure both once. Deploy to 500 items. Each scan routes to the right app screen (or web equivalent) for that specific item — **as long as the bound field has a value for that item.**

**If the bound field is empty or missing for a given item, the whole URL is treated as unresolved** — not filled in with a blank. That means:
- A primary Destination URL with an unresolved binding is skipped, and QRtub moves on to the next rule (or shows "link not ready" if nothing else matches).
- A Fallback URL with an unresolved binding is dropped entirely — QRtub falls through to the next fallback level (see below) or, if none is set there either, the default "App not available" message.

So an item missing the bound field doesn't get a broken URL with an empty segment — it silently gets no fallback at all. If you're deploying a bound Fallback URL across many items, make sure the field is populated on every one of them, or set a plain (non-bound) Fallback Message as a safety net at the Link or Item level.

## Where can I set Fallback URL and Fallback Message?

Fallback URL and Fallback Message can be set at three levels:

| Level | Where to set it | When it applies |
|-------|----------------|-----------------|
| **Destination** | In the Destination editor, per-rule | Most specific — overrides everything |
| **Link** | In the Link settings | Applies to all Destinations on this Link |
| **Item** | In the Item editor | Applies across all Links for this Item |

The most specific setting wins, **and it wins as a complete pair, not field-by-field.** If a Link-level fallback has only a Fallback Message and no URL, that Link-level message replaces the Item-level fallback entirely — it does not combine with an Item-level Fallback URL. Set both a URL and a message together at whichever level you're configuring if you want both to apply.

## Common examples

### Mitti (iAuditor)

**App link:** `iauditor://template/new_audit/{{item.templateID}}`
**Fallback URL:** `https://app.mitti.com/inspection/new?templateId={{item.templateID}}`

See the [Mitti Integration guide](/integrations/safetyculture) for full setup instructions.

### Generic enterprise app

**App link:** `mycompanyapp://workorder/create?asset={{item.assetID}}`
**Fallback URL:** `https://app.mycompany.com/workorders/new?asset={{item.assetID}}`
**Fallback Message:** `"Open the MyCompany app to create a work order."`

Set both Fallback URL and Fallback Message — QRtub uses the URL if set, otherwise shows the message.

### App download link (no web version)

**App link:** `myapp://asset/{{item.assetID}}`
**Fallback URL:** `https://apps.apple.com/app/myapp` *(or Google Play)*
**Fallback Message:** `"This feature requires the MyApp mobile app."`

A single Fallback URL can only point one place, so if iOS and Android need different app-store links, combine this with [Device Detection](/help/device-detection) to route iOS users to the App Store and Android users to Google Play.

## When should I use App Links instead of Device Detection?

They're complementary, not alternatives — they solve different problems:

| Problem | Solution |
|---------|----------|
| App not installed | A Fallback URL or Fallback Message on the Destination (this page) |
| Wrong browser on iOS blocks the app link | [Device Detection routing](/help/device-detection) |
| Different platforms need different app stores | [Device Detection routing](/help/device-detection) |

For Mitti specifically: iOS Safari works with `iauditor://` deep links, but Chrome on iOS blocks them. If your team uses mixed browsers, combine both — device routing to serve the web link on iOS Chrome, and a Fallback URL to catch anyone without the app installed on other devices.

## Limits and things to check before deploying

- **No plan restriction found** — app links and their fallback settings are part of the base Destination feature, not gated to a specific plan.
- **QRtub doesn't cap URL length.** There's no enforced maximum, but very long resolved URLs are a common source of app links that misbehave on some mobile browsers/OSes — keep bound URLs as short as the target app allows.
- **Bindings insert values exactly as stored, with no URL-encoding.** A field value containing a space, `&`, or `#` will break the deep link. Store the value pre-encoded in the Item field if it needs to appear inside a query string.
- **An empty bound field drops the whole URL, it does not insert a blank.** See "Can Fallback URLs use field bindings?" above — this is the most common cause of a fallback silently not appearing.

## Related

- [Mitti Integration](/integrations/safetyculture) — full deep link and fallback setup for iAuditor
- [Device Detection](/help/device-detection) — route users based on device, OS, and browser
- [Conditional Visibility](/help/conditional-visibility) — show Destinations to specific audiences
- [Pages Overview](/help/pages-overview) — setting up multi-Destination Pages
- [Key Concepts](/help/key-concepts) — what a Destination, Link, and Item are

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
