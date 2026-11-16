# Issue

**Title:** Restore setNumOMPthreads in the GROMACS 2025 patch

## Description

##### Description

. This supersedes #1434, which I opened against `master` before re-reading
`CONTRIBUTING.md` — the bug is present on `v2.10`, so per your policy ("if it is a bug fix, please open
a merge request on the oldest maintained branch where the bug appears") it belongs here and can then be
merged forward.

Up to `patches/gromacs-2024.3.diff` the GROMACS patch told PLUMED how many OpenMP threads GROMACS was
using (in the `/* PLUMED */` block of `src/gromacs/mdrun/runner.cpp`):

```cpp
int nth = gmx_omp_nthreads_get(ModuleMultiThread::Default);
plumed_cmd(plumedmain, "setNumOMPthreads", &nth);
```

When the 2025.0 patch was rewritten around the native PLUMED support that GROMACS 2025 ships, that call
was not carried over, so `PLMD::OpenMP::getNumThreads()` returns 1 for every GROMACS 2025 run and
anything parallelised over OpenMP silently runs serially. As discussed in #1407, setting
`PLUMED_NUM_THREADS` by hand is not a workable substitute: the right number is not known in advance on
heterogeneous hardware, and it changes once GROMACS' dynamic load balancing redistributes the work.

This restores the call in the `PlumedForceProvider` constructor, behind the same API-version gate the
2024.3 patch used.

###### Placement and safety

- `gmx_omp_nthreads_init()` is called in `runner.cpp` well before `mdModules_->initForceProviders()`,
  so the thread count is initialised by the time the force provider is constructed.
- `src/gromacs/applied_forces/{colvars,densityfitting,qmmm}` already include `gromacs/mdlib/` headers,
  so the added include does not violate GROMACS' module layering.
- The count is set once, at construction. That is sufficient: `gmx_omp_nthreads_set()` has exactly one
  production call site (inside `gmx_omp_nthreads_init()`); every other caller is a unit test, so the
  value cannot change mid-run. `PLUMED_NUM_THREADS` still wins where set, because the environment is
  consulted on the first `getNumThreads()` call, which happens after this constructor-time set.

###### Verification

- `plumedforceprovider.cpp.preplumed` on this branch is byte-identical (md5 `3dc19602…`) to the pristine
  GROMACS file at **v2025.0, v2025.1, v2025.2, v2025.3 and v2025.4**, so the hunk applies across the
  whole 2025 series. Re-confirmed on this branch by applying the generated diff to a real pristine
  v2025.4 source with the same flags `patch.sh` uses (`patch -u -l -b -F 5 -N`); the result is identical
  to the patched file committed here.
- Built GROMACS 2025.4 with this patch applied (clang 20.1.1, `-DGMX_USE_PLUMED=ON`, runtime mode) and
  ran `gmx mdrun -ntmpi 1 -ntomp N -plumed` with `PLUMED_NUM_THREADS` unset. `PLUMED.OUT` reports, with
  and without the restored block (same build tree, same kernel, only this hunk differing):

  | `-ntomp` | without the block | with the block |
  |---------:|------------------:|---------------:|
  | 1        | 1                 | 1              |
  | 4        | **1**             | 4              |
  | 8        | **1**             | 8              |

###### Two related gaps, not addressed here

The same 2024.3 block contained two further items that are also absent from the 2025 patch:

1. `plumed_cmd(plumedmain, "setGpuDeviceId", &deviceId)` (gated on API > 9);
2. the warning that `-update gpu` combined with `-plumed` produces wrong results unless PLUMED only acts
   on neighbour-list-search and/or file-writing steps.

Both would need new fields plumbed through `PlumedOptions`, the way `replex_` and `ms_` already are.
Happy to open a separate issue or PR if you confirm these were dropped unintentionally rather than
deliberately.

##### Target release

I would like my code to appear in release __v2.10__ (and to be merged forward into master from there).

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [X] I added a new regtest or modified an existing regtest to validate my changes.

  `regtest/basic/rt-make-ompthreads` checks the mechanism the patch depends on: that
  `cmd("setNumOMPthreads")` is honoured and visible to `PLMD::OpenMP::getNumThreads()`. It also pins down
  that the command is re-issuable rather than latch-once, and that 0 is mapped to 1.

  The patch itself cannot be regtested directly: `patches/` is never compiled (no object is produced
  under it), and there is no harness that patches and builds a GROMACS tree. But the mechanism *is* what
  broke, and it is what the test now covers — when it regresses, `getNumThreads()` returns 1 and every
  OpenMP-parallel action silently runs serially, which leaves results correct and only shows up as lost
  performance. End-to-end behaviour remains verified by the measured `-ntomp` table above on a real
  GROMACS 2025.4 build.

- [X] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f913f767812b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.