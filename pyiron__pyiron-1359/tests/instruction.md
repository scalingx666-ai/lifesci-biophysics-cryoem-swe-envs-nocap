# Issue

**Title:** Bump pyiron-atomistics from 0.2.21 to 0.2.22

## Description

Bumps [pyiron-atomistics](https://github.com/pyiron/pyiron_atomistics) from 0.2.21 to 0.2.22.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pyiron/pyiron_atomistics/releases">pyiron-atomistics's releases</a>.</em></p>
<blockquote>
<h2>pyiron_atomistics-0.2.22</h2>
<ul>
<li>Outcar bands (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/299">#299</a>)</li>
<li>Add error message if <code>Murnaghan.plot</code> is called on unfinished jobs  (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/291">#291</a>)</li>
<li>Make Atoms() faster via caches  (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/224">#224</a>)</li>
<li>Create new Atoms in <code>get_structure</code> if size changes  (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/241">#241</a>)</li>
<li>add mode in <code>get_neighborhood</code> (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/285">#285</a>)</li>
<li>Equivalent points (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/280">#280</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>2957fc7</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/312">#312</a> from pyiron/pyiron_base_0.2.22</li>
<li><a href=""><code>744061f</code></a> Update setup.py</li>
<li><a href=""><code>a8b5f93</code></a> Update environment.yml</li>
<li><a href=""><code>15bfd3b</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/310">#310</a> from pyiron/dependabot/pip/pyiron-base-0.2.21</li>
<li><a href=""><code>c063a2a</code></a> [dependabot skip] Update environment</li>
<li><a href=""><code>894ff21</code></a> Bump pyiron-base from 0.2.20 to 0.2.21</li>
<li><a href=""><code>81168b8</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/289">#289</a> from pyiron/dependabot/pip/pandas-1.3.1</li>
<li><a href=""><code>3f29e76</code></a> [dependabot skip] Update environment</li>
<li><a href=""><code>44d4843</code></a> Bump pandas from 1.3.0 to 1.3.1</li>
<li><a href=""><code>658e96f</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/308">#308</a> from pyiron/fix_windows_tests</li>
<li>Additional commits viewable in <a href="https://github.com/pyiron/pyiron_atomistics/compare/pyiron_atomistics-0.2.21...pyiron_atomistics-0.2.22">compare view</a></li>
</ul>
</details>
<br />

[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyiron-atomistics&package-manager=pip&previous-version=0.2.21&new-version=0.2.22)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

Dependabot will resolve any conflicts with this PR as long as you don't alter it yourself. You can also trigger a rebase manually by commenting `@dependabot rebase`.

[//]: # (dependabot-automerge-start)
[//]: # (dependabot-automerge-end)

---

<details>
<summary>Dependabot commands and options</summary>
<br />

You can trigger Dependabot actions by commenting on this PR:
- `@dependabot rebase` will rebase this PR
- `@dependabot recreate` will recreate this PR, overwriting any edits that have been made to it
- `@dependabot merge` will merge this PR after your CI passes on it
- `@dependabot squash and merge` will squash and merge this PR after your CI passes on it
- `@dependabot cancel merge` will cancel a previously requested merge and block automerging
- `@dependabot reopen` will reopen this PR if it is closed
- `@dependabot close` will close this PR and stop Dependabot recreating it. You can achieve the same result by closing it manually
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8e801a53f429`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.