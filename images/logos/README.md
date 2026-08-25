Platform logos for the "Works with" card gallery.

Store them here as files rather than hotlinking. Two reasons: a docs page should not depend on a
third-party image service at read time, and the logo.dev token is a deployment env var that is not
available when building the docs.

Fetch with the pattern the app uses (`DestinationUrlCell.tsx:69`):

    https://img.logo.dev/<domain>?token=<NEXT_PUBLIC_LOGO_DEV_API_KEY>

Then reference from a Card with `img="/images/logos/<name>.png"` instead of `icon=`.

ATTRIBUTION IS REQUIRED. The app carries "Logos provided by Logo.dev" in its footer
(`Footer.tsx:21`). If logo.dev images appear on the docs site, the same credit has to appear here —
`docs.json`'s `footer` currently has no links or text at all, so it needs adding before the first
logo ships.

## What is here, and where it came from (25 Aug 2026)

Fetched from each vendor's own CDN rather than from logo.dev, so these are the official product
marks straight from the source.

| File | Source | Size |
|---|---|---|
| `google-forms.png` | `gstatic.com/images/branding/product/2x/forms_96dp.png` | 192×192 |
| `google-sheets.png` | `gstatic.com/images/branding/product/2x/sheets_96dp.png` | 192×192 |
| `google-maps.png` | `gstatic.com/images/branding/product/2x/maps_96dp.png` | 192×192 |
| `youtube.png` | `gstatic.com/images/branding/product/2x/youtube_96dp.png` | 192×192 |
| `mitti.png` | `mitti.com/apple-touch-icon-mitti.png` | 180×180 |

`gstatic.com/images/branding/product/` is Google's own branding path. `_48dp`, `_64dp` and `_96dp`
all exist under `/2x/` and return 96, 128 and 192 pixels respectively; there is no `/4x/`.

**Because these came from the vendors, logo.dev is not involved and its attribution requirement
does not apply.** If a future logo does come from logo.dev, the "Logos provided by Logo.dev" credit
the app carries in `Footer.tsx:21` has to be added to `docs.json`'s `footer` first.

**Usage basis:** these identify the product a recipe is for — nominative use. Do not restyle,
recolour or combine them with the QRtub mark, and do not imply partnership or endorsement.

## Second batch (25 Aug 2026)

| File | Source |
|---|---|
| `notion.png` | `notion.com/front-static/logo-ios.png` — 512×512 |
| `jotform.png` | `cdn.jotfor.ms/assets/img/favicons/apple-touch-icon-180x180.png` — 180×180 |
| `maintainx.png` | their own Webflow CDN, linked from `getmaintainx.com` — 256×256 |
| `servicem8.png` | their own Webflow CDN, linked from `servicem8.com` — 256×256 |
| `whatsapp.svg` | `static.whatsapp.net` — vector, and better than a raster for a card |
| `microsoft.png` | `res.cdn.office.net/.../brand-icons/product/png/office_96x1.png` — 96×96 |

Microsoft's official product-icon CDN follows
`res.cdn.office.net/files/fabric-cdn-prod_20230815.001/assets/brand-icons/product/png/<product>_<size>x1.png`
— `sharepoint`, `onedrive` and `office` all exist at 48 and 96. The Microsoft 365 page uses
`office` because it covers SharePoint and OneDrive together.

Every file here came from the vendor. logo.dev is still not involved.
