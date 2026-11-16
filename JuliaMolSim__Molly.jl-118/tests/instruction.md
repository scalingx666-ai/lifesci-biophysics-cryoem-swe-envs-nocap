# Issue

**Title:** Hotfix for Julia 1.9

## Description

Using the new extension system, it seems the `dualize` function defined in `FordwardDiff`'s `ForwardDiffStaticArraysExt` extension module is not importable (at least I could not manage to do it).

However, since the signature defined here (in Molly) is only used from within Zygote, I supposed it's safe to simply define it independent of `ForwardDiff`'s `dualize`. 
Maybe this `dualize` could/should be called `offset_dualize` to clearly distinguish it?

Using this little trick allowed me to load Molly on 1.9.0rc1.
However, I couldn't run `Pkg.test` because it fails due to some GL stuff not working (running it on a remote node without even an display server) and I was not using the differentiable capabilities so far, so I cannot say anything about correctness.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2076b0bba0b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.