# Issue

**Title:** 309 make pip test work in m1 mac

## Description

Ok, I got it to work now by changing `glob.glob` to `Path.glob`, these two should be the same but `Path.glob` works fine.
I tried 
pip install -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple alchemlyb=="2.0.1"
and then
pytest --pyargs alchemlyb

Where all tests passed fine.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `639969e57834`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.