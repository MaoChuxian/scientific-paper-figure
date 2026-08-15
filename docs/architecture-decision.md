# Architecture Decision

## Candidates

| Candidate | Context cost | Trigger reliability | Maintenance | Figma integration | Decision |
|---|---:|---:|---:|---:|---|
| One monolithic Skill | High | High | Low | Medium | Reject: repeats official Figma rules and loads every figure mode every time. |
| Orchestrator plus separate design, reconstruct, audit, and correct Skills | Medium-high | Medium | Medium-low | Medium | Reject for now: phase handoffs add trigger/state overhead and duplicate shared scientific context. |
| One orchestrator Skill plus conditional references | Low-medium | High | High | High | Select: one stable entry point, a backend-neutral capability contract, and details loaded only when needed. |

## Selected Structure

```text
scientific-paper-figure/
|-- SKILL.md
|-- agents/openai.yaml
`-- references/
    |-- scientific-planning.md
    |-- figma-backends.md
    |-- reconstruction.md
    |-- publication-style.md
    `-- qa-and-correction.md
```

No in-Skill adapter scripts are required. The selected backend supplies execution mechanics behind the capability boundary. No rigid visual templates are included because journal constraints, reference styles, disciplines, and aspect ratios vary materially.

## Responsibility Boundary

`scientific-paper-figure` owns scientific abstraction, topology, panel decomposition, data-vs-schematic decisions, publication constraints, reconstruction judgment, QA findings, and minimal-delta correction.

The bound Figma backend owns transport and tool mechanics. Official Figma skills own those mechanics only when Official Figma is selected. The orchestration boundary requests inspection, native-object construction/editing, hierarchy, structured layout, connectors, read-back, fresh regional and whole-figure rendering, and requested export as capabilities rather than naming backend tools.

## Local Backend Decision

The 2026-08-15 local evaluation keeps the main Skill backend-neutral. Figwright 0.4.0 is the recommended local default for structured scientific construction and minimal correction: native writes, read-back, fresh PNG renders, manual-edit operations, and local defect correction all passed in the dedicated test file. TalkToFigma 0.3.5 is a recommended secondary backend for direct manipulation and Auto Layout; its native scientific run, correction path, and isolated preserve-set equality check passed, but a larger-root PNG export timed out. A controlled socket restart followed by plugin reconnect and a live `get_document_info` read-back passed on channel `2qf7edgv`.

The official Figma integration remains the control and fallback. Its Starter MCP quota prevented new B-E control runs, so this decision does not claim official parity. The Skill now binds a small orchestration-level capability contract while leaving scientific planning, reconstruction, publication style, QA taxonomy, preserve sets, and minimal-delta correction unchanged. Backend binding is sticky within a write transaction; failover requires live-state recovery before the fallback backend is bound.

## Modes

- Create from manuscript, Method, brief, or concept.
- Reconstruct scientific communication from a reference figure.
- Edit a named region while preserving user changes elsewhere.
- Audit and correct an existing editable figure.

All modes share a `figure_spec`, stable semantic node names, a preserve set, structure-plus-render review, and the same completion gate. This shared state is why a single orchestrator is preferable.
