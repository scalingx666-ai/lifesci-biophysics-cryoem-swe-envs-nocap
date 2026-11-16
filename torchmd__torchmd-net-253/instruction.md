# Issue

**Title:** Add check_errors option

## Description

Adds two new options:

- check_errors (default: True): Checks if some neighbor pair was missed due to max_num_neighbors being too small. True is incompatible with CUDA-graphs.
- static_shapes (default:False): Ensure tensornet uses statically shaped tensors regardless of the number of neighbors when true. Required for CUDA-graph compatibility.

The default values for these previous to this PR are tailored for inference (CUDA-graphs), but we have seen they can hurt performance and increase memory usage during training.
Thus, I am defaulting these to maximize training performance. Advanced users can tweak these to squeeze performance during inference.
cc @stefdoerr @guillemsimeon

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1f2f5ad8db8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.