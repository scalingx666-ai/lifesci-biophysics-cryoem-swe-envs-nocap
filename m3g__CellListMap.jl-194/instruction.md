# Issue

**Title:** Update neighbor list benchmarks to CellListMap 0.10.8 and plot with P…

## Description

…lots.jl

Rerun the CellListMap.jl entries of the neighbor list comparison with the 0.10.8 release (serial and 8 threads, same pinned cores); other packages' results are unchanged. The serial CellListMap run now uses the same random coordinates as the other packages (pair counts match exactly).

Replace plot.py with plot.jl (Plots.jl/GR, box frames, Computer Modern), regenerate b_nl_cd.png and b_nl_cv.png, and update the README speedups at 10^6 particles (3.2x vs. scipy serial, 4.5x vs. vesin with 8 threads).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8afca02c63ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.