# Issue

**Title:** Update pins

## Description

This is to do a cleanup of our requirements while ensuring that a [NEP29](https://numpy.org/neps/nep-0029-deprecation_policy.html)-ish policy is not violated by our deps (we may support more than our policy requires, for now). The inspiration is NEP29's policy toward NumPy, which reads: 

> All minor versions of `numpy` released in the 24 months prior to the project, and at minimum the last three minor versions.

For the packages relevant here:

### `openmm`

This supports exactly what NEP29 requires.

* Supported minor versions: 7.6, 7.7, 8.0
* Minor versions released in the last 24 months: 7.7, 8.0 (7.6 was released in August 2021, so just over 24 months ago)

### `ujson`

This supports more than NEP29 requires.

* Supported minor versions: 4.0.2, 4.1, 4.2, 4.3, 5.0, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8
* Minor versions released in the last 24 months: 4.2, 4.3, 5+ (4.2 released Sep 22, 2021, so barely in the window)

### `sqlalchemy`

Okay, so here's where I'm making a new rule. Executive decision: 20 patch releases equals 1 minor release. (TBH, I should probably say 10:1).

* Supported minor versions: 1.4.1-1.4.48, 2.0.0-2.0.12
* Minor versions released in the last 24 months: 1.4.24+ (from Sep 22, 2021), 2.0+

I really need to start releasing like that guy. 😂

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `382a613302a8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.