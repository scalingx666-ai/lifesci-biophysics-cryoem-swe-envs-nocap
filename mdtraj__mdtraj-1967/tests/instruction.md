# Issue

**Title:** Store and write formal charges from PDB/OpenMM

## Description

This PR adds capabilities to store formal charge from OpenMM and write to PDB , addressing #1956.

* A `formal_charge` attribute to the `Atom` class that is set to `None` by default. I've added it as the last argument (and it is optional), so this should not break any existing code. 
* When reading an OpenMM topology, the atom object is inspected for the `formalCharge` attribute. If present, it is read from the OpenMM topology and stored in the MDTraj topology (`Atom` with `formal_charge` attribute mentioned above).
* The PDB writer is modified to write the formal charge as the last two columns in an ATOM entry. If `formal_charge` is `None` for the atom, two spaces are written. Otherwise, the charge value is written.
* **While completing this PR, I also stumbled on a bug with reading the charge from PDB**. The charge was not likely to ever be read correctly for two reasons (1) the string associated with a charge was cast to `int`. This would not succeed in the case that the charge was format {number}{sign}, which is common. (2) The charge read from the PDB was not used in the internal topology (perhaps this is obvious since the attribute didn't exist before). Both of these have been corrected.
* Tests have been added for the PDB charge reading/writing, and for checking that formal charges from OpenMM ->MDTraj topology are preserved.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2e0f5a4dcc2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.