# Issue

**Title:** kenref: record the RUNPATH to a KEnRef that is not on LIBRARY_PATH

## Description

##### Description

With `--enable-kenref`, when `configure` fetches and builds KEnRef itself (also with `--with-kenref-src=DIR`), `make` fails to link the `plumed` executable:

```
ld: warning: libkenref_core.so.2, needed by libplumedKernel.so, not found (try using -rpath or -rpath-link)
ld: libplumedKernel.so: undefined reference to `kenref::ModelRegistry<double>::has(...)'
```

The same happens with a KEnRef that is found only through `PKG_CONFIG_PATH`.

The cause: the rpath that `--enable-rpath` records (on by default for KEnRef builds) is `$libdir:$LIBRARY_PATH`, not the `-L` dirs of `LIBS`. On these routes the KEnRef libdir reaches `LIBS` through `pkg-config --libs`, but it is not on `LIBRARY_PATH`. So `libplumedKernel.so` gets no RUNPATH to it, and the `plumed` executable, which is linked against the kernel only, cannot resolve it. Builds that put the KEnRef libdir on `LIBRARY_PATH` themselves, as `install.md` used to ask, were not affected.

This PR adds the `-L` dirs of the KEnRef libs to `LIBRARY_PATH` in `KENREF_CHECK_LIBS`, once each, so that the existing rpath code records them. `--disable-rpath` still turns this off. A build that already has the KEnRef libdir on `LIBRARY_PATH` gets the same RUNPATH as before, with no duplicate entry. `configure` is regenerated with autoconf 2.69 (only the new hunk changes), and `src/kenref/install.md` no longer asks users to put the KEnRef libdir on `LIBRARY_PATH` by hand.

Tested locally, with `LD_LIBRARY_PATH` unset unless noted:
- a clean `./configure --enable-kenref` (KEnRef fetched and built): fails on `master` as above; with this PR, it links, `libplumedKernel.so` and `plumed` have `kenref-deps/prefix/lib` in their RUNPATH, `ldd` resolves `libkenref_core.so.2`, and the serial kenref regtests pass;
- `--with-kenref-src=DIR` with `--enable-mpi` and `make install`: the installed `plumed info --version` runs, and the 4 kenref regtests pass;
- KEnRef already installed and on `LIBRARY_PATH`, `--enable-rpath`, libc++ (with libc++ on `LD_LIBRARY_PATH` at build time, as `install.md` says): RUNPATH is unchanged, the installed `plumed` runs, and the 4 kenref regtests pass.

The `-kenref-` CI job finds KEnRef through `PKG_CONFIG_PATH` and still sets `LD_LIBRARY_PATH`. I left it as it is.

##### Target release

I would like my code to appear in release __v2.11__

##### Type of contribution

- [ ] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [X] new module contribution or edit of a module authored by you

##### Copyright

- [X] the module I added or modified contains a `COPYRIGHT` file with the correct license information. Code should be released under an open source license. I also used the command `cd src && ./header.sh mymodulename` in order to make sure the headers of the module are correct.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2a5cd1525e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.