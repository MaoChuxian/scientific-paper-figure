# Backend Comparison

Evaluation date: 2026-08-15. Official Figma MCP remains unchanged and is the control; its Starter-plan MCP quota blocked new Test B-E calls, so no unsupported parity claim is made.

## Evidence status

| Capability | Figwright 0.4.0 | TalkToFigma 0.3.5 | Evidence |
| --- | --- | --- | --- |
| read document / hierarchy / selection / named node | empirically verified | empirically verified | Dedicated file smoke and Figwright 40-step log |
| create frame / text / shapes | empirically verified | empirically verified | Native smoke; Figwright scenario A |
| SVG/vector authoring | documented, not tested in this run | documented, not tested in this run | Upstream source inspection |
| edit text / move-resize / fills-strokes | empirically verified | empirically verified | Figwright correction read-back; TalkToFigma smoke |
| styles / variables | documented, not tested in this run | documented, not tested in this run | Upstream tool inventory |
| Auto Layout | documented and empirically verified for basic layout | empirically verified for direction/padding/spacing/sizing | Structured-layout smoke |
| connectors | documented; fixture rectangles used in batch because direct line construction was not used | documented, not included in this smoke fixture | Scenario A and upstream inventory |
| batch writes | empirically verified through sequential scenario batch; native batch API not isolated | not tested | Figwright run log |
| stable node IDs / read-back | empirically verified | empirically verified | IDs and parent-child metadata |
| screenshot / export | PNG screenshot empirically verified | small smoke export passed; large-root export retest failed after ~30 s internal timeout | `.evidence/backend-eval-20260815/` |
| structural QA / visual QA | empirically verified | structural QA empirically verified; visual QA smoke-level only because full export timed out | Read-back plus render evidence |
| preserve manual edits | empirically verified (`SHAP (edited locally)` and moved stage retained) | empirically verified with before/after preserve-set equality on an untouched sibling panel | Figwright and Talk preserve fixtures |
| minimal correction | empirically verified (overlap and clipped annotation only) | empirically verified (overlap and clipped annotation only) | Both correction fixtures |
| Starter-compatible / cloud dependency | local relay and plugin; no cloud dependency observed | local socket and plugin; no cloud dependency observed | Deployment notes |

## Scored decision

Scores are 0-5 and reflect only observed evidence; untested rows are not treated as passes.

| Criterion | Weight | Figwright | TalkToFigma |
| --- | ---: | ---: | ---: |
| Scientific figure construction | 20% | 4 | 3 |
| Editing precision | 15% | 4 | 3 |
| Visual QA | 15% | 4 | 2 |
| Structural QA | 10% | 4 | 4 |
| Manual-edit preservation | 10% | 4 | 3 |
| Minimal correction | 10% | 4 | 3 |
| Agent/tool ergonomics | 8% | 3 | 3 |
| Reliability | 7% | 3 | 2 |
| Community/maintenance | 3% | 3 | 3 |
| Other justified evidence | 2% | 3 | 3 |
| **Weighted total** | **100%** | **3.8 / 5** | **2.9 / 5** |

## Recommendation

Figwright is the **Recommended default backend** for local scientific construction and local correction, based on the completed native-object, read-back, render, manual-edit, and minimal-correction run. TalkToFigma is a **Recommended secondary backend** for direct manipulation and Auto Layout operations: its scientific construction, edit, and correction path passed, but the larger-root export timed out and should be treated as a reliability limitation. Official Figma remains the control and fallback, but Starter MCP quota makes it unsuitable for repeated local evaluation runs.

No change to `scientific-paper-figure/SKILL.md` is justified by this evidence yet. The backend-neutral contract remains the safer integration boundary.
