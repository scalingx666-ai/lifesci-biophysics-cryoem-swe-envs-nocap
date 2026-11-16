# Issue

**Title:** Write sliced trajectories to XTC files

## Description

## Description

Following the conversation at #429, this PR adds support for writing and reading (sliced) sampler states to `xtc` files using `mdtraj` IO helpers.

To do that, I have modified `openmmtools.multistate.multistatereporter.MultiStateReporter.{write,read}_sampler_states` so they use two new methods:

- `._write_sampler_states_to_xtc()`. Writes to a `<prefix>_replica.<replica_index>.xtc` file per `StateSampler` in the given iteration. `mdtraj.formats.XTCFileTrajectory` does not support `append` mode, so we have to binary concatenate a temporary `xtc` file to work around that, resulting in two write operations. This might not be ideal for performance, so we might need to add real `a` support if the benchmarks don't show an improvement.
- `._read_sampler_states_from_xtc()`. Traverses the `<prefix>_replica.*.xtc` files and generates a `SamplerState` for each requested frame/iteration index.

## Todos
- [X] Implement feature / fix bug
- [x] Benchmark performance
- [X] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
    - Note: The existing tests already hit the new methods, but maybe we need to add more subtleties (box vectors, etc).
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst) with notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6cf88cc77e08`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.