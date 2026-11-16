# Issue

**Title:** Skip the structure analysis when a phase-stability check is disabled

## Description

## Problem

`Phase.check_if_melted` and `Phase.check_if_solidfied` always compute the solid fraction with pyscal3 (`helpers.find_solid_fraction`) and only then compare it with the tolerance. With `tolerance.solid_fraction: 0` (melt detection off) or `liquid_fraction: 1` (solidification detection off), that comparison can never trigger, yet the analysis still runs.

That would only cost time, except that **pyscal3 segfaults in `find.neighbors` on some LAMMPS triclinic dumps**. In a `ts` run the check runs after every backward sweep's middle equilibration, on the freshly dumped `traj.temp.dat`. A segfault there kills the whole job, with exit 139 and no Python traceback, over a check that was switched off.

## Where it showed up

Mg2Si (antifluorite) as a 3993-atom 60° primitive cell, GRACE potential, `mode: ts`, `tolerance.solid_fraction: 0` (compounds: the solid-fraction criterion does not apply to them):

- Three separate runs of this cell segfaulted at exactly the same step: the start of a backward sweep, after its middle equilibration.
  - They used two numeric precisions (TF32 / FP32) and two temperature windows (500→1400 K, 900→1400 K).
  - The crash came in iteration 3 in two runs and iteration 1 in the third.
- `pyscal3.System(traj.temp.dat).find.neighbors(method="cutoff", cutoff=5.0)` reproduces the segfault on CPU from the saved dumps.
- What passes, which suggests a pyscal3 issue with how certain LAMMPS triclinic dump bounds are read, not with the box type:
  - an Al2Ca dump in the same kind of triclinic box;
  - synthetic disordered Mg2Si cells (triclinic and cubic);
  - the crash snapshots round-tripped through ASE.

## Change

Each check returns immediately when its tolerance disables it:

| check | skipped when |
|---|---|
| `check_if_melted` | `solid_fraction <= 0` |
| `check_if_solidfied` | `liquid_fraction >= 1` |

- Behaviour with the check enabled is unchanged.
- `liquid.py`'s melting-cycle check, which needs the analysis, is untouched.
- `mode: melting_temperature` force-enables detection, so it is unaffected.

## Tests

`tests/test_disabled_phase_checks.py`:
- The disabled checks must not call `find_solid_fraction`.
- The enabled checks still do.
- Run against this branch: 4 passed. On the unpatched code the disabled-check test fails, as it should.

The pyscal3 crash itself is worth a separate report upstream (it is reproducible from the saved dump).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_01RebMnzKCNqF5XyBrSLhBnU

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `196ace6f51f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.