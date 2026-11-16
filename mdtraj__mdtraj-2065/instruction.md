# Issue

**Title:** Bump pypa/cibuildwheel from 3.1.1 to 3.1.2

## Description

Bumps [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel) from 3.1.1 to 3.1.2.
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v3.1.2</h2>
<ul>
<li>⚠️  Add an error if <code>CIBW_FREE_THREADING_SUPPORT</code> is set; you are likely missing 3.13t wheels, please use the <code>enable</code>/<code>CIBW_ENABLE</code> (<a href="">#2520</a>)</li>
<li>🛠 <code>riscv64</code> now enabled if you target that architecture, it's now supported on PyPI (<a href="">#2509</a>)</li>
<li>🛠 Add warning when using <code>cpython-experimental-riscv64</code> (no longer needed) (<a href="">#2526</a>, <a href="">#2528</a>)</li>
<li>🛠 iOS versions bumped, fixing issues with 3.14 (now RC 1) (<a href="">#2530</a>)</li>
<li>🐛 Fix bug in Android running wheel from our GitHub Action (<a href="">#2517</a>)</li>
<li>🐛 Fix warning when using <code>test-skip</code> of <code>&quot;*-macosx_universal2:arm64&quot;</code> (<a href="">#2522</a>)</li>
<li>🐛 Fix incorrect number of wheels reported in logs, again (<a href="">#2517</a>)</li>
<li>📚 We welcome our Android platform maintainer (<a href="">#2516</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<h3>v3.1.2</h3>
<p><em>29 July 2025</em></p>
<ul>
<li>⚠️  Add an error if <code>CIBW_FREE_THREADING_SUPPORT</code> is set; you are likely missing 3.13t wheels, please use the <code>enable</code>/<code>CIBW_ENABLE</code> (<a href="">#2520</a>)</li>
<li>🛠 <code>riscv64</code> now enabled if you target that architecture, it's now supported on PyPI (<a href="">#2509</a>)</li>
<li>🛠 Add warning when using <code>cpython-experimental-riscv64</code> (no longer needed) (<a href="">#2526</a>, <a href="">#2528</a>)</li>
<li>🛠 iOS versions bumped, fixing issues with 3.14 (now RC 1) (<a href="">#2530</a>)</li>
<li>🐛 Fix bug in Android running wheel from our GitHub Action (<a href="">#2517</a>)</li>
<li>🐛 Fix warning when using <code>test-skip</code> of <code>&quot;*-macosx_universal2:arm64&quot;</code> (<a href="">#2522</a>)</li>
<li>🐛 Fix incorrect number of wheels reported in logs, again (<a href="">#2517</a>)</li>
<li>📚 We welcome our Android platform maintainer (<a href="">#2516</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>9e4e50b</code></a> Bump version: v3.1.2</li>
<li><a href=""><code>8ef9414</code></a> [pre-commit.ci] pre-commit autoupdate (<a href="">#2532</a>)</li>
<li><a href=""><code>1953c04</code></a> Adding <a href="https://github.com/mhsmith"><code>@​mhsmith</code></a> as platform maintainer for Android (<a href="">#2516</a>)</li>
<li><a href=""><code>46a6d27</code></a> Bump iOS support package versions. (<a href="">#2530</a>)</li>
<li><a href=""><code>0dc0b48</code></a> [Bot] Update dependencies (<a href="">#2529</a>)</li>
<li><a href=""><code>b887417</code></a> fix: enable <code>all</code>  shall not enable <code>cpython-experimental-riscv64</code> (<a href="">#2528</a>)</li>
<li><a href=""><code>36c5079</code></a> tests: reduce testing time on iOS (<a href="">#2523</a>)</li>
<li><a href=""><code>68c0366</code></a> feat: add deprecation warning for <code>cpython-experimental-riscv64</code> enable (<a href="">#2526</a>)</li>
<li><a href=""><code>1c38fac</code></a> fix: invalid warning on <code>test-skip = [&quot;*-macosx_universal2:arm64&quot;]</code> (<a href="">#2522</a>)</li>
<li><a href=""><code>140bc1a</code></a> tests: mark and add skips (<a href="">#2524</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/v3.1.1...v3.1.2">compare view</a></li>
</ul>
</details>
<br />

[![Dependabot compatibility score](https://dependabot-badges.githubapp.com/badges/compatibility_score?dependency-name=pypa/cibuildwheel&package-manager=github_actions&previous-version=3.1.1&new-version=3.1.2)](https://docs.github.com/en/github/managing-security-vulnerabilities/about-dependabot-security-updates#about-compatibility-scores)

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
- `@dependabot show <dependency name> ignore conditions` will show all of the ignore conditions of the specified dependency
- `@dependabot ignore this major version` will close this PR and stop Dependabot creating any more for this major version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this minor version` will close this PR and stop Dependabot creating any more for this minor version (unless you reopen the PR or upgrade to it yourself)
- `@dependabot ignore this dependency` will close this PR and stop Dependabot creating any more for this dependency (unless you reopen the PR or upgrade to it yourself)

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9665aa53cd5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.