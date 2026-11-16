# Issue

**Title:** regtest: keep the kernel loaded during the rt-protein Python shutdown

## Description

##### Description

`python/rt-protein` fails intermittently on the Linux `-mpi-`, `-nvhpc-mpi-` and `-debug-` jobs. The
calculation itself completes; the failure is at **process exit**, where an OpenMP worker can still be
inside the kernel's runtime when the Python main thread calls `dlclose`.

This is the same mechanism @Zhang-pchao diagnosed for the sibling test `python/rt-symfunc` in #1470,
with a GDB backtrace showing a worker in `gomp_barrier_wait_end` during `dlclose`. The two configs are
near-identical — `rt-symfunc` only adds `plumed_modules="symfunc adjmat"` — so `rt-protein` is exposed
to the same race and currently lacks the same mitigation.

Apply the same scoped setting. `PLUMED_LOAD_DLCLOSE=no` affects this regression only; the thread count,
calculation, tolerance and reference output are unchanged.

**Observed on unrelated pull requests**, which is what identifies it as a shutdown race rather than a
behaviour change: it fails on #1465 (`src/core` domain exchange), on #1460 (`src/adjmat`) and on #1468
(which touches `patches/` only and no source at all).

Credit to @Zhang-pchao for the diagnosis; this just carries it across to the second affected test. If
you would prefer it folded into #1470 rather than landing separately, that is fine by me — please say
so and I will close this.

##### Target release

I would like my code to appear in release __v2.10__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [X] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!-- The first box: this modifies an existing regtest's configuration, which is the change itself.
The second is left unticked until CI has run here; note that two further unrelated failures
(drr/rt-extract2dpbc on -intel-, and codecheck's GNU-mirror download) are currently red on every PR
in the repository, so a fully green board is not achievable from this PR alone. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53a1773d7cea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.