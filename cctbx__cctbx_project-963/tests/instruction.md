# Issue

**Title:** boost: switch deprecated boost/timer.hpp to boost/timer/timer.hpp

## Description

- Change header from boost/timer.hpp to boost/timer/timer.hpp
- Change boost::timer to boost::timer::auto_cpu_timer
- Handle switch to nanoseconds
- Build Boost timer library if needed

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7435a7279b44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.