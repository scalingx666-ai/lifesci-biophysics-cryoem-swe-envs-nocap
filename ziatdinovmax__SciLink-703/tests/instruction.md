# Issue

**Title:** Remove EXAFS simulation skill and its orchestrator integration

## Description

Drops the exafs_simulation skill bundle (exafs_workflow, generate_feff_input, feff_tools), the run_exafs_workflow orchestrator tool, its MCP gating and async entries, the FEFF_DIR/FEFF_BIN sandbox env allowlist, and EXAFS mentions in the route_simulation and run_mlip_simulation descriptions. General MLIP wiring (ASE runner formats, pipeline MLIP scale) is kept.

The EXAFS simulation workflow has been moved to a separate repo (https://github.com/HeteroFAM/MLMD-EXAFS/tree/main) that includes a SciLink plug-in custom tool.

To use, register the plug-in in tool/mlmd_exafs_tool.py, which follows SciLink's tool_schemas + create_tool_functions contract:
```
scilink simulate --tools MLMD-EXAFS/tool/mlmd_exafs_tool.py
```
It surfaces the same ten stages as SciLink custom tools, wired to the session's active structure file and results directory. See [tool/README.md](https://github.com/HeteroFAM/MLMD-EXAFS/blob/main/tool/README.md) for details.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `90453e548ca4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.