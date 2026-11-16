# Issue

**Title:** Fix build for OpenMM RCs

## Description

Our automated check against new OpenMM release candidates picked up a new RC last night, and the build [is failing](https://github.com/openpathsampling/openpathsampling/runs/4517804647?check_suite_focus=true) with a conda `UnsatifiableError`. I'm going to try to fix this on our side. Among other things, it seems like there's a weird combination trying to pull in `python=3.10` while also pulling in `pypy3.6`, which seems like an obvious source of conflict. We don't currently test against 3.10, so I'm guessing that's the issue (this fails before we try to install the OpenMM RC, so I suspect the problem due to changes in the GitHub Actions environment, not due to the RC itself).

I'll do a little on this now, but if my first tries don't solve, I'll come back to it later in the day (after 19:00 GMT).

The offending conda command in [line 17 in the "Install requirements" stage](https://github.com/openpathsampling/openpathsampling/runs/4517804647?check_suite_focus=true#step:5:17):

```
conda install -y -q -c conda-forge -c omnia --override-channels python=3.10   future psutil numpy scipy pandas netcdf4 svgwrite networkx matplotlib ujson!=2.*,!=3.*,!=4.0.0,!=4.0.1 mdtraj jupyter openmm!=7.5.1 openmmtools py-plumed pyemma   sqlalchemy!=1.4.0 dill   nose pytest pytest-cov coveralls nbval
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72fedad9ba8b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.