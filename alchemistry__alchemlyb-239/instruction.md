# Issue

**Title:** add R_c (convergence fraction) convergence analysis

## Description

Fix #104 

I have reimplemented the R_c implemented by @VOD555.
I have also implemented the A_c based on my understanding of the paper.
 
I have done a speed check, for a 32 window simulation where each window has 10,000 data points.

```
from alchemlyb.convergence import A_c
import time

data = pd.Series(data=range(10000))
data.attrs['temperature'] = 310
data.attrs['energy_unit'] = 'kcal/mol'
a=time.time()
value = A_c([data, ] * 32)
b=time.time()
```

Takes 0.01 second, so I think the performance is satisfying.

Includes commits from 
- Fix dhdl2series and u_nk2series would not reattach the unit #248
- Change the plot_convergence so that it could take output from R_c #249
- Remove the 'dhdl' method in decorrelating the u_nk #250

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3809fdf2a5a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.