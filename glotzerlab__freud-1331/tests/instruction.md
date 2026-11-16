# Issue

**Title:** Bump the actions-version group with 4 updates

## Description

Bumps the actions-version group with 4 updates: [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel), [actions/setup-python](https://github.com/actions/setup-python), [actions/download-artifact](https://github.com/actions/download-artifact) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release).

Updates `pypa/cibuildwheel` from 2.23.2 to 2.23.3
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v2.23.3</h2>
<ul>
<li>🛠 Dependency updates, including Python 3.13.3 (<a href="">#2371</a>)</li>
</ul>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<hr />
<h2>title: Changelog</h2>
<h1>Changelog</h1>
<h3>v2.23.3</h3>
<p><em>26 April 2025</em></p>
<ul>
<li>🛠 Dependency updates, including Python 3.13.3 (<a href="">#2371</a>)</li>
</ul>
<h3>v2.23.2</h3>
<p><em>24 March 2025</em></p>
<ul>
<li>🐛 Workaround an issue with pyodide builds when running cibuildwheel with a Python that was installed via UV (<a href="">#2328</a> via <a href="">#2331</a>)</li>
<li>🛠 Dependency updates, including a manylinux update that fixes an <a href="">'undefined symbol' error</a> in gcc-toolset (<a href="">#2334</a>)</li>
</ul>
<h3>v2.23.1</h3>
<p><em>15 March 2025</em></p>
<ul>
<li>⚠️ Added warnings when the shorthand values <code>manylinux1</code>, <code>manylinux2010</code>, <code>manylinux_2_24</code>, and <code>musllinux_1_1</code> are used to specify the images in linux builds. The shorthand to these (unmaintainted) images will be removed in v3.0. If you want to keep using these images, explicitly opt-in using the full image URL, which can be found in <a href="https://github.com/pypa/cibuildwheel/blob/v2.23.1/cibuildwheel/resources/pinned_docker_images.cfg">this file</a>. (<a href="">#2312</a>)</li>
<li>🛠 Dependency updates, including a manylinux update which fixes an <a href="">issue with rustup</a>. (<a href="">#2315</a>)</li>
</ul>
<h3>v2.23.0</h3>
<p><em>1 March 2025</em></p>
<ul>
<li>✨ Adds official support for the new GitHub Actions Arm runners. In fact these worked out-of-the-box, now we include them in our tests and example configs. (<a href="">#2135</a> via <a href="">#2281</a>)</li>
<li>✨ Adds support for building PyPy 3.11 wheels (<a href="">#2268</a> via <a href="">#2281</a>)</li>
<li>🛠 Adopts the beta pypa/manylinux image for armv7l builds (<a href="">#2269</a> via <a href="">#2281</a>)</li>
<li>🛠 Dependency updates, including Pyodide 0.27 (<a href="">#2117</a> and <a href="">#2281</a>)</li>
</ul>
<h3>v2.22.0</h3>
<p><em>23 November 2024</em></p>
<ul>
<li>🌟 Added a new <code>CIBW_ENABLE</code>/<code>enable</code> feature that replaces <code>CIBW_FREETHREADED_SUPPORT</code>/<code>free-threaded-support</code> and <code>CIBW_PRERELEASE_PYTHONS</code> with a system that supports both. In cibuildwheel 3, this will also include a PyPy setting and the deprecated options will be removed. (<a href="">#2048</a>)</li>
<li>🌟 <a href="https://peps.python.org/pep-0735/">Dependency groups</a> are now supported for tests. Use <code>CIBW_TEST_GROUPS</code>/<code>test-groups</code> to specify groups in <code>[dependency-groups]</code> for testing. (<a href="">#2063</a>)</li>
<li>🌟 Support for the experimental Ubuntu-based ARMv7l manylinux image (<a href="">#2052</a>)</li>
<li>✨ Show a warning when cibuildwheel is run from Python 3.10 or older; cibuildwheel 3.0 will require Python 3.11 or newer as host (<a href="">#2050</a>)</li>
<li>🐛 Fix issue with stderr interfering with checking the docker version (<a href="">#2074</a>)</li>
<li>🛠 Python 3.9 is now used in <code>CIBW_BEFORE_ALL</code>/<code>before-all</code> on linux, replacing 3.8, which is now EoL (<a href="">#2043</a>)</li>
<li>🛠 Error messages for producing a pure-Python wheel are slightly more informative (<a href="">#2044</a>)</li>
<li>🛠 Better error when <code>uname -m</code> fails on ARM (<a href="">#2049</a>)</li>
<li>🛠 Better error when repair fails and docs for abi3audit on Windows (<a href="">#2058</a>)</li>
<li>🛠 Better error when <code>manylinux-interpreters ensure</code> fails (<a href="">#2066</a>)</li>
<li>🛠 Update Pyodide to 0.26.4, and adapt to the unbundled pyodide-build (now 0.29) (<a href="">#2090</a>)</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>faf86a6</code></a> Bump version: v2.23.3</li>
<li><a href=""><code>4241f37</code></a> [2.x] Update dependencies (<a href="">#2371</a>)</li>
<li>See full diff in <a href="https://github.com/pypa/cibuildwheel/compare/d04cacbc9866d432033b1d09142936e6a0e2121a...faf86a6ed7efa889faf6996aa23820831055001a">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/setup-python` from 5.5.0 to 5.6.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v5.6.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Workflow updates related to Ubuntu 20.04 by <a href="https://github.com/aparnajyothi-y"><code>@​aparnajyothi-y</code></a> in <a href="">actions/setup-python#1065</a></li>
<li>Fix for Candidate Not Iterable Error by <a href="https://github.com/aparnajyothi-y"><code>@​aparnajyothi-y</code></a> in <a href="">actions/setup-python#1082</a></li>
<li>Upgrade semver and <code>@​types/semver</code> by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1091</a></li>
<li>Upgrade prettier from 2.8.8 to 3.5.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1046</a></li>
<li>Upgrade ts-jest from 29.1.2 to 29.3.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1081</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v5...v5.6.0">https://github.com/actions/setup-python/compare/v5...v5.6.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>a26af69</code></a> Bump ts-jest from 29.1.2 to 29.3.2 (<a href="">#1081</a>)</li>
<li><a href=""><code>30eafe9</code></a> Bump prettier from 2.8.8 to 3.5.3 (<a href="">#1046</a>)</li>
<li><a href=""><code>5d95bc1</code></a> Bump semver and <code>@​types/semver</code> (<a href="">#1091</a>)</li>
<li><a href=""><code>6ed2c67</code></a> Fix for Candidate Not Iterable Error (<a href="">#1082</a>)</li>
<li><a href=""><code>e348410</code></a> Remove Ubuntu 20.04 from workflows due to deprecation from 2025-04-15 (<a href="">#1065</a>)</li>
<li>See full diff in <a href="https://github.com/actions/setup-python/compare/8d9ed9ac5c53483de85588cdf95a591a75ab9f55...a26af69be951a213d495a4c3e4e4022e16d87065">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/download-artifact` from 4.2.1 to 4.3.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.3.0</h2>
<h2>What's Changed</h2>
<ul>
<li>feat: implement new <code>artifact-ids</code> input by <a href="https://github.com/GrantBirki"><code>@​GrantBirki</code></a> in <a href="">actions/download-artifact#401</a></li>
<li>Fix workflow example for downloading by artifact ID by <a href="https://github.com/joshmgross"><code>@​joshmgross</code></a> in <a href="">actions/download-artifact#402</a></li>
<li>Prep for v4.3.0 release by <a href="https://github.com/robherley"><code>@​robherley</code></a> in <a href="">actions/download-artifact#404</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/GrantBirki"><code>@​GrantBirki</code></a> made their first contribution in <a href="">actions/download-artifact#401</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v4.2.1...v4.3.0">https://github.com/actions/download-artifact/compare/v4.2.1...v4.3.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>d3f86a1</code></a> Merge pull request <a href="">#404</a> from actions/robherley/v4.3.0</li>
<li><a href=""><code>fc02353</code></a> prep for v4.3.0 release</li>
<li><a href=""><code>7745437</code></a> Merge pull request <a href="">#402</a> from actions/joshmgross/download-by-id-example</li>
<li><a href=""><code>84fc7a0</code></a> Remove path filters from Check dist workflow</li>
<li><a href=""><code>67f2bc3</code></a> Fix workflow example for downloading by artifact ID</li>
<li><a href=""><code>8ea3c2c</code></a> Merge pull request <a href="">#401</a> from actions/download-by-id</li>
<li><a href=""><code>d219c63</code></a> add supporting unit tests for artifact downloads with ids</li>
<li><a href=""><code>54124fb</code></a> revert <code>getArtifact()</code> changes - for now we have to list and filter by artifa...</li>
<li><a href=""><code>b83057b</code></a> bundle</li>
<li><a href=""><code>171183c</code></a> use the same <code>artifactClient.getArtifact</code> structure as seen above in `isSingl...</li>
<li>Additional commits viewable in <a href="https://github.com/actions/download-artifact/compare/95815c38cf2ff2164869cbab79da8d1f422bc89e...d3f86a106a0bac45b974a628896c90dbdf5c8093">compare view</a></li>
</ul>
</details>
<br />

Updates `softprops/action-gh-release` from 2.2.1 to 2.2.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/releases">softprops/action-gh-release's releases</a>.</em></p>
<blockquote>
<h2>v2.2.2</h2>
<!-- raw HTML omitted -->
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: updating release draft status from true to false by <a href="https://github.com/galargh"><code>@​galargh</code></a> in <a href="">softprops/action-gh-release#316</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore: simplify ref_type test by <a href="https://github.com/steinybot"><code>@​steinybot</code></a> in <a href="">softprops/action-gh-release#598</a></li>
<li>fix(docs): clarify the default for tag_name by <a href="https://github.com/muzimuzhi"><code>@​muzimuzhi</code></a> in <a href="">softprops/action-gh-release#599</a></li>
<li>test(release): add unit tests when searching for a release by <a href="https://github.com/rwaskiewicz"><code>@​rwaskiewicz</code></a> in <a href="">softprops/action-gh-release#603</a></li>
<li>dependency updates</li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/steinybot"><code>@​steinybot</code></a> made their first contribution in <a href="">softprops/action-gh-release#598</a></li>
<li><a href="https://github.com/muzimuzhi"><code>@​muzimuzhi</code></a> made their first contribution in <a href="">softprops/action-gh-release#599</a></li>
<li><a href="https://github.com/galargh"><code>@​galargh</code></a> made their first contribution in <a href="">softprops/action-gh-release#316</a></li>
<li><a href="https://github.com/rwaskiewicz"><code>@​rwaskiewicz</code></a> made their first contribution in <a href="">softprops/action-gh-release#603</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/softprops/action-gh-release/compare/v2...v2.2.2">https://github.com/softprops/action-gh-release/compare/v2...v2.2.2</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/blob/master/CHANGELOG.md">softprops/action-gh-release's changelog</a>.</em></p>
<blockquote>
<h2>2.2.2</h2>
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: updating release draft status from true to false by <a href="https://github.com/galargh"><code>@​galargh</code></a> in <a href="">softprops/action-gh-release#316</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore: simplify ref_type test by <a href="https://github.com/steinybot"><code>@​steinybot</code></a> in <a href="">softprops/action-gh-release#598</a></li>
<li>fix(docs): clarify the default for tag_name by <a href="https://github.com/muzimuzhi"><code>@​muzimuzhi</code></a> in <a href="">softprops/action-gh-release#599</a></li>
<li>test(release): add unit tests when searching for a release by <a href="https://github.com/rwaskiewicz"><code>@​rwaskiewicz</code></a> in <a href="">softprops/action-gh-release#603</a></li>
<li>dependency updates</li>
</ul>
<h2>2.2.1</h2>
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: big file uploads by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#562</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.1 to 22.10.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#559</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.10.2 to 22.10.5 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#569</a></li>
<li>chore: update error and warning messages for not matching files in files field by <a href="https://github.com/ytimocin"><code>@​ytimocin</code></a> in <a href="">softprops/action-gh-release#568</a></li>
</ul>
<h2>2.2.0</h2>
<h2>What's Changed</h2>
<h3>Exciting New Features 🎉</h3>
<ul>
<li>feat: read the release assets asynchronously by <a href="https://github.com/xen0n"><code>@​xen0n</code></a> in <a href="">softprops/action-gh-release#552</a></li>
</ul>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix(docs): clarify the default for tag_name by <a href="https://github.com/alexeagle"><code>@​alexeagle</code></a> in <a href="">softprops/action-gh-release#544</a></li>
</ul>
<h3>Other Changes 🔄</h3>
<ul>
<li>chore(deps): bump typescript from 5.6.3 to 5.7.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#548</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.0 to 22.9.4 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#547</a></li>
<li>chore(deps): bump cross-spawn from 7.0.3 to 7.0.6 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#545</a></li>
<li>chore(deps): bump <code>@​vercel/ncc</code> from 0.38.2 to 0.38.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#543</a></li>
<li>chore(deps): bump prettier from 3.3.3 to 3.4.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#550</a></li>
<li>chore(deps): bump <code>@​types/node</code> from 22.9.4 to 22.10.1 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#551</a></li>
<li>chore(deps): bump prettier from 3.4.1 to 3.4.2 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">softprops/action-gh-release#554</a></li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>da05d55</code></a> release 2.2.2</li>
<li><a href=""><code>6b18c2f</code></a> test(release): add unit tests when searching for a release (<a href="">#603</a>)</li>
<li><a href=""><code>e2b105c</code></a> chore(deps): bump actions/setup-node in the github-actions group (<a href="">#607</a>)</li>
<li><a href=""><code>e707470</code></a> chore(deps): bump the npm group with 4 updates (<a href="">#608</a>)</li>
<li><a href=""><code>36833a1</code></a> fix: updating release draft status (<a href="">#316</a>)</li>
<li><a href=""><code>8bb7207</code></a> chore(deps): bump actions/setup-node in the github-actions group (<a href="">#597</a>)</li>
<li><a href=""><code>93bb5ff</code></a> fix(docs): clarify the default for tag_name (<a href="">#599</a>)</li>
<li><a href=""><code>581b12c</code></a> chore: simplify ref_type test (<a href="">#598</a>)</li>
<li><a href=""><code>b540ad2</code></a> chore(deps): bump <code>@​octokit/request</code> (<a href="">#605</a>)</li>
<li><a href=""><code>ac224e9</code></a> chore(deps): bump the npm group across 1 directory with 5 updates (<a href="">#604</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/softprops/action-gh-release/compare/c95fe1489396fe8a9eb87c0abf8aa5b2ef267fda...da05d552573ad5aba039eaac05058a918a7bf631">compare view</a></li>
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `698ea7841993`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.