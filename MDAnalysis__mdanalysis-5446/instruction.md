# Issue

**Title:** [fmt] Automatically renormalize line endings to \n (LF)

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->


Regardless of whether #5445 gets merged in due to AI usage or not, the scope of that PR is to check whether bad commits went through. This preemptively deals with problems before commit + combs through existing files. This PR should be merged before #5445 (since this will solve CI failure on that PR anyways) should #5445 be deemed still needed. 

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 
- Added a `.gitattributes` file to enforce line endings to `\n` (LF). All files within `/testsuite/MDAnalysisTests/data` are outside of the scope.
- Ran `git add --renormalize .` to comb over existing files and implement the `.gitattributes` changes. 

With this PR, when staging a file with CRLF, the following message will be displayed:
```warning: in the working copy of '/package/MDAnalysis/blahblah', CRLF will be replaced by LF the next time Git touches it```
And the changes will be implemented during commit.


## LLM / AI generated code disclosure
<!-- Please update this disclosure to reflect if you did or did not use LLMs / AI to generate code -->
LLMs or other AI-powered tools (beyond simple IDE use cases) were used in this contribution: <strike>no</strike>

I guess this is a "yes" now that I read the copilot AI overview on GitHub and implemented changes (not exact) based on what was suggested? Otherwise all done manually (i.e. "no").

## PR Checklist
<!-- Please use the following checklist to ensure the PR is ready to be reviewed/merged. -->
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)
 - [x] LLM/AI disclosure was updated.

## Developers Certificate of Origin
<!-- 
In order for this PR to be merged you **must certify that you are able to submit your code to be included in MDAnalysis**.
-->

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


## Other Considerations:
Thought about just listing `.py` and `.pyx` but then there's the `.c` and `.h` and `.ini` and `.txt` (and the list goes on with `README` and `CHANGELOG`...). Might as well let git figure it out instead.

All files in `testsuite/MDAnalysisTests/data/` are excluded from the renormalization.
<strike>
Also for transparency, there were some problems with the following files which required me to manually remove `\r` (don't know why--they were pesky and behavior very much reproducible with `git reset --hard HEAD~1` and rerunning `git add --renormalize .`) but otherwise should be good now. Rerunning `git add --renormalize .` after the manual replacement in a fresh clone does not retrigger this problem so I assume it's a one-time thing. 

```
	modified:   testsuite/MDAnalysisTests/data/adk_closed_NAMD.psf
	modified:   testsuite/MDAnalysisTests/data/gms/surf2wat.gms
	modified:   testsuite/MDAnalysisTests/data/gms/symopt.gms
	modified:   testsuite/MDAnalysisTests/data/gms/symopt.inp
	modified:   testsuite/MDAnalysisTests/data/mol2/BrokenMolecule.mol2
	modified:   testsuite/MDAnalysisTests/data/mol2/Molecule.mol2
	modified:   testsuite/MDAnalysisTests/data/mol2/Molecules.mol2
```
</strike>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c882e5cd20d4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.