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
| `Sort by` | Controls final preview order. Use `Visible size` for showcase-style output, `Image contrast` for the clearest crops first, `Distance from centre` for centre-first browsing, or `Name` for alphabetical ordering. |
| `Visibility filter` | Controls how aggressively low-contrast, nearly blank preview crops are removed. `Balanced` is the recommended default. |
| `Reset to defaults` | Restores the default settings. |
| `Export CSV` | Exports the latest result table as CSV. |

## Result Table And Preview Inspector

After a successful run, the result table lists the objects that were actually used for the final outputs.

The table includes:

- Display ID.
- Main SIMBAD ID.
- Catalogue/type.
- RA/Dec.
- Crop patch size.
- Measured visibility score.

Click a row in the result table to inspect that object in the embedded preview panel below the table. The panel stays hidden until a result grid has been built, then shows a two-part inspector: a zoomed overview context on the left and the generated crop on the right.

The rendered PixInsight image windows are static outputs, so the result table is the interactive selector for jumping between preview objects.

## Tips

- If the output has too many faint or nearly blank previews, lower `Max rows`.
- If you still get too many faint catalogue-only objects, remove `SDSS` or `Gaia` from `Catalogue filters`.
- If galaxies are cropped too tightly, increase `Box size` or `Patch scale %`.
- If previews show too much empty space, reduce `Patch scale %`.
- If the preview grid looks too low-resolution, increase `Preview patch`.
- If the final image becomes very large, reduce `Preview patch` or `Max rows`.
- If labels clutter the overview, use `Numbers` label mode.
- Use `Sort by > Visible size` when you want the most prominent galaxies first.
- Use `Sort by > Image contrast` when you want the clearest detected objects first.
- Use `Visibility filter > Strict` if too many blank-looking previews remain.
- Use `Visibility filter > Permissive` or `Off` if the script removes faint objects you still want to inspect.

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

Versioning note: Patch releases stop at `.9`. The next release after `1.0.9` is `1.1.0`, not `1.0.10`, so update repositories and file listings sort naturally.

### 1.1.4

- Removed the named navigation previews from `SkyAnnotFinal` again because they added clutter and were not the desired interaction model.
- Kept the selected-preview crash fix from `1.1.3`.
- Kept cleanup for legacy `SkyAnnotSelectedContext` previews on run, reset, close button, and dialog close.

### 1.1.3

- Fixed a selected-preview crash caused by dialog-side context crops calling an engine-only helper.
- Kept the source-image context crop behaviour from 1.1.2, but made the clamp calculation local to the inspector.

### 1.1.2

- Fixed selected-preview inspector scaling so large context crops are fitted into the panel instead of being clipped.
- Built the inspector context crop on demand from the source image window for more reliable object positioning.
- Avoided storing hundreds of large context bitmaps in memory.

### 1.1.1

- Kept the embedded preview inspector hidden until a result grid exists.
- Changed the inspector to show both a zoomed overview context and the selected preview crop.
- Hid the PixInsight Process Console after a successful run to reduce workspace obstruction.
- Clarified that the result table is the interactive selector because generated PixInsight image windows are static outputs.

### 1.1.0

- Replaced external selected-preview image windows with an embedded preview inspector in the script dialog.
- Clicking a result table row now updates the enlarged preview panel directly.
- Automatically shows the first preview after a successful run.
- Improved usability when the Process Console or modal script dialog blocks access to separate PixInsight windows.

### 1.0.9

- Added initial result-table click handling.
- Stored generated preview patches with the result data.
- Added selected-object preview support for table rows.

### 1.0.8

- Added `Sort by` output ordering modes: default, visible size, image contrast, distance from centre, and name.
- Added `Visibility filter` modes: off, permissive, balanced, and strict.
- Added patch size and visibility score columns to the result table.
- Added patch size and visibility score fields to CSV export.
- Logged the selected sort and visibility modes for easier debugging.

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
