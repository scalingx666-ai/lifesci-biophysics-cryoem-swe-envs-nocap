# Issue

**Title:** Overhaul regression test reporting and add result merging tool

## Description

**Summary**

Second step of making automated test results accessible for a planned test status website (companion to #5069, which does the same for the unit test workflows). This PR makes the JUnit XML output of `tools/regression-tests/run_tests.py` complete and semantically correct, fixes several bugs found along the way, and adds a small standard-library-only tool to merge the results of distributed (sharded) runs and generate summaries, including a comparison against a previous run (new failures / fixed tests / new tests / removed tests).

**Related Issue(s)**

Companion to #5069.

**Author(s)**

Axel Kohlmeyer, Temple U (akohlmey at gmail.com)

**Licensing**

By submitting this pull request, I agree, that my contribution will be included in LAMMPS and redistributed under either the GNU General Public License version 2 (GPL v2) or the GNU Lesser General Public License version 2.1 (LGPL v2.1).

**Backward Compatibility**

The command line interface of `run_tests.py` is unchanged. The `progress.yaml`/`failure.yaml` files keep the same per-line "key: {flow mapping}" format as the bundled `reference.yaml` (they were previously written in a format that did not parse as YAML at all). The `junit_xml` Python package is no longer needed. Summary counter semantics changed slightly: runs that complete but cannot be checked (e.g. missing reference log) are no longer counted as "failed", and valgrind runs with detected leaks now are.

**Detailed Description**

*run_tests.py fixes and improvements:*

- **JUnit XML output rewritten** using `xml.etree` (the `junit_xml` PyPI package is unmaintained/archived). Every input script is now enumerated as a test case; previously skipped inputs were missing entirely and completed-but-unchecked runs were misreported as failures. Outcomes now use proper JUnit semantics: `<failure>` = numerical checks failed (with the individual failed quantities in the element body), `<error>` = crash/timeout/missing log file, `<skipped>` = not run or nothing to verify. The measured walltime goes into the `time` attribute, the example folder (relative to `examples/`) into `classname`, and build metadata (git info, compiler, compile flags, config file) into suite `<properties>` - so merged results can be keyed and compared across shards and runs.
- **`--resume` actually works now.** The progress file was written as one flow mapping per line, which does not parse as a single YAML document, so resuming always silently restarted from scratch. Entries are now emitted via `yaml.safe_dump` (quoting-safe against error messages containing quotes - the current `reference.yaml` contains an example of such corruption around `in.read_dump`). Also fixes the `'packaged not installed'` vs `'package not installed'` resume check that could never match.
- **Timeout kills the whole process group.** `subprocess.run(shell=True, timeout=...)` only kills the shell, leaving `mpirun`/LAMMPS children running for the rest of the job. Runs are now started with `start_new_session=True` and killed via `os.killpg()`.
- Statistics are now derived from the recorded per-test results instead of a dozen hand-maintained counters that had drifted (e.g. missing-reference runs were counted both as completed and failed; the stray quote in the failure-file write produced invalid YAML).
- Removed the dead `#REG` marker code paths (hardwired off since 2024); replaced `ls`/`pwd`/`cp` subprocess calls with `glob`/`os`/`shutil`; fixed `TestResult()` ignoring its `checks` argument; valgrind runs with detected leaks are now reported as failed.

*New `tools/regression-tests/merge_results.py`* (standard library only): merges JUnit XML files from sharded runs (tolerates empty/unparseable shards, warns on duplicates), writes a merged JUnit XML file, a JSON archive of all results plus metadata, and a Markdown summary. With `--compare previous-run.json` the summary additionally lists new failures, fixed tests, new tests, and removed tests relative to an earlier run - the JSON files are meant to be archived by downstream reporting so runs can be compared over time.

*Workflow changes* (`full-regression.yml`, `quick-regression.yml`): the per-shard files are uploaded directly as artifacts (no more tar wrapping, and with `if: always()` so results are available exactly when tests fail), and the artifact-merge job is replaced by a job that runs `merge_results.py` and posts the summary table with per-test failure details to the workflow step summary. Scheduling the full regression run and wiring up the run-to-run comparison will follow separately once the downstream reporting location exists.

*Testing:* exercised locally against `examples/melt`, `examples/rerun`, and `examples/neb` with a development LAMMPS binary: verified all five outcome categories (passed; numerical failure via a perturbed reference log; error via missing-replica NEB runs; timeout via a 3 s limit, with no orphaned processes left behind; skipped via config patterns), YAML parseability of the progress/failure files, `--resume` behavior, shard merging including empty and duplicate shards, and both directions of the `--compare` classification.

**AI Tools Usage**

The changes in this pull request were written by Claude Code (Claude Fable 5) under direction of Axel Kohlmeyer, who reviewed the changes.

**Post Submission Checklist**

- [x] The pull request is complete
- [x] The source code follows the LAMMPS formatting guidelines
- [ ] The feature has been verified to work with the conventional build system (n/a, testing tool change only)
- [ ] The feature has been verified to work with the CMake based build system (n/a, testing tool change only)
- [ ] Suitable tests have been added to the unittest tree. (n/a)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a1f0162e82c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.