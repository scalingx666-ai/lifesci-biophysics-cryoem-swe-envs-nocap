# Issue

**Title:** Feature/atomic distribution usability

## Description

<!--
  Feel free to delete not relevant sections below
-->

##### Description

<!-- describe here your pull request -->
I added a few new modificators for the atomic distributions:
fix that fixes the state of a distribution, can be strided (useful with cube, sphere, globs)
box that forces the box for the simulations, useful with some file reades that do not read the box correcly
wiggle that wiggles the atoms, useful with line, fcc, sc, bcc and variants

I compacted some of the option from the benchmark in the the --atom-distribution.

I added an extra --modify-trajectory to modify also a loaded file, and 
--ad-help that shows:

```
Documentation for the atomic distributions:

These are the basic atomic distributions:
    line - A line of atoms, that wiggles randomly at each step

    cube - Randomly displaced atoms in a cube, the density is circa 1/unit^3

  sphere - Atoms are displaced uniformly in a sphere, the desity is ccirca 1/unit^3

   globs - Two spheres of uniformly distribuited atosm

      sc - Simple Cubic

     bcc - Body Centered Cubic

     fcc - Face Centered Cubic

    ibcc - BCC, but in a cubic box

    ifcc - FCC, but in a cubic box


These are the modifiers that can be applied to any atomic distributions:
   reply - replicate the box by the given in all the directions:
usage:
  reply x y z

   scale - scales the atoms by a certain amount
usage:
  scale (optional: mults=2.0)

  wiggle - displaces all the atoms by a certain amount
usage:
  wiggle (optional: sphere radius=0.1)

     box - Forces a box on the atom distribution
usage:
  box xx yy zz
 or
  box xx xy xy yx yy yz zx zy zz

     fix - Fixes the trajectory for stride steps, or forever if stride is 0
usage:
  fix (optional stride=0)


To use these distributions in the benchmark you can simple the cli option --atom-distribution
like:
  --atom-distribution="sc", with no extra modifiers
  --atom-distribution="line|wiggle 0.5", use the pipe for modify the base distribution
  --atom-distribution="ifcc|scale 2.3|reply 2 1 1", do not add spaces before or after the pipes

As an extra tool you can append more modificators with --modify-trajectory,
it is not necessary since you can use --atom-distribution, but can be applied
to a trajectory read from a file, for example "box" can be used for adding
the pbcs to certain file parsers.
Remember that --modify-trajectory accepts only  modifiers, with the
same syntax of --atom-distribution
  --modify-trajectory="box 5 5 5"
  --modify-trajectory="box 0 1 1 1 1 0 1 0 1|reply 4 2 6"
```

The way AtomDistribution.cpp is organized acan be applied to other similar functionalities that have a register and are contained in a single cpp file, like the switching functions


##### Target release

<!-- please tell us where you would like your code to appear (e.g. v2.4): -->
I would like my code to appear in release __V2.11__

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6a51a09bf07`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.