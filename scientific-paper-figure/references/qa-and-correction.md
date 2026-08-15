# QA and Correction

Use current structure evidence and current rendered evidence. Keep review logically separate from construction even when one agent performs both.

## Collect Two Evidence Channels

### Structure

Inspect node types, names, hierarchy, bounds, visibility, clipping, text contents, connector geometry, grouping, repeated dimensions, raster fills, and affected node IDs. Verify that reconstructable objects remain native and independently selectable.

### Rendering

Capture the whole figure and close views of dense panels or corrected regions. Inspect actual text fit, overlap, hierarchy, visual balance, route clarity, arrowheads, and imported artifacts.

Do not substitute metadata for appearance or screenshots for editability.

Use targeted structure reads after local writes: affected objects plus endpoints, parent, and neighbors whose geometry or preserve status can be influenced. Before final approval, perform one whole-root structural audit. Render by risk rather than after every write, and collect a complete defect list before the correction pass so compatible fixes and verification can be batched.

## Audit Taxonomy

1. **Scientific correctness**: message, exact labels, equations, units, stage order, topology, arrow direction, conditions, and required evidence.
2. **Information compression**: clear inputs, mechanism, novelty, outputs, and validation without prose copied into boxes.
3. **Geometry and compactness**: alignment, spacing hierarchy, repeated dimensions, panel balance, content-driven bounds, whitespace justification, information density, and publication-area efficiency.
4. **Typography**: effective size at final publication dimensions, minimum final point size, font family/profile consistency, reference typography-class fidelity, hierarchy, wrapping, and clipping.
5. **Connectors**: relation grammar, endpoint clearance, crossings, backtracking, path-through-object, label intersection, and branch clarity.
6. **Editability**: native text/shapes/connectors, meaningful hierarchy, stable names, useful groups/components, and no unjustified flattening.
7. **Raster policy**: irreducibility, atomic fields, tight crops, and editable overlays.
8. **Rendering**: blank/missing assets, clipping, unintended overlap, occlusion, inconsistent styles, and export bounds.
9. **Change containment**: preservation of manual edits and regions outside the requested change set.
10. **Palette**: registered-HEX compliance, semantic-role reuse, contrast, unnecessary proliferation, unauthorized colors, color-only encoding, and reference palette fidelity.
11. **Reference correspondence**: panel hierarchy, relative grouping, meaningful borders/bands/stage indicators, distinctive motifs and shapes, connector/branch/merge grammar, typography class, palette roles, approximate information density and compactness, whitespace character, and absence of invented semantic structure.

For automated contrast QA, enumerate normal-text/background pairs and essential boundary/adjacent-color pairs, calculate standard sRGB relative luminance, and compare `(lighter + 0.05) / (darker + 0.05)`. Target at least 4.5:1 for normal text and 3:1 for essential non-text boundaries. Treat failures as QA findings while distinguishing faithful reconstruction from an optional accessible variant.

## Audit Compactness

For the whole canvas and each major panel, compare current bounds with content bounds, text bounds, connector clearance, and necessary padding. Check for excessive panel padding, inter-object gaps, title-to-content gaps, oversized containers, inconsistent spacing levels, large unexplained empty regions, low content-to-canvas utilization, unnecessary canvas expansion, reconstruction density deviation, and whitespace that would force avoidable publication downscaling. A figure does not pass merely because it is aligned, readable, and free of overlap when it remains unnecessarily sparse.

Use a qualitative compression test: ask whether roughly 15-25% of the current empty space could be removed without harming readability, scientific hierarchy, connector clearance, semantic grouping, or reference fidelity. Treat this range as a diagnostic prompt, never an automatic compression target. For every major empty region, record its purpose or reduce it.

## Finding Contract

Record one defect per finding:

```yaml
finding_id: stable id
region: stable panel/region id
objects: [exact names or node ids]
category: scientific | compression | geometry | compactness | typography | connector | editability | raster | rendering | containment | palette | reference-correspondence
severity: hard | warning
evidence: measurable structure or visible observation
root_cause: concise diagnosis if known
correction: required outcome
preserve: [objects or properties that must not change]
acceptance: condition verifiable in the next audit
```

Treat these as hard failures:

- wrong or invented scientific content;
- wrong direction or relationship type;
- missing required stage, condition, unit, equation, or validation;
- clipped or unreadable essential text;
- unintended overlap or connector route through unrelated content;
- ambiguous arrowhead on a scientific relationship;
- reconstructable labels, arrows, frames, legends, axes, or regular plots flattened into raster;
- unexplained composite raster evidence;
- unintended edits outside the requested region.

In reconstruction mode, also treat an invented semantic container, altered panel/branch/merge hierarchy, missing scientifically or compositionally distinctive motif, or unjustified relation/shape-grammar change as hard. Record lesser typography, palette, or density deviations as hard when the fidelity contract requires exact/high correspondence; otherwise record a warning with the declared approximation.

## Minimal-Delta Correction

1. Identify the smallest responsible object set.
2. Classify the root cause: content/topology, geometry, text metrics, connector route, grouping/z-order, raster decomposition, or style token.
3. Define the preserve set, including nearby approved objects and user changes.
4. Order dependent operations: content/object creation, geometry, text fit, connectors, grouping/z-order, then alignment/distribution.
5. Change in place when possible. Delete and replace only the exact defective object when in-place correction cannot express the fix.
6. Inspect the changed objects and immediate neighbors structurally.
7. Capture a fresh close view and whole-figure view.
8. Verify the finding's acceptance condition and check for regressions.

Never replace a panel to fix one arrow. Never rebuild the whole figure to fix one panel. Never apply a global style mutation without enumerating all affected approved objects.

For excessive whitespace, correct in this order: reduce unnecessary outer canvas space, panel padding, and excessive gaps; shrink oversized containers to content; rebalance neighboring regions; reroute connectors only when compression requires it; and consider modest typography changes only if final publication constraints still require them. Preserve type size whenever container compression solves the defect.

After compression, inspect structure, capture a close regional render and a whole-figure render, and verify final-size typography. Reject clipping, crowded labels, connector collisions, ambiguous branch/merge routes, collapsed hierarchy, or insufficient separation between unrelated groups.

## Common Corrections

### Wrong or unclear connector

Confirm source, target, relationship type, direction, and intended lane. Move nodes only when routing cannot be fixed cleanly. Preserve approved node geometry where possible. Recheck the route after any node movement.

- For nested nodes, verify endpoint anchors in page/global coordinates rather than assuming parent-local coordinates.
- For every branch or merge junction, count incoming paths, outgoing paths, and arrowheads; keep markers and arrowheads from occluding one another.
- Compare line caps, joins, bend radii, stroke classes, and arrowhead treatment with neighboring connectors of the same semantic class.
- Capture a close render with enough effective resolution to distinguish stroke joins, endpoints, and arrowheads; never approve a connector correction from a downsampled whole-figure render alone.

### Uneven repeated objects

Make equal-role objects equal in size, align the intended edge or center, preserve outer anchors, distribute consistently, and then recheck connectors and labels.

### Text clipping or over-density

Correct the text first, set deliberate bounds/wrapping, enlarge the responsible container or reduce nonessential prose, and preserve final-size legibility. Do not rasterize, auto-shrink to unreadability, or move unrelated panels without need.

### Composite raster

Split independent evidence fields. Rebuild reconstructable titles, borders, arrows, legends, axes, and annotations as native objects. Record the reason and crop for every retained raster field.

### Manual user edits present

Reinspect rather than relying on prior node IDs or screenshots. Add user-changed objects/properties to the preserve set. If the requested correction necessarily conflicts with a manual edit, report the exact conflict before overwriting it.

## Acceptance Gate

Approve only when:

- every hard finding is resolved with fresh evidence;
- exact scientific content and topology pass;
- essential text is readable, unclipped, and above the declared minimum at intended size;
- no unintended overlap or incoherent connector path remains;
- every color is registered or explicitly justified, semantic roles are consistent, contrast targets are checked, and important distinctions do not depend on color alone;
- reconstruction mode passes the declared visual-grammar and reference-correspondence contract without invented semantic structure;
- every reconstructable element is editable and every raster exception is justified;
- stable semantic names and meaningful panel hierarchy remain;
- the preserve set is unchanged except for explicitly approved effects;
- the whole-figure render remains coherent after local corrections.

Report warnings only for genuine source ambiguity or an explicitly accepted limitation; do not use warnings to excuse correctable defects.
