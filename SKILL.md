---
name: ue-graph-capture
description: Use when capturing supported Unreal Engine Blueprint graphs as complete PNG images with the ue-graph-capture CLI. Do not use it for Timeline or Widget Designer surfaces unsupported by the installed version.
---

# ue-graph-capture

Use this skill for non-interactive capture of Blueprint EventGraph, Function
Graph, and Macro Graph images.

## Workflow

Use this runner for every `ue-graph-capture` command:

```text
uvx --from "ue-graph-capture==0.1.0" ue-graph-capture \
  <command> ...
```

1. Confirm that `uv` and `uvx` are available. The capture package is not a
   persistent PATH or pipx dependency.
2. Run the runner with `--version` and verify the actual version is exactly
   `ue-graph-capture 0.1.0`.
3. If the project is authoritative source material, copy or extract it into a
   temporary workspace before running commands that modify the project.
4. Run `uvx --from "ue-graph-capture==0.1.0" ue-graph-capture setup
   --project <project.uproject>` once for the
   temporary project copy. `setup` changes the project and installs the capture
   plugins.
5. Run `uvx --from "ue-graph-capture==0.1.0" ue-graph-capture doctor
   --project <project.uproject>` after setup.
6. Use `uvx --from "ue-graph-capture==0.1.0" ue-graph-capture \
   list-graphs --project <project.uproject> --asset /Game/...` when the exact
   graph name is unknown.
7. Run `uvx --from "ue-graph-capture==0.1.0" ue-graph-capture \
   capture --project <project.uproject> --asset /Game/... --graph <exact graph
   name> --output <output.png>`.
8. Validate that the output is a readable PNG and record the verified tool version
   and capture inputs when producing a manifest.

## Safety and boundaries

- If setup may change source material, copy or extract the project into a
  temporary workspace first. Do not run setup on an authoritative ZIP,
  repository asset, or source project.
- Prefer an output path outside the project so generated PNGs cannot become
  project source changes.
- Use Unreal asset package paths such as `/Game/Blueprints/BP_Player`, not
  filesystem paths.
- Pass the graph name exactly as reported by `list-graphs`; common supported
  types are `EventGraph`, Function Graph, and Macro Graph.
- `Timeline`, Widget Designer, Anim Blueprint, Material, Niagara, and other
  unsupported surfaces may use an explicitly bounded legacy fallback. Keep
  that fallback separate from supported graph capture.
- Prefer a capture flow that needs no human interaction; fail with diagnostics
  when the project, asset, graph, editor, or PNG is invalid.
