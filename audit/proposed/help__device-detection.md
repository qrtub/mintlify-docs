---
title: "Device Detection & Routing"
description: "Route users to different Destinations based on their device type, operating system, or browser"
---

QRtub detects a visitor's device type, operating system, and browser from their scan and makes
them available as `device.*` fields in CEL conditions. There are two ways to use this: add
**Conditional Rules** to a single Item's Destination so one QR code silently redirects
differently per device (no Page needed), or give a Page multiple Destinations and control which
one is **Visible** per device. This is available on every QRtub plan — it isn't gated by tier.

## What device information can I use in conditions?

Three fields are available: `device.type` (`'mobile'`, `'tablet'`, `'desktop'`), `device.os`
(`'ios'`, `'android'`, `'windows'`, `'macos'`, `'linux'`, `'unknown'`), and `device.browser`
(`'chrome'`, `'safari'`, `'firefox'`, `'edge'`, `'opera'`, `'unknown'`), plus five convenience
booleans built from them.

| Field | Convenience flags |
|---|---|
| `device.type` | `device.isMobile`, `device.isTablet`, `device.isDesktop` |
| `device.os` | `device.isIOS`, `device.isAndroid` |
| `device.browser` | *(no convenience flags — compare the string directly)* |

```
device.isMobile
device.os == "ios"
device.browser == "safari"
device.isTablet && device.os == "android"
```

This is the condensed version for the examples on this page. See
[Using Fields](/help/using-fields#device-fields) for the complete field reference with every
type and example value.

## Two ways to route by device

Pick based on whether you want visitors to see one thing or a choice of things:

- **Conditional Rules on a single Destination** — one QR code, one silent redirect that differs
  by device. No Page required. Set up per Item, on that Item's own **Destination** tab. Best
  fit for "phone gets the app, desktop gets the web app" — the visitor never sees a second
  option, they just land in the right place.
- **Multiple Destinations on a Page, each with its own Visibility condition** — one QR code
  opens a Page, and the Destination whose condition matches is the one shown. Requires a Page
  (switched on for the whole Tub), and the same Page design applies to every Item in that Tub at
  once. Best fit when you want several *visible* options and device is just one of the filters
  narrowing which ones show — see [Conditional Visibility](/help/conditional-visibility) and
  [Pages Overview](/help/pages-overview).

Most of the scenarios below — app vs. web, app-store routing, the iOS Safari workaround — are
naturally single-redirect problems, so Conditional Rules is usually the simpler build. Reach for
Page Visibility when you specifically want the visitor to see and choose between options.

## How do I set up Conditional Rules on a single Destination?

Open the Item, go to its **Destination** tab, and make sure the **Destination Link** option is
selected (not Landing Page). Below the **Default URL** field, open **Conditional Rules**:

1. Check **Enable conditional routing**.
2. Click **Add Rule**. Each rule is a **When** condition and a **Then** redirect URL.
3. Add more rules as needed. Rules are evaluated top to bottom — the editor labels this
   **First match wins**, and shows an **Else** connector between rules to make the fallthrough
   explicit.
4. Whatever you put in **Default URL** (the field above Conditional Rules) is used only if no
   rule matches.

Each rule's URL can carry its own app-link Fallback URL and Fallback Message (the same fallback
mechanism described in [App Links & Fallback URLs](/help/app-links)), and an optional Label for
your own notes — neither is required.

**Example — mobile app vs. web app, one QR code, no visible choice:**

| Rule | When | Then redirect to |
|---|---|---|
| 1 | `device.isMobile` | `myapp://field-inspection` |
| — | *(Default URL)* | `https://app.example.com/portal` |

A phone opens the app directly; anyone else — desktop, tablet, an undetected device — lands on
the web portal via the Default URL. No second button, no Page.

Because this is set per Item, it's the right fit for a handful of Items or Items with one-off
routing needs. For a Tub-wide device policy applied identically to every Item at once, use Page
Visibility instead — Conditional Rules has no bulk-apply or Tub-level default.

## How do I set up device-specific Destinations on a Page?

This requires a Page. If the Tub doesn't have one yet: open the Tub's settings and turn on
**Show a profile page** — see [Building a Page](/help/building-a-page#before-you-start). Then:

1. In the Page Editor, add one **ActionLink** section per device scenario (each points to a
   Destination).
2. Select each ActionLink and set its **Visibility** condition in the Properties panel — while
   you type, the editor shows whether the section is currently visible, hidden, or the
   expression is invalid.
3. Test on each device type you're targeting (see below).

**Example — mobile app vs. web app, two visible buttons:**

- ActionLink "Open Mobile App" — Visibility: `device.isMobile`
- ActionLink "Open Web Portal" — Visibility: `device.isDesktop`

Because Visibility conditions are normally written to be mutually exclusive (as above), a given
visitor only ever sees one of the two buttons — the second one stays hidden. The Page design is
Tub-wide: build it once and every Item in that Tub renders it with its own data.

## Example: iOS Safari deep link workaround (Mitti)

Mitti (formerly SafetyCulture) app deep links (`iauditor://`) only work in Safari on iOS; iOS
Chrome or Firefox block them, so those visitors need the web version instead. If your only
concern is "what if the app isn't installed at all," you don't need device routing — the
[Fallback URL feature](/help/app-links) already handles that automatically. This workaround is
specifically for the case where the app **is** installed but the browser blocks the deep link.

Built as Conditional Rules on a single Destination:

| Rule | When | Then redirect to |
|---|---|---|
| 1 | `device.isIOS && device.browser != "safari"` | `https://app.mitti.com/inspection/new?templateId=template_fcbc86fd41a74180921347e4be53bdf2` |
| — | *(Default URL)* | `iauditor://template/new_audit/template_fcbc86fd41a74180921347e4be53bdf2`, with Fallback URL `https://app.mitti.com/inspection/new?templateId=template_fcbc86fd41a74180921347e4be53bdf2` |

Reasoning: only iOS-non-Safari visitors get diverted to the web URL by the rule; everyone else
(desktop, Android, iOS Safari) falls through to the Default URL, which is the app deep link — and
because a deep link's own Fallback URL is set on it, anyone in that group without the app
installed still lands on the same web page automatically. One QR code, one redirect, no visible
choice.

The same outcome can be built with two visible Page Destinations instead — "Start Inspection
(Mobile App)" with Visibility `device.isDesktop || !device.isIOS || (device.isIOS &&
device.browser == "safari")`, and "Start Inspection (Web)" with Visibility `device.isIOS &&
device.browser != "safari"` — if you'd rather the visitor see (and be able to re-tap) an explicit
button than be redirected silently.

## Example: platform-specific app store downloads

Route iPhone users to the App Store and Android users to Google Play from one QR code:

| Rule | When | Then redirect to |
|---|---|---|
| 1 | `device.isIOS` | `https://apps.apple.com/app/yourapp` |
| 2 | `device.isAndroid` | `https://play.google.com/store/apps/details?id=com.yourapp` |
| — | *(Default URL)* | a page explaining the app, or your website |

## Example: tablet and browser-specific routing

**Tablet gets a different layout:** Visibility (or a rule's When) of `device.isTablet` for the
tablet-optimised option, `!device.isTablet` for the standard one.

**A feature only works in some browsers:** `device.browser == "chrome" || device.browser ==
"firefox"` for the advanced version, the negation for the standard one.

## Limits and edge cases

**A typo in a condition fails silently, not with an error.** CEL evaluates an unrecognised field
name as `false` rather than raising an error — a condition like `device.isMoblie` (misspelled)
simply never matches, and nothing in the visitor-facing experience or the editor flags it as
broken. If a rule or a Visibility condition never seems to fire, check the field name for typos
first.

**An iPad can be classified as `desktop`, not `tablet`.** Detection depends on the device's
User-Agent string containing a recognisable token. Safari's "Request Desktop Website" behaviour
on iPadOS can send a Mac-style User-Agent with no iPad-identifying token in it, which QRtub then
reads as `device.type == "desktop"` and `device.os == "macos"` — not `tablet`/`ios`. This is
different from total detection failure (below): the device is detected, just bucketed as
desktop.

**Bots, crawlers, and link-preview unfurlers land in the same bucket as undetectable devices.**
When Slack, iMessage, or a search engine fetches your link server-side to build a preview, its
User-Agent won't match any recognised pattern either — it resolves to the same `desktop` /
`unknown` / `unknown` result as a genuinely undetectable visitor. If a shared preview looks like
the desktop version even though you tested fine from your phone, this is usually why.

**If device can't be detected at all, QRtub assumes:** `device.type == "desktop"`,
`device.os == "unknown"`, `device.browser == "unknown"`. Always give at least one Destination (a
Default URL with no rule, or an ActionLink with no Visibility condition) that doesn't depend on
device fields, so an undetected visitor still lands somewhere useful.

**Device detection is not a security control.** It's built from the User-Agent string, which a
visitor can change, and some privacy-focused browsers mask it deliberately. Use it for routing
and experience, never to restrict access to sensitive data — use proper authentication for that.

**Detection runs fresh on every scan.** It isn't cached per QR code or per visitor — the same
code will send a phone one way and a desktop another on the very next scan, and switching
devices changes the outcome immediately.

**Not gated by plan.** Device fields, Conditional Rules, and Page Visibility are available on
every QRtub plan.

## How do I test device routing before deploying?

Scan the same QR code from each device you're targeting and confirm you land in the expected
place:

1. **iPhone** — verify iOS-specific rules or Destinations fire.
2. **Android** — verify Android-specific rules or Destinations fire.
3. **Desktop** — verify the desktop/fallback path fires.
4. **Each browser your conditions reference** — Safari, Chrome, Firefox, etc., if you're
   filtering on `device.browser`.
5. **Tablet**, if you have tablet-specific rules — see the iPad caveat above if a tablet doesn't
   match as expected.

## When should I use device detection, and when should I avoid it?

**Avoid it for:** restricting access to sensitive information (use proper authentication
instead), forcing everyone into one specific app when letting them choose is simpler, or any
case where just showing all the options would be clearer than filtering.

**Use it for:** app-store routing (right store per platform), mobile app vs. web app redirects,
platform-specific deep-link workarounds, and layout differences by device type.

## Related

- [App Links & Fallback URLs](/help/app-links) — automatic fallback when a mobile app isn't
  installed
- [Using Fields](/help/using-fields) — complete field reference, including every device field
- [Conditional Visibility](/help/conditional-visibility) — the CEL condition language and its
  Page-Visibility use cases in full
- [Building a Page](/help/building-a-page) — turning on a Page and setting a section's
  Visibility
- [Pages Overview](/help/pages-overview) — Page Mode vs. Direct Mode
- [Key Concepts](/help/key-concepts) — Destinations and Links

---

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
