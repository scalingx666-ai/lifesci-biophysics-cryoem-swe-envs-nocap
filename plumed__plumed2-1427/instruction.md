# Issue

**Title:** TTSKETCH: optimisation

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

@strandn I have made some code optimization and added a regtest covering the stability of the resulting forces. In a test case using 20 CVs I get x4 better performances (in my case from 50ns/day to 200 ns/day).

##### Target release

master

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [x] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [x] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.


- [ ] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct. 

##### Tests

- [x] I added a new regtest or modified an existing regtest to validate my changes.
- [x] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9f2f564f3eb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.