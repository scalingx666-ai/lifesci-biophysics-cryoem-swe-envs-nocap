# Issue

**Title:** build(deps): Bump the actions-version group with 4 updates

## Description

Bumps the actions-version group with 4 updates: [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel), [actions/upload-artifact](https://github.com/actions/upload-artifact), [actions/setup-python](https://github.com/actions/setup-python) and [actions/download-artifact](https://github.com/actions/download-artifact).

Updates `pypa/cibuildwheel` from 2.19.1 to 2.19.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>Version 2.19.2</h2>
<ul>
<li>🐛 Update manylinux2014 pins to versions that support past-EoL CentOS 7 mirrors. (<a href="">#1917</a>)</li>
<li>🐛 Support <code>--no-isolation</code> with <code>build[uv]</code> build-frontend. (<a href="">#1889</a>)</li>
<li>🛠 Provide attestations for releases at <a href="https://github.com/pypa/cibuildwheel/attestations">https://github.com/pypa/cibuildwheel/attestations</a>. (<a href="">#1916</a>)</li>
<li>🛠 Provide CPython 3.13.0b3. (<a href="">#1913</a>)</li>
<li>🛠 Remove some workarounds now that pip 24.1 is available. (<a href="">#1891</a>, <a href="">#1892</a>)</li>
<li>📚 Remove nosetest from our docs. (<a href="">#1821</a>)</li>
<li>📚 Document the macOS ARM workaround for 3.8 on GHA. (<a href="">#1871</a>)</li>
<li>📚 GitLab CI + macOS is now a supported platform with an example. (<a href="">#1911</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<h3>v2.19.2</h3>
<ul>
<li>🐛 Update manylinux2014 pins to versions that support past-EoL CentOS 7 mirrors. (<a href="">#1917</a>)</li>
<li>🐛 Support <code>--no-isolation</code> with <code>build[uv]</code> build-frontend. (<a href="">#1889</a>)</li>
<li>🛠 Provide attestations for releases at <a href="https://github.com/pypa/cibuildwheel/attestations">https://github.com/pypa/cibuildwheel/attestations</a>. (<a href="">#1916</a>)</li>
<li>🛠 Provide CPython 3.13.0b3. (<a href="">#1913</a>)</li>
<li>🛠 Remove some workarounds now that pip 21.1 is available. (<a href="">#1891</a>, <a href="">#1892</a>)</li>
<li>📚 Remove nosetest from our docs. (<a href="">#1821</a>)</li>
<li>📚 Document the macOS ARM workaround for 3.8 on GHA. (<a href="">#1871</a>)</li>
<li>📚 GitLab CI + macOS is now a supported platform with an example. (<a href="">#1911</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>7e5a838</code></a> Bump version: v2.19.2</li>
<li><a href=""><code>19e1b8a</code></a> chore: attestations (<a href="">#1916</a>)</li>
<li><a href=""><code>73581ae</code></a> [Bot] Update dependencies (<a href="">#1917</a>)</li>
<li><a href=""><code>f21ff5e</code></a> [pre-commit.ci] pre-commit autoupdate (<a href="">#1914</a>)</li>
<li><a href=""><code>973946b</code></a> fix: support --no-isolation with build[uv] (<a href="">#1889</a>)</li>
<li><a href=""><code>5ea40e6</code></a> [Bot] Update dependencies (<a href="">#1913</a>)</li>
<li><a href=""><code>6a36f64</code></a> feat: add macOS on GitLab (<a href="">#1911</a>)</li>
<li><a href=""><code>31de15b</code></a> chore: remove nosetests from tests and docs (<a href="">#1898</a>)</li>
<li><a href=""><code>3179fd2</code></a> [Bot] Update dependencies (<a href="">#1900</a>)</li>
<li><a href=""><code>184d4e1</code></a> fix(ci): CircleCI / Azure Pipelines / GitLab (<a href="">#1899</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/v2.19.1...v2.19.2">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/upload-artifact` from 4.3.3 to 4.3.4
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/upload-artifact/releases">actions/upload-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.3.4</h2>
<h2>What's Changed</h2>
<ul>
<li>Update <code>@​actions/artifact</code> version, bump dependencies by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="">actions/upload-artifact#584</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4.3.3...v4.3.4">https://github.com/actions/upload-artifact/compare/v4.3.3...v4.3.4</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>0b2256b</code></a> Merge pull request <a href="">#584</a> from actions/robherley/bump-pkgs</li>
<li><a href=""><code>488dcef</code></a> licensed cache</li>
<li><a href=""><code>04c51f5</code></a> ncc</li>
<li><a href=""><code>32a9e27</code></a> bump <code>@​actions/artifact</code> and npm audit</li>
<li><a href=""><code>552bf37</code></a> new version</li>
<li><a href=""><code>79616d2</code></a> Merge pull request <a href="">#565</a> from actions/eggyhead/use-artifact-v2.1.6</li>
<li>See full diff in <a href="https://github.com/actions/upload-artifact/compare/v4.3.3...v4.3.4">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/setup-python` from 5.1.0 to 5.1.1
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v5.1.1</h2>
<h2>What's Changed</h2>
<h3>Bug fixes:</h3>
<ul>
<li>fix(ci): update all failing workflows by <a href="https://github.com/mayeut"><code>@​mayeut</code></a> in <a href="">actions/setup-python#863</a>
This update ensures compatibility and optimal performance of workflows on the latest macOS version.</li>
</ul>
<h3>Documentation changes:</h3>
<ul>
<li>Documentation update for cache by <a href="https://github.com/gowridurgad"><code>@​gowridurgad</code></a> in <a href="">actions/setup-python#873</a></li>
</ul>
<h3>Dependency updates:</h3>
<ul>
<li>Bump braces from 3.0.2 to 3.0.3 and undici from 5.28.3 to 5.28.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#893</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/gowridurgad"><code>@​gowridurgad</code></a> made their first contribution in <a href="">actions/setup-python#873</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v5...v5.1.1">https://github.com/actions/setup-python/compare/v5...v5.1.1</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>39cd149</code></a> Documentation update for cache (<a href="">#873</a>)</li>
<li><a href=""><code>a0d74c0</code></a> fix(ci): update all failing workflows (<a href="">#863</a>)</li>
<li><a href=""><code>4eb7dbc</code></a> Bump braces from 3.0.2 to 3.0.3 (<a href="">#893</a>)</li>
<li>See full diff in <a href="https://github.com/actions/setup-python/compare/v5.1.0...v5.1.1">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/download-artifact` from 4.1.7 to 4.1.8
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.1.8</h2>
<h2>What's Changed</h2>
<ul>
<li>Update <code>@​actions/artifact</code> version, bump dependencies by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="">actions/download-artifact#341</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v4...v4.1.8">https://github.com/actions/download-artifact/compare/v4...v4.1.8</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>fa0a91b</code></a> Merge pull request <a href="">#341</a> from actions/robherley/bump-pkgs</li>
<li><a href=""><code>b54d088</code></a> Update <code>@​actions/artifact</code> version, bump dependencies</li>
<li>See full diff in <a href="https://github.com/actions/download-artifact/compare/v4.1.7...v4.1.8">compare view</a></li>
</ul>
</details>
<br />

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
- `@dependabot ignore <dependency name> major version` will close this group update PR and stop Dependabot creating any more for the specific dependency's major version (unless you unignore this specific dependency's major version or upgrade to it yourself)
- `@dependabot ignore <dependency name> minor version` will close this group update PR and stop Dependabot creating any more for the specific dependency's minor version (unless you unignore this specific dependency's minor version or upgrade to it yourself)
- `@dependabot ignore <dependency name>` will close this group update PR and stop Dependabot creating any more for the specific dependency (unless you unignore this specific dependency or upgrade to it yourself)
- `@dependabot unignore <dependency name>` will remove all of the ignore conditions of the specified dependency
- `@dependabot unignore <dependency name> <ignore condition>` will remove the ignore condition of the specified dependency and ignore conditions

</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8a5c9df36e4a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.