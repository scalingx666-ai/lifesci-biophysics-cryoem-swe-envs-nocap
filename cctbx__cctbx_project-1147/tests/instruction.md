# Issue

**Title:** libtbx: fix wrong reading mode when unpickling temporary file

## Description

When trying to submit a Slurm job in Phenix 2.0, the job submission crashes with
```
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
  File "/software/presto/e/sebth/software/Phenix/2.0-5936-foss-2023a/phenix-2.0-5936/build/../conda_base/lib/python3.9/codecs.py", line 322, in decode
    (result, consumed) = self._buffer_decode(data, self.errors, final)
UnicodeDecodeError: 'utf-8' codec can't decode byte 0x80 in position 0: invalid start byte
```
due to libtbx attempting to read a pickle in text mode instead of binary mode. Applying the commit in this PR to libtbx in our local Phenix 2.0 installation fixes the job submission.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7d8958a7b4c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.