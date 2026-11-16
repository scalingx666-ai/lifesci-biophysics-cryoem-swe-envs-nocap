# Issue

**Title:** Fix checkpointing of systems without a time step

## Description

IntegratorHandle::on_bind_system has a guard that skips re-applying the
default -1. sentinel time_step during deserialization, since
System::set_time_step(-1.) throws std::domain_error for non-positive
values. The 5th guard clause evaluated get_value<double> on the *bool*
returned by is_type<double>(...) instead of on the stored value. The
bool coerces to 1.0, so the clause was permanently 1.0 == -1. (false)
and the guard never skipped. Restoring a System whose time_step was
never set (stays at the -1. sentinel, default Velocity Verlet / NVT)
therefore re-applied set_time_step(-1.) and raised a ValueError.

Evaluate the actual stored value: get_value<double>(params.at(key)),
restoring the pre-refactor logic. The preceding is_type<double> clause
already guarantees the variant holds a double, so this is safe.

Added testsuite/python/integrator_time_step_restore.py: a pickle
round-trip of a System with an unset time_step must succeed and keep
the -1. sentinel.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9ac1a682e78f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.