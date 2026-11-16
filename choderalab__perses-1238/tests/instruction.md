# Issue

**Title:** Fix incorrect implementation of logsumexp

## Description

* Off by a_n.max() for the final value, needs to be added back in after the call to log
* Use the scipy.special.logsumexp implementation (used elsewhere) rather than re-implement

Doesn't look like `MultiTargetDesign` is used, so feel free to close this PR. Just happened to see the bug when reading the code. 

## Description

Noticed that a custom implementation of logsumexp was incorrect, as it didn't add back in `a_n.max()`.

## Motivation and context

Incorrect implementation, will produce non-normalized log probabilities for the class `MultiTargetDesign`

<!-- Replace ??? with the issue number that this pull request resolves. -->
No associated case

## How has this been tested?

Tested by the following python script
```
import numpy as np
from numpy.typing import NDArray
from scipy.special import logsumexp


def old_log_sum_exp(vals: NDArray):
    return np.log(np.sum(np.exp(vals - vals.max())))


rng = np.random.default_rng(2023)

log_prob = rng.normal(size=1000, loc=1000)

assert not np.all(np.isclose(np.sum(np.exp(log_prob)), 1.0))

# Verify that the logsumexp from scipy produces a normalized log probability
norm_log = log_prob - logsumexp(log_prob)
print("Sum of probabilities (scipy.special.logsumexp)", np.sum(np.exp(norm_log)))
# Sum of probabilities (scipy.special.logsumexp) 0.9999999999999626
assert np.all(np.isclose(np.sum(np.exp(norm_log)), 1.0))

bad_norm_log = log_prob - old_log_sum_exp(log_prob)
print("Sum of probabilities (old logsumexp)", np.sum(np.exp(bad_norm_log)))
# Sum of probabilities (old logsumexp) inf
assert np.all(np.isclose(np.sum(np.exp(bad_norm_log)), 1.0)), np.sum(
    np.exp(bad_norm_log) 
)  # Fails

```

## Change log


<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Fix log probability normalization in `MultiTargetDesign`
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c6419f96878`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.