# Issue

**Title:** Add check for solvent_model vs parameter mismatch

## Description

This adds a check for the solvent_model parameter vs what files were loaded to TLEaP. Its a bit crude, but provides better handling of the error and tries to inspect the LEaP log for the error it knows.

Its a 2-fold trap:
First it warns the user if they have not loaded the expected leaprc file, warning looks like:
```
WARNING - yank.pipeline - WARNING: The solvent_model tip4pew may not work for loaded leaprc.water.X files.
 We expected leaprc.water.tip4pew to make your solvent model work, but did not find it.
 This is okay for tip4pew leaprc file with tip3p solvent_model, but not the other way around.
 ```

 Second, if the system fails to build, the logfile is searched for a common match error an throws an error with the following message:
 ```
 RuntimeError: Solvent {SOLVENT_ID}: It looks like the water used has virtual sites, but missing parameters.
Make sure your leap parameters use the correct water model as specified by solvent_model.
Please see the log file for LEaP for more info as there may be more wrong:
{ORIGINAL_MESSAGE}
 ```
 Where ORIGINAL_MESSAGE is filled in with the actual error thrown by the `telap.run()` function in `utils`, and SOLVENT_ID is the name of the solvent as it appears in YAML

 I have also updated the FAQ and the example in the yaml pages to use the default. cc @jchodera since this has come up a few times

 Thoughts?

 
 cc #759 as its a related bug, since this sets up the framework for catching other errors like the one there.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e9e85184c6b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.