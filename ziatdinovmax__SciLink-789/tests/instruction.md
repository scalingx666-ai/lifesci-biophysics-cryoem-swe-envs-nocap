# Issue

**Title:** simulate: make help and agent-facing scope engine-general

## Description

.

The `scilink simulate` help still described a VASP-only, prep-only tool. The text has moved since the issue was filed (from `cli/simulate.py` into `SimulateAdapter` in `cli/shell/modes.py` with the terminal-shell refactor), and it was more out of date than the issue says: the orchestrator now covers eight engines and executes locally through `run_simulation`.

## Changes

| where | before | after |
|---|---|---|
| `SimulateAdapter.description` | "Interactive VASP DFT input prep" | "Interactive simulation setup, execution, and analysis" |
| `SimulateAdapter.epilog` scope | "VASP DFT input prep only (LAMMPS support is planned follow-up)" / "Local prep only" | Engines: VASP, Quantum ESPRESSO, CP2K; NWChem, ORCA; LAMMPS, GROMACS, ASE with ML potentials. Runs locally where the engine skill provides a run command (LAMMPS, ASE) or the user gives one; otherwise prepares inputs to run elsewhere. |
| `SimulateAdapter.epilog` examples | VASP only | + a LAMMPS example |
| `cli/simulate.py` docstring | "generate VASP inputs" | engine-general |
| `SimulationOrchestratorAgent` docstring + module docstring | "coordinating VASP DFT input preparation" / "Periodic DFT (VASP, QE) is fully dispatched" | engine-general, incl. molecular QC |
| orchestrator **system prompt** | "input-preparation assistant"; "without an HPC connection, prep is local only"; no `molecular_qc` dispatch line | prepares, runs and analyzes; local execution via `run_simulation` when a run command exists; `molecular_qc` (nwchem, orca) and `cp2k` dispatch lines |
| `run_simulation` schema | scale `periodic_dft \| molecular_dynamics`, engine `vasp \| lammps` | all three scales; engine examples across them |
| `scilink --help` | "Simulation agents for MD, DFT, LAMMPS, VASP workflows" | "…for DFT, quantum chemistry, and MD workflows" |
| meta agent `delegate_to_simulation` | "generates and runs engine inputs (VASP, LAMMPS)" | adds molecular QC and the engine list |

The system prompt change matters beyond wording: it is what the agent reads, and it previously told the agent it could only prepare inputs, which contradicted both `run_simulation` and the new help.

The engine list is the set of skill bundles under `skills/periodic_dft`, `skills/molecular_qc` and `skills/molecular_dynamics` (MLIPs are potentials run through ASE or LAMMPS, not an engine). Only the LAMMPS and ASE skills provide a `default_run_command`, so other engines run locally only with a user-given command. HPC submission (`submit_simulation_job`) is not mentioned in the help, because the CLI has no command to set up the HPC connection it needs.

## Follow-ups (not in this PR)

- Build the help's engine list from the skill registry so it can't go stale again.
- With an HPC connection attached outside a SLURM allocation, `run_simulation` reports "no run_command available (no engine binary on PATH…)" when the job was actually deferred to HPC submission. That's a misleading message, worth its own issue.

## Testing

- `scilink simulate --help` renders the new description, scope, and examples.
- `get_system_prompt` renders in all three modes with the new text.
- Orchestrator / meta / router / shell / CLI tests: 274 passed. The 6 failures (`test_instruments_and_simulators.py::TestRunExperiment`) and 18 setup errors (script-style tests expecting a `model_name` fixture) are identical on `main`.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e62ab5617c00`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.