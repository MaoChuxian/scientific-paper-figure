# Figma Backends

Use this reference before selecting or switching the Figma execution backend. Keep backend mechanics out of the `figure_spec`, scientific planning, findings, and correction logic.

## Selection

1. Respect the user's explicit backend choice.
2. Otherwise prefer Figwright 0.4.0 for local construction and correction.
3. Use TalkToFigma 0.3.5 as the secondary local backend.
4. Use Official Figma as the control, fallback, or specialist backend.
5. Before writing, confirm that the selected backend can satisfy every required capability. Probe an unverified critical capability on a disposable object or select a suitable backend; do not assume parity.

Only when Official Figma is selected, load and follow its skills when triggered: `figma-use` before `use_figma`, `figma-create-new-file` before official file creation, and `figma-generate-design` for composed views. Reuse an existing user file when possible.

## Normal Figwright Operation

Use the Figwright MCP server already declared in the Codex MCP configuration. In the validated setup, Codex launches and connects `npx -y @figwright/mcp@0.4.0`; the user opens Figma Desktop, the target file, and the Figwright plugin. The agent then confirms the routed file and Page with a lightweight end-to-end read and proceeds. Ordinary figure work does not require installing Figwright, starting another server, restarting the backend, diagnosing processes, or repeating deployment smoke tests.

The normal path is:

1. Use the current configured MCP session; do not start a second Figwright server or client.
2. Confirm the target Figma file and Page. Activate the target Page before Page-sensitive operations.
3. Finish planning before the first write.
4. Apply bounded logical batches.
5. Read back affected nodes and necessary neighbors.
6. Render according to visual risk, with complete fresh visual coverage before final approval.

A transport ping confirms connectivity, not design correctness. Run deployment-style create/read/render/delete probes only after installation, reconfiguration, or a real capability failure, not at the start of every drawing task.

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

## Figwright Write Protocol

### Bounded Batches

Use `batch` for independent operations within one logical transaction: a panel or region, a connector family, coordinated repeated changes, or an idempotent global text/style patch. Prefer bounded region batches over one object per call and over one whole-figure batch.

Do not combine an entire complex figure, many font-loading text creations, complex SVG imports, and styling into one transaction when the observed latency can cross the caller timeout. Batch size is a latency and recoverability boundary, not a fixed node-count quota.

### Write/Batch Timeout

A Figwright write timeout means commit state is unknown. During the validated benchmark, a large mixed create batch continued inside the plugin after the caller timed out. Therefore timeout is neither definite failure nor proof of rollback.

On a write timeout:

1. Stop all further writes.
2. Do not retry the same batch and do not restart the backend first.
3. Read the intended parent or region from live Figma state.
4. Match semantic name, node type, geometry, and content to classify operations as completed, missing, duplicate, partial, or uncertain.
5. Remove only exact duplicates when the evidence is unambiguous.
6. Apply only the missing minimal delta, then perform targeted readback.

Blind replay is unsafe because the timed-out operation may still commit after the caller returns, producing stacked native nodes and incorrect editability even when the render looks unchanged.

## Figwright Render Protocol

### Normal Render

Screenshot, render, and export operations are Page-sensitive. Before each operation:

1. Identify the Page that owns every target node.
2. Confirm the Figwright plugin's current Page.
3. If they differ, activate the target Page first.
4. Render the smallest affected region that supplies the required evidence, followed by a larger region or whole figure only when required.

The current benchmark repeatedly observed long stalls for off-current-Page nodes, including a small card, followed by fast small and whole-root renders after Page activation. Treat Page mismatch as a high-priority diagnostic, not as proof that the image is too large or the exporter is broken. This is a verified operational rule for the validated Figwright 0.4.0 setup, not a claim that every Figwright installation must fail off-Page.

### Render Stall and Retry Budget

After the first render stall, stop issuing render calls. A retry is allowed only after changing state or obtaining new information:

1. Check target-node ownership and the plugin's current Page.
2. If the Page is wrong, activate it and try one small regional render.
3. If the Page was already correct, or that small probe still fails, enter backend recovery. Do not queue more renders.
4. After recovery, confirm end-to-end backend, file, and Page state; run one small regional probe; continue to the required large or whole render only if the probe succeeds.

Do not use `timeout -> retry -> retry`. Do not make the largest root the first recovery probe.

Write timeouts and render stalls have different first actions: reconcile live document state after a write timeout; check Page state and probe rendering after a render stall. Do not restart the backend merely because a write call timed out.

## Figwright Backend Recovery and Lifecycle

Enter backend recovery only when an end-to-end Figwright call clearly fails, or when the Page is correct and a small render probe still fails. Keep Figma Desktop and the Figwright plugin open. Restore the configured MCP transport/client, confirm the same file and Page, run a small probe, and then resume the interrupted task. Restarting Figma, the plugin, Codex, or the computer is an escalation step, not the default response.

Figwright already provides leader/follower election for its local relay. Normal tasks use the current MCP session and leave election and lifecycle management to Figwright and Codex. Do not start temporary clients for ordinary tool calls, launch a second server, or teach the scientific Skill to select or replace leaders manually. During recovery, rebind only when the existing transport has closed or is demonstrably unusable, and validate the new route end to end.

Leader handoff does not guarantee that an already-open Codex MCP transport will hot-rebind. In the validated setup, stopping the leader closed the active tool transport even though another Figwright process acquired port 3055. If the host cannot create a fresh configured client in the current session, stop Figwright tool calls and report that a host/session rebind is required. Do not compensate by launching ad-hoc clients or additional backend processes.

The validated recovery kept the plugin open, stopped the exact listening backend owner, allowed another Figwright process to take ownership, and then confirmed the configured route. This is recovery evidence, not a normal startup recipe.

### Helper Script Decision

Do not add `figwright-start.ps1`, `figwright-stop.ps1`, `figwright-health.ps1`, `figwright-reset.ps1`, or `figwright-diagnose.ps1` now. A start script would duplicate the MCP-managed server; generic health/reset scripts would not resolve Page mismatch or unknown write commit state; current recovery is infrequent enough that a process manager would add more lifecycle ambiguity than value.

Reconsider a read-only diagnose helper only if multiple devices repeatedly reproduce stale Figwright processes, recovery regularly requires manual PID/port inspection, Codex cannot release obsolete MCP processes reliably, or process diagnosis becomes a stable major cost. Reconsider start/stop/reset automation only after a single authoritative process owner and safe handoff contract are defined and empirically verified.

## Operational Decision Tree

```text
NORMAL STARTUP
  configured MCP + open Figma/plugin -> confirm file/Page -> work

NORMAL WRITE
  bounded logical batch -> targeted live readback

WRITE TIMEOUT
  STOP writes -> inspect live region -> reconcile commit state -> missing delta only

NORMAL RENDER
  confirm target Page is current -> affected region -> required large/whole coverage

RENDER STALL
  STOP render queue -> check/activate Page -> one small probe
  -> if still failing: backend recovery -> confirm file/Page -> small probe -> required render
```

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
