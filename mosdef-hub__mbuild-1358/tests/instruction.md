# Issue

**Title:** Performance: Add polymer repeat units in a batch rather than one-at-a-time

## Description

### PR Summary:
Hello! I was using the `Polymer.build()` routine to generate large polymer chains and noticed the performance scaled poorly with chain length. Inspecting the source code, I saw that monomers are added one-by-one using the `Compound.add()` method. There is a [comment](https://github.com/mosdef-hub/mbuild/blob/develop/mbuild/compound.py#L736-L738) in `Compound` that indicates you can pass in an iterable of `Compounds` to improve performance. 

I modified the [Polymer.build()](https://github.com/mosdef-hub/mbuild/blob/develop/mbuild/polymer.py#L236) method to pass in a list of all of the monomers, rather than each one individually and saw the following performance improvements.

  | Monomers | Before | After | Speedup |
  | --- | --- | --- | --- |
  | 100 | 0.60 s |  0.29 s |	2.1x |
  | 200 | 1.82 s | 0.45 s | 4.0x |
  | 400 | 7.11 s | 0.76 s | 9.4x |
  | 800 | 26.39 s | 1.89 s | 14.0x |
  | 1000 | 41.78 s | 2.04 s | 20.5x |

I identified this with the assistance of Codex profiling building larger systems and saw most of the time was spent in repeatedly calling `NetworkX.compose()` through calling `Compound.add()` multiple times.

### Questions for the maintainers:
1. Should I file an issue for this? I didn't originally since the LOC change is small and we are using an existing code path in mBuild.
2. What would you like to see in terms of unit tests for this change? The behavior shouldn't be changed, all previous tests still pass, this is just a performance improvement.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d77c6da10493`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.