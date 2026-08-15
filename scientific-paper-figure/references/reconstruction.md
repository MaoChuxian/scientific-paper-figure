# Reconstruction

Reconstruct scientific communication and its meaningful visual grammar, not pixels. Unless the user requests redesign, preserve reference structure and motifs while limiting improvements to defects that do not change meaning or visual grammar.

## Inspect Before Building

1. Inspect the highest-resolution reference available, including local crops for small text and connectors.
2. Record aspect ratio, reading order, panel bounds, labels, equations, units, legend, typography hierarchy, palette roles, connector types, and z-order.
3. Inventory the reference visual grammar: outer and panel boundaries, stage indicators, semantic containers, distinctive shapes, repeated motifs, title bars, branch/merge markers, connector and line styles, shape families, typography class, palette roles, and density/whitespace character.
4. Inventory every visible item as native text, shape/vector, connector/line, quantitative plot, repeated motif, or irreducible raster evidence.
5. Mark unreadable text, hidden endpoints, uncertain arrow direction, and ambiguous grouping. Do not invent missing scientific content.
6. Inspect the destination Figma file and establish a preserve set before writing.

## Choose Fidelity Deliberately

Must preserve unless redesign is explicit:

- panel decomposition, stage count/order, scientific and branch/merge topology, relative grouping, semantic containers, connector direction/relation type, exact labels, equations, units, major semantic color roles, relative hierarchy, and panel identity;
- distinctive visual or scientific motifs and shape grammar, such as a triangular preprocessing motif, energy-level diagram, circular feedback loop, nested mechanism, or continuous analysis band;
- the major typography class when identifiable and the reference's approximate information density, compactness, and whitespace character.

May improve without changing meaning or visual grammar:

- alignment, equal spacing, whitespace, text wrapping, clipping, minor sizing, line cleanliness, connector smoothness, minor color normalization, legibility, and source-artwork inconsistencies;
- replacement of decorative raster labels/arrows/frames with native objects;
- simplification of incidental decoration that does not carry scientific meaning.

Must not invent unless redesign is explicit:

- new semantic containers or grouping, new arrows or semantic colors, or a different panel hierarchy;
- a generic card in place of a distinctive shape;
- a stronger causal relation in place of a weak/associative relation;
- aesthetic normalization that changes grouping, branch/merge meaning, or turns every object into a rounded rectangle.

Do not pixel-trace incidental imperfections. Retain attribution and licensing notes for third-party assets when applicable.

## Rebuild in Regions

Define a `reconstruction_spec` by extending the standard `figure_spec` with:

- reference dimensions and coordinate mapping;
- a compact fidelity declaration made before drawing;
- the visual grammar inventory and any uncertain grammar;
- region bounds and construction order;
- source-to-Figma object mapping;
- exact text/transcription status;
- connector source, target, direction, route, and confidence;
- raster decomposition decisions;
- preserve list and local acceptance condition.

Use a declaration such as:

```yaml
reference_fidelity:
  topology: exact
  labels: exact
  equations: exact
  units: exact
  panel_structure: high
  grouping: high
  shape_grammar: high
  palette: high | approximate
  typography: high | approximate
  layout: high | medium
  density: high | approximate
  whitespace_character: preserve | normalize
  compactness: preserve | improve_conservatively
  allowed_improvements: [alignment, spacing, clipping, legibility, connector cleanliness]
```

Adjust levels to the request and source quality. Any departure outside `allowed_improvements` requires explicit redesign authority or a recorded scientific/source constraint.

Preserve approximate content-to-canvas utilization as part of reconstruction fidelity. Do not expand a compact journal figure into a spacious slide or UI layout unless redesign is explicit. `normalize` or `improve_conservatively` permits removing source-artwork inefficiency without collapsing scientific hierarchy; it does not permit arbitrary density expansion or aggressive packing.

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

Audit reference correspondence as its own gate: check for missing motifs; invented containers; altered grouping, shape class, panel hierarchy, connector grammar, branch/merge structure, semantic colors, typography class, or density; unjustified whitespace expansion; and omitted meaningful borders, bands, or stage indicators. Exact text, valid arrows, clean layout, and editability are necessary but do not by themselves establish reconstruction fidelity.

Prefer semantic correctness over superficial similarity whenever the two conflict, and report the conflict rather than silently redesigning.
