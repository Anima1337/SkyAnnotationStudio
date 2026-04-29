# Sky Annotation Studio

Sky Annotation Studio is a PixInsight script that helps you find and present galaxies and other deep-sky objects in your solved astrophotography images.

It queries SIMBAD for objects in your image, draws an annotated overview, creates close-up preview crops, and combines everything into a final presentation image directly inside PixInsight.

## Installation

Add this repository URL in PixInsight:

```text
https://raw.githubusercontent.com/Anima1337/SkyAnnotationStudio/main/
```

In PixInsight, go to:

```text
Resources > Updates > Manage Repositories
```

Add the URL above, then run:

```text
Resources > Updates > Check for Updates
```

After installation, you can find the script in PixInsight's script menu as `Sky Annotation Studio`.

## Requirements

You need:

- PixInsight.
- A solved FITS, FIT, or XISF image with valid astrometric/WCS metadata.
- Internet access for SIMBAD queries.

The script does not plate-solve images by itself. If your image is not solved yet, solve it first in PixInsight.

## What The Script Creates

After a successful run, Sky Annotation Studio creates three PixInsight image windows:

- `SkyAnnotOverview`: your image with object boxes and labels.
- `SkyAnnotGrid`: a grid of cropped object previews.
- `SkyAnnotFinal`: the overview and preview grid combined into one final image.

You can also export the detected object list as CSV.

## Basic Usage

1. Open a solved image in PixInsight.
2. Start `Sky Annotation Studio`.
3. Leave `Source` set to `Active image`, or choose a FITS/XISF file manually.
4. Enter the main object, for example `M81`.
5. Adjust the settings if needed.
6. Click `Query and Annotate`.
7. Inspect the generated overview, grid, and final composite.

For most images, the defaults are a good starting point.

## Parameters

| Parameter | What it does |
| --- | --- |
| `Source` | Chooses whether the script uses the active PixInsight image or opens a file from disk. |
| `FITS/XISF file` | File path used when `Source` is set to `File on disk`. |
| `Main object` | Your primary target, for example `M81`. This object is sorted first and receives the largest crop. |
| `Title` | Text shown on the annotated overview image. |
| `Catalogue filters` | Controls which catalogue/type prefixes are kept. Remove `SDSS` or `Gaia` if you want fewer very faint candidates. |
| `Label mode` | Chooses whether labels show numbers, object IDs, or both. |
| `Box size` | Base crop size in source pixels. Increase it to zoom out object previews. |
| `Patch scale %` | Global crop scale. Lower values zoom in, higher values zoom out. |
| `Preview patch` | Rendered tile size in the preview grid. This affects grid resolution, not the selected sky area. |
| `Max rows` | Maximum number of preview objects kept after filtering. Higher values go deeper, but can include more faint or ambiguous objects. |
| `Reset to defaults` | Restores the default settings. |
| `Export CSV` | Exports the latest result table as CSV. |

## Tips

- If the output has too many faint or nearly blank previews, lower `Max rows`.
- If you still get too many faint catalogue-only objects, remove `SDSS` or `Gaia` from `Catalogue filters`.
- If galaxies are cropped too tightly, increase `Box size` or `Patch scale %`.
- If previews show too much empty space, reduce `Patch scale %`.
- If the preview grid looks too low-resolution, increase `Preview patch`.
- If the final image becomes very large, reduce `Preview patch` or `Max rows`.
- If labels clutter the overview, use `Numbers` label mode.

## How It Works

Sky Annotation Studio reads the astrometric solution from your image, queries SIMBAD for objects in the field, filters for galaxy-like objects, removes many duplicate aliases, and projects the catalogue coordinates back onto your image.

It then creates crop boxes, removes very low-contrast preview crops, and renders the final annotated overview and preview grid.

The filtering is intentionally conservative. The script tries to remove obvious blank or duplicate previews while keeping scientifically interesting objects.

## Known Limitations

- SIMBAD can list objects that are technically in the field but too faint to be visible in your exposure.
- Very high `Max rows` values can still include faint or ambiguous objects.
- The visibility filter is based on image contrast, so it cannot perfectly know whether an object is meaningful to you.
- The script needs a solved image. It does not run plate solving itself.
- Internet access is required for SIMBAD queries.

## Changelog

### 1.0.7

- Added stronger duplicate suppression for high `Max rows` values.
- Reduced duplicate low-priority crops from catalogues such as SDSS, Gaia, 2MASX, and LEDA.
- Added tooltips for all relevant controls.
- Clarified parameter behaviour in the interface.

### 1.0.6

- Made `Box size` actually affect generated crop sizes.
- Kept catalogue-specific crop behaviour while making it scale from `Box size`.
- Kept `Patch scale %` as the global crop zoom control.

### 1.0.5

- Added a visibility/contrast filter for preview crops.
- Removed many low-contrast, nearly blank low-priority previews.
- Made overview, grid, and final composite use the same filtered object list.

### 1.0.4

- Improved catalogue priority to better match the original Python version.
- Made the configured main object sort first.
- Suppressed broad group aliases such as `M81 Group` and `M82 Group` as preview objects.
- Prevented some subcomponent aliases from appearing before canonical Messier objects.

### 1.0.3

- Improved crop sizing for elongated galaxies.
- Reduced cases where long edge-on galaxies were cut off.
- Improved final grid spacing around labels and boxes.

### 1.0.2

- Improved object coverage across the image.
- Broadened SIMBAD query and filtering behaviour.
- Improved handling of solved active PixInsight images.

### 1.0.1

- Improved compatibility with future PixInsight JavaScript changes.
- Removed older JavaScript patterns that were less future-safe.
- Improved direct active-image workflows.

### 1.0.0

- Initial PixInsight release.
- Added SIMBAD querying from solved image metadata.
- Added annotated overview output.
- Added preview grid output.
- Added final composite output.
- Added CSV export.
