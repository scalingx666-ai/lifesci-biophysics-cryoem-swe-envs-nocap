# Issue

**Title:** Ci nvhpc openacc tests

## Description

**Makefile**: plugins/openaccPTM/Makefile now has ACCTARGET ?= gpu. The default build is the same as before. With ACCTARGET=multicore, the plugin compiles with -acc=multicore, and the -static-nvidia and nodelete link flags are skipped because they only apply to the CUDA runtime.

**Workflow**: in linuxWF.yml, the -nvhpc- step now builds the plugin with ACCTARGET=multicore, then runs make -C regtest and make -C regtest checkfail. It sets ACC_NUM_CORES=2 and ACC_DEVICE_TYPE=multicore.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ef4ee65b21e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.