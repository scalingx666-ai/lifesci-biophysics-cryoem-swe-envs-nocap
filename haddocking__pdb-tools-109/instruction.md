# Issue

**Title:** Adjust fhandle names to files and iterables.

## Description

In #107 the `run` function was documented to be used either providing a `file handler` or an `iterator` scrolling over the `PDB` lines. However in `pdb_split*` and `pdb_tocif` the `run` function defines the output files basenames given the `.name` attribute of the `file handler`. `iterables` won't have this attribute. This PR corrects that by providing a default name, `output`, when the attribute `.name` is not present in `fhandler`. Also, if `run` is used directly, the user can define `outname` specifically, instead of relying on the default values.

You will see a new function  `get_fname` in the `__init__.py`. This function is imported where it is needed. I don't think this adds any overhead to the execution, and keeps things clean.

I am keeping this PR as draft while I write the tests, so that you can comment on it already. But we should merge asap.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97fe1dc30e65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.