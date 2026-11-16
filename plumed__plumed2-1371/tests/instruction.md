# Issue

**Title:** Skipping the setup of the request list if the NL if stride is 1

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

This change speeds up the use of NL as a linked cells algorithm with stride set to 1

It changes the prepare step to acknowledge the change

Since I saw that the prepare step with the NL is in a few places, I also compacted that step in a method in the NL to propagate this, and any future changes.

I touched some non core module: maze and s2cm

---

Here's the effect of this code on a simple call on COORDINATION:


| run name                                   | steps | total time | time in calculate |
|--------------------------------------------|-------|------------|-------------------|
| no NL                                      | 200   | 42.081126  | 41.768990         |
| using Linkcells with stride=1              | 200   | **14.712104**  | 14.356958         |
| using the Neighborlist with stride=1        | 200   | *47.810684*  | 47.455588         |
| using the Neighborlist with stride=40       | 200   | 2.487337   | 2.002722          |


| run name                                   | steps | total time | time in calculate |
|--------------------------------------------|-------|------------|-------------------|
| no NL                                      | 200   | 42.524496  | 42.202752         |
| using Linkcells with stride=1              | 200   | **1.948613**   | 1.634255          |
| using the Neighborlist with stride=1        | 200   | *40.747972*  | 40.427439         |
| using the Neighborlist with stride=40       | 200   | 2.554561   | 2.034981          |

the simulation run with the following settings, with 16000 atoms, each in a different run:
```
WO1: GROUP ATOMS=@mdatoms

no_nl: COORDINATION ...
 GROUPA=WO1
 SWITCH={RATIONAL NN=6 MM=12 D_0=0.6 R_0=1 D_MAX=2.0}
 NLISTCELLS NL_CUTOFF=2.0 NL_STRIDE=1
...

cells: COORDINATION ...
 GROUPA=WO1
 SWITCH={RATIONAL NN=6 MM=12 D_0=0.6 R_0=1 D_MAX=2.0}
 NLISTCELLS NL_CUTOFF=2.0 NL_STRIDE=1
...

nl_st_1: COORDINATION ...
 GROUPA=WO1
 SWITCH={RATIONAL NN=6 MM=12 D_0=0.6 R_0=1 D_MAX=2.0}
 NLIST NL_CUTOFF=3.5 NL_STRIDE=1
...

nl_st_40: COORDINATION ...
 GROUPA=WO1
 SWITCH={RATIONAL NN=6 MM=12 D_0=0.6 R_0=1 D_MAX=2.0}
 NLIST NL_CUTOFF=3.5 NL_STRIDE=40
...
```

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __v2.11__

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [x] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [x] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

<!--
  In case you picked one of the first two choices
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

<!--
  In case you picked the third choice (new module authored by you)
  MAKE SURE TO TICK ALSO THE FOLLOWING BOX
-->

- [ ] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct. 

##### Tests

<!--
  Make sure these boxes are checked. For Travis-CI tests, you can wait for them
  to be completed monitoring this page after your pull request has been submitted:
  http://travis-ci.org/plumed/plumed2/pull_requests
-->

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!--
  After your branch has been merged to the desired branch and then to plumed2/master, and after the
  plumed official manual has been updated, please check out the coverage scan at
  http://www.plumed.org/coverage-master
  In case your new features are not well covered, please try to add more complete regtests.
-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `274e4ce3b804`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.