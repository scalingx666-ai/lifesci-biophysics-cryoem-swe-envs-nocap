# Issue

**Title:** perf(sharding): compressed morton code and gridpoints

## Description

- [x] compressed_morton_code
- [ ] gridpoints / xyzrange

#### feat(compressed_morton_code): accept arrays as input
For shard generation and reading large cutouts, several thousand codes need to be calculated. By allowing the entire array as input, we properly utilize numpy:
```python
grid_size = Vec(520,440,260)
gpts = np.array([np.random.randint(520, size=65536), np.random.randint(440, size=65536), np.random.randint(260, size=65536)]).T

%timeit for gridpoint in gpts: compressed_morton_code(gridpoint, grid_size)
# 4.2 s ± 168 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

%timeit compressed_morton_code(gpts, grid_size)
# 8.16 ms ± 350 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a674b4f88db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.