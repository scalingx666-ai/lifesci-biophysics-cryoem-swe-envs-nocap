# Issue

**Title:** Show the solution only once

## Description

The solution for missing files and directories appears twice.

```
NOTE: The global configuration file '/home/peu/.gromacswrapper.cfg' is missing.
      You can create a basic configuration file from within python:
      >>> import gromacs
      >>> gromacs.config.setup()
NOTE: Some configuration directories are not set up yet
      ['/home/peu/.gromacswrapper', '/home/peu/.gromacswrapper/qscripts', '/home/peu/.gromacswrapper/templates', '/home/peu/.gromacswrapper/managers']
      You can create them with the command from within Python
      >>> import gromacs
      >>> gromacs.config.setup()
```

With this commit it appears only once:

```
NOTE: The global configuration file '/home/peu/.gromacswrapper.cfg' is missing.
NOTE: Some configuration directories are not set up yet:
    /home/peu/.gromacswrapper
    /home/peu/.gromacswrapper/qscripts
    /home/peu/.gromacswrapper/templates
    /home/peu/.gromacswrapper/managers
NOTE: You can create the configuration file and directories with:
    >>> import gromacs
    >>> gromacs.config.setup()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `01dcc382d087`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.