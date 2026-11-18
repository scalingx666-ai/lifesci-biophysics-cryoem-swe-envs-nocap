# Issue

**Title:** Update pyproject.toml

## Description

This PR pins the jaxley versions to be older than `0.4.32`.

I realized that, with version jax==0.4.32 or newer, simulation time is 10x slower and gradient time is 5x slower as compared to older versions of JAX 🤯 

From the CHANGELOG of JAX:
```
This release of jaxlib switched to a new version of the CPU backend, which should compile faster
and leverage parallelism better. If you experience any problems due to this change, you can
temporarily enable the old CPU backend by setting the environment variable 
XLA_FLAGS=--xla_cpu_use_thunk_runtime=false. If you need to  do this, please file a JAX
bug with instructions to reproduce.
```
Indeed, os.environ['XLA_FLAGS'] = '--xla_cpu_use_thunk_runtime=false' at the beginning of the notebook fixes it also for newer versions of JAX.

See also .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5145025858c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.