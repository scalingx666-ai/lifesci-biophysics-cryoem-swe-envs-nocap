# Issue

**Title:** core: fix non-finite FENE energy under compression

## Description

FeneBond::force() guards bond breakage with `std::abs(dr) >= drmax`
(made symmetric in PR #5195), but FeneBond::energy() still used the
one-sided guard `dr >= drmax`. For a bond compressed below r_0 - d_r_max
(dr <= -drmax, a supported config since r_0 > d_r_max is allowed), the
energy guard was not taken and `log(1 - dr*dr/drmax^2)` received a
non-positive argument, returning NaN/-inf wrapped in a non-empty optional.
That value was silently summed into the total energy with no
bond_broken_error and no NaN sanitization.

Mirror the force() guard so energy() returns an empty optional
(bond-broken) for compression beyond drmax, keeping force and energy
symmetric.

Adds src/core/unit_tests/fene_test.cpp covering the analytic energy
within range and the compression symmetry between force() and energy().

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e8c3695ea5f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.