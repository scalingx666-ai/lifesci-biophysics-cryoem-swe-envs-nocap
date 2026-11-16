# Issue

**Title:** ADD: hbond _get_bridge for solvent bridge

## Description

private for now since I don't know how to nicely display the data yet.


```python
        traj = pt.iterload(fn('tz2.ortho.nc'), fn('tz2.ortho.parm7'))
        # find solvent bridge between residue 10 and 11.
        hb = pt.hbond(
            traj, ':10,11', solvent_donor=':WAT', solvent_acceptor=':WAT')
        assert hb._get_bridge() ==  \
         [[('WAT208', 'THR10', 'TRP11')],
          [('WAT208', 'THR10', 'TRP11')],
          [('WAT208', 'THR10', 'TRP11')],
          [('WAT208', 'THR10', 'TRP11')],
          [],
          [],
          [],
          [],
          [],
          [('WAT266', 'THR10', 'TRP11')]]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7cb105ed5f3e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.