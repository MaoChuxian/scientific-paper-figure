# Figma Backends

Use this reference before selecting or switching the Figma execution backend. Keep backend mechanics out of the `figure_spec`, scientific planning, findings, and correction logic.

## Selection

1. Respect the user's explicit backend choice.
2. Otherwise prefer Figwright 0.4.0 for local construction and correction.
3. Use TalkToFigma 0.3.5 as the secondary local backend.
4. Use Official Figma as the control, fallback, or specialist backend.
5. Before writing, confirm that the selected backend can satisfy every required capability. Probe an unverified critical capability on a disposable object or select a suitable backend; do not assume parity.

Only when Official Figma is selected, load and follow its skills when triggered: `figma-use` before `use_figma`, `figma-create-new-file` before official file creation, and `figma-generate-design` for composed views. Reuse an existing user file when possible.

## Capability Contract

Bind backend-specific operations at the execution boundary to these capabilities:

- inspect document, page, node, selection, and hierarchy;
- create native editable objects and edit existing native objects;
- manage parent/child structure;
- apply structured layout;
- create and edit connectors;
- read back affected objects and relevant neighbors;
- capture a fresh regional render and a fresh whole-figure render;
- export the requested format when export is part of the request.

A successful write or transport ping is not QA evidence. Return stable affected node IDs where the backend supports them and maintain the same change set, preserve set, finding contract, and acceptance conditions across backends.

## Evidence Methods and Limits

- **Figwright 0.4.0**: native creation/editing, hierarchy read-back, basic structured layout, stable IDs, manual-edit preservation, minimal correction, and fresh PNG rendering were empirically verified. Direct connector authoring and several documented advanced capabilities were not isolated in the evaluation; probe them when critical.
- **TalkToFigma 0.3.5**: native creation/editing, hierarchy read-back, structured layout, manual-edit preservation, minimal correction, reconnect, and small PNG export were empirically verified. Large-root PNG export repeatedly timed out at the backend/plugin layer after about 30 seconds, including with a 120-second MCP client timeout. Prefer smaller regional renders or another verified evidence path; increasing only the client timeout is not a remedy. Direct connector authoring was not isolated in the evaluation.
- **Official Figma**: retain as control/fallback/specialist. Starter-plan MCP quota blocked new control runs, so do not infer untested parity from official documentation.

Rendered evidence may come from a backend screenshot, a successful regional export, or another fresh view of the live figure. It must cover both corrected regions and the whole figure; no specific screenshot/export tool is required.

### Figwright Render Recovery

Before every Figwright render, confirm that the Figma plugin is on the Page containing the requested nodes and navigate to that Page when necessary. A render of a small off-current-Page node can stall just as long as a whole root; node size alone is not a reliable predictor.

If Figwright rendering stalls after previously working, stop queued render calls and classify the event as backend reliability. Keep the Figma plugin open and restart the local Figwright backend before asking the user to restart the plugin. Expect the old MCP transport to close when its backend process stops, so bind a fresh MCP client before retesting. Verify recovery in this order: end-to-end ping routed to the same file/page, one small regional render, then the required whole-figure render. Do not make another large render the first recovery probe.

Bound retries. After one stalled render, check Page state before repeating. If the target Page is wrong, activate it and retry the small probe once. If the target Page is already correct and the probe still stalls, perform backend recovery; do not queue additional renders.

This sequence recovered Figwright 0.4.0 empirically: a follower replaced the stopped local leader while the plugin stayed open, and a fresh MCP client restored end-to-end ping and regional PNG rendering. Treat backend restart as a recovery attempt, not evidence by itself; retain the fresh structure-plus-render QA gate.

### Figwright Batch Safety

Use `batch` for bounded logical transactions and idempotent global patches. Large mixed create batches can cross the caller timeout while continuing inside the plugin. Treat timeout as unknown commit status, not atomic rollback evidence: inspect the target parent, match semantic name + type + intended geometry/content, remove exact duplicates when present, and issue only the missing delta. Prefer several bounded region batches over one whole-figure batch. Do not retry a create batch until live state is reconciled.

## Transaction Stickiness and Failover

Keep one backend bound from the first write through read-back and correction acceptance. Never silently mix write backends within a construction, edit, or correction transaction.

Before failover:

1. Stop writes.
2. Inspect the current live Figma document, page, hierarchy, and affected nodes.
3. Recover the intended change set and preserve set from live state and working notes.
4. Identify completed, missing, duplicated, and partial objects and any uncertain side effects.
5. Bind the fallback backend only after defining the remaining operation and its acceptance evidence.

Do not replay the entire transaction blindly. Continue with the smallest responsible change and revalidate preserved regions.

## Failure Attribution

Classify a failure before changing the Skill or scientific workflow:

- source ambiguity;
- scientific planning;
- figure specification;
- publication/style;
- QA/correction;
- backend capability;
- backend reliability;
- tool limitation.

Correct the owning layer. Do not weaken scientific content, topology, publication rules, preserve-set behavior, or QA gates to hide a backend capability, reliability, or tool failure.
