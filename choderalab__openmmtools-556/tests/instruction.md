# Issue

**Title:** Add replica exchange attempts during equilibration phase

## Description

## Description
The equilibration protocol currently propagates replicas only; it does not perform replica exchange. As such, it is equilibrating w.r.t. an (alchemically-indexed) family of independent canonical ensembles, rather than the grand canonical ensemble. This PR makes the equilibration loop identical to the main replica exchange loop, excluding disk IO.

Failing to equilibrate w.r.t. the same ensemble can cause transient errors in the free energies of the replicas in the soft-core asymptote region, which manifests as cryptic normalization errors with pymbar, i.e. https://github.com/choderalab/yank/issues?q=W_nk

Note that in https://github.com/choderalab/yank/blob/master/Yank/reports/YANK_Health_Report_Template.ipynb, 2 out of 3 of the free energy call sites take the discard_from_start parameter, however report.generate_free_energy() uses all of the data written to the netcdf file and thus triggers the error anyway. The only way to emulate the discard_from_start parameter for this third call site is to simply not write the bad initial equilibration data to disk in the first place, i.e. to perform equilibration w.r.t. the grand canonical ensemble.

Also note that  claims that the normalization error is due to a change in the numerical solver in pymbar. While that may be part of the problem, I think the fundamental issue is feeding bad data to the solver: if you comment out report.generate_free_energy() and increase discard_from_start (to about 5-10), the norm of the weights will rapidly approach 1 until pymbar successfully runs.

Alternatively, using this PR and setting number_of_equilibration_iterations to 5-10 in Yank, you can keep discard_from_start=1 and avoid the normalization error, even when default_number_of_iterations is arbitrarily small.

## Todos
- [ ] Implement feature / fix bug
- [ ] Add [tests](https://github.com/choderalab/openmmtools/tree/master/openmmtools/tests)
- [ ] Update [documentation](https://github.com/choderalab/openmmtools/tree/master/docs) as needed
- [ ] Update [changelog](https://github.com/choderalab/openmmtools/blob/master/docs/releasehistory.rst)Notable points that this PR has either accomplished or will accomplish.

## Status
- [ ] Ready to go

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c3b468e48f6c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.