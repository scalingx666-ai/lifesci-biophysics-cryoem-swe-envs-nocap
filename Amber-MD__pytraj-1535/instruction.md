# Issue

**Title:** remove warnings about numpy

## Description

When running the test, warnings about numpy occur, such as the following.
So, I have modified it so that there is no warning for the future.

```
  pytraj/pytraj/parallel/dataset.py:39: DeprecationWarning: Calling np.sum(generator) is deprecated, and in the future will give a different result. Use np.sum(np.fromiter(generator)) or the python sum builtin instead.
    mat = np.sum((val[0] * val[1]
  pytraj/pytraj/parallel/dataset.py:46: DeprecationWarning: Calling np.sum(generator) is deprecated, and in the future will give a different result. Use np.sum(np.fromiter(generator)) or the python sum builtin instead.
    mat = np.sum((val[0][1] * val[1]
  pytraj/pytraj/parallel/dataset.py:48: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    vecs = np.column_stack(val[0][0] for val in self.data)
  pytraj/pytraj/parallel/dataset.py:55: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    mat = np.row_stack(val[0][0] for val in self.data)
  pytraj/pytraj/parallel/dataset.py:56: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    rmsd_ = np.hstack(val[0][1] for val in self.data)
  pytraj/pytraj/parallel/dataset.py:60: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    mat = np.row_stack(val[0] for val in self.data)
  pytraj/pytraj/parallel/dataset.py:63: DeprecationWarning: Calling np.sum(generator) is deprecated, and in the future will give a different result. Use np.sum(np.fromiter(generator)) or the python sum builtin instead.
    xyz = np.sum((x[1] * x[0].xyz

  pytraj/pytraj/utils/get_common_objects.py:267: FutureWarning: elementwise comparison failed; returning scalar instead, but in the future will perform elementwise comparison
    if has_mask_arg and mask == '':

  pytraj/pytraj/utils/tools.py:312: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    return np.vstack((f.xyz.copy() for f in iterables))
  pytraj/pytraj/utils/tools.py:327: FutureWarning: arrays to stack must be passed as a "sequence" type such as list or tuple. Support for non-sequence iterables such as generators is deprecated as of NumPy 1.16 and will raise an error in the future.
    xyz = np.vstack((f.xyz.copy() for f in iterables))

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76ff89dba930`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.