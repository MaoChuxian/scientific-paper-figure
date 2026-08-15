# Device Migration Guide

This guide separates the portable repository from machine-local Figma tooling and
raw evidence. A normal Git clone restores the Skill and maintained documentation;
it does not restore Codex user configuration, Figma development plugins, upstream
research clones, or ignored evidence.

## Portable and local state

| State | Transfer method | Required |
| --- | --- | --- |
| Skill source, references, docs, and test summaries | Git clone | Yes |
| Figma design files | Sign in to the same Figma account and confirm cloud sync | Yes |
| Codex MCP entries | Recreate in the new user's `config.toml` | Yes for local backends |
| Global Skill installation | Recreate the directory link or copy | Yes |
| Figwright and TalkToFigma plugins | Reinstall/import in Figma Desktop | Backend dependent |
| `.research-repos/` | Re-clone pinned upstream revisions | Optional |
| `.evidence/` | Copy privately or archive separately | Optional but useful for audit |
| `.tmp/` | Do not transfer | No |

The ignored local directories on the original workstation are approximately
539 MB for upstream clones, 20 MB for evidence, and less than 1 MB for temporary
files. They are not signs of an incomplete Git clone.

## Before leaving the old device

1. Confirm that Figma has synchronized the relevant files to the intended account.
   The current test file is `Scientific Paper Figure Skill Tests`; the active
   Scenario C reconstruction is in `figure b`, `Page 1`.
2. Review and publish repository state:

   ```powershell
   git status --short --branch
   git log --oneline --decorate -6
   git push origin main
   ```

3. If raw evidence must be retained, copy `.evidence/` through a private transfer
   channel. It may contain large renders and local paths; do not add it to Git.
4. Record any credentials or private configuration through a password manager.
   Do not copy the complete Codex `config.toml` into this public repository.

## Bootstrap the new device

### 1. Install prerequisites

- Git and GitHub CLI.
- Codex Desktop or another Codex environment with Skills and MCP support.
- Figma Desktop, signed in to the account that owns the design files.
- Node.js 24 LTS or newer for source builds. The published Figwright MCP supports
  Node.js `20.19+` or `22.12+`, but Node 24 matches the validated workstation.
- Bun when TalkToFigma is required.

On Windows, confirm the executables before writing MCP configuration:

```powershell
git --version
gh --version
node --version
npm --version
bun --version
Get-Command node, npx, bun, bunx | Select-Object Name, Source
```

### 2. Clone the repository

```powershell
git clone https://github.com/MaoChuxian/scientific-paper-figure.git
Set-Location scientific-paper-figure
git status --short --branch
```

Use Git rather than copying the old `.git/` directory. The public remote and
default branch are `MaoChuxian/scientific-paper-figure` and `main`.

### 3. Install the Skill for development

The repository copy is authoritative. A directory junction keeps the global Skill
installation synchronized with edits made in the clone:

```powershell
$repoRoot = (Resolve-Path '.').Path
$codexSkills = Join-Path $env:USERPROFILE '.codex\skills'
$skillTarget = Join-Path $codexSkills 'scientific-paper-figure'
New-Item -ItemType Directory -Force -Path $codexSkills | Out-Null
New-Item -ItemType Junction -Path $skillTarget -Target (Join-Path $repoRoot 'scientific-paper-figure')
```

Run the last command only when `$skillTarget` does not already exist. If it does,
compare or back it up first rather than overwriting it. Restart Codex after adding
or replacing a Skill installation.

### 4. Configure Figwright 0.4.0

Figwright is the preferred local backend. Add a pinned server entry to the new
user's `%USERPROFILE%\.codex\config.toml`, using the absolute `npx.cmd` path
reported by `Get-Command` when Codex cannot resolve `PATH`:

```toml
[mcp_servers.figwright]
command = "C:\\Program Files\\nodejs\\npx.cmd"
args = ["-y", "@figwright/mcp@0.4.0"]
```

This MCP entry is the normal Figwright server lifecycle owner. Codex starts and
connects it as configured; do not run a separate `figwright-start.ps1`, launch a
second `npx @figwright/mcp` process, or add another process manager before normal
drawing work. The user only needs Figma Desktop, the target file, and the matching
Figwright plugin open. Confirm the routed file and active Page with the configured
MCP session, then proceed.

Download the matching plugin archive from the Figwright GitHub release. In Figma
Desktop use **Menu -> Plugins -> Development -> Import plugin from manifest...**,
select its `manifest.json`, then open **Plugins -> Development -> Figwright**.

For first installation, reconfiguration, or an actual capability failure, verify
in this order: end-to-end `ping`, active file/page read-back, one disposable
native-object write/read/delete, one small regional render, and then the required
whole-figure render. A relay ping alone is not deployment evidence. Do not repeat
this deployment smoke test before every ordinary figure task.

### 5. Configure TalkToFigma 0.3.5 when needed

TalkToFigma is secondary and shares local port `3055` with Figwright, so do not run
both socket/relay owners concurrently.

```toml
[mcp_servers.talk_to_figma]
command = "C:\\path\\reported\\by\\Get-Command\\bunx.exe"
args = ["cursor-talk-to-figma-mcp@0.3.5"]
```

Start its socket service separately:

```powershell
bunx cursor-talk-to-figma-socket
```

Install the community plugin, or link the local
`src/cursor_mcp_plugin/manifest.json` from a pinned source clone. Join the channel
shown by the plugin before issuing document commands. Large-root PNG export timed
out repeatedly in the validated environment; use smaller regional renders or a
different verified evidence path instead of only increasing the MCP timeout.

### 6. Recreate optional upstream clones

Only clone these when continuing backend research or building plugins from source:

```powershell
New-Item -ItemType Directory -Force '.research-repos' | Out-Null
git clone https://github.com/awdr74100/figwright.git .research-repos/figwright
git -C .research-repos/figwright checkout 4eb134065cc63e9851bf5c4058b5facda8851646
git clone https://github.com/grab/cursor-talk-to-figma-mcp.git .research-repos/cursor-talk-to-figma-mcp
git -C .research-repos/cursor-talk-to-figma-mcp checkout ddd90f3a6d454ea0b2fc29f1b084f50fd062b880
```

Figwright source builds use `pnpm 11.21.0`; TalkToFigma uses Bun. Follow each
upstream repository's lockfile and README rather than committing generated
dependencies into this repository.

## Historical handoff checkpoint

As of 2026-08-15, Scenario C is bound to Figwright 0.4.0 in `figure b` / `Page 1`,
root `3:2` (`reconstruction/model-development-workflow`). The generic compactness
rules are committed in `4fa1aa8`.

A minimal compactness mutation has been applied in Figma and passed structural
read-back:

- root `3:2`: `1600 x 1040`;
- Step 2 `3:4`: `y=270`, `1490 x 300`;
- Step 2 prediction `4:83`: `y=230`, `640 x 55`;
- Step 2 merge group `4:56`: `y=165`, original line geometry preserved;
- Step 3 `3:5`: moved to `y=580`;
- Step 4 `3:6`: moved to `y=835`;
- panel gaps remain 10 px and typography, labels, palette, and topology were not
  intentionally changed.

At that checkpoint, final visual approval was still pending. Figwright structure
reads and atomic writes worked, but both `get_screenshot` and `save_screenshots`
stalled even for a small title node after a backend-only restart. This is retained
as historical device evidence, not a normal startup instruction or a universal
Figwright behavior. Current operational handling is defined in
`scientific-paper-figure/references/figma-backends.md`: check target Page state
first, use one small probe, and enter bounded backend recovery only when the Page
is correct and the probe still fails.

## Migration acceptance checklist

- `git status` is clean and `main` matches `origin/main`.
- Codex detects `$scientific-paper-figure` from the linked Skill directory.
- The selected MCP backend reports the expected pinned version.
- Figma opens the correct cloud file and page.
- Live hierarchy read-back returns the expected semantic root.
- A disposable native object can be created, read, rendered, and deleted.
- Scenario C receives fresh regional and whole-figure rendered evidence before QA
  is closed.
