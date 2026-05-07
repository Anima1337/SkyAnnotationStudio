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
| `Preset` | Applies or saves reusable processing settings such as `Galaxy field`, `Widefield`, `Messier target`, `Deep field`, `Messier showcase`, `NGC/IC showcase`, `Deep survey`, and `Minimal clean labels`. Type a new name before saving to create a custom preset. |
| `Label mode` | Chooses whether labels show numbers, object IDs, or both. |
| `Label length` | Chooses short, medium, or full object names below preview tiles. `Medium` keeps the classic compact labels. |
| `Object type` | Controls the SIMBAD object family. Use `Galaxies only` for the normal workflow, `Galaxies, quasars off` to suppress QSO/blazar-like entries, `Known galaxies priority` to avoid survey-only candidates, or `Stars only` for stellar fields. |
| `Catalogue priority` | Chooses which catalogue aliases win during naming and duplicate handling. Prefer Messier, prefer NGC/IC, favour known catalogues, or hide Gaia entries when a stronger duplicate is nearby. |
| `Experimental SIMBAD size crops` | Uses SIMBAD galaxy major/minor axis values, where available, to enlarge crops for physically large or elongated galaxies. |
| `Box size` | Base crop size in source pixels. Increase it to zoom out object previews. |
| `Patch scale %` | Global crop scale. Lower values zoom in, higher values zoom out. |
| `Preview patch` | Rendered tile size in the preview grid. This affects grid resolution, not the selected sky area. |
| `Max rows` | Maximum number of preview objects kept after filtering. Higher values go deeper, but can include more faint or ambiguous objects. |
| `Sort by` | Controls final preview order. Use `Visible size` for showcase-style output, `Image contrast` for the clearest crops first, `Distance from centre` for centre-first browsing, `Name` for alphabetical ordering, or `Redshift` for approximate farthest-to-nearest depth ordering when SIMBAD redshift is available. |
| `Visibility filter` | Controls how aggressively low-contrast, nearly blank preview crops are removed. `Balanced` is the recommended default. |
| `Output mode` | Quick presentation style. `Scientific` keeps dense numeric+ID annotations, `Showcase` balances presentation and discovery, `Poster` favours cleaner output with fewer objects, and `Discovery Map` keeps many small candidates for inspection. |
| `Top 5 object notes` | Adds a compact educational notes footer for the most prominent selected objects. |
| `Highlight showcase objects` | Draws stronger boxes/labels for the first five output objects. |
| `Colour-code object classes` | Uses different annotation colours for object families and adds a legend to the final image. Yellow is the main target, cyan is Messier/NGC/IC, purple is quasar/AGN-like, gold is Gaia/stellar catalogue, and blue is other galaxy/survey objects. |
| `Show advanced controls` | Reveals manual keep/remove, auto-review scrolling, CSV/HTML report export, redshift, and auto-title controls. |
| `Show log` | Shows or hides the detailed run log in the script dialog. The PixInsight Process Console still receives the same messages either way. |
| `Reset to defaults` | Restores the default settings. |

## Output Modes

The `Output mode` control applies a small bundle of presentation defaults before each run:

| Mode | Best for | Behaviour |
| --- | --- | --- |
| `Scientific` | Dense reference plates and object identification. | Uses numeric+ID labels, balanced visibility filtering, and default catalogue-style ordering unless `Redshift` sorting is selected. |
| `Showcase` | General-purpose final images. | Keeps your chosen sort/filter choices and is the safest default for attractive annotated outputs. |
| `Poster` | Cleaner presentation images. | Uses stricter visibility filtering, fewer objects, numeric labels, and highlighted showcase objects. |
| `Discovery Map` | Exploring as many candidates as possible. | Keeps more objects, uses permissive filtering, and favours image-contrast ordering unless `Redshift` sorting is selected. |

If `Sort by > Redshift` is selected, output modes no longer overwrite it. Objects with usable SIMBAD redshift are sorted approximately farthest-to-nearest; objects without redshift are kept after those.

## Colour Legend

When `Colour-code object classes` is enabled, annotations and preview-grid numbers use these colours:

| Colour | Meaning |
| --- | --- |
| Yellow | Configured main target, for example `M81`. |
| Cyan | Classic catalogue objects such as Messier, NGC, and IC entries. |
| Purple | Quasar, AGN, blazar, or similar active-galaxy candidates from SIMBAD. |
| Gold | Gaia or stellar-catalogue style entries. |
| Blue | Other accepted galaxy/survey objects, such as LEDA, SDSS, 2MASX, UGC, MCG, and similar catalogues. |

The same legend is added to the bottom of the final composite image when colour coding is active.

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

The rendered PixInsight image windows are static outputs, so the result table is the interactive selector for jumping between preview objects. With advanced controls enabled, selected rows can be marked as kept or removed, then re-rendered with `Render kept`.

Double-click the `Keep` cell in the result table to toggle a row between `Yes` and `No`. `Start auto-review` steps through the table automatically at the selected delay, which makes blank or weak crops easier to spot without manually pressing the arrow keys. The default delay is `100 ms`; during auto-review the inspector uses a lightweight crop-only preview so the dedicated `Stop` button remains responsive. Click a row manually when you want the full context-plus-crop inspector.

The run log can be hidden with `Show log` to keep the dialog compact. Each run still reports the elapsed execution time in the log and in the PixInsight Process Console.

## Tips

- If the output has too many faint or nearly blank previews, lower `Max rows`.
- If you still get too many faint catalogue-only objects, remove `SDSS` or `Gaia` from `Catalogue filters`.
- If galaxies are cropped too tightly, increase `Box size` or `Patch scale %`.
- If previews show too much empty space, reduce `Patch scale %`.
- If the preview grid looks too low-resolution, increase `Preview patch`.
- If the final image becomes very large, reduce `Preview patch` or `Max rows`.
- If labels clutter the overview, use `Numbers` label mode.
- Save your favourite processing setup into one of the preset slots when a workflow feels right.
- Type a new preset name before clicking `Save preset` to create a custom setup instead of overwriting a built-in slot.
- Enable `Experimental SIMBAD size crops` when large or elongated galaxies are being cropped too tightly.
- Use `Top 5 object notes` for educational/shareable outputs, but leave it off for cleaner pure annotation plates.
- Use advanced manual curation when the automatic filters leave a few false positives that only a human eye can judge.
- Use `Label length > Full` when preparing outputs where complete object names matter more than compact spacing.
- Use `Object type > Galaxies, quasars off` if quasar-like catalogue entries are not useful for your presentation.
- Use `Catalogue priority > Prefer NGC/IC` when you want classic NGC/IC names to win over Messier aliases where possible.
- Use `Catalogue priority > Hide Gaia if duplicate exists` if Gaia survey entries are causing repeated near-identical previews.
- Use `Sort by > Visible size` when you want the most prominent galaxies first.
- Use `Sort by > Image contrast` when you want the clearest detected objects first.
- Use `Sort by > Redshift` for a depth-style presentation; objects without SIMBAD redshift are kept after the redshift-ranked objects.
- Use `Visibility filter > Strict` if too many blank-looking previews remain.
- Use `Visibility filter > Permissive` or `Off` if the script removes faint objects you still want to inspect.
- Use `Start auto-review` in the advanced controls to step through previews hands-free while manually toggling obvious false positives. If the UI feels strained, raise the delay from `100 ms` to `300-800 ms`.
- Disable `Show log` if you want a smaller script window, or enable it when you want detailed diagnostics after a run.

## How It Works

Sky Annotation Studio reads the astrometric solution from your image, queries SIMBAD for objects in the field, filters for galaxy-like objects, removes many duplicate aliases, and projects the catalogue coordinates back onto your image.

It then creates crop boxes, removes very low-contrast preview crops, and renders the final annotated overview and preview grid.

The filtering is intentionally conservative. The script tries to remove obvious blank or duplicate previews while keeping scientifically interesting objects.

## Known Limitations

- SIMBAD can list objects that are technically in the field but too faint to be visible in your exposure.
- Very high `Max rows` values can still include faint or ambiguous objects.
- The visibility filter is based on image contrast, so it cannot perfectly know whether an object is meaningful to you.
- SIMBAD object dimensions are incomplete and heterogeneous, so experimental size-based crops are helpful but not guaranteed for every object.
- The script needs a solved image. It does not run plate solving itself.
- Internet access is required for SIMBAD queries.

## Changelog

Versioning note: Patch releases stop at `.9`. The next release after `1.0.9` is `1.1.0`, not `1.0.10`, so update repositories and file listings sort naturally.

### 1.2.2

- Added a dedicated `Stop` button for auto-review instead of relying on a relabelled Start/Stop toggle during timer activity.
- Added extra event-loop checkpoints before and after timed auto-review steps so pending Stop clicks can be processed before the timer restarts.
- Made the final composite always include an object colour legend footer explaining the class colours.

### 1.2.1

- Made auto-review stop responsiveness more reliable by adding an explicit stop-request flag and event-processing checkpoints before restarting the timer.
- Changed auto-review inspection to a lightweight crop-only preview mode instead of rebuilding the full context crop on every timed step.
- Kept manual row selection unchanged: clicking a table row still shows the full context-plus-preview inspector.

### 1.2.0

- Stabilised auto-review with a one-shot timer loop and re-entry guard to reduce PixInsight/V8 UI crashes.
- Changed the auto-review default/minimum delay to `100 ms`.
- Added a `Show log` toggle so the dialog can stay compact while still logging to the PixInsight Process Console.
- Added elapsed execution time messages after normal and curated renders.
- Added a colour legend footer to the final composite when object-class colour coding is enabled.
- Expanded tooltips and README explanations for output modes, colour classes, visibility filtering, auto-review, and log behaviour.
- Increased the remembered/default dialog size floor to avoid the compressed-button layout seen in smaller windows.

### 1.1.9

- Added built-in priority presets: `Messier showcase`, `NGC/IC showcase`, `Deep survey`, and `Minimal clean labels`.
- Improved `Redshift` sorting to use approximate farthest-to-nearest cosmological depth ordering, with missing-redshift objects sorted afterwards.
- Added double-click toggling on the result table `Keep` column.
- Kept result-table keyboard focus after keep/remove actions so arrow-key curation can continue immediately.
- Added advanced auto-review scrolling with user-configurable preview delay.
- Colour-coded preview-grid numbers now match the object class colour when class colouring is enabled.
- Reduced default dialog height and added persistent remembered dialog size between launches.

### 1.1.8

- Added a visible `Output mode` control for scientific, showcase, poster, and discovery-map workflows.
- Added optional top-five object notes in the final composite output.
- Added showcase highlighting and optional colour-coded annotation classes.
- Added an advanced controls section with manual keep/remove, curated re-rendering, HTML report export, redshift inclusion, and optional auto-title.
- Added custom preset names so users can save new presets instead of overwriting built-in presets.
- Added redshift metadata to SIMBAD parsing, CSV output, HTML reports, and optional notes.

### 1.1.7

- Tightened `Strict` visibility filtering so edge stars cannot keep otherwise empty preview crops by themselves.
- Added centre-evidence requirements for `Balanced` and `Strict` visibility modes.
- Reduced the influence of global crop contrast in visibility scoring to make false positives less common.

### 1.1.6

- Added persistent processing presets with `Galaxy field`, `Widefield`, `Messier target`, and `Deep field` slots.
- Added an experimental SIMBAD-size crop option that uses major/minor axis metadata where available.
- Improved visibility scoring so bright stars near crop edges are less likely to keep otherwise empty preview patches.
- Extended SIMBAD queries and result handling with galaxy dimension metadata for future crop tuning.

### 1.1.5

- Added `Label length` control with Short, Medium, and Full preview-grid label options.
- Added `Object type` filtering modes for galaxies-only, galaxies without quasar-like entries, known-galaxy-priority output, and stars-only queries.
- Added `Catalogue priority` control for Messier-first, NGC/IC-first, known-catalogue-first, and Gaia-duplicate suppression workflows.
- Included the selected object type and catalogue-priority mode in the run log for easier reproducibility.

### 1.1.4

- Fixed selected-preview inspection after a completed run.
- Prevented legacy helper previews from remaining open after run, reset, or close.
- Improved cleanup for temporary preview state without adding extra previews to generated output images.

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
