# Issue

**Title:** add Xygauss surface

## Description

@sschott

I merged the changes here, and now you can clone the repo and checkout this branch. 

The package compiles. Can you give some simple example of the usage of this constraint, so I can add to the tests?

I have tested this:

```
filetype pdb
output xzgauss.pdb

tolerance 2.0

structure CLA.pdb
    number 50
    inside box -50 -50 -50 50 50 50
    over xygauss 21.0 5.0 10.0 20.0 -23.0 15.0
end structure
```

where CLA.pdb is:

```
HEADER
HETATM    1 CLA  CLA    2        0.000   0.000   0.000
```

but I got this error:

```
Program received signal SIGFPE: Floating-point exception - erroneous arithmetic operation.

Backtrace for this error:
#0  0x7f2d132c1ad0 in ???
#1  0x7f2d132c0c35 in ???
#2  0x7f2d12fd151f in ???
        at ./signal/../sysdeps/unix/sysv/linux/x86_64/libc_sigaction.c:0
#3  0x7f2d13227a00 in xflow
        at ../sysdeps/ieee754/dbl-64/math_err.c:40
#4  0x7f2d131eef02 in __GI___exp
        at ./w_exp_template.c:32
#5  0x559bef155cbd in comprest_
        at src/comprest.f90:170
#6  0x559bef17703d in computef_
        at src/computef.f90:78
#7  0x559bef0df8c4 in initial_
        at src/initial.f90:211
#8  0x559bef168e61 in packmol
        at app/packmol.f90:686
#9  0x559bef16bd6a in main
        at app/packmol.f90:36
Floating point exception (core dumped)
```

Is there some parameter missing in the example of the xygauss surface? Or do you see any other obvious problem?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5fa9b3e44325`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.