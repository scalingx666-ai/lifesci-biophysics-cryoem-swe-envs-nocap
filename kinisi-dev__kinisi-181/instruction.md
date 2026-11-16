# Issue

**Title:** add `YehHummer` finite size corrections

## Description

- fixes 

This PR introduces a way to correct for finite-size effects as described by Yeh and Hummer in https://pubs.acs.org/doi/10.1021/jp0477147.

<img width="500" height="360" alt="image" src="https://github.com/user-attachments/assets/3cf364e4-4c4a-4392-9f82-e7eeb500fc3d" />
from https://pubs.acs.org/cms/10.1021/jp0477147/asset/images/medium/jp0477147f00002.gif

<img width="500" height="1440" alt="image" src="https://github.com/user-attachments/assets/1583e77f-0340-4560-ba44-7d660f2ba851" />

```python
# TIP3P water data from Yeh & Hummer paper
box_lengths = np.array([18.58, 23.42, 29.51, 37.19, 46.86])  # Angstroms
D_values = np.array([4.884e-5, 5.123e-5, 5.315e-5, 5.466e-5, 5.590e-5])  # cm^2/s
D_errors = np.array([0.032e-5, 0.027e-5, 0.014e-5, 0.011e-5, 0.013e-5])  # cm^2/s

td = sc.DataArray(
    data=sc.array(
        dims=["system"], values=D_values, variances=D_errors**2, unit="cm^2/s"
    ),
    coords={
        "box_length": sc.Variable(
            dims=["system"], values=box_lengths, unit="angstrom"
        )
    },
)

yh = YehHummer(td, temperature=298)  # K
yh.mcmc(n_samples=500, n_walkers=16)

yh.plot()
```



### TODOs
- [x] tests
- [x] documentation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3aed39a7bae5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.