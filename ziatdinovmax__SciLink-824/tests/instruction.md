# Issue

**Title:** Building a structure no longer doubles every later line in the web UI

## Description

## What changes for users

After anyone asked SciLink for a DFT structure, the verbose output in the web UI became a mess for every later run on that server: each line appeared twice, once plain and once with a timestamp and logger name in front. It lasted until `scilink ui` was restarted, and it affected every session on the server, not only the one that built the structure.

With this change, building a structure leaves the output alone. Each line appears once.

## How it was checked

One server, a DFT structure request followed by an analysis run (the same pair of requests before and after):

| | DFT request | Analysis run afterwards |
|---|---|---|
| Before (0.0.83) | 66 duplicated timestamped lines | 372 duplicated timestamped lines |
| After | 0 | 0 |

The ordinary narration (structure views, sandbox notices, agent progress) still appears, once. The structure and VASP inputs were produced as before.

---

## Technical details

**Cause.** `StructurePipeline.__init__` called `logging.basicConfig(force=True, handlers=[StreamHandler(sys.stdout), StreamHandler(self.log_capture)])`. `force=True` removes every existing root handler, including the web runner's per-turn handler (`turn_log_handler`), and installs a stdout handler. Built during a turn, that handler writes to the server's stdout router, so from then on every root record reached the panel twice: through the turn handler (`%(message)s`) and through stdout (`%(asctime)s - %(levelname)s: %(name)s: %(message)s`). The capture `StringIO` also accumulated every record in the process for the server's lifetime, although only `build_structure` ever wrote it to disk.

**Fix** (`scilink/agents/sim_agents/structure_pipeline.py`):
- `__init__` configures no logging.
- `build_structure` (the standalone entry point, the only writer of `workflow_log.txt`) adds a capture handler to the root logger for the call and removes it in `finally`. It lowers the root level to INFO for the call if it was higher, and restores it.
- `generate_and_validate`, used by the simulation orchestrator tools and `SimulationPipeline`, does not touch logging.

**Tests.** `tests/test_structure_pipeline_logging.py`: construction leaves the root handlers and level unchanged; `build_structure` captures records from inside its run (not after) into `workflow_log.txt` and restores the root handlers and level; the root is restored when the run raises. Two of the three fail on `main`. The 202 non-live tests in the simulation test files pass. (The 21 errors in `tests/test_simulation_orchestrator.py` are on `main` too: it is a live script whose functions take a `model_name` argument pytest cannot supply.)

**Known follow-up, not in this PR.** `base_agent.py` (and so the MD simulation agent), `mlip_agent.py`, `packmol_agent.py` and `force_field_agent.py` each attach their own console `StreamHandler` to their logger when created. Created during a turn, it writes to the routed stderr, so those agents' lines appear twice while they run. Same class of bug, on the MD and force-field paths only. Two module-level `basicConfig` calls (`preprocess.py`, `metadata_converter.py`) run at server import, before the router exists, so they write to the real terminal and do not reach the panel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc5022878989`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.