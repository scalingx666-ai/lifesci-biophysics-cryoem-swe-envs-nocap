# Issue

**Title:** Documentation in versions: latest, each release, and dev

## Description

The documentation gets a version menu, with [mike](https://github.com/jimporter/mike), as MkDocs Material supports it:

- `latest`, the newest release, which readers see by default; each release keeps its minor version (2.0, 2.1, ...), which its patch releases update; `dev` follows master. Pre-releases write no version.
- A banner over any version other than latest, in the neutral colors of the site, links to the latest.
- The workflow `docs` writes the versions to the branch `gh-pages` (copies, not symlinks) and publishes that branch with GitHub Pages as before (source: GitHub Actions, no change of settings). Until a release has written `latest`, nothing is published: merging this writes `dev` to `gh-pages` and leaves the site of 2.0.0rc1 as it is; the tag v2.0.0 writes 2.0 and latest and publishes all.
- A 404 page at the root sends a path without a version, as in the README, on PyPI and in the banner of the 1.x docs, to the same page of latest.
- Contributing and the release notes describe it.

Checked locally with mike 2.2.0: 2.0 with latest, then dev, assembled as the workflow does, served by a server that answers missing paths with the root 404.html as GitHub Pages does, and opened in headless Chrome: the menu shows `2.0 latest` and `dev`, the banner shows on dev only, in light and dark; `/mdapy/getting-started/from-1x/` and `/mdapy/examples/plotting/#saving` land on the pages of latest, and a page missing in dev says so.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0fad6c56955`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.