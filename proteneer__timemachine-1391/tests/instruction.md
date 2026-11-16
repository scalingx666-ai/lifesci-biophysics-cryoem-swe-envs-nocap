# Issue

**Title:** Clean up HREX, BDExchangeMover and TIBDExchangeMover

## Description

* Fixes a potential bug in the BDExchangeMover that doesn't pass the stream to a kernel. Not seen in practice because we use the default stream as well as the fact that we synchronize the stream immediately before calling the kernel in question. Good to fix regardless
* Fixes an HREX test that was no longer patching the correct function as of #1334, added assertions to avoid missing again
* Removes duplicate equilibration logic in `run_sims_hrex` to avoid code becoming stale. 
* Avoids setting the same seed for TIBDExchangeMover in `get_context` across initial states. Use the integrator seed, which is distinct to each InitialState.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `758725e6385a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.