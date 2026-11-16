# Issue

**Title:** Improve performance of HREX swap moves

## Description

Investigations done as part of #1287 showed that the performance of HREX falls off rapidly for larger numbers of windows. This is particularly pronounced in vacuum, where the overhead of the HREX moves is large compared to sampling and computation of the $u_{kl}$ matrix. See the right-hand panel of the following figure:

<img width="500px" src="https://private-user-images.githubusercontent.com/5840832/323736023-9ca7a411-bf5e-4379-93a9-0211badc07ef.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTQxNDgxMTgsIm5iZiI6MTcxNDE0NzgxOCwicGF0aCI6Ii81ODQwODMyLzMyMzczNjAyMy05Y2E3YTQxMS1iZjVlLTQzNzktOTNhOS0wMjExYmFkYzA3ZWYucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDQyNiUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA0MjZUMTYxMDE4WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZDIwMTNiNzA0NDA0OGU2YzZkYzcxZmE5YzI3YjUwOWQyNWFkZDViNWMzMTNhMDU1MTMyOTE3NDcyMWUzNTVhYiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ.AOCi5GfSoqY8_WfAA6SNzTdfB08qpQdtmB4FkFPQJ7A"></img>

The rapid dropoff in vacuum performance is largely due to the rapid growth (as $L^3$, according to the [heuristic](https://github.com/proteneer/timemachine/blob/70706e301ab99d8f3dd513ccd0af2235f8c89e82/timemachine/md/hrex.py#L220) we currently use) of the number of swap attempts per HREX iteration with $L$ windows. E.g. with 48 windows, we do $48^3 \approx 10^5$ swap attempts per iteration. Because a batch of swap attempts is currently implemented as an unoptimized pure-Python for loop, there should be significant room for performance improvement in the implementation.

This PR adds a specialized implementation of a batch of neighbor swap moves that generates random numbers in batches and leverages JAX's jit compilation for performance. Benchmarks show that this implementation is significantly more performant (~50-100x) for large numbers of windows, although the initial compilation does introduce a small amount of overhead with a small number of windows.

### HREX MD timings
100 frames, no water sampling. As expected, performance improvement is significant for vacuum and marginal for solvent (where sampling and $u_{kl}$ computation is much more significant)

![benchmarks](https://github.com/proteneer/timemachine/assets/319411/71bfe0b1-980d-4466-a11e-dd2dee001aa7)

### Profiling permutation moves
With 4 windows, the overhead of JIT compilation is significant:

- [master, 4 windows](https://github.com/proteneer/timemachine/assets/319411/eca94d75-0bc6-427a-883c-992901cf668e)
- [branch, 4 windows](https://github.com/proteneer/timemachine/assets/319411/5946fdda-d83b-4082-93f6-ae2cea35e9df)

With 48 windows, optimized version is ~10x faster:

- [master, 48 windows](https://github.com/proteneer/timemachine/assets/319411/24a0e544-6f51-4e78-8e4b-5879ee8516bb)
- [branch, 48 windows](https://github.com/proteneer/timemachine/assets/319411/d64a94ba-a2a6-4869-afa0-47b7022ec78f)

Time to run a single permutation move (consisting of $L^3$ swap attempts), for the current version ("ref") and the version in this PR ("fast"). The time to compute the $u_{kl}$ matrix ("u_kl") is shown for reference. Note: this does not include the initial JIT compilation time for the fast version, which only runs once for a given number of windows and takes about half a second.

![timings](https://github.com/proteneer/timemachine/assets/319411/8bff32e5-117b-4187-90a8-0a02eeefdbff)

### Other changes
- Removes `BatchedMixtureOfMoves` in favor of adding a method `move_n` to the base class. Subclasses can override the default definition of this and `sample_chain` to provide efficient implementations.

### TODO
- [x] Docstrings
- [ ] Update tests to also exercise reference version
- [ ] Test more directly comparing results of reference and fast version?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2349142300c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.