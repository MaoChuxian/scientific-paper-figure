# Publication Style

Derive visual tokens in this priority order:

1. explicit user requirements;
2. target journal or venue constraints;
3. supplied reference figure;
4. conventions already present in the destination Figma file;
5. restrained academic defaults.

Do not overwrite a specified or established style with the defaults below.

## Default Academic Direction

- Use a white or near-white canvas, dark neutral text, restrained strokes, and a small set of semantic accents.
- Build hierarchy through position, grouping, whitespace, type weight/size, and then color.
- Keep at most three primary hierarchy levels within one panel.
- Use consistent geometry for equal-role objects and visibly different geometry for different semantic roles.
- Prefer direct labels over a legend when repetition does not justify the legend.

Treat all dimensions as tokens derived from canvas and final output, not universal constants. Establish and reuse tokens for outer margin, gutter, node gap, padding, corner treatment, stroke classes, connector classes, and type roles.

## Final-Size Readability

- Determine the intended printed/display width when available and inspect at an equivalent scale.
- Keep the smallest essential label comfortably readable; enlarge the figure or reduce content before shrinking critical text.
- Use concise labels and deliberate line breaks. Do not rely on auto-shrink that produces inconsistent typography.
- Keep equations and units exact. Use text or supported vector math rather than screenshot text.
- Reserve panel-scale headings for panel identity; avoid oversized titles inside compact scientific panels.

## Color Semantics

- Assign colors to stable meanings such as input, proposed method, baseline, control, output, or evidence.
- Do not recolor the same entity across panels without a stated reason.
- Do not encode a scientific distinction by color alone; add shape, label, line treatment, or position.
- Check text/background contrast and common color-vision deficiencies.
- Use saturation sparingly for novelty or the primary scientific path, not decoration.

## Connectors and Lines

- Use a consistent visual class for each relationship type.
- Make the main reading path visually stronger than secondary association or control links.
- Keep axes, separators, leaders, and causal/process arrows visually distinct.
- Avoid decorative arrow styles that obscure direction or endpoint attachment.

## Multi-Panel Composition

- Align panel edges and shared baselines where possible.
- Balance visual mass, not merely panel widths.
- Use panel letters only when the manuscript/caption or multi-panel reading requires them.
- Keep panel letters editable, consistently placed, and separate from imported plots/images.
- Preserve enough gutter for captions, connector routes, and independent panel export.

## Scientific Assets

- Prefer native Figma shapes and vectors for schematic objects.
- Import quantitative plots as vector artifacts generated from real data.
- Keep each microscopy/photo/evidence field separate when it represents a distinct condition or channel.
- Rebuild titles, legends, annotations, borders, and scale-bar labels as editable objects.
- Use domain icons only when they clarify identity; prefer simple, consistent, licensable vectors over decorative stock imagery.

## Export Readiness

Before delivery, verify the figure at target size and at higher zoom. Check font availability, vector/raster boundaries, clipping, stroke consistency, panel export bounds, and requested color mode. Keep the Figma source as the primary editable artifact even when exporting SVG, PDF, or raster previews.

