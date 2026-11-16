# Issue

**Title:** Update pypa/gh-action-pypi-publish action to v1.13.0

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [pypa/gh-action-pypi-publish](https://redirect.github.com/pypa/gh-action-pypi-publish) | action | minor | `v1.12.4` -> `v1.13.0` |

---

### Release Notes

<details>
<summary>pypa/gh-action-pypi-publish (pypa/gh-action-pypi-publish)</summary>

### [`v1.13.0`](https://redirect.github.com/pypa/gh-action-pypi-publish/releases/tag/v1.13.0)

[Compare Source](https://redirect.github.com/pypa/gh-action-pypi-publish/compare/v1.12.4...v1.13.0)

<p align="right"><i><a href="https://anaconda.surveymonkey.com/r/py-package-2025">Take the 2025 Python Packaging Survey if you still haven't!</a></i></p>

> \[!important]
> 🚨 This release includes fixes for [GHSA-vxmw-7h4f-hqxh](https://redirect.github.com/pypa/gh-action-pypi-publish/security/advisories/GHSA-vxmw-7h4f-hqxh) discovered by [@&#8203;woodruffw](https://redirect.github.com/woodruffw)[💰](https://redirect.github.com/sponsors/woodruffw).
> We've also integrated [Zizmor](http://zizmor.sh) to catch similar issues in the future and you should too.

##### ✨ New Stuff

[@&#8203;woodruffw](https://redirect.github.com/woodruffw)[💰](https://redirect.github.com/sponsors/woodruffw) updated the README to no longer mention the attestations feature being experimental in : it's been rather stable for a year already 🎉
He also added more diagnostic output which includes printing out the GitHub Environment claim via  and warning about the unsupported reusable workflows configurations , when using Trusted Publishing.

> \[!tip]
> The official support for reusable workflows is currently blocked on changes to PyPI. To get updates about progress on the action side, you may want to subscribe to .
> At PyCon US 2025 Sprints, [@&#8203;facutuesca](https://redirect.github.com/facutuesca)[💰](https://redirect.github.com/sponsors/facutuesca), [@&#8203;miketheman](https://redirect.github.com/miketheman)[💰](https://redirect.github.com/sponsors/miketheman), [@&#8203;woodruffw](https://redirect.github.com/woodruffw)[💰](https://redirect.github.com/sponsors/woodruffw) and I[💰][GH Sponsors URL] spent several hours IRL brainstorming how to fix this and migrate projects that happen to rely on an obscure corner case with reusable workflows that temporarily allows them to function by accident.
> The result of that discussion is posted @&#8203; .
> Note that this is a volunteer-led effort and there is no ETA. If you need this soon, make your employer sponsor the PSF and maybe they'll be able to hire somebody for this work on Warehouse.

In addition to that, [@&#8203;konstin](https://redirect.github.com/konstin)[💰](https://redirect.github.com/sponsors/konstin) sent  to pin `actions/setup-python` to a SHA hash. This makes `pypi-publish` compatible with new GitHub policies that allow organizations to mandate hash-pinning actions used in workflows.

##### 🛠️ Internal Dependencies

[@&#8203;webknjaz](https://redirect.github.com/webknjaz)[💰][GH Sponsors URL] made a bunch of updates to the action runtime which includes bumping it to Python 3.13 in  and updating the dependency tree across the board. `pip-with-requires-python` is no longer being installed (). Some related bumps were contributed by [@&#8203;woodruffw](https://redirect.github.com/woodruffw)[💰](https://redirect.github.com/sponsors/woodruffw) () and [@&#8203;kurtmckee](https://redirect.github.com/kurtmckee)[💰](https://redirect.github.com/sponsors/kurtmckee) sent a contributor-facing PR, bumping the linting configuration via .

##### 💪 New Contributors

- [@&#8203;kurtmckee](https://redirect.github.com/kurtmckee) made their first contribution in
- [@&#8203;konstin](https://redirect.github.com/konstin) made their first contribution in

**🪞 Full Diff**: <https://github.com/pypa/gh-action-pypi-publish/compare/v1.12.4...v1.13.0>

**🧔‍♂️ Release Manager:** [@&#8203;webknjaz](https://redirect.github.com/sponsors/webknjaz) [🇺🇦](https://stand-with-ukraine.pp.ua)

**💬 Discuss** [on Bluesky 🦋](https://bsky.app/profile/webknjaz.me/post/3lxxzvzhvfc2e), [on Mastodon 🐘](https://mastodon.social/@&#8203;webknjaz/115143522527224444) and [on GitHub][release discussion].

[![GH Sponsors badge]][GH Sponsors URL]

[release discussion]: https://redirect.github.com/pypa/gh-action-pypi-publish/discussions/379

[GH Sponsors badge]: https://img.shields.io/badge/%40webknjaz-transparent?logo=githubsponsors&logoColor=%23EA4AAA&label=Sponsor&color=2a313c

[GH Sponsors URL]: https://redirect.github.com/sponsors/webknjaz

</details>

---

### Configuration

📅 **Schedule**: Branch creation - At any time (no schedule defined), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d254797552ba`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.