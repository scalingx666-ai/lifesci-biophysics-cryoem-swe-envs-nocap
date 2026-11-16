# Issue

**Title:** Optimize call to `box_vector_length_angle` in `compute_distances_core`

## Description

In a trajectory with many frames (>1000), the bottleneck in a distance calculation is the call to `box_vector_length_angle`. This is because of the `for` loop, iterating over every frame. It is much faster to use a numpy-vectorized call.

```python
from mdtraj.utils.unitcell import box_vectors_to_lengths_and_angles
import numpy as np
import timeit

np.random.seed(42)
n_frames = 5000

unitcell_vectors = np.zeros((n_frames, 3, 3), dtype=np.float32)
unitcell_vectors[:, 0, 0] = 3.0 + np.random.rand(n_frames) * 0.1
unitcell_vectors[:, 1, 1] = 3.0 + np.random.rand(n_frames) * 0.1
unitcell_vectors[:, 2, 2] = 3.0 + np.random.rand(n_frames) * 0.1

# OLD: Python loop, one frame at a time (what was in compute_distances_core)
def old_way(unitcell_vectors):
    unitcell_angles = []
    for fr_unitcell_vectors in unitcell_vectors:
        _, _, _, alpha, beta, gamma = box_vectors_to_lengths_and_angles(
            fr_unitcell_vectors[0],
            fr_unitcell_vectors[1],
            fr_unitcell_vectors[2],
        )
        unitcell_angles.append(np.array([alpha, beta, gamma]))
    orthogonal = np.allclose(np.array(unitcell_angles), 90)
    return orthogonal

# NEW: single vectorized call
def new_way(unitcell_vectors):
    _, _, _, alpha, beta, gamma = box_vectors_to_lengths_and_angles(
        unitcell_vectors[:, 0, :],
        unitcell_vectors[:, 1, :],
        unitcell_vectors[:, 2, :],
    )
    orthogonal = np.allclose(alpha, 90) and np.allclose(beta, 90) and np.allclose(gamma, 90)
    return orthogonal

# Verify correctness
assert old_way(unitcell_vectors) == new_way(unitcell_vectors)

# Also test non-orthogonal
unitcell_vectors2 = unitcell_vectors.copy()
unitcell_vectors2[:, 1, 0] = 0.5
assert old_way(unitcell_vectors2) == new_way(unitcell_vectors2)

print(f'Benchmarking with {n_frames} frames...')
iters = 100
t_old = timeit.timeit(lambda: old_way(unitcell_vectors), number=iters)
t_new = timeit.timeit(lambda: new_way(unitcell_vectors), number=iters)
print(f'  Old (per-frame Python loop): {t_old:.4f}s over {iters} calls')
print(f'  New (single vectorized call): {t_new:.4f}s over {iters} calls')
print(f'  Speedup: {t_old/t_new:.1f}x')
```
  Old (per-frame Python loop): 10.6442s over 100 calls
  New (single vectorized call): 0.0520s over 100 calls
  Speedup: 204.6x

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e9c1152ee3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.