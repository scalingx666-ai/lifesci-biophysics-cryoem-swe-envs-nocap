# Issue

**Title:** Update the pytest command IO cases for the tightened GPUMD input contracts

## Description

## Summary

Twenty-three command IO smoke tests in `tests_pytest` fail against master, because four GPUMD input contracts differ from what the cases use. `deform` restricts its legacy numeric form to orthogonal boxes. `deposit` names the deposited species by an element symbol. `compute_rdf` and `compute_angular_rdf` refuse a sampling interval longer than the run. `run` requires an `ensemble` before it.

This MR revises the four cases to the contracts that apply, and adds a test for each contract. It also covers the `compute_angular_rdf` type-pair options, whose argument count and type index bound carry no test.

## Testing

Added to `tests_pytest`:

- `test_deform_legacy_format_requires_an_orthogonal_box` pins that the legacy numeric form runs on `bulk_perovskite` and is refused on `bulk_C` with the documented message.
- `test_deposit_rejects_a_numeric_species` pins that a type index is refused.
- `test_sampling_interval_beyond_the_run_is_rejected` pins the limit for `compute_rdf` and `compute_angular_rdf`.
- `test_angular_rdf_accepts_an_atom_type_pair` pins that `atom type1 type2` adds a fourth column to `angular_rdf.out`.
- `test_invalid_angular_rdf_options_are_rejected` pins the incomplete triple and a type index equal to the number of types, which is the first index out of range.

Every rejection test asserts the diagnostic alongside the exit code. The contract that `run` requires an `ensemble` is covered by `invalid_run_without_ensemble` and `invalid_second_run_without_ensemble` in `tests_regression`.

- the full `tests_pytest` suite: 2 failed, 792 passed, 244 skipped in 457 s.

All 23 cases this MR is about pass. The two failures are `test_tnep_dump_command`, which compares against golden arrays with a tolerance of `rtol=1e-6, atol=1e-8`. This failure will be handled separately.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `155a4ba2b1ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.