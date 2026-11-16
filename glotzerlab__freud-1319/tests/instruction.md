# Issue

**Title:** Bump the actions-version group with 4 updates

## Description

Bumps the actions-version group with 4 updates: [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel), [actions/upload-artifact](https://github.com/actions/upload-artifact), [actions/setup-python](https://github.com/actions/setup-python) and [actions/download-artifact](https://github.com/actions/download-artifact).

Updates `pypa/cibuildwheel` from 2.22.0 to 2.23.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v2.23.2</h2>
<ul>
<li>🐛 Workaround an issue with pyodide builds when running cibuildwheel with a Python that was installed via UV (<a href="">#2328</a> via <a href="">#2331</a>)</li>
<li>🛠 Dependency updates, including a manylinux update that fixes an <a href="">'undefined symbol' error</a> in gcc-toolset (<a href="">#2334</a>)</li>
</ul>
<h2>v2.23.1</h2>
<ul>
<li>⚠️ Added warnings when the shorthand values <code>manylinux1</code>, <code>manylinux2010</code>, <code>manylinux_2_24</code>, and <code>musllinux_1_1</code> are used to specify the images in linux builds. The shorthand to these (unmaintainted) images will be removed in v3.0. If you want to keep using these images, explicitly opt-in using the full image URL, which can be found in <a href="https://github.com/pypa/cibuildwheel/blob/v2.23.1/cibuildwheel/resources/pinned_docker_images.cfg">this file</a>. (<a href="">#2312</a>)</li>
<li>🛠 Dependency updates, including a manylinux update which fixes an <a href="">issue with rustup</a>. (<a href="">#2315</a>)</li>
</ul>
<h2>v2.23.0</h2>
<ul>
<li>✨ Adds official support for the new GitHub Actions Arm runners. In fact these worked out-of-the-box, now we include them in our tests and example configs. (<a href="">#2135</a> via <a href="">#2281</a>)</li>
<li>✨ Adds support for building PyPy 3.11 wheels (<a href="">#2268</a> via <a href="">#2281</a>)</li>
<li>🛠 Adopts the beta pypa/manylinux image for armv7l builds (<a href="">#2269</a> via <a href="">#2281</a>)</li>
<li>🛠 Dependency updates, including Pyodide 0.27 (<a href="">#2117</a> and <a href="">#2281</a>)</li>
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
<li>🛠 Now cibuildwheel uses dependency-groups for development dependencies (<a href="">#2064</a>, <a href="">#2085</a>)</li>
<li>📚 Docs updates and tidy ups (<a href="">#2061</a>, <a href="">#2067</a>, <a href="">#2072</a>)</li>
</ul>
<h3>v2.21.3</h3>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>d04cacb</code></a> Bump version: v2.23.2</li>
<li><a href=""><code>5f4e019</code></a> [2.x] Update dependencies (<a href="">#2334</a>)</li>
<li><a href=""><code>2efa648</code></a> fix: always resolve --python argument (<a href="">#2328</a>) (<a href="">#2331</a>)</li>
<li><a href=""><code>42728e8</code></a> Bump version: v2.23.1</li>
<li><a href=""><code>6e1527b</code></a> Fix unit test when other warnings are present</li>
<li><a href=""><code>c25fe60</code></a> fix: image deprecation warning (<a href="">#2314</a>)</li>
<li><a href=""><code>a880bf5</code></a> fix: warn on deprecated images being set (<a href="">#2312</a>)</li>
<li><a href=""><code>c087d85</code></a> Update dependencies</li>
<li><a href=""><code>6cccd09</code></a> Bump version: v2.23.0</li>
<li><a href=""><code>aa1534d</code></a> chore(2.x): update dependencies (<a href="">#2294</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/ee63bf16da6cddfb925f542f2c7b59ad50e93969...d04cacbc9866d432033b1d09142936e6a0e2121a">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/upload-artifact` from 4.6.1 to 4.6.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/upload-artifact/releases">actions/upload-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.6.2</h2>
<h2>What's Changed</h2>
<ul>
<li>Update to use artifact 2.3.2 package &amp; prepare for new upload-artifact release by <a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> in <a href="">actions/upload-artifact#685</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/salmanmkc"><code>@​salmanmkc</code></a> made their first contribution in <a href="">actions/upload-artifact#685</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/upload-artifact/compare/v4...v4.6.2">https://github.com/actions/upload-artifact/compare/v4...v4.6.2</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>ea165f8</code></a> Merge pull request <a href="">#685</a> from salmanmkc/salmanmkc/3-new-upload-artifacts-release</li>
<li><a href=""><code>0839620</code></a> Prepare for new release of actions/upload-artifact with new toolkit cache ver...</li>
<li>See full diff in <a href="https://github.com/actions/upload-artifact/compare/4cec3d8aa04e39d1a68397de0c4cd6fb9dce8ec1...ea165f8d65b6e75b540449e92b4886f43607fa02">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/setup-python` from 5.4.0 to 5.5.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/setup-python/releases">actions/setup-python's releases</a>.</em></p>
<blockquote>
<h2>v5.5.0</h2>
<h2>What's Changed</h2>
<h3>Enhancements:</h3>
<ul>
<li>Support free threaded Python versions like '3.13t' by <a href="https://github.com/colesbury"><code>@​colesbury</code></a> in <a href="">actions/setup-python#973</a></li>
<li>Enhance Workflows: Include ubuntu-arm runners, Add e2e Testing for free threaded and Upgrade <code>@​action/cache</code> from 4.0.0 to 4.0.3 by <a href="https://github.com/priya-kinthali"><code>@​priya-kinthali</code></a> in <a href="">actions/setup-python#1056</a></li>
<li>Add support for .tool-versions file in setup-python by <a href="https://github.com/mahabaleshwars"><code>@​mahabaleshwars</code></a> in <a href="">actions/setup-python#1043</a></li>
</ul>
<h3>Bug fixes:</h3>
<ul>
<li>Fix architecture for pypy on Linux ARM64 by <a href="https://github.com/mayeut"><code>@​mayeut</code></a> in <a href="">actions/setup-python#1011</a>
This update maps arm64 to aarch64 for Linux ARM64 PyPy installations.</li>
</ul>
<h3>Dependency updates:</h3>
<ul>
<li>Upgrade <code>@​vercel/ncc</code> from 0.38.1 to 0.38.3 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1016</a></li>
<li>Upgrade <code>@​actions/glob</code> from 0.4.0 to 0.5.0 by <a href="https://github.com/dependabot"><code>@​dependabot</code></a> in <a href="">actions/setup-python#1015</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/colesbury"><code>@​colesbury</code></a> made their first contribution in <a href="">actions/setup-python#973</a></li>
<li><a href="https://github.com/mahabaleshwars"><code>@​mahabaleshwars</code></a> made their first contribution in <a href="">actions/setup-python#1043</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/setup-python/compare/v5...v5.5.0">https://github.com/actions/setup-python/compare/v5...v5.5.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>8d9ed9a</code></a> Add e2e Testing for free threaded and Bump <code>@​action/cache</code> from 4.0.0 to 4.0.3 ...</li>
<li><a href=""><code>19e4675</code></a> Add support for .tool-versions file in setup-python (<a href="">#1043</a>)</li>
<li><a href=""><code>6fd11e1</code></a> Bump <code>@​actions/glob</code> from 0.4.0 to 0.5.0 (<a href="">#1015</a>)</li>
<li><a href=""><code>9e62be8</code></a> Support free threaded Python versions like '3.13t' (<a href="">#973</a>)</li>
<li><a href=""><code>6ca8e85</code></a> Bump <code>@​vercel/ncc</code> from 0.38.1 to 0.38.3 (<a href="">#1016</a>)</li>
<li><a href=""><code>8039c45</code></a> fix: install PyPy on Linux ARM64 (<a href="">#1011</a>)</li>
<li>See full diff in <a href="https://github.com/actions/setup-python/compare/42375524e23c412d93fb67b49958b491fce71c38...8d9ed9ac5c53483de85588cdf95a591a75ab9f55">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/download-artifact` from 4.1.9 to 4.2.1
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v4.2.1</h2>
<h2>What's Changed</h2>
<ul>
<li>Add unit tests by <a href="https://github.com/GhadimiR"><code>@​GhadimiR</code></a> in <a href="">actions/download-artifact#392</a></li>
<li>Fix bug introduced in 4.2.0 by <a href="https://github.com/GhadimiR"><code>@​GhadimiR</code></a> in <a href="">actions/download-artifact#391</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v4.2.0...v4.2.1">https://github.com/actions/download-artifact/compare/v4.2.0...v4.2.1</a></p>
<h2>v4.2.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Update README.md by <a href="https://github.com/lkfortuna"><code>@​lkfortuna</code></a> in <a href="">actions/download-artifact#384</a></li>
<li>Bump artifact version, do digest check by <a href="https://github.com/GhadimiR"><code>@​GhadimiR</code></a> in <a href="">actions/download-artifact#383</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/lkfortuna"><code>@​lkfortuna</code></a> made their first contribution in <a href="">actions/download-artifact#384</a></li>
<li><a href="https://github.com/GhadimiR"><code>@​GhadimiR</code></a> made their first contribution in <a href="">actions/download-artifact#383</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/actions/download-artifact/compare/v4.1.9...v4.2.0">https://github.com/actions/download-artifact/compare/v4.1.9...v4.2.0</a></p>
</blockquote>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>95815c3</code></a> Merge pull request <a href="">#391</a> from GhadimiR/main</li>
<li><a href=""><code>278fca4</code></a> Move log statements</li>
<li><a href=""><code>6890984</code></a> Merge branch 'main' into main</li>
<li><a href=""><code>f9415c0</code></a> Run unit tests in CI</li>
<li><a href=""><code>76a6eb5</code></a> Merge pull request <a href="">#392</a> from GhadimiR/add_unit_tests</li>
<li><a href=""><code>a2426d7</code></a> Merge branch 'main' into add_unit_tests</li>
<li><a href=""><code>3ffa694</code></a> lint</li>
<li><a href=""><code>53f6aa5</code></a> Add extra assertion to download single artifact test</li>
<li><a href=""><code>b456700</code></a> lint</li>
<li><a href=""><code>9eab798</code></a> Configure tsconfig</li>
<li>Additional commits viewable in <a href="https://github.com/actions/download-artifact/compare/cc203385981b70ca67e1cc392babf9cc229d5806...95815c38cf2ff2164869cbab79da8d1f422bc89e">compare view</a></li>
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `007f5e343036`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.