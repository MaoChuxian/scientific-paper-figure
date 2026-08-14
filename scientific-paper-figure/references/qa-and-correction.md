# QA and Correction

Use current structure evidence and current rendered evidence. Keep review logically separate from construction even when one agent performs both.

## Collect Two Evidence Channels

### Structure

Inspect node types, names, hierarchy, bounds, visibility, clipping, text contents, connector geometry, grouping, repeated dimensions, raster fills, and affected node IDs. Verify that reconstructable objects remain native and independently selectable.

### Rendering

Capture the whole figure and close views of dense panels or corrected regions. Inspect actual text fit, overlap, hierarchy, visual balance, route clarity, arrowheads, and imported artifacts.

Do not substitute metadata for appearance or screenshots for editability.

## Audit Taxonomy

1. **Scientific correctness**: message, exact labels, equations, units, stage order, topology, arrow direction, conditions, and required evidence.
2. **Information compression**: clear inputs, mechanism, novelty, outputs, and validation without prose copied into boxes.
3. **Geometry**: alignment, spacing, repeated dimensions, panel balance, whitespace, and bounds.
4. **Typography**: hierarchy, wrapping, final-size legibility, font consistency, and clipping.
5. **Connectors**: relation grammar, endpoint clearance, crossings, backtracking, path-through-object, label intersection, and branch clarity.
6. **Editability**: native text/shapes/connectors, meaningful hierarchy, stable names, useful groups/components, and no unjustified flattening.
7. **Raster policy**: irreducibility, atomic fields, tight crops, and editable overlays.
8. **Rendering**: blank/missing assets, clipping, unintended overlap, occlusion, inconsistent styles, and export bounds.
9. **Change containment**: preservation of manual edits and regions outside the requested change set.

## Finding Contract

Record one defect per finding:

```yaml
finding_id: stable id
region: stable panel/region id
objects: [exact names or node ids]
category: scientific | compression | geometry | typography | connector | editability | raster | rendering | containment
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

## Common Corrections

### Wrong or unclear connector

Confirm source, target, relationship type, direction, and intended lane. Move nodes only when routing cannot be fixed cleanly. Preserve approved node geometry where possible. Recheck the route after any node movement.

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
- essential text is readable and unclipped at intended size;
- no unintended overlap or incoherent connector path remains;
- every reconstructable element is editable and every raster exception is justified;
- stable semantic names and meaningful panel hierarchy remain;
- the preserve set is unchanged except for explicitly approved effects;
- the whole-figure render remains coherent after local corrections.

Report warnings only for genuine source ambiguity or an explicitly accepted limitation; do not use warnings to excuse correctable defects.

