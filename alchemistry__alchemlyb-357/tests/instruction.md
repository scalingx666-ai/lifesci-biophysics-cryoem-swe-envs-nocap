# Issue

**Title:** Speed up the MBAR calculation

## Description

I have adapted @mrshirts 's advice of using BAR results as initial guess into the MBAR, which seems to provide a 5-fold speed up. 
```
from alchemtest.gmx import load_ABFE
from alchemlyb.parsing.gmx import extract_u_nk
import pandas as pd
df_list = [extract_u_nk(data, 310) for data in load_ABFE()['data']['complex']]
df = pd.concat(df_list)
mbar = MBAR(initial_f_k=None)
%timeit mbar.fit(df)
1.66 s ± 7.42 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
mbar = MBAR(initial_f_k='BAR')
%timeit mbar.fit(df)
248 ms ± 2.6 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
I tried to optimise the convergence detection as well, where using the result from the previous MBAR run as input into the next MBAR run. Compared to using BAR as input, this approach still provides some speed up.

```
from alchemlyb.convergence import forward_backward_convergence
%timeit forward_backward_convergence(df_list)
```

Using BAR as initial guess
3.58 s ± 35.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
Using the previous step as initial guess.
2.25 s ± 23.6 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55870c8b1b3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.