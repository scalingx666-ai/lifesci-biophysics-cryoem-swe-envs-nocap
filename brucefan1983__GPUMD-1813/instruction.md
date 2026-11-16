# Issue

**Title:** Write each row of the NEP training report once for both the standard output and loss.out

## Description

## Summary

`Fitness::report_error` in `src/main_nep/fitness.cu` wrote every row of the training report twice, once with `printf` to the standard output and once with `fprintf` to `loss.out`. The NEP, charge-model and TNEP branches each held the format string and the argument list in two identical copies, and a column added to one copy had to be added to the other by hand.

This PR replaces the duplicated calls with one lambda, `write_row`, that takes a `FILE*` and is called once with `stdout` and once with `fid_loss_out`. It holds the three model branches and writes the energy-difference columns within the branch of the potential models, which are the only models that accept `lambda_d`. The format strings and the order of the columns are unchanged.

## Testing

No test is added, since the output is unchanged. Baseline and candidate `nep` were built from master and from this branch with `-DDEBUG`, and run on the same inputs with a test set and three report rows each: NEP, charge_mode 1, charge_mode 2, charge_vdw, vdw, TNEP dipole and TNEP polarizability models, the potential models with and without `lambda_d`. `loss.out` is byte-identical in all 10 cases, and the standard output differs only in the two lines that report the wall-clock time. An independent run of 28 cases, which adds model_type 3, runs without a test set and a restart from `nep.restart`, gave the same result.

`pytest -m "not slow"` on `tests_pytest/test_ediff_loss.py` and `tests_pytest/test_nep_train_outputs.py`: 87 passed. `git-clang-format` reports no changes. The full `tests_regression` suite was not run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb7fbbf17a78`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.