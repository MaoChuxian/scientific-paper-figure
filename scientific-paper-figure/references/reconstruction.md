# Reconstruction

Reconstruct structure and scientific communication, not pixels. Preserve exact scientific content while improving spacing, hierarchy, and consistency when the user has not requested literal visual fidelity.

## Inspect Before Building

1. Inspect the highest-resolution reference available, including local crops for small text and connectors.
2. Record aspect ratio, reading order, panel bounds, labels, equations, units, legend, typography hierarchy, palette roles, connector types, and z-order.
3. Inventory every visible item as native text, shape/vector, connector/line, quantitative plot, repeated motif, or irreducible raster evidence.
4. Mark unreadable text, hidden endpoints, uncertain arrow direction, and ambiguous grouping. Do not invent missing scientific content.
5. Inspect the destination Figma file and establish a preserve set before writing.

## Choose Fidelity Deliberately

Preserve:

- scientific message, entities, stage order, comparisons, equations, labels, units, directions, and evidence;
- panel identity and reading logic when scientifically meaningful;
- style features the user explicitly requests or that encode meaning.

Allow improvement unless prohibited:

- spacing, alignment, text boxes, routing, hierarchy, consistency, accessibility, and grouping;
- replacement of decorative raster labels/arrows/frames with native objects;
- simplification of incidental decoration that does not carry scientific meaning.

Do not trace a published figure pixel for pixel when a clearer original arrangement communicates the same science. Retain attribution and licensing notes for third-party assets when applicable.

## Rebuild in Regions

Define a `reconstruction_spec` by extending the standard `figure_spec` with:

- reference dimensions and coordinate mapping;
- region bounds and construction order;
- source-to-Figma object mapping;
- exact text/transcription status;
- connector source, target, direction, route, and confidence;
- raster decomposition decisions;
- preserve list and local acceptance condition.

Build backgrounds and panel frames first, then scientific objects, labels, connectors, legends, and annotations. Validate each region against both its reference crop and the complete figure.

## Enforce Atomic Raster Evidence

Retain one raster object only for the smallest scientifically irreducible field. Split composite microscopy arrays, before/after pairs, prediction grids, channel stacks, and photographic comparisons into independent evidence objects when separate manipulation matters.

For every raster object, record:

```text
object: stable id
reason: specific scientific reason native reconstruction is inappropriate
atomic: true
crop: tight source bounds or crop values
contains_reconstructable_overlay: false
overlays_rebuilt: labels, frames, arrows, legends, scale-bar labels, or annotations
```

Do not retain a whole panel image because decomposition is inconvenient. Never rasterize regular plots when a data-driven vector source is available.

## Reconstruct Connectors Semantically

Infer a connector only from visible endpoints, labels, scientific context, or explicit source text. If direction or relation type is uncertain, mark it as ambiguity. Do not choose arrow semantics merely because a line looks similar.

Route connectors after principal nodes are stable. Recheck endpoints after alignment or resizing changes.

## Compare at Two Scales

- Use a far view to check panel balance, reading path, hierarchy, and global correspondence.
- Use close views for exact text, equations, arrowheads, clipping, small annotations, and raster boundaries.
- Compare the current render, not a remembered or stale screenshot.

Prefer semantic correctness over superficial similarity whenever the two conflict, and report the conflict.

