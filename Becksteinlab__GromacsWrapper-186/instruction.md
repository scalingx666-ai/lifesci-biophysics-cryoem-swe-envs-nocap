# Issue

**Title:** additional checks for gmx output when importing

## Description

On some clusters, gmx can install in a way that it says this:
![image](https://user-images.githubusercontent.com/38009554/89211602-cb59ee00-d5ca-11ea-8cc8-65a0b20bcf64.png)

without my additional checks 
`import gromacs`
fails for such an installation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `de73d84f54c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.