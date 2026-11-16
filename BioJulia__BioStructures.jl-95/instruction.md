# Issue

**Title:** Add AlphaFold DB download functions

## Description

I extracted this from https://github.com/HolyLab/GPCRAnalysis.jl/, as it is general to any protein. But if you don't think this is the right home, I can find an alternate.

This agent-generated commit extends the version I had written in GPCRAnalysis with better metadata-fetching, a `downloadalphafold(f::Function, args...; kwargs...)` form, and a download-and-read command. I assume the functional form was inspired by `downloadpdb(f, ...)`; Claude wanted me to tell you 😆 that this version downloads to a temporary directory and thus will never overwrite and then delete a file you already have saved in a local directory.

Claude's commit message follows.

-----------------

`alphafoldentry` returns the AlphaFold DB API metadata for a model, `downloadalphafold` downloads the latest model version, and `retrievealphafold` downloads and reads it, following the corresponding PDB functions. Requests send a BioStructures user agent because the AlphaFold DB file server rejects the Downloads.jl default. JSON is added as a dependency to parse API responses.

Example J in the docs now uses `retrievealphafold`, since AlphaFold DB no longer serves the v4 model it downloaded.

Assisted-by: Claude Opus 5.5 <noreply@anthropic.com>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ecad27693bff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.