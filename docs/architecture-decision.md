# Architecture Decision

## Candidates

| Candidate | Context cost | Trigger reliability | Maintenance | Figma integration | Decision |
|---|---:|---:|---:|---:|---|
| One monolithic Skill | High | High | Low | Medium | Reject: repeats official Figma rules and loads every figure mode every time. |
| Orchestrator plus separate design, reconstruct, audit, and correct Skills | Medium-high | Medium | Medium-low | Medium | Reject for now: phase handoffs add trigger/state overhead and duplicate shared scientific context. |
| One orchestrator Skill plus conditional references | Low-medium | High | High | High | Select: one stable entry point, official Figma skills remain the execution layer, and mode-specific detail loads only when needed. |

## Selected Structure

```text
scientific-paper-figure/
|-- SKILL.md
|-- agents/openai.yaml
`-- references/
    |-- scientific-planning.md
    |-- reconstruction.md
    |-- publication-style.md
    `-- qa-and-correction.md
```

No scripts are required. Figma construction, inspection, screenshots, and file creation are already deterministic tool capabilities maintained by the official plugin. No rigid visual templates are included because journal constraints, reference styles, disciplines, and aspect ratios vary materially.

## Responsibility Boundary

`scientific-paper-figure` owns scientific abstraction, topology, panel decomposition, data-vs-schematic decisions, publication constraints, reconstruction judgment, QA findings, and minimal-delta correction.

Official Figma skills own Plugin API mechanics, file creation, node construction, fonts, Auto Layout mechanics, variables/styles, incremental calls, returned node IDs, and generic Figma error recovery.

## Modes

- Create from manuscript, Method, brief, or concept.
- Reconstruct scientific communication from a reference figure.
- Edit a named region while preserving user changes elsewhere.
- Audit and correct an existing editable figure.

All modes share a `figure_spec`, stable semantic node names, a preserve set, structure-plus-render review, and the same completion gate. This shared state is why a single orchestrator is preferable.

