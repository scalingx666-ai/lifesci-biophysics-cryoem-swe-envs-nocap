# Issue

**Title:** Block averages

## Description

This PR adds an `AverageObservableLogger` which keeps a running average of some observation, while maintaining an estimation of the standard deviation on this average. 
As an example, the following code 
```julia
using Molly

n_atoms = 1000
atom_mass = 10.0u"u"
atoms = [Atom(mass = atom_mass, σ = 0.2u"nm", ϵ = 0.2u"kJ * mol^-1") for i=1:n_atoms]

# Initialization
box_size = SVector(6.0, 6.0, 6.0)u"nm"
coords = place_diatomics(n_atoms ÷ 2, box_size, 0.2u"nm", 0.2u"nm")

temp = 50.0u"K"
velocities = [velocity(atom_mass, temp)*0.01 for i=1:n_atoms]


# Interaction potentials
pairwise_inters = (SoftSphere(nl_only=true, cutoff=DistanceCutoff(0.6u"nm")),)

bonds = [HarmonicBond(b0=0.2u"nm", kb=10000u"kJ * mol^-1 * nm^-2") for i=1:(n_atoms ÷ 2)]
specific_inter_lists = (InteractionList2Atoms(collect(1:2:n_atoms), collect(2:2:n_atoms), repeat([""], length(bonds)),bonds),)

# Define system
nf = DistanceNeighborFinder(dist_cutoff=0.6u"nm", nb_matrix=trues(n_atoms, n_atoms))


sys = System(atoms=atoms,
    coords=coords,
    velocities=velocities,
    neighbor_finder=nf,
    pairwise_inters=pairwise_inters,
    specific_inter_lists=specific_inter_lists,
    box_size=box_size,
    )
simulator = LangevinSplitting(dt=0.002u"ps", friction=10.0u"u* ps^-1", temperature=temp, splitting="BAOAB")
simulate!(sys,simulator,5000) # equilibriate

sys = System(atoms=atoms,
    coords=coords,
    velocities=velocities,
    neighbor_finder=nf,
    pairwise_inters=pairwise_inters,
    specific_inter_lists=specific_inter_lists,
    box_size=box_size,
    loggers=(avg_temp=AverageObservableLogger(Molly.temperature_wrapper,typeof(1.0u"K"),1),)
    )

simulate!(sys, simulator, 10000)
(t,σ)=values(sys.loggers.avg_temp)
print("t= ",t," ± ",σ)
```
gives as an output
```console
t= 49.79195051999282 K ± 0.07213115483541217 K
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b42b82de636`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.