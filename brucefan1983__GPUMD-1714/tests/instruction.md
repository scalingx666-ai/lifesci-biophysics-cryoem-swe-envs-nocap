# Issue

**Title:** Expect the current add_spring output file names

## Description

## Summary

The `add_spring` smoke case in `tests_pytest/test_io_basic_setup.py` looked for a file that the run does not write, so it failed on a run that was otherwise correct. The spring force file and the ghost restart file are named after the group method, the group ID, and the spring ID, as documented in `doc/gpumd/input_parameters/add_spring.rst`. The case now expects `spring_gm0_g0_s0.out` and `spring_gm0_g0_s0.restart`.

The case also checks the contents of the spring force file through a `parse_check`. It verifies the seven column header, one row per output stride, and that every field parses as a number. The expected number of rows follows from `output_stride_` in `src/measure/add_spring.cuh` and from `BASE_N_STEPS`, counting from step 0.

No source file is touched.

## Testing

`pytest tests_pytest -q` on an RTX 3080 Ti with CUDA 12.9 gives 754 passed, 2 failed, and 225 skipped. All nine `add_spring` parametrizations pass, with two skips for the qNEP models that are absent for `bulk_C`.

The two failures are unrelated to this change and reproduce by running `gpumd` directly.

* `test_command_io[bulk_C-nep-deform]` stops at `measure/deform.cu:225` with "The legacy deform format only supports orthogonal boxes", and the `bulk_C` cell is triclinic.
* `test_deposition_generates_a_usable_dump_xyz_line` stops at `main_gpumd/deposition.cu:54` with "deposit atom species should be specified using an element symbol".

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db7325b1067b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.