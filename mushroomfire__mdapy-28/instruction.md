# Issue

**Title:** Toward 2.0: compatibility, Linux on ARM, Python 3.15, documentation from release tags

## Description

- **Compatibility.** A new page, About → Compatibility, states what 2.x promises from 2.0.0 on: semantic versioning, what counts as the public API, deprecations that warn until 3.0, results that change only to fix a bug, experimental features outside the promise, the support of Python, NumPy and polars, and how to stay on 1.x. The README and the home page add it to the reasons for MDAPY.
- **Linux on ARM.** Wheels for aarch64, built natively on `ubuntu-24.04-arm` and tested like the others; `tests.yml` runs the tests there too.
- **Python 3.15.** A test job and the classifier; the weekly upstream run moves to 3.15.
- **Documentation from release tags.** `docs.yml` publishes the site from the tag of each release, so that it describes the version `pip` installs, not what master holds for the next one; a run by hand publishes the ref it runs on. Contributing gains a section on releases.
- **CITATION.cff**, for GitHub's "Cite this repository", pointing to the paper.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c16349f91132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.