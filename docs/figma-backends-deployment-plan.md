# Figwright + TalkToFigma Deployment, Validation, and Integration Plan

## 1. Objective and decision rule

Deploy and evaluate two local Figma MCP backends for `scientific-paper-figure`:

- Figwright: `awdr74100/figwright`
- TalkToFigma: `grab/cursor-talk-to-figma-mcp`

The existing official Figma integration remains installed, named distinctly, and available as a control and fallback. This plan does not assume that either local backend should be adopted. The final decision must be based on reproducible evidence for scientific construction, structural inspection, visual QA, minimal correction, preservation of manual edits, reliability, and agent ergonomics. Installation effort has zero decision weight.

Deployment is complete only after each backend passes this sequence on a real, isolated Figma document:

```text
connect -> read live state -> write native objects -> read back objects
       -> obtain rendered evidence -> edit existing objects -> verify the edit
```

Tool discovery, a plugin "connected" indicator, or a successful `ping` alone is not sufficient evidence.

## 2. Guardrails

1. Inspect upstream source, setup scripts, package metadata, releases, and relevant issues before running installation commands.
2. Prefer auditable repository/package-manager installation. Do not use blind remote `curl | shell` or `irm | iex` when an inspectable path exists.
3. Keep the official Figma MCP and its installed skills unchanged. Do not consume its Starter read quota for local-backend experiments except for the explicitly scoped control run.
4. Never commit tokens, credentials, authentication data, temporary plugin credentials, machine-specific absolute paths, or generated secrets.
5. Use a dedicated Figma test file, never a valuable research figure. Keep backend test areas isolated by page or top-level frame.
6. Keep the main Skill backend-neutral until empirical results justify an adapter or backend selection policy.
7. Do not vendor complete upstream repositories into this project. Store only concise, sanitized findings and reproducible test metadata.
8. For every write, retain affected node IDs and a change set. For every correction, retain a preserve set and verify unchanged neighboring nodes.

## 3. Repository deliverables

The following artifacts are planned; create only those that are useful after testing:

| Artifact | Purpose | When created |
| --- | --- | --- |
| `docs/figma-backends-deployment-plan.md` | This execution plan and acceptance gates | Before deployment |
| `docs/figma-backends.md` | Sanitized per-backend installation, lifecycle, version, and troubleshooting notes | After deployment research |
| `tests/backend-comparison.md` | Capability matrix, evidence links, scores, and final recommendation | After A/B tests |
| `tests/backend-fixtures.md` (optional) | Reproducible backend-neutral figure specs and defect fixtures | Only if fixtures cannot live in existing scenarios |
| `scripts/` (optional) | Small, audited Windows launch/verification helpers | Only if repeated manual steps justify them |

Update `tests/results.md` only with live results; do not mark blocked or unexecuted scenarios as passing. Update `docs/architecture-decision.md` only after the final evidence review.

## 4. Phase 0: Baseline and environment inventory

**Purpose:** establish a control and record the Windows environment before adding local services.

Tasks:

- Read `scientific-paper-figure/SKILL.md`, all current references, `tests/scenarios.md`, `tests/results.md`, and architecture/research docs.
- Record OS build, Node version, Bun version (if present), Git, GitHub CLI, Figma Desktop version, and the currently installed official Figma skills. Do not record usernames or secrets.
- Inspect current Codex MCP configuration without changing it; capture sanitized names only (`figma-official`, `figwright`, `talk-to-figma` conceptually).
- Verify the existing official backend with one small control read/write/screenshot only if quota permits; link to the existing live Test A evidence rather than repeating calls unnecessarily.
- Create a run identifier and an evidence directory outside the committed repository for raw screenshots, metadata, logs, and exports. Commit only sanitized summaries and stable public links.

Exit gate:

- Existing official integration is still available and unchanged.
- Required runtimes and configuration locations are known.
- A dedicated Figma test file and isolated pages/frames are available or scheduled for creation.

## 5. Phase 1: Primary-source research

**Purpose:** understand actual architectures and limitations before deployment.

### Figwright research

Inspect the current README, releases, package metadata, MCP server, plugin source, `skills/figma-build`, setup scripts, security notes, screenshots, and issues mentioning Codex, Windows, Figma Desktop, WebSocket, fonts, screenshots, writing, or stability.

Record:

- supported Node versions and package entry points;
- Codex configuration shape and stdio lifecycle;
- local WebSocket topology and plugin handshake;
- read/write/batch/screenshot/export tools;
- hierarchy, Auto Layout, text/fonts, SVG/vector, connectors, selection, variables/styles;
- error recovery, atomicity, deterministic IDs, and tool-context overhead;
- plugin installation and update procedure.

### TalkToFigma research

Inspect the current README, recent commits/releases, MCP server, plugin, WebSocket server, channel protocol, setup scripts, Windows notes, helper prompts, tool inventory, and relevant issues.

Record:

- Bun version/install requirement and Windows behavior;
- MCP configuration and process lifecycle in Codex (Cursor-specific examples must be translated, not copied blindly);
- socket server startup, channel creation, reconnect behavior, and port conflicts;
- read/create/modify/text/style/move/resize/Auto Layout/export tools;
- batch support, component support, connector limitations, screenshot or equivalent visual feedback;
- error quality, object targeting, and recovery after plugin/server restarts.

Research output: a short source table in `docs/figma-backends.md` with repository URL, tested commit/version, date, and links to the inspected primary files. Distinguish documented behavior from claims requiring empirical verification.

Exit gate:

- Installation path for each backend is understood and auditable.
- No security-sensitive step is unresolved.
- A backend-specific verification checklist exists before any Figma mutation.

## 6. Phase 2: Deploy Figwright

**Purpose:** establish a complete, reproducible Codex-to-Figma path.

Tasks:

1. Validate the supported Node runtime and install/configure Figwright from the inspected upstream path.
2. Inspect and install/import the Figwright development plugin using the documented Windows procedure.
3. Add a distinct Codex MCP configuration entry without overwriting the official entry.
4. Start the MCP path, launch the plugin in the dedicated Figma file, and confirm both MCP and plugin handshakes.
5. Verify the active document/page and record the backend, file, page, and channel identifiers in sanitized notes.
6. Run the smoke tests in Phase 4 and capture raw metadata plus rendered evidence.

Lifecycle choice must be evidence-based: prefer Codex-managed MCP lifecycle if it is reliable; otherwise document a small explicit PowerShell launcher. Do not create a Windows service or startup task unless repeated tests show a clear operational benefit.

Figwright deployment status must include:

```text
installed / version / Codex connected / plugin connected
read verified / write verified / read-back verified
screenshot or export verified / batch verified (or unsupported)
```

## 7. Phase 3: Deploy TalkToFigma

**Purpose:** establish a complete, reproducible Codex-to-socket-to-plugin path on Windows.

Tasks:

1. Validate/install the required Bun runtime using an auditable method.
2. Install the server and plugin from the inspected upstream repository.
3. Configure a distinct Codex MCP entry; keep the official entry intact.
4. Start the WebSocket server, resolve the selected local port, and document the startup command and shutdown behavior.
5. Launch the plugin, establish the required channel, and verify reconnect behavior after a controlled server/plugin restart.
6. Verify the active document/page and run the smoke tests in Phase 4 with fresh evidence.

Do not copy WSL-only commands into the Windows procedure. Document quoting, path, firewall, and port-conflict behavior if encountered.

TalkToFigma deployment status must include:

```text
installed / version / Codex connected / socket connected / channel connected
read verified / write verified / read-back verified
export or equivalent visual verification / reconnect verified
```

## 8. Phase 4: Equivalent smoke-test protocol

Run independently for Official (control, if available), Figwright, and TalkToFigma in isolated clean areas. Use the same naming and geometry fixtures.

### Read checks

- identify the current Figma document and page;
- inspect current selection;
- inspect a named node and its parent/children;
- read position, size, visibility, and node type.

### Write checks

- create a frame and rectangle;
- create editable text;
- rename, move, resize, fill, and stroke nodes;
- establish parent-child relationships;
- read back every changed property.

### Structured-layout checks

- create Auto Layout or the closest supported structured layout;
- set padding and spacing;
- confirm hierarchy remains meaningful and independently selectable.

### Evidence checks

- obtain a fresh screenshot, SVG, PNG, or backend-native equivalent;
- compare rendered evidence with metadata;
- record unsupported operations as explicit failures, not blanks.

Each operation records: timestamp, backend/version, tool name, input summary, affected node IDs, result, evidence path/link, latency if useful, and recovery action after failure.

## 9. Phase 5: Scientific A/B tests

Use the existing scenarios and the same `figure_spec`; do not invent a product-UI example.

### Scenario A: Simple ML Pipeline

Build independently with Figwright and TalkToFigma:

```text
Material Data -> Preprocessing -> Feature Engineering
              -> Model Training -> SHAP -> Prediction
```

Acceptance conditions: native editable objects, six stable semantic stage IDs, independent labels and connectors, clear training-only SHAP semantics, left-to-right reading order, no fabricated quantitative chart, and no connector crossing a stage or label.

### Scenario D: Manual-edit preservation

After each backend creates the figure, manually move one node, replace one label, and slightly resize one stage in `panel-b/mechanism`. Ask the backend to modify only the SHAP/mechanism region. Verify that all nodes outside the change set retain text, bounds, visibility, and parent IDs, and that the manual move remains intact.

### Scenario E: Minimal correction

Create one overlap, one route-through-object connector, one clipped annotation, one inconsistent repeated width, and one flattened legend. For each backend: inspect, report five findings, fix only responsible nodes, capture fresh evidence, and verify neighboring content is unchanged.

### Optional Scenario B/C

Run Method-to-figure and reference reconstruction after A/D/E if both backends pass the basic gates. These broaden semantic compression and raster/vector coverage without hiding failures in the core workflow.

## 10. Evidence and scoring protocol

Every backend result is assessed structurally and visually. A successful tool response never substitutes for a fresh render or read-back.

### Required measurements

- object count and hierarchy depth;
- semantic-name coverage;
- editable text/frame/connector counts;
- number of calls and calls per stage/panel/node correction;
- batch size and safe batching behavior;
- read-back agreement with requested properties;
- screenshot/export availability and QA effort;
- deterministic repeat behavior;
- error specificity and recovery time;
- preservation of manual edits and minimal-delta correction.

### Weighted decision score

Start with the requested weighting and adjust only with a written rationale:

| Criterion | Weight |
| --- | ---: |
| Scientific figure construction | 20% |
| Editing precision | 15% |
| Visual QA | 15% |
| Structural QA | 10% |
| Manual-edit preservation | 10% |
| Minimal correction | 10% |
| Agent/tool ergonomics | 8% |
| Reliability | 7% |
| Community/maintenance | 3% |
| Other justified evidence | 2% |

Score each criterion from 0 to 5 and retain the evidence reference. Installation complexity receives 0% weight. Capability rows must distinguish `documented`, `empirically verified`, `failed`, and `not tested`.

## 11. Capability matrix and final decision

Create `tests/backend-comparison.md` with at least these rows:

```text
read document / hierarchy / selection / named node
create frame / text / shapes / SVG-vector
edit text / move-resize / fills-strokes / styles / variables
Auto Layout / connectors / batch writes / stable node IDs
screenshot / export / structural QA / visual QA
preserve manual edits / minimal correction
Starter-compatible / cloud dependency / local-only mode
```

For each cell record status plus evidence, not a prose guess. Classify each backend as exactly one of:

- Recommended default backend
- Recommended secondary backend
- Useful specialist backend
- Experimental
- Not recommended

The conclusion may be complementary, for example Figwright for structured writes, TalkToFigma for a specific local operation, and Official Figma for a capability only it exposes.

## 12. Integration decision gates

Do not modify `scientific-paper-figure/SKILL.md` until Phases 1-5 produce enough evidence to answer all of these:

1. Is a backend-neutral interface useful, or do the APIs differ too much for a thin adapter?
2. Should official Figma remain the primary path?
3. Should local backends be optional references, separate backend skills, or adapters?
4. Which backend, if any, is the default for scientific construction?
5. Which operations need a specialist backend?
6. Does official Figma remain a fallback/specialist backend despite Starter quota limits?

Preferred integration shape, unless evidence disproves it:

```text
scientific-paper-figure (figure_spec, topology, QA, preserve set)
                 |
        small backend capability contract
                 |
 official | Figwright | TalkToFigma
```

Keep backend mechanics out of the main Skill. If an adapter is justified, add only a concise reference or adapter skill containing capability selection, lifecycle, and known limitations; defer Plugin API details to upstream skills.

## 13. Risks and mitigations

| Risk | Mitigation | Stop/escalate condition |
| --- | --- | --- |
| Starter MCP quota blocks control evidence | Use local backends for new runs; link existing official Test A evidence | Do not claim official parity without fresh evidence |
| Plugin/socket appears connected but writes are stale | Mandatory read-back and fresh render after every write | Mark backend failed if live state cannot be proven |
| Local paths/secrets leak into public docs | Sanitize config examples and review diff before commit | Stop publication if credentials are found |
| WebSocket port or firewall instability | Record port, restart/reconnect test, and explicit launcher | Classify reliability based on repeated failures |
| Backend flattens or loses editability | Count node types and test independent edits | Do not adopt for native scientific output |
| Full regeneration destroys manual edits | Preserve-set assertions around Scenario D | Reject backend for edit mode if unavoidable |
| Visual QA is unavailable | Use export/plugin-native render equivalent | Mark visual QA as failed, not inferred |
| Upstream changes during evaluation | Pin tested commit/version in notes | Re-run smoke test if version changes |

## 14. Commit and reporting checkpoints

Use small intentional commits after each evidence boundary:

1. Add this plan.
2. Add sanitized research/deployment notes.
3. Add smoke-test evidence and backend comparison.
4. Add architecture decision and any minimal Skill/reference change.

Before each commit run link checks, ASCII check where applicable, `git diff --check`, and the existing Skill validator. Push only sanitized repository content. The final report must include deployment status, actual architecture, capability matrix, A/B results, recommendation, repository updates, test documentation, and remaining limitations.

## 15. Current starting state

- Existing `scientific-paper-figure` Skill and five scenarios are present.
- Official Figma Test A has live structure and screenshot evidence; Tests B-E are not yet live-passing because the Starter MCP limit was reached.
- This document is the next executable checkpoint. No new backend is considered deployed until the stop condition in Section 1 is met.
