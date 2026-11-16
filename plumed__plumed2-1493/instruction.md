# Issue

**Title:** Fix the -intel- (rt-make-angle) and -nvhpc-mpi- CI failures on master

## Description

##### Description

This fixes the two CI jobs that fail on `master` (and so on every PR into it): `-intel-` and `-nvhpc-mpi-`. One commit each.

**1. `-intel-`: `tools/rt-make-angle` (intermittent; 3 of master's last 7 runs).** Only the `float` part of the test fails: two parallel vectors give an angle of `0.000345267` instead of `0`, and two antiparallel ones `3.14125` instead of `pi`.

`Angle::compute` takes a shortcut when `dpnn` is within `epsilon` of ±1. `PLMD::epsilon` is the *double* epsilon, though, so for `T=float` the threshold `1.0-epsilon` means "exactly 1". A one-ulp error in `dp*nn` (`0.99999994f`) skips the shortcut, and `acos(0.99999994f)` is `0.000345267`. The fix uses `std::numeric_limits<T>::epsilon()`. For `double` that is `PLMD::epsilon` itself, so double results cannot change.

I reproduced it locally with clang `-ffast-math`, which approximates `1/sqrt` the way the Intel compiler's default floating-point model may. Without the fix, the output matches the CI diff character for character. With the fix it matches the reference at `-O0`, at `-O3`, and with `-ffast-math` for both haswell and skylake-avx512. I have not found why the failure is intermittent with the same compiler (oneAPI 2026.1 in both passing and failing runs); my guess is the runner's CPU. `rt-make-angle`, the other `tools/` regtests and the 35 regtests that use `Angle` pass with this change.

**2. `-nvhpc-mpi-`: internal compiler error.** Building the openacc plugin, nvc++ 24.3 stops with `Internal compiler error. BAD sptr in func_refsym` in `core/ActionWithArguments.h`. It started with my #1485 (C++20 for the libtorch CI step), once `v2.10` was merged into `master`:

- The compiler is nvc++ 24.3 both in the last passing run (`2f64cb67f`) and in every failing one since `8743620c3`.
- Before #1485, `configure` could not enable libtorch and libmetatomic under C++17 in this job, and dropped them with a warning. Now it enables both and builds with `-std=c++20`, and the plugin inherits PLUMED's flags.
- #1485 was tested green on my fork, `-nvhpc-mpi-` included. That run was on `v2.10`, which has no openacc plugin, so the plugin never met the C++20 configuration until the merge into `master`.

This job only checks that the openacc plugin compiles, so the second commit skips the libmetatomic/libtorch step there. That restores exactly what the job built before #1485. I could not reproduce the ICE locally with nvc++ 24.3 and the same flags (C++17 or C++20, with or without MPI, several `-tp` targets), so this PR's own `-nvhpc-mpi-` job is the test of this commit.

Found and fixed with the help of Claude.

##### Target release

I would like my code to appear in release __master__ (neither the float `Angle` test nor the openacc plugin exists on `v2.10`).

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

No new regtest: `tools/rt-make-angle` already covers it, but it only fails where floating-point math is approximated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d14af1bf59fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.