# Issue

**Title:** Repair dps_extended.get_new_detector

## Description

This is to fix  Please look at the discussion there.

@phyy-nx 
> Yes dropping the else path makes sense to me.

In this PR I kept the old (`else`) code path because the unification would change the behaviour (whether to store the shift in the hierarchy root or panels). Please feel free to delete the old branch if you think we don't need to preserve the old behaviour.

> For the hierarchy root there should be no difference between set_frame and set_local_frame, right?

Right. Both should do the same for the root. I nonetheless changed it to `set_frame`, because `get_origin` etc is semantically global.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5dcfaa4e1d76`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.