# Issue

**Title:** Auto-Multi-Analysis

## Description

This PR implements 2 major improvements:

1. All YANK "Automatic Analysis" codes go through a singular, common class: the `YankAutoExperimentAnalyzer`. So the API, the Notebooks, and the CLI all invoke and work through the same class. Even the print functions now parse the regularized output. This does change some of the keys you may be used to in other codes.
2. It is now possible to analyze every experiment defined through 1 yaml file through the API and CLI with the `MultiExperimentAnalyzer`, which also supports MPI parallelization. 

The `YankAutoExperimentAnalyzer` can output a singular Python dictionary of all the analysis values it tracked (e.g. if invoked through API, you can get all the numbers instead of dumping to command line), and the `MultiExperimentAnalyzer` can combine those outputs together into one dictionary and serialize to a pickle object, ready for use by downstream, post-yank programs. 

I think I caught all the corner cases, and this update should be drop-in replaceable to any other API code. There might be some CLI code which needs changed though. 

The API docs should be fixed and I also fixed the OpenEye block in `yank selftest` while I was tinkering with the CLI.

There is alot here, but I could not think of how to make it smaller since its all inter-dependent, so it will take some time to go over.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `395ca353f447`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.