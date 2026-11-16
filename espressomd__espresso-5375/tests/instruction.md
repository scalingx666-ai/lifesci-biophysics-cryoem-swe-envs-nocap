# Issue

**Title:** observables: honor documented LBVelocityProfile optional defaults

## Description

LBProfileObservable::do_construct built the core observable via
make_shared_from_args, which resolves every parameter with get_value and
throws "Parameter '...' is missing." when an optional one is omitted. The
sampling_delta_*/sampling_offset_*/allow_empty_bins parameters are documented
as optional with concrete defaults (1.0/0.0/False), so following the documented
API (e.g. LBVelocityProfile(n_x_bins=..., min_x=..., max_x=..., ...)) raised a
RuntimeError instead of constructing the observable.

Construct the observable explicitly with std::make_shared, using get_value_or
to supply the documented defaults for the seven optional parameters while
keeping get_value for the required n_*_bins / min_* / max_*. This mirrors the
convention already used by the cylindrical profile observables. Added a
regression test in testsuite/python/observables.py that constructs
LBVelocityProfile relying on the defaults and asserts the read-only getters
return them, plus a round-trip check with explicit values.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0918352d3284`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.