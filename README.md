# skill-ue-graph-capture

An [Agent Skill](https://agentskills.io) for capturing supported Unreal Engine
Blueprint graphs with the `ue-graph-capture` CLI.

## What it does

- Verifies the persistent CLI and its version.
- Prepares a temporary `.uproject` with `setup` and checks it with `doctor`.
- Discovers exact graph names with `list-graphs`.
- Captures EventGraph, Function Graph, and Macro Graph surfaces as PNGs.
- Keeps unsupported Timeline and Widget Designer surfaces on a separate
  fallback path.
- Requires PNG validation and discourages human interaction during capture.

## Installation

```powershell
pipx install ue-graph-capture==0.1.0
ue-graph-capture --version
```

## Example

```powershell
$project = 'C:\Temp\ExtractedSolution\GP_Final_2026.uproject'
ue-graph-capture setup --project $project
ue-graph-capture doctor --project $project
ue-graph-capture list-graphs --project $project --asset /Game/FinalExam/StudentWork/BP_FE_Goal
ue-graph-capture capture --project $project --asset /Game/FinalExam/StudentWork/BP_FE_Goal --graph EventGraph --output 'C:\Temp\goal-event-graph.png'
```

`setup` changes the project copy. Keep authoritative ZIPs and repository assets
outside the setup path, and place generated PNGs outside the project.

## License

MIT
