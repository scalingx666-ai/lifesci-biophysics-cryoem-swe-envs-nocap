# Issue

**Title:** Exact PME electrostatics treatment and fast computation of reduced potentials

## Description

Fix #319.

I've run some `timeit` tests, and reading the charges from the `CustomNonbondedForce` is actually pretty fast so I don't think implementing the caching system for the charges is worth it. With
```python
import timeit

def read_charges(system):
    original_charges_force = None
    for force in system.getForces():
        if (isinstance(force, openmm.CustomNonbondedForce) and
                    force.getEnergyFunction() == '0.0;'):
            original_charges_force = force

    # Set alchemical atoms charges.
    _, alchemical_atoms = original_charges_force.getInteractionGroupParameters(0)
    for atom_idx in alchemical_atoms:
        original_charge = original_charges_force.getParticleParameters(atom_idx)[0]

test_system = testsystems.SrcExplicit()
factory = AbsoluteAlchemicalFactory(alchemical_pme_treatment='exact')

# Alchemically modify first 150 atoms of Src.
alchemical_region = AlchemicalRegion(alchemical_atoms=range(150))
alchemical_system = factory.create_alchemical_system(test_system.system, alchemical_region)
globals = {
    'read_charges': read_charges,
    'alchemical_system': alchemical_system,
}
number = 1000
tot_time = timeit.timeit('read_charges(alchemical_system)', globals=globals, number=number)
print('read_charges: time for {} executions: {} s (time per execution {} s)'.format(number, tot_time, tot_time / number))
```
I got on the cluster (CUDA platform)
```
read_charges: time for 1000 executions: 0.13602294865995646 s (time per execution 0.00013602294865995646 s)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0359f680ec97`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.