# Issue

**Title:** Handle exceptions raised during PythonForce deserialization

## Description

If unpickling a PythonForce's computation function raises a Python exception, then deserializing the PythonForce causes a segmentation fault, since in

<https://github.com/openmm/openmm/blob/ba0aa236ac4d07241382869604c4d2ea58c4f3f5/wrappers/python/src/swig_doxygen/swig_lib/python/pythonforce.i#L167>

`function` will be null, and it is subsequently passed to

<https://github.com/openmm/openmm/blob/ba0aa236ac4d07241382869604c4d2ea58c4f3f5/wrappers/python/src/swig_doxygen/swig_lib/python/pythonforce.i#L17>

This change handles the exception like `ComputationWrapper::compute` does.

I found this when trying to work with an ML potential library that raised an exception while unpickling a model under certain circumstances, so it's not contrived that a user could run into the problem, and this will hopefully make the resulting debugging experience easier.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ba0aa236ac4d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.