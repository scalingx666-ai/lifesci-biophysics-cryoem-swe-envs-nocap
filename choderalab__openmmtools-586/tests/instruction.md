# Issue

**Title:** Allow overriding of n_equilibration_iterations and statistical_inefficiency args for computation of free energy

## Description

## Description
The `MultiStateSamplerAnalyzer` automatically determines n_equilibration_iterations and statistical inefficiency (g_t) [here](https://github.com/choderalab/openmmtools/blob/0771ddc36f9a7d4cb183cafe3b95bbd1988fd048/openmmtools/multistate/multistateanalyzer.py#L1466).

I have discovered that for my barnase:barstar repex simulations, number_equilibrated (aka n_equilibration_iterations) is always 1, which is too small. I'd like to be able to manually feed in n_equilibration_iterations and/or statistical inefficiency before retrieving the free energy.

This PR allows overriding n_equilibration_iterations and statistical_inefficiency in the following way:
```python
# Given a MultiStateReporter, create a MultiStateSamplerAnalyzer
analyzer = MultiStateSamplerAnalyzer(reporter, max_n_iterations=3000, n_equilibration_iterations=2001, statistical_inefficiency=10)

# Compute free energy
f_ij, df_ij = analyzer.get_free_energy()
```

Note that the changes in this PR only affect the determination of the decorrelated samples (which are inputed to MBAR to compute the free energy). It does not effect functions like `read_energies()` or `generate_mixing_statistics()`

Replaces 

## Todos
- [x] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [x] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `875f1e69a4a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.