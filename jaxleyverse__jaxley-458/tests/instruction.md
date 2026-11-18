# Issue

**Title:** Reduce warnings

## Description

# Fixing deprecation warnings
We change all:
- jax.tree_map -> jax.tree_util.tree_map
- Pytest ignores TestSynapse

Note: We do not change use of `a_max` in `jnp.clip`, because this require jax>0.4.30 (which is too recent yet)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26acd32dcbfa`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.