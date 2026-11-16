# Issue

**Title:** Update dependency prefix-dev/pixi to v0.82.1

## Description

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [prefix-dev/pixi](https://redirect.github.com/prefix-dev/pixi) | uses-with | patch | `v0.82.0` → `v0.82.1` |

---

### Release Notes

<details>
<summary>prefix-dev/pixi</summary>

### [`v0.82.1`](https://redirect.github.com/prefix-dev/pixi/releases/tag/v0.82.1)

[Compare Source](https://redirect.github.com/prefix-dev/pixi/compare/v0.82.0...v0.82.1)

##### \[0.82.1] - 2026-10-20

##### 🔒 Security 🔒

This patch release contains security fixes for the incident
described at <https://emudhra.we-have-a-problem.com/>.
**All users are encouraged to upgrade to this version immediately.**

The affected `webpki-root-certs` and `rustls-webpki` dependencies
have been bumped to patched releases `1.0.10` and `0.103.16` respectively.

##### Features

- pixi-build-r: Add CMake and pkg-config when SystemRequirements asks for them by [@&#8203;roaldarbol](https://redirect.github.com/roaldarbol) in
- spec: Support exemptions in the resolved exclude-newer config by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- manifest: Read exemptions from `exclude-newer` in `[workspace]` by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- Pass arbitrary virtual packages to the solver by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Configure virtual package detectors and write shared config by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- Run channel-registered detectors during host detection by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in

##### Fixes

- config: Reject relative `detached-environments` paths by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- cli: Render colours in log messages instead of escape codes by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Correct muddled post-link / pre-unlink script check by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- workspace: Error when removing a feature that does not exist by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- environment: Detect relocated environments again by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- tasks: Error on undefined environments in task definitions by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Stop printing a panic message for build dispatch errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Bump webpki-root-certs and rustls-webpki for eMudhra intermediate compromise by [@&#8203;wolfv](https://redirect.github.com/wolfv) in

##### Documentation

- Artifactory oidc login through pixi by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- Document auth-workload-identity inputs of setup-pixi v0.11.1 by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- \[project] is the deprecated alias for \[workspace], not the reverse by [@&#8203;ZainnQureshii](https://redirect.github.com/ZainnQureshii) in
- Update setup-pixi docs by [@&#8203;pavelzw](https://redirect.github.com/pavelzw) in
- Note that conda-forge is the only default channel for pixi global by [@&#8203;alisterburt](https://redirect.github.com/alisterburt) in
- cli: Improve descriptions by [@&#8203;timhoffm](https://redirect.github.com/timhoffm) in
- Fix admonition rendering by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### Maintenance

- release: Add `rust` to `backends-release` env by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- release: Make `cargo` available in `backends-release` by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- release: Bump `pixi-build-r` to `0.2.0` by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- deps: Bump rustls and aws-smithy-json by [@&#8203;wolfv](https://redirect.github.com/wolfv) in
- release: Add `release` scope to automated commit messages by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- init: Show source context for pyproject.toml read errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- workspace: Detect host capabilities during construction by [@&#8203;Hofer-Julian](https://redirect.github.com/Hofer-Julian) in
- core: Type the activation errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- task: Type the task environment errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Type the workspace accessor errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Type the workspace registry errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Type the environment helper errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- Network utilities available in rosdistro by [@&#8203;Timple](https://redirect.github.com/Timple) in
- core: Type the PyPI resolve errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in
- core: Type the lock-file load, write and prefix errors by [@&#8203;lucascolley](https://redirect.github.com/lucascolley) in

##### New Contributors

- [@&#8203;alisterburt](https://redirect.github.com/alisterburt) made their first contribution in
- [@&#8203;ZainnQureshii](https://redirect.github.com/ZainnQureshii) made their first contribution in

</details>

---

### Configuration

📅 **Schedule**: (in timezone America/Detroit)

- Branch creation
  - At any time (no schedule defined)
- Automerge
  - At any time (no schedule defined)

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/glotzerlab/freud).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0NC4xNzEuMiIsInVwZGF0ZWRJblZlciI6IjQ0LjE3MS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJkZXBlbmRlbmNpZXMiXX0=-->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9691842ebba2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.