# Issue

**Title:** flat interface intrinsic distance fix

## Description

should fix #288 

a simple check:

    import pytim
    import numpy as np
    from pytim.datafiles import WATER_GRO
    import MDAnalysis as mda
    u = mda.Universe(WATER_GRO)
    u.atoms[:8].positions=np.array([[0.,0,5],[0,1,5],[1,0,5],[1,1,5],
                [0.,0.,-5.],[0,1,-5],[1,0,-5],[1,1,-5]])
    upper,lower=u.atoms[:4],u.atoms[4:8]
    inter = pytim.SimpleInterface(u,symmetry='planar', upper=upper,
                lower=lower,alpha=5.)
    g = u.atoms[8:12]
    g.atoms.positions=np.asarray([[.5,.5,6],[.5,.5,4],[.5,.5,-4],
                [.5,.5,-6]])
    print(pytim.observables.IntrinsicDistance(inter).compute(g))
        
    [ 1. -1. -1.  1.]
"""

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85413ba6797d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.