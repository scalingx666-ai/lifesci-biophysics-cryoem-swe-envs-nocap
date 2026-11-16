# Issue

**Title:** Updated reference: needed_improperids -> self.needed_improperids

## Description

This pull request closes issue #20 

I'm not sure if pull requests from "unknown users" are encouraged in this repo, but in any case, I got the following error message while running ``ltemplify.py`` on a system with improper dihedrals:

```bash
File "ltemplify.py", line 3901, in PostProcess3
    for improperid in needed_improperids:
NameError: name 'needed_improperids' is not defined
```

It looks like it can be amended by referring to ``self.needed_improperids`` rather than just ``needed_improperids``. This pull request introduces this change and closes issue #20

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b43bad99672f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.