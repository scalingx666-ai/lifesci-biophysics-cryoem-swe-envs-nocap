# Issue

**Title:** parse query on Select object creation

## Description

This PR makes the use of the selection string `@sel_str` much faster, because the query is parsed on the construction of the object, and not on the application of the query to each atom. For example:

Before:

```julia-repl
julia> struc = retrievepdb("4YC6")

julia> @btime collectatoms($struc, $(sel"name CA"));
  37.468 ms (605588 allocations: 16.10 MiB)
```

After:

```julia-repl
julia> @btime collectatoms($struc, $(sel"name CA"));
  889.474 μs (41122 allocations: 2.06 MiB)
```

Also, the `Select` object has a custom show method:

```julia-repl
julia> sel"name CA and resnum 1"
Select("name CA and resnum 1")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a98a59d77dc4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.