# Test Results

## Completed

- Skill initialized with the official `skill-creator` script.
- `quick_validate.py` passed: `Skill is valid!`.
- Frontmatter contains only valid required metadata, the description is 673 characters, and no TODO placeholders remain.
- `agents/openai.yaml` contains a 52-character UI description and a default prompt that explicitly names `$scientific-paper-figure`.
- Five replayable acceptance scenarios were defined in [scenarios.md](scenarios.md), including expected structure and QA assertions.
- Research covered the installed official Figma Skills plus Scientific Illustrator, AutoFigure-Edit, paper-craft-skills, codex-paper-figure-skill, drawio-mcp, FigMirror, and an additional GitHub search.
- Figma authentication was restored and a live test file was created: [Scientific Paper Figure Skill Tests](https://www.figma.com/design/NiVmDGQL0iO1FtSpWsoUCH).
- Live Test A created one native figure frame, six semantic stage frames, six editable text labels, five editable line segments, five editable arrowhead polygons, and one annotation.
- Test A structure metadata confirmed stable semantic names and independent text, frame, line, polygon, and annotation nodes.
- Test A visual QA found bidirectional line caps, replaced only the five connector caps with explicit single-end arrowhead objects, then found and corrected reversed arrowhead rotation. The final fresh screenshot showed a left-to-right process path with no clipped labels or route-through-object defects.

## Backend Runtime Results

Figma authentication is valid. The official Starter plan reached its MCP tool-call limit when live Test B attempted to create its first frame; the failed `use_figma` call was atomic, so it did not create a partial Test B region. This remains an official-control limitation, not a local-backend result.

Live status:

- Test A: passed after two minimal connector corrections.
- Test B: blocked before construction by the Starter-plan MCP limit.
- Tests C-E: not live-executed through the official backend because the same limit applies to all Figma MCP calls.

Local backend evidence:

- Figwright: passed the complete 40-step batch run in the dedicated file, including native creation, read-back, fresh PNG renders, a simulated manual edit, local overlap/clipping correction, and cleanup. Raw evidence is outside Git under `.evidence/backend-eval-20260815/`.
- TalkToFigma: passed the equivalent read/write/structured-layout/PNG-export smoke sequence and deletion read-back on channel `qyf00z2n`. Full scientific A/D/E parity was not rerun in this checkpoint and remains explicitly untested.

The A-E specifications remain replayable after the tool-call quota resets or the Figma plan changes. Do not claim official B-E as live-passing until fresh structure and screenshot evidence is collected. See [backend-comparison.md](backend-comparison.md) for the local-backend decision record.

## Remaining Limits

- No journal-specific numeric tokens are hardcoded; final typography and dimensions still depend on venue, canvas, and target-size constraints.
- Quantitative plot generation is intentionally delegated to the user's data tool; this Skill validates composition and editability of imported artifacts but does not replace statistical plotting software.
- Domain-specific conventions remain an ambiguity surface and should be supplied by the paper, reference, or user when generic topology is insufficient.
