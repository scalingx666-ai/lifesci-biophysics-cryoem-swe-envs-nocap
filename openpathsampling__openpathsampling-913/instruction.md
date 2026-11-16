# Issue

**Title:** Fix: OPS hangs when external engine errors/exits unexpectedly

## Description

Problem: I was using GROMACS and OPS couldn't tell when GROMACS had exited due to errors.

Reason: External engine get's checked if it's still running using subprocess.Popen.process.is_running() call. This doesn't actually communicate with the process to ask if it's still running, it returns the value corresponding to last time it was checked. In this case, when the process was set up, so case of not running never gets called.

Solution: Altered to use subprocess.Popen.process.poll() - return None is process still running, and the return code if it isn't. 

I've also altered the stop signal to copy with the process already having exited of it's own accord

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `417b5aae08c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.