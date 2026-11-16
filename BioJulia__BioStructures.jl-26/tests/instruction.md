# Issue

**Title:** Add ProteinStructure constructors from MMCIFDict and MMTFDict

## Description

# Add ProteinStructure constructors from MMCIFDict and MMTFDict 

This adds two new constructors
```julia
ProteinStructure(::MMCIFDict; kwargs...)
ProteinStructure(::MMTFDict; kwargs...)
```

Having these functions allows one to avoid having to parse a mmCIF or MMTF file twice in the case where one wants both the dictionary and the ProteinStructure with the coordinates.

I imagine there is a small performance benefit as well as reduced disk traffic (which can be slow on a networked file system).

Not sure if adding an additional constructor is the right way to do this, but that way i didn't have to introduce new function names.

## Example

```julia
# Example for the mmCIF case
# before
mmcif_dict = MMCIFDict("path/to/file.cif")
struc = read("path/to/file.cif", MMCIF)

# after:
mmcif_dict = MMCIFDict("path/to/file.cif")
struc = ProteinStructure(mmcif_dict)
```

## Benchmark on 4v4g (a rather extreme example)

```julia
julia> @btime (cif = MMCIFDict("4v4g.cif.gz"; gzip=true); struc = read("4v4g.cif.gz", MMCIF; gzip=true));
  8.893 s (49723548 allocations: 3.70 GiB)

julia> @btime (cif = MMCIFDict("4v4g.cif.gz"; gzip=true); struc = ProteinStructure(cif));
  5.762 s (28749625 allocations: 2.28 GiB)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `996de9a43481`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.