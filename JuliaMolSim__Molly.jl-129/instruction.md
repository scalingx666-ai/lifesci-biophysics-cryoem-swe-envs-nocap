# Issue

**Title:** Add option to disable loggers in all Simulator structs

## Description

Thoughts on adding `run_loggers` parameter to all Simulator structs? I noticed it exists in the `StepestDescentMinimizer` but not other things like `VelocityVerlet`.

I ran into a use case where I would like to disable logging for the first 300k steps of a VV scheme and then re-enable it after the system has come to the correct temperature. Could also be worth allowing users to change what gets logged in-between calls to `simulate!` within the same code (maybe you can do this already I dunno). Something like `modify_loggers!(sys, ...)`. I also think there's a lot of performance to be gained here, maybe by writing to a file instead of RAM, just a thought.

I have not gone thru and made all the necessary changes just wanted to open this to discussion.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c0631401a2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.