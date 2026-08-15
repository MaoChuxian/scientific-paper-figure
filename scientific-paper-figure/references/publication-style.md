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

- Record publisher/profile, target width and height, column mode, and final use. Determine the scale from Figma dimensions to the intended publication dimensions and evaluate effective point sizes after that scaling.
- Never approve typography from canvas appearance or zoom alone. If final dimensions are unknown, report that publication-size verification remains conditional.
- Keep the smallest essential label comfortably readable; enlarge the figure or reduce content before shrinking critical text.
- Use concise labels and deliberate line breaks. Do not rely on auto-shrink that produces inconsistent typography.
- Keep equations and units exact. Use text or supported vector math rather than screenshot text.
- Reserve panel-scale headings for panel identity; avoid oversized titles inside compact scientific panels.

## Font Profiles

- **classic-paper**: Times New Roman. Use for traditional journal figures, identifiable serif references, and Elsevier-style work when appropriate.
- **modern-paper**: Arial, with Helvetica fallback. Use for modern scientific schematics, Nature-like sans-serif work, and dense figures where sans-serif improves readability.
- **user-preferred**: preserve an explicitly selected family, including Comic Sans MS. Do not silently replace it; warn during journal-compliance QA when the venue recommends another font and let the user retain it after accepting the warning.

For reconstruction, preserve an identifiable suitable reference font. If unavailable, use Times New Roman for a serif reference and Arial/Helvetica for a sans-serif reference. Treat serif-versus-sans and comparable typography class as fidelity, not decoration. Define final-point targets for body, secondary text, section titles, panel labels, and an essential-text minimum in the working spec; derive values from the venue/profile rather than imposing one universal size.

## Color Semantics

- Lock a palette contract before styling. Register neutral text, background, every semantic role, allowed HEX values, and the maximum number of semantic accents. Every non-neutral color requires a semantic role, reference-fidelity justification, or user-requested role. Once locked, do not introduce another HEX without recording one of those reasons.
- Assign colors to stable meanings such as input, proposed method, baseline, control, output, or evidence.
- Reuse the exact registered HEX for repeated meanings. Prefer neutral body text such as `#111111`, `#222222`, `#444444`, or `#555555`; reserve colored text for a reference-faithful or deliberate semantic role with adequate contrast.
- Do not recolor the same entity across panels without a stated reason.
- Do not encode a scientific distinction by color alone; add shape, label, line treatment, or position.
- Avoid red-versus-green-only distinctions and rainbow palettes unless a continuous ordered scientific variable requires a suitable colormap.
- Compute contrast from final foreground/background colors. Target at least 4.5:1 for normal text and 3:1 for essential non-text boundaries as conservative internal QA thresholds, not claims about a specific journal.
- Use saturation sparingly for novelty or the primary scientific path, not decoration.

### Available Palette: user-bright-scientific

Registered accents: `#CC247C`, `#E95351`, `#F7A24F`, `#FBEB66`, `#4EA660`, `#79CAFB`, `#5292F7`, `#AA77E9`.

Treat this as a user-preferred option, not a requirement to use all eight colors. Ordinary workflow/mechanism figures should normally map 2-4 accents to named semantic roles and leave the rest unused. Add no interpolated or nearby purple, blue, orange, or other accent merely to create variety.

### Reference and AMFE Palettes

In reconstruction mode, preserve a meaningful reference palette as closely as practical. If it has accessibility problems, record the finding, keep the faithful version by default, and optionally propose an accessible alternative; do not silently recolor it.

Treat `https://color.amfe.space/#/` as optional palette inspiration, not an official journal standard. When selected, record the chosen HEX values, lock them, assign semantic roles, and do not keep inventing additional colors.

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
