# Issue

**Title:** Decouple elastic bond lower cutoff from the decay shift

## Description

## Summary

`ApplyRubberBand` fed the `-el`/`lower_bound` value into the decay function as its shift parameter, rather than using it as a plain distance cutoff (as `-eu`/`upper_bound` already is). This splits it into two independent parameters:

- **`lower_bound` (`-el`)** is now a hard distance cutoff: bonds shorter than it are removed, mirroring `upper_bound`.
- **`decay_shift` (new `-es` flag)** carries the old shift behaviour for the decay function, independent of the cutoff.

Defaults are unchanged (both were effectively 0 before), so existing runs behave the same unless `-el` is set explicitly.

## Changes

- `vermouth/processors/apply_rubber_band.py` — `compute_force_constants`, `apply_rubber_band` and `ApplyRubberBand` gain a `decay_shift` parameter (default `0`). The decay function is shifted by `decay_shift` instead of `lower_bound`, and `lower_bound` is applied as `constants[distance_matrix < lower_bound] = 0`. Docstrings updated.
- `vermouth/bin/martinize2.py` — new `-es` flag (`dest="rb_decay_shift"`, float, default `0`) in the "Protein elastic network" group, passed through to `ApplyRubberBand`. The `-el` help text is corrected: it said `F = Fc if rij < lo`, now `F = 0 if rij < lo`.
- `vermouth/tests/test_apply_rubber_band.py` — `decay_shift` added to the `test_compute_force_constants` parametrization, plus two new cases covering the hard lower cutoff and the shift-without-cutoff behaviour.
- `doc/source/tutorials/elastic_networks.rst`, `doc/source/martinize2_workflow.rst` — document `-es`, describe `-el`/`-eu` as hard cutoffs, and add a migration note (commands relying on the old `-el` shift behaviour should use `-es`).
- `doc/source/tutorials/elastic_examples.png` — regenerated from `compute_force_constants`: the shifted example now uses `-es`, and a new example shows a hard lower cutoff.

## Testing

- `test_apply_rubber_band.py`: 44 passed.
- Full unit suite: 2584 passed. Two failures (`pdb/test_cif.py::test_equal_output`, `test_name_moltype.py::test_martinize2_moltypes`) are pre-existing — they fail identically on unmodified master.
- End-to-end on `EN_chain/aa.pdb`: with `-ea 0.5 -ep 2 -es 0.8`, force constants near 0.8 nm move toward the base constant (bond at 0.814 nm: 359 → 500). With `-el 0.6`, the network is truncated at 0.609 nm. The existing EN integration tests use only defaults, so their output is unchanged.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cfcad6ecc231`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.