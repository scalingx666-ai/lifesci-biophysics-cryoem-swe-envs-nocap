# Issue

**Title:** Reject delta_N < 1 in accumulators

## Description

The Python accumulator API documented delta_N as a positive integer sample
spacing, but no layer validated it. delta_N <= 0 reached the core unchecked:

- Correlator(delta_N=0): m_dt = time_step * 0 = 0, so the m_tau_max <= m_dt
  guard (e.g. 2.0 <= 0.0) is false; initialize_operations() then computes
  ceil(1 + log2(inf)) and static_cast<int>(inf) for the hierarchy depth (UB),
  producing a bad multi_array resize. Observed as MemoryError
  (std::bad_array_new_length) at construction. This UB does not throw, so the
  existing parallel_try_catch around construction did not catch it.
- MeanVarianceCalculator/TimeSeries/ContactTimes(delta_N=0) constructed fine,
  then auto_update_accumulators.add(acc) + integrator.run() set frequency=0,
  making next_update() return 0 -> integrate(0) never advances -> infinite
  loop (release) / assert abort (debug).
- The runtime setter acc.delta_N = 0 was equally unguarded.

Fix (PREVENT invalid input, matching the existing tau_lin/tau_max validation
convention): add a single chokepoint guard in the core
Accumulators::AccumulatorBase ctor (all four accumulators chain to it) that
throws std::runtime_error("delta_N must be >= 1") before initialize() runs,
killing both the Correlator UB and the auto-update hang. Wrap the previously
unwrapped MeanVarianceCalculator and TimeSeries SI ctors in
parallel_try_catch so the throw is MPI-coordinated (Correlator/ContactTimes
already do this). Guard the SI delta_N setter the same way so acc.delta_N = 0
is rejected and leaves the previous value intact.

Extend test_correlator_exceptions and add test_delta_N_validation in
accumulator_correlator.py to cover delta_N in (0, -1) at construction and via
the runtime setter for Correlator, MeanVarianceCalculator and TimeSeries.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da6966608a3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.