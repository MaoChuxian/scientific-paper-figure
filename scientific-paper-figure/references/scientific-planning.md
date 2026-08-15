# Scientific Planning

Use this reference for every figure. Keep the working spec concise enough to guide construction and QA.

## Extract the Visual Story

Reduce the source to these roles:

- **entities**: samples, devices, datasets, variables, organisms, modules, models, and environments;
- **processes**: preparation, transformation, inference, training, measurement, intervention, and analysis;
- **inputs/outputs**: material, signals, features, predictions, decisions, products, and observations;
- **novelty**: the new mechanism, representation, control, comparison, or integration;
- **evidence**: assays, metrics, ablations, images, plots, validation cohorts, and uncertainty;
- **constraints**: conditions, branches, timing, assumptions, and exclusions;
- **relationships**: sequence, causality, data transfer, control, feedback, association, comparison, and hierarchy.

Discard prose that does not change the visual explanation. Do not promote every noun to a node or every sentence to a panel.

## Test the Message

Make the figure answer, in the intended reading order:

1. What goes in?
2. What happens?
3. What is new?
4. What comes out?
5. How is it validated?

If the figure is intentionally narrower, explicitly mark omitted questions as out of scope.

## Define Topology

Assign a relationship type before choosing a visual connector:

| Relationship | Meaning | Default visual treatment |
|---|---|---|
| process | chronological or procedural next step | solid directed arrow |
| causal | one state produces or changes another | emphasized directed arrow |
| data | information or tensor transfer | directed arrow with data label where needed |
| control | parameter, gating, or regulation | distinct thin/dashed directed line |
| feedback | downstream state returns upstream | routed return arrow, explicitly labeled |
| inhibition | suppresses or blocks | blunt/T endpoint or discipline-standard notation |
| association | related but not causal | undirected or light dashed line |
| comparison | alternatives evaluated side by side | shared alignment/container, not a fake causal arrow |
| hierarchy | containment, composition, or scale | nesting, bracket, or tree structure |

Honor discipline-specific conventions when the source or user defines them. Include a legend when more than two non-obvious connector grammars appear.

## Decompose Panels

Create a panel only when it has a distinct question or scale of explanation. Prefer one clear overview plus selected detail/evidence panels over many equally weighted boxes.

Common structures:

- **Method/workflow**: input -> preparation -> intervention/transformation -> measurement -> analysis -> evidence.
- **ML pipeline**: data -> preprocessing -> representation/features -> training -> interpretation/evaluation -> prediction/deployment.
- **Model architecture**: input representation -> core blocks -> fusion/attention/routing -> heads/losses -> outputs; separate training-only paths from inference.
- **Mechanism schematic**: context/compartments -> actors -> interactions -> state change -> phenotype/readout -> supporting evidence.
- **System/framework**: physical/data boundary -> modules -> interfaces -> control/data flows -> outputs -> validation.
- **Graphical abstract**: problem/context -> central innovation -> outcome/impact, with fewer details than a Method figure.
- **Multi-panel figure**: overview, mechanism/detail, quantitative evidence, and validation arranged by the paper's argument.

Split a panel when fitting it would require long prose, tiny text, ambiguous routing, or more than one unrelated reading path. Merge panels when they repeat the same semantic role.

## Figure Spec Contract

Use this structure in working notes; omit empty optional fields:

```yaml
mode: create | reconstruct | edit | audit
message: one sentence
audience: discipline and assumed knowledge
reading_order: left-to-right | top-to-bottom | cycle | explicit sequence
style_source: user | journal | reference | existing-file | default
target:
  aspect_ratio: value or source
  final_width: value and unit if known
  final_height: value and unit if known
  column_mode: single | double | full-page | screen | other
  final_use: print | screen | both
  export: Figma plus requested formats
publication:
  publisher_or_profile: venue name, custom brief, or unspecified
  target_width_mm: value if known
  target_height_mm: value if known
typography:
  profile: classic-paper | modern-paper | user-preferred | reference
  family: exact family or approved fallback
  body_final_pt: value or range
  secondary_final_pt: value or range
  section_title_final_pt: value or range
  panel_label_final_pt: value or range
  minimum_final_pt: hard floor for essential text
palette:
  source: reference | journal | user | custom | amfe | colorblind-safe
  locked: true
  neutral_text: registered HEX
  background: registered HEX
  semantic_roles: {role: registered HEX}
  allowed_hex: []
  max_semantic_accents: value
exact_content:
  labels: []
  equations: []
  units: []
  required_relations: []
panels:
  - id: panel-a
    question: what this panel must explain
    bounds_role: overview | detail | evidence | validation
    objects: []
    incoming: []
    outgoing: []
    acceptance: []
topology:
  - source: stable object id
    target: stable object id
    relation: process | causal | data | control | feedback | inhibition | association
    label: optional exact label
    route: intended lane and endpoints
    source_anchor: optional side or point
    target_anchor: optional side or point
    junction: optional stable junction id and merge/branch role
    arrowhead: target | none
data_artifacts:
  - panel: stable panel id
    source: real data or supplied vector
    generator: Python/Matplotlib/Origin/other
raster:
  - object: stable object id
    reason: why native reconstruction is scientifically inappropriate
    atomic: true
    overlays_rebuilt: []
preserve: []
ambiguities: []
```

Record intended final dimensions before approving type. If they are unavailable, mark them as unresolved and avoid claiming publication-size compliance. Lock palette values before styling: once `locked` is true, use only registered HEX values unless the working spec records an explicit reference-fidelity, scientific, or user-requested reason.

## Plan Connectors Before Drawing

- Reserve outer or inter-row lanes for cross-panel and feedback routes.
- Connect from the side facing the destination; avoid immediate backtracking.
- Keep routes outside unrelated nodes and labels.
- Separate parallel routes consistently.
- Use few bends and make branch/merge points explicit.
- Assign arrowheads to the segment that carries direction; incoming merge segments are normally arrowless when one outgoing arrow defines the merged flow.
- Reposition nodes before accepting avoidable crossings.
- Leave clearance for arrowheads and labels at both endpoints.

## Resolve Missing Information

Ask only when a choice changes scientific meaning, such as whether a line is causal or associative, whether a stage occurs only during training, or which condition is the control. For visual choices without a specified constraint, use a conservative academic default and record the assumption.
