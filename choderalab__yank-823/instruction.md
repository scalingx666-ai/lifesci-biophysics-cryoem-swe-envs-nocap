# Issue

**Title:** Cerberus

## Description

Complete conversion to the `Cerberus` validation package from the `Schema` validation package.

This is a doosey of a PR buy this will improve our input error handling to the point of full clarity as to what went wrong and why. Users should see zero changes to the functionality of the code. As such, I recommend taking two passes through this:
1.  Go through and see the implementation, ignore inefficiencies and just focus on what is being done
2. Make a second pass through for cleanup and implementation optimizations.

If you have to review this in chunks and we can talk about each one, happy to go through that with anyone as well.

cc @davidlmobley who I thank for helping convince us to finally fix this.

 

Also cleans up some old Py2 compatibility issues.

Cry "Havoc!", and let slip the dogs of war!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b8d725e4713`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.