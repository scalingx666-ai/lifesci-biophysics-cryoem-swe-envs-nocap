# Issue

**Title:** NeighborList: build the reduced atom list without sorting all the pairs

## Description

# Force propagation: plan (2026-11-12, updated 2026-11-13)

Builds on [[force-propagation-analysis.md](https://claude.ai/cowork/force-propagation-analysis.md)](force-propagation-analysis.md). The phase 1 results are in [[force-propagation-phase1.md](https://claude.ai/cowork/force-propagation-phase1.md)](force-propagation-phase1.md).

The order is: investigate → cover with tests → fix, and only then improve. Every improvement must be measured and must not slow down the general case (the two-kernel DETAILED_TIMERS harness from the cudaCoord notes).

## Decisions
- 2026-11-12, Carlo: get the whole picture before fixing. All of phase 1 was completed and triaged before phase 2.
- 2026-11-12, where to run:
  - **Phases 0–2:** the cloud container (OpenMPI, 2 cores, 7 GB RAM).
  - **Carlo's Mac:** a confirmation run, and the phase 3 timings.
  - **Leonardo:** large-scale numbers.
- 2026-11-13, Carlo: fix for the thread count after init is option **(c)**. While an initialized PLUMED instance exists, `setNumOMPthreads` can lower the count but not raise it; a larger value is ignored with a **warning** (not an error).

## Phase 0: Setup (done)
See phase1.md for the builds, forcecheck.py and the container pitfalls.

## Phase 1: Investigate and cover (done 2026-11-13)
Results in phase1.md:
- 1.1 double MPI sum: not a bug (guarded);
- 1.2 the core force propagation is consistent; the sweep found the VES_DELTA_F multi-walker bug;
- 1.3 thread count after init: confirmed crash, a class of bugs;
- 1.4 coverage: all special force paths are covered except the two threaded branches of ActionWithValue;
- 1.5 noderiv: phase 3 item;
- ASan: clean.

## Phase 2: Fix (one PR per item)
1. **Thread count after init: DONE.** Branch `omp-threads-after-init` (cloud, commit ac0a2daec on master 2088bf663). The cloud cannot push, so the patch is in the Mac's `Claude outputs/force-propagation/0001-Do-not-let-the-number-of-OpenMP-threads-grow-after-i.patch`.
   - `OpenMP::setNumThreads` returns false while `nlocks>0` and nt > the current value.
   - `lockNumThreads()`/`unlockNumThreads()`, with a mutex, are called by PlumedMain::init and the destructor.
   - The lock first reads PLUMED_NUM_THREADS, so the env behaviour before init is unchanged.
   - The rule is "never above the current value", not "never above the value at init". An action created after a decrease is sized for the smaller count, so going back up would be unsafe too.
   - Warning: to the log if it is open, otherwise to stderr ("PLUMED: WARNING: setNumOMPthreads(N) ignored ...").
   - Guard `plumed_massert(nt<=omp_forces.size())` in `ParallelTaskManager::applyForces`.
   - Regtest basic/rt-make-ompthreads-after-init covers: increase after init (ignored, same forces); decrease (accepted; an increase after the decrease is ignored); another instance trying to increase; release of the limit after destruction.
   - Entries in CHANGES/v2.11.md: users (Core) and developers.
   - Checked: all regtest/basic tests pass, also the new test with PLUMED_NUM_THREADS=3, and astyle reports no changes.
   - Not tested: thread-MPI (`--tmpi-threads`). The cloud build has real MPI. The counter is protected by a mutex.
2. **VES_DELTA_F, multiple walkers with more than 1 rank per walker:** patch vesdeltaf-multiwalker.diff, plus a regtest copy of rt-VesDeltaF-mwalkers with mpiprocs=4.
3. **Regtest for the threaded branches** of ActionWithValue::checkForForces and clearDerivatives (the synthetic `threaded-paths` input; generate the 6000-atom xyz in plumed_regtest_before).
4. **Cleanups:**
   - remove the empty "5B Update forces" timer;
   - swap the inverted ForcesOutput field names;
   - optionally let SERIAL bypass the AVOID_THREAD_GATHER+MPI guard.
- Dropped: the old 2.1 "double MPI sum" fix (not a bug), and the old 2.2 "grow omp_forces" (replaced by item 1).

## Phase 3: Improvements (measure, then decide)
1. **Baseline profile** with DETAILED_TIMERS and per-pass timing on representative inputs (on the Mac, large cases on Leonardo):
   - Carlo's funnel input;
   - large GROUPA×GROUPB COORDINATION;
   - matrix CVs (COORDINATIONNUMBER, Q6);
   - many-CV METAD;
   - SAXS in component mode;
   - an input with many CVs that are only printed.
   Measure separately: clearDerivatives, filling the Jacobians, checkForForces, the backward re-run in PTM, and MPI.
2. **Honour `noderiv`** (phase 1 bench: up to ~25% of the forward pass of printed-only CVs). Start in core: Colvar sizing, clearDerivatives and a cheap early-out. Then do the heaviest CVs one at a time.
3. **MPI reduction volume.** Reduce only the touched part of forcesForApply, or merge the reductions.
4. **Backward re-run of vector/matrix CVs.** Prototype caching the per-task derivatives during the forward pass, and compare that against re-running `performTask`. Adopt it only if it is a clear win and it keeps the code simple.
5. **Possibly:** make AVOID_THREAD_GATHER MPI-capable (reduce once, after the last pass).
6. Each improvement goes in its own PR with numbers, and the regtests must stay bitwise identical (or within documented tolerance).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b5edd52d55f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.