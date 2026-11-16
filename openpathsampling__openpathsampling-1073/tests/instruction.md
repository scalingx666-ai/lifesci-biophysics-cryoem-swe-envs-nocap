# Issue

**Title:** Switch CI install to mamba

## Description

Third (and, I hope, final!) version of this.  and #1071 for the history of trying to debug this problem.

Main points, quoting from previous:

> Py27 builds have taken too long for a while, and now they're so bad that they're failing CI. (20+ minutes was bad, but then it jumped to 2 hours -- and now it takes more than 6 hours?!?!?!) I don't know why this is getting worse, but this is insane.
> 
> In this PR, I'm going to try to switch to [mamba](https://github.com/mamba-org/mamba). I've personally been using mamba for a while now. I tested locally and could build a Py27 environment for OPS in a few minutes, using the same command that hangs in CI. (The conda version of the same command is _still_ trying to run -- the problem isn't with GitHub actions, since it affects me, too.)

However, that change resulted in a segfault that appears to have been because a test didn't close a storage before removing the file (which is fixed in this PR).

In addition to fixing that segfault and switching to `mamba` for installation, this PR:

* Adds a commented debugging step (allowing one to SSH into the worker) to the main CI workflow.
* Improves the requirements installation in several ways, including giving a way to override/replace conda installs with pip installs, and using a more consistent (and purely conda-forge) installation procedure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a760afe29c0d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.