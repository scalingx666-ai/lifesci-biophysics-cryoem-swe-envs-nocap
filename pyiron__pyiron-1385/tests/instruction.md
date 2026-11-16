# Issue

**Title:** Bump pyiron-atomistics from 0.2.30 to 0.2.31

## Description

Bumps [pyiron-atomistics](https://github.com/pyiron/pyiron_atomistics) from 0.2.30 to 0.2.31.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pyiron/pyiron_atomistics/releases">pyiron-atomistics's releases</a>.</em></p>
<blockquote>
<h2>pyiron_atomistics-0.2.31</h2>
<ul>
<li>Make NeighborTrajectory simpler (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/445">#445</a>)</li>
<li>Use state instead of Settings (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/424">#424</a>)</li>
<li>use unwrapped_positions in displacements (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/451">#451</a>)</li>
<li>Store shells in Neighbors Trajectory (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/444">#444</a>)</li>
<li>replace full output info in <strong>str</strong> by chemical formula (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/439">#439</a>)</li>
<li>Fix Lammps h5md parser (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/446">#446</a>)</li>
<li>Use FlattenedStorage in NeighborsTrajectory (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/387">#387</a>)</li>
<li>Clear up TypeError in StructureContainer.append (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/441">#441</a>)</li>
<li>Add clear error when calling animate_structure on empty job (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/442">#442</a>)</li>
<li>add get_primitive_cell from spglib (<a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/433">#433</a>)</li>
<li>dependencies: <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/449">#449</a>, <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/432">#432</a>, <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/437">#437</a>, <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/430">#430</a>, <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/456">#456</a></li>
<li>GitHub infrastructure: <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/438">#438</a></li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>b691055</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/456">#456</a> from pyiron/base_0.4.3</li>
<li><a href=""><code>d3f7459</code></a> Update setup.py</li>
<li><a href=""><code>7105c22</code></a> Update to base 0.4.3</li>
<li><a href=""><code>18d8356</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/445">#445</a> from pyiron/simpletraj</li>
<li><a href=""><code>c096e1c</code></a> Do not recompute neighbors when loading from HDF</li>
<li><a href=""><code>2d97c1a</code></a> Call _compute_neighbors only if has_structure was set</li>
<li><a href=""><code>3380d99</code></a> Fix typos</li>
<li><a href=""><code>63928b1</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/424">#424</a> from pyiron/use_state</li>
<li><a href=""><code>1272acf</code></a> Merge pull request <a href="https://github-redirect.dependabot.com/pyiron/pyiron_atomistics/issues/451">#451</a> from pyiron/total_displacements</li>
<li><a href=""><code>8489698</code></a> Call compute_neighbors automatically</li>
<li>Additional commits viewable in <a href="https://github.com/pyiron/pyiron_atomistics/compare/pyiron_atomistics-0.2.30...pyiron_atomistics-0.2.31">compare view</a></li>
</ul>
</details>
<br />

[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pyiron-atomistics&package-manager=pip&previous-version=0.2.30&new-version=0.2.31)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da8ac16dbebb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.