# Issue

**Title:** Monte-Carlo for molecules

## Description

This PR adds some simple Monte-Carlo functionality i.e., Metropolis Monte-Carlo for molecules.  I was initially thinking of writing the simulator in `simulators.jl` but there may be some monte-carlo specific code (like the random translation trial moves here) that doesn't fit there so I added a new file.

Some things that need to be done before merge:

- [x] Use of neighbor finder for calculating potential energy at each step?
- [ ] Each step uses `copy()` which is not very efficient, any suggestions for improvement?
- [x] Example simulations (one example given below)
- [x] Docs and Tests

I have run a test simulation given below:
```julia
n_atoms = 100
atom_mass = 10.0u"u"
atoms = [Atom(mass=atom_mass, charge=10, σ=0.3u"nm", ϵ=0.2u"kJ * mol^-1") for i in 1:n_atoms]
boundary = CubicBoundary(2.0u"nm", 2.0u"nm", 2.0u"nm")
coords = place_atoms(n_atoms, boundary; min_dist=0.3u"nm")
pairwise_inters = (Coulomb(),)

temp_mult = [1598.0u"K", 1198.0u"K", 798.0u"K", 398.0u"K", 198.0u"K", 98.0u"K", 8.0u"K"]
sys = System(
	  atoms=atoms,
	  pairwise_inters=pairwise_inters,
	  coords=coords,
	  boundary=boundary,
	  loggers=(coords=CoordinateLogger(n_atoms),),
)

trial_args = Dict(:shift_scaling => 0.1, :length_units => u"nm")
for t in temp_mult
    sim = MetropolisMonteCarlo(; 
		    temperature=t,
		    trial_moves=random_normal_translation!,
		    trial_args=trial_args
	    )
    
    simulate!(sys, sim, 20000)
end
```

This is what the simulation looks like:


https://user-images.githubusercontent.com/59474411/199914858-3d8d077e-9701-4ad5-abcd-48c06018acc4.mp4

As expected the atoms are arranging into a lattice (sort of). Another simulation that was suggested by @jgreener64 was to sample the torsional angle of ethane. I am not quite sure how to set that up so some help will be appreciated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5cf8e423f39d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.