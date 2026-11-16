# Issue

**Title:** Make the ParticleSystem pairwise path compatible with juliac --trim

## Description

These changes allow code that uses `ParticleSystem`/`pairwise!` (in particular Packmol.jl's command line app) to be compiled with `juliac --trim=safe`. The trim verifier requires every call reachable from the entry point to be resolved at compile time.

## Changes

- **`show_progress`** (`pairwise!`, `neighborlist`, `InPlaceNeighborList` and the internal `_pairwise!` functions) is now `Union{Nothing,Bool}` and defaults to `nothing`, instead of `false`. With `nothing`, known at compile time, the ProgressMeter code is not compiled at all; with a runtime `Bool` it always was, even if never executed. `InPlaceNeighborList` gets a type parameter for the field. `show_progress = true`/`false` keep working as before.
- **Coordinate validation**: the descriptive error of the default `_validate_coordinates` (which interpolates the coordinates into the message, not trim-compatible) was thrown from `_build!` whenever NaN coordinates were found while locating the particles, even if a custom `validate_coordinates` function had been given. It is now thrown only with the default function: with a custom one (already called before the build) or with `nothing`, nothing is done there (the invalid particles were already placed at the origin). Incidentally, `validate_coordinates = nothing` now actually disables the check, as documented. `_build!` also specializes on `validate_coordinates`, which it only passes through.
- **Task synchronization in cell list construction**: `mapreduce(fetch, &, tasks)` becomes `mapreduce(t -> fetch(t)::Bool, &, tasks)`, and the `@sync` blocks become explicit task vectors with `foreach(wait, tasks)`. (`fetch` on a `Task` is untyped, and `@sync` waits on the untyped elements of a `Channel{Any}`.)
- **`@inline`** on `getproperty(sys::AbstractParticleSystem, s::Symbol)` and `getproperty(p::NeighborPair, s::Symbol)`, which forward to the `Val(s)` methods, so that the property is always constant-folded.

The default behavior is unchanged.

## Testing

- Full test suite passes (2454 tests).
- Packmol.jl's CLI, using this branch, builds with `juliac --trim=safe` (Julia 1.13) with no verifier errors, and the resulting executable packs systems correctly.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

https://claude.ai/code/session_019dbJyEoXHqbEtWi5pxgwuR

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f176615cd26`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.