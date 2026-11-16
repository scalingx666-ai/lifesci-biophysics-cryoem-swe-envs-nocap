# Issue

**Title:** Cap Documenter.jl to 0.19 on Travis

## Description

_Hi! This is an automated pull request by @mortenpi._

The next [Documenter.jl](https://github.com/JuliaDocs/Documenter.jl) release (0.20) will make breaking changes to the `makedocs` and `deploydocs` functions. This PR makes sure that the your Travis CI runs will install and use Documenter 0.19, so that the documentation deployment would not break when 0.20 is released.

Subscribe to JuliaDocs/Documenter.jl#861 to be notified about the 0.20 release. It will have release notes detailing how to upgrade your `make.jl` script. Note that the conversation in that issue is locked, so you should not receive any other notifications from there.

If there is a problem with the PR, please tag @mortenpi.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a1a50925903`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.