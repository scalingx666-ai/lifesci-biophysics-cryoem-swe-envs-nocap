# Issue

**Title:** Speed up analysis by only retrieving end thermodynamic states as needed

## Description

Examining logging timesteps suggests the slowest step in `yank analyze` is currently retrieval of the end thermodynamic states:
```
[chodera@lilac:c-Met_hinge_binders]$ yank analyze --store sams-twostage-rmsd-mcligand-dense/experiments/cmethingesinglehinge0
2018-05-12 11:05:01,293: Reading energies...
2018-05-12 11:05:01,629: Done.
2018-05-12 11:05:01,630: Assembling effective timeseries...
2018-05-12 11:05:01,973: Done.
2018-05-12 11:05:01,993: Checking if we need to unbias the restraint...
2018-05-12 11:05:01,993: Trying to get radially symmetric restraint data...
2018-05-12 11:05:01,993: Retrieving end thermodynamic states...
2018-05-12 11:16:09,873: Isolating restraint force...
2018-05-12 11:16:27,693: No force of type class 'openmmtools.forces.RadiallySymmetricRestraintForce' could be found. The restraint will not be unbiased.
2018-05-12 11:16:27,758: Reading energies...
2018-05-12 11:16:27,778: Done.
2018-05-12 11:16:27,778: Assembling effective timeseries...
2018-05-12 11:16:27,985: Done.
2018-05-12 11:16:28,004: Assembling uncorrelated energies...
2018-05-12 11:16:28,081: Found expanded cutoff states in the energies!
2018-05-12 11:16:28,081: Free energies will be reported relative to them instead!
2018-05-12 11:16:28,081: Done.
2018-05-12 11:16:28,084: Computing free energy differences...
2018-05-12 11:16:30,663: Done.
2018-05-12 11:16:30,664: Computing covariance matrix...
2018-05-12 11:18:35,459: Deltaf_ij:
```
Specifically, [this line](https://github.com/choderalab/yank/blob/master/Yank/multistate/multistateanalyzer.py#L1231) takes approximately ten minutes, regardless of the length of the output file:
```
end_states = self._get_end_thermodynamic_states()
```
The call to [`_get_end_thermodynamic_states()`](https://github.com/choderalab/yank/blob/master/Yank/multistate/multistateanalyzer.py#L614-L626) unnecessarily reads all the thermodynamic states and discards all but the first and last. I have no idea why this takes ten minutes---something we have to fix in the future---but this is trivial to speed up by only retrieving the two states we need.

This PR adds a reporter method to just retrieve the end states, which reduces the time for this step from 10 minutes to 1 minute:
```
2018-05-12 12:10:35,976: Retrieving end thermodynamic states...
2018-05-12 12:11:40,464: Isolating restraint force...
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f0671df0ef90`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.