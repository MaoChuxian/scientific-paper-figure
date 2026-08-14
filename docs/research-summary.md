# Research Summary

Research date: 2026-08-14. Repository metadata was read through the connected GitHub app. Direct `git clone` was attempted first but `github.com:443` was unreachable from the shell.

## Sources

### Official Codex Figma plugin (v2.0.17)

Read the installed `figma-use`, `figma-generate-design`, and `figma-create-new-file` skills and their UI metadata guidance.

Keep:

- compose with official skills instead of restating Plugin API syntax;
- inspect before writing, build incrementally, return affected node IDs, and validate each region;
- use native text, auto-layout containers, semantic names, components/instances for repeated structures, metadata, and fresh screenshots;
- stop and diagnose failed atomic calls before retrying;
- inspect existing files and preserve user-authored work during updates.

Do not duplicate:

- font loading, page switching, paint mutation, variable binding, Auto Layout API details, or generic component construction;
- design-system discovery rules intended for product UI screens rather than scientific figures.

### [icebird1998/scientific-illustrator](https://github.com/icebird1998/scientific-illustrator)

Observed: 536 stars, 44 forks, MIT, pushed 2026-08-08. Read the actual design, audit, correct, PowerPoint/draw.io reconstruction skills and README.

Keep:

- explicit scientific message, exact-label inventory, topology, panel plan, connector lanes, and raster decomposition;
- structure evidence plus renderer evidence; successful tool calls are not visual proof;
- stable defect records and minimal object-level corrections;
- fresh re-audit after correction and preservation of already approved regions.

Adapt for Figma:

- collapse Designer/Drawer/Reviewer/Corrector into logical phases of one orchestrator because official Figma tools already provide the Drawer;
- replace backend-specific score thresholds and tool maps with evidence-based hard gates and concise findings;
- do not require a separate Skill transition for every phase.

### [ResearAI/AutoFigure-Edit](https://github.com/ResearAI/AutoFigure-Edit)

Observed: 4,105 stars, 266 forks, MIT, pushed 2026-07-25. Read the README and core pipeline implementation.

Keep:

- structured editable output and explicit separation of reconstructable overlays from irreducible imagery;
- placeholder/region correspondence and coordinate alignment as useful reconstruction concepts.

Do not adopt:

- raster-first generation, SAM segmentation, background removal, and icon reinsertion as the default path;
- claiming fully editable output when extracted icon regions remain raster.

Figma can create semantic native nodes directly; raster decomposition is a fallback for microscopy, photographs, complex textures, or other irreducible evidence.

### [zsyggg/paper-craft-skills](https://github.com/zsyggg/paper-craft-skills)

Observed: 1,028 stars, 67 forks, pushed 2026-05-29. Read `paper-comic`, its paper-figure style reference, and `paper-analyzer`.

Keep:

- select only content that benefits from visualization;
- make the overview answer input, mechanism, novelty, output, and evidence;
- split a dense mechanism instead of shrinking labels or copying prose into boxes.

Do not adopt:

- raster image generation as the delivery contract;
- fixed page-count and promotional style rules that do not fit journal figures.

### [pengqianhan/codex-paper-figure-skill](https://github.com/pengqianhan/codex-paper-figure-skill)

Observed: 5 stars, 1 fork, MIT, pushed 2026-07-23. Read the complete Skill.

Keep:

- concise figure brief fields and an editable-output contract;
- exact terms, entities, relationships, and constraints before drawing.

Do not adopt:

- mandatory image generation before editable reconstruction;
- embedding a full draw.io XML tutorial inside the domain Skill;
- third-party icon browsing as a default dependency.

### [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp)

Observed: 5,184 stars, 322 forks, Apache-2.0, pushed 2026-08-03. Read the Codex Skill and plugin README.

Keep:

- author in the native editable format, validate the artifact, then export;
- select the simplest authoring method that preserves structure;
- keep shared low-level format guidance outside the main Skill.

### [VILA-Lab/FigMirror](https://github.com/VILA-Lab/FigMirror)

Observed: 500 stars, pushed 2026-08-12. Read the README and repository structure.

Keep:

- far-view plus close-view visual review;
- reviewer bounding boxes/regions and an accumulating preserve list;
- generate quantitative figures from real data as editable Matplotlib code and vector output.

Scope boundary:

- FigMirror addresses quantitative plot style transfer, not Figma schematic composition. It supports the policy that charts should be generated from data outside Figma and imported as vectors.

## Search Result

Additional GitHub searches for Figma scientific-diagram agents and editable graphical-abstract agents found no mature Figma-native scientific-figure Skill. The available projects mostly target raster generation, SVG reconstruction, draw.io/PowerPoint, or Matplotlib plots. This supports a thin scientific orchestrator layered on the official Figma plugin.

