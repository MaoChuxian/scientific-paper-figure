# Figma Backend Deployment Evidence

Status: deployment and core local-backend evaluation complete on 2026-08-15. This file records observed facts only; it is not a recommendation and does not change `scientific-paper-figure/SKILL.md`.

## Phase 0: Windows baseline

Recorded on 2026-08-15 (Asia/Shanghai):

| Item | Observed value |
| --- | --- |
| Node.js | `v24.19.0` |
| npm | `11.17.0` |
| Bun | installed with WinGet, `1.3.14` |
| Figma Desktop | `126.7.10`, running |
| Codex MCP config | `config.toml` at the user Codex home |
| Existing official Figma integration | preserved; no installed skill or official entry was changed |
| Dedicated Figma file | `Scientific Paper Figure Skill Tests`, file key recorded in prior test results |
| Raw evidence directory | kept outside the repository under the current local evaluation run |

The Codex config now has distinct local entries named `figwright` and `talk_to_figma`. The official integration remains separate and unchanged. Machine-specific executable paths are intentionally omitted here.

## Phase 1: Primary-source revisions

### Figwright

- Repository: <https://github.com/awdr74100/figwright>
- Tested commit: `4eb134065cc63e9851bf5c4058b5facda8851646`
- Published MCP package inspected: `@figwright/mcp` `0.4.0`
- Node requirement from package metadata: `^20.19.0 || >=22.12.0`
- Architecture observed in README/source: MCP stdio server, local relay on `127.0.0.1:3055`, Figma plugin over WebSocket, leader/follower relay election.
- Plugin manifest: `packages/plugin/manifest.json`; plugin build produces `dist/code.js` and `dist/index.html`.
- Documented capability surface includes read/write tools, Auto Layout, styles, variables, components, batch writes, screenshots, PDF export, and video export. These remain documented until live plugin tests verify them.
- Security source inspected: `SECURITY.md`; relay uses loopback plus Host/Origin checks and writes no telemetry.

### TalkToFigma

- Repository: <https://github.com/grab/cursor-talk-to-figma-mcp>
- Tested commit: `ddd90f3a6d454ea0b2fc29f1b084f50fd062b880`
- Package metadata version: `0.3.5`
- Runtime: Bun; local machine installed Bun `1.3.14`.
- Architecture observed in source: stdio MCP server plus a separate Bun WebSocket server on port `3055`; the Figma plugin joins a named channel before regular commands are sent.
- Plugin manifest: `src/cursor_mcp_plugin/manifest.json`.
- Documented tools include document/selection/node reads, frame/rectangle/text creation, text batches, Auto Layout settings, styles, move/resize, parent changes, connectors, and image/PDF/SVG export. These remain documented until live plugin tests verify them.
- Windows note: the repository's WSL-specific `0.0.0.0` change was not applied; the default local hostname remains in use.

## Deployment progress

| Backend | Server install/build | Local transport check | Plugin | Live Figma read/write |
| --- | --- | --- | --- | --- |
| Figwright | installed from `@figwright/mcp 0.4.0`; plugin built from source | passed: e2e ping, metadata, document, and selection reads in the dedicated test file | connected to `Scientific Paper Figure Skill Tests` / `Page 1` | passed; native writes, read-back, screenshots, manual-edit preservation, minimal correction, and cleanup verified |
| TalkToFigma | Bun `1.3.14`; dependencies installed; `dist/server.js` built | passed: socket server and named channel `qyf00z2n` | connected to the dedicated test file during smoke run | passed for smoke and structured-layout sequence; full A/D/E comparison not rerun in this checkpoint |

The `/ping` result proves only that the Figwright relay process is alive. It does not count as deployment completion under the execution plan because no Figma plugin is connected and no native object has yet been read back or rendered.

Both upstreams default to local port `3055`. They must therefore be tested sequentially unless a backend is explicitly patched and its plugin manifest is updated; no such patch has been made.

## Codex configuration shape

The local entries use the official Codex `config.toml` MCP shape:

```toml
[mcp_servers.figwright]
command = "<absolute node executable>"
args = ["<absolute Figwright dist/index.mjs>"]

[mcp_servers.talk_to_figma]
command = "<absolute bun executable>"
args = ["<absolute TalkToFigma dist/server.js>"]
cwd = "<TalkToFigma repository>"
```

No credentials, tokens, or authentication data were added.

## Live Figwright evidence

The batch run used the dedicated file and completed 40 recorded steps. It created a top-level `backend/scenario-a/figwright` frame with six stage frames, six editable text labels, five connector fixture rectangles, and one annotation. Read-back confirmed stable IDs, parent-child relationships, positions, sizes, and edited text (`SHAP (edited locally)`). A deliberate overlap and clipped annotation were introduced, audited, corrected locally, and read back. PNG renders were saved before and after correction; the final deletion read-back returned `node: null`.

Raw logs and renders are retained outside the repository under `.evidence/backend-eval-20260815/` and are intentionally ignored by Git. The primary run log is `figwright-full-run.log`; rendered evidence includes `figwright-full/10-29.png`.

## Live TalkToFigma evidence

TalkToFigma completed the equivalent native smoke sequence in the same dedicated file: document/selection/design reads, frame and text creation, node read-back, Auto Layout direction/padding/spacing, fill/stroke, hug sizing, move/resize, PNG export, and deletion read-back. The plugin channel was `qyf00z2n`. Full scientific A/D/E parity was not claimed because this checkpoint focused on restoring Figwright and avoiding another manual plugin switch.
