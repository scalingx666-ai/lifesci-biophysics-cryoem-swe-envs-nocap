# Issue

**Title:** Bump the actions-version group with 3 updates

## Description

Bumps the actions-version group with 3 updates: [pypa/cibuildwheel](https://github.com/pypa/cibuildwheel), [actions/download-artifact](https://github.com/actions/download-artifact) and [softprops/action-gh-release](https://github.com/softprops/action-gh-release).

Updates `pypa/cibuildwheel` from 2.23.3 to 3.1.3
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/releases">pypa/cibuildwheel's releases</a>.</em></p>
<blockquote>
<h2>v3.1.3</h2>
<ul>
<li>🐛 Fix bug where &quot;latest&quot; dependencies couldn't update to pip 25.2 on Windows (<a href="">#2537</a>)</li>
<li>🛠 Use pytest-rerunfailures to improve some of our iOS/Android tests (<a href="">#2527</a>, <a href="">#2539</a>)</li>
<li>🛠 Remove some GraalPy Windows workarounds in our tests (<a href="">#2501</a>)</li>
</ul>
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
<h2>v3.1.1</h2>
<ul>
<li>🐛 Fix a bug showing an incorrect wheel count at the end of execution, and misrepresenting test-only runs in the GitHub Action summary (<a href="">#2512</a>)</li>
<li>📚 Docs fix (<a href="">#2510</a>)</li>
</ul>
<h2>v3.1.0</h2>
<ul>
<li>🌟 CPython 3.14 wheels are now built by default - without the <code>&quot;cpython-prerelease&quot;</code> <code>enable</code> set. It's time to build and upload these wheels to PyPI! This release includes CPython 3.14.0rc1, which is guaranteed to be ABI compatible with the final release. (<a href="">#2507</a>) Free-threading is no longer experimental in 3.14, so you have to skip it explicitly with <code>'cp31?t-*'</code> if you don't support it yet. (<a href="">#2503</a>)</li>
<li>🌟 Adds the ability to <a href="https://cibuildwheel.pypa.io/en/stable/platforms/#android">build wheels for Android</a>! Set the <a href="https://cibuildwheel.pypa.io/en/stable/options/#platform"><code>platform</code> option</a> to <code>android</code> on Linux or macOS to try it out! (<a href="">#2349</a>)</li>
<li>🌟 Adds Pyodide 0.28, which builds 3.13 wheels (<a href="">#2487</a>)</li>
<li>✨ Support for 32-bit <code>manylinux_2_28</code> (now a consistent default) and <code>manylinux_2_34</code> added (<a href="">#2500</a>)</li>
<li>🛠 Improved summary, will also use markdown summary output on GHA (<a href="">#2469</a>)</li>
<li>🛠 The riscv64 images now have a working default (as they are now part of pypy/manylinux), but are still experimental (and behind an <code>enable</code>) since you can't push them to PyPI yet (<a href="">#2506</a>)</li>
<li>🛠 Fixed a typo in the 3.9 MUSL riscv64 identifier (<code>cp39-musllinux_ricv64</code> -&gt; <code>cp39-musllinux_riscv64</code>) (<a href="">#2490</a>)</li>
<li>🛠 Mistyping <code>--only</code> now shows the correct possibilities, and even suggests near matches on Python 3.14+ (<a href="">#2499</a>)</li>
<li>🛠 Only support one output from the repair step on linux like other platforms; auditwheel fixed this over four years ago! (<a href="">#2478</a>)</li>
<li>🛠 We now use pattern matching extensively (<a href="">#2434</a>)</li>
<li>📚 We now have platform maintainers for our special platforms and interpreters! (<a href="">#2481</a>)</li>
</ul>
<h2>v3.0.1</h2>
<ul>
<li>🛠 Updates CPython 3.14 prerelease to 3.14.0b3 (<a href="">#2471</a>)</li>
<li>✨ Adds a CPython 3.14 prerelease iOS build (only when prerelease builds are <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable">enabled</a>) (<a href="">#2475</a>)</li>
</ul>
<h2>v3.0.0</h2>
<p>See <a href="https://github.com/henryiii"><code>@​henryiii</code></a>'s <a href="https://iscinumpy.dev/post/cibuildwheel-3-0-0/">release post</a> for more info on new features!</p>
<ul>
<li>
<p>🌟 Adds the ability to <a href="https://cibuildwheel.pypa.io/en/stable/platforms/#ios">build wheels for iOS</a>! Set the <a href="https://cibuildwheel.pypa.io/en/stable/options/#platform"><code>platform</code> option</a> to <code>ios</code> on a Mac with the iOS toolchain to try it out! (<a href="">#2286</a>, <a href="">#2363</a>, <a href="">#2432</a>)</p>
</li>
<li>
<p>🌟 Adds support for the GraalPy interpreter! Enable for your project using the <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code> option</a>. (<a href="">#1538</a>, <a href="">#2411</a>, <a href="">#2414</a>)</p>
</li>
<li>
<p>✨ Adds CPython 3.14 support, under the <a href="https://cibuildwheel.pypa.io/en/stable/options/#enable"><code>enable</code> option</a> <code>cpython-prerelease</code>. This version of cibuildwheel uses 3.14.0b2. (<a href="">#2390</a>)</p>
<p><em>While CPython is in beta, the ABI can change, so your wheels might not be compatible with the final release. For this reason, we don't recommend distributing wheels until RC1, at which point 3.14 will be available in cibuildwheel without the flag.</em> (<a href="">#2390</a>)</p>
</li>
<li>
<p>✨ Adds the <a href="https://cibuildwheel.pypa.io/en/stable/options/#test-sources">test-sources option</a>, and changes the working directory for tests. (<a href="">#2062</a>, <a href="">#2284</a>, <a href="">#2437</a>)</p>
<ul>
<li>If this option is set, cibuildwheel will copy the files and folders specified in <code>test-sources</code> into the temporary directory we run from. This is required for iOS builds, but also useful for other platforms, as it allows you to avoid placeholders.</li>
<li>If this option is not set, behaviour matches v2.x - cibuildwheel will run the tests from a temporary directory, and you can use the <code>{project}</code> placeholder in the <code>test-command</code> to refer to the project directory. (<a href="">#2420</a>)</li>
</ul>
</li>
<li>
<p>✨ Adds <a href="https://cibuildwheel.pypa.io/en/stable/options/#dependency-versions"><code>dependency-versions</code></a> inline syntax (<a href="">#2122</a>)</p>
</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/pypa/cibuildwheel/blob/main/docs/changelog.md">pypa/cibuildwheel's changelog</a>.</em></p>
<blockquote>
<hr />
<h2>title: Changelog</h2>
<h1>Changelog</h1>
<h3>v3.1.3</h3>
<p><em>1 August 2025</em></p>
<ul>
<li>🐛 Fix bug where &quot;latest&quot; dependencies couldn't update to pip 25.2 on Windows (<a href="">#2537</a>)</li>
<li>🛠 Use pytest-rerunfailures to improve some of our iOS/Android tests (<a href="">#2527</a>, <a href="">#2539</a>)</li>
<li>🛠 Remove some GraalPy Windows workarounds in our tests (<a href="">#2501</a>)</li>
</ul>
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
<h3>v3.1.1</h3>
<p><em>24 July 2025</em></p>
<ul>
<li>🐛 Fix a bug showing an incorrect wheel count at the end of execution, and misrepresenting test-only runs in the GitHub Action summary (<a href="">#2512</a>)</li>
<li>📚 Docs fix (<a href="">#2510</a>)</li>
</ul>
<h3>v3.1.0</h3>
<p><em>23 July 2025</em></p>
<ul>
<li>🌟 CPython 3.14 wheels are now built by default - without the <code>&quot;cpython-prerelease&quot;</code> <code>enable</code> set. It's time to build and upload these wheels to PyPI! This release includes CPython 3.14.0rc1, which is guaranteed to be ABI compatible with the final release. (<a href="">#2507</a>) Free-threading is no longer experimental in 3.14, so you have to skip it explicitly with <code>'cp31?t-*'</code> if you don't support it yet. (<a href="">#2503</a>)</li>
<li>🌟 Adds the ability to <a href="https://cibuildwheel.pypa.io/en/stable/platforms/#android">build wheels for Android</a>! Set the <a href="https://cibuildwheel.pypa.io/en/stable/options/#platform"><code>platform</code> option</a> to <code>android</code> on Linux or macOS to try it out! (<a href="">#2349</a>)</li>
<li>🌟 Adds Pyodide 0.28, which builds 3.13 wheels (<a href="">#2487</a>)</li>
<li>✨ Support for 32-bit <code>manylinux_2_28</code> (now a consistent default) and <code>manylinux_2_34</code> added (<a href="">#2500</a>)</li>
<li>🛠 Improved summary, will also use markdown summary output on GHA (<a href="">#2469</a>)</li>
<li>🛠 The riscv64 images now have a working default (as they are now part of pypy/manylinux), but are still experimental (and behind an <code>enable</code>) since you can't push them to PyPI yet (<a href="">#2506</a>)</li>
<li>🛠 Fixed a typo in the 3.9 MUSL riscv64 identifier (<code>cp39-musllinux_ricv64</code> -&gt; <code>cp39-musllinux_riscv64</code>) (<a href="">#2490</a>)</li>
<li>🛠 Mistyping <code>--only</code> now shows the correct possibilities, and even suggests near matches on Python 3.14+ (<a href="">#2499</a>)</li>
</ul>
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>352e013</code></a> Bump version: v3.1.3</li>
<li><a href=""><code>c463e56</code></a> tests: another iOS flaky spot (<a href="">#2539</a>)</li>
<li><a href=""><code>8c5c738</code></a> docs(project): add Falcon to working examples (<a href="">#2538</a>)</li>
<li><a href=""><code>feeb399</code></a> tests: add flaky test handling (<a href="">#2527</a>)</li>
<li><a href=""><code>60b9cc9</code></a> fix: never call pip directly (<a href="">#2537</a>)</li>
<li><a href=""><code>e2c7102</code></a> chore: remove some GraalPy Windows workarounds. (<a href="">#2501</a>)</li>
<li><a href=""><code>9e4e50b</code></a> Bump version: v3.1.2</li>
<li><a href=""><code>8ef9414</code></a> [pre-commit.ci] pre-commit autoupdate (<a href="">#2532</a>)</li>
<li><a href=""><code>1953c04</code></a> Adding <a href="https://github.com/mhsmith"><code>@​mhsmith</code></a> as platform maintainer for Android (<a href="">#2516</a>)</li>
<li><a href=""><code>46a6d27</code></a> Bump iOS support package versions. (<a href="">#2530</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/pypa/cibuildwheel/compare/faf86a6ed7efa889faf6996aa23820831055001a...352e01339f0a173aa2a3eb57f01492e341e83865">compare view</a></li>
</ul>
</details>
<br />

Updates `actions/download-artifact` from 4.3.0 to 5.0.0
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/actions/download-artifact/releases">actions/download-artifact's releases</a>.</em></p>
<blockquote>
<h2>v5.0.0</h2>
<h2>What's Changed</h2>
<ul>
<li>Update README.md by <a href="https://github.com/nebuk89"><code>@​nebuk89</code></a> in <a href="">actions/download-artifact#407</a></li>
<li>BREAKING fix: inconsistent path behavior for single artifact downloads by ID by <a href="https://github.com/GrantBirki"><code>@​GrantBirki</code></a> in <a href="">actions/download-artifact#416</a></li>
</ul>
<h2>v5.0.0</h2>
<h3>🚨 Breaking Change</h3>
<p>This release fixes an inconsistency in path behavior for single artifact downloads by ID. <strong>If you're downloading single artifacts by ID, the output path may change.</strong></p>
<h4>What Changed</h4>
<p>Previously, <strong>single artifact downloads</strong> behaved differently depending on how you specified the artifact:</p>
<ul>
<li><strong>By name</strong>: <code>name: my-artifact</code> → extracted to <code>path/</code> (direct)</li>
<li><strong>By ID</strong>: <code>artifact-ids: 12345</code> → extracted to <code>path/my-artifact/</code> (nested)</li>
</ul>
<p>Now both methods are consistent:</p>
<ul>
<li><strong>By name</strong>: <code>name: my-artifact</code> → extracted to <code>path/</code> (unchanged)</li>
<li><strong>By ID</strong>: <code>artifact-ids: 12345</code> → extracted to <code>path/</code> (fixed - now direct)</li>
</ul>
<h4>Migration Guide</h4>
<h5>✅ No Action Needed If:</h5>
<ul>
<li>You download artifacts by <strong>name</strong></li>
<li>You download <strong>multiple</strong> artifacts by ID</li>
<li>You already use <code>merge-multiple: true</code> as a workaround</li>
</ul>
<h5>⚠️ Action Required If:</h5>
<p>You download <strong>single artifacts by ID</strong> and your workflows expect the nested directory structure.</p>
<p><strong>Before v5 (nested structure):</strong></p>
<pre lang="yaml"><code>- uses: actions/download-artifact@v4
  with:
    artifact-ids: 12345
    path: dist
# Files were in: dist/my-artifact/
</code></pre>
<blockquote>
<p>Where <code>my-artifact</code> is the name of the artifact you previously uploaded</p>
</blockquote>
<p><strong>To maintain old behavior (if needed):</strong></p>
<pre lang="yaml"><code>&lt;/tr&gt;&lt;/table&gt;
</code></pre>
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>634f93c</code></a> Merge pull request <a href="">#416</a> from actions/single-artifact-id-download-path</li>
<li><a href=""><code>b19ff43</code></a> refactor: resolve download path correctly in artifact download tests (mainly ...</li>
<li><a href=""><code>e262cbe</code></a> bundle dist</li>
<li><a href=""><code>bff23f9</code></a> update docs</li>
<li><a href=""><code>fff8c14</code></a> fix download path logic when downloading a single artifact by id</li>
<li><a href=""><code>448e3f8</code></a> Merge pull request <a href="">#407</a> from actions/nebuk89-patch-1</li>
<li><a href=""><code>47225c4</code></a> Update README.md</li>
<li>See full diff in <a href="https://github.com/actions/download-artifact/compare/d3f86a106a0bac45b974a628896c90dbdf5c8093...634f93cb2916e3fdff6788551b99b062d0335ce0">compare view</a></li>
</ul>
</details>
<br />

Updates `softprops/action-gh-release` from 2.2.2 to 2.3.2
<details>
<summary>Release notes</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/releases">softprops/action-gh-release's releases</a>.</em></p>
<blockquote>
<h2>v2.3.2</h2>
<ul>
<li>fix: revert fs <code>readableWebStream</code> change</li>
</ul>
<h2>v2.3.1</h2>
<!-- raw HTML omitted -->
<h2>What's Changed</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: fix file closing issue by <a href="https://github.com/WailGree"><code>@​WailGree</code></a> in <a href="">softprops/action-gh-release#629</a></li>
</ul>
<h2>New Contributors</h2>
<ul>
<li><a href="https://github.com/WailGree"><code>@​WailGree</code></a> made their first contribution in <a href="">softprops/action-gh-release#629</a></li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/softprops/action-gh-release/compare/v2.3.0...v2.3.1">https://github.com/softprops/action-gh-release/compare/v2.3.0...v2.3.1</a></p>
<h2>v2.3.0</h2>
<!-- raw HTML omitted -->
<ul>
<li>Migrate from jest to vitest</li>
<li>Replace <code>mime</code> with <code>mime-types</code></li>
<li>Bump to use node 24</li>
<li>Dependency updates</li>
</ul>
<p><strong>Full Changelog</strong>: <a href="https://github.com/softprops/action-gh-release/compare/v2.2.2...v2.3.0">https://github.com/softprops/action-gh-release/compare/v2.2.2...v2.3.0</a></p>
</blockquote>
</details>
<details>
<summary>Changelog</summary>
<p><em>Sourced from <a href="https://github.com/softprops/action-gh-release/blob/master/CHANGELOG.md">softprops/action-gh-release's changelog</a>.</em></p>
<blockquote>
<h2>2.3.2</h2>
<ul>
<li>fix: revert fs <code>readableWebStream</code> change</li>
</ul>
<h2>2.3.1</h2>
<h3>Bug fixes 🐛</h3>
<ul>
<li>fix: fix file closing issue by <a href="https://github.com/WailGree"><code>@​WailGree</code></a> in <a href="">softprops/action-gh-release#629</a></li>
</ul>
<h2>2.3.0</h2>
<ul>
<li>Migrate from jest to vitest</li>
<li>Replace <code>mime</code> with <code>mime-types</code></li>
<li>Bump to use node 24</li>
<li>Dependency updates</li>
</ul>
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
<!-- raw HTML omitted -->
</blockquote>
<p>... (truncated)</p>
</details>
<details>
<summary>Commits</summary>
<ul>
<li><a href=""><code>72f2c25</code></a> release 2.3.2</li>
<li><a href=""><code>552dc55</code></a> fix: revert <code>fs:readableWebStream</code> change (<a href="">#632</a>)</li>
<li><a href=""><code>f3cad8b</code></a> release 2.3.1</li>
<li><a href=""><code>07a2257</code></a> fix: fix file closing issue (<a href="">#629</a>)</li>
<li><a href=""><code>d5382d3</code></a> release 2.3.0</li>
<li><a href=""><code>a0e2122</code></a> feat: migrate from jest to vitest (<a href="">#626</a>)</li>
<li><a href=""><code>8836085</code></a> chore: replace <code>mime</code> with <code>mime-types</code> (<a href="">#624</a>)</li>
<li><a href=""><code>8646335</code></a> chore: bump node to 20.19.2</li>
<li><a href=""><code>46b2847</code></a> chore(deps): bump the npm group across 1 directory with 5 updates (<a href="">#623</a>)</li>
<li><a href=""><code>37fd9d0</code></a> chore(deps): bump undici from 5.28.5 to 5.29.0 (<a href="">#621</a>)</li>
<li>Additional commits viewable in <a href="https://github.com/softprops/action-gh-release/compare/da05d552573ad5aba039eaac05058a918a7bf631...72f2c25fcb47643c292f7107632f7a47c1df5cd8">compare view</a></li>
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

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6931fdb12a5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.