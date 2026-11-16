# Issue

**Title:** Support Python 3.10; Python 3.11

## Description

This is a follow-up on #1132, and should only be merged after that one.

This PR will bring OPS tests in line with the [NEP29](https://numpy.org/neps/nep-0029-deprecation_policy.html) guidance on Python version support: namely, we will drop anything before 3.9, and support everything after 3.9 that is also supported by released versions of our major dependencies (typically involved waiting on support in the OpenMM ecosystem to filter through).

To-do:

- [x] Test on Python 3.10
- [x] Test on Python 3.11

Due to major changes in opcodes in Python 3.11, we're using SimStore's `CallableCodec` to serialize callables in Python 3.11, even in netcdfplus storage. This has the side effect that we needed to change the `CallableCodec`'s output to be base64-encoded so that it was serializable by `ujson` (SimStore's JSON codec ecosystem knows how to store raw bytes; `ujson` does not.) This means that SimStore storages will change, although we should still be able to load the old objects. It also means that `dill` is now a strict requirement for OPS (technically only on Python 3.11, but we'll include it in packaging everywhere).

A few extra to-do's as a result:

* ~Add warning that on Python 3.11 you should probably switch to SimStore~ [I decided against this. It has always been the case that you could use different code paths for serialization in different Python versions. This is just a *much* different code path. I'll probably add `FutureWarning` on `Storage` after the next release.]
- [x] Add warning when loading old SimStore storages that the serialization has changed
- [x] Add test to ensure we can load old SimStore storages

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b2809c19b10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.