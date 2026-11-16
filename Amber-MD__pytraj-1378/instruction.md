# Issue

**Title:** fix git version writing

## Description

For unknown reasons (that I don't want to go deeper), this failed after switching to git submodule.
Only affect if making AmberTools tarfile.

https://travis-ci.org/Amber-MD/ambertools-ci/jobs/231353763


```bash
fatal: Not a git repository: ../../../.git/modules/AmberTools/src/pytraj

Traceback (most recent call last):

  File "setup.py", line 100, in <module>

    write_version_py()

  File "/home/travis/build/Amber-MD/ambertools-ci/amber16/AmberTools/src/pytraj/scripts/base_setup.py", line 120, in write_version_py

    FULLVERSION, GIT_REVISION = get_version_info()

  File "/home/travis/build/Amber-MD/ambertools-ci/amber16/AmberTools/src/pytraj/scripts/base_setup.py", line 88, in get_version_info

    GIT_REVISION = git_version()

  File "/home/travis/build/Amber-MD/ambertools-ci/amber16/AmberTools/src/pytraj/scripts/base_setup.py", line 64, in git_version

    out = subprocess.check_output(['git', 'rev-parse', 'HEAD'])

  File "/home/travis/build/Amber-MD/ambertools-ci/amber16/miniconda/lib/python2.7/subprocess.py", line 219, in check_output

    raise CalledProcessError(retcode, cmd, output=output)

subprocess.CalledProcessError: Command '['git', 'rev-parse', 'HEAD']' returned non-zero exit status 128

make[2]: *** [pytraj] Error 1

make[2]: Leaving directory `/home/travis/build/Amber-MD/ambertools-ci/amber16/AmberTools/src'

make[1]: *** [serial] Error 2

make[1]: Leaving directory `/home/travis/build/Amber-MD/ambertools-ci/amber16/AmberTools/src'

make: *** [install] Error 2
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e589d669bd40`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.