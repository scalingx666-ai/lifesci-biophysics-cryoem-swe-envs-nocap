# Issue

**Title:** Allow multiple experiments to keep running if one NaN's

## Description

Enables multiple and/or combinatorial experiments to run if one of them goes to NaN.
Introduces a special error which can be trapped and sent up the stack from a sampler through the phase and experiment. Should also print out to the end of the logger a warning to the user if any NaN.

~This has NOT been tested yet and should NOT be merged, but I want to get the implementation discussion going while I figure out the correct way to test this. I'll strike this line and remove the [DNM] flag once its formally tested.~

*edited:* Added the test so this can be merged when everything passes now.

,

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `134e1f078f5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.