# Issue

**Title:** Reduce cost of u_kl computation in HREX

## Description

Builds on #1308, using `Potential::execute_batch_sparse` introduced in the latter to avoid computing (very likely) unused elements of $u_{kl}$ corresponding to far-away states for a given replica. This introduces a new parameter ~`n_neighbor_states`~ `max_delta_states` controlling the number of neighbors on either side of a replica's current state for which to evaluate the potential; 4 seems to be a reasonable value empirically. With 48 windows, this would result in 1/6th of the potential evaluations we do currently, potentially yielding a significant speed boost.

With current defaults, distribution of the number of states a given replica is displaced during a single HREX permutation move (consisting of $K^3$ swap attempts). The probability mass outside the interval [-4, +4] is insignificant:

![image](https://github.com/proteneer/timemachine/assets/319411/ac855857-818f-4765-aa21-dda2193a6f4c)

Note: only `n_neighbors >= 1` is needed for correctness; as long as this is satisfied, choosing a number that significantly truncates the distributions above will result in slower mixing, but still asymptotically correct sampling.

<a id="benchmarks"></a>
## Benchmarks

- system: `timemachine.testsystems.relative.get_hif2a_ligand_pair_single_topology`
- default parameters w/ HREX, except setting `n_frames=100`
- 3 replicates
- `a` is current master branch; `b` is this branch

![image](https://github.com/proteneer/timemachine/assets/319411/04f85ce8-2217-46f8-9acd-614ed90cd0db)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a821ddcdb24a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.