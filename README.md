# skill-ue-graph-capture

An [Agent Skill](https://agentskills.io) for capturing supported Unreal Engine
Blueprint graphs with the on-demand `ue-graph-capture` CLI runner.

## What it does

- Verifies `uv`/`uvx` and resolves the exact `ue-graph-capture==0.1.0`
  package on demand without a persistent install.
- Prepares a temporary `.uproject` with `setup` and checks it with `doctor`.
- Discovers exact graph names with `list-graphs`.
- Captures EventGraph, Function Graph, and Macro Graph surfaces as PNGs.
- Keeps unsupported Timeline and Widget Designer surfaces on a separate
  fallback path.
- Requires PNG validation and discourages human interaction during capture.

## Prerequisites

Install `uv` using the package manager appropriate for the platform. On
Windows, the supported managed source is WinGet:

```powershell
winget install --id astral-sh.uv --exact --source winget `
  --accept-package-agreements --accept-source-agreements --disable-interactivity
```

The skill does not install `ue-graph-capture` persistently. It always uses:

```text
uvx --from "ue-graph-capture==0.1.0" ue-graph-capture <command> ...
```

## Example

```powershell
$project = Join-Path $env:TEMP 'ExtractedSolution\GP_Final_2026.uproject'
$output = Join-Path $env:TEMP 'goal-event-graph.png'
uvx --from "ue-graph-capture==0.1.0" `
  ue-graph-capture --version
uvx --from "ue-graph-capture==0.1.0" `
  ue-graph-capture setup --project $project
uvx --from "ue-graph-capture==0.1.0" `
  ue-graph-capture doctor --project $project
uvx --from "ue-graph-capture==0.1.0" `
  ue-graph-capture list-graphs --project $project `
  --asset /Game/FinalExam/StudentWork/BP_FE_Goal
uvx --from "ue-graph-capture==0.1.0" `
  ue-graph-capture capture --project $project `
  --asset /Game/FinalExam/StudentWork/BP_FE_Goal --graph EventGraph `
  --output $output
```

`setup` changes the project copy. Keep authoritative ZIPs and repository assets
outside the setup path, and place generated PNGs outside the project.

## License

MIT
