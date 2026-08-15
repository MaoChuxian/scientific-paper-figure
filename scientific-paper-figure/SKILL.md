---
name: scientific-paper-figure
description: Create, reconstruct, edit, and audit publication-oriented scientific figures as native, structured, editable Figma objects. Use when Codex needs to turn manuscript or Method text into a workflow, mechanism schematic, model architecture, system/framework diagram, experimental pipeline, multi-panel figure, or graphical-abstract draft; rebuild the scientific communication of a supplied paper figure; modify only a named panel or object in an existing Figma figure; or review and correct scientific semantics, topology, connectors, readability, raster use, and editability. Keep quantitative plots data-driven and use Figma for schematic composition, annotation, and layout.
---

# Scientific Paper Figure

Create scientific communication in Figma, not a decorated flowchart. Keep reconstructable content native and editable. Treat scientific correctness, structure, rendered appearance, and preservation of user edits as independent requirements.

## Bind a Figma Backend

Read [figma-backends.md](references/figma-backends.md), respect an explicit backend choice, and otherwise prefer Figwright. Bind one backend before Figma construction, editing, or correction and request capabilities rather than backend-specific tool names throughout the scientific workflow.

Load and follow the official `figma-use`, `figma-create-new-file`, and `figma-generate-design` skills only when Official Figma is selected and their documented trigger applies. Keep the backend sticky for each write transaction; follow the reference's recovery sequence before any failover.

Before changing this Skill in response to a failure, attribute the failure using the reference taxonomy. Do not alter scientific logic to compensate for a backend defect.

## Select the Mode

Choose one primary mode while retaining the same QA loop:

1. **Create**: derive a figure from a manuscript section, Method, brief, or concept.
2. **Reconstruct**: recover the reference's scientific logic and structure as editable objects; read [reconstruction.md](references/reconstruction.md).
3. **Edit**: inspect the current Figma state, identify the requested region, and preserve everything outside the change set.
4. **Audit**: inspect without writing first, report findings, and correct only when the user requested correction.

For every mode, read [scientific-planning.md](references/scientific-planning.md). Read [publication-style.md](references/publication-style.md) before fixing visual tokens or preparing final-size output. Read [qa-and-correction.md](references/qa-and-correction.md) before audit, correction, or final approval.

## Establish Ground Truth

1. Inspect all supplied text, figures, equations, legends, journal constraints, data artifacts, and the existing Figma file.
2. Separate verified facts from inference. Preserve exact terms, symbols, equations, units, directions, stage order, and required panel labels.
3. Ask one concise question only when an unresolved ambiguity changes scientific meaning, required content, or the output target. Otherwise record a conservative assumption.
4. Treat the user's latest Figma edits as authoritative unless they conflict with an explicit scientific requirement; surface that conflict instead of silently reverting the edit.

## Produce a Figure Spec

Before drawing, define a compact `figure_spec` using the contract in [scientific-planning.md](references/scientific-planning.md). At minimum capture:

- one-sentence scientific message and intended audience;
- inputs, processes, novelty, outputs, validation/evidence, and reading order;
- exact-label inventory and scientific topology;
- panels/regions with stable semantic IDs, hierarchy, bounds, and acceptance conditions;
- visual grammar, connector semantics, data-plot handoffs, and raster declarations;
- style source, compactness and content-driven sizing intent, target aspect ratio, final-use constraints, ambiguity list, and preserve set.

Do not map manuscript paragraphs directly to boxes. Compress prose into entities, transformations, relationships, evidence, and comparisons.

## Plan Before the First Write

Resolve the scientific topology and the first-pass geometry before creating canvas objects. The working plan must name the panels, principal nodes, connector lanes and anchors, typography and palette tokens, approximate bounds, spacing hierarchy, density, and expected canvas size. For reconstruction, also finish the region/object mapping and visual-grammar inventory first.

Use the plan to form bounded logical transactions such as one panel, one connector family, or one idempotent global patch. Do not discover the figure by alternating one-object writes with design decisions that could have been made from the source and `figure_spec`.

## Separate Data From Schematic Work

- Generate scatter plots, heatmaps, SHAP plots, parity plots, learning curves, error bars, and other quantitative charts from real data in Python, Matplotlib, Origin, or the user's plotting tool.
- Import quantitative results as SVG/PDF/vector artifacts when supported, then add editable panel labels, legends, callouts, and composition in Figma.
- Never fabricate quantitative geometry from prose or redraw a data plot by eye when source data or an existing vector artifact is available.
- Retain raster only for scientifically irreducible evidence such as microscopy, photographs, or complex measured textures. Keep titles, borders, scale-bar labels, arrows, legends, and annotations as separate editable objects.

## Construct Incrementally

1. Inspect the target page and nearby conventions before adding nodes.
2. Create or identify one top-level figure frame with a semantic name. Establish margins, panel grid, reading order, shared anchors, and reserved connector lanes.
3. Build one logical panel or region per major construction step. Use native text, shapes, vectors, lines, frames, and Auto Layout where structurally appropriate.
4. Name important nodes by scientific role, for example `panel-a/data-input`, `stage/feature-engineering`, or `connector/training-to-evaluation`. Avoid names such as `Rectangle 42` for meaningful objects.
5. Use components or instances for repeated motifs only when repetition is real and future synchronized editing is useful.
6. Keep panel frames, labels, shapes, connectors, imported data artifacts, and irreducible raster evidence independently selectable.
7. Return all affected node IDs from writes. Maintain a change set and preserve set across calls.
8. Validate the new region in whole-figure context and at readable close range before building the next region.

Prefer batch writes for independent operations in the same logical transaction, but keep batches bounded by the backend's observed latency and failure semantics. Separate large runs of font-loading text creation, complex SVG import, and styling when a mixed batch could cross a timeout. A timed-out write has unknown commit status until live readback proves otherwise: inspect the intended parent and semantic names, classify completed/missing/duplicate objects, and apply only the missing delta. Never blindly replay a timed-out create batch.

After a successful region write, read back the affected nodes and only the neighbors needed to verify bounds, hierarchy, endpoints, and preservation. Reserve whole-root structural inspection for the final audit or when a local change may have propagated globally.

Prefer exact, stable geometry over premature decoration. Do not flatten a panel to hide construction defects.

## Review and Correct

After each major region and after the whole figure:

1. Capture fresh structural evidence plus fresh regional and whole-figure rendered evidence.
2. Audit scientific semantics, topology, geometry, typography, connectors, editability, raster policy, and rendering using [qa-and-correction.md](references/qa-and-correction.md).
3. Record each defect with region, objects, evidence, correction outcome, preserve set, and measurable acceptance condition.
4. Diagnose the root cause and apply the smallest responsible object-level change.
5. Reinspect the changed objects and their neighbors, capture fresh rendered evidence, and rerun the affected checks.

Choose render coverage by visual risk. Exact text or registered-token replacement can use structural verification plus a representative visual sample; size, spacing, and resize changes require a regional render; connectors, junctions, wrapping, clipping, compression, and reference-sensitive motifs require an affected-region render. Collect all visible defects in one review pass, batch compatible corrections, and then rerender the corrected regions. Always obtain complete visual coverage before final approval.

Never approve from stale evidence. Never redraw an accepted panel to fix one label or arrow.

## Completion Gate

Finish only when current evidence shows:

- exact required labels, equations, units, stages, directions, and relationships;
- a clear answer to what goes in, what happens, what is new, what comes out, and how it is validated;
- no clipped text, unintended overlap, path-through-object connectors, ambiguous arrowheads, or incoherent crossings;
- readable hierarchy at the intended publication size and a style consistent with the stated source;
- native editability for every reconstructable element and explicit justification for every raster object;
- a meaningful Figma hierarchy with stable panel and object names;
- no unintended change outside the requested edit region;
- a fresh whole-figure render plus structural inspection after the final correction.

Report the Figma file/node, construction mode, figure message, panels, data artifacts, raster exceptions, preserved regions, resolved findings, unresolved scientific ambiguities, and final QA verdict.
