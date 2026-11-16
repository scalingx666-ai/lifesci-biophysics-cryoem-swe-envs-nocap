# Issue

**Title:** Fix NoseHooverIntegrator crash with no thermostats and CMMotionRemover

## Description

`NoseHooverIntegrator::initializeThermostats` read `noseHooverChains[0]` whenever the System had a CMMotionRemover. With no thermostats the vector is empty, so Context creation crashed. The CMMotionRemover block now runs only if there is at least one chain. There are no degrees of freedom to remove otherwise.

Added `testNoThermostatsWithCMMotionRemover` to the shared NoseHooverIntegrator tests. On the Reference platform it segfaults without the fix and passes with it.

Not verified: the CPU, OpenCL and CUDA test executables (not built here, only Reference), and the Python script from the issue (no Python build available, used the C++ equivalent).

Thanks to @jinzomotsu for the report and the pointer to the offending block.

AI disclosure: this fix and test were produced by an AI agent (Claude Code) and checked by building and running the Reference test before and after the change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1c39e7844b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.