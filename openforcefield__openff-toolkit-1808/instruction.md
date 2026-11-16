# Issue

**Title:** Implement __repr__ and _repr_pretty_ for ValenceDict

## Description

This just makes it possible to see the parameters of a force field at a glance, rather than have to drill in to a `ValenceDict` by casting it to a regular dict. This has annoyed me before and I hit it again writing a notebook today so I'm fixing it.

I admit the afters are very wordy, but that's consistent with how Python does other large dictionaries and also without this it's very painful to see what's inside a ValenceDict 

## Before

### in Jupyter:

```python
from openff.toolkit import ForceField, Molecule

ff = ForceField("openff-2.1.0.offxml")
mol = Molecule.from_smiles(r"CCCO")

ff.label_molecules(mol.to_topology())
```

```
[{'Constraints': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa5b70>,
  'Bonds': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefccfa0>,
  'Angles': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa7430>,
  'ProperTorsions': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa7280>,
  'ImproperTorsions': <openff.toolkit.topology.topology.ImproperDict at 0x7f93eefa7400>,
  'vdW': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa7850>,
  'Electrostatics': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa67d0>,
  'LibraryCharges': {},
  'ToolkitAM1BCC': <openff.toolkit.topology.topology.ValenceDict at 0x7f93eefa6ad0>}]
```

### in the Python console:

```python
>>> from openff.toolkit import ForceField, Molecule
>>> 
>>> ff = ForceField("openff-2.1.0.offxml")
>>> mol = Molecule.from_smiles(r"CCCO")
>>> 
>>> ff.label_molecules(mol.to_topology())
[{'Constraints': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff60ff1c90>, 'Bonds': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff6124d810>, 'Angles': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff6439fb90>, 'ProperTorsions': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff8df5f110>, 'ImproperTorsions': <openff.toolkit.topology.topology.ImproperDict object at 0x7eff64340b50>, 'vdW': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff669677d0>, 'Electrostatics': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff89608090>, 'LibraryCharges': {}, 'ToolkitAM1BCC': <openff.toolkit.topology.topology.ValenceDict object at 0x7eff643540d0>}]
```

## After

### in Jupyter:

```python
from openff.toolkit import ForceField, Molecule

ff = ForceField("openff-2.1.0.offxml")
mol = Molecule.from_smiles(r"CCCO")

ff.label_molecules(mol.to_topology())
```

```
[{'Constraints': ValenceDict(
    {(0, 4): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (0, 5): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (0, 6): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (1, 7): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (1, 8): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (2, 9): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (2, 10): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >,
     (3, 11): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >}),
  'Bonds': ValenceDict(
    {(0,
      1): <BondType with smirks: [#6X4:1]-[#6X4:2]  id: b1  length: 1.527940216866 angstrom  k: 419.9869268191 kilocalorie / angstrom ** 2 / mole  >,
     (0,
      4): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >,

-------------------<snip>-------------------

     (9,): <vdWType with smirks: [#1:1]-[#6X4]-[#7,#8,#9,#16,#17,#35]  epsilon: 0.01640924602775 kilocalorie / mole  id: n3  rmin_half: 1.449786411317 angstrom  >,
     (10,): <vdWType with smirks: [#1:1]-[#6X4]-[#7,#8,#9,#16,#17,#35]  epsilon: 0.01640924602775 kilocalorie / mole  id: n3  rmin_half: 1.449786411317 angstrom  >,
     (11,): <vdWType with smirks: [#1:1]-[#8]  epsilon: 1.232599966667e-05 kilocalorie / mole  id: n12  rmin_half: 0.2999999999997 angstrom  >}),
  'Electrostatics': ValenceDict({}),
  'LibraryCharges': {},
  'ToolkitAM1BCC': ValenceDict({})}]
```

### in the Python console:

```python
>>> from openff.toolkit import ForceField, Molecule
>>> 
>>> ff = ForceField("openff-2.1.0.offxml")
>>> mol = Molecule.from_smiles(r"CCCO")
>>> 
>>> ff.label_molecules(mol.to_topology())
>>> from openff.toolkit import ForceField, Molecule
>>> 
>>> ff = ForceField("openff-2.1.0.offxml")
>>> mol = Molecule.from_smiles(r"CCCO")
>>> 
>>> ff.label_molecules(mol.to_topology())
[{'Constraints': ValenceDict({(0, 4): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (0, 5): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (0, 6): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (1, 7): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (1, 8): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (2, 9): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (2, 10): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >, (3, 11): <ConstraintType with smirks: [#1:1]-[*:2]  id: c1  >}), 'Bonds': ValenceDict({(0, 1): <BondType with smirks: [#6X4:1]-[#6X4:2]  id: b1  length: 1.527940216866 angstrom  k: 419.9869268191 kilocalorie / angstrom ** 2 / mole  >, (0, 4): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (0, 5): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (0, 6): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (1, 2): <BondType with smirks: [#6X4:1]-[#6X4:2]  id: b1  length: 1.527940216866 angstrom  k: 419.9869268191 kilocalorie / angstrom ** 2 / mole  >, (1, 7): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (1, 8): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (2, 3): <BondType with smirks: [#6:1]-[#8:2]  id: b14  length: 1.423822414975 angstrom  k: 545.2782783431 kilocalorie / angstrom ** 2 / mole  >, (2, 9): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (2, 10): <BondType with smirks: [#6X4:1]-[#1:2]  id: b84  length: 1.090139506109 angstrom  k: 719.6424928981 kilocalorie / angstrom ** 2 / mole  >, (3, 11): <BondType with smirks: [#8:1]-[#1:2]  id: b88  length: 0.981124525388 angstrom  k: 1069.809209734 kilocalorie / angstrom ** 2 / mole  >}), 'Angles': ValenceDict({(0, 1, 2): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (0, 1, 7): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (0, 1, 8): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 0, 4): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 0, 5): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 0, 6): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 2, 3): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 2, 9): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (1, 2, 10): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (2, 1, 7): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (2, 1, 8): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (2, 3, 11): <AngleType with smirks: [*:1]-[#8:2]-[*:3]  angle: 111.9874516071 degree  k: 237.851218935 kilocalorie / mole / radian ** 2  id: a28  >, (3, 2, 9): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (3, 2, 10): <AngleType with smirks: [*:1]~[#6X4:2]-[*:3]  angle: 110.0631999136 degree  k: 121.1883270155 kilocalorie / mole / radian ** 2  id: a1  >, (4, 0, 5): <AngleType with smirks: [#1:1]-[#6X4:2]-[#1:3]  angle: 108.5839257083 degree  k: 75.08254435747 kilocalorie / mole / radian ** 2  id: a2  >, (4, 0, 6): <AngleType with smirks: [#1:1]-[#6X4:2]-[#1:3]  angle: 108.5839257083 degree  k: 75.08254435747 kilocalorie / mole / radian ** 2  id: a2  >, (5, 0, 6): <AngleType with smirks: [#1:1]-[#6X4:2]-[#1:3]  angle: 108.5839257083 degree  k: 75.08254435747 kilocalorie / mole / radian ** 2  id: a2  >, (7, 1, 8): <AngleType with smirks: [#1:1]-[#6X4:2]-[#1:3]  angle: 108.5839257083 degree  k: 75.08254435747 kilocalorie / mole / radian ** 2  id: a2  >, (9, 2, 10): <AngleType with smirks: [#1:1]-[#6X4:2]-[#1:3]  angle: 108.5839257083 degree  k: 75.08254435747 kilocalorie / mole / radian ** 2  id: a2  >}), 'ProperTorsions': ValenceDict({(0, 1, 2, 3): <ProperTorsionType with smirks: [*:1]-[#6X4:2]-[#6X4:3]-[*:4]  periodicity1: 3  phase1: 0.0 degree  id: t1  k1: 0.1526959283148 kilocalorie / mole  idivf1: 1.0  >, (0, 1, 2, 9): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#6X4:4]  periodicity1: 3  phase1: 0.0 degree  id: t4  k1: 0.08586880062944 kilocalorie / mole  idivf1: 1.0  >, (0, 1, 2, 10): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#6X4:4]  periodicity1: 3  phase1: 0.0 degree  id: t4  k1: 0.08586880062944 kilocalorie / mole  idivf1: 1.0  >, (1, 2, 3, 11): <ProperTorsionType with smirks: [#6X4:1]-[#6X4:2]-[#8X2H1:3]-[#1:4]  periodicity1: 3  periodicity2: 1  phase1: 0.0 degree  phase2: 0.0 degree  id: t94  k1: 0.4475764336372 kilocalorie / mole  k2: 0.1222812484103 kilocalorie / mole  idivf1: 1.0  idivf2: 1.0  >, (2, 1, 0, 4): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#6X4:4]  periodicity1: 3  phase1: 0.0 degree  id: t4  k1: 0.08586880062944 kilocalorie / mole  idivf1: 1.0  >, (2, 1, 0, 5): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#6X4:4]  periodicity1: 3  phase1: 0.0 degree  id: t4  k1: 0.08586880062944 kilocalorie / mole  idivf1: 1.0  >, (2, 1, 0, 6): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#6X4:4]  periodicity1: 3  phase1: 0.0 degree  id: t4  k1: 0.08586880062944 kilocalorie / mole  idivf1: 1.0  >, (3, 2, 1, 7): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#8X2:4]  periodicity1: 3  periodicity2: 1  phase1: 0.0 degree  phase2: 0.0 degree  id: t9  k1: 0.1006464828354 kilocalorie / mole  k2: 0.482791305955 kilocalorie / mole  idivf1: 1.0  idivf2: 1.0  >, (3, 2, 1, 8): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#8X2:4]  periodicity1: 3  periodicity2: 1  phase1: 0.0 degree  phase2: 0.0 degree  id: t9  k1: 0.1006464828354 kilocalorie / mole  k2: 0.482791305955 kilocalorie / mole  idivf1: 1.0  idivf2: 1.0  >, (4, 0, 1, 7): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (4, 0, 1, 8): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (5, 0, 1, 7): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (5, 0, 1, 8): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (6, 0, 1, 7): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (6, 0, 1, 8): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (7, 1, 2, 9): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (7, 1, 2, 10): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (8, 1, 2, 9): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (8, 1, 2, 10): <ProperTorsionType with smirks: [#1:1]-[#6X4:2]-[#6X4:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t3  k1: 0.2516073078789 kilocalorie / mole  idivf1: 1.0  >, (9, 2, 3, 11): <ProperTorsionType with smirks: [*:1]-[#6X4:2]-[#8X2:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t93  k1: 1.015112840557 kilocalorie / mole  idivf1: 3.0  >, (10, 2, 3, 11): <ProperTorsionType with smirks: [*:1]-[#6X4:2]-[#8X2:3]-[#1:4]  periodicity1: 3  phase1: 0.0 degree  id: t93  k1: 1.015112840557 kilocalorie / mole  idivf1: 3.0  >}), 'ImproperTorsions': <openff.toolkit.topology.topology.ImproperDict object at 0x7feef12f03d0>, 'vdW': ValenceDict({(0,): <vdWType with smirks: [#6X4:1]  epsilon: 0.1088406109251 kilocalorie / mole  id: n16  rmin_half: 1.896698071741 angstrom  >, (1,): <vdWType with smirks: [#6X4:1]  epsilon: 0.1088406109251 kilocalorie / mole  id: n16  rmin_half: 1.896698071741 angstrom  >, (2,): <vdWType with smirks: [#6X4:1]  epsilon: 0.1088406109251 kilocalorie / mole  id: n16  rmin_half: 1.896698071741 angstrom  >, (3,): <vdWType with smirks: [#8X2H1+0:1]  epsilon: 0.2094735324129 kilocalorie / mole  id: n19  rmin_half: 1.682099169199 angstrom  >, (4,): <vdWType with smirks: [#1:1]-[#6X4]  epsilon: 0.01577948280971 kilocalorie / mole  id: n2  rmin_half: 1.48419980825 angstrom  >, (5,): <vdWType with smirks: [#1:1]-[#6X4]  epsilon: 0.01577948280971 kilocalorie / mole  id: n2  rmin_half: 1.48419980825 angstrom  >, (6,): <vdWType with smirks: [#1:1]-[#6X4]  epsilon: 0.01577948280971 kilocalorie / mole  id: n2  rmin_half: 1.48419980825 angstrom  >, (7,): <vdWType with smirks: [#1:1]-[#6X4]  epsilon: 0.01577948280971 kilocalorie / mole  id: n2  rmin_half: 1.48419980825 angstrom  >, (8,): <vdWType with smirks: [#1:1]-[#6X4]  epsilon: 0.01577948280971 kilocalorie / mole  id: n2  rmin_half: 1.48419980825 angstrom  >, (9,): <vdWType with smirks: [#1:1]-[#6X4]-[#7,#8,#9,#16,#17,#35]  epsilon: 0.01640924602775 kilocalorie / mole  id: n3  rmin_half: 1.449786411317 angstrom  >, (10,): <vdWType with smirks: [#1:1]-[#6X4]-[#7,#8,#9,#16,#17,#35]  epsilon: 0.01640924602775 kilocalorie / mole  id: n3  rmin_half: 1.449786411317 angstrom  >, (11,): <vdWType with smirks: [#1:1]-[#8]  epsilon: 1.232599966667e-05 kilocalorie / mole  id: n12  rmin_half: 0.2999999999997 angstrom  >}), 'Electrostatics': ValenceDict({}), 'LibraryCharges': {}, 'ToolkitAM1BCC': ValenceDict({})}]

```

--------

- [ ] Tag issue being addressed
- [ ] Add [tests](https://github.com/openforcefield/openff-toolkit/tree/main/openff/toolkit/_tests)
- [ ] Update docstrings/[documentation](https://github.com/openforcefield/openff-toolkit/tree/main/docs), if applicable
- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [ ] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.rst)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `13bebaf7e22c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.