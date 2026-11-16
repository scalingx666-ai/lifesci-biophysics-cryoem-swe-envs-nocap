# Issue

**Title:** Fix GB radii when bonds are constrained

## Description

Hi! I noticed that implicit solvent radii change when constraints are turned on. mbondi2/mbondi3 give hydrogens bonded to nitrogen the `H_N` radius, but the bonds passed to the implicit solvent setup are the ones left after constraints are taken out. With `constraints=:hbonds` no hydrogen looks bonded to anything, so they all get the plain `H` radius.

I have used Claude code to identify the problem and to help identify a fix.

### Reproducer

```julia
using Molly

data_dir = joinpath(pkgdir(Molly), "data")
ff = MolecularForceField(joinpath(data_dir, "force_fields", "ff99SBildn.xml"); units=false)

build(solvent, constraints) = System(joinpath(data_dir, "6mrr_nowater.pdb"), ff;
    boundary=CubicBoundary(100.0), units=false, implicit_solvent=solvent,
    constraints=constraints)

for solvent in (SetupImplicitSolventOBC(), SetupImplicitSolventGBN2())
    radii(c) = build(solvent, c).general_inters[1].offset_radii
    println(nameof(typeof(solvent)), ": ",
            count(radii(:none) .!= radii(:hbonds)), " atoms change radius with :hbonds")
end

sys = build(SetupImplicitSolventOBC(), :allbonds)
inject_gradients(sys, Molly.extract_parameters(sys, ff))
```

On master this prints 140 (OBC) and 105 (GBn2) changed atoms, then fails with `MethodError: no method matching getindex(::Tuple{...}, ::Nothing)`.
With this PR both counts are 0 and `inject_gradients` works.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e35e3127f564`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.