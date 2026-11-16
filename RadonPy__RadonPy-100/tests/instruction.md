# Issue

**Title:** Derive the LAMMPS deformation variables when the stage thermo hides them

## Description

## What this fixes

The LAMMPS elongation analysis (`Deformation_Analysis`) now works. Before this, it failed on every LAMMPS run of `Deformation.exec_deform`, although the simulation itself finished.

## Cause

`exec_deform` adds `v_EngStrain` and `v_GamNsurf` to the deformation stage with its own `thermo_style` line. `LAMMPS.make_input` then writes the per-stage `# Update thermo_style` block after it, which replaces that line. The deformation log therefore has no `v_EngStrain` column, and `log_extraction` raises an `UnboundLocalError` because `deform_log` is never assigned. The same failure stops `density_plot` and `nemorder_plot`.

## Change

`log_extraction` locates the deformation run from the `fix DEF` line of the input. When the column is missing, it derives both variables from that run's box length and pressure tensor, with the definitions the input uses:

- `v_EngStrain = (L - L0) / L0`
- `v_GamNsurf = L * (Pxx - (Pyy + Pzz) / 2) * 101325 / 1e10 * 1e3`

Logs that still have the columns are read as before. The usage is unchanged:

```python
from radonpy.sim.preset import elong

analysis = elong.Deformation_Analysis(work_dir='elong', axis='x', deform_strain=0.5)
whole_log, deform_log = analysis.log_extraction()   # deform_log['v_EngStrain'], ['v_GamNsurf']
analysis.density_plot()
analysis.nemorder_plot()
```

## Test plan

- [x] Re-analyzed LAMMPS elongation runs (PMMA, 50% strain): `log_extraction` returns the deformation stage with strain from 0 to 0.5, and `density_plot` and `nemorder_plot` complete.
- [x] Existing tests (`pytest -c tests/pytest.ini tests`, without MD engines) pass.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8834b9071ae9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.