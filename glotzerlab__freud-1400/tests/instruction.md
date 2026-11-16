# Issue

**Title:** Add additional unit cells to `freud.data.UnitCell`

## Description

<!-- Provide a general summary of your changes in the Title above. -->

## Description

Additional unit cells for common crystals have been added:

- [x] Simple and Centered rectangular (2d Bravais lattices)
- [x] Oblique (2d bravais lattice)
- [x] Graphene (two-dimensional non-bravais lattice)

The result of this is users can now construct all 2d bravais lattices by default. Graphene is included so we have a nontrivial 2d example structure available as well.

## How Has This Been Tested?

New tests have been added that verify the output.

### Images

#### Graphene
<img width="600" height="425" alt="image" src="https://github.com/user-attachments/assets/8b5d1f64-76c4-4d50-b4c0-4af2c5feed1c" />

#### Rectangular
##### Default
<img width="600" height="425" alt="image" src="https://github.com/user-attachments/assets/fe998d58-fe25-4258-91b9-3a3f519ab722" />

##### Centered = True
<img width="600" height="425" alt="image" src="https://github.com/user-attachments/assets/3eb980a5-3494-40e2-8035-ad57dd745979" />


#### Oblique

##### Default:
<img width="600" height="409" alt="image" src="https://github.com/user-attachments/assets/45ab8a07-f797-402d-8e51-03e41de0e906" />

##### Aspect=2
<img width="600" height="425" alt="image" src="https://github.com/user-attachments/assets/82da6243-720b-402d-8806-29cc210ea7e7" />


## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [x] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99f48e6eda04`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.