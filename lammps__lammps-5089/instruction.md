# Issue

**Title:** Add switch to temporarily turn off chemistry in DPD-REACT

## Description

**Summary**

The idea is that during an initial equilibration phase, the modeling of reaction chemistry is turned off, so that when reaction chemistry is turned back on for a production run, it starts from a realistic state. This switch is activated via `fix_modify`, as shown in the documentation update as part of the switch. An example usage is shown in `examples/PACKAGES/dpd-react/dpdrx-shardlow/in.dpdrx-shardlow`.

**Author(s)**

@jjramsey James J. Ramsey, from DEVCOM Army Research Repository, Physical Modeling and Simulation Branch.

Current work e-mail is james.j.ramsey14.civ@army.mil
Personal e-mail is jjramsey@pobox.com

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Artificial Intelligence (AI) Tools Usage**

<!--Please update the following sentence as needed in case you were using AI tools to
generate code.  Please mention which specific AI tool or tools you were using and which
parts of the code were AI generated versus the parts written by a human.  The use of AI
tools for checking spelling and syntax does not need to be reported.-->

By submitting this pull request, I confirm that I did NOT use any AI tools to generate
all or parts of the code and modifications in this pull request.

**Backward Compatibility**

This should be backwards compatible. Since the chemistry is turned on and off via `fix_modify`, it does not effect how the fix has been initially set up.

**Implementation Notes**

This is implemented by adding a boolean `skipChemistry`, which is initialized to `false` (thus keeping backward compatibility), and an override to the virtual function `modify_param()`. It was testing by comparing the results from it to the results from a previous approach to turning off chemistry that was only used internally at ARL.

**Post Submission Checklist**

<!--Please check the fields below as they are completed **after** the pull request has been submitted. Delete lines that don't apply-->

- [ ] The feature or features in this pull request is complete
- [ ] Licensing information is complete
- [ ] Corresponding author information is complete
- [ ] The source code follows the LAMMPS formatting guidelines
- [ ] Suitable new documentation files and/or updates to the existing docs are included
- [ ] The added/updated documentation is integrated and tested with the documentation build system
- [ ] The feature has been verified to work with the conventional build system
- [ ] The feature has been verified to work with the CMake based build system
- [ ] Suitable tests have been added to the unittest tree.
- [ ] A package specific README file has been included or updated
- [ ] One or more example input decks are included

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `21fba3965ba2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.