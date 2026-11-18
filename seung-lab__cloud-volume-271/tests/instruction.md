# Issue

**Title:** sets ACL by default for s3 interface

## Description

On S3 when a user from a separate account puts objects into a S3 bucket that they do not own, the default behavior is that the actual owner of that bucket has no privileges to those objects.  This is in my experience rarely desired behavior and can be quite tedious to fix after the fact.

This PR attempts to set a default ACL for objects created using cloud volume.

This is not generalized and might be necessary for other clouds as well.

Also, would need to be propagated through the code in order for a user to be able to supply a custom ACL, in the case that they actually want ACL to limit owner control.

Happy to to more work on this as needed but wanted to check in first to make sure it was something wanted or if you had some other idea how to tackle this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd7c5fd8a61f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.