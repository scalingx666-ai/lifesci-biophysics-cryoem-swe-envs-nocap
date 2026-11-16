# Issue

**Title:** Fix issue #270

## Description

This PR  by using a list rather than a set to store the names of protein and nucleic acid residues. This makes the search string for backbone atoms reproducible, hence the tests now repeatedly pass. I'm still not sure why this issue has only cropped up recently, since `_prot_res` has been a set since the code was first added.

```python
In [1]: x = {"1", "2", "3"}

In [2]: ",".join(x)
Out[2]: '2,3,1'

In [3]: x = ["1", "2", "3"]

In [4]: ",".join(x)
Out[4]: '1,2,3'
```

* I confirm that I have merged the latest version of `devel` into this branch before issuing this pull request (e.g. by running `git pull origin devel`): [y]
* I confirm that I have permission to release this code under the GPL3 license: [y]

## Suggested reviewers:
@chryswoods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9e5423725fdc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.