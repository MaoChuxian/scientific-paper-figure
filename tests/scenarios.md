# Scientific Paper Figure Test Scenarios

These are acceptance scenarios for the Skill. They are intentionally phrased as user requests so they can be replayed after Figma reauthentication.

## A. Simple ML Pipeline

Prompt:

> Use `$scientific-paper-figure` to create an editable Figma figure for `Material Data -> Preprocessing -> Feature Engineering -> Model Training -> SHAP -> Prediction`. Keep the main reading order left to right and make training-only interpretation explicit.

Expected `figure_spec`:

- one message about transforming material measurements into an interpretable prediction;
- six stable stage IDs and one primary process topology;
- a visible distinction between data, computation, interpretation, and output;
- editable labels, stage frames, arrows, and a semantic highlight for SHAP;
- no fabricated quantitative chart.

Figma assertions:

- one top-level figure frame;
- six named stage frames or equivalent semantic groups;
- text remains `TEXT`, panels remain frames/groups, and connectors remain individually selectable;
- no connector crosses a stage or label;
- the SHAP stage is visually emphasized without changing the arrow semantics.

QA gate: exact stage labels, direction, alignment, endpoint clearance, final-size text fit, and zero hard findings.

## B. Paper Method to Figure

Prompt:

> Use `$scientific-paper-figure` to turn this Method into a one-panel overview plus a mechanism inset:
>
> "We collect paired spectra and composition measurements from each alloy batch. Spectra are baseline-corrected and resampled, then fused with composition descriptors. A gated encoder produces a latent representation. During training, an auxiliary contrastive loss aligns batches from the same processing condition. The predictor estimates tensile strength, and an attribution analysis identifies wavelength regions that drive the estimate. We validate on a held-out batch split."

Expected compression:

- entities: spectra, composition descriptors, alloy batch, processing condition;
- processes: correction/resampling, fusion, gated encoder, contrastive alignment, prediction, attribution;
- evidence: held-out batch split and attribution output;
- topology: data flow plus a training-only control/loss path, not a false inference arrow;
- overview answers input, mechanism, novelty, output, and validation; inset expands the gated/contrastive mechanism.

Figma assertions:

- training-only loss path is visually and textually marked;
- `tensile strength` and `wavelength regions` remain exact text;
- the validation split is separate from the predictor output path;
- no paragraph is pasted into a box.

## C. Reconstruction

Reference fixture:

- Use a supplied paper workflow image or a local reference such as `.research-repos/codex-paper-figure-skill/outputs/multimodal-gnn/` when available.
- Ask for editable Figma reconstruction of the scientific communication, not pixel-perfect tracing.

Expected behavior:

- inspect the reference at far and close views;
- record a region/object mapping and ambiguity list before writing;
- rebuild text, panel frames, arrows, legends, and regular plots as native objects;
- retain only irreducible image fields as atomic raster objects;
- preserve exact labels and relationship direction while allowing clearer spacing.

Figma assertions:

- every reconstructed region has stable semantic names;
- no whole-panel screenshot is used as a shortcut;
- raster objects, if any, carry a reason and have editable overlays rebuilt;
- reference comparison uses fresh rendered evidence after each region and at whole-figure completion.

## D. Local Editing After Manual Figma Changes

Setup:

- Create or open a multi-panel figure and manually change only `panel-b/mechanism` (for example, replace one label and move one node).

Prompt:

> Use `$scientific-paper-figure` to change only Panel B: replace the label `Encoder` with `Gated encoder` and route the contrastive-loss connector around the panel. Preserve all other panels and my manual edits.

Expected behavior:

- inspect current node IDs, names, bounds, and changed properties first;
- define Panel B as the change set and all other panels as preserve set;
- mutate only the label and connector plus any directly necessary local geometry;
- re-audit Panel B and the whole figure for regressions.

Figma assertions:

- nodes outside Panel B have identical text, bounds, visibility, and parent IDs;
- the manual node move remains intact;
- the new route avoids labels and panel content;
- no global style or auto-layout mutation shifts other panels.

## E. Audit and Minimal Correction

Fixture defects:

- overlap `panel-a/stage-2` and `panel-a/stage-3` by 12 px;
- route `connector/feedback` through `panel-a/stage-2`;
- clip the last line of `panel-b/annotation`;
- give repeated evidence cards inconsistent widths;
- flatten one editable legend into a raster.

Prompt:

> Use `$scientific-paper-figure` to audit this figure, report each defect with evidence and acceptance conditions, then correct only the responsible objects and re-audit.

Expected findings:

- five separate findings with stable region/object IDs and hard severity where applicable;
- root causes distinguish geometry, connector route, typography, repeated dimensions, and editability;
- correction plan preserves approved regions and does not rebuild either panel;
- a fresh render and structure inspection verify every acceptance condition.

Figma assertions:

- overlap and connector defects are fixed by local geometry/routing;
- clipped text is fixed by its text/container bounds, not rasterization;
- card widths are aligned/distributed without moving unrelated anchors;
- legend is rebuilt as editable text/shapes;
- final audit contains zero hard findings.
