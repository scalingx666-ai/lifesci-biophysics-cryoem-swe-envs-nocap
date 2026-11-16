# Issue

**Title:** Keep the OpenMP kernel loaded during Python regression shutdown

## Description

## Summary

Scope the existing PLUMED_LOAD_DLCLOSE=no option to python/rt-symfunc. Keep the loaded kernel resident until process exit so OpenMP workers are not left waiting in an unloaded runtime. Numerical output, reference files, tolerance and the configured thread count are unchanged. This is a regression-process lifetime fix; it does not change the production kernel or wrapper defaults.

## Evidence

The Linux MPI CI run of #1465 completed the calculation but failed at process exit. A local two-thread reproduction and GDB backtrace show an OpenMP worker in gomp_barrier_wait_end while the Python main thread calls dlclose from Plumed deallocation. One-thread runs completed. A local unchanged-target control also completed, so the failure is timing-dependent; it is not attributed to the domain-exchange change.

With this scoped setting, three independent two-thread processes completed and their logfile files matched the original reference exactly. No scientific or numerical criterion was relaxed.

This targets v2.10: the regression is absent from v2.9. It is independent of #1465 and can be reviewed first.

## CI follow-up (2026-10-07)

The NVHPC-MPI job reports exit 139 in the unchanged `python/rt-grid` regression. This patch changes only `rt-symfunc/config` and its README; the available job log does not establish the cause of the separate `rt-grid` crash or that it has the same shutdown mechanism. No run artifacts were available at the audit snapshot. The NVHPC check remains unresolved. The `codecheck` failure is an Autoconf download timeout before source checks. Numerical references and tolerances are unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53a1773d7cea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.