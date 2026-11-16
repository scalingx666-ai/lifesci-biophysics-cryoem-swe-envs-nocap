# Issue

**Title:** Report the specific C++ standard at init time, stop warning about C++97/03

## Description

We are using enough C++11 features now that it would be impossible for anybody to build without it. On the other hand, we may be having a similar situation with newer C++ standards.

I'm proposing to just print out the raw value of `__cplusplus` at init time, while also warning that this might be inaccurate on Windows builds. Folks use VMD much more than other codes on Windows, but VMD never took in my patch to fix the reporting problem:
https://github.com/Colvars/colvars/blob/2d9ca52ef05d83eee638690c01cd7a73c21edee5/cmake/CMakeLists.txt#L69-L70
https://learn.microsoft.com/en-us/cpp/build/reference/zc-cplusplus?view=msvc-170

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2d9ca52ef05d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.