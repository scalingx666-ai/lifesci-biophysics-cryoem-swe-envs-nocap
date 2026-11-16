# Issue

**Title:** Add atom selection via atom_indices to SASA computation (try #2, three years after)

## Description

A little holiday coding for something I had working locally for 3 years and never pushed (# was closed as stale/wontfix). It's been crucial for all my projects all this time.

Here's the original PR message:
> Limit the SASA computation to some atoms of interest via atom_indices when calling md.shrake_rupley
> 
> The excluded atoms are still considered as potential surface accessibility blockers, but their own SASA doesn't get computed and gets set to zero instead.
> 
> This is particularly helpful for membrane-embedded proteins where you want the lipids "there" but you don't really care about their SASA.
>  

I followed @rmcgibbo 's suggestion for the c++ implementation. 

The return SASA-value for 'excluded' atoms/residues is set to np.nan in 9b0d5b81021e0eabb42958462281073d5caf77d0. I can revert that if you prefer a 0 value.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4c9bb6e8bc7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.