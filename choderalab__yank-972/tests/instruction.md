# Issue

**Title:** Make restoration and last iteration more robust

## Description

This is the first of a couple changes to make checkpoint restoration and access more robust.

Next step will to be make `MultiStateAnalyzer` able to detect corruption and back up X number of checkpoints. I split the PRs to minimize amount which has to be reviewed and to set the underlying structure first.

I also changed some logic on masking maximum number of available iterations to prevent unassociated data from being accessed, cut down on some lines.

@jchodera @andrrizzi any other exposed methods/functionality you think we need before I try to make the analyzer able to handle data corruption better?

@andrrizzi what would be the effect in the `MultiStateAnalyzer` if I changed the other `CachedProperty` objects to depend on `max_n_iterations`?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12c6ec14ae8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.