# Issue

**Title:** Improve speed of GROMOS11 reader

## Description

<!-- 
Insert issue number that this PR fixes (if any) just after 'Fixes #'.
If this PR does not fix an existing issue, consider opening one or remove 'Fixes #' from the PR description.
-->


The GROMOS11 trajectory reader written by @JoStoe was merged in #4292 (see issue #4291 )

The performance in the current state is very slow

Changes made in this Pull Request:
<!-- Describe the changes that this PR makes. If applicable, use the following bullet list. --> 

- Use seek to skip over the POSITIONRED block when creating the Universe
- Prevent calling strip() on the same line multiple times
- Improved reading speed of the POSITIONRED block by using a buffer and processing with numpy.fromstring()
- Pre-allocate numpy arrays instead of reading into list() and calling numpy.asarray()
- Add the GROMOS11 reader to the benchmark set
- Add a test with a trajectory containing inconsistent whitespace

All-in-all this provides a significant speed boost. Internal testing showed the following speed increase when loading and iterating over a 1 GB trajectory of 2000 frames:

Before these changes: 86.97 s +/- 1.13 s (n=10)
With changes in this PR: 42.05 s +/- 0.66 s (n=10)
Speed improvement -51.6%

On the newly added benchmark the following speed improvement was measured:
traj_reader.TrajReaderCreation.time_reads: 5.339 ms -> 3.579 ms (-33.0%)
traj_reader.TrajReaderIteration.time_strides: 7.642 ms -> 4.524 ms (-40.8%)

(Note that the benchmark trajectories are very small which reduces the impact on the TrajReaderCreation.time_reads benchmark. Based on testing on larger trajectories, the time for the Universe creation is reduced by 70-80% if the file has consistent block sizes)

Most of the speed improvement comes from the skipping over the POSITIONRED block, meaning that it is not necessary to read the full trajectory upon universe creation. 
However, note that when the size of the POSITIONRED block is not consistent (whitespace), the speed is impacted.

## PR Checklist
 - [x] Issue raised/referenced?
 - [x] Tests updated/added?
 - [ ] Documentation updated/added?
 - [x] `package/CHANGELOG` file updated?
 - [x] Is your name in `package/AUTHORS`? (If it is not, add it!)

I don't think that the documentation needs to be updated.

## Developers Certificate of Origin

I certify that I can submit this code contribution as described in the [**Developer Certificate of Origin**](https://developercertificate.org/), under the MDAnalysis [LICENSE](https://github.com/MDAnalysis/mdanalysis/blob/develop/LICENSE).


<!-- readthedocs-preview mdanalysis start -->
----
📚 Documentation preview 📚: https://mdanalysis--5080.org.readthedocs.build/en/5080/

<!-- readthedocs-preview mdanalysis end -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b113fd5193f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.