# Issue

**Title:** Add new restraints Application layer interface

## Description

Note: This PR is being merged into the new `multirestraint` branch. Much like the SAMS/MultiStateSampler branch, I wanted to make a branch we can all work on that is not master as we start this process, comment, and review as we would normal PRs. I have not configured travis to run on this branch yet, but my local tests are passing. 

* Added new `restraints` block to the YAML
    * Exactly the same as it used to be in `experiments` block but with unique ID string
    * Added new `name` key which can be used to target custom `lambda_name` variables, optional and defaults to `restraints`. Non-unique to let multiple restraints be controlled by same variable.
    * Block is 100% optional
* `experiments` block allows `restraint: target` to target restraint in `restraints` block.
    * Supports old style to preserve backwards compatibility
    * Supports targeting a single restraint from the `restraints` block
    * Supports a list of targets in the `restraints` block to use multi-restraints
* Updated and added tests for new application layer feature
    * Note: Tests should pass but there is no underlying mechanism to use this yet
* Updated docs
    * New `restraints` doc page
    * Updated `experiments` page with behavior changes to the `restraint` keyword
    * Updated index pages to include new page and pointers.

A key thing: These application layer changes should be 100% compatible with the older versions. So we should not have to have users change their YAML files like we did with the MultiStateSampler conversion. That said, this is still a major new feature and the start of the 0.24.0 release.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd87df0a1dc4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.