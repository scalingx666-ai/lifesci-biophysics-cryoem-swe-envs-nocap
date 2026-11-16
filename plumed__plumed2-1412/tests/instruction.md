# Issue

**Title:** Adding the FCC and BCC atomic distributions to the collection

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

<!-- describe here your pull request -->
While the benchmarks are running I made this little addition to the AtomDistribution utility.

I added the more dense FCC and less dense BCC atomic distribution, that can be used with the benchmark.

The new distributions can be used "as is" (fcc or bcc) or with a "i" variant (ibcc or ifcc):
The "i" variants create the distributions in a orthogonal box, whereas the non-"i" variants can be used to stress also the pbc in more exotic boxes


**bcc**
<img width="500" height="500" alt="bcc" src="https://github.com/user-attachments/assets/f555b2fb-decb-40b3-a03c-8e0da4a2b8ef" />

**fcc**
<img width="500" height="500" alt="fcc" src="https://github.com/user-attachments/assets/1d8a98fc-d5a6-4a39-8e60-97b922ed741d" />

**ibcc**
<img width="500" height="500" alt="ibcc" src="https://github.com/user-attachments/assets/2a49fdc3-2504-4f71-aaf3-1015e54b0f4c" />

**ifcc**
<img width="500" height="500" alt="ifcc" src="https://github.com/user-attachments/assets/6dc70897-b179-48d5-8f3e-7beb9d43034c" />

##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __v2.11__

##### Type of contribution

<!--
  Please select the type of your contribution among these:
  (Change [ ] to [X] to tick an option)
-->
- [x] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b1771655af6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.