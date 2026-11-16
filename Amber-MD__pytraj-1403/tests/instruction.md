# Issue

**Title:** ENH: slice on-dist trajectory with velocity will give a on-mem trajectory with velocity

## Description

- Code example

```python
    def test_velocity(self):
        traj = pt.iterload(
            fn('issue807/trunc.nc'), fn("issue807/system.prmtop"))

        aa_eq(pt.get_velocity(traj),
              pt.get_velocity(traj[:]))

        aa_eq(pt.get_velocity(traj, frame_indices=[1, 3, 5]),
              pt.get_velocity(traj[[1, 3, 5]]))
```

- Also (gradually) adapt pytest style

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b1938c863f8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.