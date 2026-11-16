# Issue

**Title:** Make `UnAssignedChemistryInPDBError` hints clearer

## Description

This is my entry in the competition mentioned in #1918

Before:

```
UnassignedChemistryInPDBError: Some bonds or atoms in the input could not be 
identified.

Hint: The following residue names with unassigned atoms were not found in the
substructure library. While the OpenFF Toolkit identifies residues by matching
chemical substructures rather than by residue name, it currently only supports
the 20 'canonical' amino acids.
    198
    SO4


Hint: The following residues were assigned names that do not match the residue
name in the input, or could not be assigned residue names at all. This may
indicate that atoms are missing from the input or some other error. The OpenFF
Toolkit requires all atoms, including hydrogens, to be explicit in the input to
avoid ambiguities in protonation state or bond order:
    Input residue B:SO4#0001 contains atoms matching substructures {'No match'}
    Input residue B:198#0002 contains atoms matching substructures {'No match'}
    Input residue D:NA#0001 contains atoms matching substructures {'Na'}
    Input residue D:NA#0002 contains atoms matching substructures {'Na'}
    Input residue D:NA#0003 contains atoms matching substructures {'Na'}
    Input residue D:NA#0004 contains atoms matching substructures {'Na'}
    Input residue D:NA#0005 contains atoms matching substructures {'Na'}
    Input residue D:NA#0006 contains atoms matching substructures {'Na'}
    Input residue D:NA#0007 contains atoms matching substructures {'Na'}
    Input residue D:NA#0008 contains atoms matching substructures {'Na'}
    Input residue D:NA#0009 contains atoms matching substructures {'Na'}
    Input residue D:NA#0010 contains atoms matching substructures {'Na'}
    Input residue D:NA#0011 contains atoms matching substructures {'Na'}
    Input residue D:NA#0012 contains atoms matching substructures {'Na'}
    Input residue D:NA#0013 contains atoms matching substructures {'Na'}
    Input residue D:NA#0014 contains atoms matching substructures {'Na'}
    Input residue D:NA#0015 contains atoms matching substructures {'Na'}
    Input residue D:NA#0016 contains atoms matching substructures {'Na'}
    Input residue D:NA#0017 contains atoms matching substructures {'Na'}
    Input residue D:NA#0018 contains atoms matching substructures {'Na'}
    Input residue D:NA#0019 contains atoms matching substructures {'Na'}
    Input residue D:NA#0020 contains atoms matching substructures {'Na'}
    Input residue D:NA#0021 contains atoms matching substructures {'Na'}
    Input residue D:NA#0022 contains atoms matching substructures {'Na'}
    Input residue D:NA#0023 contains atoms matching substructures {'Na'}
    Input residue D:NA#0024 contains atoms matching substructures {'Na'}
    Input residue D:NA#0025 contains atoms matching substructures {'Na'}
    Input residue D:NA#0026 contains atoms matching substructures {'Na'}
    Input residue D:NA#0027 contains atoms matching substructures {'Na'}
    Input residue D:NA#0028 contains atoms matching substructures {'Na'}
    Input residue D:NA#0029 contains atoms matching substructures {'Na'}
    Input residue D:CL#0030 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0031 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0032 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0033 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0034 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0035 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0036 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0037 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0038 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0039 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0040 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0041 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0042 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0043 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0044 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0045 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0046 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0047 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0048 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0049 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0050 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0051 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0052 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0053 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0054 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0055 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0056 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0057 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0058 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0059 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0060 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0061 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0062 contains atoms matching substructures {'Cl'}
    Input residue D:CL#0063 contains atoms matching substructures {'Cl'}

Error: The following 34 atoms exist in the input but could not be assigned
chemical information from the substructure library:
    Atom  4049 (S) in residue B:SO4#0001
    Atom  4050 (O1) in residue B:SO4#0001
    Atom  4051 (O2) in residue B:SO4#0001
    Atom  4052 (O3) in residue B:SO4#0001
    Atom  4053 (O4) in residue B:SO4#0001
    Atom  4054 (C16) in residue B:198#0002
    Atom  4055 (C17) in residue B:198#0002
    Atom  4056 (C18) in residue B:198#0002
    Atom  4057 (F18) in residue B:198#0002
    Atom  4058 (C19) in residue B:198#0002
    Atom  4059 (C20) in residue B:198#0002
    Atom  4060 (C15) in residue B:198#0002
    Atom  4061 (S14) in residue B:198#0002
    Atom  4062 (O14) in residue B:198#0002
    Atom  4063 (O15) in residue B:198#0002
    Atom  4064 (C13) in residue B:198#0002
    Atom  4065 (C11) in residue B:198#0002
    Atom  4066 (O11) in residue B:198#0002
    Atom  4067 (C12) in residue B:198#0002
    Atom  4068 (C10) in residue B:198#0002
    Atom  4069 (O10) in residue B:198#0002
    Atom  4070 (N9) in residue B:198#0002
    Atom  4071 (C1) in residue B:198#0002
    Atom  4072 (C2) in residue B:198#0002
    Atom  4073 (C6) in residue B:198#0002
    Atom  4074 (C5) in residue B:198#0002
    Atom  4075 (C4) in residue B:198#0002
    Atom  4076 (C8) in residue B:198#0002
    Atom  4077 (N8) in residue B:198#0002
    Atom  4078 (C3) in residue B:198#0002
    Atom  4079 (C7) in residue B:198#0002
    Atom  4080 (F7B) in residue B:198#0002
    Atom  4081 (F7C) in residue B:198#0002
    Atom  4082 (F7A) in residue B:198#0002
```

After:
```
UnassignedChemistryInPDBError: Some bonds or atoms in the input could not be
identified.

Hint: The following residue names with unassigned atoms were not found in the
substructure library. While the OpenFF Toolkit identifies residues by matching
chemical substructures rather than by residue name, it currently only supports
the 20 'canonical' amino acids.
    198
    SO4


Hint: The following residues were assigned names that do not match the residue
name in the input, or could not be assigned residue names at all. This may
indicate that atoms are missing from the input or some other error. The OpenFF
Toolkit requires all atoms, including hydrogens, to be explicit in the input to
avoid ambiguities in protonation state or bond order:
    Input residue B:SO4#0001 contains atoms matching substructures {'NO MATCH'}
    Input residue B:198#0002 contains atoms matching substructures {'NO MATCH'}

Error: The following 34 atoms exist in the input but could not be assigned
chemical information from the substructure library:
    Atom  4049 (S) in residue B:SO4#0001
    Atom  4050 (O1) in residue B:SO4#0001
    Atom  4051 (O2) in residue B:SO4#0001
    Atom  4052 (O3) in residue B:SO4#0001
    Atom  4053 (O4) in residue B:SO4#0001
    Atom  4054 (C16) in residue B:198#0002
    Atom  4055 (C17) in residue B:198#0002
    Atom  4056 (C18) in residue B:198#0002
    Atom  4057 (F18) in residue B:198#0002
    Atom  4058 (C19) in residue B:198#0002
    Atom  4059 (C20) in residue B:198#0002
    Atom  4060 (C15) in residue B:198#0002
    Atom  4061 (S14) in residue B:198#0002
    Atom  4062 (O14) in residue B:198#0002
    Atom  4063 (O15) in residue B:198#0002
    Atom  4064 (C13) in residue B:198#0002
    Atom  4065 (C11) in residue B:198#0002
    Atom  4066 (O11) in residue B:198#0002
    Atom  4067 (C12) in residue B:198#0002
    Atom  4068 (C10) in residue B:198#0002
    Atom  4069 (O10) in residue B:198#0002
    Atom  4070 (N9) in residue B:198#0002
    Atom  4071 (C1) in residue B:198#0002
    Atom  4072 (C2) in residue B:198#0002
    Atom  4073 (C6) in residue B:198#0002
    Atom  4074 (C5) in residue B:198#0002
    Atom  4075 (C4) in residue B:198#0002
    Atom  4076 (C8) in residue B:198#0002
    Atom  4077 (N8) in residue B:198#0002
    Atom  4078 (C3) in residue B:198#0002
    Atom  4079 (C7) in residue B:198#0002
    Atom  4080 (F7B) in residue B:198#0002
    Atom  4081 (F7C) in residue B:198#0002
    Atom  4082 (F7A) in residue B:198#0002
```

- [x] [Lint](https://open-forcefield-toolkit.readthedocs.io/en/latest/developing.html#style-guide) codebase
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5cbfe6063f5f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.