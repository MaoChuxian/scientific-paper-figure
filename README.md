# Scientific Paper Figure

`scientific-paper-figure` is a Codex Skill for creating, reconstructing, editing,
and auditing publication-oriented scientific figures as native editable Figma
objects. The scientific workflow is backend-neutral; Figwright is the tested
default local backend, TalkToFigma is secondary, and Official Figma remains a
control or fallback.

## Repository layout

```text
scientific-paper-figure/
|-- SKILL.md                 Skill entry point
|-- agents/                  Codex UI metadata
`-- references/              Planning, style, reconstruction, QA, and backend rules
docs/                        Architecture, research, deployment, and migration notes
tests/                       Scenarios, verified results, and backend comparison
.research-repos/             Ignored local upstream clones
.evidence/                   Ignored raw logs and rendered evidence
.tmp/                        Ignored disposable working files
```

Only the Skill, maintained documentation, and verified test summaries belong in
Git. Upstream clones and raw evidence are intentionally machine-local so a clean
clone stays small.

## Start here

1. Read [the device migration guide](docs/device-migration.md) when setting up a
   new workstation.
2. Install or link `scientific-paper-figure/` into the local Codex skills
   directory.
3. Configure one Figma backend and verify it against a disposable object before
   editing a real figure.
4. Use [tests/scenarios.md](tests/scenarios.md) and
   [tests/results.md](tests/results.md) as the behavioral contract and evidence
   record.

The architecture rationale is in
[docs/architecture-decision.md](docs/architecture-decision.md). Empirical backend
limits and configuration shape are in
[docs/figma-backends.md](docs/figma-backends.md) and
[tests/backend-comparison.md](tests/backend-comparison.md).

## Development rules

- Keep `scientific-paper-figure/SKILL.md` concise; put detailed reusable guidance
  in `scientific-paper-figure/references/`.
- Do not commit credentials, Codex user configuration, upstream repositories, or
  raw Figma evidence.
- Record only verified test outcomes in `tests/results.md`.
- Keep generic Skill changes separate from figure-specific Figma mutations.
