# Issue

**Title:** Bump pyiron-atomistics from 0.2.20 to 0.2.21

## Description

Bumps [pyiron-atomistics](https://github.com/pyiron/pyiron_atomistics) from 0.2.20 to 0.2.21.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pyiron/pyiron_atomistics/releases">pyiron-atomistics's releases</a>.</em></p>
<blockquote>
<h2>pyiron_atomistics-0.2.21</h2>
<ul>
<li>Neighbor analysis for the entire trajectory (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/251">#251</a>)</li>
<li>Get neighbors (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/239">#239</a>)</li>
<li>Save all VCSGC parameters in generic input (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/262">#262</a>)</li>
<li>Only force skew when tensions are applied(<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/263">#263</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>ed730bf</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/283">#283</a> from pyiron/base0.2.20</li>
<li><a href=""><code>24eed62</code></a> Update environment.yml</li>
<li><a href=""><code>d10bab6</code></a> Update setup.py</li>
<li><a href=""><code>e954f09</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/251">#251</a> from pyiron/neighbors_traj</li>
<li><a href=""><code>7766aba</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/282">#282</a> from pyiron/dependabot/pip/numpy-1.21.1</li>
<li><a href=""><code>2d507db</code></a> Merging new neighbor function changes</li>
<li><a href=""><code>853b376</code></a> Merge remote-tracking branch 'origin/master' into neighbors_traj</li>
<li><a href=""><code>6d51b09</code></a> Merge remote-tracking branch 'origin/master' into neighbors_traj</li>
<li><a href=""><code>62e39a4</code></a> Remove unused import</li>
<li><a href=""><code>1b74ecc</code></a> Refactoring class name and attributes (Marvin's suggestion)</li>
<li>Additional commits viewable in <a href="https://github.com/pyiron/pyiron_atomistics/compare/pyiron_atomistics-0.2.20...pyiron_atomistics-0.2.21">compare view</a></li>
</ul>
</details>
<br />

[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyiron-atomistics&package-manager=pip&previous-version=0.2.20&new-version=0.2.21)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3dd9a7ee68ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.