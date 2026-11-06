# Brand assets

Home Assistant renders a grey **"icon not available"** placeholder for custom
integrations that have no entry in the [home-assistant/brands](https://github.com/home-assistant/brands)
repository. These files are this integration's icon.

| File | Size | Purpose |
|---|---|---|
| `icon.svg` | vector | Source of truth — edit this, regenerate the rest |
| `icon.png` | 256×256 | What `brands` requires |
| `icon@2x.png` | 512×512 | High-DPI variant `brands` also expects |

## Design

A storage cylinder with a check badge: the thing being backed up, and the
confirmation that it happened — which is precisely what this integration
reports. Home Assistant's own accent blue (`#41BDF5`) on white, so it sits
naturally in the Devices & Services list rather than fighting it.

The mark is drawn to survive being shrunk. Home Assistant renders it as small
as 24px in list rows, where the platter seams disappear and only the silhouette
and the badge remain readable — which is why the badge has a thick knockout
ring separating it from the cylinder rather than sitting flush against it.

No part of UrBackup's own branding is used or imitated.

## Getting Home Assistant to use it

The icon has to live in the brands repository, not here — this directory is
only the source. Open a pull request against
[home-assistant/brands](https://github.com/home-assistant/brands) adding:

```
custom_integrations/urbackup/icon.png      (256×256)
custom_integrations/urbackup/icon@2x.png   (512×512)
```

`custom_integrations/` is the correct path — it is the part of the brands repo
that exists for integrations outside Home Assistant core, so no core PR or
integration review is involved. Once it merges, Home Assistant and HACS pick
the icon up for everyone running this integration; there is nothing to change
in this repository and no release to cut.

## Regenerating the PNGs

```sh
convert -background none icon.svg -resize 256x256 icon.png
convert -background none icon.svg -resize 512x512 icon@2x.png
```

> [!NOTE]
> ImageMagick's built-in SVG renderer defaults `stroke` to black instead of
> `none`, so every shape in `icon.svg` sets `stroke` explicitly. Dropping those
> attributes puts a black outline around everything. `rsvg-convert` or Inkscape
> do not have this problem if either is available.
