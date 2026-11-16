# Issue

**Title:** Derive atom bonds without the Graphs extension

## Description

Add `atombonds(chain; strict)`, which returns a chain's covalently bonded atom pairs from the residue templates in `residuedata`. This is the bond derivation that previously lived inside `MetaGraph(chain)` in the Graphs extension, extracted so callers who want connectivity but not a graph type -- force fields, internal-coordinate parametrizations -- can reach it without depending on Graphs.jl and MetaGraphs.jl. `MetaGraph(chain)` now delegates to it, unchanged in behavior.

Bonding follows from residue and atom names, so `atombonds` never consults coordinates: a distorted or clashing conformation yields the same bonds as the deposited one.

Fix a latent double-bond: the C-terminal `C`-`OXT` special case also fired for the C-terminal variant templates (`CALA`, `CSER`, ...), whose entries already list that bond. `add_edge!` silently absorbed the duplicate in graph form; the raw pair list did not, so `atombonds` now adds the special-case bond only when the residue's own template omits it.

The derivation code goes in the hand-written `src/atombonds.jl` rather than `src/bonding.jl`, which `extractdata/bonding.jl` regenerates wholesale; that generator now also stamps a do-not-edit header onto its output.

Assisted-by: Claude Code (Opus 4.8)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2e804614e2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.