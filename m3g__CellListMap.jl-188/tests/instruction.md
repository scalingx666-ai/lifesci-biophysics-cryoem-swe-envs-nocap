# Issue

**Title:** Reset the threaded output copies in parallel

## Description

`pairwise!` reset every threaded copy of the output serially, before spawning the worker tasks. For large array outputs this was a major cost: it grew with the number of threads and could take longer than the pair computation itself.

Each worker task now resets its own copy before it processes any cells. The main output is still reset once, as before.

- **No new tasks.** The reset runs in tasks that were already being spawned, so outputs that are cheap to reset (scalars, small arrays) cost the same as before.
- **Better memory placement.** Each copy is first written by the thread that later accumulates into it.
- **No API change.** Each copy is still reset with the user's `reset_output!(x)`.

## Benchmarks

`pairwise!` time with random particles at density 0.1 and cutoff 2.5:

| n | threads | scalar output, `main` | scalar output, this PR | force vector, `main` | force vector, this PR |
|---|---|---|---|---|---|
| 100 | 8 | 5.2 µs | 5.5 µs | 5.6 µs | 5.5 µs |
| 1,000 | 32 | 23.3 µs | 22.5 µs | 48.3 µs | 44.0 µs |
| 10,000 | 32 | 118 µs | 118 µs | 556 µs | 431 µs |
| 100,000 | 32 | 830 µs | 814 µs | 10.2 ms | 7.6 ms |

Scalar outputs are unchanged within noise.

Packmol.jl case: 2.77M atoms, with an output holding per-atom gradients and per-molecule values. The time per `pairwise!` call went from 158 to 126 ms with 16 threads, and from 220 to 152 ms with 32 threads. In that case the serial reset alone took 70 ms with 16 threads and 133 ms with 32.

All tests pass locally (2,370).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ed37dfbe9666`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.