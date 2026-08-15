# Test Results

## Completed

- Skill initialized with the official `skill-creator` script.
- `quick_validate.py` passed: `Skill is valid!`.
- Frontmatter contains only valid required metadata, the description is 673 characters, and no TODO placeholders remain.
- `agents/openai.yaml` contains a 52-character UI description and a default prompt that explicitly names `$scientific-paper-figure`.
- The backend-neutral orchestration refactor passed `quick_validate.py` on 2026-08-15 (`Skill is valid!`); `SKILL.md` remains 65 measured nonblank/content lines and delegates backend mechanics to `references/figma-backends.md`.
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
- TalkToFigma: completed a 44-step scientific run on channel `m4omllkl`, including native stage/label construction, structured layout, manual label/stage edits, overlap and clipping correction, and cleanup read-back. The larger-root PNG export timed out; the earlier smoke export remains a separate passed visual-export result. The stale failed-run frames were removed explicitly.
- TalkToFigma export retest: with an outer 120-second MCP timeout, a focused 1500x520 root again returned `Error exporting node as image: Request to Figma timed out` after about 30 seconds. Increasing the client timeout does not resolve the backend/plugin export timeout.
- TalkToFigma preserve-set check: after a controlled socket restart and reconnect on channel `2qf7edgv`, a targeted edit changed only Panel B; before/after metadata for untouched Panel A was byte-equal (`preserveSetStable: true`).

## Scenario B: Paper Method to Figure

Scenario B passed through Figwright 0.4.0 on 2026-08-15. Before drawing, the run recorded entities, processes, inputs, outputs, novelty, evidence, constraints, relationship types, path separation, ambiguities, and a compact `figure_spec` under the Git-ignored `.evidence/scenario-b-method-to-figure/` directory.

The editable Figma result is root `21:112` (`scenario-b/method-to-figure`) in `Scientific Paper Figure Skill Tests` / `Page 1`. It contains an overview, mechanism inset, and independent validation/evidence region. Final structural inspection counted 122 native nodes: 42 frames, 2 rectangles, 38 text nodes, and 40 vector connector children; no IMAGE nodes or IMAGE fills were present. Exact-label read-back included `tensile strength`, `wavelength regions`, `TRAINING ONLY`, and `Held-out batch split`.

Regional read-back and rendered evidence were collected after each logical region. One genuine typography defect was found: the mechanism heading wrapped into its explanatory note. A minimal-delta correction resized only heading node `25:170` from `560 x 30` to `800 x 30`; fresh regional and whole-figure renders then passed. The pre-existing preserve root `1:2` had exact full-JSON equality before and after (`13,852` characters each). Final scientific, compression, topology, visual hierarchy, typography, connector, editability, rendering, and publication-readability checks passed. Raw logs and PNGs remain outside Git; the final render is `.evidence/scenario-b-method-to-figure/renders/audit/21-112.png`.

Observed connection/recovery and batched-render failures were attributed to backend reliability or the local test harness, not to scientific logic. No `SKILL.md` change is proposed from Scenario B, and Scenario C was not started.

The A-E specifications remain replayable after the tool-call quota resets or the Figma plan changes. Do not claim official B-E as live-passing until fresh structure and screenshot evidence is collected. See [backend-comparison.md](backend-comparison.md) for the local-backend decision record.

## Remaining Limits

- No journal-specific numeric tokens are hardcoded; final typography and dimensions still depend on venue, canvas, and target-size constraints.
- Quantitative plot generation is intentionally delegated to the user's data tool; this Skill validates composition and editability of imported artifacts but does not replace statistical plotting software.
- Domain-specific conventions remain an ambiguity surface and should be supplied by the paper, reference, or user when generic topology is insufficient.
