# Brand assets

Home Assistant renders a grey **"icon not available"** placeholder for
integrations it has no brand images for. These files are this integration's
icon.

| File | Size | Purpose |
|---|---|---|
| `icon.svg` | vector | Source of truth — edit this, regenerate the PNGs |
| `icon.png` | 256×256 | What Home Assistant loads |
| `icon@2x.png` | 512×512 | High-DPI variant |

## How Home Assistant finds these

**Since Home Assistant 2026.3**, a custom integration ships its own images in a
`brand/` folder inside the integration folder — which is where this directory
sits. Home Assistant serves them through its own `brands` proxy endpoint, and
*local images take priority over the brands CDN*. Nothing needs to be added to
`manifest.json`, and no pull request against
[home-assistant/brands](https://github.com/home-assistant/brands) is required.

> [!IMPORTANT]
> This only works on **Home Assistant 2026.3 or newer**. On older versions the
> images here are ignored and the placeholder stays, because those versions
> only ever look at the brands CDN. The fallback for them is a pull request
> adding `icon.png` and `icon@2x.png` to `custom_integrations/urbackup/` in the
> brands repository — what that repo's README now calls the legacy folder.

The Marketplace reads them straight from this repository, at
`https://brands.home-assistant.io/marketplace/urbackup/icon.png`.

## Design

A storage cylinder with a check badge: the thing being backed up, and the
confirmation that it happened — which is precisely what this integration
reports. Home Assistant's own accent blue (`#41BDF5`) on white, so it sits
naturally in the Devices & Services list rather than fighting it.

The mark is drawn to survive being shrunk. Home Assistant renders it as small
as 24px in list rows, where the platter seams disappear and only the silhouette
and the badge remain readable — which is why the badge has a thick knockout
ring separating it from the cylinder rather than sitting flush against it.

No part of UrBackup's own branding is used or imitated. There is no `logo.png`
for the same reason: a wordmark would have to spell out someone else's product
name. Home Assistant falls back to the icon when no logo is present, which is
the intended behaviour here.

## Regenerating the PNGs

```sh
convert -background none icon.svg -resize 256x256 icon.png
convert -background none icon.svg -resize 512x512 icon@2x.png
```

Brand images must be PNG, trimmed of padding, and square for icons — 256×256,
with 512×512 for the `@2x` variant. A `dark_icon.png` may be added for dark
backgrounds; this icon is legible on both, so there is none.

> [!NOTE]
> ImageMagick's built-in SVG renderer defaults `stroke` to black instead of
> `none`, so every shape in `icon.svg` sets `stroke` explicitly. Dropping those
> attributes puts a black outline around everything. `rsvg-convert` or Inkscape
> do not have this problem if either is available.
