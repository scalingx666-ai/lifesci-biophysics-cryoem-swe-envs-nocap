# Issue

**Title:** ENH: add TPR position and velocity read support

## Description

Fixes gh-464.

* Add support for reading positions and velocities from GMX `.tpr` files over the full range of `tpx` versions supported by regular `.tpr` topology parsing (`58` through `137`).  We've had an issue open to add this capability for almost a decade now.

Notes for reviewers:

1. Suggestions for removing the use of `global` to track TPR file precision are appreciated.
2. Performance/duplication considerations -- as Richard noted in the cognate issue, the way our topology and coordinate handling code is organized, it may make sense to keep the separation here, but there should at least be opportunity to reduce code duplication if not completely avoid re-reading the topology to seek to the coordinate positions); that said, our `.tpr` reading performance is atrocious anyway and my attempts to fix at gh-4098 have stalled for more than a year, so I suggest we defer performance (and probably even duplication) considerations until after the capability and tests are cemented in.
3. Please help me identify TPR testing scenarios that are missing, and ideally suggest the TPR file(s) I should use to test them.
4. I'll probably deal with `CHANGELOG` and docs changes when it looks like we're close to ready to merge, but I still appreciate suggestions on i.e., locations where doc changes will be needed.




PR Checklist
------------
 - [x] Tests?
 - [ ] Docs?
 - [x] CHANGELOG updated?
 - [x] Issue raised/referenced?

## Developers certificate of origin
- [X] I certify that this contribution is covered by the LGPLv2.1+ license as defined in our [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE) and adheres to the [**Developer Certificate of Origin**](https://developercertificate.org/).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--4873.org.readthedocs.build/en/4873/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10eba92d326e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.