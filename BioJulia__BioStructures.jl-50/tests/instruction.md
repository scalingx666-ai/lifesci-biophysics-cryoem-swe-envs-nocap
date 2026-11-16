# Issue

**Title:** Speed up parsing of PDB line

## Description

By checking the length of the line before parsing it, we can avoid the
overhead of `try/catch` for the optional fields.

PDBTools writes data into the optional fields like "charge", so
```julia
julia> using PDBTools

julia> PDBTools.writePDB(PDBTools.wget("6HN6"), "test.pdb")
```

gives a file that on `master` yields
```julia
julia> @btime read("test.pdb", BioStructures.PDB);
  49.759 ms (44601 allocations: 2.09 MiB)
```

and on this branch yields
```julia
julia> @btime read("test.pdb", BioStructures.PDB);
  2.581 ms (40373 allocations: 1.96 MiB)
```

This is ~WIP~RFC because some of the tests reach into internals to check the error message for specific fields when supplied a too-short string. Thus a decision is needed about whether we're OK with the modified approach here; if so, perhaps those tests can just be deleted.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a70f6f48830f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.