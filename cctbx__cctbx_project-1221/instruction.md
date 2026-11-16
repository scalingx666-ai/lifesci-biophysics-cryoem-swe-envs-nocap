# Issue

**Title:** clashscore2: hydrogenate and process ensembles per model, not per file

## Description

`clashscore2` processed and hydrogenated multi-model files as one combined
hierarchy. Ensemble models are spatially superimposed, so making restraints
across the whole file is quadratic in the model count: a 21-model NMR entry was
measured peaking over 13 GB. Placement and flip optimization are per-model
operations anyway (the Optimizer already iterates MODEL records independently),
so this hydrogenates each model separately and recombines, then runs the single
interpretation-only re-process, which is linear, on the combined model. It also
processes restraints once per ensemble and caches the probe2 phil parse.

On 1D3Z (10 models, 12,310 atoms), peak memory with hydrogens regenerated goes
from 1,980 MB to 278 MB, and the kept-hydrogens run from 29.5 s to 10.8 s.

It is also a correctness fix. The hydrogen-deletion set the Optimizer reports
after a whole-ensemble run describes a single model, and applying it across the
combined hierarchy removes the wrong atoms: on 1D3Z the whole-file path returns
6,543 of 12,310 atoms and 523 of 6,290 hydrogens. All ten per-model clashscores
change as a result. Afterwards, regenerating hydrogens reproduces the
deposited-hydrogen clashscores exactly in all ten models, where before it
disagreed in all ten.

Single-model probe2 output is byte-identical, and tst_add_hydrogen_1 through
_11 pass.

`mmtbx/reduce/Optimizers.py` still resets its deletion set per model, so direct
multi-model reduce2 runs keep the old behaviour and want a separate fix.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b7f00f116a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.