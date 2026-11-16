# Issue

**Title:** Bump pyiron-atomistics from 0.2.11 to 0.2.12

## Description

Bumps [pyiron-atomistics](https://github.com/pyiron/pyiron_atomistics) from 0.2.11 to 0.2.12.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pyiron/pyiron_atomistics/releases">pyiron-atomistics's releases</a>.</em></p>
<blockquote>
<h2>pyiron_atomistics-0.2.12</h2>
<ul>
<li>getting structure faster</li>
<li>updating atoms symbols via atoms.symbols</li>
<li>Steinhardt parameter implementation</li>
<li>Badr analysis</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>92584be</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/192">#192</a> from pyiron/c2</li>
<li><a href=""><code>a8ecf4f</code></a> Don't call property multiple times</li>
<li><a href=""><code>441292a</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/183">#183</a> from pyiron/steinhardt</li>
<li><a href=""><code>220f81f</code></a> update docstring</li>
<li><a href=""><code>b0e6082</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/188">#188</a> from pyiron/dependabot/pip/pymatgen-2022.0.7</li>
<li><a href=""><code>9e6f0cc</code></a> add docstring</li>
<li><a href=""><code>b74bf64</code></a> Update environment.yml</li>
<li><a href=""><code>e34599d</code></a> use perfect hcp + add cutoff_radius in doc + liams suggestion on assertRaises...</li>
<li><a href=""><code>43d2475</code></a> Avoid tolist()+array()</li>
<li><a href=""><code>86a2465</code></a> tests added</li>
<li>Additional commits viewable in <a href="https://github.com/pyiron/pyiron_atomistics/compare/pyiron_atomistics-0.2.11...pyiron_atomistics-0.2.12">compare view</a></li>
</ul>
</details>
<br />

[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyiron-atomistics&package-manager=pip&previous-version=0.2.11&new-version=0.2.12)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `194141c70944`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.